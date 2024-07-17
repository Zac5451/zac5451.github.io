在 Linux 系统上，两块硬盘驱动器的路径通常在 `/dev` 目录下表示为 `/dev/sda`, `/dev/sdb` 等等，这取决于驱动器连接到系统的顺序。你可以使用 `lsblk` 或 `fdisk -l` 命令来查看硬盘驱动器的详细信息。

假设你有两块硬盘驱动器 `/dev/sda` 和 `/dev/sdb`，我们可以将它们挂载到特定目录并通过 Samba 共享。以下是详细步骤：

### 1. 挂载硬盘驱动器
首先，你需要创建挂载点并挂载硬盘驱动器。

创建挂载点：

```bash
sudo mkdir -p /mnt/disk1
sudo mkdir -p /mnt/disk2
```

挂载硬盘驱动器：

```bash
sudo mount /dev/sda1 /mnt/disk1
sudo mount /dev/sdb1 /mnt/disk2
```

### 2. 配置自动挂载（可选）
为了在系统重启后自动挂载硬盘驱动器，你需要编辑 `/etc/fstab` 文件。

首先，获取硬盘驱动器的 UUID：

```bash
sudo blkid
```

编辑 `/etc/fstab` 文件并添加以下行：

```bash
UUID=<UUID_of_sda1> /mnt/disk1 ext4 defaults 0 2
UUID=<UUID_of_sdb1> /mnt/disk2 ext4 defaults 0 2
```

将 `<UUID_of_sda1>` 和 `<UUID_of_sdb1>` 替换为实际的 UUID。

### 3. 配置 Samba 共享
编辑 Samba 配置文件 `/etc/samba/smb.conf`：

```bash
sudo nano /etc/samba/smb.conf
```

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

### 4. 设置目录权限
确保共享目录具有适当的权限：

```bash
sudo chown -R nobody:nogroup /mnt/disk1
sudo chown -R nobody:nogroup /mnt/disk2
sudo chmod -R 0775 /mnt/disk1
sudo chmod -R 0775 /mnt/disk2
```

### 5. 启动和重启 Samba 服务
重启 Samba 服务以使配置生效：

```bash
sudo systemctl restart smbd.service
sudo systemctl restart nmbd.service
```

### 6. 检查 Samba 服务状态
确保 Samba 服务正在运行：

```bash
sudo systemctl status smbd.service
sudo systemctl status nmbd.service
```

### 7. 设置防火墙规则
确保防火墙允许 Samba 流量：

对于 `ufw`：

```bash
sudo ufw allow samba
```

对于 `firewalld`：

```bash
sudo firewall-cmd --permanent --add-service=samba
sudo firewall-cmd --reload
```

现在，你的两块硬盘驱动器已经通过 Samba 共享。可以通过网络上的其他设备访问这些共享，例如在 Windows 文件资源管理器中输入 `\\your_server_ip\disk1` 和 `\\your_server_ip\disk2`。