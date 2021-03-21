# Ubuntu

## SSH

### [How to open ssh port 22 on Ubuntu 20.04 Focal Fossa Linux](https://linuxconfig.org/how-to-open-ssh-port-22-on-ubuntu-20-04-focal-fossa-linux)

```shell
sudo ufw status enable

sudo ufw status verbose

sudo ufw allow ssh

sudo ufw allow from <IP> to any port ssh
```

### [sudo ufw status return 'inactive'](https://www.digitalocean.com/community/questions/sudo-ufw-status-return-inactive)

```shell
sudo ufw enable
sudo ufw default deny
```
