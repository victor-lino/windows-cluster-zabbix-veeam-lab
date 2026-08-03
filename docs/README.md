\# Guias de Instalacao e Configuracao



Este documento resume as etapas logicas seguidas para a construcao do ambiente.



\## 1. Infraestrutura Base

\- Configuracao do pfSense para roteamento e regras de firewall da rede 192.168.244.0/24.

\- Promocao dos servidores DC01, DC02 e DC03 a Controladores de Dominio.

\- Ingresso do NODE01 e NODE02 no dominio Active Directory.



\## 2. Storage (TrueNAS)

\- Criacao de Pools e Zvols no TrueNAS.

\- Configuracao do servico Block (iSCSI) criando Portals, Initiators (apontando para os IPs 230 e 240), Targets e Extents.

\- Mapeamento das LUNs no Gerenciamento de Disco do Windows Server nos dois nos.



\## 3. Alta Disponibilidade (WSFC)

\- Instalacao da feature de Failover Clustering no NODE01 e NODE02.

\- Execucao do Cluster Validation Report para garantir a conformidade dos discos iSCSI e rede.

\- Criacao do cluster e definicao do disco de Quorum.



\## 4. Monitoramento (Zabbix e Grafana)

\- Instalacao do Zabbix Server e configuracao de descoberta de rede.

\- Instalacao do Zabbix Agent nos Controladores de Dominio, TrueNAS (via SNMP) e Nos do Cluster.

\- Conexao do Grafana ao banco de dados do Zabbix via plugin/API para geracao dos paineis.



\## 5. Protecao de Dados (Veeam)

\- Instalacao do Veeam Backup \& Replication.

\- Adicao dos hosts do cluster na infraestrutura do Veeam.

\- Criacao de rotinas de backup (Jobs) direcionadas a um repositorio externo para o Failover Cluster e os Agentes Windows/Linux.

