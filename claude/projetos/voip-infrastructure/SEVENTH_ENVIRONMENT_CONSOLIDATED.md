# Ambiente SEVENTH - Consolidação Completa

**Data:** 2025-08-09  
**Ambiente:** SEVENTH (Legado CHAN-SIP)  
**Status:** ✅ **MAPEAMENTO COMPLETO**  
**Servidores:** 3 servidores AWS (GV1P + GV2P + GV3P)

---

## 🎯 RESUMO EXECUTIVO

**AMBIENTE SEVENTH** é um ecossistema completo de **Portaria Virtual** baseado em **Asterisk CHAN-SIP** que atende **130+ revendas** (empresas de segurança) com **1.300+ ramais** distribuídos em **3 servidores AWS** especializados. Cada servidor tem propósito específico e atende diferentes perfis de clientes.

---

## 🏢 MODELO DE NEGÓCIO

### **Hierarquia Comercial:**
```
VOCÊ (Provedor de Infraestrutura)
└── SEVENTH (Cliente Principal)
    └── REVENDAS (130+ empresas de segurança)
        ├── CENTRAIS/ILHAS (ring-groups de atendentes)
        └── CONDOMÍNIOS (clientes finais)
```

### **Produtos Oferecidos pela Seventh:**
1. **"Portaria na Nuvem"**
2. **"Condomínio Autônomo"**

**Ambos produtos:** Mesma infraestrutura técnica VoIP

### **Arquitetura Multi-Servidor:**
- **GV2P:** Servidor principal (volume alto)
- **GV1P:** Grandes revendas migradas (otimização)
- **GV3P:** Cliente dedicado (exclusividade)

---

## 📊 VISÃO GERAL COMPARATIVA

### **Estatísticas Consolidadas:**

| Métrica | GV2P | GV1P | GV3P | **TOTAL** |
|---------|------|------|------|-----------|
| **Revendas** | 125 | 7 (5 ativas) | 3 (1 ativa) | **130+** |
| **Ramais (.dial)** | 500+ | 305 | 573 | **1.378+** |
| **Ring-groups** | 124 | 11 | 17 | **152** |
| **Endpoints Ativos** | 410+ | 38+ | 365+ | **813+** |
| **IP Externo** | 52.67.72.145 | 18.228.54.15 | 18.228.236.165 | 3 IPs |
| **Contexto** | local | local | from-internal | Variado |

### **Propósito de Cada Servidor:**

#### **GV2P - SERVIDOR PRINCIPAL:**
- **Função:** Atendimento massivo multi-tenant
- **Perfil:** 125 revendas de pequeno/médio porte
- **Características:** Numeração padrão 7xxx, alta concorrência

#### **GV1P - GRANDES REVENDAS:**
- **Função:** Revendas que cresceram e precisavam de mais espaço
- **Perfil:** 3 revendas grandes migradas do GV2P
- **Características:** Numeração expandida, menor concorrência

#### **GV3P - CLIENTE DEDICADO:**
- **Função:** Servidor exclusivo para 1 cliente gigante
- **Perfil:** RealProtecao com 365+ condomínios
- **Características:** Numeração customizada, 17 ilhas

---

## 🔐 SISTEMA DE CONTROLE MULTI-TENANT

### **Arquitetura Unificada:**
**TODOS os 3 servidores** utilizam o **mesmo sistema de controle**:

#### **Isolamento por Variável REVENDA:**
- **Contexto comum:** Todos ramais no mesmo contexto
- **Controle:** Variável REVENDA no endpoint SIP
- **Validação:** No dialplan de cada ring-group

#### **Revendas de Suporte Global (presentes em todos):**
```
PREDIOTECH (00001): Suporte técnico Seventh
REMIAO101 (00002):  Seu suporte técnico
```
**Privilégio:** Podem acessar **todas as revendas** em **todos os servidores**

#### **Regras de Discagem:**
1. **SUPORTE (00001, 00002)** → Acesso global a tudo
2. **MESMA REVENDA** → Permite comunicação
3. **EXCEÇÕES CUSTOMIZADAS** → Definidas no dialplan
4. **DIFERENTES REVENDAS** → Bloqueia comunicação

