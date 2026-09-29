# AGENTS.md — layer-selkies-kde-desktop

Standalone candy repo for the `selkies-kde-desktop` layer — the KDE Plasma flavor
of the Selkies streaming desktop, a headless WebRTC-streamed Plasma pod. The
candy lives in `charly.yml` at the repo root: the `candy:` composition and the
`check:` probes, plus the embedded `skill:` entity projected into the marketplace
corpus as `/charly-selkies:selkies-kde-desktop`.

Canonical files:

- `charly.yml` — the `selkies-kde-desktop:` candy entity and the
  `selkies-kde-desktop-skill:` skill entity.
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — user overview only; never agent guidance.

## Load these skills first (R0)

- `/charly-selkies:selkies-kde-desktop` — the owning skill. The three-tier
  decomposition (selkies-core / kde-selkies / kde-shell), the de-SDDM design,
  the runtime encoder selection, and the KWin `wl:` coverage. Load before editing
  or troubleshooting the candy.
- `/charly-image:layer` — the candy authoring reference (`charly.yml` schema,
  `plan:` step verbs incl. `check:`, the nested `candy:` composition list).

## Build / validate / test

- The merge gate is the **org-wide** `charly/pr-validator` (required check
  `validate / validate`, defined in `opencharly/.github`); this repo has **no
  per-repo candy gate** and ships only `.github/workflows/tag-on-merge.yml`.
- `charly box validate` at the repo root checks the manifest parses and
  validates.
- The candy's `plan:` `check:` steps split into build-context (the kde-selkies
  launcher + its `startplasma-wayland` exec, `kwin_wayland`, `plasmashell`,
  Chrome, pipewire, the Plasma session/package) and runtime probes (the composed
  session reaching RUNNING under supervisord without crash-looping, and the
  streamed HTTPS UI answering on port 3000). The runtime probes are driven by the
  `check-selkies-kde-pod` bed.
- The stability check reads supervisord's reported uptime via charly's
  `eventually`/`retry_interval` rather than a fixed sleep — keep that shape.

## Modify this repo

- Edit the `selkies-kde-desktop:` candy entity AND the
  `selkies-kde-desktop-skill:` skill entity in `charly.yml` together. The skill
  is the projected usage source, so a composition or probe change not mirrored in
  the skill leaves the corpus stale.
- This metalayer installs nothing of its own — its observable behaviour IS the
  union of its children, so each child's key artifact needs a matching `check:`
  probe.
- The KDE seam is the nested compositor; do not duplicate `selkies-core` (R3).

## Landing

- PR-only. Every change lands through a pull request; the org-required
  `charly/pr-validator` validates the diff and body and arms native auto-merge
  on PASS. Direct pushes to `main` are blocked.
- History lives in `CHANGELOG/` (written by `tag-on-merge` at merge time); the
  PR body IS the changelog.
- The authoritative rulebook is the umbrella `AGENTS.md` in
  `opencharly/opencharly` and `charly/AGENTS.md` in the charly repo. Do not
  restate its rules here.
