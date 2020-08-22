# Tips & Tricks - Openshift

## How to create deployments, jobs & pods 

###  "oc run" command can be used to generate pod/deployment/job. restart=Never generates a pod, restart=Always generates deployment, restart=onFailure generates a job
```
oc run busybox --image=busybox --restart=Never --dry-run=true -o yaml
oc run busybox --image=busybox --restart=Always --dry-run=true -o yaml
oc run busybox --image=busybox --restart=OnFailure --dry-run=true -o yaml
```
