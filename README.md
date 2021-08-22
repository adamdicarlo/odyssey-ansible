# odyssey-ansible

Ansible configuration for home server (Odyssey X86).

## Set up a machine for Ansible

On the Ubuntu server, create a user, `admin`, and add it to the group where it can do sudo commands:

```bash
adduser admin
passwd admin
(create a password when it asks)
sudo usermod -aG sudo admin
```

Add a sudoers rule so it can do passwordless sudo commands (which Ansible needs):

```bash
echo 'admin ALL=(ALL) NOPASSWD: ALL' >> /etc/sudoers
```

On your local machine, create an SSH key and then upload it to the server:

```bash
ssh-keygen -t ed25519 -f ~/.ssh/ubuntu_ansible -C "Ansible SSH Key"
ssh-copy-id -i ~/.ssh/ubuntu_ansible admin@SERVER_HOSTNAME 
(type in the password for the admin user when it asks)
```

Test out logging into the machine with the SSH key:

```bash
ssh admin@SERVER_HOSTNAME -i ~/.ssh/ubuntu_ansible
```

## Running Ansible

Run Ansible on **server1**:

```bash
ansible-playbook -i hosts site.yml -l server1
```

Run Ansible on **server1**, showing output of what was changed:

```bash
ansible-playbook -i hosts site.yml -l server1 --diff
```

Run Ansible on **server1** in a dry-run mode to see what would be changed, without actually doing it (`--check`):

```bash
ansible-playbook -i hosts site.yml -l server1 --diff --check
```

Run Ansible with a specific set of tags (you can specify more than one in a comma separated list):

Run Ansible on **server1**:

```bash
ansible-playbook -i hosts site.yml -l server1 --tags="avahi,netatalk"
```