---

## 📞 SISTEMAS DE NUMERAÇÃO

### **GV2P - NUMERAÇÃO PADRÃO:**
```
Ramais: 1000-9999 (4 dígitos)
Ring-groups: 7001-7124 (padrão 7xxx)
Revendas: 125 com ranges específicos por revenda
Atendentes: Séries 2000, 3000, 4000, 6000
Condomínios: Distribuídos conforme revenda
```

### **GV1P - NUMERAÇÃO EXPANDIDA:**
```
Ramais: 1000-9999 (4 dígitos) + série 5000 completa
Ring-groups: 7xxx + alguns especiais (95, 200x)
Revendas: 5 ativas (3 operacionais + 2 suporte)
Atendentes: Séries 2000, 3000
Condomínios: Série 1000, 5000 (314 ramais), 8000
```

### **GV3P - NUMERAÇÃO CUSTOMIZADA:**
```
Condomínios: APENAS série 4000 (4001-4365+)
Ring-groups: 1000, 1010, 1100-1900, 2000, 3000, 4000, 4295 + 7000, 7101
Ilhas: 17 ring-groups não-padrão
Atendentes: Séries 1000, 2000, 3000
Customização: Total para necessidades do cliente
```

---

## 🏆 TOP REVENDAS CONSOLIDADAS

### **Por Volume de Endpoints:**

| Posição | Servidor | Revenda | Endpoints | Ring-Group | Observação |
|---------|----------|---------|-----------|------------|------------|
| 1 | **GV3P** | **RealProtecao** | **365+** | 17 ilhas | Servidor dedicado |
| 2 | **GV2P** | **PredioTech** | 27 | 7000 | Suporte (todos servers) |
| 3 | **GV2P** | **Efetiva** | 23 | 7001 | Grande revenda |
| 4 | **GV2P** | **Epavi** | 19 | 7002 | Grande revenda |
| 5 | **GV2P** | **Heidrich** | 19 | XXXX | Grande revenda |
| 6 | **GV2P** | **LM** | 19 | 7031 | Grande revenda |
| 7 | **GV2P** | **Cindapa** | 15 | 7112 | Revenda estabelecida |
| 8 | **GV2P** | **Confianca** | 15 | 7026 | Revenda estabelecida |

### **Revendas Especiais:**
- **RealProtecao (GV3P):** Cliente único com servidor dedicado
- **PredioTech:** Suporte técnico em todos os 3 servidores
- **Remiao101:** Suporte especializado em todos os 3 servidores

---

## 🔄 ARQUITETURA DE RING-GROUPS

### **Distribuição por Servidor:**

#### **GV2P - PADRÃO MASSIVO:**
- **124 ring-groups** (7001-7124)
- **1 ring-group por revenda** (padrão)
- **Numeração sequencial** organizada

#### **GV1P - OTIMIZADO:**
- **11 ring-groups** (mix de padrões)
- **Menos concorrência** entre revendas
- **Numeração híbrida** (7xxx + especiais)

#### **GV3P - CUSTOMIZADO:**
- **17 ring-groups** (1000, 1010, 1100... + 7xxx)
- **Todas para 1 cliente** (RealProtecao)
- **17 ilhas de atendimento** diferentes

### **Padrões de Ring-Groups:**
```
Padrão SEVENTH: 7xxx (7001-7124)
Especiais GV1P: 95, 200-203, 2000, 2100
Customizado GV3P: 1000-4295 (não-padrão)
```

---

## 🏗️ ARQUITETURA TÉCNICA CONSOLIDADA

### **Tecnologia Unificada:**
- **Asterisk:** CHAN-SIP em todos os 3 servidores
- **Versão:** Asterisk 15 (presumido)
- **Protocolo:** SIP tradicional porta 5060
- **Driver:** CHAN-SIP (legado/estável)

