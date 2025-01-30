# Building a Kubernetes Controller with Kubebuilder 4.5.0

We will: 
- Use Kubebuilder to generate a controller
- Watch a Custom Resource (CRD) and manage a Deployment

# Step 1: Initialize the Project with kubebuilder

```bash
kubebuilder init --domain example.com --repo github.com/mahdibouaziz/kubebuilder-playground/my-controller
```

## What this does?

- `kubebuilder init`: Initializes the project
- `--domain example.com`: The API group will be myresource.example.com
- `--repo github.com/mahdibouaziz/kubebuilder-playground/my-controller`: The Go module name.

## What happens?

- Creates a Go module (`go.mod`).
- Generates the core project structure (`config/`, `cmd/main.go`).
- Adds a `PROJECT` file that describes the Kubebuilder project structure.

# Step 2: Create a Custom Resource Definition (CRD)

Now, we define a new API group for our controller.

```bash
kubebuilder create api --group apps --version v1 --kind MyResource
```

## What this does?

- `kubebuilder create api` : Scaffold a Kubernetes API by writing a Resource definition and/or a Controller.
- `--group apps` : Defines API group as `apps.example.com`
- `--version v1`: API version is `v1`
- `--kind MyResource`: The name of our custom resource.

- This wil ask you if you want to create the scaffold of: 
    - A Resource (under `api/<version>`)
    - A controller (under `internal/controller`)

For the CRD, we are intersted with the `api/v1` folder in this case

```bash
api/v1/
├── myresource_types.go        # Defines the schema for MyResource
├── groupversion_info.go       # Defines API group and version metadata
├── zz_generated.deepcopy.go   # Auto-generated deep copy functions
```

# Step 3: Define the CRD Schema

Check `api/v1/myresource_types.go` to define your CRD

- `MyResourceSpec` → Defines the desired state (Replicas, Image).
- `MyResourceStatus` → Reflects the actual state (AvailableReplicas).
- Annotations:
    - `+kubebuilder:object:root=true` → Marks this as a CRD.
    - `+kubebuilder:subresource:status` → Adds a .status field for updates.

# Step 4: Generate the CRD YAML

Run:

```bash
make manifests
```

This will:

- Generates `config/crd/bases/apps.example.com_myresources.yaml`
- Creates RBAC roles in `config/rbac/role.yaml`

# Step 5: Implement the Controller

go to `internal/controller/myresource_controller.go` and implement the controller logic

# Step 6: Deploy the Controller

Generate manifests & RBAC

```bash
make manifests
make generate
```

Apply the CRD

```bash
make install
```

Run the controller locally

```bash
make run
```

Apply a CR Create myresource.yaml

```yaml
apiVersion: apps.example.com/v1
kind: MyResource
metadata:
  name: myapp
spec:
  replicas: 3
  image: nginx
```