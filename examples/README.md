## Pipeline using Jumpstarter Setup SA Client
This pipeline dynamically sets up the client config using the Kubernetes service account, then acquires a lease, runs a command, and releases the lease.

## Pipeline using a Preconfigured Client Secret
This pipeline assumes a Jumpstarter Secret containing the client config already exists and is mounted via a workspace, then acquires a lease, runs a command, and releases the lease.
