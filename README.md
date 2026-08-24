# T3 Code Linux ARM64 update feed

This repository builds the unmodified upstream [T3 Code](https://github.com/pingdotgg/t3code)
nightly source for Linux ARM64 and publishes the AppImage metadata expected by
`electron-updater`.

The upstream release currently publishes a Linux x86_64 AppImage but no Linux ARM64 artifact.
This feed exists so a locally built ARM64 desktop app can use T3 Code's normal in-app update flow.

## How it works

- A scheduled workflow checks the upstream releases every three hours.
- It checks out the exact newest nightly tag and builds on a native GitHub-hosted ARM64 runner.
- The build embeds this repository as its update provider.
- The workflow publishes the ARM64 AppImage, `nightly-linux-arm64.yml`, and SHA-256 checksum.
- Build and publish jobs have separate permissions, so upstream build scripts never receive a
  token that can write to this repository.

Releases are unofficial architecture builds. T3 Code itself remains copyright T3 Tools Inc. and
is distributed under its upstream MIT license.

