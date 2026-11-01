# The harness image: Stalwart, plus Stalwart's own CLI, which the entrypoint configures it with.
#
# Both pinned by multi-arch index digest (linux/amd64 + linux/arm64). Bump deliberately: re-resolve
# the digest (`docker buildx imagetools inspect <image>:<tag>`, the top-level `Digest:`) and update
# the version in the tag. The CLI is a static binary, so it runs in the server's image as copied.
FROM ghcr.io/stalwartlabs/cli:1.0.12@sha256:fe199affac1d120a8c200ef39ae629765a2976270e0453575c1caf906ee15b52 AS cli

FROM stalwartlabs/stalwart:v0.16.27@sha256:c429b311c93db5606598abc1c68225a85a6953183848ae01bee9c6a98885836c

COPY --from=cli /usr/local/bin/stalwart-cli /usr/local/bin/stalwart-cli

# Where the sign-in server mints its HTTPS front's certificate (entrypoint.sh). It exists in the
# image, owned by the server's user, because a named volume first mounted here copies that
# ownership; one mounted at a path the image lacks is root's, and the server cannot write to it.
USER root
RUN mkdir -p /harness/front-tls && chown stalwart:stalwart /harness/front-tls
USER stalwart
