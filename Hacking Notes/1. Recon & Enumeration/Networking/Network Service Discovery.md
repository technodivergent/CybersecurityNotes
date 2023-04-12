# NFS
Port 2049/tcp
## Enumeration
Standard command: `showmount -e [ip]`
### nmap
```bash
nmap --script=nfs-ls # List NFS exports & permissions
nmap --script=nfs-showmount # Shows showmount under each NFS service
nmap --script=nfs-statfs # Disk stats and share info
```
### metasploit
```
# Module for scanning NFS mounts and list permissions
scanner/nfs/nfsmount
```

### Mounting
```
mkdir /mnt/nfs_mount
sudo mount -t nfs [-o vers=2] <ip>:/remote /mnt/nfs_mount -o nolock
```

## Permissions
Create a user/group with same UID/GID to impersonate & gain access to NFS share
```
sudo usermod -u 1001 kass
sudo groupmod -g 1001 kass
```