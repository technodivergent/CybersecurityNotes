| Layer                    | Description                                                                                            | Information Category                                                                               |
| ------------------------ | ------------------------------------------------------------------------------------------------------ | -------------------------------------------------------------------------------------------------- |
| `1. Internet Presence`   | Identification of internet presence and externally accessible infrastructure.                          | Domains, Subdomains, vHosts, ASN, Netblocks, IP Addresses, Cloud Instances, Security Measures      |
| `2. Gateway`             | Identify the possible security measures to protect the company's external and internal infrastructure. | Firewalls, DMZ, IPS/IDS, EDR, Proxies, NAC, Network Segmentation, VPN, Cloudflare                  |
| `3. Accessible Services` | Identify accessible interfaces and services that are hosted externally or internally.                  | Service Type, Functionality, Configuration, Port, Version, Interface                               |
| `4. Processes`           | Identify the internal processes, sources, and destinations associated with the services.               | PID, Processed Data, Tasks, Source, Destination                                                    |
| `5. Privileges`          | Identification of the internal permissions and privileges to the accessible services.                  | Groups, Users, Permissions, Restrictions, Environment                                              |
| `6. OS Setup`            | Identification of the internal components and systems setup.                                           | OS Type, Patch Level, Network config, OS Environment, Configuration files, sensitive private files |

Goals
1. Identify all targets worth investigating and testing
2. Understand the barriers we might face
3. Understand functionality and business use of target system
4. Identify dependencies between processes
5. Identify what's possible with privileges
6. Understand how administrators manage systems and what sensitive information can be captured

# Infrastructure-based
Examine SSL certs on public websites to expand awareness of target

## Certificate Transparency
Use [crt.sh](crt.sh) to find additional subdomains. This is a source of Certificate Transparency logs, a process intended to enable verification of issued certs for encrypted connections.

```bash
curl -s https://crt.sh/\?q\=inlanefreight.com\&output\=json | jq .

curl -s https://crt.sh/\?q\=inlanefreight.com\&output\=json | jq . | grep name | cut -d":" -f2 | grep -v "CN=" | cut -d'"' -f2 | awk '{gsub(/\\n/,"\n");}1;' | sort -u

```

## Shodan
Use shodan to do passive recon
```bash
for i in $(cat ip-addresses.txt);do shodan host $i;done
```

## DNS Lookups
```bash
dig any inlanefreight.com
```

## Cloud Recon
[Domain.glass](domain.glass) is a third-party tool that can be useful to ID infra
[Public Buckets by GrayhatWarfare](https://buckets.grayhatwarfare.com/)
Google Dorking can help identify AWS buckets, Azure blobs or GCP cloud storage

```
intext: [company] inurl:amazon.aws.com
intext: [company] inurl:blob.core.windows.net
```
