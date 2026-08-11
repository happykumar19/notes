 1. What “DOM bugs” and “prototype bugs” are

  DOM XSS

  Attacker-controlled browser sources flow into dangerous sinks without sanitization. No server reflection needed.

  ┌─────────────────────────────────────────────────────────────────────────────┬───────────────────────────────────────────────────────────────────────────────────────────────────────────┐
  │ Sources                                                                     │ Sinks                                                                                                     │
  ├─────────────────────────────────────────────────────────────────────────────┼───────────────────────────────────────────────────────────────────────────────────────────────────────────┤
  │ location.hash, location.search, document.URL, document.referrer,            │ innerHTML, outerHTML, document.write, eval, setTimeout(string), new Function, element.src / href,         │
  │ window.name                                                                 │ location                                                                                                  │
  └─────────────────────────────────────────────────────────────────────────────┴───────────────────────────────────────────────────────────────────────────────────────────────────────────┘

  Classic: https://target.com/#<img src=x onerror=alert(1)> where JS does el.innerHTML = location.hash.slice(1).

  Related high-value DOM bugs:
  • postMessage XSS — listener with weak/missing origin check → attacker page posts HTML/JS into a sink
  • DOM clobbering — named HTML elements overwrite window/document properties that scripts later trust
  • Open redirect / JS URL sinks — location = userInput with javascript: or attacker host

  Client-side Prototype Pollution (CSPP)

  Attacker sets properties on Object.prototype via URL/JSON merge. Later code reads a “config” property that now comes from the prototype → unexpected behavior → often XSS via a gadget.

  ?__proto__[x]=1
  ?#__proto__[x]=1
  ?constructor[prototype][x]=1

  Flow from the article:
  1. Pollution confirmed (DOM Invader lights up)
  2. Gadget found (code path that turns polluted props into XSS)
  3. Vendor patches one sink → scanner goes quiet
  4. You find another gadget manually → still XSS

  Root fix = stop pollution (sanitize __proto__ / constructor / recursive merges), not only one XSS sink.

  ────────────────────────────────────────

  2. Frontend bug hunting methodology

  Phase A — Surface map (15–30 min)

  1. Browse the app logged in + logged out; note SPAs, widgets, OAuth, iframes, SDKs.
  2. Pull JS bundles (Network → JS, or crawl).
  3. Prefer pages with: hash routing, query-driven UI, third-party embeds, markdown/HTML preview, search that updates DOM without reload.

  # Pull JS URLs then download
  katana -u https://target.com -jc -d 2 | grep '\.js'
  # Or from Burp: right-click site map → Copy URLs → filter .js

  Phase B — Source → sink (core loop)

  1. Grep bundles for sinks + sources + pollution vectors.
  2. Trace: does user input reach that sink?
  3. Confirm in DevTools (breakpoint on sink / DOM breakpoints).

  # High-signal greps on downloaded JS
  grep -RniE 'innerHTML|outerHTML|document\.write|eval\(|new Function|setTimeout\([^,]+,' ./js/
  grep -RniE 'location\.(hash|search|href)|document\.(URL|referrer)|window\.name' ./js/
  grep -RniE '__proto__|constructor\[|Object\.assign|lodash\.merge|jquery\.extend|deepMerge|JSON\.parse' ./js/
  grep -RniE "addEventListener\(['\"]message|postMessage" ./js/

  Phase C — Prototype pollution specifically

  1. Enable Burp DOM Invader → Prototype Pollution.
  2. Browse many hosts/pages (wildcard scopes matter — same as the article).
  3. When Invader shows Scan for Gadgets → run it → test suggested URL.
  4. If pollution exists but no gadgets:
    • Grep for known gadget patterns (jQuery, DOMPurify configs, script.src, iframe.srcdoc, transport_url, etc.)
    • Use PortSwigger / BlackFan gadget lists + HTB-style manual gadget hunting
  5. After a “fix”, re-test pollution + hunt new gadgets — don’t trust “no Exploit button.”

  Quick confirmation in console after loading a polluted URL:

  ({}).polluted          // should be attacker value if polluted
  Object.prototype.x     // same idea

  Phase D — postMessage / iframe / OAuth UI

  // In DevTools on target page
  getEventListeners(window).message

  Host an attacker page that postMessages into the iframe; try missing origin, indexOf, endsWith, === "null".

  Phase E — Impact before report

  alert(document.domain) is detection only. For bounty:
  • cookie / storage token theft (if not HttpOnly)
  • CSRF token steal → state-changing action
  • session / OAuth code theft via postMessage
  • admin-panel XSS
  • self-XSS alone usually dies unless you escalate (stored → other user, or cookie XSS sitewide)

  ────────────────────────────────────────

  3. Tools — what to use and when

  ┌─────────────────────────────────────────────────┬─────────────────────────────────┬─────────────────────────────────────────────────────────────────────┐
  │ Tool                                            │ Role                            │ How                                                                 │
  ├─────────────────────────────────────────────────┼─────────────────────────────────┼─────────────────────────────────────────────────────────────────────┤
  │ Burp + DOM Invader                              │ Best default for DOM XSS + CSPP │ Built-in browser → DOM Invader → enable PP + XSS; click through app │
  ├─────────────────────────────────────────────────┼─────────────────────────────────┼─────────────────────────────────────────────────────────────────────┤
  │ Burp postMessage-tracker                        │ Logs every postMessage          │ Install extension; watch origins + payloads                         │
  ├─────────────────────────────────────────────────┼─────────────────────────────────┼─────────────────────────────────────────────────────────────────────┤
  │ DevTools                                        │ Manual truth                    │ Sources breakpoints, Event Listener breakpoints, getEventListeners  │
  ├─────────────────────────────────────────────────┼─────────────────────────────────┼─────────────────────────────────────────────────────────────────────┤
  │ JSBeautifier / source maps                      │ Readable JS                     │ If .map exists, load it — huge advantage                            │
  ├─────────────────────────────────────────────────┼─────────────────────────────────┼─────────────────────────────────────────────────────────────────────┤
  │ LinkFinder / katana -jc                         │ Find JS URLs                    │ Recon only                                                          │
  ├─────────────────────────────────────────────────┼─────────────────────────────────┼─────────────────────────────────────────────────────────────────────┤
  │ dalfox / xsstrike                               │ Reflected XSS automation        │ Weak on pure DOM/hash bugs — don’t rely on them for SPA DOM         │
  ├─────────────────────────────────────────────────┼─────────────────────────────────┼─────────────────────────────────────────────────────────────────────┤
  │ ppmap / find something like BlackFan gadgets    │ Extra PP gadget scan            │ After Invader; never sole authority                                 │
  ├─────────────────────────────────────────────────┼─────────────────────────────────┼─────────────────────────────────────────────────────────────────────┤
  │ PortSwigger Academy (DOM XSS, CSPP, clobbering) │ Skill building                  │ Same stack the article’s HTB module teaches                         │
  └─────────────────────────────────────────────────┴─────────────────────────────────┴─────────────────────────────────────────────────────────────────────┘

  Automation rule for frontend: scanners find signals. You own gadget + impact. That is exactly the Medium takeaway.

  ────────────────────────────────────────

  4. Can Claude Code / AI agents find frontend bugs?

  Yes as an analyst and JS auditor — no as a replacement for the browser.

  What AI is good at

  • Deobfuscating / summarizing large bundles
  • Tracing source → sink in code you paste or download
  • Listing gadget hypotheses after pollution is confirmed
  • Generating attacker HTML for postMessage PoCs
  • Ranking which sinks matter (XSS vs dead code)
  • Writing the report after you have a live PoC

  What AI is bad at alone

  • Executing real DOM gadgets in the live app
  • Knowing CSP / WAF / cookie flags without live checks
  • Finding “Scan for Gadgets” hits by clicking subdomains for an hour
  • Avoiding theoretical findings (could / might)

  Practical Claude Code workflow

  1. You: download JS + open Burp DOM Invader yourself.
  2. Agent: grep / read bundles for sinks, merges, message listeners.
  3. Agent: propose concrete URLs/payloads and a gadget checklist.
  4. You: load payloads in Burp browser; confirm alert / cookie exfil.
  5. Agent: help escalate impact and draft the report from proven behavior.

  Prompt shape that works:

  Here are the app's main JS files. Find:
  1) user-controlled sources reaching sinks
  2) recursive merges / Object.assign of query/hash/JSON
  3) postMessage listeners and origin checks
  For each hit, give: file, line hint, attack URL/shape, how to verify in DevTools.
  Do not claim a vuln without a verification step I can run.

  Use AI as a second analyst (hypothesis → one concrete experiment). Live DOM + two-account impact still decide if it’s a bug.

  ────────────────────────────────────────

  5. Minimal weekly practice plan

  1. PortSwigger: DOM XSS → then Client-side Prototype Pollution → DOM clobbering
  2. One real target: DOM Invader on for 1 hour of navigation only
  3. One session: download JS, manual source→sink on one feature (search, preview, share link)
  4. One session: postMessage tracker on pages with iframes/OAuth
  5. After any PP hit: practice manual gadget even if Invader already pops

  ────────────────────────────────────────

  Bottom line

  Frontend money is usually DOM XSS, CSPP→gadget XSS, postMessage, clobbering, not nuclei greps. The article’s method is the methodology: Invader for detection → confirm XSS → after patch,
  assume more gadgets exist → hunt manually. Claude Code accelerates reading JS and planning tests; Burp + your browser close the finding.
