\# Plano de Testes, Simulacao de Falhas e Validacoes



Relatorio de execucao de testes praticos de resiliencia, alta disponibilidade e verificacao de alertas no ambiente.



\## Teste 01: Simulacao de Queda do No Ativo (NODE01)



\### Procedimento Executado:

1\. Identificado que o NODE01 era o proprietario ativo dos recursos de armazenamento e do grupo de cluster.

2\. Forcada a desconexao da interface de rede principal do NODE01 via hipervisor.



\### Comportamento Observado:

\- Tempo de Deteccao do WSFC: 3.5 segundos.

\- Failover Automatico: O NODE02 assumiu automaticamente a propriedade do Disco de Dados e do Quorum.

\- Deteccao pelo Zabbix: Trigger de indisponibilidade disparada em menos de 10 segundos.

\- Resposta no Grafana: O painel referente ao NODE01 alterou instantaneamente seu estado para OFFLINE (fundo vermelho).



\## Teste 02: Recuperacao do No e Failback



\### Procedimento Executado:

1\. Interface de rede do NODE01 reativada.

2\. Servicos do sistema inicializados.



\### Comportamento Observado:

\- NODE01 ingressou novamente no cluster como no secundario (Standby).

\- Zabbix detectou o retorno da conectividade e fechou o evento de alerta.

\- Grafana normalizou o status do NODE01 para ONLINE (fundo verde).



\## Teste 03: Execucao das Rotinas de Backup no Veeam



\### Procedimento Executado:

1\. Disparo manual do job Job FailOver Cluster no Veeam Backup \& Replication.



\### Comportamento Observado:

\- O Veeam processou as informacoes dos nos e concluiu o backup com status Success.

