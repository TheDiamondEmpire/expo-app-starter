**Web export in Chrome (axe-core, every screen of the main flow)**

| Viewport and scheme | Screens scanned | axe violations | axe "needs review" items | Screens with horizontal scroll | Flow steps that failed | Console warnings or errors | Failed requests | Hosts contacted |
|---|---:|---:|---:|---:|---:|---:|---:|---|
| 390 px, light | 14 | 0 | 0 | 0 | 0 | 0 | 0 | 1 (the local test server only: yes) |
| 390 px, dark | 13 | 0 | 0 | 0 | 0 | 0 | 0 | 1 (the local test server only: yes) |
| 1280 px, light | 13 | 0 | 0 | 0 | 0 | 0 | 0 | 1 (the local test server only: yes) |
| 1280 px, dark | 13 | 0 | 0 | 0 | 0 | 0 | 0 | 1 (the local test server only: yes) |

axe-core 4.14.0, default rule set (including best-practice rules), 53 screen scans in total. "Needs review" items are results axe could not decide by itself (see Limits).

**Bundle sizes (exported files, never run on a device)**

| Export | Measure | Bytes | Gzip bytes | Budget (set before measuring) |
|---|---|---:|---:|---:|
| Web | all JavaScript (19 files) | 1,291,426 | 348,635 | 3,000,000 / 900,000 gzip |
| Web | largest single file | 1,116,475 | 293,279 | none |
| Web | all files (40) | 1,327,669 | 362,312 | 3,500,000 |
| iOS | Hermes bytecode bundle (1 file) | 2,844,562 | 1,235,469 | 9,000,000 |
| Android | Hermes bytecode bundle (1 file) | 2,830,090 | 1,230,560 | 9,000,000 |
| Native | 33 asset files shared by both platforms | 26,760 | | none |

Over budget: nothing.

**Other checks**

- Contrast from tokens: 48 of 48 pairs pass (24 pairs in each of 2 themes); the smallest margin is `link` on `surfaceAlt` (light) = 5.01:1 (needs 4.5:1).
- Packages in the lock file: 1367; production tree 603, of which 23 outside the permissive set; development-only 764, of which 10 outside the permissive set. Direct production dependencies: 17, all permissive: yes.
- Generated assets: 4 PNG files, signature and size correct: yes, identical to a fresh regeneration: yes; largest 23,314 bytes.
- Absolute local paths or user names in any project file: 0 of 125 files scanned; author rule problems: 0 of 110 customer files; unexpected URLs in source: 0; source files that call a network API: src/api/http-adapter.ts; tracking hits: 0.
- Customer ZIP: `dist/expo-app-starter-0.1.0.zip`, 110 files, 312,296 bytes, identical on two builds: yes; contains node_modules: no; contains results: no.

Run: 2026-11-03T22:46:56.740Z to 2026-11-03T22:49:22.252Z; chrome 156.0.8078.80 via playwright-core 1.64.1; Node v25.9.0; Expo 58.0.22, Expo Router 58.0.29, React Native 0.88.1, React Native Web 0.21.4; win32 10.0.26200 x64.

| Suite | Command | Tests | Passed | Failed |
|---|---|---:|---:|---:|
| jest (unit, component, app) | `npm test` | 191 | 191 | 0 |
| static (paths, docs, assets, licences, package) | `npm run test:static` | 78 | 78 | 0 |
| web (axe-core in Chrome on the web export) | `npm run test:web` | 11 | 11 | 0 |
| **Total** | | 280 | 280 | 0 |
