# Plano de Endereçamento IP - Homelab
# Rede: 192.168.100.0/24

## Infraestrutura (192.168.100.1-49)
192.168.100.1     - Gateway/Router
192.168.100.2     - Proxmox (homelab)
192.168.100.10-19 - Servidores de infraestrutura (DNS, DHCP, etc)
192.168.100.20-29 - Storage/NAS (TrueNAS)
192.168.100.30-49 - Reservado infraestrutura

## Desenvolvimento (192.168.100.50-99)
192.168.100.50    - dev-server (Ubuntu + Docker)
192.168.100.51-79 - Outros servidores dev
192.168.100.80-99 - Containers/testes dev

## Produção (192.168.100.100-199)
192.168.100.100   - prod-apps (Plex, Frigate, etc)
192.168.100.101   - prod-db (PostgreSQL, etc)
192.168.100.102-149 - Outros serviços prod
192.168.100.150-199 - Containers prod

## DHCP/Dinâmico (192.168.100.200-254)
192.168.100.200-254 - Pool DHCP para dispositivos temporários

## Resumo por ambiente:
- Infra:  .1-.49   (50 IPs)
- Dev:    .50-.99  (50 IPs)
- Prod:   .100-.199 (100 IPs)
- DHCP:   .200-.254 (55 IPs)
