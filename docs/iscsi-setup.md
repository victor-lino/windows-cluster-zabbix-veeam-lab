# Guia Tecnico: Configuracao de Storage Block-Level iSCSI no TrueNAS CORE

Este documento descreve os passos necessarios para disponibilizar e configurar volumes iSCSI no TrueNAS CORE para uso como armazenamento compartilhado em um ambiente Windows Server Failover Cluster (WSFC).

## 1. Criacao dos Volumes de Armazenamento (Zvols)

1. Acesse a interface web do TrueNAS CORE.
2. Navegue ate Storage -> Pools.
3. Selecione o Zpool principal e clique nos tres pontos -> Add Zvol.
4. Configure o primeiro Zvol (Disco de Quorum):
   - Name: quorum_vol
   - Size: 1 GiB
   - Sync: Standard
   - Compression level: Inherit (lz4)
5. Repita o processo para o segundo Zvol (Disco de Dados):
   - Name: data_vol
   - Size: 50 GiB
   - Sync: Standard
   - Compression level: Inherit (lz4)

## 2. Configuracao do Servico iSCSI

Navegue ate Sharing -> Block Shares (iSCSI) e siga a ordem de configuracao das abas:

### 2.1. Portals
1. Clique em Portals -> Add.
2. Description: Portal-Cluster-WSFC
3. IP Address: Selecione o IP do TrueNAS (192.168.244.50).
4. Port: 3260.

### 2.2. Initiators Groups
1. Clique em Initiators Groups -> Add.
2. Deixe a opcao "Allow all initiators" desmarcada para aplicar restricoes de seguranca.
3. Adicione os IQNs dos servidores Windows:
   - IQN NODE01: iqn.1991-05.com.microsoft:node01.santechsous.local
   - IQN NODE02: iqn.1991-05.com.microsoft:node02.santechsous.local

### 2.3. Targets
1. Clique em Targets -> Add.
2. Target Name: target-cluster
3. Target Alias: wsfc-storage
4. Portal Group ID: 1
5. Initiator Group ID: 1

### 2.4. Extents
1. Clique em Extents -> Add.
2. Extent 1 (Quorum):
   - Name: extent_quorum
   - Extent Type: Device
   - Device: select quorum_vol
   - Logical Block Size: 512
3. Extent 2 (Dados):
   - Name: extent_data
   - Extent Type: Device
   - Device: select data_vol
   - Logical Block Size: 512

### 2.5. Associated Targets
1. Associe os Extents ao Target criado:
   - Target: target-cluster | LUN ID: 0 | Extent: extent_quorum
   - Target: target-cluster | LUN ID: 1 | Extent: extent_data

## 3. Ativacao do Servico
1. Navegue ate System -> Services.
2. Localize o servico iSCSI, marque a caixa "Start Automatically" e inicie o servico.