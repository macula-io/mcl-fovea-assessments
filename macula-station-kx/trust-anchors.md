# Trust-anchor register — macula-station-kx

*This exists so every `by_design` claim in the matrix rests on a named
root-of-trust, and a reader can ask of each: if this breaks, is the rest of
the system revocable in one bounded change?*

The rule (macula-fovea spec v0.4, 14-instantiation): only things whose
compromise would require **rebuilding the system**, not a bounded change,
are anchors. Anchors that are partly strong (keys that can be re-initialized
after a compromise) still count; their tamper-readiness is a property of the
anchor's *ceremony*, not of the anchor as an object.

## Anchors

1. **The io.macula realm key and its ceremony.**
   The realm's public key is the trust anchor every org-namespaced
   advertisement, every realm member endorsement and every fovea observation
   verifies against. Its compromise is not a rotation: every verifier that
   pinned the old key, every endorsement and advertisement it signed, and the
   attribution of every mesh service flows through it. The ceremony that
   generates and guards the realm's signing key is part of this anchor.
   *Guard:* the ceremony (generation, custody, backup, destruction) is out of
   this assessment's scope — it belongs to the macula-mesh-realm assessment —
   but this assessment depends on it and names it here.

2. **The foundation key(s) and the realm trust list.**
   A station's admission of realm-signed records flows through the foundation
   trust list (record type `0x0F`), signed by a foundation key. A station
   holds the foundation key ids it is configured with; with no current trust
   list it can check no realm-signed slot. Compromise of a foundation key
   re-roots every station's trust at once.
   *Guard:* the foundation keys' ceremony and custody, like the realm's, is
   named here and assessed elsewhere.

3. **The build-and-release pipeline identity.**
   The macula-ci-images attestation identity (the OIDC identity and workflow
   that sign every image's cosign signature, SPDX SBOM and SLSA provenance)
   and the release tag signers. A compromised pipeline ships a backdoored
   station to every box that trusts its attestations; there is no bounded
   recovery, because the fleet's whole verification path ran through it.
   *Guard:* pinning and signer allowlists on the boxes (assessed in the
   `create` and `deliver` columns); the pipeline's own compromise is the
   residual risk named here.

4. **The deployment path: macula-fleet and its signer.**
   Whoever can sign a fleet release and change `macula-fleet`'s config can
   change what every station runs and whom it trusts, in one push. The
   repository, its allowed signers and the reconciler the boxes run are one
   trust anchor: if it breaks, every box follows it.
   *Guard:* signed tags, on-box `allowed_signers`, no-rollback reconciliation
   (assessed in `deliver`); a hostile signer is the residual risk named here.

## Deliberately not anchors

- **Each station's node identity key.** Its compromise is serious, but
  bounded: one key, one station, recoverable by rotating that key and
  re-pointing the clients that pin its node id. It is treated in the
  `admin`, `operate` and `decommission` columns, not here.
- **The observer's identity (mcl-fovea) and the keeper.** They witness the
  stations and archive their records; their compromise corrupts *evidence*,
  not the stations. Named in the matrix (`machine_agent`, `temporal`), not
  here.
- **Vendor CAs, firmware signing keys, datacentre physical security.**
  Out of scope (see `fovea.yaml` `scope.out`); their owners' ceremonies are
  their own. A compromised vendor CA that could re-sign a station image
  would, however, be indistinguishable from a pipeline compromise — named
  here so the dependency is explicit.
