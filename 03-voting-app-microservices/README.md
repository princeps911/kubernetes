# 03 - Multi-Tier Voting App (Production-Grade)
This is a complex, 5-service microservices application (Vote, Result, Worker, Redis, Postgres) managed with Kustomize.

### 🎯 Objectives
- **Environment Management:** Use Kustomize overlays (`dev`/`prod`) to handle different configurations.
- **Self-Healing:** Implement Liveness and Readiness probes for automated recovery.
- **Auto-Scaling:** Configure Horizontal Pod Autoscaler (HPA) to scale based on CPU metrics.
- **Traffic Routing:** Use NGINX Ingress to route external traffic via custom hostnames.

### 🛡️ Real-World Troubleshooting (The "11-Point Fix")
This project involved a significant debugging phase where I identified and resolved:
1. **DNS Resolution:** Fixed `nslookup` timeouts by managing CoreDNS and FQDNs.
2. **Auth Mismatch:** Resolved "Waiting for DB" loops caused by hardcoded credential mismatches in the Worker (.NET) and Result (Node.js) code.
3. **Kustomize Prefixes:** Managed the `namePrefix` conflict that was breaking internal service discovery.
4. **Metrics Visibility:** Enabled the Minikube metrics-server to fix `<unknown>` HPA status.

### 🚀 Commands
```powershell
# Deploy the Dev environment
kubectl apply -k overlays/dev/

# Verify health
kubectl get hpa -n vote-dev
kubectl logs -l app=worker -n vote-dev
