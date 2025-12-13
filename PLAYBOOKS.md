# Playbooks do Homelab

## Workflow de Provisionamento

### 1️⃣ Preparar Template (execute 1x ou quando precisar renovar)
```bash
ansible-playbook -i ansible/inventory/proxmox.ini ansible/playbooks/preparar_template_ubuntu.yml --ask-pass
```
**O que faz:**
- Baixa imagem cloud Ubuntu Noble
- Cria template VMID 9000 (TEMPLATE-UBT-SERVER)
- Configura cloud-init com usuário `ubuntu` e sua chave SSH

**Quando usar:**
- Setup inicial
- Atualizar template para nova versão Ubuntu
- Recuperar template deletado

---

### 2️⃣ Provisionar VMs (execute sempre que precisar nova VM)
```bash
ansible-playbook ansible/playbooks/provisionar_dev_ubt.yml --ask-vault-pass
```
**O que faz:**
- Clona template para criar VM dev-server
- Aplica configurações de rede (IP 192.168.100.50)
- Inicia VM e aguarda boot

**Quando usar:**
- Criar VM dev novo
- Reprovisionar VM existente

**Nota:** Configure `proxmox_vms` em `ansible/group_vars/proxmox_vms.yml` com specs da VM

---

### 3️⃣ Configurar Docker (execute após VM estar rodando)
```bash
ansible-playbook -i ansible/inventory/proxmox.ini ansible/playbooks/configurar_docker.yml --ask-vault-pass
```
**O que faz:**
- Instala Docker Engine + compose
- Configura daemon e permissões
- Valida instalação

**Quando usar:**
- Instalar Docker na VM provisionada
- Preparar VM para aplicações containerizadas

---

### 4️⃣ Remover VMs (com segurança)
```bash
ansible-playbook ansible/playbooks/remover_vm.yml --ask-vault-pass --extra-vars "vm_name=dev-server"
```
**O que faz:**
- Lista VMs conhecidas
- Pede confirmação para VMs com tags `prod` ou `template`
- Remove VM selecionada

**Quando usar:**
- Deletar VM específica com segurança

---

## Fluxo Recomendado

```
1. Preparar template (1x)
   ↓
2. Provisionar VM (1x por VM)
   ↓
3. Configurar Docker (1x por VM)
   ↓
4. Deploy aplicações (via Docker Compose ou Ansible)
```

## Configurações Importantes

- **Vault**: `ansible/vault/proxmox_access.yml` → credenciais Proxmox (criptografado)
- **Inventory**: `ansible/inventory/proxmox.ini` → hosts e conexões SSH
- **VMs**: `ansible/group_vars/proxmox_vms.yml` → specs (cores, RAM, disco, IP)
- **SSH**: `~/.ssh/id_rsa.pub` → injetada no template automaticamente

## Troubleshooting

| Problema | Solução |
|----------|---------|
| SSH timeout na VM | VM ainda bootando, aguarde 45s e tente novamente |
| Vault decrypt failed | Verificar senha vault (criada em `proxmox_access.yml`) |
| Template não encontrado | Executar `preparar_template_ubuntu.yml` primeiro |
| IP não configurado | Playbook `provisionar_dev_ubt.yml` trata automaticamente |
