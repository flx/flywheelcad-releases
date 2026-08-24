# FlywheelCAD releases

Public download host for [FlywheelCAD](https://digitalhandstand.com/flywheelcad/).
The application source is private; only the built disk images are published here,
as **release assets**.

## Downloads

| Version | Download | SHA-256 |
|---|---|---|
| 0.21 (beta) | [FlywheelCAD-0.21.dmg](https://github.com/flx/flywheelcad-releases/releases/download/v0.21/FlywheelCAD-0.21.dmg) | `10f040ed9c71cdc5735c57cc0eeb2c7ac87a7ef3456f87910c1ee3b42c256cc2` |
| 0.20 (beta) | [FlywheelCAD-0.20.dmg](https://github.com/flx/flywheelcad-releases/releases/download/v0.20/FlywheelCAD-0.20.dmg) | `5372f1ae039942a69c6559ce0e48fa9629ec2987140bbb6ffc1ce35aa7c9b4f7` |

macOS on Apple silicon. **0.21 is notarized by Apple** — it opens normally, with
no Gatekeeper warning. (0.20 is signed but not notarized and needs a
right-click → **Open** on first launch.) The build bundles its own CPython
runtime, so no system Python is required.

## Why this repo exists

digitalhandstand.com is deployed by Cloudflare Pages, which refuses any single
asset over **25 MiB** and fails the *entire* deploy when one exceeds it, silently
leaving the live site on the previous commit. FlywheelCAD 0.20 is 29.5 MiB — it
bundles a Python runtime, where the 0.7–0.10 builds were 6–10 MiB and slipped
under the limit. So the images cannot live in the website repo.

## Publishing a new version

1. Build in the app repo: `scripts/package.sh` (notarizes and staples).
   It **auto-increments** `MARKETING_VERSION` on every run; pass
   `--no-version-bump` to re-package the same version.
2. Copy the output to the exact public filename — the asset takes the name of
   the file on disk, and `file#Label` sets only a display label:

       cp build/dmg/FlywheelCAD-<X.Y>-UNNOTARIZED.dmg /tmp/FlywheelCAD-<X.Y>.dmg

3. Publish it as a release asset (nothing is committed to this repo):

       gh release create v<X.Y> /tmp/FlywheelCAD-<X.Y>.dmg \
         --repo flx/flywheelcad-releases --title "FlywheelCAD <X.Y> (beta)"

4. Point the website download button at:

       https://github.com/flx/flywheelcad-releases/releases/download/v<X.Y>/FlywheelCAD-<X.Y>.dmg

   A newly uploaded asset can 404 for a few seconds while the CDN propagates.
