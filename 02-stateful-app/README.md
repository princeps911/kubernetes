# 02 - Stateful Application & Persistence
The goal of this project was to move from "disposable" containers to an application that retains data across restarts.

### 🎯 Objectives
- Implement **StatefulSets** instead of Deployments for the database.
- Use **Persistent Volume Claims (PVC)** to request storage from the cluster.
- Test data persistence by deleting a pod and verifying the data remains when the pod recreates.

### 🛠️ Key Takeaways
- Understanding the stable network identity of StatefulSets (e.g., `db-0`, `db-1`).
- Learning how Kubernetes manages storage lifecycle independently from pod lifecycle.
