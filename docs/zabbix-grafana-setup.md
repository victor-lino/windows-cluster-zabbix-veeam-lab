# Guia Tecnico: Monitoramento com Zabbix Agent 2 e Grafana API

Procedimento para coleta de telemetria no Windows Server e exposicao de metricas em dashboards dinamicos no Grafana.

## 1. Instalacao do Zabbix Agent 2 no Windows

Instale o Zabbix Agent 2 nos nos NODE01 e NODE02 utilizando o PowerShell:

```powershell
msiexec.exe /i "zabbix_agent2-7.0.0-win-amd64.msi" /qn SERVER="192.168.244.128" SERVERACTIVE="192.168.244.128" HOSTNAME="NODE01" ENABLEPATH=1
New-NetFirewallRule -DisplayName "Zabbix Agent 2" -Direction Inbound -Protocol TCP -LocalPort 10050 -Action Allow
2. Configuracao do Host no Zabbix Server
Acesse o Zabbix Web UI (http://192.168.244.128/zabbix).

Va em Data Collection -> Hosts -> Create Host.

Host name: NODE01.

Templates adicionados:

Windows by Zabbix agent active

Cluster Service by Zabbix agent

Interfaces: Agent -> IP: 192.168.244.230 | Port: 10050.

3. Integracao com o Grafana
Acesse o Grafana (http://192.168.244.128:3000).

Em Data Sources -> Add Data Source, selecione Zabbix.

Settings:

URL: http://192.168.244.128/zabbix/api_jsonrpc.php

Username: Admin

Password: (sua senha)

Clique em Save & Test.


---

### Caminho do arquivo: `monitoring/README.md`

```markdown
# Estrutura de Monitoramento e Observabilidade

Este documento detalha o plano de monitoramento implementado no Zabbix 7.0 LTS e a construcao dos paineis visuais no Grafana.

## Metricas Monitoradas via Zabbix

O monitoramento e dividido entre coleta via Zabbix Agent 2 (hosts Windows) e consultas SNMP (TrueNAS e pfSense).

### Principais Itens Coletados:
1. Status dos Servicos do Windows:
   - `service.info[ClusSvc,state]`: Garante que o servico de cluster esta em execucao (`Running`).
   - `service.info[MSiSCSI,state]`: Garante que o servico do iniciador iSCSI esta operacional.

2. Rede e Latencia:
   - `icmpping`: Monitoramento continuo de disponibilidade de rede.
   - `net.if.in` e `net.if.out`: Trafego e largura de banda consumida pelas interfaces.

3. Armazenamento e Hardware:
   - Espaco livre e tempo de resposta de leitura/escrita nas LUNs iSCSI.
   - Utilizacao de CPU e Memoria RAM dos nos do cluster.

## Integracao Zabbix e Grafana

O Grafana comunica-se com o Zabbix via API `http://192.168.244.128/zabbix/api_jsonrpc.php`.

### Configuracao do Painel do Cluster:
- Painel 1 (Status do Cluster): Utiliza a consulta do item `icmpping` e `service.info[ClusSvc,state]`.
- Mapeamento de Valor (Value Mapping):
  - Valor `1` -> Exibe `ONLINE` com fundo verde.
  - Valor `0` -> Exibe `OFFLINE` com fundo vermelho vibrante e acionamento de alerta visua