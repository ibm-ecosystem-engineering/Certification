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

#### - Check the deployments & pods that are running
```
oc get deploy
oc get pods
```
Pods are failing with ImagePullBack error. 

#### - Find out the image used:
```
oc get dc nginx -o yaml
```
Image used here is: image-registry.openshift-image-registry:5000/broken/nginx

#### - Pull the image to see if it exists
```
docker login $(oc get route default-route -n openshift-image-registry -o jsonpath='{.spec.host}') -u $(oc whoami) -p $(oc whoami -t)
docker pull image-registry.openshift-image-registry:5000/broken/nginx
```
Image is not available in the private registry

#### - Tag & Push the image to the private registru
```
docker pull bitnami/nginx
docker tag bitnami/nginx $(oc get route default-route -n openshift-image-registry -o jsonpath='{.spec.host}')/broken/nginx
docker push $(oc get route default-route -n openshift-image-registry -o jsonpath='{.spec.host}')/broken/nginx
```
</p>
</details>