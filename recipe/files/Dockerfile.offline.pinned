# GENERATED L-009 hermetic build (--network=none, SBOM+SLSA-attestation-producing).
# Pinned bases from BASE_IMAGE_DIGESTS.lock; vendored Go tree + debs from Dockerfile.vendor.
FROM golang:1.27.4-bookworm@sha256:4fa7c968ba93f1635670522dbb01a2c78150443e1385746924566079f36570ea AS build_cloudflared
WORKDIR /src
COPY vendored/cloudflared-src/ .
ENV CGO_ENABLED=0 GOTOOLCHAIN=local GOWORK=off GOFLAGS="-mod=vendor -buildvcs=false" GOPROXY=off SOURCE_DATE_EPOCH=1795440149
RUN go build -trimpath -ldflags='-X main.Version=2026.11.2 -X main.BuildTime=2026-10-26T09:34:58Z -X github.com/cloudflare/cloudflared/metrics.Runtime=virtual' -o /cloudflared ./cmd/cloudflared && /cloudflared --version > /tmp/cloudflared.version 2>&1; cat /tmp/cloudflared.version; grep -qF 'cloudflared version 2026.11.2 (built 2026-10-26T09:34:58Z)' /tmp/cloudflared.version || { echo 'cloudflared --version does not report cloudflared version 2026.11.2 (built 2026-10-26T09:34:58Z)'; exit 1; } && \
    { readelf -lW /cloudflared | sed -n 's/.*Requesting program interpreter: \(.*\)]$/\1/p'; readelf -dW /cloudflared | sed -n 's/.*Shared library: \[\(.*\)\]$/\1/p'; } > /cloudflared.needs
# rs-probe: the HEALTHCHECK helper (standard library only, from the kit's rs-probe.go)
FROM golang:1.27.4-bookworm@sha256:4fa7c968ba93f1635670522dbb01a2c78150443e1385746924566079f36570ea AS build_rs_probe
WORKDIR /probe
COPY rs-probe.go main.go
COPY rs-probe.mod go.mod
ENV CGO_ENABLED=0 GOTOOLCHAIN=local GOFLAGS="-buildvcs=false" GOPROXY=off SOURCE_DATE_EPOCH=1795440149
RUN go build -trimpath -ldflags='-s -w' -o /rs-probe .
# the chiselled rootfs (vendored/chisel, cut by Dockerfile.vendor), the service user, the binaries
FROM ubuntu:24.04@sha256:9c7f200c07e3d4a03e7249d2c16bf66a51120fb876aa2242cf31451412543289 AS rootfs
ARG SOURCE_DATE_EPOCH=1795440149
ADD vendored/chisel/rootfs.tar /rootfs/
RUN printf 'cloudflared:x:65532:65532::/home/cloudflared:/bin/sh\n' >> /rootfs/etc/passwd && printf 'cloudflared:x:65532:\n' >> /rootfs/etc/group && \
    mkdir -p /rootfs/home/cloudflared /rootfs/etc/cloudflared && chown -R 65532:65532 /rootfs/home/cloudflared /rootfs/etc/cloudflared
COPY --from=build_cloudflared /cloudflared /rootfs/usr/local/bin/cloudflared
COPY --from=build_cloudflared /cloudflared.needs /tmp/needs/cloudflared
COPY --from=build_rs_probe /rs-probe /rootfs/usr/local/bin/rs-probe
RUN chmod 755 /rootfs/usr/local/bin/cloudflared /rootfs/usr/local/bin/rs-probe
RUN --mount=type=bind,source=vendored/chisel,target=/chisel set -eu; \
    for n in /tmp/needs/*; do while read -r lib; do [ -n "$lib" ] || continue; \
      case "$lib" in /*) [ -e "/rootfs$lib" ] ;; *) [ -n "$(find /rootfs/usr/lib /rootfs/usr/lib64 -name "$lib" 2>/dev/null | head -1)" ] ;; esac || \
      { echo "$(basename "$n") needs $lib, which the chiselled rootfs does not have: add its slice to runtime_base.slices" >&2; exit 1; }; \
    done < "$n"; done; rm -rf /tmp/needs; \
    [ -e /rootfs/etc/os-release ] || { echo "no /etc/os-release: base-files_release-info is missing" >&2; exit 1; }; \
    [ "$(grep -c '^Package: ' /rootfs/var/lib/dpkg/status)" = "$(wc -l < /chisel/packages.tsv)" ] || \
      { echo "/var/lib/dpkg/status does not list every package of vendored/chisel/packages.tsv" >&2; exit 1; }; \
    rm -rf /rootfs/var/log/dpkg.log /rootfs/var/log/alternatives.log /rootfs/var/log/apt /rootfs/var/cache/ldconfig/aux-cache /rootfs/var/cache/fontconfig
FROM scratch
ARG SOURCE_DATE_EPOCH=1795440149
COPY --from=rootfs /rootfs/ /
ENV PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin TUNNEL_METRICS=0.0.0.0:20241
LABEL org.opencontainers.image.licenses="Apache-2.0"
LABEL org.opencontainers.image.title="cloudflared"
LABEL org.opencontainers.image.description="cloudflared is the Cloudflare Tunnel connector: it makes your web services, SSH and other TCP services reachable through Cloudflare without opening any inbound port. Run it with your tunnel token; it connects out to Cloudflare and forwards requests to your local services."
LABEL org.opencontainers.image.source="https://github.com/cloudflare/cloudflared"
LABEL org.opencontainers.image.version="2026.11.2-CE-ubuntu24.04-r1"
LABEL org.opencontainers.image.revision="dc57315806d5faa8715f728fd37b5302c81c467b"
HEALTHCHECK --interval=5s --timeout=10s --start-period=30s \
  CMD /usr/local/bin/rs-probe http://127.0.0.1:20241/ready || exit 1
WORKDIR /home/cloudflared
USER cloudflared
EXPOSE 20241
ENTRYPOINT ["cloudflared", "--no-autoupdate"]
CMD ["tunnel", "run"]
