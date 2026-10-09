# Peeranha nest

An installable Nuthatch nest for **Peeranha** on Polygon - the community-driven Q&A protocol: users,
communities, tags, posts, replies and reputation.

```sh
nuthatch init --from https://github.com/nuthatch-org/peeranha-nest
nuthatch dev --dir peeranha-nest --rpc <a polygon archive endpoint> --window 640 --seal-direct
```

## Why this one

**The Graph's gateway cannot serve Peeranha's subgraph.** Asked directly, it answers:

```
{"errors":[{"message":"subgraph not found: no allocations"}]}
```

Deployment `QmW4Vo3ZYV79pizzYKbNZ2TTKfHQhdRQDrmGDG25UkBpuz` carries **10,673 GRT signalled** and **zero
active indexer allocations**. Somebody paid to have this data produced; nobody is producing it.

It was not chosen. It was found by indexing allocations and curation signal and asking which
deployments have the second without the first - see `graph-allocations-nest`.

## Provenance

Ported by `nuthatch init --from-subgraph QmW4Vo3ZYV79pizzYKbNZ2TTKfHQhdRQDrmGDG25UkBpuz`, using the
manifest's own pinned ABIs. Five contracts, 33 tables, no factories or templates - a plain
fixed-contract nest, which is why the import needed no manual resolution at all.

Backfilled from block 29,595,889 to tip: **62.7 million Polygon blocks**.

## Endpoints

Polygon needs an **archive** endpoint and the config deliberately names none, because an archive
endpoint usually carries an API key and this file is pinned into the nest's content address. Pass one
with `--rpc`.

Measured 2026-08-19: the keyless `polygon-bor-rpc.publicnode.com` is **not** archive, and
`polygon.drpc.org` is archive but caps `getLogs` at **80 blocks** - 780,000 requests for this range,
which is free but impractical. A commercial archive endpoint measured a 1,280-block ceiling, so
`--window 640` is the figure to use.

## Honest scope

Event data only, matching the manifest. The subgraph's derived entities - reputation totals, post
counts, community rollups - are views over this data rather than part of it. Those that are pure
functions of the events are reproducible exactly; see nuthatch RFC-0038 §6a for the ones that are not.
