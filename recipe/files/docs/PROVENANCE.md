# Provenance — cloudflared (`2026.11.2-CE-ubuntu24.04-r1`)

Every item below is copied from a record in this archive; the record is named in brackets.

## Source

- Upstream repository: https://github.com/cloudflare/cloudflared
- Revision built: `2026.11.2` (commit `dc57315806d5faa8715f728fd37b5302c81c467b`)
- Source tree: `0ca381f59364278563e355dce12623e00dd1d2f0f08e3232ee04b762c0fcdaf4` over 488 files [`docs/provenance/SOURCE_TREE_HASH.json`]
- Community Edition only: enterprise code is removed or refused before the build (CE audit at acquire).

## Build

- Method: `source-rebuild-go`; base image `scratch + a chiselled Ubuntu 24.04 runtime (vendored/chisel/slices.txt, packages.tsv)`
- Compiled with `--network=none` from vendored source, dependencies and base images; BuildKit pinned to `moby/buildkit:v0.31.2` (shipped in `toolchain/`).
- SOURCE_DATE_EPOCH: `1795440149` [`SOURCE_DATE_EPOCH.txt`]
- Frozen inputs: [`docs/provenance/FREEZE_MANIFEST.json`], build recipe: [`docs/provenance/BUILD_RECIPE.json`].

## Output

- Published image: `readystack/cloudflared:2026.11.2-CE-ubuntu24.04-r1` — index digest `sha256:c2eee3c8da0ba64f20bd5a1aea1bb7c6642414f9b91dbdea4f5e5d4837c3a82b` [`docs/provenance/LOCKED_DIGEST.txt`]
- linux/amd64 image digest `sha256:26e585bb55a1aa5108088bdf9c0e6af5ea6cf5c6f67cb19a253aa3d510c4d253` — reproduced from this archive before publishing [`docs/provenance/REPRO_VERIFICATION.json`].

## Attestations and signature

- SBOM + SLSA provenance attached to the image: attached: an SPDX SBOM and SLSA v1 provenance [`docs/provenance/SCOUT_VERDICT.json`]
- cosign signature by the ReadyStack release key: verified before publishing. Check it: `cosign verify --key https://readystack.dev/keys/cosign.pub readystack/cloudflared:2026.11.2-CE-ubuntu24.04-r1`

## Pinned inputs, and why

- 26 pinned inputs, each with where it came from, why this one, how it was checked and when it was decided: `docs/PINS.md` [`docs/provenance/PINS.json`] (upstream source 2, base and build images 3, image digests 2, dependency versions readystack sets 4, pins in the kit's dockerfiles 8, readystack's build engine 5, dependencies upstream chose 2).

## Build trail

- How this release passed through the ReadyStack harness, record by record (every chain, drive, stage, review round, defect and check, each naming the record it is copied from): `ReadyStack-cloudflared-2026.11.2-CE-ubuntu24.04-r1.trail.json`, published beside this archive and signed with the release key (`ReadyStack-cloudflared-2026.11.2-CE-ubuntu24.04-r1.trail.json.sigstore`), and readable at https://readystack.dev/products/cloudflared/trail/2026.11.2-CE-ubuntu24.04-r1.

