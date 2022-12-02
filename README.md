# 3proxy + Cloudflare WARP
Bundle 3proxy tiny proxy server + Cloudflare WARP for greater online anonymity

## Available OS
1. Ubuntu 18.04
2. Ubuntu 20.04
3. Ubuntu 22.04
4. Centos 8

## How to use
1. Install ansible [https://www.ansible.com/](https://www.ansible.com/)
2. Clone repo
```shell script
git clone https://github.com/sadovojav/3proxy-warp.git
```

### Set host credentials
Add hosts credentials to: inventory/servers.yml
**Example:**
```yaml
all:
  hosts:
    host-001:
      ansible_ssh_host: 34.109.23.122
      ansible_ssh_user: root
      ansible_ssh_password: mECfNERKFMFcriwhRb9F
```
### Generate password hash
```
openssl passwd -1 "password"
```
### Add new user account
Add new user account to the proxy_users section in to the file deploy.yaml
```
---
- hosts: all
  gather_facts: yes
  roles:
    - role: system
    - role: warp
    - role: 3proxy
      proxy_users:
        - { name: "proxyuser", hash: "$1$v.M1sfkU$8l5ghypVa4ZC.hp1TlLsQ1" }
```

### Fix error
Using a SSH password instead of a key is not possible because Host Key checking is enabled and sshpass does not support this. Please add this host's fingerprint to your known_hosts file to manage this host.

```
export ANSIBLE_HOST_KEY_CHECKING=False
```

### Run ansible
```shell script
ansible-playbook -i inventory/servers.yaml deploy.yaml
```

### Test Socks5 connection
```shell script
curl -x socks5h://proxyuser:password@34.109.23.122:1080 ipinfo.io
```

### Test HTTP connection
```shell script
curl -x proxyuser:password@34.109.23.122:3128 ipinfo.io
```