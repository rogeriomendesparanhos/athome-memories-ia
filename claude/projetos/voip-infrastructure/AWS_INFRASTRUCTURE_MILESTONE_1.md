# AWS Infrastructure - MILESTONE 1: Mapeamento Completo

**Data:** 2025-08-08  
**Status:** ✅ **CONCLUÍDO**  
**Objetivo:** Descoberta e catalogação completa da infraestrutura AWS

---

## 🎯 RESUMO EXECUTIVO

Mapeamento completo de **8 instâncias EC2**, **11 volumes EBS**, **2 VPCs distintas** e **2 ambientes de clientes** (SEVENTH e TECNOFY) para sistemas de **Portaria Virtual** baseados em Asterisk.

---

## 🏗️ ARQUITETURA DESCOBERTA

### **AMBIENTE SEVENTH (Legado/Tradicional)**
- **Cliente:** Seventh (Portaria Virtual)
- **VPC:** `vpc-e85e2a8d` 
- **Tecnologia:** **CHAN-SIP** (driver SIP antigo)
- **Conectividade:** SIP tradicional porta **5060**
- **Security Group:** "Paranhos" - Porta 5066 + IPs específicos

#### **Instâncias (4):**
| Nome | Tipo | Status | IP Público | IP Privado | Storage |
|------|------|--------|------------|------------|---------|
| **GV_1P** | t3.micro | ✅ running | 18.228.54.15 | 172.25.1.102 | 50GB + 250GB |
| **GV_2P** | t3.small | ✅ running | 52.67.72.145 | 172.25.1.140 | 50GB + 250GB |
| **GV_3P** | t3.small | ✅ running | 18.228.236.165 | 172.25.1.147 | 100GB + 60GB |
| **AUDIO** | t3.nano | ✅ running | 54.232.201.186 | 172.25.1.143 | **500GB** |

### **AMBIENTE TECNOFY (Moderno)**
- **Cliente:** Tecnofy (Portaria Virtual)
- **VPC:** `vpc-0f125b724af1100b0`
- **Tecnologia:** **PJSIP** (driver SIP moderno)
- **Conectividade MÚLTIPLA:**
  - **PJSIP puro** → Porta **5060** (dispositivos SIP)
  - **WebRTC + Certificação Digital** → Portas **8088/8089** (browsers)
- **Security Group:** "WEBRTC NOVO" - Portas 8088/8089 TCP/UDP abertas

#### **Instâncias (4):**
| Nome | Tipo | Status | IP Público | IP Privado | Storage | Função |
|------|------|--------|------------|------------|---------|---------|
| **VOIP1** | t3.medium | ✅ running | 54.207.156.75 | 172.31.8.89 | 100GB | Asterisk Principal |
| **VOIP2** | t3.micro | ⏸️ stopped | 54.232.235.128 | 172.31.8.20 | 100GB | Standby |
| **PILOTO** | t3.nano | ⏸️ stopped | NULL | 172.31.14.5 | 50GB | Testes |
| **CLAUDE-CODE** | t3.medium | ⏸️ stopped | 52.67.178.193 | 172.31.9.184 | 50GB | **Desenvolvimento** |

---

## 🔄 RECURSO COMPARTILHADO

### **SERVER AUDIO - Centralizador de Gravações**
- **Localização:** Ambiente SEVENTH (físico)
- **Função:** Recebe e armazena gravações de **AMBOS ambientes**
- **Storage:** **500GB** (maior volume - para gravações)
- **Tecnologia:** Apache HTTP + Asterisk + WebRTC Softphone
- **Futuro:** Plano para criar servidor AUDIO dedicado no ambiente TECNOFY

#### **Fluxo de Gravações:**
```
SEVENTH: [GV_1P, GV_2P, GV_3P] ──┐
                                  ├──> [SERVER AUDIO] ──> Armazena gravações
TECNOFY: [VOIP1, VOIP2, PILOTO] ──┘
```

---

## 🛡️ SECURITY GROUPS MAPEADOS

### **Paranhos (SEVENTH)**
- **ID:** `sg-0b59945e6d1b82d61`
- **Regras Inbound:**
  - **TCP 5066** → 0.0.0.0/0 (SIP público)
  - **All Traffic** → IPs específicos:
    - `54.232.201.186/32` (próprio AUDIO)
    - `189.6.241.29/32`, `191.39.67.23/32` (clientes)
    - `172.31.0.0/20` (rede interna)

### **WEBRTC NOVO (TECNOFY)**
- **ID:** `sg-088fde7d109a4b14b`
- **Regras Inbound:**
  - **TCP/UDP 8088** → 0.0.0.0/0 (WebRTC)
  - **TCP/UDP 8089** → 0.0.0.0/0 (WebRTC)

---

## 💾 STORAGE ANALYSIS

### **Volumes EBS Mapeados (11 volumes):**

#### **Volumes de Sistema (OS):**
- **SEVENTH:** 50-100GB por instância
- **TECNOFY:** 50-100GB por instância (gp3 moderno)

#### **Volumes de Dados:**
| Instância | Volume Dados | Tamanho | Finalidade |
|-----------|-------------|---------|------------|
| **AUDIO** | vol-04375b998a559448c | **500GB** | 🎵 **Gravações centralizadas** |
| **GV_1P** | vol-0f38c17cf3ba162b4 | 250GB | Dados operacionais |
| **GV_2P** | vol-06eb926e4176ed3ed | 250GB | Dados operacionais |
| **GV_3P** | vol-0a95771357d17aa02 | 60GB | Dados operacionais |

