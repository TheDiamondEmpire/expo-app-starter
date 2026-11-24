# 4. Replacing the mock backend

The screens never talk to a server directly. They talk to one interface, `ApiAdapter`, defined in `src/api/types.ts`. Out of the box that interface is implemented by an in-memory mock. To use your real backend you write one adapter and return it from one function. Made by The Diamond Empire, https://thedempire.shop/

## What the mock does

`src/api/mock-adapter.ts` behaves like a small server that lives inside the app:

- It has a demo account (`demo@example.com`, password `Diamond-123`). Sign-up creates more accounts.
- Every call waits a fake delay (400 ms by default, `EXPO_PUBLIC_MOCK_LATENCY_MS`) so you can see loading states.
- It returns the same errors a real server would: wrong password, email already registered, expired session, too many sign-in attempts, not found, invalid data.
- Accounts, sessions and favourites are saved in the device storage so they survive an app restart. Passwords are stored as a salted checksum, not in plain text, but **this is not real security**: it exists only to keep plain text out of the demo data. Never reuse its code to store real passwords.
- Tests can make a call fail on purpose: `mock.failNext('listItems', 'network', 2)` fails the next two `listItems` calls.

## What the interface asks for

`ApiAdapter` has nine methods:

| Method | Does |
|---|---|
| `signIn`, `signUp` | return a `Session` (a token and the user) |
| `signOut(token)` | ends the session |
| `getCurrentUser(token)` | checks the token and returns the user |
| `requestPasswordReset(email)` | starts a reset; should succeed whether or not the account exists |
| `updateProfile(token, patch)` | returns the changed user |
| `listItems`, `getItem`, `setFavorite` | the example catalog feature |

Every method rejects with an `ApiError`. The error `code` is one of `network`, `server`, `rate_limited`, `invalid_credentials`, `email_taken`, `unauthorized`, `not_found`, `validation` or `unknown`. The screens show a translated message for each code (`errors.*` in `src/i18n/en.ts`).

## Steps

1. **Start from the example.** `src/api/http-adapter.ts` is a complete adapter for an invented REST contract (`POST /auth/sign-in`, `GET /me`, `GET /items`, and so on). Copy it and change the routes, the request bodies and the way it reads responses to match your server. It was tested only against a fake `fetch`, never against a real server, so treat it as a worked example, not as a library.
2. **Map your server's answers to error codes.** In the example, `codeFor` turns HTTP statuses into codes. Pay attention to the two that matter most: `401` on the sign-in route means "wrong email or password" (`invalid_credentials`), while `401` anywhere else means "your session ended" (`unauthorized`); the app signs the user out on `unauthorized`.
3. **Choose your adapter.** Edit `createApi` in `src/api/index.ts`. Today it picks the mock or the example adapter from `EXPO_PUBLIC_API_MODE`. Add a branch, or replace the body, so it returns your adapter.
4. **Set the address.** Put your server address in `EXPO_PUBLIC_API_BASE_URL` (in `.env` for development, in the `env` section of the profile in `eas.json` for builds). It is public: it ends up in the app.
5. **Run the tests.** `src/api/client.ts` wraps your adapter: it turns any thrown error into an `ApiError` and retries reads and other repeat-safe calls (not sign-in or sign-up) up to twice with a growing delay. The tests in `tests/unit/http-adapter.test.ts` show how to test an adapter against a fake `fetch`; copy that file for yours.

## Things a real backend must get right

These are not done for you:

- **Tokens.** The session token is kept in the secure store (`expo-secure-store`: the iOS keychain and the Android keystore). Decide token lifetime, refresh and revocation on the server. The starter has no refresh flow.
- **Server-side validation.** The checks in the forms are for the user's comfort. The server must validate everything again.
- **Rate limiting and lock-out** belong on the server. The mock's limit exists only so you can see the screen.
- **HTTPS only.** Both iOS and Android block plain HTTP by default for good reasons.
- **Privacy.** If your server collects personal data, say so in your privacy policy and in the store forms (guide 6).

## The sync queue

Changes the user makes while the server cannot be reached (in the example, marking an item as a favourite) are kept in a queue on the device (`src/sync/queue.ts`) and shown on the Sync tab. **This part is a stub-level pattern, not a finished feature.** The queue persists, orders and de-duplicates changes and counts attempts. Replaying goes through your adapter (`createApiProcessor`), but it runs only when the user taps "Sync now". There is no automatic sync when the network returns, no background task and no conflict handling. Decide your own rules (last write wins, server wins, ask the user) before you rely on it.
