---
id: erigon-build
name: Erigon Build
description: Build the Erigon binary using make. Use this when you need to compile erigon before running any erigon commands.
category: Blockchain
requires: []
examples:
  - "How do I build the Erigon binary?"
  - "Compile erigon from source"
---

# Build Erigon Binary

Build the Erigon binary by running `make erigon` from the repository root.

## Build Command

```bash
make erigon
```

This compiles the Erigon binary and places it at `./build/bin/erigon`.

## What the Build Does

- Compiles the main Erigon executable
- Sets version information from git (commit, branch, tag)
- Applies appropriate CGO flags for the platform
- Outputs binary to `build/bin/erigon`

## Prerequisites

- Go (version specified in go.mod)
- C compiler (gcc or clang)
- Make

## After Building

The binary will be available at:
```
./build/bin/erigon
```

You can verify the build by running:
```bash
./build/bin/erigon --version
```
