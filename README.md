# excalidraw-stack

Portainer Git stack for [Excalidraw](https://github.com/excalidraw/excalidraw) on `apple-pi.lan`.

- **Stack name in Portainer:** `excalidraw`
- **URL:** https://excalidraw.svc.lan (LAN only, no published port)
- **TLS:** Caddy's internal CA — the same root already trusted for `invoiceninja.svc.lan`.
  Other clients need it installed; see the `bheus/caddy-stack` README.

## Where drawings live

Nowhere on the server. The image is a static nginx site; scenes are kept in the
browser's localStorage. Use **Save to disk** (`.excalidraw`) for anything worth keeping.

**Share link** and **Live collaboration** are wired at build time to excalidraw.com's
backend, so they upload the (end-to-end encrypted) scene to excalidraw.com. Self-hosting
those needs `excalidraw-room` plus a custom image build.
