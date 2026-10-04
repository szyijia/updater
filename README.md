# Patchwing updater

[![License: MIT](https://img.shields.io/badge/license-MIT-blue.svg)](./LICENSE-MIT)
[![License: Apache](https://img.shields.io/badge/license-Apache-orange.svg)](./LICENSE-APACHE)

Native runtime and patch packaging tools for Patchwing. The Rust runtime exposes a C API and is linked into the Flutter engine delivered by the Patchwing SDK.

## App developer workflow

Install the public CLI from [patchwing.net](https://patchwing.net) and follow the [quick start](https://docs.patchwing.net/getting-started/):

```bash
pw create my_app --org com.example --platforms android,ios
cd my_app
pw release --platforms=android --artifact=apk
pw release --platforms=ios --export-method=development
```

After changing Dart code, publish a patch for the existing release version:

```bash
pw patch --platforms=android --release-version=1.0.0+1
pw patch --platforms=ios --release-version=1.0.0+1
```

Use the actual baseline version and normal platform signing requirements. A development-signed Release IPA is not an App Store distribution package.

The CLI downloads precompiled SDK tools. App developers do not need to compile this Rust library, access private compiler sources, or install Rust to use automatic updates. Downloaded patches activate in the original installation on a subsequent cold start; installing another APK or IPA is not patch verification.

## Repository components

- `library`: Native runtime that checks, downloads, validates and applies patches, with startup and rollback state management.
- `patch`: Native packaging executable distributed with the CLI SDK tools.
- `shorebird_code_push`: Optional Dart bindings for direct interaction with the runtime. This internal package name and the existing C ABI are retained; standard automatic updates do not require adding a manual dependency.

## SDK production

SDK publishers build this runtime with the exact Flutter, Dart and updater source revisions recorded in the release seal. Do not substitute an independently built runtime into another SDK version. Source checks, native build checks, packaged artifact integrity, public downloads and consumer/device acceptance are separate gates.

Existing license and copyright notices are preserved. Runtime algorithms and interfaces are not changed by product documentation or repository branding.
