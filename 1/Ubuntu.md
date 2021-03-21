# Ubuntu

## Docker

### [How to Configure the Linux Firewall for Docker Swarm on Ubuntu 16.04](https://www.digitalocean.com/community/tutorials/how-to-configure-the-linux-firewall-for-docker-swarm-on-ubuntu-16-04)

```shell
sudo ufw allow 22/tcp
sudo ufw allow 2376/tcp
sudo ufw allow 2377/tcp
sudo ufw allow 7946/tcp
sudo ufw allow 7946/udp
sudo ufw allow 4789/udp

sudo ufw reload

sudo ufw enable

sudo systemctl restart docker
```

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
