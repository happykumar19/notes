Read `docs/AGENTS.md` and `prompts/AGENTS.md` first and follow them strictly.

Covers `docs/todo.md` Tasks 2.4, 2.5, and 2.6.

Replace the mocked auth from prompt `06` with real Clerk authentication. Keep the screen designs and navigation flow exactly as they are. If a design change is unavoidable, ask first.

Follow the current Clerk Expo documentation: <https://clerk.com/docs/expo/getting-started/quickstart>. Clerk is not yet installed, so state which packages you are adding and why before installing.

Implement:

- `ClerkProvider` in the root layout with the token cache backed by `expo-secure-store`, not AsyncStorage.
- Email code sign-up and sign-in, with the six-digit verification wired to Clerk's flow.
- Apple and Google OAuth.
- Sign out.
- Session restoration on cold start, with a splash hold so the app never flashes the wrong route.

Routing, replacing the placeholder logic from prompt `04`:

- No session goes to `(onboarding)`.
- Session but onboarding incomplete goes to `(onboarding)`.
- Session and onboarded goes to `(tabs)/today`.

Rules from `docs/AGENTS.md` § Clerk Rules that are not negotiable: never store passwords, never build custom auth, never put a Clerk secret key in the app. Only the publishable key ships in the client. The Clerk user ID is the external identity and it is what the database will key on in prompt `08`.

Handle the states the mock did not: network failure, expired code, already-registered email, cancelled OAuth, and account not found on sign-in.
