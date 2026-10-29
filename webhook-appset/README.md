# webhook-appset

ApplicationSet that creates one Argo CD Application per `apps/*/config.yaml`.
A GitHub push webhook refreshes it, so pushing a new app folder creates the app within seconds instead of on the next poll.

## Layout

```
webhook-appset/
  appset.yaml            # ApplicationSet "webhook-apps"
  parent-app.yaml        # optional: manages appset.yaml via GitOps
  apps/
    demo/
      config.yaml        # name / cluster / namespace for the generated app
      manifests/         # what the generated app deploys
```

## Setup

1. Install the ApplicationSet, using either:
   - `argocd appset create webhook-appset/appset.yaml`, or
   - `kubectl apply -f webhook-appset/parent-app.yaml` (GitOps-managed)
2. GitHub repo → Settings → Webhooks → Add webhook:
   - Payload URL: `https://<instance>.cd.akuity.cloud/api/webhook`
   - Content type: `application/json`
   - Events: Just the push event
   - Secret: optional; if set, use the same value in Akuity under Instance → Settings → Webhooks → GitHub

## Add an app

```bash
mkdir -p webhook-appset/apps/myapp/manifests
cat > webhook-appset/apps/myapp/config.yaml <<CFG
name: myapp
cluster: general-demo
namespace: myapp
CFG
# add manifests to webhook-appset/apps/myapp/manifests/
git add webhook-appset/apps/myapp && git commit -m "add myapp" && git push
```

Verify with `argocd appset get webhook-apps` and `argocd app get myapp`.

## Remove an app

Delete the folder and push. The ApplicationSet deletes the Application, and `prune: true` removes its resources.
