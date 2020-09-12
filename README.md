# SETUP SFTP LINUX

## INSTALLASI
Installasi sftp server dengan CHROOT

### Debian / Ubuntu
```bash
sudo apt update && sudo apt install sshd
```
Check  status ssh 
```bash
sudo systemctl status sshd
```

### Fedora / CentOS
```bash
sudo yum install sshd
```
Check status ssh
```bash
sudo systemctl status sshd
```

## Edit config ssh 
File config sshd ada pada directory /etc/ssh/sshd_config dan tambahkan config berikut
```bash
#Subsystem sftp /usr/lib/openssh/sftp-server
Subsystem sftp internal-sftp
```
Berikan komentar ke Subsystem sftp /usr/lib/openssh/sftp-server dan tambahkan config Subsystem sftp internal-sftp seperti diatas.

Tambahkan config group sftp seperti dibawah ini.

```bash
Match Group sftponly
        ChrootDirectory %h
        ForceCommand internal-sftp
        X11Forwarding no
        AllowTcpForwarding no
```
Setelah selesai restart sshd service dengan perintah berikut.
```bash
sudo systemctl restart sshd
```

## Menambahkan user dan group sftp
```bash
groupadd sftponly
useradd user -f sftponly -s /bin/false
```

## Berikan password ke user
```bash
passwd user
```

## Buatkan directory user dan permission
```bash
mkdir -p /home/user/datadir
chown root /home/user
chmod 755 /home/user
chown user /home/user/datadir
chmod 755 /home/user/datadir
```

Untuk configurasi centos bisa menambahkan perintah berikut untuk allow selinux
```bash
setsebool -P ssh_chroot_rw_homedirs on 
```

Service sftp sudah bisa digunakan
