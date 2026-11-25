# kestraflowstate

Kestra hackathon submission. Workflows are built on the Jaffle Shop dataset and live in [`flows/`](flows/).

## Run it

1. Import the YAML files from `flows/` into Kestra, or sync them with `io.kestra.plugin.git.SyncFlows` using `gitDirectory: flows`.
2. Open a flow and click **Execute**. Each flow's description lists any setup it needs.
