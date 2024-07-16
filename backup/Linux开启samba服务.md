要在 Linux 系统上开启 SMB（Samba）服务，你需要安装并配置 Samba。以下是详细步骤：

1. 安装 Samba
首先，你需要确保 Samba 已安装。如果没有安装，可以使用以下命令进行安装。

对于基于 Debian 的系统（如 Ubuntu）：

sudo apt update
sudo apt install samba

对于基于 Red Hat 的系统（如 CentOS、Fedora）：

sudo yum install samba
# 或者对于 Fedora
sudo dnf install samba


2. 配置 Samba
安装完成后，需要配置 Samba 以共享目录。编辑 Samba 的配置文件 `/etc/samba/smb.conf`。

sudo nano /etc/samba/smb.conf


在文件的末尾添加以下内容来配置一个共享目录（例如共享 `/srv/samba/share` 目录）：

```ini
[share]
   path = /srv/samba/share
   browseable = yes
   read only = no
   guest ok = yes
```

以上配置创建了一个名为 `share` 的共享，它位于 `/srv/samba/share`，允许浏览和写入。

3. 创建共享目录并设置权限
创建共享目录并设置适当的权限：

sudo mkdir -p /srv/samba/share
sudo chown nobody:nogroup /srv/samba/share
sudo chmod 0775 /srv/samba/share


4. 启用并启动 Samba 服务
启用并启动 Samba 服务：

sudo systemctl enable smb.service
sudo systemctl start smb.service

sudo systemctl enable nmb.service
sudo systemctl start nmb.service


5. 验证 Samba 服务状态
检查 Samba 服务是否正在运行：

sudo systemctl status smb.service
sudo systemctl status nmb.service


6. 设置防火墙规则
确保防火墙允许 Samba 流量。如果你使用 `ufw`，可以使用以下命令：

sudo ufw allow samba

对于 `firewalld`，使用以下命令：

sudo firewall-cmd --permanent --add-service=samba
sudo firewall-cmd --reload


7. 创建 Samba 用户（可选）
如果你希望通过用户名和密码控制访问，可以创建 Samba 用户。首先，添加系统用户：


sudo useradd -M -d /srv/samba/share smbuser

然后，设置 Samba 密码：


sudo smbpasswd -a smbuser

确保目录权限允许 `smbuser` 访问：


sudo chown smbuser:smbuser /srv/samba/share

现在，SMB 服务已经启动并配置完毕。你可以通过 Windows 或其他操作系统上的文件管理器访问共享。例如，在 Windows 中，可以在文件资源管理器的地址栏中输入 `\\your_server_ip\share` 来访问共享目录。


在 Linux 系统上，两块硬盘驱动器的路径通常在 `/dev` 目录下表示为 `/dev/sda`, `/dev/sdb` 等等，这取决于驱动器连接到系统的顺序。你可以使用 `lsblk` 或 `fdisk -l` 命令来查看硬盘驱动器的详细信息。

假设你有两块硬盘驱动器 `/dev/sda` 和 `/dev/sdb`，我们可以将它们挂载到特定目录并通过 Samba 共享。以下是详细步骤：


挂载硬盘
1. 挂载硬盘驱动器
首先，你需要创建挂载点并挂载硬盘驱动器。

创建挂载点：

sudo mkdir -p /mnt/disk1
sudo mkdir -p /mnt/disk2

挂载硬盘驱动器：

sudo mount /dev/sda1 /mnt/disk1
sudo mount /dev/sdb1 /mnt/disk2


2. 配置自动挂载（可选）
为了在系统重启后自动挂载硬盘驱动器，你需要编辑 `/etc/fstab` 文件。

首先，获取硬盘驱动器的 UUID：

sudo blkid

编辑 `/etc/fstab` 文件并添加以下行：

UUID=<UUID_of_sda1> /mnt/disk1 ext4 defaults 0 2
UUID=<UUID_of_sdb1> /mnt/disk2 ext4 defaults 0 2


