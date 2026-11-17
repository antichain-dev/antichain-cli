# Antichain CLI releases

This repository holds the releases of `antichain`, the Antichain command-line client.

## What a release holds

The version is in every file name. A release of version `<version>`, tagged `v<version>`, holds:

- `antichain_v<version>_linux_amd64`
- `antichain_v<version>_linux_arm64`
- `antichain_v<version>_darwin_arm64`
- `antichain_v<version>_darwin_amd64`
- `antichain_v<version>_SHA256SUMS`, the sha256 of each binary
- `antichain_v<version>_SHA256SUMS.sig`, the signature of `antichain_v<version>_SHA256SUMS` by the Antichain release key
- `antichain_v<version>_SHA256SUMS.sigstore.json`, the same signature as a Sigstore bundle

For example, the binary for macOS on Apple silicon in `v0.1.0-beta.12` is `antichain_v0.1.0-beta.12_darwin_arm64`, and its checksums file is `antichain_v0.1.0-beta.12_SHA256SUMS`.

## Verify a download

Download the binary, `antichain_v<version>_SHA256SUMS`, `antichain_v<version>_SHA256SUMS.sig` and `antichain_v<version>_SHA256SUMS.sigstore.json` from the release into one directory, save the `cosign.pub` in this repository beside them, and run the commands there. In each command, put the release's version in place of `<version>`.

Check the binary against `antichain_v<version>_SHA256SUMS` (on Linux, `sha256sum --ignore-missing -c antichain_v<version>_SHA256SUMS` runs the same check):

```sh
shasum -a 256 --ignore-missing -c antichain_v<version>_SHA256SUMS
```

Check the signature on `antichain_v<version>_SHA256SUMS` against `cosign.pub` with [cosign](https://docs.sigstore.dev/cosign/system_config/installation/) 3:

```sh
cosign verify-blob --key cosign.pub --bundle antichain_v<version>_SHA256SUMS.sigstore.json --insecure-ignore-tlog=true antichain_v<version>_SHA256SUMS
```

The signature is not in a public transparency log, so the check is just against the key; cosign prints a warning saying so and then `Verified OK`. Cosign 2 verifies `antichain_v<version>_SHA256SUMS.sig` instead, with `--signature antichain_v<version>_SHA256SUMS.sig` in place of `--bundle antichain_v<version>_SHA256SUMS.sigstore.json`.

Without cosign, OpenSSL verifies `antichain_v<version>_SHA256SUMS.sig`, which is base64, and prints `Verified OK`:

```sh
openssl base64 -d -A -in antichain_v<version>_SHA256SUMS.sig | openssl dgst -sha256 -verify cosign.pub -signature /dev/stdin antichain_v<version>_SHA256SUMS
```

## Documentation

The documentation is at https://antichain.dev/docs/cli/.

## Contributions and issues

This repository holds no source and takes no contributions.
