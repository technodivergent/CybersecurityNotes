# Via SMB
```bash
mkdir share
python3.9 /opt/impacket/examples/smbserver.py -smb2support -username <victim_user> -password <victim_pass> public share
```