### **Configuração de Rede:**
```
GV2P: 52.67.72.145  (172.25.1.140) - Contexto: local
GV1P: 18.228.54.15  (172.25.1.102) - Contexto: local  
GV3P: 18.228.236.165 (172.25.1.147) - Contexto: from-internal
```

### **Arquitetura de Arquivos (padrão em todos):**
```
/etc/asterisk/
├── __CENTRALS__.DEF          (cadastro revendas)
├── sip.conf → endpoints_sip.def
├── extensions.conf → endpoints_dialplan.def + endpoints_dialplan_rg.def  
└── dialplan/*.dial + *.rgdial
```

### **Codecs Suportados:**
- **Audio:** ulaw, alaw, gsm, g726, g729, slin
- **Vídeo:** h264, mpeg4 (onde aplicável)

---

## 📈 ANÁLISE DE CRESCIMENTO E MIGRAÇÃO

### **Histórico de Expansão:**

#### **FASE 1 - GV2P (Servidor Original):**
- **125 revendas** cadastradas
- **Crescimento orgânico** desde 2015
- **Numeração limitada** por concorrência

#### **FASE 2 - GV1P (Expansão):**
- **Grandes revendas migradas** do GV2P
- **Numeração expandida** (série 5000)
- **Divisão de carga** para otimização

#### **FASE 3 - GV3P (Dedicado):**
- **Cliente gigante** solicita servidor próprio  
- **365+ condomínios** justificam exclusividade
- **Numeração totalmente customizada**

### **Padrão de Senhas (cronológico):**
- **2015-2018:** Senhas simples (`Nome2015`, `Nome2017`)
- **2019-2021:** Padrão corporativo (`Nome2019!`, `Nome2020!`)
- **2022-2024:** Senhas complexas (`Nome2023!`, `Nome2024!`)

---

## 🎙️ SISTEMA DE GRAVAÇÃO UNIFICADO

### **Implementação (todos os servidores):**
```asterisk
[record]     - Chamadas individuais
[record-rg]  - Chamadas ring-groups

Diretório: /monitor/YYYY/MM/DD/
Formato: internal/rg-{caller}-{dest}-{timestamp}-{uniqueid}.wav
Tecnologia: MixMonitor (Asterisk)
```

### **Volume de Gravações (estimado):**
- **GV2P:** Alto (125 revendas, 410+ endpoints)
- **GV1P:** Médio (3 revendas, 38+ endpoints)  
- **GV3P:** Muito Alto (365+ condomínios, 1 cliente)

**TOTAL:** 800+ endpoints gerando gravações diárias

---

## 🔍 FLUXO OPERACIONAL CONSOLIDADO

### **Chamada Típica no Ambiente SEVENTH:**

#### **1. ORIGEM (Condomínio):**
- Morador/porteiro liga para número da portaria
- Sistema identifica servidor (GV1P/GV2P/GV3P)

#### **2. IDENTIFICAÇÃO (Servidor):**
- Sistema identifica revenda responsável
- Valida permissões via variável REVENDA

#### **3. DISTRIBUIÇÃO (Ring-Group):**
- Ring-group específico da revenda
- Distribui para atendentes (ramais DIAL)

#### **4. ATENDIMENTO (Ilha):**
- Atendentes da revenda recebem chamada
- Gravação automática ativada

#### **5. CONTROLE (Multi-tenant):**
- Isolamento garantido entre revendas
- Suporte pode acessar qualquer revenda

### **Fluxos Específicos:**

#### **GV2P - Multi-tenant Massivo:**
```
125 revendas → 124 ring-groups → 410+ atendentes
```

#### **GV1P - Grandes Revendas:**
```
3 revendas → 11 ring-groups → 38+ atendentes
```

#### **GV3P - Cliente Dedicado:**
```
1 revenda → 17 ilhas → 100+ atendentes → 365+ condomínios
```

---

## 🛡️ SEGURANÇA E ISOLAMENTO

### **Controle Multi-Tenant:**
- ✅ **Variável REVENDA** em cada endpoint
- ✅ **Validação no dialplan** de cada ring-group
- ✅ **Isolamento total** entre revendas
- ✅ **Exceções controladas** (suporte + customizadas)

