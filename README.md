# One HTML Page Ansible Deployment

This repository contains an Ansible-based automation setup for provisioning and configuring AWS EC2 instances to serve selected HTML files via Apache 

---

## **1. Prerequisites**

- **Ansible** installed on your system  
- **AWS account** with access keys  
- SSH key pair for EC2 access (created during provisioning if not present)  

---

## **2. Vault Password File**

This project uses **Ansible Vault** to store sensitive AWS credentials.  
To set it up:

```bash
echo "ansible_challenge" > ~/vault_password.txt
chmod 600 ~/vault_password.txt
```

**Note:**  
- The password `ansible_challenge` is the vault password for this challenge.  
- Your AWS credentials will be stored in an encrypted file.  
- Update them with your own values before running playbooks.

---

## **3. Updating AWS Credentials**

Edit the vaulted file:

```bash
ansible-vault edit group_vars/all/aws_creds.yml --vault-password-file ~/vault_password.txt
```

Inside, set your own:

```yaml
aws_access_key_id: YOUR_ACCESS_KEY
aws_secret_access_key: YOUR_SECRET_KEY
```

---

## **4. Installing Collections**

Install required Ansible collections:

```bash
ansible-galaxy collection install -r collections/requirements.yml
```

---

## **5. Linting Playbooks**

To check for best practices and common errors:

```bash
ansible-lint site.yml
```

---

## **6. Running the Playbook**

Run the main playbook to provision EC2 instances and configure them:

```bash
ansible-playbook -i inventory/ec2_inventory.ini site.yml
```

---

## **7. Notes**

- Make sure your AWS credentials are valid before running the playbook.  
- If provisioning fails due to key pair or instance issues, clean up resources manually in AWS before retrying.  
- The `ec2_inventory.ini` file should contain the current EC2 instance details (auto-generated if using dynamic inventory).  
