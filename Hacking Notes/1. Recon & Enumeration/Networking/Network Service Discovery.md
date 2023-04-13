# FTP
Data port: 20
Control port: 21

Active: Client connects via control port, opens a data port for server to connect to.
Passive: Server opens a different port, client is instructed to connect on that. Allows outbound connection from behind firewall.

FTP Commands
```
status
debug
trace
ls
```

Connect over telnet or openssl
```
telnet [ip] 21
nc -nv [ip] 21
openssl s_client -connect [ip]:21 -starttls ftp
```
# SMB
Older NetBIOS SMB service connects over TCP 137-139, but CIFS uses TCP 445 only.

Each host participates in a workgroup, an arbitrary collection of computers and resources on SMB network. NetBIOS Name Server is older technology so each host on the network has a name. Mostly replaced by WINS.

nmap SMB scripts don't provide much info. Use `rpcclient` instead.

## rpcclient
```
$ rpcclient -U "" [ip]
> srvinfo                 # server info
> enumdomains             # enum all deployed domains
> querydomaininfo         # provide info of deployed domains
> netshareenumall         # enum all shares
> netsharegetinfo [share] # gets info for share
> enumdomusers            # enums all domain users
> queryuser [RID]         # provides info about specified user
```

### Brute forcing user RIDs
```bash
for i in $(seq 500 1100);do rpcclient -N -U "" 10.129.14.128 -c "queryuser 0x$(printf '%x\n' $i)" | grep "User Name\|user_rid\|group_rid" && echo "";done
```

## Additional SMB enumeration tools
[Impacket's samrdump](https://github.com/SecureAuthCorp/impacket/blob/master/examples/samrdump.py)
[smbmap](https://github.com/ShawnDEvans/smbmap)
[CrackMapExec](https://github.com/byt3bl33d3r/CrackMapExec)
[enum4linux-ng](https://github.com/cddmp/enum4linux-ng)
# NFS
Port 111/tcp and 2049/tcp
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

## Dangerous Settings
| Option         | Description                                                                                               |
| -------------- | ---------------------------------------------------------------------------------------------- |
| rw             | read/write permissions                                                                         |
| insecure       | ports above 1024 are used                                                                      |
| nohide         | if another fs was mounted below an exported dir, this dir is exported by its own exports entry |
| no_root_squash | all files created by root are kept with UID/GID as 0                                           |

# DNS
[Most Popular Types of DNS Attacks)](https://securitytrails.com/blog/most-popular-types-dns-attacks)
[DNSenum](https://github.com/fwaeytens/dnsenum)
```bash
dnsenum --dnsserver 10.129.14.128 --enum -p 0 -s 0 -o subdomains.txt -f /opt/useful/SecLists/Discovery/DNS/subdomains-top1million-110000.txt inlanefreight.htb
```

## Dangerous settings
| Option          | Description                                                    |
| --------------- | -------------------------------------------------------------- |
| allow-query     | Defines which hosts can send requests to DNS server            |
| allow-recursion | Defines which hosts can send recursive requests to DNS server  |
| allow-transfer  | Defines which hosts can receive zone transfers from DNS server | 
| zone-statistics | Collects statistical data of zones                             |

# SMTP
MUA -> MSA -> MTA -> MDA -> POP3/IMAP

Mail User Agent converts to header/body.
Mail Submission Agent checks validity/origin of e-mail ("Relay Server")
Mail Transfer Agent sends/receives e-mail
Mail Delivery Agent transfers to recipient's mailbox

DKIM and SPF help secure against mail spoofing

| Command      | Description                                                                                      |
| ------------ | ------------------------------------------------------------------------------------------------ |
| `AUTH PLAIN` | AUTH is a service extension used to authenticate the client.                                     |
| `HELO`       | The client logs in with its computer name and thus starts the session.                           |
| `MAIL FROM`  | The client names the email sender.                                                               |
| `RCPT TO`    | The client names the email recipient.                                                            |
| `DATA`       | The client initiates the transmission of the email.                                              |
| `RSET`       | The client aborts the initiated transmission but keeps the connection between client and server. |
| `VRFY`       | The client checks if a mailbox is available for message transfer.                                |
| `EXPN`       | The client also checks if a mailbox is available for messaging with this command.                |
| `NOOP`       | The client requests a response from the server to prevent disconnection due to time-out.         |
| `QUIT`       | The client terminates the session.                                                               |

# IMAP / POP3

# SNMP
SNMP can be useful to locate info about routers & other network devices

## MIB
Management Information Base is an independent format for storing device info. Text file. Contains at least 1 Object Identifier (OID) and unique address & name, type, access rights and description of object. Written in Abstract Syntax Notation One (ASN.1). Do not contain data, but explain where to find information.

## OID
Represents a node in hierarchical namespace. 

Examination of process parameters might reveal creds passed on command line
`snmpwalk -v 2c -c private [ip]`

`onesixtyone` can be used to brute force community string names using dict of common community strings.
```shell-session
onesixtyone -c /opt/useful/SecLists/Discovery/SNMP/snmp.txt 10.129.14.128
```

Once a community string is known, we can use it with `braa` to brute-force the OIDs and enumerate info behind them.
```shell-session
braa [string]@[ip]:.1.3.6.*
```

## Dangerous Settings
| Settings                                         | Description                                                                           |
| ------------------------------------------------ | ------------------------------------------------------------------------------------- |
| `rwuser noauth`                                  | Provides access to the full OID tree without authentication.                          |
| `rwcommunity <community string> <IPv4 address>`  | Provides access to the full OID tree regardless of where the requests were sent from. |
| `rwcommunity6 <community string> <IPv6 address>` | Same access as with `rwcommunity` with the difference of using IPv6.                  |

# MySQL
nmap scripts
# MSSQL
Impacket's `mssqlclient.py`
nmap scripts
metasploit module: `scanner/mssql/mssql_ping`

# Oracle

# IPMI
