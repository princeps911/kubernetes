🛠️ Troubleshooting Log: Voting App Microservices
Project: 03-voting-app-microservices

Cluster: Minikube (Local)

1. Image Name Normalization
Issue: Deployments were failing with ErrImagePull.

Fix: Updated all manifests to use the full, official Docker Hub names (e.g., dockersamples/examplevotingapp_worker:latest) instead of truncated local names.

2. Health Probe Implementation (Liveness/Readiness)
Issue: Pods would occasionally hang without restarting.

Fix: Added HTTP-based probes for vote and result apps and TCP probes for the db. This ensures Kubernetes automatically kills and restarts "zombie" containers.

3. Anti-Affinity Scheduling Conflicts
Issue: Redis pods were stuck in Pending because of a requiredDuringScheduling rule on a single-node cluster.

Fix: Downgraded the rule to preferredDuringScheduling to allow the pod to land on the same node as others in a local environment.

4. Resource Constraints & HPA Visibility
Issue: Horizontal Pod Autoscaler (HPA) showed <unknown> CPU usage.

Fix: Defined explicit resources.requests and limits in all deployments and enabled the metrics-server addon in Minikube.

5. Kustomize NamePrefix Conflict
Issue: Using namePrefix: dev- in the overlay renamed the database service to dev-db, but the worker app was hardcoded to look for db.

Fix: Standardized service names by removing the prefix to ensure internal DNS discovery matched the application logic.

6. NodePort Port-Mapping
Issue: Services were competing for the same default ports.

Fix: Assigned unique, static NodePorts (e.g., 31000 for Vote and 31001 for Result) to allow simultaneous local access.

7. Database Initialization & Persistent Data
Issue: The database pod (db-0) was holding onto old configurations/passwords from previous failed attempts.

Fix: Performed a "Hard Reset" by deleting the Persistent Volume Claim (PVC) to force the database to re-initialize with the correct Secret values.

8. The "Hardcoded Credential" Handshake (Critical)
Issue: Worker and Result apps were stuck in a "Waiting for db" loop despite the service being "Running."

Fix: Identified that the application code expected the password postgres. Updated the K8s Secret from K8sRulez! to postgres to match the internal app requirements.

9. CoreDNS Resolution Timeout
Issue: nslookup db was timing out inside the pods, preventing the Worker from finding the database.

Fix: Restarted the coredns deployment in the kube-system namespace to refresh the cluster's internal phonebook.

10. FQDN (Fully Qualified Domain Name) Routing
Issue: Short-name DNS (db) was inconsistent.

Fix: Updated environment variables to use the full internal address: db.vote-dev.svc.cluster.local.

11. Ingress Hostname Mapping
Issue: Local browsers couldn't resolve dev.vote.local.

Fix: Configured the NGINX Ingress controller and mapped the Minikube IP to the custom hostnames in the local /etc/hosts file.
