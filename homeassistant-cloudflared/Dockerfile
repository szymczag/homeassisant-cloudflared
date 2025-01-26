# ---- We'll define a build arg pointing to the official or community base image
ARG BUILD_FROM=ghcr.io/hassio-addons/base:14.0.0
# hadolint ignore=DL3006
FROM ${BUILD_FROM}

# ---- Define build-time args (defaults can be overridden) ----
ARG CF_VERSION=2025.1.0
ARG CF_ARCH
ARG CF_URL=https://github.com/cloudflare/cloudflared/releases/download

# ---- Install cloudflared
RUN curl -L ${CF_URL}/${CF_VERSION}/cloudflared-linux-${CF_ARCH} \
    -o /usr/local/bin/cloudflared \
  && chmod +x /usr/local/bin/cloudflared

# ---- Copy the S6 service definition (including 'run' script) into place
COPY rootfs/ /
