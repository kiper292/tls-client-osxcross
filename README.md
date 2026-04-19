# osxcross

Pre-built osxcross binaries for cross-compiling macOS binaries on Linux.

## What is osxcross?

[osxcross](https://github.com/tpoechtrager/osxcross) is a tool that allows building macOS applications on Linux. It provides clang-based cross-compilers that target macOS (amd64 and arm64).

Building osxcross from source takes several minutes. This repository provides pre-built binaries to speed up CI/CD workflows.

## Download

Download the latest release tarball from the [Releases](https://github.com/kiper292/osxcross-prebuilt/releases) page.

## Requirements

- Linux x86_64 (tested on Ubuntu 24.04 on GitHub Actions)
- macOS SDK: 15.0

## Usage

In GitHub Actions:

```yaml
- name: Download osxcross
  run: |
    URL=$(curl -sSL https://api.github.com/repos/kiper292/osxcross-prebuilt/releases/latest | jq -r '.assets[] | select(.name | endswith(".tar.gz")) | .browser_download_url')
    curl -sL "$URL" -o osxcross-target.tar.gz
    tar -xzf osxcross-target.tar.gz -C $HOME
    echo "$HOME/osxcross/target/bin" >> $GITHUB_PATH
```

Or if you need a specific version:

```yaml
- name: Download osxcross
  run: |
    VERSION=v1.0.0
    URL=$(curl -sSL "https://api.github.com/repos/kiper292/osxcross-prebuilt/releases/tags/$VERSION" | jq -r '.assets[] | select(.name | endswith(".tar.gz")) | .browser_download_url')
    curl -sL "$URL" -o osxcross-target.tar.gz
    tar -xzf osxcross-target.tar.gz -C $HOME
    echo "$HOME/osxcross/target/bin" >> $GITHUB_PATH
```

Then use the cross-compilers:

| Target | Compiler |
|--------|----------|
| macOS amd64 | `o64-clang` |
| macOS arm64 | `oa64-clang` |

Example for Go:

```bash
GOOS=darwin GOARCH=amd64 CC=o64-clang CGO_ENABLED=1 go build ...
GOOS=darwin GOARCH=arm64 CC=oa64-clang CGO_ENABLED=1 go build ...
```

## Building a New Release

Run the `release` workflow manually or push a tag matching `v*.*.*`.

## License

osxcross is licensed under the [Apache License 2.0](https://github.com/tpoechtrager/osxcross/blob/master/LICENSE).