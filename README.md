# renovate-config

Shared renovate configuration and GitHub Action

## Custom Managers

### pre-commit entry images

The pre-commit manager bumps `rev` only. This manager bumps a Docker image
that a hook pins in its `entry:` override. The image needs both a tag and a
digest:

```yaml
-   id: hadolint-docker
    entry: ghcr.io/hadolint/hadolint:v2.15.1@sha256:32dac94127fd60b7b7e3fbfc65e1383b9b5e25c9bfd7b8536de7a539fe68a12d hadolint
```

### balena-cloud

This is a customer manager & datasource to query the balenaCloud application releases
and manage bh.cr registry tags.

Any of the following matches:

```Dockerfile
# Note that tags following the : are ignored by the bh.cr registry, so we use them for semver tracking
FROM bh.cr/gh_klutchell/tailscale-amd64/94bc62ba78e5d0611338535ce72054df:1.82.5-rev1
FROM bh.cr/gh_klutchell/tailscale-amd64/94bc62ba78e5d0611338535ce72054df
FROM bh.cr/gh_klutchell/tailscale-amd64:1.82.5-rev1
FROM bh.cr/gh_klutchell/tailscale-amd64/1.82.5-rev1
```

Will be replaced with:

```Dockerfile
# Note that tags following the : are ignored by the bh.cr registry, so we use them for semver tracking
FROM bh.cr/gh_klutchell/tailscale-amd64/94bc62ba78e5d0611338535ce72054df:1.82.5-rev1
```

Multiarch example usage:

```Dockerfile
# Dockerfile.template
ARG BALENA_ARCH=%%BALENA_ARCH%%

FROM bh.cr/gh_klutchell/tailscale-amd64:1.82.5-rev1 AS balena-tailscale-amd64
FROM bh.cr/gh_klutchell/tailscale-aarch64:1.82.5-rev1 AS balena-tailscale-aarch64
FROM bh.cr/gh_klutchell/tailscale-armv7hf:1.82.5-rev1 AS balena-tailscale-armv7hf

# hadolint ignore=DL3006
FROM balena-tailscale-${BALENA_ARCH}
```
