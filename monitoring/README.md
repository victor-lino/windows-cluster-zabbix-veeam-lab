\# Monitoramento e Observabilidade



A estrategia de monitoramento foi desenhada para ter visibilidade total sobre a saude dos nos, do storage e dos servicos de rede.



\## Arquitetura do Monitoramento



\- Zabbix Server: Atua como o motor principal de coleta de metricas e definicao de triggers (alertas).

\- Zabbix Agent: Instalado nos servidores Windows (NODE01, NODE02, DC01, DC02, DC03) para monitoramento de recursos do sistema operacional (CPU, Memoria, Discos, Status de Servicos).

\- SNMP: Utilizado para monitorar appliances de rede e storage (pfSense e TrueNAS).



\## Dashboards no Grafana



O Grafana consome os dados do Zabbix para criar visualizacoes amigaveis e dinamicas.



Paineis configurados incluem:

\- Status de conectividade (ICMP Ping) de todos os servidores essenciais.

\- Paineis de status destacando a disponibilidade do NODE01 e NODE02 em tempo real.

\- Visualizacao de logs de eventos criticos e status do servico de cluster do Windows.



A cor de fundo dos paineis (Verde para Online, Vermelho para Offline) foi configurada utilizando thresholds baseados nos valores de retorno do Zabbix, permitindo identificacao visual instantanea de incidentes.

