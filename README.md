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
