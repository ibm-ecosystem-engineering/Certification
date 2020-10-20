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



