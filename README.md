# This folder contains files required for gitops-style halyard
## This folder contains files required for git-based Halyard updates.
1. config and default : The init script copies them to go into .hal directory of the halyard pod
2. halyard.yaml: this, along with a generic halyard-local.yaml (created by init) are copied into the /opt/halyard/config folder

## Triggering the hal-deploy-apply
This is achieved with two files
1. hal-deploy-apply.yaml: This is a job with halyard image that executes hal-deploy-apply command. It essentially waits for the halyard to be ready and executes "hal apply deploy".
2. pipeline-redeploy-hal.json uses the above hal-deploy-apply.yaml and is used to create a pipeline to make the new changes effective.

## USAGE
For this entire thing to work we need the following:
1. This folder
2. Dynamic accounts repo where Jenkins credentials, Kubernetes accounts & AWS credentials are stored.
2. Specify the Jenkins URL in the config file under ci.jenkins.masters.address; Jenkins credentials shall be encoded and specified in the Dynamic accounts repo.
3. OES helm chart that goes with this
3. Dynamic account git repo that exists and url fed via opsmx-gitops-secret.yaml file in oes helm chart.
4. A pipeline can be created in spinnaker using pipeline-redeploy-hal.json that shall be triggered whenever changes are made to this repo.
