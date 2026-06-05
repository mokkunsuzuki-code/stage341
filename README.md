# REMEDA Stage341

## Signed Verification Session Manifest

Stage341 extends Stage340 by signing the verification session manifest.

## Purpose

Stage340 created:

```text
qsp_session_result.json
↓
session_manifest.json
↓
local_witness.json
↓
session_anchor_receipt.json

Stage341 adds signatures and verification to the session manifest.

session_manifest.json
↓
GPG signature
↓
Ed25519 witness signature
↓
Sigstore bundle
↓
independent verification
What Stage341 Adds
GPG detached signature for session_manifest.json
Ed25519 witness signature
Ed25519 witness public key
allowed signers file
Sigstore bundle
Sigstore verification
session_signature_manifest.json
signed_session_manifest.json
Public Files
docs/session/session_manifest.json
docs/session/session_manifest.json.gpg.asc
docs/session/session_signature_manifest.json
docs/session/signed_session_manifest.json
docs/witnesses/session_manifest.ed25519.sig
docs/witnesses/stage341_ed25519_witness.pub
docs/witnesses/allowed_signers
docs/anchors/session_manifest.sigstore.bundle
Verification

GPG:

gpg --verify docs/session/session_manifest.json.gpg.asc docs/session/session_manifest.json

Ed25519 witness:

ssh-keygen -Y verify \
  -f docs/witnesses/allowed_signers \
  -I stage341-ed25519-witness \
  -n stage341-session-manifest \
  -s docs/witnesses/session_manifest.ed25519.sig \
  < docs/session/session_manifest.json

Sigstore:

cosign verify-blob \
  --bundle docs/anchors/session_manifest.sigstore.bundle \
  --certificate-identity "mokkun.suzuki@gmail.com" \
  --certificate-oidc-issuer "https://github.com/login/oauth" \
  docs/session/session_manifest.json
Safety Boundary

Stage341 does not publish:

private keys
attack code
dangerous prompts
exploit payloads
bypass procedures
automated attack logic
Meaning

Stage341 proves:

who signed the verification session manifest
whether it was modified
whether witness verification succeeds
whether Sigstore verification succeeds

This prepares the system for Stage342 External Anchor Layer.

License

MIT License

Copyright (c) 2025 Motohiro Suzuki
