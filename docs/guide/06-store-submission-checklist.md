# 6. Store submission checklist

A list of things to check before you submit to the Apple App Store or Google Play. **Passing this list does not mean a store will approve your app.** Review decisions are the stores' and their rules change; read the current guidelines (Apple's App Store Review Guidelines and Google Play's Developer Program Policies) before you submit. Made by The Diamond Empire, https://thedempire.shop/

## What the starter was and was not tested on

The starter was verified with automated tests, a type check, lint, a web export opened in a desktop browser, and the size of the exported native bundles. It was **never run on a phone, a tablet, an emulator or a simulator**. Everything on this list that depends on a real device is yours to test.

## Before you build

- [ ] Replaced the sample name, bundle identifier (`ios.bundleIdentifier`) and package name (`android.package`) in `app.config.ts`.
- [ ] Replaced the icon, splash image and adaptive icon with your own artwork (guide 3), and checked the icon on a real home screen, including the Android adaptive shapes.
- [ ] Replaced all sample content (the invented catalog, the demo hint on the sign-in screen, the sample brand text).
- [ ] Connected a real backend (guide 4) and removed or hidden the demo account hint (`auth.signIn.demoHint` in both language files).
- [ ] Set the production values in the `production` profile in `eas.json`; no secret in any `EXPO_PUBLIC_` value.
- [ ] Ran `npm run typecheck`, `npm run lint` and `npm test`.
- [ ] Checked every sign-in, sign-up and password reset path against your real server, including error cases.

## On real devices

- [ ] Sign-in with the keyboard open and closed; the fields are not hidden by the keyboard.
- [ ] Safe areas: nothing is cut off by a notch, a camera cut-out or the home indicator, on a small phone and a large one.
- [ ] Screen reader pass with VoiceOver (iPhone) and TalkBack (Android): every control has a sensible name, the order makes sense, errors are announced.
- [ ] Largest system text size: nothing is cut off or overlaps. The starter stacks rows at large sizes and caps heading growth, but only a person can judge the result.
- [ ] Dark mode and light mode, and switching while the app is open.
- [ ] Offline: turn on airplane mode, open the app and use it; then turn it off and check recovery.
- [ ] The session survives closing and reopening the app (this depends on the secure store working on the device).
- [ ] Sign out removes the session and the saved personal data.

## Accounts, privacy and policy

- [ ] A **privacy policy** at a public web address, covering what your backend collects. Both stores ask for it.
- [ ] The **data safety form** (Google Play) and **App Privacy details** (Apple) answered to match what your app and your backend really do. The starter itself contains no analytics, advertising or tracking code, but your backend and any library you add may.
- [ ] If the app lets people create an account, check each store's current rules about **account deletion**: both stores have requirements about letting users delete their account. The starter has no delete-account feature. You must add one.
- [ ] If you add sign-in with a third party (Google, Apple, and so on), check the stores' rules on offering equivalent options.
- [ ] If you add notifications, ask for permission at a moment that makes sense. The starter's notification screen is a preferences screen only: it does not request permission or send anything.
- [ ] Age rating questionnaires answered honestly.

## Store listing

- [ ] App name, short and long description, keywords, support address and a support web page.
- [ ] Screenshots from real devices at the sizes the stores ask for, and an icon that follows their rules.
- [ ] Release notes for the first version.
- [ ] Test account details for the reviewers, if the app needs a sign-in to see anything. Make sure the account works against your production server on the day of review.

## After submitting

- [ ] Watch for questions from reviewers and answer promptly.
- [ ] Plan how you will ship fixes (a new build, or EAS Update for JavaScript-only changes; guide 5).
- [ ] Keep your signing credentials and store accounts safe and backed up. Losing them can block updates.

Nothing in this checklist is a promise about review outcomes, timing or approval.
