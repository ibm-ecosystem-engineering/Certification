* ClusterIP service type is covered in the exam

## DNS Operator
The DNS operator is responsible for the following:
- Creating a default cluster DNS name (cluster.local).
- Assigning a DNS name to a namespace(for example, backend.cluster.local).
- Assigning DNS names to services that you define (for example, db.backend.cluster.local).
- Assigning DNS names to pods in a namespace (such as db001.backend.cluster.local).

Run the below command to review the configuration of DNS operator

`oc describe dns.operator/default`

DNS is deployed as DaemonSet in openshift-dns namespace

`oc get ds -n openshift-dns`

`oc get pods -n openshift-dns`

## Cluster Network Operator
OpenShift Container Platform uses the Cluster Network Operator for managing the SDN. This includes the network CIDR to use, the network mode, the network provider, and the IP address pools.
Run the following oc get command as an administrative user to consult the SDN configuration, which is managed by the Network.config.openshift.io custom resource definition.

`oc get Network cluster -o yaml`

## OpenShift Network Modes
- NetworkPolicy mode is covered in exam
- After declaring any network policy in a project, OpenShift defaults to preventing all ingress and egress traffic, unless you explicitly define a rule to allow network connections.

### Expose an application over HTTP
* Create an application

`oc new-app https://github.com/RedHatTraining/DO280-apps.git --strategy=docker --context-dir=hello-world-nginx --name=hello-world-test`

* Expose the route with the hostname 'hello-world-http.<ocp-subdomain>' 
  
  `oc expose svc/hello-world-test --hostname=hello-world-http.apps.myocp.os.fyre.ibm.com`
  
 Run `oc status` command to verify that route is pointing to right service and service is able to route to right pods
 
 * Use curl command to check if route is working
 
 `curl http:\\hello-world-http.apps.myocp.os.fyre.ibm.com`
 
 ### Expose an application over HTTPS with edge termination using default router certificate
 * Since we need to use default router certificate, so we don't need to worry about the certificate. Just expose the route with edge option and it will automatically use router certificate.
 
 `oc create route edge edge-https-hello --service=hello-world-test --hostname=hello-world-edge-https.apps.myocp.os.fyre.ibm.com`
 
 Run `oc status` command to verify that route is pointing to right service and service is able to route to right pods
 
 * Use curl command to check if route is working
 
 `curl -k https://hello-world-edge-https.apps.myocp.os.fyre.ibm.com`
 
 
 
 
  
