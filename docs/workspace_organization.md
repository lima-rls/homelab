# Estrutura de Projeto Ansible - Homelab

Organização **simplificada** para playbooks com separação dev/prod e proteção de ambientes.

## 📁 Estrutura do Projeto

```
ansible/
├── inventory/
│   ├── dev.ini                      # Inventário dev
│   └── prod.ini                     # Inventário prod
│
├── playbooks/
│   ├── infrastructure/              # Playbooks de infraestrutura
│   │   ├── docker.yml               # Configura Docker
│   │   ├── truenas.yml              # Configura TrueNAS
│   │   ├── database.yml             # Configura Banco de Dados
│   │   └── [outros...]
│   │
│   └── apps/                        # Playbooks de aplicações
│       ├── frigate.yml              # Exemplo: Frigate
│       ├── home_assistant.yml       # Exemplo: Home Assistant
│       └── [outros...]
│
├── group_vars/
│   ├── all_dev.yml                  # Todas variáveis dev
│   └── all_prod.yml                 # Todas variáveis prod (Vault)
│
└── vault/
    └── prod_secrets.yml             # Apenas senhas/tokens prod
```

---

## 🎯 Princípios de Uso

### 1. **Playbooks por Sistema**
Cada playbook configura um sistema completo, com tasks diretas (sem roles):

```yaml
# playbooks/docker.yml
- name: Configurar Docker
  hosts: docker_hosts
  vars_files:
    - "../group_vars/all_{{ env }}.yml"
    - "../vault/prod_secrets.yml"
  
  tasks:
    - name: Instalar Docker
      apt:
        name: docker.io
        state: present
    
    - name: Configurar memória
      template:
        src: daemon.json.j2
        dest: /etc/docker/daemon.json
      vars:
        memory_limit: "{{ docker_memory }}"
```

### 2. **Variáveis Centralizadas por Ambiente**
Um arquivo para todas as variáveis de cada ambiente:

**group_vars/all_dev.yml**
```yaml
docker_memory: 2GB
truenas_pool: tank-dev
frigate_cameras: 1
use_mock_data: true
```

**group_vars/all_prod.yml**
```yaml
docker_memory: 5GB
truenas_pool: tank-prod
frigate_cameras: 4
enable_monitoring: true
```

### 3. **Proteção Vault para Prod**
Credenciais sensíveis ficam em `vault/prod_secrets.yml` (criptografado):

```yaml
# vault/prod_secrets.yml (criptografado)
db_password: "senhaSegura123"
api_token: "token_secreto"
```

---

## 🚀 Como Executar

### Desenvolvimento (sem senha)
```bash
# Executar playbook de infraestrutura
ansible-playbook \
  -i inventory/dev.ini \
  -e env=dev \
  playbooks/infrastructure/docker.yml

# Executar playbook de app
ansible-playbook \
  -i inventory/dev.ini \
  -e env=dev \
  playbooks/apps/frigate.yml
```

### Produção (requer password vault)
```bash
# Infraestrutura (com proteção Vault)
ansible-playbook \
  -i inventory/prod.ini \
  -e env=prod \
  playbooks/infrastructure/docker.yml \
  --ask-vault-pass

# Aplicações (com proteção Vault)
ansible-playbook \
  -i inventory/prod.ini \
  -e env=prod \
  playbooks/apps/frigate.yml \
  --ask-vault-pass
```

---

## 🔐 Segurança

✅ **Dev**: Sem senhas, execução rápida e fácil  
✅ **Prod**: Requer `--ask-vault-pass`, evita execução acidental  
✅ **Vault**: Credenciais criptografadas em `vault/prod_secrets.yml`  
✅ **Variáveis**: Isoladas por ambiente em `group_vars/`  

**Nunca commitar** `vault/prod_secrets.yml` descriptografado!

---

## 📋 Estrutura Simplificada

**Vantagens:**
- ✅ Sem roles (tasks diretas nos playbooks)
- ✅ Playbooks separados: infrastructure/ e apps/
- ✅ Variáveis centralizadas (um arquivo por ambiente)
- ✅ Fácil de entender e manter

**Mantém as premissas:**
- ✅ Um playbook por sistema
- ✅ Separação infra (base) vs apps (aplicações)
- ✅ Variáveis isoladas dev/prod
- ✅ Vault obrigatório para prod  

**Nunca commitar** `vault/prod_secrets.yml` descriptografado!

---

## 🔄 Fluxo de Desenvolvimento

1. **Criar/testar em DEV** (iterativo, sem senha)
   ```bash
   ansible-playbook -i inventory/dev.ini -e env=dev playbooks/infrastructure/docker.yml
   ```

2. **Ajustar variáveis** em `group_vars/all_dev.yml`

3. **Preparar para PROD**
   - Atualizar `group_vars/all_prod.yml`
   - Adicionar senhas em `vault/prod_secrets.yml` (criptografado)
   - Testar com `--check` antes de aplicar

4. **Deploy em PROD** (com proteção obrigatória)
   ```bash
   ansible-playbook -i inventory/prod.ini -e env=prod playbooks/infrastructure/docker.yml --ask-vault-pass
   ```

---

**Última atualização:** Dezembro 2025
