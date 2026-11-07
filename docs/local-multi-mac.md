# Local two-Mac setup

This fork is used by two Threadbear runners sharing one Convex deployment.

## Deployment

* Convex project: `threadbear`
* Shared production deployment: `https://fabulous-starfish-611.convex.cloud`
* Primary runner: `RM-MBP16M2`
* Secondary runner: `RM-MBP14M1`
* Secondary runner ID: `7ab9d0024679`

Do not commit or copy either Mac's `runner.json` or `secrets/convex-key`.
Each Mac must keep its own runner key. The primary Mac owns the deployment;
the secondary Mac uses a restricted runner key issued by the primary.

## Update procedure

Updates should preserve the local data directory under:

```text
~/Library/Application Support/Threadbear/
```

In particular, preserve:

* `runner.json`
* `secrets/convex-key`
* `sync-sent.json`
* local thread data and state

If the secondary Mac is ever reinstalled, use the supported joining flow:

```sh
./threadbear setup --existing --name "RM-MBP14M1"
```

Then, on the primary Mac, issue its dedicated key:

```sh
./runner/bin/tb runner add "RM-MBP14M1" --ssh <secondary-mac-host>
```

Never put the primary deployment owner key on the secondary Mac.

## Recovery check

On each Mac, inspect only the non-secret configuration:

```sh
cat "$HOME/Library/Application Support/Threadbear/runner.json"
```

The secondary must show the shared deployment URL and its own runner ID. If
its key is revoked or lost, issue a new one from the primary and restart its
runner; do not run ordinary setup against the shared deployment.
