# 2. Project structure

Everything the app does is in `src/`. This guide says where things are so you can change them. Made by The Diamond Empire, https://thedempire.shop/

```text
src/
  app/            the screens. Every file is a route (Expo Router)
    _layout.tsx   providers, splash screen, and the protected-route gate
    (auth)/       sign-in, sign-up, forgot-password (only for signed-out users)
    onboarding.tsx  shown once after the first sign-in
    (app)/        the signed-in area
      (tabs)/     Catalog, Sync, Settings tabs
      item/[id].tsx, edit-profile.tsx, notifications.tsx   screens opened on top of the tabs
  api/            the backend interface, the mock server, the HTTP example, retry logic
  auth/           AuthProvider: session, sign-in, sign-up, sign-out, onboarding flag
  config/         reads and checks the settings from app.config.ts
  core/           services.tsx: builds the storage, secure storage, API and sync queue
  features/catalog/  the example feature (list, cache, favourites)
  hooks/          small hooks (large text detection)
  i18n/           English and Spanish text, and the translate function
  navigation/     route names and the navigation theme
  settings/       theme, language and notification preferences, saved on the device
  storage/        key-value storage, secure storage, JSON helpers
  sync/           the offline sync queue (a stub-level pattern)
  theme/          colour tokens (tokens.json), spacing, radius and the theme provider
  ui/             buttons, text fields, screens, loading, empty and error views, art
  validation/     form rules and the useForm hook
tests/            unit, component and app-level tests
assets/           the icon, splash image and favicon (PNG) and their SVG sources in assets/src/
```

## How the screens are protected

`src/app/_layout.tsx` wraps everything in providers and then picks which group of screens exists:

- Signed out: only `(auth)` exists. Any other address redirects to sign-in.
- Signed in, onboarding not finished: only `onboarding` exists.
- Signed in and onboarded: only `(app)` exists.

This uses `Stack.Protected` from Expo Router. When the session changes (sign in, sign out, onboarding done) the groups change and the router moves the user to the right place. To protect a new screen, put it in `(app)`.

## Adding a screen

1. Make a file in `src/app/(app)/`, for example `src/app/(app)/orders.tsx`. Its default export is the screen.
2. Add its name to `src/app/(app)/_layout.tsx` if you want a title in the header.
3. Add its path to `src/navigation/routes.ts` and navigate with `router.push(routes.orders)`.
4. Use the shared parts from `@/ui` (`Screen`, `Text`, `Button`, `TextField`) so it follows the theme, text scaling and accessibility rules.

## Adding a tab

Add a file in `src/app/(app)/(tabs)/`, add a `Tabs.Screen` with a title and icon in the `_layout.tsx` next to it, and add the title to both language files.

## Replacing the example feature

The catalog is the example feature. To replace it:

1. Change the types in `src/api/types.ts` (`CatalogItem`, `CatalogApi`).
2. Change the sample data in `src/api/catalog-data.ts` and the mock in `src/api/mock-adapter.ts`.
3. Change the hook in `src/features/catalog/useCatalog.ts` and the screens that use it.

Keep the pattern: show saved data first, ask the server, keep the saved data if the server cannot be reached.

## Forms

`src/validation/rules.ts` holds the rules (they return a message key, not text, so errors follow the language). `src/validation/useForm.ts` holds the form state: values, errors shown after a field was left or a submit was attempted, a `submitting` flag that blocks a double submit, and focus moving to the first invalid field.

## Text and languages

All visible text is in `src/i18n/en.ts` and `src/i18n/es.ts`. Add a key to `en.ts` first; the Spanish file will not type-check until it has the same key, and a test checks that the `{placeholders}` match. To add a language, add a file, add it to `dictionaries` and `LANGUAGES` in `src/i18n/index.tsx`, and add an option in the Settings screen.

## Accessibility defaults

The shared components set roles, labels and states, and text scales with the system font size. If you build a new component, keep the same habits: give every control an `accessibilityLabel` and `accessibilityRole`, keep touch targets at least 48 points tall, and never use fixed heights for text. Automated tests find only part of the accessibility problems; test with a screen reader on a real device.
