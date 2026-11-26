# PixelOS Signing Keys

Generated signing keys for PixelOS (LineageOS-based) builds.

## Installation

Via `repo sync` (manifest `snippets/marble.xml` already maps this repo to
`vendor/custom/signing`):

```bash
cd <rom-root>
mkdir -p vendor/lineage-priv
ln -s ../custom/signing vendor/lineage-priv/keys
```

Manual clone instead:

```bash
cd <rom-root>/vendor
git clone git@github.com:pavelc4-labs/vendor_custom_signing-keys.git custom_signing-keys
mkdir -p lineage-priv
ln -s ../custom_signing-keys lineage-priv/keys
```

The build system auto-includes `vendor/lineage-priv/keys/keys.mk`
(see `vendor/lineage/config/common.mk`).

## Usage

Sign only applies to `user` builds:

```bash
lunch <device>-user
make bacon
```

`eng`/`userdebug` builds fall back to the test key.

## Contents

- Platform keys: `releasekey`, `platform`, `shared`, `media`, `networkstack`,
  `bluetooth`, `sdk_sandbox`, `verity`, `testkey`, `signed`
- APEX certificates for all `com.android.*` modules (`.pk8` + `.x509.pem` +
  `.avbpubkey` + `.pem`), used to override `PRODUCT_CERTIFICATE_OVERRIDES`
- `keys.mk` wiring everything to `vendor/custom_signing-keys`
- `gen_keys.py` + `gen_keys_py/` to regenerate keys

## Regenerating keys

```bash
cd vendor/custom_signing-keys
python3 gen_keys.py
```

Requires pyOpenSSL (Python 3.14 on some distros ships a broken pyOpenSSL;
use a venv with pyOpenSSL >= 26 if it fails).

## Warning

This repo contains **private keys**. Keep it **private**, back it up, and
never leak it. Losing or exposing the keys invalidates OTA updates and
requires a full data wipe on devices.
