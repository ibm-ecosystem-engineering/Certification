# Level-0 question

## Troubleshooting 

### Troubleshoot and fix the following application. On the cluster you are working, create the application by executing the following:
```
oc new-project broken
oc apply -f app.yaml
```

#### - Image for the deployment should come from internal registry. Original image: bitnami/nginx
#### - Create a route to access the application externally

<details><summary>Solution</summary>
<p>

#### - Check the pods that are running
```
oc run nginx --image=bitnami/nginx --restart=Always --replicas=3
```

</p>
</details>