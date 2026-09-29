# excalidraw-stack

Portainer Git stack for [Excalidraw](https://github.com/excalidraw/excalidraw) on `apple-pi.lan`.

- **Stack name in Portainer:** `excalidraw`
- **URL:** https://excalidraw.svc.lan (LAN only, no published port)
- **TLS:** Caddy's internal CA — the same root already trusted for `invoiceninja.svc.lan`.
  Other clients need it installed; see the `bheus/caddy-stack` README.

## Image and updates

Upstream's Docker Hub image stopped publishing on 2026-05-06, so this repo builds its own.
`excalidraw/` is a submodule tracking upstream's `release` branch; CI builds it with
upstream's Dockerfile on a native arm64 runner, pushes `ghcr.io/bheus/excalidraw:latest`
(plus a tag per upstream commit), then POSTs the Portainer webhook
(`PORTAINER_EXCALIDRAW_WEBHOOK` secret, public `deploy.builtbybrendan.com` URL).

Updating is merging Dependabot's weekly submodule PR, or by hand:

```bash
git submodule update --remote excalidraw && git commit -am "Bump excalidraw" && git push
```

The stack is **webhook-only** (no polling) so Portainer never redeploys before the new
image exists. Rollback: redeploy with `image: ghcr.io/bheus/excalidraw:<older upstream sha>`.

## Where drawings live

Nowhere on the server. The image is a static nginx site; scenes are kept in the
browser's localStorage. Use **Save to disk** (`.excalidraw`) for anything worth keeping.

**Share link** and **Live collaboration** are wired at build time to excalidraw.com's
backend, so they upload the (end-to-end encrypted) scene to excalidraw.com. Self-hosting
those needs `excalidraw-room` plus a custom image build.
