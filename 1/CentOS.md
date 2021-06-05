# CentOS

环境： Cent OS 7 on Vmware Fusion.

## 20200605

### Keyboard & Mouse

设置 CentOS 7 的键盘映射，从标准的 `Profile - Default` 复制一份命名为 `Profile - CentOS7`, 然后将 `Key Mappings` -> `Virtual Machine Shortcut` 的部分全部带上 `Shift`, 例如 `Control - C` 变更为 `Shift - Control - C`.

### 设置静态 IP

1. Vmware Fusion 外面设置连接方式为 Bridged Networking -> Wi-Fi.
2. /etc/sysconfig/network-scripts/ifcfg-ens33 从 BOOTPROTO=dhcp 变更为:
    ```text
    BOOTPROTO=static
    IPADDR=192.168.50.200
    NETMASK=255.255.255.0
    GATEWAY=192.168.50.1
    DNS1=192.168.50.1
    ```
3. /etc/sysconfig/network 设定 DNS:
    ```text
    DNS1=192.168.50.1
    ```
4. 运行 service network restart

参考：

* [Centos 7 学习之静态IP设置(续)](https://blog.csdn.net/johnnycode/article/details/50184073)
* [Questions about CentOS-7](https://wiki.centos.org/FAQ/CentOS7#head-a21a9e454157700367c9b7e9ccb1ff9954bec881)

### Tips

* su - 可以切换到 root 账号，效果同 sudo su
* [What's the difference between `chmod a+x` and `chmod +x`?](https://unix.stackexchange.com/a/639441)
* [Why does chmod +w not give write permission to other(o)](https://unix.stackexchange.com/a/429424)
