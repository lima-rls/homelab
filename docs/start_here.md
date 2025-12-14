# Homelab - Start Here 🚀

Índice central de documentação do projeto homelab.

---

## 📚 Documentação

### [plano-rede.md](plano-rede.md)
Plano de endereçamento IP da rede `192.168.100.0/24`
- Faixas de IP por ambiente (infra, dev, prod, DHCP)
- IPs atribuídos a cada recurso (Proxmox, TrueNAS, VMs)

### [resource_allocation.md](resource_allocation.md)
Distribuição de RAM e vCPUs entre as VMs
- Tabela de alocação (TrueNAS: 8GB, Prod Apps: 5GB, Prod DB: 2GB)
- Total: 16 GB RAM, 8 vCPUs em 4 núcleos físicos

### [workspace_organization.md](workspace_organization.md)
Estrutura do projeto Ansible
- Organização de playbooks, inventories e variáveis
- Separação dev/prod com proteção Vault
- Fluxo de execução de playbooks

### [PLAYBOOKS.md](../PLAYBOOKS.md)
Lista e descrição dos playbooks disponíveis
- Como executar cada playbook
- Playbooks de infraestrutura e aplicações

---

## 🚀 Quick Start

1. **Ambiente de controle**: `./scripts/bootstrap_control_env.sh`
2. **Ver playbooks**: Consultar `PLAYBOOKS.md`
3. **Ver IPs**: Consultar `plano-rede.md`
4. **Ver recursos**: Consultar `resource_allocation.md`

---

## 🔑 Recursos Principais

- **Proxmox** (`192.168.100.2`) - Hipervisor
- **TrueNAS** (`192.168.100.20-29`) - Storage
- **Dev Server** (`192.168.100.50`) - Desenvolvimento
- **Prod Apps** (`192.168.100.100`) - Plex + Frigate
- **Prod DB** (`192.168.100.101`) - Banco de dados

---

**Última atualização:** Dezembro 2025

---

## 🗣️ Diretriz de Chat

- Todas as respostas no chat devem ser em português (pt-BR).
- Comandos e comentários devem manter linguagem em pt-BR.
- Termos técnicos podem ser mantidos no original com breve explicação.
