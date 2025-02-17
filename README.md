# Blockscout

# Custom Blockscout Helm Chart

Deploy the Blockscout Helm chart using custom values provided in myvalues.yaml.

This approach uses a custom myvalues.yaml file to override specific deployment settings.

The original chart can be found at the following GitHub repository: https://github.com/blockscout/helm-charts/tree/main/charts/blockscout-stack

# How to Use

## Pre-requisites

terraform fmt

terraform init

terraform validate

terraform apply

On the first apply process, there will be a failure because the kubernetes config is initialize at the previous config. Therefore, please RETRY by running

terraform apply

again, and on the second run the kubernetes config will be correctly initialize with the current config and kubernetes secrets will be successful.

DO NOT terraform destroy !

Run Terraform script to ensure the RDS and Kubernetes cluster is created. 

## To use this customized Helm chart, run the following command:

helm install -f ./myvalues.yaml my-blockscout blockscout/blockscout-stack 

### To access the frontend and backend url

kubectl get svc

There will be an external IP for each service

### To connect frontend to backend

Open myvalues.yaml and navigate to the frontend section

Find the "env" section

Replace NEXT_PUBLIC_API_HOST value with backend url and NEXT_PUBLIC_APP_HOST value with frontend url

Upgrade after replacing the configuration in myvalues.yaml

helm upgrade my-blockscout blockscout/blockscout-stack -f myvalues.yaml

### To Enable HPA

Comment out the replicasCount field to ensure no conflict when using hpa to dynamically scale your pods

Ensure that the blockscout release name is not changed during install

Then apply these commands

kubectl apply -f hpa.yaml

helm upgrade my-blockscout blockscout/blockscout-stack -f myvalues.yaml 

## Jenkins CI

Jenkinsfile : Script to automate deployment

ConsoleOutput.txt : Console Output for successful build using Jenkins CI


