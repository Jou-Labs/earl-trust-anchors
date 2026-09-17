# EARL Trail-of-Bits Evaluation Packet v3.1 — Out-of-Band Trust Anchors

Issued 2026-09-17 by Jou Labs. Compare these values against what the evidence
package reports. A package that verifies WITHOUT these pins is only
SELF-CONSISTENT; verified AGAINST these pins, it is authenticated to the Jou
Labs producer.

Packet: `EARL-ToB-evaluation-packet-v3.1-2026-09-17.zip`
(v3.1 is the corrective reissue of v3: replay grouped by source archive + probe
two-pass; correct authored-vs-offered accounting; open-world `--days 0`; the
observatory regression + streaming check now ship (13 checks); verification
wording scoped; a complete build-verified corrected source archive.)

```
SIGNED ZIP SHA-256
  a07172369f3f2f89a1bbdf0f367647a6a98c701498d809631f89fb35692a3ed3
ROOT MANIFEST SHA-256 (MANIFEST.sha256; covers all 225 packet files)
  efc7edc18f328abc11c49eb905a9f35d88707d321cc25ce5cfdb73f54312d475
PUBLISHER Ed25519 PUBLIC KEY (base64; signs PACKET-SIGNATURE.json and the observatory)
  Iw1tIgNyOBr0OCoB0uHwsPQpz4J9JM1ObfXaoDnwjq4=
PUBLISHER KEY FINGERPRINT (SHA-256 of the raw 32-byte public key)
  e21ba43885cccc617fb1b3b07b1cfe9065ad9d722f7723cb338169161d00d899
CORRECTED SOURCE ARCHIVE SHA-256 (07-source-archives/earl-src-corrected-10d5b06f.tar.gz)
  3ad6d2ab0cc33173e7b4a8c9799488916164a23b2ec4f52df147ef9717005f22
```

Source provenance (externally retrievable at `github.com/Jou-Labs/earl`):

```
DELIVERY TIP COMMIT (branch genesis/g1-founder-genome)
  b48de4e64247973df374912bcbdc2ac77c7026ae
CORRECTED-HARNESS BUILD-CLOSURE COMMIT (the corrected source archive; builds with --locked)
  10d5b06f0c120588f52870bbecbfb26756d59a3c
```

## Verify the packet on your own machine (no Jou Labs service)

```
# 1. the manifest self-checks; then confirm its digest equals the pin above
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

# 3. the corrected harness builds standalone (no GitHub checkout)
tar xzf 07-source-archives/earl-src-corrected-10d5b06f.tar.gz -C build-corrected
cd build-corrected && cargo build --release --locked -p earl-world
```

## Scope note (read before drawing conclusions)

Delivered as an **experimental baseline plus prospective fixes and open findings
for independent review** — not "fully corrected" or "validation complete." The
corrected harness is build-verified, but a **native clean-machine replay diff**
against the historical ledgers is NOT RUN in this packet, and the containment /
assurance items (host and signing-key privileges, full gate-conjunct
reconstruction, preregistration chronology, cryptographic review, beacon
authentication) remain OPEN. All statuses are in the packet's `RELEASE-MATRIX.md`.
