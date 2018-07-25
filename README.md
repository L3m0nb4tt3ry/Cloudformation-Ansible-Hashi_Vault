# Cloudformation-Ansible-Hashi_Vault
Create cloudformation template using ansible and Hashi vault roles

Warning: This set up is for demo purpose do not use this setup in production.
Set up

Pre-requisites


[Ansible](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html/)

[HashiVault](https://www.vaultproject.io/downloads.html/)

Set up Hashi_vault:
```
$ vault server -dev
```
Take a note of root token generated by above command. We will need this token to authenticate vault
```
$ export VAULT_ADDR=http://127.0.0.1:8200
```
Check is vault is properly working 
```
$ vault status
```
Configure vault to store aws credentials
```
$ vault secrets enable -path=aws aws

$ vault write aws/config/root     access_key=AABCDXXXXX     secret_key=abcdxxxxxx
vault write aws/roles/deploy \
    arn=arn:aws:iam::ACCOUNT-ID-WITHOUT-HYPHENS:role/RoleNameToAssume
```
Clone the repo
```
$ git clone https://github.com/L3m0nb4tt3ry/Cloudformation-Ansible-Hashi_Vault.git
$ cd Cloudformation-Ansible_Hashi_Vault
$ ansible-playbook -i inventory -K cisplaybook.yml --extra-vars "vault_token=ROOT_TOKEN"
```
Ansible will create a cloudformation stack with cis_audit lambda function.

Future Scope:
- Execute lambda function from same ansible playbook
- Run CIS_FIX script from playbook

