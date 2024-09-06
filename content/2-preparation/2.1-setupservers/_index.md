---
title: "Server Setup for the Project"
date:  "`r Sys.Date()`" 
weight: 1 
chapter: false
pre: " <b> 2.1 </b> "
---

#### Install Ubuntu Server

First, we will use the Ubuntu version 24.04 operating system to install on a virtual machine.

- [Link to Ubuntu Server 24.04 ISO file](https://ubuntu.com/download/server/thank-you?version=24.04&architecture=amd64&lts=true)

We will create a virtual machine using **VMware Workstation**, to install the application you can refer to the following link:

- [Set up VMware Workstation Pro 17 on Windows 10/11](https://thelinuxforum.com/articles/960-how-to-install-free-vmware-workstation-pro-17-on-windows-10-11)

Proceed to create the virtual machine on **VMware Workstation** by following these steps:

![alt text](/images/2-preparation/2.1-setupservers/2-1-1.png)
![alt text](/images/2-preparation/2.1-setupservers/2-1-2.png)
![alt text](/images/2-preparation/2.1-setupservers/2-1-3.png)

Select the Ubuntu Server ISO file we just downloaded.

![alt text](/images/2-preparation/2.1-setupservers/2-1-4.png)

Name the virtual machine and choose a folder that you manage.

![alt text](/images/2-preparation/2.1-setupservers/2-1-5.png)

Depending on your machine's configuration, you can select the appropriate **Processors** and **Cores**, but GitLab requires at least **2 CPU (Processor Cores)**.

![alt text](/images/2-preparation/2.1-setupservers/2-1-6.png)
![alt text](/images/2-preparation/2.1-setupservers/2-1-7.png)
![alt text](/images/2-preparation/2.1-setupservers/2-1-8.png)

Next, choose the default configurations.

![alt text](/images/2-preparation/2.1-setupservers/2-1-9.png)
![alt text](/images/2-preparation/2.1-setupservers/2-1-10.png)

After the virtual machine is installed successfully, we will boot the virtual machine and configure the Ubuntu Server.

![alt text](/images/2-preparation/2.1-setupservers/2-1-11.png)

Continue to choose the default configurations.

In the **Storage Configuration** section, configure the **Root ( / )** partition with the remaining available space.

![alt text](/images/2-preparation/2.1-setupservers/2-1-12.png)
![alt text](/images/2-preparation/2.1-setupservers/2-1-13.png)

Tick the option to install OpenSSH.

![alt text](/images/2-preparation/2.1-setupservers/2-1-14.png)

Wait for Ubuntu to be successfully set up and proceed with Reboot.

![alt text](/images/2-preparation/2.1-setupservers/2-1-15.png)

After the reboot, log in to the server with the username and password you just created. **Note the IPv4 upon login, used for SSH into the server**.
![alt text](/images/2-preparation/2.1-setupservers/2-1-16.png)

{{% notice tip %}}
You can use the `ip a` command to view IPv4 and other server information.
{{% /notice %}}

Open **CMD** on Windows to SSH into the server using the IPv4 and Username.

![alt text](/images/2-preparation/2.1-setupservers/2-1-17.png)

However, when the server is turned off and back on, the IPv4 may change, so we need to set up a static IP for the server. When rebooting, the IP will remain unchanged, making it easier for us to configure.

Switch to Root privileges using the `sudo -i` command for easier configuration.

Edit the configuration in the `50-cloud-init.yaml` file with the command `vi /etc/netplan/50-cloud-init.yaml` and write the following configuration:
``` yaml
# This is the network config written by 'subiquity'
network:
  ethernets:
    ens33:
      dhcp4: no
      addresses: [192.168.181.100/24]
      gateway4: 192.168.181.2
      nameservers:
              addresses: [8.8.8.8, 8.8.4.4]
  version: 2
```

Save the file and run the following commands:

```shell
$ netplan apply

$ reboot
```

{{% notice info %}}
The **gateway4** line is the NAT Gateway IP. We can get it in VMware Workstation by accessing **Edit** -> **Virtual Network Editor** -> **NAT Setting**.
{{% /notice %}}

![alt text](/images/2-preparation/2.1-setupservers/2-1-18.png)

After successfully configuring the static IP, we will proceed to create a Snapshot and Clone to other servers.

![alt text](/images/2-preparation/2.1-setupservers/2-1-19.png)

Right-click on **Gitlab Server** -> **Snapshot** -> **Take Snapshot** and fill in the information.

![alt text](/images/2-preparation/2.1-setupservers/2-1-21.png)

![alt text](/images/2-preparation/2.1-setupservers/2-1-22.png)

Thus, we have successfully created a Snapshot, after that, we will proceed to clone the server.

Access Snapshot Manager and perform the following steps:

![alt text](/images/2-preparation/2.1-setupservers/2-1-23.png)

![alt text](/images/2-preparation/2.1-setupservers/2-1-20.png)

After that, we will configure the servers according to the following table:

| Server             | Hostname           | IP Tĩnh         |
|--------------------|--------------------|-----------------|
| Gitlab Server      | gitlab-server      | 192.168.181.101 |
| Development Server | development-server | 192.168.181.102 |
| Build Server       | build-server       | 192.168.181.103 |
