# 3. Theming and branding

Made by The Diamond Empire, https://thedempire.shop/

## Colours, spacing and type sizes

All design values are in one file: `src/theme/tokens.json`. It has a `light` and a `dark` palette with the same names, plus spacing, corner radius, font sizes and the minimum touch target.

To rebrand, change the hex colours in both palettes. Use six-digit hex values (`#b8106f`). The names say what a colour is for:

| Name | Used for |
|---|---|
| `background` | the screen |
| `surface`, `surfaceAlt` | cards, list rows, inputs, pressed rows |
| `text`, `textMuted` | normal and secondary text |
| `primary`, `onPrimary` | buttons and the selected option, and the text on them |
| `link` | link-style buttons and the active tab |
| `border` | input and option borders |
| `danger`, `success` | error and success messages |
| `focus` | the keyboard focus ring |
| `tabBar` | the tab bar and header background |

## Check your colours

After you change colours, run:

```text
npm install --prefix tools
npm run contrast
```

The script reads `tokens.json`, computes the contrast ratio of every foreground and background pair the components use, in both themes, and fails if one is below the WCAG 2.x minimum for its use (4.5:1 for normal text, 3:1 for large text and for the borders and icons that identify a control). Add a pair to `PAIRS` in `tools/contrast.mjs` when you add a new combination. A contrast check covers colour only; it does not tell you whether a screen is usable with a screen reader or a switch.

## Light, dark and system

The Settings screen offers System, Light and Dark. The choice is saved on the device (`src/settings`). System follows the phone's appearance setting. The theme provider is `src/theme/ThemeProvider.tsx`; components read colours with `useTheme()` and never hard-code a colour.

## Fonts

The app uses the system font on every platform, so nothing needs licensing and nothing is downloaded. If you add a custom font, load it with `expo-font`, check its licence, and record it in `THIRD-PARTY.md`.

## Icon, splash screen and favicon

The sample artwork is drawn as SVG in `assets/src/`:

- `icon.svg`: the app icon (1024 by 1024, with a background)
- `adaptive-foreground.svg`: the Android adaptive icon foreground (transparent, keep the artwork inside the centre 70 percent)
- `splash.svg`: the splash image (transparent)

To use your own artwork, replace those three files (keep them 1024 by 1024) and run:

```text
npm install --prefix tools
npm run assets
```

The script draws each SVG in your installed Chrome or Edge and saves `assets/icon.png`, `assets/adaptive-icon.png`, `assets/splash-icon.png` and `assets/favicon.png`. The splash background colour and the Android icon background colour are set in `app.config.ts` (`#000624` in the sample). Change them to match.

## App name, identifiers and links

In `app.config.ts`:

- `name` is shown under the icon. `slug` and `scheme` identify the project and deep links.
- `ios.bundleIdentifier` and `android.package` are your app's permanent identifiers in the stores. The sample value `shop.thedempire.starter` is a placeholder: **change it before your first store build**, and do not change it afterwards.
- The version is `version`. EAS can raise the build number for you (`eas.json`, guide 5).

## Replacing the sample brand name in text

The words "Diamond Empire" appear in `src/i18n/en.ts` and `src/i18n/es.ts` (`app.name`), in `app.config.ts`, in `package.json` and in the file headers. Search for it and replace it with your brand, and replace the sample catalog content.
