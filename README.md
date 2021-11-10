# Digital Ocean Initial Server Setup

Digital Ocean provides a machine with root access and insecure setup. This ansible playbook is designed to fix that. It is based on Ubuntu 20.04 (LTS) or 21.04 image, but it should be appliable to other Ubuntu images.

## Set Up Server

Copy inventory file

```bash
cp inventory.sample inventory
```

Update information in the inventory file. Mostly like you will need to update the server IP and hostname fields. Then run main ansible playbook.

```bash
ansible-playbook main.yml
```

The main ansible playbook will both set up the machine and also secure the machine.

### Set up machine:

1. Create Users: Create "ansible" and "ubuntu" users and allow them sudo access. The idea is to have "ansible" to run ansible playbooks automatically and "ubuntu" for ad hoc manual server management.
2. Configure Machine: Set the hostname (based on inventory file) and timezone (Los Angeles Time)
3. Update machine: Simply update and upgrade all applications shipped with the OS.

### Secure machine:

1. Install firewall
2. Disable the default ssh port of 22, and set up the alternative port 2200.
3. Enable firewall to allow 2200 and deny 22.
4. Disable root account access
5. Disable password authentication.

After running the main playbook, you can no longer re-run these two playbooks because you no longer have the root account access. Instead, you need to use "ubuntu" or "ansible" users to access server using ssh key through port 2200.

## Other considerations

You may want to experiment the machine setup without the security lockdown, or vice versa. The repo provides separate playbooks for setup and security

Setup:

```bash
ansible-playbook main_setup.yml
```

Security:

```bash
ansible-playbook main_security.yml
```

That's it!