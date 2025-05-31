# Ansible for Infrastructure as Code (IaC) 🚀

## 📖 Introduction: Ansible Beyond Configuration
Ansible is not just for configuration management—it can also provision **infrastructure** (compute, network, storage) in cloud and on-premise environments. Using **cloud modules** and **dynamic inventories**, Ansible can manage your entire infrastructure stack as code.

This guide focuses on **infrastructure provisioning (IaC)** with Ansible.

---

## 🌐 Key Concepts for IaC with Ansible
🔸 **Inventory**: Can be static (ini/yaml) or dynamic (via plugins for AWS, Azure, GCP, etc.)  
🔸 **Modules**: Specialized modules to provision infrastructure (e.g., `amazon.aws.ec2`, `azure.azcollection.azure_rm`, `google.cloud.gcp_compute_instance`)  
🔸 **Playbooks**: Define desired infrastructure states (e.g., VMs, networks, storage).  
🔸 **Idempotency**: Ensures infrastructure is created/updated, not duplicated.  
🔸 **Dynamic Inventory**: Automatically discovers existing cloud resources.

---

## ☁️ Cloud Infrastructure Provisioning with Ansible

### 🔸 AWS Example: Provision EC2 + VPC
#### Inventory (Dynamic AWS)
```ini
plugin: aws_ec2
regions:
  - us-east-1
```

#### Playbook (`provision_aws_infra.yml`)
```yaml
- name: Provision AWS Infrastructure
  hosts: localhost
  connection: local
  gather_facts: no
  collections:
    - amazon.aws

  vars:
    region: us-east-1
    vpc_cidr: 10.0.0.0/16
    subnet_cidr: 10.0.1.0/24
    key_name: my-key
    instance_type: t2.micro
    ami_id: ami-0abcdef1234567890

  tasks:
    - name: Create VPC
      amazon.aws.ec2_vpc_net:
        name: my_vpc
        cidr_block: "{{ vpc_cidr }}"
        region: "{{ region }}"
        tags:
          Environment: Dev
      register: vpc

    - name: Create Subnet
      amazon.aws.ec2_vpc_subnet:
        vpc_id: "{{ vpc.vpc.id }}"
        cidr: "{{ subnet_cidr }}"
        az: "{{ region }}a"
        map_public: yes
        tags:
          Name: my_subnet
      register: subnet

    - name: Launch EC2 Instance
      amazon.aws.ec2_instance:
        name: my_vm
        key_name: "{{ key_name }}"
        instance_type: "{{ instance_type }}"
        image_id: "{{ ami_id }}"
        vpc_subnet_id: "{{ subnet.subnet.id }}"
        wait: yes
        tags:
          Environment: Dev
      register: ec2

    - debug:
        var: ec2
```

---

### 🔸 Azure Example: Provision VM + Network
```yaml
- name: Provision Azure VM and Network
  hosts: localhost
  connection: local
  gather_facts: no
  collections:
    - azure.azcollection

  vars:
    resource_group: myRG
    location: eastus
    vm_size: Standard_B1s
    admin_username: azureuser
    admin_password: ComplexPassword123!

  tasks:
    - name: Create Resource Group
      azure.azcollection.azure_rm_resourcegroup:
        name: "{{ resource_group }}"
        location: "{{ location }}"

    - name: Create Virtual Network
      azure.azcollection.azure_rm_virtualnetwork:
        name: myVNet
        resource_group: "{{ resource_group }}"
        address_prefixes: "10.1.0.0/16"

    - name: Create Subnet
      azure.azcollection.azure_rm_subnet:
        name: mySubnet
        resource_group: "{{ resource_group }}"
        virtual_network: myVNet
        address_prefix: "10.1.0.0/24"

    - name: Create Public IP
      azure.azcollection.azure_rm_publicipaddress:
        name: myPublicIP
        resource_group: "{{ resource_group }}"
        allocation_method: Dynamic

    - name: Create Network Interface
      azure.azcollection.azure_rm_networkinterface:
        name: myNIC
        resource_group: "{{ resource_group }}"
        subnet_name: mySubnet
        virtual_network_name: myVNet
        public_ip_name: myPublicIP

    - name: Create Virtual Machine
      azure.azcollection.azure_rm_virtualmachine:
        resource_group: "{{ resource_group }}"
        name: myVM
        admin_username: "{{ admin_username }}"
        admin_password: "{{ admin_password }}"
        vm_size: "{{ vm_size }}"
        network_interfaces: myNIC
        image:
          offer: UbuntuServer
          publisher: Canonical
          sku: 18.04-LTS
          version: latest
```

---

## ⚡ Key Ansible Modules for IaC
| Cloud Provider | Modules                                |
|----------------|----------------------------------------|
| AWS            | `ec2_vpc_net`, `ec2_vpc_subnet`, `ec2_instance`, `ec2_security_group`, `cloudformation` |
| Azure          | `azure_rm_resourcegroup`, `azure_rm_virtualmachine`, `azure_rm_networkinterface` |
| GCP            | `gcp_compute_instance`, `gcp_compute_network`, `gcp_compute_subnetwork` |
| OpenStack      | `os_server`, `os_network`, `os_subnet` |

---

## 🔄 Ansible vs Terraform for IaC

| Feature             | Ansible                                | Terraform                       |
|---------------------|-----------------------------------------|---------------------------------|
| Language            | YAML                                   | HCL (HashiCorp)                 |
| State Management    | Stateless                              | Maintains state files           |
| Execution Mode      | Imperative (procedural)                | Declarative (desired state)     |
| Cloud Integrations  | Extensive modules for many providers   | Deep provider integrations      |
| Idempotency         | Ensures desired state                  | Tracks resource state precisely |
| Use Case            | Config + IaC + App deployment          | Primarily IaC (infra only)      |

---

## 📦 Best Practices for IaC with Ansible

✅ Use **collections** for cloud modules (e.g., `amazon.aws`, `azure.azcollection`).  
✅ Use **dynamic inventory** for cloud environments.  
✅ Separate playbooks for **infra provisioning** and **app configuration**.  
✅ Version control playbooks with Git.  
✅ Secure sensitive data with `ansible-vault`.  
✅ Use `ansible-lint` and Molecule for playbook validation and testing.  
✅ Consider combining with **Terraform** for complex IaC and Ansible for configuration.

---

## 📚 Resources
- [Ansible Collections for Cloud](https://docs.ansible.com/ansible/latest/collections/index.html)
- [Ansible AWS Guide](https://docs.ansible.com/ansible/latest/scenario_guides/guide_aws.html)
- [Ansible Azure Guide](https://docs.ansible.com/ansible/latest/scenario_guides/guide_azure.html)
- [Ansible GCP Guide](https://docs.ansible.com/ansible/latest/scenario_guides/guide_gce.html)
- [Dynamic Inventory Plugins](https://docs.ansible.com/ansible/latest/plugins/inventory.html)

---

*This updated guide now covers **Infrastructure provisioning (IaC)** using Ansible across cloud providers. Happy automating your infra! 🌐⚙️*
