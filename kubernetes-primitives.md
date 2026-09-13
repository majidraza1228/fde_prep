# Kubernetes Primitives — FDE Interview Prep

Core K8s concepts that come up in platform engineering interview questions.

---

## Deployment

**One-liner:** Manages stateless pods — declares how many replicas to run and handles rolling updates with zero downtime.

**How it works:** A Deployment creates a ReplicaSet under the hood. When you update the image, it creates a new ReplicaSet and gradually shifts traffic (rolling update). `maxSurge` and `maxUnavailable` control how aggressive that rollout is.

**Interview answer:** "A Deployment is for stateless workloads. I define the desired replica count and pod template, and Kubernetes handles keeping that many healthy replicas running. Rolling updates are built-in — Kubernetes spins up new pods before killing old ones so there's no downtime."

---

## StatefulSet

**One-liner:** Like a Deployment but for stateful workloads — pods get stable network identities and persistent storage that survive restarts.

**How it works:** Each pod gets a predictable name (`pod-0`, `pod-1`), its own PersistentVolumeClaim, and pods start/stop in order. This matters for databases, Kafka, Zookeeper — anything where pod identity matters.

**Interview answer:** "StatefulSet is for workloads where state matters — databases, message queues. Unlike a Deployment, each pod has a stable hostname and its own volume that sticks around even if the pod is deleted and recreated."

**Deployment vs StatefulSet:** Deployment for your Flask API or inference service. StatefulSet for Postgres, Redis, vector DB.

---

## ConfigMap

**One-liner:** Stores non-secret configuration (env vars, config files) as key-value pairs that pods consume at runtime.

**How it works:** Injected either as environment variables or mounted as a file in the container. Changing a ConfigMap doesn't automatically restart pods — you have to trigger that separately (or use a tool like Reloader).

**Interview answer:** "ConfigMap externalizes config from the container image. Instead of baking API endpoints or feature flags into the image, I put them in a ConfigMap and mount it — so I can change config without rebuilding."

**ConfigMap vs Secret:** Secrets are base64-encoded (not encrypted by default) and intended for passwords/tokens. In production you'd back Secrets with Vault or AWS Secrets Manager, not store them raw in etcd.

---

## Helm Chart

**One-liner:** A package manager for Kubernetes — bundles all your YAML manifests into a versioned, templatable unit you can deploy with one command.

**How it works:** Charts use Go templating so you can parameterize values (image tag, replica count, resource limits) via `values.yaml`. `helm upgrade --install` is idempotent. `helm rollback` reverts a release.

**Interview answer:** "Helm is how teams actually ship K8s apps in production. Instead of maintaining 10 raw YAML files per environment, you write one chart and override values per environment — dev gets 1 replica, prod gets 10. It also gives you rollback out of the box."

---

## Common Follow-up: `kubectl apply` walkthrough

*"Walk me through what happens when you run `kubectl apply -f deployment.yaml`."*

The YAML hits the API server → gets validated and stored in etcd → the controller manager's Deployment controller sees the desired state doesn't match actual state → creates a ReplicaSet → the scheduler assigns pods to nodes → kubelet on each node pulls the image and starts the container.

---

## OOMKilled Debugging (common scenario)

*"A customer says their AI inference pod keeps getting OOMKilled. What do you check?"*

1. `kubectl describe pod <name>` — confirms OOMKilled exit code and shows the memory limit
2. Check if the model loads into memory all at once at startup (common with large models)
3. Fix paths: increase memory limit, switch to model sharding, or use a quantized model variant
4. Check if multiple replicas are co-scheduled on the same node — that's a resource request misconfiguration

---

## Quick Reference

| Object | Use for | Key property |
|--------|---------|--------------|
| Deployment | Stateless apps, APIs, inference services | Rolling updates, replica count |
| StatefulSet | Databases, queues, anything with identity | Stable hostnames, per-pod PVCs |
| ConfigMap | Non-secret config, env vars, config files | Decouples config from image |
| Secret | Passwords, tokens, API keys | Base64-encoded, back with Vault in prod |
| Helm Chart | Packaging + deploying full apps | Templated YAML, versioned releases |
