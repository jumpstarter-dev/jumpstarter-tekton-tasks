# Jumpstarter Setup SA Client Task

This Tekton task creates a Jumpstarter client config from the Kubernetes service account. This requires that Jumpstarter is configured to trust authentication from Kubernetes.

## Parameters
- `endpoint`: The Jumpstarter grpc endpoint. **(Required)**
- `namespace`: The Jumpstarter client namespace. **(Required)**
- `name`: The Jumpstarter client name. **(Required)**
- `insecure-tls`: Use insecure TLS for gRPC. (__default__: `"false"`)
- `tls-ca`: CA Cert for the TLS endpoint. (__default__: `""`)

## Results
- `config`: The content of the generated Jumpstarter client config file.

## Workspaces
- `config-dir`: The workspace which going to contain the Jumpstarter client config file.

## Example Output
The task generates a `default.yaml` file in the `config-dir` workspace with the following structure:

```yaml
apiVersion: jumpstarter.dev/v1alpha1
kind: ClientConfig
metadata:
  namespace: <namespace>
  name: <name>
endpoint: <endpoint>
tls:
  insecure: <insecure-tls>
  ca: "<tls-ca>"
token: __POD_TOKEN__
drivers:
  allow: []
  unsafe: True
```

## Usage
```yaml
apiVersion: tekton.dev/v1
kind: TaskRun
metadata:
  name: jumpstarter-setup-sa-client-run
spec:
  taskRef:
    name: jumpstarter-setup-sa-client
  params:
    - name: endpoint
      value: grpc.jumpstarter.apps.cluster.com:443
    - name: namespace
      value: jumpstarter
    - name: name
      value: my-client
    - name: insecure-tls
      value: "true"
    - name: tls-ca
      value: ""
  workspaces:
    - name: config-dir
      emptyDir: {}
```