---

## 🔧 TECNOLOGIAS IDENTIFICADAS

### **SEVENTH (Ambiente Legado):**
- **Asterisk:** CHAN-SIP driver
- **Protocolo:** SIP tradicional
- **Porta:** 5060 (padrão SIP)
- **Características:** Configuração tradicional, estável

### **TECNOFY (Ambiente Moderno):**
- **Asterisk:** PJSIP driver (moderno)
- **Protocolos:**
  - **PJSIP** → Porta 5060 (compatível com SIP padrão)
  - **WebRTC** → Portas 8088/8089 + certificação SSL
- **Características:** Dual connectivity, browser-friendly

### **Servidor AUDIO:**
- **Apache HTTP Server 2.4.6** (CentOS 7.8)
- **Asterisk PBX**
- **WebRTC Softphone** (phone.html, phone_ios.html)
- **Sistema de Agenda:** 10 botões configuráveis
- **Gravação:** Semi-automática (cross-platform)

---

## 📊 BANCO DE DADOS CRIADO

### **Estrutura `aws-servers` Database:**

#### **Tabela: `ec2_instances`**
- 8 instâncias catalogadas
- Campos: instance_id, name, type, state, IPs, VPC, security_groups, environment

#### **Tabela: `storage`**
- 11 volumes EBS mapeados
- Campos: resource_id, type, size_gb, attached_instance, name

#### **Tabela: `security_groups`**
- 2 grupos documentados
- Campos: group_id, group_name, description, vpc_id

#### **Tabela: `security_rules`**
- 8+ regras catalogadas
- Campos: group_id, protocol, ports, source_cidr, rule_type

#### **Tabela: `services`**
- Estrutura criada para próxima fase
- Campos: instance_id, service_name, port, protocol, status, version

---

## 📈 ESTATÍSTICAS

### **Instâncias por Status:**
- ✅ **Running:** 5 instâncias (62.5%)
- ⏸️ **Stopped:** 3 instâncias (37.5%)

### **Distribuição por Ambiente:**
- **SEVENTH:** 4 instâncias (50%)
- **TECNOFY:** 4 instâncias (50%)

### **Storage Total:**
- **Total EBS:** 1,320GB across 11 volumes
- **Maior volume:** 500GB (AUDIO - gravações)
- **Storage moderno:** gp3 (TECNOFY), gp2 (SEVENTH)

### **Conectividade:**
- **IPs Públicos:** 5 ativos, 3 não alocados (stopped)
- **VPCs:** 2 ambientes isolados
- **Security Groups:** 2 perfis distintos

---

## 🎯 PRÓXIMAS FASES

### **FASE 2: Detalhamento Individual dos Servidores**
- [ ] **VOIP1** (TECNOFY) - Asterisk principal moderno
- [ ] **GV_1P, GV_2P, GV_3P** (SEVENTH) - Asterisk legado
- [ ] **VOIP2, PILOTO** (TECNOFY) - Servidores inativos
- [ ] **CLAUDE-CODE** (TECNOFY) - Ambiente de desenvolvimento

### **FASE 3: Análise de Serviços**
- [ ] Scan de portas em instâncias ativas
- [ ] Descoberta de versões Asterisk
- [ ] Mapeamento de extensões SIP
- [ ] Análise de configurações

### **FASE 4: Sistema de Gerenciamento**
- [ ] Interface de monitoramento
- [ ] Scripts de automação
- [ ] Dashboards operacionais
- [ ] Alertas e métricas

---

## 🔄 FERRAMENTAS UTILIZADAS

### **Descoberta AWS:**
- **AWS CLI 2.28.6** com usuário `claude-code-user1`
- **Permissões:** ReadOnlyAccess (segurança)
- **Comandos:** describe-instances, describe-volumes, describe-security-groups

### **Armazenamento Local:**
- **MariaDB** na WSL Windows 11
- **Database:** `aws-servers`
- **Usuário:** `claude_aws` (sem senha - ambiente controlado)

### **Documentação:**
- **Arquivos:** `/mnt/c/claudedoc/` (sistema estruturado)
- **Versionamento:** Markdown com timestamps
- **Integração:** Claude Code + WSL + SSH servers

---

## ✅ VALIDAÇÕES REALIZADAS

### **Conectividade:**
- ✅ SSH para servers audio/voip1 (já existente)
- ✅ AWS CLI configurado e funcional
- ✅ MariaDB operacional com dados inseridos

### **Consistência:**
- ✅ IPs públicos conferem com security groups
- ✅ Volumes EBS vinculados corretamente às instâncias  
- ✅ VPCs e subnets mapeados consistentemente
- ✅ Ambientes classificados corretamente (SEVENTH/TECNOFY)

### **Documentação:**
- ✅ Milestone consolidado
- ✅ Banco de dados estruturado
- ✅ Próximas fases planejadas

---

**CONCLUSÃO:** Infraestrutura AWS completamente mapeada e catalogada. Sistema pronto para análise detalhada de serviços individuais e desenvolvimento de ferramentas de gerenciamento.

**Próximo Milestone:** Detalhamento do primeiro servidor VoIP (VOIP1 - TECNOFY)