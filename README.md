# Infraestrutura Integrada para Gestão de Projetos e Recursos — TechSolutions

Este repositório contém o código de Infraestrutura como Código (IaC) para a conceção, aprovisionamento e administração de uma infraestrutura informática para a empresa fictícia TechSolutions.

O projeto simula um ambiente de suporte à gestão de projetos recorrendo a virtualização (VirtualBox/Vagrant), automação de configurações (Ansible), contentorização (Docker), Reverse Proxy com SSL/TLS (Nginx), base de dados isolada (PostgreSQL), monitorização em tempo real (Uptime Kuma) e cópias de segurança cifradas (Restic).

---

## 🎯 Requisitos Implementados

| ID | Requisito Técnico | Estado |
|---|---|---|
| R01 | Plataforma OpenProject via HTTP/HTTPS centralizada por Nginx Reverse Proxy. | 🔄 Em curso |
| R02 | Isolamento da BD PostgreSQL na VM 2, aceitando ligações apenas da VM 1 (IP 192.168.56.10). | ✅ Concluído |
| R03 | Persistência de dados através de volumes Docker mapeados no host. | 🔄 Em curso |
| R04 | Acesso remoto SSH restrito a chaves públicas com desativação de passwords e root. | ✅ Concluído |
| R05 | Regras estritas de Firewall (UFW) em ambas as VMs (política deny incoming). | ✅ Concluído |
| R06 | Monitorização de conectividade e serviços em tempo real via Uptime Kuma. | 🔄 Em curso |
| R07 | Rotinas de backup incremental cifrado (AES-256) via Restic com script automatizado. | ✅ Concluído |
| R08 | Reprodutibilidade total da infraestrutura via Vagrantfile e playbooks Ansible. | ✅ Concluído |

---

## 🔒 Conformidade com o RGPD (Privacy by Design & by Default)

A arquitetura foi desenhada para cumprir os princípios de Segurança do Tratamento (Artigo 32.º do RGPD) e Minimização de Dados:

- **Cifragem em Trânsito:** Comunicação HTTPS no Nginx e tráfego encriptado via SSL/TLS entre a VM 1 (OpenProject) e a VM 2 (PostgreSQL).
- **Cifragem em Repouso:** Repositórios de cópias de segurança geridos pelo Restic encriptados com a cifra AES-256.
- **Segregação de Acessos & Hardening:** Isolamento de rede na VM 2 via UFW e controlo de acessos rigoroso ao nível de utilizadores de base de dados e sistema.
- **Plano de Recuperação:** Medição e testes de RTO (Recovery Time Objective) e RPO (Recovery Point Objective) através de cenários de restauro automatizados.

---

## 🛠️ Pré-requisitos

Antes de iniciar, garante que tens os seguintes programas instalados no computador anfitrião (host):

- VirtualBox (versão 7.x ou superior)
- Vagrant (versão 2.3+)
- Ansible (versão 2.12+)

### 🪟 Instalação no Windows

No Windows, o **Ansible não corre nativamente**, pelo que a execução deve ser feita através do **WSL2 (Windows Subsystem for Linux)** ou deixando o próprio Vagrant gerir a execução do Ansible dentro das máquinas virtuais.

#### Opção A: Via winget / PowerShell (Recomendado)

1. Abre o PowerShell ou Prompt de Comando como Administrador e instala o VirtualBox e o Vagrant:
```cmd
winget install Oracle.VirtualBox
winget install HashiCorp.Vagrant
```
(Reinicia o computador após a instalação para carregar os drivers do VirtualBox e as variáveis de ambiente).

2. Instala o WSL2 (Ubuntu) para executar comandos de automação Ansible:
```DOS
wsl --install
```
3. Abre o terminal do Ubuntu no WSL2 e instala o Ansible e o Git:
```Bash
sudo apt update
sudo apt install -y git ansible
```

#### Opção B: Download Manual

Download do VirtualBox para Windows: https://www.virtualbox.org/wiki/Downloads

Download do Vagrant para Windows: https://developer.hashicorp.com/vagrant/install

### Instalação no Arch Linux:
```bash
sudo pacman -S --needed base-devel git ansible virtualbox linux-headers virtualbox-host-dkms
yay -S vagrant
```

## 🚀 Como Executar a Infraestrutura

1. Clonar este repositório:
```Bash
git clone [https://github.com/o-teu-utilizador/infra-techsolutions.git](https://github.com/o-teu-utilizador/infra-techsolutions.git)
cd infra-techsolutions
```

2. Subir as Máquinas Virtuais e aplicar o aprovisionamento:
```Bash
vagrant up
```
O Vagrant criará as VMs no VirtualBox, configurará as redes privadas e executará os playbooks do Ansible para instalar e configurar todos os serviços automaticamente[cite: 90].

3. Re-aplicar configurações (após alterações no Ansible):
```Bash
vagrant provision
```

4. Aceder via SSH às Máquinas Virtuais:
```Bash
vagrant ssh vm1-app   # Entrar na VM de Aplicação
vagrant ssh vm2-db    # Entrar na VM de Base de Dados
```

5. Destruir o ambiente (reset limpo):
```Bash
vagrant destroy -f
```

## 📁 Estrutura do Repositório

```text
.
├── Vagrantfile               # Definição das VMs, recursos e orquestração
├── README.md                 # Documentação do projeto
└── ansible/                  # Automação de Infraestrutura como Código (IaC)
    ├── ansible.cfg           # Configurações globais do Ansible
    ├── inventory             # Inventário de IPs e variáveis
    ├── site.yml              # Playbook principal
    └── roles/                # Roles modulares do Ansible
        ├── common/           # Hardening SSH e pacotes base
        ├── ufw/              # Regras de firewall segmentadas
        ├── postgres/         # Instalação e hardening do PostgreSQL
        ├── restic/           # Automação de backups cifrados
        ├── docker/           # Contentores OpenProject e Uptime Kuma
        └── nginx/            # Reverse Proxy com SSL/TLS
```
