# Level-0 question

## Idling Applications

### Create an application and set it up so that it can conserve resources when not used (5 mins)
#### - Create a deployment named nginx from bitnami/nginx image from dockerhub with 3 copies running on the cluster
#### - Create a service to access the deployment
#### - Create a route to access the application externally
#### - Setup the deployment to conserve resources when not used

<details><summary>Solution</summary>
<p>

#### - Create a deployment named nginx from bitnami/nginx image from dockerhub with 3 copies running on the cluster
```
oc run nginx --image=bitnami/nginx --restart=Always --replicas=3
```

#### - Create a service to access the deployment
```
oc expose dc nginx --port=8080 
```

#### - Create a route to access the application externally
```
oc expose service nginx
```

#### - Check if application is accessible
```
curl $(oc get route nginx -o jsonpath='{.spec.host}')
```

#### - Sertup service to idle
```
oc idle service nginx 
```

#### - Manually scale up services
```
oc scale --replicas=3 dc nginx 
```

#### - Automatic unidyling of services (doesn't seem to work)
```
curl $(oc get route nginx -o jsonpath='{.spec.host}')
```

</p>
</details>