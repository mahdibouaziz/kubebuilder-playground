# Building a Kubernetes Controller with Kubebuilder 4.5.0

We will: 
- Use Kubebuilder to generate a controller
- Watch a Custom Resource (CRD) and manage a Deployment

# Intitalize a kubebuilder project

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