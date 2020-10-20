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
* Expose the deployment on port 80

`oc expose deploy/hello-world --name=hello-world --port=80 --target-port=8080 --type=ClusterIP`
* Expose the route with the hostname 'hello-world-http.<ocp-subdomain>' 
  
  `oc expose svc/hello-world --hostname=hello-world-http.apps.myocp.os.fyre.ibm.com`
  
 Run `oc status` command to verify that route is pointing to right service and service is able to route to right pods
 
 
  
