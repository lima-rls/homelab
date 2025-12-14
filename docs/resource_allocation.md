# Alocação de Recursos - Homelab

Distribuição de RAM e vCPUs entre VMs no Proxmox (16 GB total, 4 núcleos físicos).

## Alocação

| VM | RAM | vCPUs | Função |
|---|---|---|---|
| **TrueNAS** | 8 GB | 2 | Storage ZFS (ARC Cache) |
| **Prod DB Host** | 2 GB | 2 | Banco de dados |
| **Prod App Host** | 5 GB | 4 | Plex + Frigate (IA/transcodificação) |
| **Proxmox (Sistema)** | 1 GB | - | Hipervisor |
| **TOTAL** | **16 GB** | **8 vCPUs** | |

## Observações

- **TrueNAS**: 8 GB para ZFS ARC Cache (otimização I/O)
- **Prod App Host**: Recebe mais recursos (5 GB / 4 vCPUs) por ser o mais intensivo
- **Prod DB Host**: 2 GB suficiente para cache de consultas
- Alocação exata sem over-provisioning
