# Guia Tecnico: Implantacao do Windows Server Failover Cluster (WSFC)

Este documento instrui a configuracao dos dois nos Windows Server 2022 para formacao do cluster de alta disponibilidade.

## 1. Pre-requisitos de Dominio e Rede

1. Garanta que NODE01 (192.168.244.230) e NODE02 (192.168.244.240) estao ingressados no dominio santechsous.local.
2. Verifique a resolucao de nomes executando nslookup mutuo entre os nos e os controladores de dominio.
3. Permita o trafego do Cluster (Porta UDP/TCP 3343) no Firewall do Windows.

## 2. Conexao dos Discos iSCSI no Windows

Execute os seguintes comandos via PowerShell em modo Administrador no NODE01 e NODE02:

```powershell
Set-Service -Name MSiSCSI -StartupType Automatic
Start-Service -Name MSiSCSI

New-iSCSITargetPortal -TargetPortalAddress 192.168.244.50
Connect-IscsiTarget -NodeAddress iqn.2005-10.org.freenas.ctl:target-cluster -IsPersistent $true
No NODE01, inicialize, particione e formate os discos:

PowerShell
Get-Disk | Where-Object PartitionStyle -eq 'RAW' | Initialize-Disk -PartitionStyle MBR

New-Partition -DiskNumber 1 -UseMaximumSize -AssignDriveLetter | Format-Volume -FileSystem NTFS -NewFileSystemLabel "Quorum" -Confirm:$false
New-Partition -DiskNumber 2 -UseMaximumSize -AssignDriveLetter | Format-Volume -FileSystem NTFS -NewFileSystemLabel "ClusterData" -Confirm:$false
3. Instalacao da Feature e Criacao do Cluster
Execute em ambos os nos:

PowerShell
Install-WindowsFeature -Name Failover-Clustering -IncludeManagementTools
No NODE01, execute a validacao da infraestrutura e crie o cluster:

PowerShell
Test-Cluster -Node NODE01, NODE02
New-Cluster -Name CLUSTER-HA -Node NODE01, NODE02 -StaticAddress 192.168.244.250
4. Configuracao do Quorum (Disk Witness)
Defina o disco de 1 GB como testemunha de quorum:

PowerShell
Set-ClusterQuorum -NodeAndDiskMajority "Cluster Disk 1"

---

### Caminho do arquivo: `docs/veeam-setup.md`

```markdown
# Guia Tecnico: Protecao de Dados do Cluster com Veeam Backup & Replication

Instrucoes para a configuracao de jobs de backup automatizados e com consistencia de dados para a infraestrutura de cluster.

## 1. Cadastro da Infraestrutura no Veeam

1. Abra o console do Veeam Backup & Replication.
2. Navegue ate Inventory -> Infrastructure Jobs -> Managed Servers -> Add Server.
3. Selecione a opcao Microsoft Windows.
4. Insira o FQDN ou IP do Cluster (CLUSTER-HA.santechsous.local ou 192.168.244.250).
5. Forneca as credenciais de Administrador do Dominio.
6. Aguarde a instalacao automatica dos agentes nos nos NODE01 e NODE02.

## 2. Criacao do Protection Group para o Cluster

1. Em Inventory -> Physical Infrastructure, clique em Add Protection Group.
2. Name: PG-Windows-Cluster.
3. Type: Cluster object.
4. Adicione o nome do cluster CLUSTER-HA.

## 3. Configuracao do Job de Backup (Backup Job)

1. Navegue ate Home -> Jobs -> Add Job -> Windows Computer.
2. Type: Server.
3. Mode: Managed by backup server.
4. Name: Job FailOver Cluster.
5. Objects: Selecione o Protection Group PG-Windows-Cluster.
6. Backup Mode: Entire Computer.
7. Retention Policy: 7 dias / 7 pontos de restauracao.
8. Guest Processing: Ativar "Enable application-aware processing".
9. Schedule: Configurar para execucao diaria automatica.