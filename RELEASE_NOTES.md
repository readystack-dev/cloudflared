# cloudflared 2026.11.2 — ReadyStack release `2026.11.2-CE-ubuntu24.04-r1`

Generated 2026-11-24 from this release's build records and strategy. Statements that come from a single record name it in brackets; the others come from this release's strategy and build log.

## Release

- **Image:** `readystack/cloudflared:2026.11.2-CE-ubuntu24.04-r1` (linux/amd64). Index digest `sha256:c2eee3c8da0ba64f20bd5a1aea1bb7c6642414f9b91dbdea4f5e5d4837c3a82b` [`docs/provenance/LOCKED_DIGEST.txt`].
- **Image manifest digest:** `sha256:26e585bb55a1aa5108088bdf9c0e6af5ea6cf5c6f67cb19a253aa3d510c4d253` [`TARGET_DIGEST.txt`].
- **Upstream:** cloudflared 2026.11.2 built from source at `2026.11.2` of https://github.com/cloudflare/cloudflared — upstream notes: https://github.com/cloudflare/cloudflared/releases/tag/2026.11.2 [`docs/provenance/BUILD_RECIPE.json`].
- **Base image:** none (`FROM scratch`): a chiselled Ubuntu 24.04 runtime, the files Canonical's chisel cut from Ubuntu's own packages (`vendored/chisel/slices.txt`, `vendored/chisel/packages.tsv`); runtime user `cloudflared`.
- **Build:** `source-rebuild-go`, compiled offline with `--network=none`.
- **Supersedes:** ReadyStack release `2026.11.1-CE-ubuntu24.04-r2`.

## What changed in this release

- Upstream version 2026.11.1 → 2026.11.2.
- Minimum dependency versions ReadyStack enforces on top of upstream (raised only where upstream's own version was lower); shipped versions from the SBOM [`docs/sbom/SBOM.json`]:
  - `github.com/klauspost/compress` at least `v1.18.7`: not raised, upstream already pins `v1.20.3` (shipped: `v1.20.3`)
  - `github.com/go-jose/go-jose/v4` at least `v4.1.6`: not raised, upstream already pins `v4.1.6` (shipped: `v4.1.6`)
  - `golang.org/x/net` at least `v0.62.0`: not raised, upstream already pins `v0.62.0` (shipped: `v0.62.0`)
- This release moves cloudflared from 2026.11.1 to 2026.11.2, a newer release of upstream's 2026.11.x line. Upstream's release notes list what changed: https://github.com/cloudflare/cloudflared/releases/tag/2026.11.2

## Security status at release (measured 2026-11-23)

- **Docker Scout:** 0 fixable critical/high vulnerabilities; runs as `cloudflared` (non-root); SBOM + provenance attestations present (SPDX SBOM + SLSA v1) [`docs/provenance/SCOUT_VERDICT.json`].
- **OSV cross-check:** no fixable critical/high vulnerability beyond Docker Scout's findings.
- **Signature:** cosign signature verified by ReadyStack's release gate before publishing; check it yourself with `cosign verify --key https://readystack.dev/keys/cosign.pub readystack/cloudflared:2026.11.2-CE-ubuntu24.04-r1` (signatures are recorded in the public Sigstore transparency log).
- **ReadyStack release check at release:** passed (ReadyStack's own test, not a Docker Scout grade). Vulnerabilities are disclosed every day and the image never changes, so it is re-checked daily, beside Docker Scout's own dated grade, at https://readystack.dev/queue.
- **Before exposing this service:** The metrics port 20241 has no authentication and also serves /debug/pprof (Go profiling) and /config (the tunnel's ingress rules). Publish it only to a private network, or not at all.
- **Before exposing this service:** The tunnel token is a credential: anyone with it can run a connector for your tunnel. Keep it in an env file only you can read (mode 600) and never commit that file.

## Reproducibility

Rebuilds offline from this archive to the identical image digest `sha256:26e585bb55a1aa5108088bdf9c0e6af5ea6cf5c6f67cb19a253aa3d510c4d253`: run `./BUILD_OFFLINE.sh`, then `./VERIFY_OFFLINE.sh` (add `--online` to confirm the Docker Hub tag holds the same digest). Before publishing, ReadyStack rebuilt the kit with `./BUILD_OFFLINE.sh` and compared the result with the published digest [`docs/provenance/REPRO_VERIFICATION.json`], then ran `./BUILD_OFFLINE.sh` and `./VERIFY_OFFLINE.sh` from this archive (without `--online`).

## Validation

- 5 of 5 use cases passed (5 authored) against a container of this image started with `tunnel --hello-world` after the image name instead of the image's default command (`tunnel run`) [`docs/validation/test-results.json`]. A container started the default way can answer differently.

## Known issues

- **medium** (upstream): Unauthenticated metrics port 20241 by default [`RSD-cloudflared-d63cf083cb`]

## Defects fixed

- 13 problems found before release and fixed before it shipped (13 by review); each is listed in `docs/defects/defects.json`.

## Verify this archive

```
sha256sum -c CHECKSUMS.sha256
./BUILD_OFFLINE.sh && ./VERIFY_OFFLINE.sh --online
cosign verify --key https://readystack.dev/keys/cosign.pub readystack/cloudflared:2026.11.2-CE-ubuntu24.04-r1
```

ReadyStack records every signature in the public Sigstore transparency log (Rekor), so the command needs no extra flags.

