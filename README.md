<p align="center">
  <b>pkgs</b>, the dfkup packages registry<br>
  Consumed by <code>dfkup pkg</code>. One JSON file, no backend.
</p>

## What is this?

This repository holds the package index for the
[dfkup](https://github.com/dfkup/dfkup) scripting language. The dfkup
package manager (powered by [datpkgr](https://github.com/openpeeps/datpkgr))
downloads one file from here:

```
https://raw.githubusercontent.com/dfkup/pkgs/main/packages.json
```

and seeds its local database from it. That file is the entire registry.

## Entry schema

`packages.json` is a JSON array. Each entry looks like this:

```json
{
  "name": "myutils",
  "url": "https://github.com/you/myutils",
  "method": "git",
  "tags": [],
  "description": "handy string helpers",
  "license": "MIT",
  "web": "https://github.com/you/myutils"
}
```

| Field         | Required | Notes                                                              |
|---------------|----------|--------------------------------------------------------------------|
| `name`        | yes      | Must match the `name` in the package's `package.kup` manifest.     |
| `url`         | yes      | Git clone URL of the package repository.                           |
| `method`      | no       | Download method. Only `git` is supported today.                    |
| `tags`        | yes      | Informational tag list (may be `[]`). Real versions come from tags.|
| `description` | yes      | One-line description shown by package tooling.                     |
| `license`     | yes      | Informational license identifier.                                  |
| `web`         | yes      | Homepage of the package.                                           |

An entry missing any required field is skipped silently, as is any entry
carrying an `alias` key. Keep the array sorted by `name` so diffs stay
readable. When in doubt, copy the example above and fill in your values.

## Versions

Available versions are discovered from the package repository's git tags
at install time, not from this file. Tag releases as semver (`v0.1.0`,
`0.1.0`) so version constraints (`>= 0.2.0`, `^1.0.0`) resolve sanely.
Repos without semver tags install as rolling `HEAD`.

## Adding a package

1. Your repository must carry a `package.kup` manifest at its root whose
   `name` matches the entry you add here (see the dfkup README for the
   manifest format). Exported functions carry the `*` marker so scripts
   can call them after `import "pkg/<name>"`.
2. Add one entry to `packages.json` (keep it sorted by `name`).
3. Open a pull request. Keep the file valid JSON - a trailing comma
   breaks every dfkup install until it is fixed.

## License

MIT. Made by Humans from [OpenPeeps](https://github.com/openpeeps).
Copyright dfkup & Contributors &mdash; All rights reserved.
