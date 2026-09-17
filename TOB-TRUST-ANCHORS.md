# EARL Trail-of-Bits Evaluation Packet v3 — Out-of-Band Trust Anchors

Issued 2026-09-17 20:23 UTC by Jou Labs. Compare these values against what the
evidence package reports. A package that verifies WITHOUT these pins is only
SELF-CONSISTENT; verified AGAINST these pins, it is authenticated to the Jou
Labs producer.

Packet: `EARL-ToB-evaluation-packet-v3-2026-09-17.zip`

```
SIGNED ZIP SHA-256
  bcf1596db5c7e77fe037b272165352e16e77590dfcebe251474ca36bdde26915
ROOT MANIFEST SHA-256 (MANIFEST.sha256; covers all 223 packet files)
  31f1565c31f77a16a78eaa6eebf3a5cf60aa499368c2e60ce4c4dd9da71e43a7
PUBLISHER Ed25519 PUBLIC KEY (base64; signs PACKET-SIGNATURE.json and the observatory)
  Iw1tIgNyOBr0OCoB0uHwsPQpz4J9JM1ObfXaoDnwjq4=
PUBLISHER KEY FINGERPRINT (SHA-256 of the raw 32-byte public key)
  e21ba43885cccc617fb1b3b07b1cfe9065ad9d722f7723cb338169161d00d899
```

Source provenance (externally retrievable at `github.com/Jou-Labs/earl`):

```
DELIVERY TIP COMMIT (branch genesis/g1-founder-genome)
  86a2ee7290d5f928081b96f09c8fe716d2f12143
CORRECTED-HARNESS BUILD-CLOSURE COMMIT (pinned by 07-source-archives/corrected-snapshot)
  c003ef67d86cbbd27f3bf68e27873a34e0acccfd
```

## Verify the packet on your own machine (no Jou Labs service)

```
# 1. the manifest self-checks (every file matches its sha256 line)
#    then confirm the manifest digest equals the pin above
sha256sum MANIFEST.sha256          # must equal the ROOT MANIFEST SHA-256

# 2. the detached publisher signature verifies over the manifest bytes,
#    using ONLY the packet's bundled stdlib verifier + this published key
python - <<'PY'
import json, base64, sys
sys.path.insert(0, "05-verification-tools")
import ed25519_pure as ed
s = json.load(open("PACKET-SIGNATURE.json"))
msg = open("MANIFEST.sha256", "rb").read()
assert s["pubkey_b64"] == "Iw1tIgNyOBr0OCoB0uHwsPQpz4J9JM1ObfXaoDnwjq4="
print("signature verifies:",
      ed.verify(base64.b64decode(s["sig_b64"]), msg, base64.b64decode(s["pubkey_b64"])))
PY
```

A pass authenticates the packet bytes to the holder of the published key above.

## Scope note (read before drawing conclusions)

This packet is delivered as an **experimental baseline plus prospective fixes
and open findings for independent review** — not "fully corrected" or
"validation complete." Clean-machine native build reproduction (V2-2) and the
containment/assurance items (V2-8: host and signing-key privileges, full
gate-conjunct reconstruction, preregistration chronology, cryptographic review,
beacon authentication) remain OPEN and are labeled so in the packet's
`RELEASE-MATRIX.md`.
