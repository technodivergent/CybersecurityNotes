
**Link-Local Multicast Name Resolution**
Identifies hosts when DNS fails (Formerly NBT-NS)

**Key Flaw**
Services utilize a user's username and NTLMv2 hash when appropriately responded to

# Overview of Attack
`Responder.py` is part of impacket (`/opt/impacket`).
```bash
responder -I eth0 -rdwv
```
1. Target host requests an unknown/typo'd hostname
2. Target broadcasts on network to identify unknown hostname
3. Hacker intercepts, responds with request for hash
4. Target sends hash
5. Use a [[Hash Attacks|Hash Attack]] to crack (or pass the hash?)
# Mitigation
[LLMNR/NBT-NS Poisoning | MITRE ATT&CK](https://attack.mitre.org/techniques/T1557/001/)
1. Disable LLMNR and NetBIOS in local computer security settings or by group policy if they are not needed within an environment.
2. Use host-based security software to block LLMNR/NetBIOS traffic. Enabling SMB Signing can stop NTLMv2 relay attacks.
3. Network intrusion detection and prevention systems that can identify traffic patterns indicative of AiTM activity can be used to mitigate activity at the network level.
4. Network segmentation can be used to isolate infrastructure components that do not require broad network access. This may mitigate, or at least alleviate, the scope of AiTM activity.
If organization cannot disable LLMNR/NBT-NS:
1. Require Network Access Control (MAC addr proofing)
2. Require strong user passwords to prevent hash cracking