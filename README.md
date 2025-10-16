###part 1 - Git & SSH


###Difference b/w git pull and git fetch

-----------------------------------
git fetch  ---> Only downloads changes from the remote repository tp local 
                repository. It updates your remote tracking branches but does                 not modify your working files

we use command "git fetch origin"


git pull  ---> The git pull basically is  "git fetch + git merge".
               It not only downloads the changes but also automatically merge
               them into current branch.

We use command "git pull origin main"



####** How to Reslove a Merge Conflict (Example Scenario) **


1. Suppose branch 'feature-A' added 'conflict.txt' with "Line from feature-A"    and 'feature-B' added the same file with different content.
2. When merging 'feature-B' into 'main' (which already has 'feature-A), Git     will mark 'conflict.txt' with conflict markers '<<<<<', '=======', '>>>>>'.
3. To resolve:
   - Open the file, choose or combine the changes.
   - git add conflict.txt
   - git commit -m "resolve merge conflict"

Part 2 — Docker & Containerization

3) Concepts (explain in README)

Dockerfile — a text file with instructions to build an image.

Docker image — a read-only template built from a Dockerfile (can be versioned/tagged).

Docker container — a running instance of an image (the runtime).

4) Reduce image size tips

Use smaller base images (e.g., python:3.11-slim, alpine where appropriate).

Use multi-stage builds for compiled languages.

Combine RUN statements to reduce layers.



Part 3 — Kubernetes (EKS Basics)
1) Explain Pod vs Deployment vs Service (README)

Pod: smallest deployable unit in Kubernetes; a pod can contain one or more containers that share network and storage.

Deployment: manages stateless pods by defining desired state and handling replica sets, rollouts, rollbacks.

Service: stable network endpoint to expose pods. Types: ClusterIP, NodePort, LoadBalancer.

2) Why EKS (managed Kubernetes)?

Managed control plane (master nodes) maintenance, patching, high availability, and integrations with AWS services (IAM, CloudWatch).

Reduces operational overhead and improves security/updates vs DIY on raw VMs.
Remove caches and unnecessary files; use --no-cache-dir for pip.


Part 4 — CI/CD Pipeline
2) How pipeline would change for Kubernetes deploy

High-level changes:

After image build & tests, push image to a registry (DockerHub/Amazon ECR).

Add a deployment step: either use kubectl with kubeconfig stored as secrets or use a deployment action to update the Kubernetes Deployment image (e.g., kubectl set image ... or kubectl apply -f k8s-deployment.yaml).

Add rollout status check: kubectl rollout status deployment/flask-demo.

Ensure secrets and service account with least privileges to access the cluster.

Part 5 — Monitoring & Logs
1) Metrics vs Logs vs Traces (README)

Metrics: numeric measurements over time (CPU, memory, request rates). Time-series friendly and aggregated.

Logs: raw event records — text lines with variable structure describing what happened.

Traces: distributed traces show end-to-end request flow across services (timings across services & spans).

3) Suggested monitoring tools for AWS EKS

CloudWatch — built-in with AWS, integrates with EKS for cluster logs/metrics, centralized and simple for AWS users.

Prometheus + Grafana — industry standard for metrics collection (Prometheus) and dashboards (Grafana); flexible, powerful queries and alerting (Alertmanager).

ELK/EFK (Elasticsearch + Fluentd/Fluent Bit + Kibana) — good for centralized log search & visualization.

AWS Distro for OpenTelemetry / OpenTelemetry — standardized traces/metrics collection.

Recommendation: Prometheus + Grafana for metrics + Fluent Bit -> CloudWatch/Elasticsearch for logs + OpenTelemetry for traces. Explain tradeoffs in README (cost vs flexibility).

Part 6 — Problem-Solving Scenario (high-level design)

Requirement: GitHub code → containerize → auto-deploy on merge to main → logs visible.

High-level steps:

Code repo (GitHub) with Dockerfile, k8s YAML, and GitHub Actions workflow.

CI (build): GitHub Actions triggered on PR and push to main.

Build Docker image and run unit tests.

Tag image (e.g., registry/myapp:${{github.sha}}) and push to registry (ECR/DockerHub).

CD (deploy):

After successful push, update k8s Deployment image — either kubectl set image or update manifests and kubectl apply.

Use cluster credentials stored securely (GitHub Secrets / OIDC with IRSA for EKS).

Kubernetes infra:

EKS cluster with node groups (or Fargate), IAM roles, and namespaces.

Logging & Monitoring:

Use Fluent Bit/Fluentd to forward pod logs to CloudWatch/ELK.

Prometheus + Grafana for metrics; set up Alertmanager for alerts.

Observability & Security:

Configure liveness/readiness probes.

Use RBAC for least privilege & Image scanning for vulnerabilities.

Rollback strategy:

Use Kubernetes Deployment rollouts and kubectl rollout undo on failures; keep image tags immutable (use sha).

Deliverables for assignment:

README with high-level architecture diagram (can be ASCII or image).

All files: Dockerfile, k8s YAML, GitHub Actions workflow, and screenshots of local runs/builds.


