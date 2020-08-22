# Tips & Tricks - Openshift

## Practice clusters (This recoommendation  is just for certification purposes)
### Which cluster should you use to practice for certification?

#### - Avoid using IBM Cloud clusters, mainly because cloud clusters add some customizations which will make it different from the one you will get in certification. If you are an expert user, who can deal with the customizations, it's not an issue.
#### - Use https://learn.openshift.com/ for most exervises, but here you get only single node clusters
#### - Fyre clusters 


## How to create deployments, jobs & pods 
###  "oc run" command can be used to generate pod/deployment/job. 
#### restart=Never generates a pod, restart=Always generates deployment, restart=onFailure generates a job
```
oc run busybox --image=busybox --restart=Never --dry-run=true -o yaml
oc run busybox --image=busybox --restart=Always --dry-run=true -o yaml
oc run busybox --image=busybox --restart=OnFailure --dry-run=true -o yaml
```
