# Antichain CLI releases

This repository holds the releases of `antichain`, the Antichain command-line client.

## What a release holds

- `antichain-linux-amd64`
- `antichain-linux-arm64`
- `antichain-darwin-arm64`
- `antichain-darwin-amd64`
- `SHA256SUMS`, the sha256 of each binary
- `SHA256SUMS.sig`, the signature of `SHA256SUMS` by the Antichain release key
- `SHA256SUMS.sigstore.json`, the same signature as a Sigstore bundle

## Verify a download

Download the binary, `SHA256SUMS`, `SHA256SUMS.sig` and `SHA256SUMS.sigstore.json` from the release into one directory, save the `cosign.pub` in this repository beside them, and run the commands there.

Check the binary against `SHA256SUMS` (on Linux, `sha256sum --ignore-missing -c SHA256SUMS` runs the same check):

```sh
shasum -a 256 --ignore-missing -c SHA256SUMS
```

Check the signature on `SHA256SUMS` against `cosign.pub` with [cosign](https://docs.sigstore.dev/cosign/system_config/installation/) 3:

```sh
cosign verify-blob --key cosign.pub --bundle SHA256SUMS.sigstore.json --insecure-ignore-tlog=true SHA256SUMS
```

The signature is not in a public transparency log, so the check is just against the key; cosign prints a warning saying so and then `Verified OK`. Cosign 2 verifies `SHA256SUMS.sig` instead, with `--signature SHA256SUMS.sig` in place of `--bundle SHA256SUMS.sigstore.json`.

Without cosign, OpenSSL verifies `SHA256SUMS.sig`, which is base64, and prints `Verified OK`:

```sh
openssl base64 -d -A -in SHA256SUMS.sig | openssl dgst -sha256 -verify cosign.pub -signature /dev/stdin SHA256SUMS
```

## Documentation

The documentation is at https://antichain.dev/docs/cli/.

## Contributions and issues

This repository holds no source and takes no contributions.
