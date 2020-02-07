## Prerequisites
- Install Terraform
- Install Cloud SDK
https://cloud.google.com/sdk/docs/quickstart-debian-ubuntu#troubleshooting_your_installation

- GCP Project
- Enable API for the GCP Project
  - Google Compute Engine API
  - Kubernetes Engine API

## Procedure
1. Create Service Account
- create sa using GKE
- add role `GKE Admin`, `Compute Admin`, `Storage Admin`

2. Authenticate to GCP
- Log in with the service account which you created 
<details>
<summary> Commands </summary>

tetsuya@tetsuya-Ubuntu:~/GCP$ gcloud auth activate-serviceaccount sa-terraform-gke@devops-hpe-isogai.iam.gserviceaccount.com --key-file /home/tetsuya/.gcp-key/devops-hpe-isogai-e64ce9f61bfb.json
ERROR: (gcloud.auth) Invalid choice: 'activate-serviceaccount'.
Maybe you meant:
  gcloud auth

To search the help text of gcloud commands, run:
  gcloud help -- SEARCH_TERMS
tetsuya@tetsuya-Ubuntu:~/GCP$ gcloud auth --help
tetsuya@tetsuya-Ubuntu:~/GCP$ gcloud auth activate-service-account sa-terraform-gke@devops-hpe-isogai.iam.gserviceaccount.com --key-file /home/tetsuya/.gcp-key/devops-hpe-isogai-e64ce9f61bfb.json
Activated service account credentials for: [sa-terraform-gke@devops-hpe-isogai.iam.gserviceaccount.com]
tetsuya@tetsuya-Ubuntu:~/GCP$ gcloud config set project devops-hpe-isogai
Updated property [core/project].
WARNING: You do not appear to have access to project [devops-hpe-isogai] or it does not exist.
tetsuya@tetsuya-Ubuntu:~/GCP$ gcloud auth list
                      Credentialed Accounts
ACTIVE  ACCOUNT
*       sa-terraform-gke@devops-hpe-isogai.iam.gserviceaccount.com

To set the active account, run:
    $ gcloud config set account `ACCOUNT`

tetsuya@tetsuya-Ubuntu:~/GCP$ gcloud config set account sa-terraform-gke@devops-hpe-isogai.iam.gserviceaccount.com
Updated property [core/account].
tetsuya@tetsuya-Ubuntu:~/GCP$ gcloud config list
[core]
account = sa-terraform-gke@devops-hpe-isogai.iam.gserviceaccount.com
disable_usage_reporting = True
project = devops-hpe-isogai

Your active configuration is: [default]
</details>

3. Create Bucket for tfstate file
`gcloud mb -p <project-name> -c <storage-class> -l Asia gs://devops-hpe-isogai-bucket/`
 
gsutil mb -p devops-hpe-isogai -c Nearline -l Asia gs://devops-hpe-isogai-bucket/

4. Install Terraform
- download terraform from https://www.terraform.io/downloads.html  
- `unzip` the zip file
- locate anywhare you want and set `$PATH`

