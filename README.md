# ansible role windows update


## Requirements

None

## Functionalities

- Check for windows updates
- Install update
- Reboot

## Dependencies

None

## Example Playbook

```
- name: Install / check Security updates
  hosts:
    - all
  roles:
    - role: ./roles/ansible-windows-update
  gather_facts: no
```

## Example hosts file

```
[myHosts]
host1
host2
host3

[all:vars]
# use `$ kinit admin@domain.lan` to get a kerberos ticket
ansible_user=admin@DOMAIN.LAN
#ansible_password=password
ansible_connection=winrm
ansible_port=5985
ansible_winrm_transport=kerberos
ansible_winrm_server_cert_validation=ignore
```

## Run

Lancer un premier playbook avec le tag "check" pour vérifier si un reboot est nécessaire suite à une précédante installation.
Les tags "check, reboot" permettront de réaliser le reboot si requis.
Le tag "install", permettra d'installer les patchs.
En option les tags "install, reboot" lancera un reboot après installation des patchs si nécessaire.

```
# check available updates
$ ansible-playbook -i hosts --limit 'lot1' playbook.yml --tags check
# reboot if check indicate reboot is required
$ ansible-playbook -i hosts --limit 'lot1' playbook.yml --tags check,reboot
# install without reboot
$ ansible-playbook -i hosts --limit 'lot1' playbook.yml --tags install
# install with reboot
$ ansible-playbook -i hosts --limit 'lot1' playbook.yml --tags install,reboot
```

Cette dernière commande sera probablement utilisée plusieurs fois pour mener à bien l'application de tous les patchs sur une même machine.
