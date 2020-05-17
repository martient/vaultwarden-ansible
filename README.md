# Bitwarden RS Ansible
## Ansible deployement for bitwarden rs on raspberry pi

### Required

* Ansible 2.9.7 or update
* Domain or sub-domain acces (DNS)
* PORTS 80 & 443 not used

### Before the playbook execution

#### SSH key

For ansible you need to give to your raspberry pi your ssh key, the easy way is 

``` bash
ssh-copy-id pi@RASP_LOCAL_IP
```

after that, you can quit your rasp and try to connect to if he doesn't request a password, this part is ready

#### host

Open the file named 'host' and edit the line where '<---' is

```
all:
    vars:
        ansible_python_interpreter: /usr/bin/python3
    hosts:
        node1:
            ansible_host: RASP_LOCAL_IP <---
            ansible_user: pi
    children:
        base:
            hosts:
                node1
        bitwarden:
            hosts:
                node1
```

#### docker-compose

Open the file named '.env' and edit the line where '<---' is
 
You have to edit for bitwarden
```
#Bitewardemrs
WEBSOCKET_ENABLED=true
SIGNUPS_ALLOWED=true
ADMIN_TOKEN=DEFINE-YOUR-PERSONNAL-ADMIN-TOKEN <---
DOMAIN_HTTPS=https://YOUR-DOMAIN-OR-SUB-DOMAIN <---

#caddy
ACME_AGREE=true
DOMAIN=YOUR-DOMAIN-OR-SUB-DOMAIN <---
EMAIL=YOUR-EMAIL-FOR-KNOW-WHEN-SSL-IS-RELOAD <---
```

### Deployment playbook to your raspberry pi

``` bash
ansible-playbook -i host playbook.yaml
```