# Homelab and Projects

Hi, my name is Wayne Altuama. I'm a recent Algonquin College graduate and a Computer Systems and Networking Technician, and this repo documents the home lab Im building to practice the skills ive developed while garnering new skills.
# Homelab Projects

| Project                                                                               | Stack                                            |
| ------------------------------------------------------------------------------------- | ------------------------------------------------ |
| [Proxmox Virtualization Server Setup](Proxmox-Cluster.md)                             | Proxmox VE                                       |
| [VirtualBox to Proxmox VM Migration](./Proxmox/VirtualBox-to-Proxmox-VM-Migration.md) | Proxmox, VirtualBox, VBoxManage                  |
| [Self-Hosted Obsidian Vault Sync](Self-Hosted-Obsidian-Vault-Sync.md)                 | CouchDB, Self-hosted LiveSync, Tailscale, Docker |
| [2-Node Proxmox Cluster](Proxmox-Cluster.md)                                          | Proxmox VE                                       |
### The Lab

| Host                            | Role                       | Specs                                               |
| ------------------------------- | -------------------------- | --------------------------------------------------- |
| `pve.lab` (Dell OptiPlex 7040)  | Primary Proxmox hypervisor | Intel i7-6700, 32GB DDR4, 1TB SATA SSD              |
| `pve2.lab` (Dell OptiPlex 7020) | Secondary Proxmox node     | Intel i7 (DDR3 gen), 32GB DDR3, 512GB SSD, GTX 1650 |
| External SSD                    | Backup target              | 512GB                                               |

_All hosts sit behind Tailscale, no ports forwarded to the internet._

# Algonquin College Projects

| Project                                                                                           | Stack                                                                                                                      |
| ------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------- |
| [Advanced Enterprise LAN & Routing](./Algonquin/Advanced-Enterprise-LAN-and-Routing.md)           | Cisco IOS, Packet Tracer, IPv4, OSPFv2, BGPv4, RSTP, LACP                                                                  |
| [Enterprise LAN & Routing](./Algonquin/Enterprise-LAN-and-Routing.md)                             | Cisco IOS, IPv4, IPv6, OSPFv2, Routing, SSH, TFTP                                                                          |
| [Windows Enterprise Administration](./Algonquin/Windows-Enterprise-Administration.md)             | Windows Server 2022, Exchange Server 2019, Active Directory, DNS, OWA, Thunderbird, VMware                                 |
| [Linux Network Services](./Algonquin/Linux-Network-Services.md)                                   | RHEL, Bash, BIND DNS, NFS, Samba, SSH, iptables, VMware                                                                    |
| [Malware Traffic Analysis - Pushdo Trojan](./Algonquin/Malware-Traffic-Analysis-Pushdo-Trojan.md) | Security Onion, Sguil, Kibana, Wireshark, NetworkMiner, VirusTotal, MITRE ATT&CK, VirtualBox                               |
| [Enterprise MSP Project](./Algonquin/Emerging-Tech-MSP.md)                                        | Proxmox, Windows Server, Ubuntu, VPN, Veeam, LDAP, Active Directory, TrueNAS, DNS, HTTPS, DMZ, DHCP, RDP, SSH, iSCSI, IPMI |
| [Cloud Helpdesk Ticketing System](./Algonquin/Cloud-Helpdesk-Ticketing-System.md)                 | Spiceworks, Cloud Hosted, Email Authentication                                                                             |

## Skills Demonstrated

| Category                                  | Key Skills                                                                                                                                                                                                                                                               |
| :---------------------------------------- | :----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Virtualization & Infrastructure**       | Proxmox VE deployment & 2-node clustering · VM lifecycle management (create/migrate/backup) · VirtualBox → Proxmox migration (`VBoxManage`) · Docker & Docker Compose · TrueNAS/ZFS storage design (NFS/SMB/iSCSI) · Hardware selection & capacity planning              |
| **Networking & Routing**                  | Cisco IOS routing/switching (multi-area OSPFv2, BGPv4, RSTP, VTPv2, LACP EtherChannel) · IPv4/IPv6 dual-stack · Route summarization, redistribution & floating static routes · Tailscale & WireGuard VPN (subnet routing, site-to-site) · VLAN segmentation & DMZ design |
| **Windows Systems Administration**        | Windows Server 2019/2022 deployment · Active Directory (multi-forest, OUs, users/groups) · DNS (zones, conditional forwarders, MX) · Exchange Server 2019 (mailboxes, cross-domain mail flow) · DFS Namespace/Replication · OWA / IMAP / POP3 client access              |
| **Linux Systems Administration**          | RHEL & Ubuntu server administration · BIND DNS · NFS & Samba file services · OpenLDAP directory services · GlusterFS replicated storage · SSH hardening & iptables firewalling · Bash scripting & systemd · SELinux configuration                                        |
| **Security Operations & Threat Analysis** | Security Onion, Sguil, Kibana · Wireshark packet analysis · NetworkMiner host/OS fingerprinting · VirusTotal threat intel verification · MITRE ATT&CK mapping · IOC identification & documentation                                                                       |
| **Network Security**                      | Default-DROP firewall policy design · Network segmentation & access control · SSH/port security hardening · Stateful firewall rules (OPNsense/pfSense)                                                                                                                   |
| **Backup & Disaster Recovery**            | Veeam Backup & Replication job design · Off-site repository replication (3-2-1 strategy) · Config backup via TFTP                                                                                                                                                        |
| **IT Service Management**                 | Spiceworks Cloud Helpdesk deployment · Multi-tenant, branded ticketing portals · Email-verified authentication & ticket lifecycle management                                                                                                                             |
| **Tools & Methodologies**                 | Cisco Packet Tracer · VMware/VirtualBox · Incident investigation methodology · Technical documentation & report writing · CLI proficiency (Linux, Windows PowerShell, Cisco IOS)                                                                                         |

## Contact

- LinkedIn: [https://www.linkedin.com/in/hussein-altuama-7b6203335/](https://www.linkedin.com/in/hussein-altuama-7b6203335/)
- Resume: available on request
- Email: waynealtuama@gmail.com
