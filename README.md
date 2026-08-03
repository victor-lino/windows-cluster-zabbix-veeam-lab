# Advanced Homelab: Windows Server Failover Cluster, TrueNAS, Zabbix, Grafana and Veeam

Practical laboratory focused on the implementation, real-time monitoring and data protection of a High Availability (HA) corporate infrastructure using Windows Server Failover Cluster (WSFC).

## Project Overview

This project documents the construction and validation of a mission critical environment containing:
- High Availability (HA): Failover Cluster of two nodes (NODE01 and NODE02) on Windows Server.
- Shared Storage (SAN/iSCSI): TrueNAS Core providing iSCSI LUNs for Quorum and Data.
- Observability and Monitoring: Zabbix Server and Grafana for collecting metrics and alerts in real time.
- Data Protection: Veeam Backup & Replication for automated cluster backups.

## Documentation Structure

To facilitate navigation, the documentation for this laboratory was divided into the following modules:

- [Architecture and Topology](architecture/README.md): Network details, IP mapping and topology.
- [Configuration Guides](docs/README.md): Summary of the service implementation process.
- [Monitoring](monitoring/README.md): Details of the integration between Zabbix and Grafana.
- [Tests and Validation](validation/README.md): Stress test report, failover and backup status.

## Evidence Gallery

Visual evidence of the environment's operation is located in the `/images` folder. The flow demonstrates:
1. The environment is in a healthy state.
2. Forced interruption of NODE01.
3. Immediate failure detection by Zabbix and Grafana.
4. Continuity of services at NODE02.
5. The success of Veeam backup routines.

## Technologies Used

- Operating Systems: Windows Server, TrueNAS CORE, Linux Ubuntu
- Storage and Networks: iSCSI Target/Initiator, ZFS, pfSense (Firewall/Gateway)
- High Availability: Windows Server Failover Clustering (WSFC)
- Monitoring: Zabbix 7.0 LTS, Grafana
- Backup: Veeam Backup & Replication Community Edition