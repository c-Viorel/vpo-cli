# vpo

`vpo` is the command-line tool of **VPO Core**, VPO Labs' crash reporting,
usage analytics, feedback and in-app messages service for Mac apps. It
signs in to VPO Admin, registers an app and repairs its set-up
(`vpo enroll`), recognises an app by its bundle id (`vpo app list`), and
uploads symbols and event schemas on every release.

This repository holds **signed binary releases only**: the source of
`vpo`, VPOCoreKit and the server is private.

- macOS 14 or later, Apple silicon and Intel (one universal binary).
- Signed with VPO Labs' Developer ID (team `X34UB4R785`, identifier
  `com.vpolabs.vpo`) and the hardened runtime, and notarized. The install
  below refuses a binary that is not.

## Install

Each [release](https://github.com/c-Viorel/vpo-cli/releases) has
`vpo-<version>-macos.tar.gz`, its dSYM and `SHA256SUMS`. In Terminal:

```sh
d="$(mktemp -d)" && cd "$d" \
  && curl -fsSLO https://github.com/c-Viorel/vpo-cli/releases/download/1.9.0/vpo-1.9.0-macos.tar.gz \
  && curl -fsSL https://github.com/c-Viorel/vpo-cli/releases/download/1.9.0/SHA256SUMS | grep " vpo-1.9.0-macos.tar.gz$" | shasum -a 256 -c - \
  && tar -xzf vpo-1.9.0-macos.tar.gz vpo \
  && codesign --verify --strict -R='anchor apple generic and certificate 1[field.1.2.840.113635.100.6.2.6] exists and certificate leaf[field.1.2.840.113635.100.6.1.13] exists and certificate leaf[subject.OU] = "X34UB4R785" and identifier "com.vpolabs.vpo"' vpo \
  && mkdir -p ~/.local/bin && install -m 755 vpo ~/.local/bin/vpo \
  && ~/.local/bin/vpo version
```

If Terminal then says `vpo: command not found`, add `~/.local/bin` to your
PATH: `echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.zshrc`.

## Use it

The easiest way is to let your AI coding agent do it. In the app's project
folder, give it this prompt:

> Add VPOCoreKit to this project: read https://core.vpolabs.com/agent.md and follow it.

The agent installs `vpo` from here (checking its SHA-256 and its
signature), recognises the
app, and asks you to run `vpo login` once: VPO Admin opens in your browser,
you check the code and click **Allow**, and `vpo` writes the app's
enrollment token to the repository's `secrets/` folder without showing it
to anyone.

**Get started** (VPO Labs, signed in): https://admin.vpolabs.com/apps/vpocore/get-started

## License

Proprietary; see [LICENSE](LICENSE).
