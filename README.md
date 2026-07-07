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

# application-set-single-cluster

launches an app set that launches arbitary numbers of apps based on the contents of the apps/ directory.

# application-set-matrix-generator

launches arbitrary apps based on apps/ directory, but duplicated across multiple clusters. In this case using a matrix generator to launch an app for each app in apps/ , then the same set on each cluster with the label `environment: prod`