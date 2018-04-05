# Install SAMBA Service - NAS Drive

Install SAMBA Service - NAS Drive

### Prerequisites

Server must have 2 or more HDD

NB: You can use system disk but you will need to skip the part of mount the HDD.

## Getting Started

### Install ntfs-3g

```
sudo apt-get install ntfs-3g
```

### Mount the HDD that we need to share

You can use `fdisk -l` to locat target hdd, my disk is `sda` and the partition is `sda1`.

By using any text editor, add these following lines to fstab file:
NB: Create a empty folder anywhere you want, for example I will use this path `/media/NASDrive`

```
sudo mkdir /media/NASDrive
```

Open and edit this file: `/etc/fstab`

```
/dev/sda1 /mnt/windows ntfs-3g defaults 0 0
/dev/sda1 /media/NASDrive auto nofail,uid=enter_uid_here,gid=enter_gid_here,noatime 0 0
```

### Install SAMBA

```
apt-get install samba samba-common-bin
```

### Create Samba users

First you need to create a new Samba user, this user well have a specific permissions.

```
useradd [new_username] -m -G users
passwd [password of new user]

smbpasswd -a [username]
```

use this folliwing commands to get UID and GID of new user:

```
id -u [username]
id -g [username]
```

### Samba configuration

Change directory to `/etc/samba`, then open `smb.conf`  and make these changes on it:

- Under the “`Authentication`” header, remove the hash (`#`) before `security = user`

- Under the “`Share Definitions`” header, change `read only = yes` to `read only = no`

Append these following lines in the last lines of `smb.conf`:

```
[Shared folder name]
comment = This is NAS drive
path = /media/NASDrive
valid users = @users
force group = users
create mask = 0660
directory mask = 0771
read only = no
```

#### For any informations: medjamal.ouazani@gmail.com