### **Revendas de Suporte:**
- ✅ **PredioTech (00001):** Acesso global (todos servidores)
- ✅ **Remiao101 (00002):** Acesso global (todos servidores)
- ✅ **Senhas consistentes** entre servidores

### **Logs e Monitoramento:**
- ✅ **Gravações automáticas** em todos servidores
- ✅ **Logs de chamadas** (CDR)
- ✅ **Controle de acesso** via REVENDA

---

## 📊 ESTATÍSTICAS FINAIS

### **Volume Total Ambiente SEVENTH:**
```
Revendas Únicas: 130+ empresas de segurança
Ramais Configurados: 1.378+ extensões
Ring-Groups: 152 grupos de chamadas
Endpoints Ativos: 813+ ramais operacionais
Condomínios: 500+ condomínios atendidos
Ilhas de Atendimento: 150+ ilhas distribuídas
Gravações Diárias: Milhares de arquivos
```

### **Distribuição por Servidor:**
- **GV2P:** 89% das revendas (125/130+)
- **GV1P:** 6% das revendas (5/130+) + suporte
- **GV3P:** 5% das revendas (1/130+) + suporte

### **Crescimento e Capacidade:**
- **Capacidade atual:** 1.378+ ramais configurados
- **Utilização:** ~60% (813/1.378)
- **Margem crescimento:** 40% disponível
- **Escalabilidade:** Novos servidores conforme demanda

---

## 🔧 MANUTENÇÃO E OPERAÇÃO

### **Padrões Estabelecidos:**
1. **Numeração:** 4 dígitos (1000-9999)
2. **Ring-groups:** Preferencialmente 7xxx
3. **Revendas suporte:** Sempre presentes
4. **Variável REVENDA:** Controle obrigatório
5. **Gravações:** Automáticas em todos ring-groups

### **Exceções Documentadas:**
- **GV3P:** Numeração customizada total
- **GV1P:** Ring-groups especiais (não-7xxx)
- **Clientes especiais:** Configurações sob demanda

### **Procedimentos Padrão:**
1. **Nova revenda:** Cadastro no __CENTRALS__.DEF
2. **Novos ramais:** Arquivos .dial individuais
3. **Novos ring-groups:** Arquivos .rgdial + include
4. **Migração:** Mover configurações entre servidores
5. **Suporte:** Sempre manter PredioTech/Remiao101

---

## ⚠️ LIMITAÇÕES E DESAFIOS IDENTIFICADOS

### **1. SISTEMA OPERACIONAL (CentOS EOL):**
- ⚠️ **CentOS Linux:** Descontinuado (não mais gratuito)
- ⚠️ **Red Hat:** Opção paga (custos elevados)
- ⚠️ **Migração necessária:** Para Rocky Linux, AlmaLinux ou Ubuntu Server
- ⚠️ **Dependências:** Asterisk CHAN-SIP + bibliotecas específicas
- ⚠️ **Risco:** Falta de atualizações de segurança futuras

### **2. ESGOTAMENTO DE NUMERAÇÃO (4 dígitos):**
- ⚠️ **Range limitado:** 1000-9999 (apenas 9.000 ramais totais)
- ⚠️ **Distribuição restrita:** Pequenos blocos por revenda
- ⚠️ **Crescimento bloqueado:** Impossível expandir revendas grandes
- ⚠️ **Fragmentação:** Números dispersos, difícil gerenciamento
- ⚠️ **Conflitos:** Necessidade de migração entre servidores (GV2P→GV1P)

### **3. ARQUITETURA LEGADA (CHAN-SIP):**
- ⚠️ **Driver descontinuado:** CHAN-SIP deprecated no Asterisk
- ⚠️ **Sem WebRTC nativo:** Limitado a dispositivos SIP tradicionais
- ⚠️ **Configuração complexa:** NAT, firewall, RTP ranges
- ⚠️ **Manutenção difícil:** Conhecimento específico necessário
- ⚠️ **Futuro incerto:** Trend para PJSIP + WebRTC

