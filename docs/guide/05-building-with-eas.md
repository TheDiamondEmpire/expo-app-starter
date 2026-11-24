# 5. Building with EAS (in plain words)

Expo Application Services (EAS) is Expo's cloud service that turns your code into an app file you can install: an `.apk` or `.aab` for Android and an `.ipa` for iPhone. You do not need Xcode or Android Studio for this. Made by The Diamond Empire, https://thedempire.shop/

**Important:** the starter's makers did not run EAS or any cloud build. `eas.json` is a skeleton with no credentials. The steps below follow Expo's documented process; check Expo's current documentation (https://docs.expo.dev/build/introduction/) because commands and requirements change.

## Words you will meet

- **Build**: EAS compiles the app on its servers and gives you a file.
- **Profile**: a named set of build options in `eas.json`. The skeleton has `development`, `preview` and `production`.
- **Development build**: a build that contains the developer tools and lets you load your code from your computer. Use it when Expo Go is not enough.
- **Internal distribution**: a build you share by link with your team and testers, without going through a store.
- **Credentials**: the signing certificates and keys the stores require. EAS can create and store them for you if you let it. They are never in this project and never should be.
- **Store submission**: sending the finished build to the Apple App Store or Google Play (`eas submit`).

## What you need before the first build

1. An **Expo account** (free) and the EAS command line: run `npx eas-cli login`.
2. For Android: a **Google Play Console** account (a one-time registration fee) to publish. You can build and install a test file without one.
3. For iPhone: an **Apple Developer Program** membership (an annual fee). Even test builds on real iPhones need it.
4. Your own **bundle identifier** and **package name** in `app.config.ts` (the sample `shop.thedempire.starter` is a placeholder; change it before the first build).

## The steps

```text
npx eas-cli login
npx eas-cli build:configure
npx eas-cli build --profile preview --platform android
```

1. `build:configure` links the project to your Expo account and checks `eas.json`.
2. `build --profile preview` makes an internal test build. EAS asks whether it may create signing credentials; answering yes is the simplest route.
3. When the build finishes, EAS gives you a link and a QR code to install it on a test device.
4. For the stores, run `build --profile production`, test the result, then run `npx eas-cli submit --platform android` (or `ios`) and follow the prompts.

## What the three profiles do in this project

| Profile | Meant for | Notes |
|---|---|---|
| `development` | you, while building | `developmentClient: true`, internal distribution, `APP_ENV=development` |
| `preview` | testers | internal distribution, `APP_ENV=staging` |
| `production` | the stores | `autoIncrement` raises the build number each time, `APP_ENV=production` |

Set `EXPO_PUBLIC_API_MODE` and `EXPO_PUBLIC_API_BASE_URL` in each profile's `env` block so test builds use your test server and production uses your real one. Those values are public. **Never put a secret in `eas.json` or in a variable that starts with `EXPO_PUBLIC_`.** Use EAS secrets (`eas secret:create`, see Expo's documentation) only for build-time values that must not be in the repository; remember that anything the app needs at run time is inside the app and can be read.

## Updates without a store release

EAS Update can send changes to the JavaScript and images of an already-installed app without a store review. It cannot change native code (adding a library with native code needs a new build). The starter does not configure EAS Update; see Expo's documentation if you want it.

## Costs and limits

EAS has a free tier with limits on builds and a queue; paid plans exist. Store accounts have their own fees. Prices and limits change, so read Expo's pricing page rather than relying on a number here.
