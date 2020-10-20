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
 
 
 ### Expose an application over HTTPS with edge termination using self-signed certificates
 * Create a self-signed certificate using below openssl command. Give the common name 'hello-world-edge2-https.apps.myocp.os.fyre.ibm.com'
 
 `openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout tls.key -out tls.crt`
 
 * Create an edge route for the service 'hello-world-test' with the hostname 'hello-world-edge2-https.apps.myocp.os.fyre.ibm.com'
 
 `oc create route edge edge2-https-hello --service=hello-world-test --hostname=hello-world-edge2-https.apps.myocp.os.fyre.ibm.com --key tls.key --cert tls.crt`
 
 Run `oc status` command to verify that route is pointing to right service and service is able to route to right pods
 
 * Use curl command to verify the generated key/cert has been used for TLS. You may verify it by using browser.
 
 `curl -kv https://hello-world-edge2-https.apps.myocp.os.fyre.ibm.com`
 
 `curl --cacert tls.crt https://hello-world-edge2-https.apps.myocp.os.fyre.ibm.com`
 
 ### Expose an application over HTTPS with edge termination using certificate singed by openshift router-ca
 * Extract router-ca key and certificate.
 
 `oc extract secrets/router-ca --keys tls.key -n openshift-ingress-operator`
 
 `oc extract secrets/router-ca --keys tls.crt -n openshift-ingress-operator`
 * Now generate a CSR for the hostname hello-world-edge3-https.apps.myocp.os.fyre.ibm.com. Generate a key for the application
 Generate the key:
 
 `openssl genrsa -out app.key 2048`
 
 Generate the CSR:
 `openssl req -new -subj "/CN=hello-world-edge3-https.apps.myocp.os.fyre.ibm.com" -key app.key -out app.csr`
 
 Sign the CSR with router-ca CA:
 
 `openssl x509 -req -in app.csr -CA tls.crt -CAkey tls.key -CAcreateserial -out app.crt -days 365 -sha256`
 
 You can verify a x509 certificate using below command:
 
 `openssl x509 -in app.crt -text -noout`
 
 Now use the signed app.key and app.crt to generate an edge secure route
 
 `oc create route edge edge3-https-hello --service=hello-world-test --hostname=hello-world-edge3-https.apps.myocp.os.fyre.ibm.com --key app.key --cert app.crt`
 
 
 ### Expose an application over HTTPS with passthrough termination using certificate singed by a CA
 * In this question, if it provides the CA key and cert, then you can sign your generated application key/cert with the provided CA key/cert. If it asks to use router-ca, then you can use steps mentioned above to pull router ca and key and use them to sign your generated application key/cert. If it asks to use self-signed certificate, you can use the step provided above to generate a self-signed key pair and use that.
 
 * After the application certificate and key are created, create a tls secret using the key/cert in the appropriate namespace
 * Mount the secret onto deployment/dc at the mentioned mountPath
 * Finally create a passthrough route to TLS port of the service; for example if service exposes port 8443 over TLS, then:
 
 `oc create route passthrough passthrough-https --service=hello-world-test --port=8443 --hostname=hello-world-passthrough-httpsapps.myocp.os.fyre.ibm.com`
 
  
