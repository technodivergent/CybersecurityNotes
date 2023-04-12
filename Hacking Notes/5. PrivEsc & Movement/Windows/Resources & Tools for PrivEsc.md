# Payloads and Techniques
- [PayloadsAllTheThings - Windows Privilege Escalation](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Windows%20-%20Privilege%20Escalation.md)
- [Priv2Admin - Abusing Windows Privileges](https://github.com/gtworek/Priv2Admin)
- [RogueWinRM Exploit](https://github.com/antonioCoco/RogueWinRM)
- [Potatoes](https://jlajara.gitlab.io/others/2020/11/22/Potatoes_Windows_Privesc.html)
- [Decoder's Blog](https://decoder.cloud/)
- [Token Kidnapping](https://dl.packetstormsecurity.net/papers/presentations/TokenKidnapping.pdf)
- [Hacktricks - Windows Local Privilege Escalation](https://book.hacktricks.xyz/windows-hardening/windows-local-privilege-escalation)
- [GTFOBins](https://gtfobins.github.io/)
- [LOLBAS](https://lolbas-project.github.io/#)
# Windows
## WinPEAS
[Download WinPEAS](https://github.com/carlospolop/PEASS-ng/tree/master/winPEAS)
## PrivescCheck
[Download PrivescCheck](https://github.com/itm4n/PrivescCheck)
```cmd
Set-ExecutionPolicy Bypass -Scope process -Force
. .\PrivescCheck.ps1
Invoke-PrivescCheck
```

## Windows Exploit Suggester - Next Gen (WES-NG)
Python script to run from kali (stealthier) - [Download WES-NG](https://github.com/bitsadmin/wesng)

1. Update vulnerability database: `wes.py --update`
2. Run `systeminfo > systeminfo.txt`
3. Copy to kali and run `wes.py systeminfo.txt`

## Seabelt
https://github.com/GhostPack/Seatbelt
## JAWS
https://github.com/411Hall/JAWS

# Cross-Platform
## Metasploit
If system has active Meterpreter shell: `multi/recon/local_exploit_suggester`

## PEASS
 [PEASS (Linux & Windows)](https://github.com/carlospolop/privilege-escalation-awesome-scripts-suite)