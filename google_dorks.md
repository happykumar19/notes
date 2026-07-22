Google dorks can be very effective for **authorized security testing** and bug bounty reconnaissance. The key is to use them **only on programs where you have permission to test** and to stay within the program's scope.

## 1. Find exposed configuration files

```text
site:example.com ext:env
site:example.com ext:ini
site:example.com ext:yaml
site:example.com ext:yml
site:example.com ext:json
site:example.com ext:config
site:example.com ext:bak
site:example.com ext:old
site:example.com ext:backup
site:example.com ext:zip
site:example.com ext:tar.gz
site:example.com ext:sql
```

---

## 2. Find admin panels

```text
site:example.com inurl:admin
site:example.com inurl:login
site:example.com inurl:dashboard
site:example.com inurl:cpanel
site:example.com inurl:portal
site:example.com intitle:"Admin"
site:example.com intitle:"Login"
```

---

## 3. Directory listing (Index of)

```text
site:example.com intitle:"Index of"
site:example.com "Index of /"
site:example.com intitle:"index of" backup
site:example.com intitle:"index of" uploads
site:example.com intitle:"index of" logs
site:example.com intitle:"index of" config
```

---

## 4. API discovery

```text
site:example.com inurl:api
site:example.com inurl:v1
site:example.com inurl:v2
site:example.com inurl:graphql
site:example.com inurl:swagger
site:example.com inurl:openapi
site:example.com inurl:docs
```

---

## 5. Swagger / API documentation

```text
site:example.com swagger
site:example.com "Swagger UI"
site:example.com "OpenAPI"
site:example.com "API Documentation"
site:example.com "graphql playground"
```

---

## 6. JavaScript files

```text
site:example.com ext:js
site:example.com inurl:static ext:js
site:example.com inurl:assets ext:js
site:example.com inurl:bundle ext:js
```

These are useful because JavaScript files may reveal:

* Hidden endpoints
* API URLs
* Feature flags
* Hardcoded keys (public and sometimes accidentally sensitive)

---

## 7. Interesting files

```text
site:example.com ext:log
site:example.com ext:txt
site:example.com ext:xml
site:example.com ext:pdf
site:example.com ext:xls
site:example.com ext:xlsx
site:example.com ext:doc
site:example.com ext:docx
```

---

## 8. Sensitive keywords

```text
site:example.com password
site:example.com secret
site:example.com token
site:example.com api_key
site:example.com "AWS"
site:example.com "Bearer"
site:example.com "Authorization"
```

---

## 9. Git exposure

```text
site:example.com ".git"
site:example.com ".git/config"
site:example.com ".gitignore"
```

---

## 10. Environment & configuration

```text
site:example.com ".env"
site:example.com "config.json"
site:example.com "settings.py"
site:example.com "application.properties"
site:example.com "web.config"
```

---

## 11. Error pages

```text
site:example.com "Exception"
site:example.com "Stack trace"
site:example.com "Internal Server Error"
site:example.com "Whitelabel Error Page"
site:example.com "Traceback"
```

---

## 12. GraphQL discovery

```text
site:example.com graphql
site:example.com inurl:graphql
site:example.com "GraphQL Playground"
site:example.com "Apollo Sandbox"
```

---

## 13. Cloud storage references

```text
site:example.com s3.amazonaws.com
site:example.com storage.googleapis.com
site:example.com blob.core.windows.net
```

---

## 14. Backup and temporary files

```text
site:example.com ext:swp
site:example.com ext:tmp
site:example.com ext:save
site:example.com ext:orig
site:example.com ext:copy
```

---

## 15. Bug bounty favorites

```text
site:example.com ext:env
site:example.com ext:sql
site:example.com ext:bak
site:example.com ext:zip
site:example.com intitle:"Index of"
site:example.com inurl:graphql
site:example.com ext:js
site:example.com inurl:swagger
site:example.com "api_key"
site:example.com "Authorization"
```

### Pro tips

* Replace `example.com` with the in-scope domain for your target.
* Combine dorks with `site:` to keep searches within the authorized scope.
* After finding JavaScript files, analyze them with tools like `linkfinder`, `xnLinkFinder`, or `SecretFinder`.
* Cross-check findings with archived URLs from `gau` or `waybackurls` to discover historical endpoints and files.

