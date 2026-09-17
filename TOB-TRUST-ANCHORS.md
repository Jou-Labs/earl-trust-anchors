# EARL Trail-of-Bits Evaluation Packet v3.3 — Out-of-Band Trust Anchors

Issued 2026-09-17 by Jou Labs. Compare these values against what the evidence
package reports. A package that verifies WITHOUT these pins is only
SELF-CONSISTENT; verified AGAINST these pins, it is authenticated to the Jou
Labs producer.

Packet: `EARL-ToB-evaluation-packet-v3.3-2026-09-17.zip`
(v3.3 corrects v3.1: the probe replay recipe preserves the banked seed
ORDER (campaign3/seeds.json 'pairs'); release_check.sh really verifies the
90-entry ledger manifest and 26-entry corpus manifest; and replay --run fails on
zero comparisons. (Carries forward v3.2's fresh signature, shared-corpus probe,
runnable quickstart, real sealed-binary filenames, and the release gate.))

```
SIGNED ZIP SHA-256
  b466a31f9a4cc2559e4964f2ae0b48862248edb3ece910e034d68494cb6cc668
ROOT MANIFEST SHA-256 (MANIFEST.sha256; covers all 226 packet content files)
  e6865992716be81d840dc9fdff1f61c1608a881acfca853451b5d3d0435e8f5c
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
  df792fa06009d5691e127c6c6d30c04eecf30d3f
CORRECTED-HARNESS BUILD-CLOSURE COMMIT (the corrected source archive; builds with --locked)
  10d5b06f0c120588f52870bbecbfb26756d59a3c
```

## Verify the packet on your own machine (no Jou Labs service)

```
# scripted gate over every runnable check:
bash 10-corrections/release_check.sh "$(pwd)"

# or the signature alone, with the packet's own stdlib verifier + this key:
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

## Scope note (read before drawing conclusions)

Delivered as an **experimental baseline plus prospective fixes and open findings
for independent review** — not "fully corrected" or "validation complete." The
corrected harness is build-verified, but a native clean-machine replay diff
against the historical ledgers is NOT RUN in this packet, and the containment /
assurance items (host and signing-key privileges, full gate-conjunct
reconstruction, preregistration chronology, cryptographic review, beacon
authentication) remain OPEN. All statuses are in the packet's `RELEASE-MATRIX.md`.