### **4. ESCALABILIDADE LIMITADA:**
- ⚠️ **3 servidores:** Divisão artificial por limitação numérica
- ⚠️ **Configurações duplicadas:** Manutenção em triplo
- ⚠️ **Complexidade operacional:** Diferentes padrões por servidor
- ⚠️ **Crescimento custoso:** Novos servidores para contornar limites

## 🎯 CONCLUSÕES E JUSTIFICATIVAS

### **Por que Environment TECNOFY foi Necessário:**
Todos esses **limitações críticas** justificaram a criação de uma **nova infraestrutura moderna**:

#### **1. Migração Tecnológica:**
- ✅ **Linux moderno:** Ubuntu Server (suporte longo prazo)
- ✅ **Asterisk PJSIP:** Driver moderno e suportado
- ✅ **WebRTC nativo:** Acesso via browser + certificação SSL
- ✅ **Dual connectivity:** PJSIP + WebRTC simultâneos

#### **2. Numeração Expandida:**
- ✅ **Numeração livre:** Não limitado a 4 dígitos
- ✅ **Crescimento ilimitado:** Ranges flexíveis por revenda
- ✅ **Servidor único:** Não necessita divisão artificial
- ✅ **Gerenciamento simples:** Configuração centralizada

#### **3. Futuro Sustentável:**
- ✅ **Tecnologia atual:** PJSIP + WebRTC (padrão mercado)
- ✅ **OS supportado:** Ubuntu LTS com atualizações garantidas
- ✅ **Escalabilidade real:** Crescimento horizontal natural
- ✅ **Manutenção moderna:** Ferramentas atuais, documentação abundante

### **Ambiente SEVENTH - Características (Revisadas):**
- ✅ **Infraestrutura funcional:** CHAN-SIP estável **porém limitada**
- ✅ **Multi-tenant eficiente:** 130+ revendas isoladas **mas numeração esgotada**
- ⚠️ **Escalabilidade limitada:** 3 servidores **por necessidade, não escolha**
- ⚠️ **Flexibilidade restrita:** Customização **limitada pela numeração**
- ⚠️ **Tecnologia EOL:** CentOS + CHAN-SIP **descontinuados**

### **Modelo de Negócio (Análise Crítica):**
- ✅ **130+ empresas de segurança** atendidas **com dificuldade de crescimento**
- ✅ **Portaria Virtual** funcionando **mas limitada tecnologicamente**
- ✅ **Modelo B2B2C** validado **porém precisa modernização**
- ⚠️ **Receita limitada** por impossibilidade de crescer revendas grandes
- ⚠️ **Crescimento insustentável** devido a limitações técnicas

### **Decisão Estratégica TECNOFY:**
**Environment TECNOFY** foi criado como **evolução natural** para resolver **todas as limitações identificadas** no ambiente SEVENTH, permitindo:
- **Crescimento sustentável** sem limites de numeração
- **Tecnologia moderna** (PJSIP + WebRTC)
- **Sistema operacional suportado** (Ubuntu LTS)
- **Arquitetura escalável** (servidor único, expansão horizontal)
- **Futuro garantido** com tecnologias atuais do mercado

---

## 🚀 STATUS FINAL

**AMBIENTE SEVENTH COMPLETAMENTE MAPEADO E DOCUMENTADO:**

- ✅ **3 servidores** analisados (GV1P + GV2P + GV3P)
- ✅ **130+ revendas** catalogadas
- ✅ **1.378+ ramais** documentados  
- ✅ **152 ring-groups** mapeados
- ✅ **Sistema multi-tenant** compreendido
- ✅ **Arquitetura técnica** consolidada
- ✅ **Modelo de negócio** estabelecido
- ✅ **Fluxos operacionais** definidos

---

**Documentação gerada em:** 2025-08-09  
**Responsável:** Claude Code  
**Ambiente:** SEVENTH (Legado CHAN-SIP)  
**Status:** ✅ **CONSOLIDAÇÃO COMPLETA**  

**Próximo ambiente:** TECNOFY (Moderno PJSIP + WebRTC)