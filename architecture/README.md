\# Arquitetura da Infraestrutura e Topologia de Rede



Este documento detalha o plano de enderecamento IP, a integracao de armazenamento compartilhado via iSCSI e a distribuicao de papeis dentro da rede do laboratorio.



\## Mapeamento de Enderecamento IP



Toda a infraestrutura do laboratorio esta alocada na sub-rede 192.168.244.0/24.



| Hostname | Interface / IP | Funcao no Ambiente | Sistema Operacional |

| :--- | :--- | :--- | :--- |

| pfSense | 192.168.244.1 | Gateway Padrao e Firewall | FreeBSD / pfSense CE |

| DC01 | 192.168.244.129 | Domain Controller Primario, DNS, AD DS | Windows Server 2022 |

| DC02 | 192.168.244.130 | Domain Controller Secundario, DNS | Windows Server 2022 |

| DC03-Physical | 192.168.244.10 | Replica AD DS (Host Fisico) | Windows Server 2022 |

| TrueNAS-Storage | 192.168.244.50 | Target iSCSI / SAN Storage Provider | TrueNAS CORE (ZFS) |

| NODE01 | 192.168.244.230 | No 01 do Windows Server Failover Cluster | Windows Server 2022 |

| NODE02 | 192.168.244.240 | No 02 do Windows Server Failover Cluster | Windows Server 2022 |

| Zabbix-Server | 192.168.244.128 | Zabbix Server 7.0 LTS + Grafana | Ubuntu Server 22.04 LTS |



\## Arquitetura de Armazenamento SAN (iSCSI)



O TrueNAS CORE disponibiliza o armazenamento compartilhado via protocolo iSCSI para consumo exclusivo dos nos do cluster (NODE01 e NODE02).



\### Especificacoes dos Volumes (Extents/Zvols):

1\. Disk Witness (Quorum):

&#x20;  - Tamanho: 1 GB

&#x20;  - Funcao: Disco testemunha do cluster para manutencao do quorum em cenarios de divisao ou perda de no.

&#x20;  - Formato: NTFS / MBR



2\. Volume de Dados do Cluster:

&#x20;  - Tamanho: 50 GB

&#x20;  - Funcao: Volume compartilhado para servicos e arquivos de alta disponibilidade.

&#x20;  - Formato: NTFS / GPT



\### Configuracoes do Target iSCSI:

\- IQN do Target: `iqn.2005-10.org.freenas.ctl:target-cluster`

\- Portal: 192.168.244.50:3260

\- Grupo de Iniciadores: Permissao restrita aos IQNs do NODE01 e NODE02.

