# Index
1. Deploy an app of apps in Openshift using ArgoCD and Helm
2. Deploy a multiArgoCD architecture
3. Permission problems


## 1. Deploy an app of apps in Openshift using ArgoCD and Helm
We will use an ArgoCD instance that deploys an app that deploys other apps.

#### 1. Install Openshift Gitops operator

Once installed, a namespace called openshift-gitops with a predefined ArgoCD should be created.
```bash
oc process -f templates/argocd-sub.yaml | oc apply -f -
```

#### 2. Get into ArgoCD
- Inside the openshift-gitops project, go to Routes and open the ArgoCD route.
- Log in via Openshift with your cluster-admin user.

#### 3. Deploy the main app
- Select "Create an app" and edit the yaml.
- Copy and paste the content of argocd-config/application-main.yaml.
- The main app should be created deploying an instance of myapp.
- In case of a permission error, go to the [Permission problems section](#permission-problems).

## 2. Deploy a multiArgoCD architecture
In this case, we will have two ArgoCDs:

##### First ArgoCD
- It is created by default by the Openshift Gitops operator.
- It will be in charge of deployin the second ArgoCD and creating apps namespaces.
##### Second ArgoCD
- It will deploy the apps in the namespaces created by the first ArgoCD.

#### 1. Install the Openshift Gitops operator as done before.
#### 2. Get into ArgoCD as done before.
#### 3. Deploy the second ArgoCD objects and the apps namespaces.
- Select "Create an app" and edit the yaml.
- Copy and paste the content of multicluster/argocd-applications/application-main-gitops.yaml.
- This should deploy the contents of multicluster/second-layer.
#### 4. Get into the second ArgoCD.
- Inside the gitops project, go to Routes and open the ArgoCD route.
- Again, log in via Openshift with your cluster-admin user.
#### 5. Deploy the app.
- Select "Create an app" and fill depending on the app you would like to deploy.
- Take into account that if this app is deploying a namespace, it should be managed by the second ArgoCD. See more in the Permission problems section.

## 3. Permission problems

#### Problem 1: Unable to create project: permission denied
- The user that is creating the app does not have permission to do so.
- This can be changed in the ArgoCD object definition, in the rbac section:
  
```yaml
  rbac:
    policy: |
      g, system:cluster-admins, role:admin
      g, cluster-admins, role:admin
    scopes: '[groups]'
```
- Solution 1: Make sure the user is a member of the groups.
- Solution 2: Create a new group with the user and add the group to the rbac.

#### Problem 2: Namespace is not managed.
- Happens when ArgoCD does not have permission to manage a namespace.
- Solution: Label the namespace as the following:
```bash
oc label namespace <name-of-namespace> argocd.argoproj.io/managed-by=<name-of-argocd-instance>
```


