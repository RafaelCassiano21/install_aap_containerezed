# Playbook for Installing Ansible Automation Platform (AAP) on Containers

This repository provides a playbook to assist with the installation of Ansible Automation Platform in versions **2.4** or **2.5** using RHEL 9.

---

## **Minimum Hardware Requirements**
- **Operating System**: RHEL 9
- **RAM**: 8 GB
- **CPU**: 4 vCPU
- **Disk**: 40 GB

---

## **Steps for Manual Installation**

1. **Access the Red Hat Portal:**
   - Navigate to [Subscriptions > Subscription Allocations](https://access.redhat.com/management/subscription_allocations).
   - Generate the license (manifest) and download the `.zip` file.

2. **Create a User on RHEL 9:**
   - Create a user named `ansible` without privileges.
   - All installations will be performed using this user.

3. **Download AAP:**
   - Access [the official Red Hat website](https://access.redhat.com/downloads).
   - Choose the desired version (2.4 or 2.5) for RHEL 9.
   - Download the installer "**Ansible Automation Platform Containerized Setup Bundle**".

4. **Move all Files for /opt:**
   - Save the installer and the manifest `.zip` file in the `/opt` directory on RHEL 9.

5. **Clone Repository:**
   - Clone the files from this repository into the `/opt` directory:
     ```bash
     git clone <repository-url>
     ```
   - Make sure to include the files: `install_aap.yml`, `inventory`, `inventory_template.j2`, and `secrets.pass`.

6. **Edit the Password File:**
   - Modify only the `secrets.pass` file with your information.
   - Modify the variable name `version_install` define your version 2.4 or 2.5

7. **Install Ansible Core:**
   - Run the following command:
     ```bash
     sudo dnf install -y ansible-core
     ```

8. **Encrypt the Password File:**
   - Encrypt the `secrets.pass` file using Ansible Vault:
     ```bash
     ansible-vault encrypt secrets.pass
     ```
   - Set a password for the vault.

9. **Start the Setup:**
   - In the `/opt` directory, execute:
     ```bash
     ansible-playbook -i inventory install_aap.yml --ask-vault-pass
     ```
   - Wait for the process to finish.

10. **Start AAP Installation:**
    - Navigate to the created version directory:
      ```bash
      cd <folder-name>
      ```
    - Execute the following command:
      ```bash
      ansible-playbook -i inventory ansible.containerized_installer.install
      ```
    - Wait for the process to finish.

---

## **Accessing AAP**

Use `http` or `https` protocols with the following ports:

- **8080 / 8443**: Automation Controller
- **8081 / 8444**: Automation Hub
- **8082 / 8445**: Event-Driven Ansible Controller

---

## **FINALY : Clear All /opt:**
```bash
sudo rm -rf /opt/*
```

## **OPTIONAL : Command to Remove Installation:**

Run the following command to remove the installation:
```bash
ansible-playbook -i inventory ansible.containerized_installer.uninstall
```

---

Documentation

For more information, consult the Official Documentation.
  ```bash
https://docs.redhat.com/en/documentation/red_hat_ansible_automation_platform/2.4/html/containerized_ansible_automation_platform_installation_guide/aap-containerized-installation#adding-execution-nodes_aap-containerized-installation
```

