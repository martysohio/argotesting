# argotesting

What is:

A series of basic argocd self tutorial setups for the Akuity platform

# basic nginx 

Single pod nginx container that can be deployed.


argocd app create basic-nginx \
  --repo git@github.com:martysohio/argotesting.git \
  --path basic-nginx \
  --dest-server https://cluster-<cluster-name>:8001 \
  --dest-namespace basic-nginx