\# Changelog



Todas as alteracoes notaveis neste projeto serao documentadas neste arquivo.



O formato e baseado em \[Keep a Changelog](https://keepachangelog.com/pt-BR/1.0.0/).



\## \[1.0.0] - 2026-08-01



\### Adicionado

\- Implementacao do Windows Server Failover Cluster (WSFC) composto por NODE01 e NODE02.

\- Configuracao do TrueNAS CORE como provedor de storage iSCSI (LUNs de Quorum e Dados).

\- Integracao do Zabbix Server 7.0 LTS com Zabbix Agent 2 nos nos de cluster e controladores de dominio.

\- Criacao de Dashboards de observabilidade no Grafana utilizando a API do Zabbix.

\- Implantacao do Veeam Backup \& Replication Community Edition para execucao de jobs de backup do cluster.

\- Documentacao tecnica completa dividida por modulos (arquitetura, instalacao, monitoramento, validacao e troubleshooting).

\- Relatorio e evidencias dos testes praticos de failover e resiliencia de rede.

