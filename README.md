# Osprey Home Assistant apps

This repository is the Home Assistant **app repository** for Osprey. Home
Assistant reads an app repository from the root of a Git repository: a
`repository.yaml` file at the root and one directory per add-on, named after
the add-on slug.

Add it in Home Assistant under **Settings > Apps > App store > Repositories**:

```
https://github.com/grayslawson/osprey-ha-apps
```

## What is in here

| Path | What it is |
| --- | --- |
| `repository.yaml` | The repository metadata Home Assistant reads. |
| `osprey/` | The Osprey add-on: its manifest, its entrypoint and the application source it is built from. |
| `osprey/src/` | The Osprey application source, vendored so the add-on can be built on its own. |

The add-on installs a published image from GHCR, pinned to the add-on version
in `osprey/config.yaml`. The image itself is built by the release workflow in
the [Osprey repository](https://github.com/grayslawson/osprey).

## How it stays in step

`osprey/` and `osprey/src/` are mirrored from the Osprey repository by
`scripts/sync-ha-apps.sh` there. The [sync workflow](.github/workflows/sync.yml)
in this repository runs that script on a schedule and on demand, and commits
the result when something changed. The mirror is therefore a copy: see the
Osprey repository for the source of truth.
