# System landscape — macula-station-kx

*One diagram. The test is bidirectional: every column of the matrix must
point to something on it, and everything on it must be under at least one
column (macula-fovea spec v0.4, 14-instantiation).*

```
                              build & release (github, CI)
  macula-io/macula ── macula-pqc ── macula-ci-images (sign+SBOM+provenance)
        │                                    │ ghcr.io (images by digest)
        └────────────────────────────────────┤
                                             ▼
                              ┌────────── macula-fleet (signed tags) ──────┐
                              │                                            │
                              ▼                                            ▼
                      reconciler on each box                      operator (admin)
                              │                                            │
   observer (mcl-fovea) ──────┼────── probes station-nl-ams :4433 ──►      │
   on its own box             │                                            │
        │                     ▼                                            │
        │        ┌───────────────────────────────────┐                     │
        │        │ station box (VPS, debian)         │                     │
        │        │  ├─ station OTP release           │                     │
        │        │  │   ├─ QUIC listener :4433       │◄── clients / SDKs   │
        │        │  │   ├─ DHT record store          │◄── peers (stations) │
        │        │  │   ├─ pubsub relay (Plumtree)   │                     │
        │        │  │   └─ RPC relay / mesh pool     │                     │
        │        │  ├─ node identity key (volume)    │                     │
        │        │  └─ config (fleet-managed)        │                     │
        │        └───────────────────────────────────┘                     │
        ▼              ▲                                                   │
   signed verdicts ────┘ (DHT: latest record per slot)                     │
        │                                                                  │
        ▼                                                                  │
   keeper (lab box, 15 min) ──► mcl-fovea-records (public archive)         │
                                                          ▲                │
   realm (io.macula): key, endorsements, tombstones, trust list ──────────┘
```

## Elements and the columns that cover them

| Element | Covered by columns |
|---|---|
| Station OTP release (QUIC listener, DHT store, pubsub, RPC relay, pool) | `operate`, `in_motion`, `in_use`, `machine_agent` |
| Node identity key (volume on the box) | `admin`, `at_rest`, `in_use`, `decommission`, `temporal` |
| Config and fleet-managed files | `deliver`, `admin`, `operate` |
| Build: macula-io/macula, macula-pqc, CI | `create`, `acquire` |
| Release: macula-ci-images attestations, ghcr images | `create`, `deliver`, `trusted_partner` |
| Deployment: macula-fleet signed tags, reconciler, boxes | `deliver`, `admin`, `machine_agent` |
| Clients and SDKs dialing the stations | `external`, `in_motion` |
| Other stations and the mesh pool | `operate`, `in_motion` |
| The realm (key, endorsements, tombstones, trust list) | `admin`, `temporal`, `trusted_partner` |
| The observer (mcl-fovea) and its signed verdicts | `machine_agent`, `temporal` |
| The keeper and mcl-fovea-records | `temporal`, `socio_legal` |
| The operator and maintainers | `internal`, `admin` |
| The VPS providers and datacentres | `physical_natural`, `trusted_partner`, `socio_legal` |
| Law, courts, jurisdictions | `socio_legal` |
| Time: expiry, key aging, scheme deprecation | `temporal` |

Elements drawn dashed in the diagram (out of scope, with reason): the
realm's own governance process and CA ceremony (the macula-mesh-realm
assessment); the mcl-* services' internals (each assesses itself); vendor
firmware and datacentre physical security (their owners'); clients' own keys
and boxes.
