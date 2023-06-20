## Deploy an app of apps in Openshift using ArgoCD and Helm 🌟

### Important info
There are two ways of the deploying the whole setup:
 1. Installing the Openshift GitOps operator in openshift-operators namespace. 
 2. Installing manually ArgoCD.

### OPTION 1: Openshift Gitops operator
 1. In this case, you will use the ArgoCD and ArgoCD project created by default in the openshift-gitops namespace. 
 2. Go to the ArgoCD route and access using its secrets:
    oc extract secret/openshift-gitops-cluster -n openshift-gitops --to=-
 3. Inside ArgoCD, create the app using the application-main.yaml
 4. Finished! The app should create other app from the java-helm repo.

### OPTION 2: Manual ArgoCD installation
1. Install ArgoCD using the argocd-installation folder. A namespace called argocd and the operator subscription should be created.
2. Configure ArgoCD using the argo-config folder. It will create an ArgoCD project and an ArgoCD cluster.
3. Go to the ArgoCD route and access using its secrets:
    oc extract secret/openshift-gitops-cluster -n openshift-gitops --to=-
 3. Inside ArgoCD, create the app using the application-main.yaml
 4. Should show permission problem!

