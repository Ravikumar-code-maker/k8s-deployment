# k8s-deployment
1️⃣ RollingUpdate (Default Strategy)

Definition:
The RollingUpdate strategy updates pods gradually, replacing old pods with new ones without downtime. This ensures your application stays available during updates. You can control the rollout using parameters like:

maxUnavailable → maximum number of old pods that can be unavailable during update
maxSurge → maximum number of extra pods that can be created temporarily

Use Case: Most applications like web servers or APIs where uptime is critical.

Example YAML:

spec:
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxUnavailable: 1
      maxSurge: 1

Real-Time Flow:
Pods are updated one by one or in small batches. If one pod fails, Kubernetes stops the rollout, preventing downtime.

2️⃣ Recreate

Definition:
The Recreate strategy deletes all existing pods first and then creates new pods with the updated version. This means downtime occurs while the old pods are terminated and the new ones start.

Use Case: Applications that cannot run multiple versions simultaneously, like some database-backed apps or legacy systems.

Example YAML:

spec:
  strategy:
    type: Recreate

Real-Time Flow:
All old pods are terminated → new pods are created → application is back online after rollout completes.

3️⃣ Blue-Green Deployment (Conceptual)

Definition:
Blue-Green deployment involves running two separate deployments: one for the current (Blue) version and one for the new (Green) version. Traffic is switched from Blue to Green once the new version is verified.

Use Case: Zero-downtime production deployments where rollback is easy—just switch traffic back to the old deployment.

Implementation: Kubernetes doesn’t provide a native Blue-Green strategy, but you implement it using two Deployments + Service switching.

Real-Time Flow:
Blue version running → Green version deployed alongside → test Green → switch Service → old version stays idle for rollback.

4️⃣ Canary Deployment (Conceptual)

Definition:
Canary deployment updates only a small subset of users or pods with the new version first. Once the new version is validated, rollout continues to the rest of the users.

Use Case: Testing new features or performance improvements safely in production before full rollout.

Implementation: Kubernetes achieves this using labels, multiple Deployments, and Services to route traffic gradually.

Real-Time Flow:
10% of users see new version → monitor metrics → gradually increase traffic → full rollout when stable.

✅ Summary Table
Strategy	Downtime	Gradual Update	Use Case
RollingUpdate	No	Yes	Most apps, web/API
Recreate	Yes	No	Apps that cannot run multiple versions
Blue-Green	No	Yes	Zero-downtime, production-ready
Canary	No	Partial	Testing new features safely

If you want, I can make real-time YAML examples for Blue-Green and Canary deployments too, so you can actually deploy them in GKE for practice.
