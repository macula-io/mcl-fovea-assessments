# mcl-fovea-assessments

The Fovea Assessments Forge for the io.macula Mesh Realm: security
assessments written in the [macula-fovea](https://github.com/macula-io/macula-fovea)
format, whose probe declarations
[mcl-fovea](https://github.com/macula-services/mcl-fovea) observes on the
running mesh. Each observation is a signed record that anyone can verify
offline against the realm key and the revision of the assessment it names
(macula-fovea spec v0.6, `15-observations`), which is why this repository is
public.

Releases are tags signed by the maintainer. An observer adopts a signed
revision, never a branch.

## Assessments

| Directory | Claim | State |
|---|---|---|
| [`macula-station-kx/`](macula-station-kx/fovea.yaml) | On all six io.macula fleet stations: a station accepts only post-quantum key exchange (`kx_post_quantum_only`, probe `kx_group` v1), and a station runs a signed public release (`station_runs_signed_release`, probe `station_release` v1, from spec v0.6) | Grid-complete: all 80 cells answered (`fovea lint` clean). mcl-fovea observes both probe-declared claims. |

`macula-station-kx` is grid-complete under spec v0.6: every one of the 80
cells is answered — `assessed` only where executable evidence exists (the kx
claim's probe and test, the release claim's probe), otherwise `assumed`
(drafted from the repositories and design documents they cite), on `roadmap`
with a `review_by` date, or `na` with a written reason. The observer observes
the claims of the signed revision it vendors; a new signed tag
(`macula-station-kx-vN`) is what moves it onto a new revision.

## Checking an assessment

```bash
fovea lint macula-station-kx
```

(`fovea` is macula-fovea's CLI.) `fovea lint` reports no findings for
`macula-station-kx`.
