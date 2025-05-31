# Chef for Infrastructure as Code (IaC) 🚀

## 📖 Introduction: Chef as IaC
Chef is a powerful automation platform that transforms infrastructure into code. Traditionally used for **configuration management**, Chef also supports **infrastructure provisioning** through **Chef Provisioning (formerly Chef Metal)** and **Chef Habitat**.

This guide focuses on using Chef for **Infrastructure as Code (IaC)**, covering infrastructure provisioning and configuration workflows.

---

## 🌐 Key Concepts for IaC with Chef
🔸 **Chef Infra (Client & Server)**: Applies cookbooks and policies for node configuration.  
🔸 **Cookbooks & Recipes**: Define resources (e.g., packages, files, services) and their states.  
🔸 **Chef Provisioning**: Extends Chef to provision infrastructure (compute, networking, storage).  
🔸 **Chef Habitat**: Manages application packaging and deployment as code.  
🔸 **Chef Workstation**: Local setup for authoring and running cookbooks and provisioning tasks.

---

## ☁️ Infrastructure Provisioning with Chef

### 🔸 Chef Provisioning Example: AWS EC2 + VPC
Chef Provisioning can provision infrastructure resources (e.g., AWS EC2 instances) using **drivers** (like `fog`, `aws`).  
📄 `provision_aws.rb`
```ruby
# Load AWS driver
with_driver 'aws::us-east-1'

# Define a VPC
aws_vpc 'my_vpc' do
  cidr_block '10.0.0.0/16'
end

# Define a Subnet
aws_subnet 'my_subnet' do
  vpc 'my_vpc'
  cidr_block '10.0.1.0/24'
  availability_zone 'us-east-1a'
end

# Create Security Group
aws_security_group 'my_sg' do
  vpc 'my_vpc'
  inbound_rules [
    { port: 22, protocol: :tcp, sources: ['0.0.0.0/0'] }
  ]
end

# Launch EC2 instance
machine 'my_vm' do
  machine_options bootstrap_options: {
    subnet_id: 'my_subnet',
    security_group_ids: ['my_sg'],
    instance_type: 't2.micro',
    image_id: 'ami-0abcdef1234567890'
  }
  action :converge
end
```

Run with:
```bash
chef-client -z provision_aws.rb
```

---

### 🔸 Chef for VM and Configuration Management
- Provision infrastructure (VMs, network) using **Chef Provisioning**.
- Apply **cookbooks** to configure software and services.
- Example:
```ruby
# Define VM with provisioning
machine 'app_server' do
  machine_options bootstrap_options: { instance_type: 't2.medium', image_id: 'ami-xyz' }
  recipe 'my_app::default'  # Apply my_app cookbook
end
```

---

### 🔸 Chef Habitat for App and Infra as Code
Chef Habitat packages applications and their dependencies for deployment across environments.  
Key Concepts:
- **Plan.sh**: Defines how to build and run an application.
- **Habitat Builder**: Builds and promotes packages.
- **Exports**: Deploy applications to Kubernetes, Docker, VMs.

---

## ⚡ Key Chef Tools for IaC
| Component             | Purpose                                |
|-----------------------|----------------------------------------|
| Chef Infra Server     | Central policy and state management    |
| Chef Workstation      | Author and test cookbooks, provision infra |
| Chef Provisioning     | Provision infrastructure as code       |
| Chef Habitat          | Application automation and packaging   |
| Chef InSpec           | Infrastructure testing and compliance  |

---

## 🔄 Chef vs Terraform vs Ansible for IaC

| Feature               | Chef                                   | Terraform                       | Ansible                        |
|-----------------------|----------------------------------------|---------------------------------|--------------------------------|
| Language              | Ruby DSL                               | HCL (HashiCorp)                 | YAML                          |
| State Management      | Chef Server (policy) or local          | Maintains state files           | Stateless                     |
| Execution Mode        | Imperative/Declarative hybrid          | Declarative                     | Imperative                    |
| Infra Provisioning    | Chef Provisioning (less popular)       | Core IaC tool                   | Supported via modules         |
| App Config Mgmt       | Strong (Chef Infra, Cookbooks)         | Limited (external tools)        | Strong                        |
| Use Case              | Infra + App + Compliance               | Infra only                      | Infra + App + Config          |

---

## 📦 Best Practices for IaC with Chef

✅ Use **Chef Workstation** for local development and testing.  
✅ Store **cookbooks and provisioning scripts** in version control (Git).  
✅ Leverage **Chef InSpec** for infrastructure compliance and testing.  
✅ Use **Policyfiles** to define node configurations consistently.  
✅ Secure sensitive data with **Chef Vault**.  
✅ Consider **Chef Habitat** for application packaging and delivery.  
✅ For large-scale IaC, combine with **Terraform** for infra, and Chef for configuration.

---

## 📚 Resources
- [Chef Infra Documentation](https://docs.chef.io/infra/)
- [Chef Provisioning Docs](https://docs.chef.io/provisioning/)
- [Chef Habitat](https://www.habitat.sh/docs/)
- [Chef InSpec](https://www.chef.io/products/chef-inspec)
- [Chef Policyfiles](https://docs.chef.io/policyfile/)

---

*This guide focuses on using **Chef for Infrastructure as Code (IaC)**, including provisioning, configuration, and app deployment workflows. Happy automating with Chef! 🍳*
