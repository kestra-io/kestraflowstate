# kestraflowstate

Kestra hackathon submission. Workflows are built on the Jaffle Shop dataset and live in [`flows/`](flows/).

## Run it

Two flows in [`sync/`](sync/) pull everything in `flows/` into a Kestra namespace (`hackathon` by default) and run it. Neither hardcodes flow names, so new files in `flows/` get picked up. Set `run` to false to sync without running, and `branch` to sync from somewhere other than `main`.

- [`sync_and_run_kestraflowstate`](sync/sync_and_run_kestraflowstate.yaml) syncs, then runs every flow in the namespace. No checks.
- [`safe_sync_and_run_kestraflowstate`](sync/safe_sync_and_run_kestraflowstate.yaml) scans the flows first, then calls the flow above. One failed check stops the sync.

Each flow runs with its input defaults and in its own Loop iteration, so one failing flow doesn't stop the others or fail the sync execution. Check each flow's own execution in the target namespace.

### Setup (Kestra 2.0)

- Deploy both sync flows to the same namespace.
- Add a `KESTRA_API_TOKEN` secret there. The sync flows call the Kestra API with it.
- Install the plugins: `git` for both, `script-shell` for the safe flow, and whatever the synced flows use (currently `jdbc-duckdb`, `serdes` and `ai`).
- Set `kestra_url` so the Kestra worker can reach the webserver.
- For the safe flow, mount the Docker socket into Kestra and set `kestra_url_from_docker` so a sibling container can reach the webserver. On Docker Desktop that's `host.docker.internal` plus the host port.
- Each synced flow's description lists any setup of its own.

### What the safe flow checks

It runs in an Alpine container against the cloned repo:

- `kestractl flows validate` passes.
- No external code: no `scripts.*`, Docker, Kubernetes, SSH or task-runner task types (old `io.kestra.core.tasks.scripts.*` names included), and no `taskRunner` or `containerImage` anywhere.
- Task types start with a prefix in `allowed_type_prefixes`.
- Every URL outside text fields (`description`, `message`, `errorMessage`, `displayName`) starts with a prefix in `allowed_url_prefixes`. HTTP task URLs can only be literals or `{{ vars.x }}`, since a URL built at runtime can't be checked.
- DuckDB SQL doesn't `INSTALL`/`ATTACH`, load `httpfs` or read s3, gcs or azure paths.
- Subflows only call flows in their own namespace.

Both allowlists are in the flow's `variables`. It's a static scan, so it catches obvious problems rather than proving a flow safe. AI tasks are allowed and send their prompts to the provider. Keep the target namespace's secrets to what the flows need.

You can also import the files in `flows/` by hand and click **Execute** on each one.
