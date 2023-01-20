# Running TKL Images in LXC Container #


Extracted from [TKL-FORUM](https://www.turnkeylinux.org/forum/support/sun-20200405-2332/how-run-turnkey-appliances-lxd-converting-proxmox-lxd-containers)

Re-edited by Marcos Méndez / popsolutions.co

## Requirements ##

Getting started with TurnKeyLinux templates as Linux Containers in LXD.

This tutorial assumes you have a working LXD instalation (version 3.23 or greater) and does not cover setup and configuration of LXD -- there are plenty of these guides.

[To Install LXD on Ubuntu](https://snapcraft.io/lxd)

[To install following original LXD Documentation](https://linuxcontainers.org/lxd/getting-started-cli/)


- Pro tip 1: If possible, use the snap version of LXD to get the most current features.

- Pro tip 2: [add your user to the lxd group](https://linuxize.com/post/how-to-add-user-to-group-in-linux/) so that you don't need to use sudo before every command (requires logging out/in to take effect)

This guide was adapted from:  
https://ubuntu.com/tutorials/create-custom-lxd-images#1-overview

https://linuxcontainers.org/lxd/getting-started-cli/

https://us.images.linuxcontainers.org/

https://help.ubuntu.com/lts/serverguide/lxd.html

https://www.turnkeylinux.org/docs/builds

------------------------
## [TKL IMAGES REPOSITORY](http://mirror.turnkeylinux.org/turnkeylinux/images/proxmox/) 

Visit and download your desired images:

For this tutorial we are gona use the original tkl-core debian 9.. But you can use whatever you want.

- debian-9-turnkey-core_15.0rc1-1_amd64.tar.gz

# Getting started

1. Create a Folder for your project and enter it
   
2. Create a metadata file
   
3. Convert the Yaml file to tar.gz

4. Import the LXD Image your are going to use

5. Launch the LXC Image

6. Configure your TKL Image setting (DB, User, Password)

----

## 1.Create YAML File

 The metadata.yaml file describes things like image creation date, name, architecture and description.
To create an LXD image, we need to provide such a file.

To create the YAML file

```
nano metadata.yaml
```

paste the following snippet in to the yaml file

```
architecture: "x86_64"
creation_date: 1584391985 # use `date +%s` command
properties:
architecture: "x86_64"
description: "Debian Stretch Turnkey Core"
os: "debian"
release: "stretch"
```


After creating the metadata file, create a tarball containing this file:

```
tar -cvzf metadata.tar.gz metadata.yaml
```
Aditionally you can remove the original YAML file

```
rm -rf metadata.yaml
```

## 2. Import LXD image
 
Next, import the two tarballs into LXD image:

```
sudo lxc image import metadata.tar.gz debian-9-turnkey-core_15.0rc1-1_amd64.tar.gz --alias tkl-core-9
```

Notice that the command should look like

lxc image import metadata.tar.gz image --alias 

## 3. Create new container image

We can now create a new container from this image:
the correct syntax should be like 

lxc launch <image/alias name/new container name>

Check the LXC Images list

```
lxc image list
```

Launch the container
```
lxc launch tkl-core-9 tkldemo
```

Check if it is listed now again

```
lxc image list
```

## 4. Configure your TKL Instance


Connect to terminal as root and set initial password:


```
lxc exec tkldemo bash
```
To set initial root password:

```
passwd
```

Leave the container

```
exit
```

## 5. Now Run inital TKL setup using ssh

Use the IP shown in the lxc list command from step 3

Use the fresh defined password from step 4


```
ssh root@ip.address
```

After completing initial setup....

Connect to webmin on port 12321

Connect to web terminal on port 12320

http://ip.address

https://ip.address


# PRO TIP

If you are using PROXMOX and need to develop applications that are courrently on production in your local computer just use as IMAGE one of the BACKUPS. Both tar.gz and tar.zstd are accepted by LXD Server.


Usefull links: 
https://forum.proxmox.com/threads/lxd-to-proxmox.68501/

Increase Storage Volume Size lxc storage set <storage_name> volume.size <new_volumeGB>
