# Rudder for Infrastructure as Code (IaC) 🚀

## 📖 Introduction: Rudder as IaC
Rudder is an **open-source continuous configuration and audit solution**, combining **configuration management** with **compliance**. It offers a declarative approach to managing infrastructure, which aligns perfectly with **Infrastructure as Code (IaC)** principles.

This guide focuses on using Rudder for **IaC**, highlighting provisioning, configuration management, and compliance.

---

## 🌐 Key Concepts in Rudder IaC
🔸 **Nodes**: Managed systems (like minions/agents).  
🔸 **Policies**: Declarative configurations for target systems.  
🔸 **Directives**: Building blocks of policies (like tasks or states).  
🔸 **Techniques**: Reusable templates of actions.  
🔸 **Compliance**: Automatic tracking and reporting of policy application.  
🔸 **Inventory**: Automatic node inventory (OS, hardware, etc.).  
🔸 **Rudder Agent**: Installed on nodes for applying configurations.  
🔸 **API/CLI**: Manage Rudder programmatically.

---

## 🏗️ Infrastructure as Code with Rudder

### 🔸 Rudder Server and Node Setup
1️⃣ Install Rudder Server:
```bash
curl https://download.rudder.io/apt/rudder-6.x-install.sh | sudo bash
sudo apt-get install rudder-server-root
```

2️⃣ Install Rudder Agent on Nodes:
```bash
curl https://download.rudder.io/apt/rudder-6.x-install.sh | sudo bash
sudo apt-get install rudder-agent
rudder agent inventory
rudder agent run
```

---

### 🔸 Define Configuration as Code (Policies)

📄 Create a **Technique**:
- Go to Rudder Web UI → Techniques → Create a new Technique
- Example: **Install Apache**
    - Install package `apache2`
    - Ensure service `apache2` is running and enabled
    - Deploy custom `/etc/apache2/apache2.conf` file

📄 **Policy**: Combine Directives based on this Technique
- Assign Policy to nodes with role `webserver`

### 🔸 Version Control of Policies (Git)
- Export policies and techniques to **Git** for version control:
```bash
rudder-cli export techniques --dir /path/to/git-repo/techniques
rudder-cli export directives --dir /path/to/git-repo/directives
rudder-cli export policies --dir /path/to/git-repo/policies
git add .
git commit -m "Update Rudder IaC policies"
git push
```

---

### 🔸 Compliance and Reporting
Rudder automatically reports compliance of each node:
- **Compliant**: Node matches policy.
- **Non-compliant**: Drift detected.
- **Reports**: Exportable for audits.

---

### 🔸 Rudder API and CLI for IaC Automation
📄 Use Rudder CLI to manage nodes, policies, techniques:
```bash
rudder-cli nodes list
rudder-cli policies apply
rudder-cli techniques import /path/to/techniques
```

📄 Rudder REST API for automated workflows (e.g., GitOps):
```bash
curl -X GET -H "X-API-Token: your-token" https://rudder.example.com/rudder/api/latest/nodes
```

---

## 🔄 Rudder vs Terraform vs Ansible vs Salt vs Chef for IaC

| Feature           | Rudder                              | Terraform           | Ansible             | SaltStack            | Chef                 |
|-------------------|-------------------------------------|---------------------|---------------------|-----------------------|----------------------|
| Language          | Built-in Techniques & Directives    | HCL                 | YAML                | YAML                 | Ruby DSL             |
| Provisioning      | Limited (focus on config)           | Core IaC            | Modules             | Salt Cloud            | Chef Provisioning    |
| Config Mgmt       | Strong                              | Weak                | Strong              | Strong               | Strong               |
| Compliance        | Built-in Compliance Tracking        | External tools      | Limited             | External tools       | External tools       |
| Orchestration     | Limited (focus on config)           | External tools      | Limited             | Orchestrate          | Push/Pull + Habitat  |
| GitOps Support    | Via CLI/API                         | Strong              | Strong              | Strong               | Strong               |

---

## 🛡️ Best Practices for IaC with Rudder

✅ Treat **Techniques and Directives** as code: store them in **Git**.  
✅ Use **API or CLI** for automated IaC workflows (e.g., Jenkins, GitLab CI).  
✅ Define clear **node groups and roles** for targeted policies.  
✅ Regularly review and **audit compliance reports**.  
✅ Use **Rudder Inventory** to track node states.  
✅ Combine with **Terraform** or **Pulumi** for infra provisioning, using Rudder for post-provision configuration.

---

## 📚 Resources
- [Rudder Documentation](https://docs.rudder.io/)
- [Rudder Techniques Guide](https://docs.rudder.io/reference/techniques.html)
- [Rudder API Reference](https://docs.rudder.io/api/)
- [Rudder CLI Usage](https://docs.rudder.io/reference/cli.html)
- [Compliance and Reporting](https://docs.rudder.io/reference/compliance.html)

---

*This guide focuses on using **Rudder for Infrastructure as Code (IaC)**, covering configuration management, compliance, and best practices. Happy automating with Rudder! 🎛️*
