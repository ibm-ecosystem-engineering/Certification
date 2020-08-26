# Tips & Tricks - Openshift

## Practice clusters 
### Which cluster should you use to practice for certification?

#### - IBM Cloud clusters: Understand IBM adds some customizations which will make it different from the one you will get in certification. Ex: RBAC roles, CNI (IBM Cloud uses calico), Persistent Storage using cloud storage classes etc.
#### - Use https://learn.openshift.com/ for most exercises, but here you get only single node clusters


## How to create deployments, jobs & pods 
###  "oc run" command can be used to generate pod/deployment/job

#### restart=Never generates a pod, restart=Always generates deployment, restart=onFailure generates a job
```
oc run busybox --image=busybox --restart=Never --dry-run=true -o yaml
oc run busybox --image=busybox --restart=Always --dry-run=true -o yaml
oc run busybox --image=busybox --restart=OnFailure --dry-run=true -o yaml
```

## Create aliases for commonly used commands
### Minimize typing by creating alises for commonly used commands
```
alias ogp='oc get pods'
alias ogd='oc describe pod'
```