---
apiVersion: argoproj.io/v1alpha1
kind: Rollout
metadata:
  name: rollouts-demo

spec:
  replicas: 5
  revisionHistoryLimit: 2
  selector:
    matchLabels:
      app: rollouts-demo
  template:
    metadata:
      labels:
        app: rollouts-demo
    spec:
      containers:
      - name: rollouts-demo
        image: argoproj/rollouts-demo:blue # Initial version
---

# argotesting

What is:

A series of basic argocd self tutorial setups for the Akuity platform

Bootstrap/app/appset files exclude themselves to avoid recursive loops like this snippet in the source for basic-nginx-app.yaml

'''
    directory:
      exclude: 'basic-nginx-app.yaml' # Prevents the app from syncing itself
'''

# basic nginx 

Single pod nginx container that can be deployed.

argocd app create -f basic-nginx-app.yaml

# cronjobs

Simple alpine container that appears and sleeps for 30 seconds every 5 minutes

argocd app create -f alpine-cronjob-app.yaml

# application-set-single-cluster

launches an app set that launches arbitary numbers of apps based on the contents of the apps/ directory.

similarly ```argocd app create -f application-set-single-cluster/parent-app.yaml```

# application-set-matrix-generator

launches arbitrary apps based on apps/ directory, but duplicated across multiple clusters. In this case using a matrix generator to launch an app for each app in apps/ , then the same set on each cluster with the label `environment: prod`

```
argocd app create -f application-set-matrix-generator/parent-app.yaml
```