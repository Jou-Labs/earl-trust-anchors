# EARL Evaluation — Out-of-Band Trust Anchors

Issued 2026-09-17 17:32 UTC by Jou Labs. Compare these values against what the
evidence package and the evaluation console report. A package that
verifies WITHOUT these pins is only SELF-CONSISTENT; verified
AGAINST these pins, it is authenticated to the Jou Labs producer.

```
AUTHOR KEY FINGERPRINT (evidence packaging)
  aa9458b67e9b21d4172e692f4f9b21b5a0079cc2feec29b19056b18cd1dd9be6
AUTHORITY KEY FINGERPRINT (sealed ruleset)
  b1da3a27a782053ddecfcbd94bb68bd33ba2d779eedbd218f4fee89096306cec
VERIFIER SHA-256 (verify_package.py)
  abdca9fe3746271a537b1b18fda7c80a92d0269b26539115b8fc0c8d77192895
RUNTIME SHA-256 (earl_containment_runtime.py)
  3952b2e782af5ffbc46e3f87b88f89e9a418ae5d67c18085498763f8fadae5db
RULESET SHA-256 (containment_contract_v2.ruleset.json)
  1de629c0b05992eb8a67f5e2c0f96d0d7dbd9beca641753fc8aa9fed4b4bc9e0
AUDIT SIGNER FINGERPRINT (fairness ledger)
  7a4149de5e5a72c45798b77c3f5aafcdddc0eb1741ee0e0b544406d12f1c3b0c
AUDIT VERIFIER SHA-256 (verify_fairness_ledger.py)
  90f11164e165c90338591b30ed750e0002a925a57b62ca497b055a97584afee1
AUDIT PIPELINE BINARY SHA-256 (this deployment)
  6e60ea5c547cad71aff0e6c9b88e3434de70b7a4261e7093065aadab57546893
```

Verify a package on your own machine (no Jou Labs service):

```
python verify_package.py <package.zip> \
    --pin-author aa9458b67e9b21d4172e692f4f9b21b5a0079cc2feec29b19056b18cd1dd9be6 \
    --pin-authority b1da3a27a782053ddecfcbd94bb68bd33ba2d779eedbd218f4fee89096306cec
```

Check the verifier itself before running it:

```
sha256sum verify_package.py   # must equal the pinned value
```
