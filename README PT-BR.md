# Homelab Avancado: Windows Server Failover Cluster, TrueNAS, Zabbix, Grafana e Veeam

Laboratorio pratico focado na implementacao, monitoramento em tempo real e protecao de dados de uma infraestrutura corporativa de Alta Disponibilidade (HA) utilizando Windows Server Failover Cluster (WSFC).

## Visao Geral do Projeto

Este projeto documenta a construcao e validacao de um ambiente de missao critica contendo:
- Alta Disponibilidade (HA): Cluster de Failover de dois nos (NODE01 e NODE02) em Windows Server.
- Storage Compartilhado (SAN/iSCSI): TrueNAS Core disponibilizando LUNs iSCSI para Quorum e Dados.
- Observabilidade e Monitoramento: Zabbix Server e Grafana para coleta de metricas e alertas em tempo real.
- Protecao de Dados: Veeam Backup & Replication para copias de seguranca automatizadas do cluster.

## Estrutura de Documentacao

Para facilitar a navegacao, a documentacao deste laboratorio foi dividida nos seguintes modulos:

- [Arquitetura e Topologia](architecture/README.md): Detalhes de rede, mapeamento de IPs e topologia.
- [Guias de Configuracao](docs/README.md): Resumo do processo de implementacao dos servicos.
- [Monitoramento](monitoring/README.md): Detalhes da integracao entre Zabbix e Grafana.
- [Testes e Validacao](validation/README.md): Relatorio do teste de stress, failover e status dos backups.

## Galeria de Evidencias

As evidencias visuais do funcionamento do ambiente estao localizadas na pasta `/images`. O fluxo demonstra:
1. O ambiente em estado saudavel.
2. A interrupcao forgada do NODE01.
3. A deteccao imediata da falha pelo Zabbix e Grafana.
4. A continuidade dos servicos no NODE02.
5. O sucesso das rotinas de backup no Veeam.

## Tecnologias Utilizadas

- Sistemas Operacionais: Windows Server, TrueNAS CORE, Linux Ubuntu
- Storage e Redes: iSCSI Target/Initiator, ZFS, pfSense (Firewall/Gateway)
- Alta Disponibilidade: Windows Server Failover Clustering (WSFC)
- Monitoramento: Zabbix 7.0 LTS, Grafana
- Backup: Veeam Backup & Replication Community Edition