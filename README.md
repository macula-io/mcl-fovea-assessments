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
| [`macula-station-kx/`](macula-station-kx/fovea.yaml) | A macula station accepts only post-quantum key exchange (`kx_post_quantum_only`, probe `kx_group` v1), on station-nl-ams | One cell, by design: not grid-complete, so `fovea lint` reports the other 79 cells as missing, and nothing else |

## Checking an assessment

```bash
fovea lint macula-station-kx
```

(`fovea` is macula-fovea's CLI.) The only findings for `macula-station-kx`
are `grid_missing_cell`, for the cells it deliberately leaves out.
