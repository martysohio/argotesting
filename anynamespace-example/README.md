# Apps in Any Namespace (Akuity)

An nginx Application whose CR lives in the `anynamespace` namespace on the Argo CD
control plane instead of `argocd`, deployed to the `general-demo` cluster.

```
anynamespace-example/
├── bootstrap-app.yaml          # parent app (argocd ns) -> control-plane/
├── control-plane/              # synced to in-cluster (Akuity control plane)
│   ├── namespace.yaml          # anynamespace namespace       (wave -2)
│   ├── appproject.yaml         # anynamespace AppProject      (wave -1)
│   └── nginx-app.yaml          # Application anynamespace/nginx (wave 0)
└── manifests/nginx/            # synced to general-demo / team-a-nginx
    └── deployment.yaml
```

## Prerequisites on the Akuity instance

1. **Apps in Any Namespace** enabled: Akuity UI → Argo CD instance → Settings →
   General → *Apps in any namespace*, or declaratively:

   ```yaml
   spec:
     instanceSpec:
       appInAnyNamespaceConfig:
         enabled: true
   ```

2. **Declarative Management** enabled (exposes the `in-cluster` destination so
   Argo CD can create the namespace, AppProject, and Application on the
   control plane).
3. A connected cluster named `general-demo`.

## Deploy

```sh
kubectl apply -f anynamespace-example/bootstrap-app.yaml   # against the Akuity instance
# or
argocd app create -f anynamespace-example/bootstrap-app.yaml
```

Check it:

```sh
argocd app list                 # shows argocd/anynamespace-bootstrap and anynamespace/nginx
argocd app get anynamespace/nginx
```

## How it works

- Argo CD only reconciles Applications outside `argocd` when the namespace is
  allowed by the instance (`application.namespaces`, which Akuity manages via
  the setting above).
- The Application's project must list that namespace in
  `spec.sourceNamespaces`. The `default` project does not, so the app uses the
  `anynamespace` project, which also restricts it to this repo and the
  `general-demo`/`team-a-nginx` destination.
- AppProjects themselves must stay in the `argocd` namespace.
