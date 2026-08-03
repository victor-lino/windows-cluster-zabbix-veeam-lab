# Troubleshooting e Catalogo de Erros

Este guia contem a documentacao de problemas reais enfrentados durante a montagem e os testes do laboratorio, acompanhados de suas respectivas solucoes.

## Problema 1: Falha na Conexao iSCSI Apos Reinicializacao do TrueNAS
- Sintoma: Os nos do cluster perdiam os discos mapeados e o volume de Quorum entrava em estado Failed.
- Causa: O servico iSCSI Initiator do Windows tentava reconectar antes da inicializacao completa do pool ZFS no TrueNAS.
- Solucao: 
  1. Configurado o servico Microsoft iSCSI Initiator Service para Automatic (Delayed Start).
  2. Ajustado o tempo de timeout do iSCSI no registro do Windows:
     ```cmd
     reg add "HKLM\SYSTEM\CurrentControlSet\Services\Disk" /v TimeOutValue /t REG_DWORD /d 60 /f
     ```

## Problema 2: Event ID 1135 no FailoverClustering
- Sintoma: Registros constantes do Event ID 1135 no Log de Eventos do Windows Server ("Cluster node 'NODE01' was removed from the active failover cluster membership").
- Causa: Latencia de rede entre a interface de heartbeat e a interface de gerenciamento quando sob alta carga.
- Solucao: Ajuste do threshold de tolerancia do cluster via PowerShell:
  ```powershell
  (Get-Cluster).SameSubnetDelay = 2000
  (Get-Cluster).SameSubnetThreshold = 10