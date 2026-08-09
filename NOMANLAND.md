# wlroots — NomanLand base layer

This is a downstream fork of [swaywm/wlroots](https://github.com/swaywm/wlroots)
(`origin` = `kanazawahere/wlroots`, `upstream` = `swaywm/wlroots`), forming the
compositor-toolkit base layer of **NomanLand**: an agent-friendly Wayland
stack — i.e. a Wayland environment that does not force interactive
human-consent gates on things a real desktop legitimately gates (screen
capture, input injection) when there is no human present to grant them
(unattended/headless server use cases).

## Status: unmodified so far

As of the initial fork (2026-08-09), **no code changes have been made to
wlroots itself**. Everything NomanLand needed so far — headless screen
capture without a consent dialog (`zwlr_screencopy_manager_v1`), input
injection without a consent dialog (`/dev/uinput`, entirely outside
Wayland's protocol layer), clipboard access without a consent dialog (core
`wl_data_device_manager`, no portal involved) — was **already present,
unmodified, in upstream wlroots**. The human-consent gate that motivated
this project (the `ScreenCast` portal's mandatory dialog) lives in the
`xdg-desktop-portal-*` backend layer, one level above wlroots, and NomanLand
simply talks to wlroots' own protocols directly instead of going through
that portal layer — see the sibling fork
[kanazawahere/rustdesk](https://github.com/kanazawahere/rustdesk)
(`docs/NOMANLAND.md`) for the actual client-side implementation.

This fork exists to establish a base **we control** for whatever wlroots-level
changes NomanLand does eventually need, and to pin a known-good version
independent of upstream's release cadence — not because wlroots required
surgery to reach the current state.

## Relationship to the rest of NomanLand

| Layer | Repo | Status |
|---|---|---|
| Compositor toolkit | this repo (`wlroots`) | forked, unmodified so far |
| Compositor | [kanazawahere/sway](https://github.com/kanazawahere/sway) | forked, unmodified so far |
| Client (screen capture backend) | [kanazawahere/rustdesk](https://github.com/kanazawahere/rustdesk) | forked, patched (`wlr_screencopy` module) |

See `03_toolkit/deps.toml` (entry `wlroots`) in the ATP Central_Command fleet
repo for how this fits into the broader dependency-tracking scheme.
