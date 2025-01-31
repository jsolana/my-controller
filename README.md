# my-controller

A dummy k8s controller using `kubebuilder`.

For the scaffolding:

```bash
# Install kubebuilder
brew install kubebuilder

# scaffolding of the project following https://book-v1.book.kubebuilder.io/basics/project_creation_and_structure

kubebuilder init --domain example.com --repo github.com/jsolana/my-controller --owner "jsolana"

kubebuilder create api --group example --version v1 --kind Task
```

## Run the example

Prepare the cluster:

```bash
# Cluster creation with kind
kind create cluster

# Generate the CRDs
make manifests

# Install the manifests
make install

# Run the controller from the host
make run
```

Create an instance of the new `Task` resource:

```console
kubectl apply -f - <<EOF
apiVersion: example.example.com/v1
kind: Task
metadata:
  name: example-task
spec:
  foo: Hello World!
EOF
```

And the controller should trigger a log event like this one:

```console
2025-01-31T20:41:49+01:00       INFO    Task Hello World!       {"controller": "task", "controllerGroup": "example.example.com", "controllerKind": "Task", "Task": {"name":"example-task","namespace":"default"}, "namespace": "default", "name": "example-task", "reconcileID": "0540a4ad-01e0-4b44-9814-343c7bd4f449"}
```
