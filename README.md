# Actions Sync

<p align="center">
  <img src="docs/arrow.png">
</p>

Sync GitHub Action repositories from  to your GHES instance.

This tool is a standalone Go tool to allow you to sync from [GitHub](https://www.github.com) to a [GitHub Enterprise instance](https://github.com/enterprise). Referred to as `GHES` throughout this readme.

* Current status: **ready for production use**
* Download from: [releases page](https://github.com/actions/actions-sync/releases/)
* Build status: ![Actions Sync Status](https://github.com/actions/actions-sync/workflows/CI/badge.svg)

It is designed to work when:
* The GitHub Enterprise instance is seperate from the rest of the internet.
* The GitHub Enterprise instance is connected to the rest of the internet.

## Connected instances

When there are machines which have access to both the public internet and the the GHES instance run `actions-sync sync`.

**Command:**

`actions-sync sync`

**Arguments:**

- `cache-dir` _(required)_
   The directory in which to cache repositories as they are synced. This speeds up re-syncing.
- `destination-url` _(required)_
   The URL of the GHES instance to sync repositories onto.
- `destination-token` _(required)_
   A personal access token to authenticate against the GHES instance when uploading repositories.
- `repo-name` _(optional)_
   A single repository to be synced. In the format of `owner/repo`. Optionally if you wish the repository to be named different on your GHES instance you can provide an aliase in the format: `upstream_owner/up_streamrepo:destination_owner/destination_repo`
- `repo-name-list` _(optional)_
   A comma-separated list of repositories to be synced. Each entry follows the format of `repo-name`.
- `repo-name-list-file` _(optional)_
   A path to a file containing a newline separate listof repositories to be synced. Each entry follows te format of `repo-name`.

**Example Usage:**

```
  actions-sync sync \
    --cache-dir "tmp/cache" \
    --destination-token "token" \
    --destination-url "www.example.com" \
    --repo-name actions/setup-node
```

## Not connected instances

When no machine has access to both the public internet and the GHES instance:

1. `actions-sync pull` on a machine with public internet access
2. copy the provided `cache-dir` to a machine with access to the GHES instance
3. run `actions-sync push` on the machine with access to the GHES instance

**Command:**

`actions-sync pull`

**Arguments:**

- `cache-dir` _(required)_
   The directory to cache the pulled repositories into.
- `repo-name` _(optional)_
   A single repository to be synced. In the format of `owner/repo`. Optionally if you wish the repository to be named different on your GHES instance you can provide an aliase in the format: `upstream_owner/up_streamrepo:destination_owner/destination_repo`
- `repo-name-list` _(optional)_
   A comma-separated list of repositories to be synced. Each entry follows the format of `repo-name`.
- `repo-name-list-file` _(optional)_
   A path to a file containing a newline separate listof repositories to be synced. Each entry follows te format of `repo-name`.

**Example Usage:**

```
  bin/actions-sync pull \
    --cache-dir "/tmp/cache" \
    --repo-name actions/setup-node
```

**Command:**

`actions-sync push`

**Arguments:**

- `cache-dir` _(required)_
   The directory containing the repositories fetched using the `pull` command.
- `destination-url` _(required)_
   The URL of the GHES instance to sync repositories onto.
- `destination-token` _(required)_
   A personal access token to authenticate against the GHES instance when uploading repositories.

**Example Usage:**

```
  bin/actions-sync push \
    --cache-dir "/tmp/cache" \
    --destination-token "token" \
    --destination-url "http://www.example.com"
```


## Contributing

If you would like to contribute your work back to the project, please see
[`CONTRIBUTING.md`](CONTRIBUTING.md).