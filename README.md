# T3 Code Linux ARM64 update feed

This repository builds the exact upstream [T3 Code](https://github.com/pingdotgg/t3code)
nightly source for Linux ARM64 and publishes the AppImage metadata expected by
`electron-updater`. The build applies one packaging-only setting from
[upstream PR #715](https://github.com/pingdotgg/t3code/pull/715): electron-builder's static
AppImage runtime, which avoids requiring `libfuse2` on the host.

The upstream release currently publishes a Linux x86_64 AppImage but no Linux ARM64 artifact.
This feed exists so a locally built ARM64 desktop app can use T3 Code's normal in-app update flow.
When upstream starts publishing the expected ARM64 AppImage and updater manifest, the workflow
mirrors those official files instead of rebuilding them. Installing that release switches the
embedded updater back to `pingdotgg/t3code`, so existing installations hand off without a manual
reinstall.

## How it works

- A scheduled workflow checks the upstream releases every three hours.
- If the release has no official Linux ARM64 files, it checks out the exact nightly tag and builds
  on a native GitHub-hosted ARM64 runner.
- It applies the tracked static-runtime patch before packaging; application source is unchanged.
- The build embeds this repository as its update provider.
- The workflow publishes the ARM64 AppImage, `nightly-linux-arm64.yml`, and SHA-256 checksum.
- If an official ARM64 AppImage and manifest are present, it validates their GitHub digests,
  architecture, version, and embedded upstream update provider before mirroring them unchanged.
- Build and publish jobs have separate permissions, so upstream build scripts never receive a
  token that can write to this repository.

Releases are unofficial architecture builds. T3 Code itself remains copyright T3 Tools Inc. and
is distributed under its upstream MIT license.