Used thoughtfully, these dorks can help you uncover exposed files, documentation, APIs, and other assets that are relevant during legitimate bug bounty reconnaissance.




If a bug bounty program gives you a wildcard scope like:

```text
*.example.com
```

then you can broaden your Google dorks to search across **all indexed subdomains** instead of just one host.

## Search every subdomain

```text
site:*.example.com
```

or

```text
site:example.com
```

Google treats `site:example.com` as including subdomains in many cases, but using `site:*.example.com` can make your intent clearer.

---

## Find exposed `.env` files

```text
site:*.example.com ext:env
site:*.example.com ".env"
site:*.example.com ext:ini
site:*.example.com ext:yaml
site:*.example.com ext:json
```

---

## Find backup files

```text
site:*.example.com ext:bak
site:*.example.com ext:old
site:*.example.com ext:zip
site:*.example.com ext:tar.gz
site:*.example.com ext:sql
site:*.example.com ext:7z
```

---

## Find admin portals

```text
site:*.example.com inurl:admin
site:*.example.com inurl:login
site:*.example.com inurl:dashboard
site:*.example.com intitle:"Admin"
```

---

## Find APIs

```text
site:*.example.com inurl:api
site:*.example.com inurl:v1
site:*.example.com inurl:v2
site:*.example.com inurl:graphql
site:*.example.com inurl:swagger
site:*.example.com "swagger-ui"
```

---

## Find JavaScript

```text
site:*.example.com ext:js
site:*.example.com inurl:static ext:js
site:*.example.com inurl:assets ext:js
```

---

## Find directory listings

```text
site:*.example.com intitle:"Index of"
site:*.example.com "Index of /"
site:*.example.com intitle:"index of" backup
site:*.example.com intitle:"index of" uploads
```

---

## Find interesting keywords

```text
site:*.example.com "password"
site:*.example.com "secret"
site:*.example.com "api_key"
site:*.example.com "token"
site:*.example.com "Authorization"
site:*.example.com "Bearer"
```

---

## Find Git exposure

```text
site:*.example.com ".git"
site:*.example.com ".git/config"
site:*.example.com ".gitignore"
```

---

## Find cloud storage references

```text
site:*.example.com s3.amazonaws.com
site:*.example.com storage.googleapis.com
site:*.example.com blob.core.windows.net
```

---

## Find error pages

```text
site:*.example.com "Stack trace"
site:*.example.com "Internal Server Error"
site:*.example.com "Exception"
site:*.example.com "Traceback"
```

---

# Useful operators to combine

```text
site:*.example.com -www
site:*.example.com inurl:dev
site:*.example.com inurl:test
site:*.example.com inurl:staging
site:*.example.com inurl:beta
site:*.example.com inurl:internal
site:*.example.com inurl:prod
```

---

# Advanced combinations

Find JavaScript on development hosts:

```text
site:*.example.com inurl:dev ext:js
```

Find admin pages on staging:

```text
site:*.example.com inurl:staging inurl:admin
```

Find GraphQL endpoints:

```text
site:*.example.com inurl:graphql
site:*.example.com "GraphQL Playground"
```

Find exposed backups on API hosts:

```text
site:*.example.com inurl:api ext:zip
site:*.example.com inurl:api ext:bak
```

---

## My favorite "high-signal" dorks

These tend to uncover assets worth investigating during authorized bug bounty reconnaissance:

```text
site:*.example.com ext:env
site:*.example.com ext:bak
site:*.example.com ext:zip
site:*.example.com ext:sql
site:*.example.com intitle:"Index of"
site:*.example.com inurl:graphql
site:*.example.com inurl:swagger
site:*.example.com ext:js
site:*.example.com "api_key"
site:*.example.com "Authorization"
site:*.example.com "Bearer"
site:*.example.com ".git"
site:*.example.com "Stack trace"
```

One additional tip: don't rely only on Google. Many bug bounty hunters combine Google dorks with searches of **GitHub**, the **Internet Archive (Wayback Machine)**, **Common Crawl**, and certificate transparency logs to discover subdomains and historical content that Google may not index. Using multiple sources generally yields much better coverage than Google alone.