将 `<UUID_of_sda1>` 和 `<UUID_of_sdb1>` 替换为实际的 UUID。

3. 配置 Samba 共享
编辑 Samba 配置文件 `/etc/samba/smb.conf`：

sudo nano /etc/samba/smb.conf


在文件末尾添加以下内容来配置共享目录：

```ini
[disk1]
   path = /mnt/disk1
   browseable = yes
   read only = no
   guest ok = yes

[disk2]
   path = /mnt/disk2
   browseable = yes
   read only = no
   guest ok = yes
```

4. 设置目录权限
确保共享目录具有适当的权限：

sudo chown -R nobody:nogroup /mnt/disk1
sudo chown -R nobody:nogroup /mnt/disk2
sudo chmod -R 0775 /mnt/disk1
sudo chmod -R 0775 /mnt/disk2


5. 启动和重启 Samba 服务
重启 Samba 服务以使配置生效：

sudo systemctl restart smbd.service
sudo systemctl restart nmbd.service


6. 检查 Samba 服务状态
确保 Samba 服务正在运行：

sudo systemctl status smbd.service
sudo systemctl status nmbd.service


7. 设置防火墙规则
确保防火墙允许 Samba 流量：

对于 `ufw`：


sudo ufw allow samba

对于 `firewalld`：


sudo firewall-cmd --permanent --add-service=samba
sudo firewall-cmd --reload

现在，你的两块硬盘驱动器已经通过 Samba 共享。可以通过网络上的其他设备访问这些共享，例如在 Windows 文件资源管理器中输入 `\\your_server_ip\disk1` 和 `\\your_server_ip\disk2`。


zac@zac-HP:~$ sudo blkid
/dev/nvme0n1p2: UUID="da5e11dc-0b8b-4015-8d34-f9ebdc1c1db8" BLOCK_SIZE="4096" TYPE="ext4" PARTUUID="b4bc6fdd-bc1b-4e81-bff8-163ac6f29ba4"
/dev/loop1: BLOCK_SIZE="131072" TYPE="squashfs"
/dev/nvme0n1p1: UUID="8B68-7952" BLOCK_SIZE="512" TYPE="vfat" PARTUUID="f28a847d-7a68-42c4-b314-9d612304d898"
/dev/loop8: BLOCK_SIZE="131072" TYPE="squashfs"
/dev/sdb2: LABEL="2T" BLOCK_SIZE="512" UUID="34C0849DC08466C8" TYPE="ntfs" PARTLABEL="Basic data partition" PARTUUID="1ef71de1-f172-4d6c-b02f-0702e879aed9"
/dev/sdb1: PARTLABEL="Microsoft reserved partition" PARTUUID="31641f69-a5fe-4ef5-830d-958eee4e2249"
/dev/loop6: BLOCK_SIZE="131072" TYPE="squashfs"
/dev/loop4: BLOCK_SIZE="131072" TYPE="squashfs"
/dev/loop11: BLOCK_SIZE="131072" TYPE="squashfs"
/dev/loop2: BLOCK_SIZE="131072" TYPE="squashfs"
/dev/loop0: BLOCK_SIZE="131072" TYPE="squashfs"
/dev/loop9: BLOCK_SIZE="131072" TYPE="squashfs"
/dev/loop7: BLOCK_SIZE="131072" TYPE="squashfs"
/dev/sda1: LABEL="500G" BLOCK_SIZE="512" UUID="B69491199490DCE5" TYPE="ntfs" PARTUUID="d11994a0-01"
/dev/loop5: BLOCK_SIZE="131072" TYPE="squashfs"
/dev/loop12: BLOCK_SIZE="131072" TYPE="squashfs"
/dev/loop3: BLOCK_SIZE="131072" TYPE="squashfs"
/dev/loop10: BLOCK_SIZE="131072" TYPE="squashfs"
