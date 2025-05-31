# Otomi for Infrastructure as Code (IaC) 🚀

## 📖 Introduction: Otomi as IaC
[Otomi](https://otomi.io/) is an open-source **self-hosted Kubernetes platform** that acts as a **pre-configured, production-ready platform for Kubernetes**, offering security, governance, monitoring, and automation out of the box. It packages best-of-breed open-source components into a single **GitOps-driven platform**, making it an ideal tool for **Infrastructure as Code (IaC)** practices.

This guide focuses on using **Otomi** for **IaC**, highlighting cluster setup, GitOps configuration management, and best practices.

---

## 🌐 Key Concepts in Otomi IaC
🔸 **Kubernetes-native**: Otomi runs on a Kubernetes cluster (EKS, AKS, GKE, on-prem).  
🔸 **GitOps**: Declarative configuration of the platform via Git.  
🔸 **Pre-packaged Components**: Istio, Prometheus, Loki, Grafana, Harbor, Keycloak, and more.  
🔸 **Otomi Console**: Central UI to manage workloads, security, and policies.  
🔸 **Otomi Chart**: Helm chart that bootstraps the platform.  
🔸 **Cluster and Platform Separation**: Otomi manages the platform; you bring the cluster.

---

## 🏗️ Infrastructure as Code with Otomi

### 🔸 Pre-requisites
1️⃣ Existing Kubernetes cluster (e.g., EKS, AKS, GKE, Minikube, etc.)  
2️⃣ Helm and kubectl installed.  
3️⃣ Git repository for **Otomi values** and **cluster configuration**.

---

### 🔸 Install Otomi Platform

1️⃣ **Clone Otomi Git repository**:
```bash
git clone https://github.com/otomi-io/otomi-core.git
cd otomi-core
```

2️⃣ **Initialize Otomi Configuration**:
```bash
export KUBECONFIG=/path/to/your/kubeconfig
./bin/otomi bootstrap
```

3️⃣ **Configure Otomi via Git (Infrastructure as Code)**:
- Edit `values/otomi.yaml` to customize your platform (e.g., domain, authentication, certificates, platform settings).
- Commit changes to Git:
```bash
git add values/otomi.yaml
git commit -m "Initial Otomi configuration"
git push
```

4️⃣ **Apply Otomi Configuration (GitOps)**:
```bash
./bin/otomi apply
```

---

### 🔸 Manage Configuration as Code
- All configurations (cluster-wide policies, workloads, security) are stored as **YAML files** in Git.
- Use **Git branching and pull requests** to manage changes (e.g., platform updates, service configuration).
- Example directory structure:
```
otomi-values/
├── values/
│   └── otomi.yaml
├── envs/
│   └── dev/
│       └── otomi.yaml
├── charts/
└── secrets/
```

---

### 🔸 Otomi GitOps Pipeline
Use tools like **ArgoCD**, **Flux**, or **Otomi CLI** for GitOps workflows.
- Example with ArgoCD:
    - Watch the Otomi Git repository.
    - Automatically deploy changes to the Kubernetes cluster.
    - Ensure configuration drift is corrected automatically.

---

### 🔸 Security, Compliance & Monitoring
- Built-in with Otomi:  
  ✅ **Istio Service Mesh** for secure service-to-service communication.  
  ✅ **OPA Gatekeeper** for policies.  
  ✅ **Prometheus/Grafana** for observability.  
  ✅ **Loki** for logs.  
  ✅ **Keycloak** for identity management.  
  ✅ **Harbor** for container image security and scanning.

---

### 🔸 Comparison with Other IaC Tools

| Feature            | Otomi                                      | Terraform          | Ansible             | SaltStack           | Chef                |
|--------------------|---------------------------------------------|--------------------|---------------------|---------------------|---------------------|
| Language           | YAML + GitOps                               | HCL                | YAML                | YAML                | Ruby DSL            |
| Provisioning       | Bring your cluster (EKS, GKE, AKS)          | Core IaC           | Modules             | Salt Cloud          | Chef Provisioning   |
| Config Mgmt        | Strong (K8s-native)                         | Weak               | Strong              | Strong              | Strong              |
| Compliance         | Built-in (OPA, Keycloak, Istio)             | External           | Limited             | External            | External            |
| Orchestration      | GitOps (ArgoCD, Flux)                       | External           | Limited             | Orchestrate         | Push/Pull           |
| GitOps Support     | Core Feature                                | Strong             | Strong              | Strong              | Strong              |

---

## 🛡️ Best Practices for IaC with Otomi

✅ **Version control** all `values.yaml` files in a Git repository.  
✅ Use **branching strategies** for staging, testing, and production.  
✅ **Bootstrap Otomi in dev/staging clusters first** before production.  
✅ Regularly **sync Git with Otomi** to maintain compliance.  
✅ Use **ArgoCD or Flux** for continuous delivery of configuration.  
✅ Integrate with **Terraform** or **Pulumi** to provision clusters, and manage cluster state.  
✅ Review Otomi logs and compliance dashboards regularly.

---

## 📚 Resources
- [Otomi Documentation](https://otomi.io/docs/)
- [Otomi GitOps Model](https://otomi.io/docs/otomi-core/gitops/)
- [Otomi Helm Chart](https://github.com/otomi-io/otomi-core)
- [ArgoCD Integration](https://argo-cd.readthedocs.io/)
- [Kubernetes GitOps Patterns](https://www.gitops.tech/)

---

*This guide focuses on using **Otomi for Infrastructure as Code (IaC)**, covering GitOps-driven configuration management, security, compliance, and best practices. Happy automating with Otomi! 🎛️*
