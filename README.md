# Bitwarden RS Ansible
## Ansible deployement for bitwarden rs on raspberry pi

### Required

* Ansible 2.9.7 or update

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

Open the file named 'docker-compose.yaml' and edit the line where '<---' is
 
You have to edit for bitwarden
```
    environment:
      WEBSOCKET_ENABLED: 'true' # Required to use websockets
      SIGNUPS_ALLOWED: 'true'   # set to false to disable signups
      ADMIN_TOKEN: "DEFINE-YOUR-PERSONNAL-ADMIN-TOKEN" <---
      DOMAIN: "https://YOUR-DOMAIN-OR-SUB-DOMAIN" <---
```

You have to edit for caddy
```
    environment:
      ACME_AGREE: "true" # agree to Let's Encrypt Subscriber Agreement
      DOMAIN: "YOUR-DOMAIN-OR-SUB-DOMAIN" # CHANGE THIS! Used for Auto Let's Encrypt SSL <---
      EMAIL: "YOUR-EMAIL-FOR-KNOW-WHEN-SSL-IS-RELOAD"  # CHANGE THIS! Optional, provided to Let's Encrypt <---
```

### Deployment playbook to your raspberry pi

``` bash
ansible-playbook -i host playbook.yaml
```