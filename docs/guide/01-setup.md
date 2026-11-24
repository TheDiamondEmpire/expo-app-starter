# 1. Setup

Diamond Empire App Starter is an Expo (React Native) app written in TypeScript. This guide gets it running on your computer. It is made by The Diamond Empire, https://thedempire.shop/

## What you need

- **Node.js 22 or newer** and npm. Expo's own tooling lists Node 22.13+ or 24.3+ as the supported versions. The starter was built and tested on a newer odd-numbered Node release, which npm warns about but which worked; if you can, use an even-numbered release (22 or 24).
- **A phone with the Expo Go app, or an emulator.** Expo Go is free and installs from the app stores. The starter uses only libraries that Expo Go includes, so you can try it on a phone without building anything.
- **A code editor.** Any editor works. One with TypeScript support makes the work easier.

You do not need a Mac to work on the app. You do need one to run the iOS Simulator. To put an iPhone build on a real phone you use EAS, which builds in the cloud (guide 5).

## First run

Open a terminal in the project folder and run:

```text
npm ci
npm start
```

`npm ci` installs exactly the versions in `package-lock.json`. `npm start` starts the Expo development server and shows a QR code. Scan it with your phone (the Expo Go app on Android, the Camera app on iPhone). Press `w` in the terminal to open the web version in your browser instead.

You can sign in with the demo account. It exists only inside the built-in mock server (guide 4):

- Email: `demo@example.com`
- Password: `Diamond-123`

Or tap "Create an account" and make your own. The mock server keeps accounts on the device, so they stay until you clear the app data.

## What you will see

1. **Sign in, Create account, Forgot password.** Forms that check the input before anything is sent.
2. **Onboarding.** Three short screens, shown once per user on a device.
3. **Three tabs.** Catalog (a sample list you can pull down to refresh), Sync (the offline queue demo) and Settings (profile, theme, language, notification preferences).

Everything in the catalog is invented sample data. Replace it with your own (guide 2).

## Checks you can run

```text
npm run typecheck     # TypeScript, strict mode
npm run lint          # ESLint with the Expo rules
npm test              # unit and component tests (Jest and React Native Testing Library)
npm run export:web    # builds the web version into dist/web as a build check
```

These are the same commands the CI workflow in `.github/workflows/ci.yml` runs.

## Settings you can change

Copy `.env.example` to `.env` to change the defaults:

| Variable | Default | Meaning |
|---|---|---|
| `APP_ENV` | `development` | `development`, `staging` or `production`. Shown on the Settings screen. |
| `EXPO_PUBLIC_API_MODE` | `mock` | `mock` uses the built-in fake server. `http` uses the example HTTP adapter (guide 4). |
| `EXPO_PUBLIC_API_BASE_URL` | empty | The server address when the mode is `http`. |
| `EXPO_PUBLIC_MOCK_LATENCY_MS` | `400` | How long the fake server waits before answering, so you can see loading states. |

**Never put a secret in these variables.** Anything starting with `EXPO_PUBLIC_` is copied into the app, and anyone who has the app can read it. Secrets belong on your server.

## Known limits

- The starter was **never run on a phone or a simulator** by its makers. It was tested with automated tests and in a desktop browser through the web version. Things that only show up on a real device (keyboard behaviour, safe areas on a notch, the secure store, performance) are yours to check. See the store submission checklist (guide 6).
- The web version is useful for development and checks, but React Native on the web is not the same as the native app.
