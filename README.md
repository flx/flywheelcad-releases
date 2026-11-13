# FlywheelCAD releases

Public download host for [FlywheelCAD](https://flywheelcad.com/).
The application source is private; only the built macOS disk images and Linux AppImages are published
here, as **release assets**.

## Downloads

**Always the newest build:** [FlywheelCAD.dmg](https://github.com/flx/flywheelcad-releases/releases/latest/download/FlywheelCAD.dmg) (macOS) ·
[FlywheelCAD-x86_64.AppImage](https://github.com/flx/flywheelcad-releases/releases/latest/download/FlywheelCAD-x86_64.AppImage) (Linux) — GitHub resolves each URL to
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

**macOS** (Apple silicon, macOS 15.6+): **0.21 and later are notarized by Apple** — it opens normally, with
no Gatekeeper warning. (0.20 is signed but not notarized and needs a
right-click → **Open** on first launch.) The build bundles its own CPython
runtime, so no system Python is required.

**Linux** (x86_64, built and tested on Ubuntu 26.04), from the first release
that lists an AppImage: `chmod +x FlywheelCAD-x86_64.AppImage` and run it. It
bundles GTK and its own CPython; GPU meshing uses the system Vulkan driver and
falls back to the CPU without one. If it does not start, install `fuse3`.
Verify it with `sha256sum` against the row below.

## Why this repo exists

The disk images are larger than the site host accepts as a single asset, so they
are published here as release assets instead of alongside the website.
