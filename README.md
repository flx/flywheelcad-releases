# FlywheelCAD releases

Public download host for [FlywheelCAD](https://flywheelcad.com/).
The application source is private; only the built disk images are published here,
as **release assets**.

## Downloads

**Always the newest build:** [FlywheelCAD.dmg](https://github.com/flx/flywheelcad-releases/releases/latest/download/FlywheelCAD.dmg) — GitHub resolves that URL to
the newest release's asset of that name, so the download buttons on
flywheelcad.com point there and never change. The
versioned rows below are the same images under their versioned names, with
the checksum to verify against.

| Version | Download | SHA-256 |
|---|---|---|
| 0.35 (beta) | [FlywheelCAD-0.35.dmg](https://github.com/flx/flywheelcad-releases/releases/download/v0.35/FlywheelCAD-0.35.dmg) | `55a860e05f2d4ef59456a694e0efb81406d6061c8a41a52bbd2f9437989e571c` |
| 0.25 (beta) | [FlywheelCAD-0.25.dmg](https://github.com/flx/flywheelcad-releases/releases/download/v0.25/FlywheelCAD-0.25.dmg) | `fa586fcb48a7dd457c500896ff816a9cd68f47223dfe9db9f251ca64f858b884` |
| 0.23 (beta) | [FlywheelCAD-0.23.dmg](https://github.com/flx/flywheelcad-releases/releases/download/v0.23/FlywheelCAD-0.23.dmg) | `fb18c9dac50f1722fe36e218b1f35ef6a294f57c7ca19d7c7fc5ef2573128dd6` |
| 0.22 (beta) | [FlywheelCAD-0.22.dmg](https://github.com/flx/flywheelcad-releases/releases/download/v0.22/FlywheelCAD-0.22.dmg) | `6a9e3bf6d41837667a0a4f8ab710338ad9dc2515519a3587e1a21ee13e8094c0` |
| 0.21 (beta) | [FlywheelCAD-0.21.dmg](https://github.com/flx/flywheelcad-releases/releases/download/v0.21/FlywheelCAD-0.21.dmg) | `10f040ed9c71cdc5735c57cc0eeb2c7ac87a7ef3456f87910c1ee3b42c256cc2` |
| 0.20 (beta) | [FlywheelCAD-0.20.dmg](https://github.com/flx/flywheelcad-releases/releases/download/v0.20/FlywheelCAD-0.20.dmg) | `5372f1ae039942a69c6559ce0e48fa9629ec2987140bbb6ffc1ce35aa7c9b4f7` |

macOS on Apple silicon. **0.21 and later are notarized by Apple** — it opens normally, with
no Gatekeeper warning. (0.20 is signed but not notarized and needs a
right-click → **Open** on first launch.) The build bundles its own CPython
runtime, so no system Python is required.

## Why this repo exists

flywheelcad.com (like digitalhandstand.com before it) is deployed by Cloudflare Pages, which refuses any single
asset over **25 MiB** and fails the *entire* deploy when one exceeds it, silently
leaving the live site on the previous commit. Every build since 0.20 has been
around 29–30 MiB (0.23 is 29.3 MiB) — they bundle a Python runtime, where the
0.7–0.10 builds were 6–10 MiB and slipped under the limit. So the images cannot
live in the website repo.

## Publishing a new version

1. Build in the app repo: `scripts/package.sh` (notarizes and staples).
   It **auto-increments** `MARKETING_VERSION` on every run; pass
   `--no-version-bump` to re-package the same version.
2. Check the name you are about to publish. The asset takes the name of the file
   on disk (`file#Label` sets only a display label), and `package.sh` names the
   image by whether it notarized:

   * full run → `build/dmg/FlywheelCAD-<X.Y>.dmg` — already the public name,
     upload it as-is;
   * `--skip-notarize` → `build/dmg/FlywheelCAD-<X.Y>-UNNOTARIZED.dmg`, and that
     suffix is deliberate. **Do not rename it to the public name** — the
     filename is the only thing distinguishing an unnotarized dev build, and a
     rename publishes one as if it were notarized. Re-run without the flag.

   Verify before publishing, because "signed" and "notarized" fail differently
   and only the second is checked here:

       spctl -a -t open --context context:primary-signature build/dmg/FlywheelCAD-<X.Y>.dmg
       xcrun stapler validate build/dmg/FlywheelCAD-<X.Y>.dmg

   Expect `source=Notarized Developer ID` from the first and "The validate
   action worked!" from the second. **Both**: `spctl` also passes on a
   notarized-but-unstapled image, and the staple is what lets a first launch
   work with no network.

3. Publish it as release assets (nothing is committed to this repo) — BOTH
   the versioned image and the constant-name copy `package.sh` writes beside
   it, in one release:

       gh release create v<X.Y> build/dmg/FlywheelCAD-<X.Y>.dmg build/dmg/FlywheelCAD.dmg \
         --repo flx/flywheelcad-releases --title "FlywheelCAD <X.Y> (beta)"

   The copy is what the sites' "latest" link resolves to
   (`…/releases/latest/download/FlywheelCAD.dmg`); a release without it
   leaves the buttons serving the PREVIOUS version's copy. Do not mark the
   release `--prerelease` or leave it a draft — "latest" skips both.

4. Add the row to the table above. The hash is already computed —
   `package.sh` writes `build/dmg/FlywheelCAD-<X.Y>.dmg.sha256` next to the
   image, so copy it rather than re-running `shasum` against a different file
   than the one you uploaded.

5. The download buttons need NO edit: every one on flywheelcad.com links to
   the always-latest URL above. A newly uploaded asset can 404 for a few
   seconds while the CDN propagates.

   The User Manual, the component library page, the AI guide and the sample
   bundles on flywheelcad.com are COPIES from the app repo and go stale
   silently. Refresh them in the same sitting, from the website repo
   (`~/Documents/Website/flywheelcad`), pointing at the app checkout that was
   just released — the site pulls from the app; nothing in the app writes
   into the site:

       python3 scripts/import_app.py --app ~/Documents/Programming/swift/FlyWheelCADV3
       python3 scripts/check_excerpts.py --app ~/Documents/Programming/swift/FlyWheelCADV3

   then put the version in `partials/version.txt`, run `python3 scripts/chrome.py`,
   add the release post under `public/news/` (and to `news/index.html`,
   `news/feed.xml` and the home page's news list), commit, and push `main`
   (its pre-push hook runs `scripts/chrome.py --check --links`). No `?v=`
   cache-busting: Pages revalidates every file on each request. At 0.23 the
   published AI guide was found five commits behind its source, so this is a
   real failure mode and not a formality.
