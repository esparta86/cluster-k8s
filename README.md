# KUBERNETES

## 📖 Docs
* [Deploy a app web with containers](https://cloud.google.com/kubernetes-engine/docs/tutorials/hello-app) -


## 🚀 CREATE A CLUSTER USING GCP 

#### 1. CREATE A SERVICE ACCOUNT

```bash

gcloud iam service-accounts create custom-sa-k8s --display-name "custom-sa-k8s"


```

#### 2. GRANT ROLES TO SERVICE ACCOUNT

```bash
#-------------------------------------- Kubernetes Engine Cluster Admin: Management of Kubernetes Clusters. --------------------------------------
$ gcloud projects add-iam-policy-binding devops-k8-86 --member serviceAccount:custom-sa-k8s@devops-k8-86.iam.gserviceaccount.com --role roles/container.clusterAdmin 

#-------------------------------------- Compute admin: Full control of all Compute Engine resources. --------------------------------------
$ gcloud projects add-iam-policy-binding devops-k8-86 --member serviceAccount:custom-sa-k8s@devops-k8-86.iam.gserviceaccount.com --role roles/compute.admin 

#-------------------------------------- Source Repository Administrator: Admin access to repositories --------------------------------------
$ gcloud projects add-iam-policy-binding devops-k8-86 --member serviceAccount:custom-sa-k8s@devops-k8-86.iam.gserviceaccount.com --role roles/source.admin 

#-------------------------------------- Storage Admin :	Full control of GCS resources.. --------------------------------------
$ gcloud projects add-iam-policy-binding devops-k8-86 --member serviceAccount:custom-sa-k8s@devops-k8-86.iam.gserviceaccount.com --role roles/storage.admin
		
#-------------------------------------- Service Account User :Run operations as the service account. --------------------------------------

$ gcloud projects add-iam-policy-binding devops-k8-86 --member serviceAccount:custom-sa-k8s@devops-k8-86.iam.gserviceaccount.com --role roles/iam.serviceAccountUser


#-------------------------------------- Cloud build editor :Can create and cancel builds. --------------------------------------
$ gcloud projects add-iam-policy-binding devops-k8-86 --member serviceAccount:custom-sa-k8s@devops-k8-86.iam.gserviceaccount.com --role roles/cloudbuild.builds.editor

#-------------------------------------- Compute network admin : Full control of Compute Engine networking resources. --------------------------------------
$ gcloud projects add-iam-policy-binding devops-k8-86 --member serviceAccount:custom-sa-k8s@devops-k8-86.iam.gserviceaccount.com --role roles/compute.networkAdmin


#-------------------------------------- Storage Object Admin :  Full control of GCS objects... --------------------------------------
$ gcloud projects add-iam-policy-binding devops-k8-86 --member serviceAccount:custom-sa-k8s@devops-k8-86.iam.gserviceaccount.com --role roles/storage.admin

#-------------------------------------- Deployment Manager Editor : Read and Write access to all Deployment Manager resources. --------------------------------------
$ gcloud projects add-iam-policy-binding devops-k8-86 --member serviceAccount:custom-sa-k8s@devops-k8-86.iam.gserviceaccount.com --role roles/deploymentmanager.editor



```

#### 3. CREATE SERVICE ACCOUNT KEY , DOWNLOAD KEY


#### 3. ACTIVATE SERVICE ACCOUNT KEY 



```bash

gcloud auth activate-service-account custom-sa-k8s@devops-k8-86.iam.gserviceaccount.com  --key-file=devops-k8-86-137921350dfa.json

```

#### 4. REQUIRED ENABLE cloudresourcemanager.googleapis.com : Creates, reads, and updates metadata for Google Cloud Platform resource containers.


```bash

I tried to activate using service account but i got the next message
Enabling service [cloudresourcemanager.googleapis.com] on project [981069393234]...

WARNING: Listing available projects failed: User [custom-sa-k8s@devops-k8-86.iam.gserviceaccount.com] does not have permission to access service [cloudresourcemanager.googleapis.com:enable] (or it may not exist): The caller does not have permission

FIX: Activate api using GCP CONSOLE-> api&services -> libraries -> cloudresourcemanager




```

#### 6. REQUIRED ENABLE container.googleapis.com

before you are going to create a new cluster, you must to enable.

when you enable that API the following members were created:

```bash

NUMBER-PROJECT-compute@developer.gserviceaccount.com
NUMBER-PROJECT@cloudservices.gserviceaccount.com
service-NUMBER-PROJECT@compute-system.iam.gserviceaccount.com
service-NUMBER-PROJECT@container-engine-robot.iam.gserviceaccount.com
service-NUMBER-PROJECT@containerregistry.iam.gserviceaccount.com


```


#### 7. CREATE A NEW CLUSTER

```bash
$ gcloud container clusters create hello-cluster --num-nodes=2 --zone=us-west1-b

```

If you see the cluster´s vms, the service account in each vms is   NUMBER-PROJECT-compute@developer.gserviceaccount.com

please delete the hello-cluster

Create a new cluster again:
please execute the next gcloud command:

```bash
 gcloud beta container --project "devops-k8-86"  clusters create esparta-cluster --num-nodes 2 --cluster-version 1.11.7-gke.12  --machine-type n1-standard-2 --enable-ip-alias  --zone "us-west1-b" --enable-network-policy  --image-type ubuntu --service-account "custom-sa-k8s@devops-k8-86.iam.gserviceaccount.com" 

 ```




