# mcl-fovea-assessments

The Fovea Assessments Forge for the io.macula Mesh Realm: security
assessments written in the [macula-fovea](https://github.com/macula-io/macula-fovea)
format, whose probe declarations
[mcl-fovea](https://github.com/macula-services/mcl-fovea) observes on the
running mesh. Each observation is a signed record that anyone can verify
offline against the realm key and the revision of the assessment it names
(macula-fovea spec v0.4, `15-observations`), which is why this repository is
public.

Releases are tags signed by the maintainer. An observer adopts a signed
revision, never a branch.

## Assessments

| Directory | Claim | State |
|---|---|---|
| [`macula-station-kx/`](macula-station-kx/fovea.yaml) | A macula station accepts only post-quantum key exchange (`kx_post_quantum_only`, probe `kx_group` v1), on all six io.macula fleet stations | Grid-complete: all 80 cells answered (`fovea lint` clean). The one probe-declared claim is the one mcl-fovea observes. |

`macula-station-kx` is grid-complete under spec v0.4: every one of the 80
cells is answered — `assessed` only where executable evidence exists (the kx
claim's probe and test), otherwise `assumed` (drafted from the repositories
and design documents they cite), on `roadmap` with a `review_by` date, or
`na` with a written reason. The header's `policy` and `targets` are
unchanged, and the observer keeps observing the claim from the signed
revision it ships (`macula-station-kx-v1`); a new signed tag is what moves
the observer onto this grid-complete revision.

## Checking an assessment

```bash
fovea lint macula-station-kx
```

(`fovea` is macula-fovea's CLI.) `fovea lint` reports no findings for
`macula-station-kx`.
