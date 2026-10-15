# Projects

A project groups the websites (targets) you want to compare, their crawl runs, baselines and comparisons. Every project is a plain folder on your disk that you can copy, back up or move to another machine.

## Project folder

```text
<Project>/
  .diffkin/
    project.json        configuration: targets, capture profiles, rules
    state.sqlite        crawl runs, captures, comparisons, review decisions
    objects/            screenshots and DOM snapshots (content-addressed)
    runs/ comparisons/ exports/
    backups/            copies made automatically before a format migration
```

Diffkin only ever writes inside `.diffkin/`. Other files in the project folder are yours and are never touched. Credentials are **not** stored in the project folder; they live in your operating system's secure store and the project only keeps a reference to them.

## Create a project

1. Open **Projects** and choose **New project…**.
2. Enter a name and pick the location. Diffkin creates a folder named after the project inside that location (for example `Client Site` becomes `Client-Site`).
3. Choose **Create**. The project is registered and opened.

If a non-empty folder with that name already exists, Diffkin refuses to create the project. Pick another name or import the existing folder instead.

## Import a project

Use **Import folder…** and select a folder that contains a `.diffkin` directory, for example one copied from another machine. Diffkin validates the configuration, upgrades older project formats automatically (a backup is written to `.diffkin/backups/` first) and adds the project to your list.

A project created by a newer Diffkin version than the one installed cannot be opened. Update Diffkin; the files are left unchanged.

## Rename

**Rename** changes the display name stored in `project.json`. The folder name stays as it is.

## Remove from Diffkin vs. delete data

These are deliberately separate actions:

| Action | What happens |
| --- | --- |
| **Remove from Diffkin** | The project disappears from your list. The folder and all data stay on disk. Import it again at any time. |
| **Delete data…** | The `.diffkin` folder with all captures, baselines and comparisons is permanently deleted. You must type the project name to confirm. Other files in the project folder are kept; an empty project folder is removed. |

## Unavailable projects

If a project folder was moved or deleted, the project is listed as **unavailable**. Remove it from Diffkin and import it from its new location.

## Recovering from a failed upgrade

If upgrading a project format fails, Diffkin reports the error and the location of the backup in `.diffkin/backups/`. The original files are not modified. Copy `project.json` and `state.sqlite` from the backup back into `.diffkin/` to return to the previous state, then report the problem with the error message.

If Diffkin reports that the project is locked for migration although no other Diffkin instance is running, delete `.diffkin/migration.lock` and try again.
