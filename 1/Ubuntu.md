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

## Proxy

### [How to Set the Proxy for APT on Ubuntu 18.04](https://www.serverlab.ca/tutorials/linux/administration-linux/how-to-set-the-proxy-for-apt-for-ubuntu-18-04/)

```shell
vim /etc/apt/apt.conf.d/proxy.conf

Acquire::http::Proxy "http://172.16.8.1:6152"
Acquire::https::Proxy "http://172.16.8.1:6152"
Acquire::socks5::proxy "socks://172.16.8.1:6153"
```
