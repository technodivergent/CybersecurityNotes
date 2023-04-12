```
# samba/smb (port 139/445)  
nmap -p 139,445 --script=smb-enum-shares.nse,smb-enum-users.nse [ip]  
smbclient //ip/share  
smbget -R smb://ip/share  
  
# nfs (port 111)  
nmap -p 111 --script=nfs-ls,nfs-statfs,nfs-showmount [ip]
```