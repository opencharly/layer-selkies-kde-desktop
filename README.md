# selkies-kde-desktop

The KDE Plasma flavor of the Selkies streaming desktop for OpenCharly images —
a headless, WebRTC-streamed Plasma pod.

`selkies-kde-desktop` composes the shared
[`selkies-core`](https://github.com/opencharly/pod-selkies-core) (pixelflux WebRTC
transport + Chrome/CDP + `wl-*` tooling + fonts + a11y + terminal-recording +
sshd) with the
[`kde-selkies`](https://github.com/opencharly/pod-kde-selkies) flavor primitive
(`startplasma-wayland` nested in pixelflux's `wayland-1`, de-SDDM, started by a
supervisord poll-for-`wayland-1` service). The SDDM-free Plasma session packages
(`kde-shell`) are pulled transitively via `kde-selkies`'s `require`.

This is the KDE sibling of `selkies-desktop`; the two flavors differ ONLY in the
nested compositor, sharing `selkies-core` unchanged (R3). KDE ships its own audio
applet, panel, and notifier, so there is no waybar / swaync / pavucontrol here.

## What it provides

| Property | Value |
|---|---|
| Layer / candy | `selkies-kde-desktop` (metalayer) |
| Composes | `selkies-core`, `kde-selkies` (pulls `kde-shell`) |
| Key artifacts | `~/.local/bin/kde-selkies-session`, `kwin_wayland`, `plasmashell`, `startplasma-wayland` |
| Port | HTTPS stream on `3000` |
| Install files | none — its observable behaviour IS the union of its children |

## How to use it

Compose the layer by pinning this repo in a box's `candy:` list:

```yaml
my-kde-pod:
  candy:
    base: cachyos
    candy:
      - '@github.com/opencharly/layer-selkies-kde-desktop:v2026.247.1643'
```

The streamed Plasma desktop answers over HTTPS on port 3000; the encoder is
auto-selected at runtime (VAAPI on an AMD/Intel render node, x264 otherwise,
NVENC only from the `*-nvidia` box).

## Layout

- `charly.yml` — the `selkies-kde-desktop:` candy entity (the `candy:`
  composition and the `check:` probes) and the embedded
  `selkies-kde-desktop-skill:` skill entity.
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — this user overview.

## Related

- Owning skill: `/charly-selkies:selkies-kde-desktop`
- Shared core: `/charly-selkies:selkies-core`
- Labwc sibling: `/charly-selkies:selkies-desktop-layer`
- Beds: `check-selkies-kde-pod`, `check-selkies-kde-nvidia-vm`
- [`opencharly/charly`](https://github.com/opencharly/charly) — the charly CLI and image builder
- [`opencharly/opencharly`](https://github.com/opencharly/opencharly) — the umbrella
