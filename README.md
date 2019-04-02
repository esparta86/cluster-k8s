# CI&CD 2018

## 📖 Docs
* [Deploy a app web with containers](https://cloud.google.com/kubernetes-engine/docs/tutorials/hello-app) -


## 🚀 Getting Started

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

#### 4. Push to gcr.io the new version of image


```bash

$ gcloud docker -- push gcr.io/${PROJECT_ID}/php-mailer:VERSION     # type in version the correct version of image. 

#OUTPUT


WARNING: `gcloud docker` will not be supported for Docker client versions above 18.03.
As an alternative, use `gcloud auth configure-docker` to configure `docker` to
use `gcloud` as a credential helper, then use `docker` as you would for non-GCR
registries, e.g. `docker pull gcr.io/project-id/my-image`. Add
`--verbosity=error` to silence this warning: `gcloud docker
--verbosity=error -- pull gcr.io/project-id/my-image`.
See: https://cloud.google.com/container-registry/docs/support/deprecation-notices#gcloud-docker
The push refers to repository [gcr.io/ti-is-devenv-01/php-mailer]
1430662f1e78: Pushed
ab101d35e70e: Pushed
0c748938246f: Pushed
68924d4e0107: Pushed
59c0bbf6e92d: Pushed
8148a896c159: Pushed
b96406f48542: Pushed
0bfe5b9ff896: Pushed
ea6c48058994: Pushed
8938bb6e21b3: Pushed
a999f4404317: Pushed
5c9f958f650e: Pushed
187644261ecb: Pushed
46e31b575320: Pushed
d672e1a90ae3: Pushed
588c419cab1e: Pushed
7230a1d38d48: Pushed
5eadb277a6a3: Pushed
08c80a513b86: Pushed
f4f00e5dd437: Pushed
fd7e5ff971db: Pushed
0972cc82b682: Layer already exists
v1: digest: sha256:9436405af4435e2d7dc49bcd6f5d6fa38e0f5a67f0991628d7f9a1f92a8799a8 size: 4926



```

#### 6. Update k8s/deployment-mailer.yaml with the new version image of php-mailer
Please Open the file and replace the version
please see the example:

```bash

apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  name: php-mailer-prod
  labels:
    app: php-mailer-prod
spec:
  replicas: 1
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxSurge: 1
      maxUnavailable: 0
  template:
    metadata:
      name: php-mailer-prod
      labels:
        app: php-mailer-prod
      labels:
        app: php-mailer-prod
        env: production
    spec:
      containers:
        - name: php-mailer
          image: gcr.io/ti-is-devenv-01/php-mailer:v3
          ports:
            - containerPort: 80

```


#### 3. Create/Update Deployment and Services
The last image is gcr.io/ti-is-devenv-01/php-mailer:v3

You can see the container registry for adauth.
Go to Google Cloud Platform -> Container Registry -> adauth

Name                     |  Hostname     | Visibility
php-mailer               | gcr.io        | Private


```bash
$ kubectl apply -f  k8s/deployment-phpmailer.yaml -n <NAMESPACE>

$ kubectl apply -f  k8s/service-phpmailer.yaml  -n <NAMESPACE>


```




