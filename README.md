# kestraflowstate

Kestra hackathon submission. Workflows are built on the Jaffle Shop dataset and live in [`flows/`](flows/).

## Run it

Two flows in [`sync/`](sync/) pull everything in `flows/` into a Kestra namespace (`hackathon` by default) and run it. Neither hardcodes flow names, so new files in `flows/` get picked up. Set the `run` input to false to sync without running.

- [`sync_and_run_kestraflowstate.yaml`](sync/sync_and_run_kestraflowstate.yaml) syncs and runs, no checks.
- [`safe_sync_and_run_kestraflowstate.yaml`](sync/safe_sync_and_run_kestraflowstate.yaml) checks every flow in a container first (valid with `kestractl`, no script or container tasks, no data from hosts outside an allowlist), then calls the flow above. One failed check stops the sync.

1. Add both to the same namespace in Kestra 2.0. The comment at the top of each file lists the plugins and the `KESTRA_API_TOKEN` secret they need.
2. Execute one. Each synced flow's description lists any setup of its own.

You can also import the files in `flows/` by hand and click **Execute** on each one.
