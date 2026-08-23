# FlywheelCAD releases

Public download host for [FlywheelCAD](https://digitalhandstand.com/flywheelcad/).
The application source lives in a private repository; only the built disk images
are published here.

## Current

| Version | File | Size | SHA-256 |
|---|---|---|---|
| 0.20 (beta) | [`FlywheelCAD-0.20.dmg`](FlywheelCAD-0.20.dmg) | 29.5 MiB | `5372f1ae039942a69c6559ce0e48fa9629ec2987140bbb6ffc1ce35aa7c9b4f7` |

Apple silicon. Signed with a Developer ID but **not notarized**, so macOS warns
on first launch — right-click the app and choose **Open**. The build bundles its
own CPython runtime, so no system Python is needed.

## Why this repo exists

The website is deployed by Cloudflare Pages, which refuses any single asset over
**25 MiB** and fails the *entire* deploy when one exceeds it — silently leaving
the live site on the previous commit. FlywheelCAD 0.20 is 29.5 MiB (the bundled
Python runtime; the 0.7–0.10 builds were 6–10 MiB and slipped under). So the
disk images cannot live in the website repo.

## Publishing a new version

1. Build: `scripts/package.sh --skip-notarize` in the app repo. It
   **auto-increments** `MARKETING_VERSION` on every run — pass
   `--no-version-bump` to re-package the same version.
2. Copy `build/dmg/FlywheelCAD-<X.Y>-UNNOTARIZED.dmg` here as
   `FlywheelCAD-<X.Y>.dmg`, update the table above, commit and push.
3. Point the download button in the website repo at the new file:
   `https://github.com/flx/flywheelcad-releases/raw/main/FlywheelCAD-<X.Y>.dmg`

Keep older versions or delete them; nothing links to them once the button moves.
