# Build Strategies

## Docker Build
### Create the project
```
oc new-project build-strategies
```

### Create Imagestream
```
oc apply -f imagestream.yaml
```

### Buildconfig with docker strategy
```
oc apply -f buildconfig-dockerbuild.yaml
oc start-build build-bc
```

### Create the deployment, service & router
```
oc apply -f deployment.yaml
oc apply -f service.yaml
oc apply -f route.yaml
```

### Test the deployed application
```
curl "http://$(oc get routes build-rt -o jsonpath='{.spec.host}')/basicop/add?n1=100&n2=200"
```

### Cleanup the deployes application
```
oc delete -f route.yaml
oc delete -f service.yaml
oc delete -f deployment.yaml
oc delete -f buildconfig-binarybuild.yaml
oc delete -f imagestream.yaml
```

## Binary Build
### Create the project
```
oc new-project build-strategies
```

### Create Imagestream
```
oc apply -f imagestream.yaml
```

### Binary build
```
oc apply -f buildconfig-binarybuild.yaml
oc start-build build-bc --from-dir=../apps/Simple-SpringBoot-App/
```

### Create the deployment, service & router
```
oc apply -f deployment.yaml
oc apply -f service.yaml
oc apply -f route.yaml
```

### Test the deployed application
```
curl "http://$(oc get routes build-rt -o jsonpath='{.spec.host}')/basicop/add?n1=100&n2=200"
```

### Cleanup the deployes application
```
oc delete -f route.yaml
oc delete -f service.yaml
oc delete -f deployment.yaml
oc delete -f buildconfig-binarybuild.yaml
oc delete -f imagestream.yaml
```