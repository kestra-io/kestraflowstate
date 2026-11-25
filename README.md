# kestraflowstate

Kestra hackathon submission. Workflows are built on the Jaffle Shop dataset and live in [`flows/`](flows/).

## Run it

[`sync/sync_and_run_kestraflowstate.yaml`](sync/sync_and_run_kestraflowstate.yaml) pulls everything in `flows/` from `main` into a Kestra namespace (`hackathon` by default) and can run the flows straight after. Set the `run` input to false to sync only.

1. Add the sync flow to Kestra 2.0. The comment at the top of the file lists the plugins and the `KESTRA_API_TOKEN` secret it needs.
2. Execute it. Each synced flow's description lists any setup of its own.

You can also import the files in `flows/` by hand and click **Execute** on each one.
