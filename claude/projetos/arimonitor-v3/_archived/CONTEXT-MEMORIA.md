# ARIMONITOR - CONTEXTO E MEMÓRIA DO PROJETO

## **CONEXÃO FUNDAMENTAL COM INFRAESTRUTURA MAPEADA** 🔗

### **IMPORTANTE**: 
O sistema ARIMONITOR opera sobre a **MESMA INFRAESTRUTURA** que mapeamos anteriormente no servidor VOIP1.

### **CLIENTES/REVENDAS** (dados reais mapeados):
- **112**: PV - Residencial Vila Madalena 
- **113**: [outros clientes mapeados]
- **114**: [outros clientes mapeados]
- Etc...

### **ENDPOINTS/RAMAIS** (dados reais mapeados):
- **1121, 1122**: Ramais do cliente 112
- **1131, 1132**: Ramais do cliente 113  
- **1141, 1142**: Ramais do cliente 114
- Etc...

### **ARQUIVOS DE CONFIGURAÇÃO**:
- **`__CLIENTES__.DEF`**: O MESMO arquivo que estava em `/etc/asterisk/` no VOIP1
- **`endpoints_pjsip.def`**: As MESMAS configurações PJSIP que analisamos
- **Ring-groups, atendentes**: A MESMA estrutura organizacional

### **FLUXO LÓGICO**:
```
Infraestrutura VOIP1 (mapeada anteriormente)
    ↓
Arquivos de configuração (clientes, endpoints, pjsip)  
    ↓
ARIMONITOR carrega na memória (CLIENTE{}, ENDPOINT{})
    ↓
Monitora eventos ARI desses ramais específicos
    ↓
Envia informações para sistema web Tecnofy via RabbitMQ
```

### **PROPÓSITO DO ARIMONITOR**:
Monitorar e processar as chamadas dos **clientes e ramais reais** que conhecemos, alimentando o sistema web da Tecnofy com informações detalhadas de cada chamada.

---

---

## **ANÁLISE CONCLUÍDA - MÓDULO ARIMONITOR.PY** ✅

### **PARTE 1 - INICIALIZAÇÃO** (Concluída):
1. **Configuração inicial**: logs, PID, timestamps
2. **Redirecionamento saída**: classe Tee com rotação
3. **Criação diretórios**: estrutura ARIDIRECTOR
4. **Console interativo**: thread Keyboard em background
5. **Detecção IPs**: AutoIPFix para correção automática PJSIP
6. **Health Check**: aguarda ARI/AMI do Asterisk (portas 8088/5038)
7. **Carregamento config**: clientes, endpoints, MQServers (modo TEXT-BASED)
8. **Serviços auxiliares**: service.run + Manager Socket Listener
9. **TRANSFERÊNCIA CONTROLE**: instancia Client WebSocket → ws.py

### **PARTE 2 - CONSOLE COMANDOS** (Adiada para migração Python 3):
- **Status**: Extremamente prolixo e ineficiente
- **Ação**: Reestruturação completa durante migração
- **Foco**: Arquitetura moderna de CLI

### **OPORTUNIDADES PYTHON 3 IDENTIFICADAS**:
- string.replace() → str.replace() + f-strings
- os.system() → subprocess para segurança
- Instanciação global → dependency injection
- Configurações hardcoded → environment variables
- Modo híbrido: TEXT-BASED + REALTIME (MariaDB)
- CLI moderno para console de comandos
- Logging estruturado
- Type hints e docstrings
- Error handling robusto

---

**CRIADO**: 2025-08-12  
**ÚLTIMA ATUALIZAÇÃO**: 2025-08-12  
**MOTIVO**: Manter contexto conectado entre análise de infraestrutura e análise do sistema ARIMONITOR  
**STATUS**: ✅ Análise arimonitor.py + ws.py (loop eventos) concluída - Pronto para próximos módulos

---

## **ESTRUTURA DOS JSONS MQ - SISTEMA TECNOFY** 📋

### **EVENTOS DE CHAMADAS** (event_code: 8000):

#### **1. RECEIVED-CALL (channelstatus: "open")**:
- **Quando**: Após DIALEDPEERNUMBER confirmar ring válido
- **Dados**: origin, destiny, tipos de ramal, callnumber, guid único
- **Finalidade**: Sistema web recebe "chamada iniciada"

#### **2. RECEIVED-CALL (channelstatus: "oncall")**:
- **Quando**: Após Dial(ANSWER) confirmar atendimento
- **Dados**: + extra_channel, ringduration calculado
- **Finalidade**: Sistema web recebe "chamada atendida"

#### **3. RECEIVED-CALL (channelstatus: "closed")**:
- **Quando**: Após ChannelDestroyed finalizar chamada
- **Dados**: + callduration, urlfilename (MP3), dtmf acumulado
- **Finalidade**: Sistema web recebe "chamada completa com áudio"

### **EVENTOS DE GERENCIAMENTO**:
- **8001**: client-create/change/delete
- **8002**: endpoint-create/change 
- **8007**: endpoint-delete
- **8008**: ip-banned (segurança)
- **8010-8012**: ia-create/change/delete (ramais IA)
- **8013**: endpoint-status (PeerStatusChange)
- **8014**: rgrule-create (regras ring-group)

### **ESTRUTURA PADRÃO**:
```json
{
    "operation": {
        "type": "received-call",
        "event_code": "8000", 
        "timestamp": unix_timestamp,
        "vhost": "cliente_vhost"
    },
    "data": {
        "origin": "1121",
        "destiny": "1131", 
        "origin_type": "unidade",
        "destiny_type": "apto",
        "callnumber": 123,
        "guid": "uuid-v7-único",
        "channelstatus": "open|oncall|closed",
        "urlfilename": "http://audio.tecnofy.com.br/voip1/arquivo.mp3"
    }
}
```

---

## **CLARIFICAÇÃO FUNDAMENTAL - ARIMONITOR vs MANAGER** ⚠️

### **ARIMONITOR** (sistema que estamos analisando):
- **Função**: MONITORAMENTO PASSIVO de chamadas
- **JSONs tratados**: 
  - ✅ received-call (8000): open/oncall/closed
  - ✅ clicktocall (8001): chamadas via web
- **Papel**: "OUVIDO" - observa e reporta eventos

### **MANAGER v2.7** (script separado - próxima análise):
- **Função**: GERENCIAMENTO ATIVO da infraestrutura
- **JSONs tratados**: Todos os demais (create/change/delete)
- **Papel**: "BRAÇO EXECUTOR" - recebe comandos web e modifica Asterisk
- **Status**: Já implementa MODE HÍBRIDO (TEXT-BASED + REALTIME)

### **FLUXO CORRETO**:
```
Sistema Web → MANAGER v2.7 → Modifica Asterisk
              ↓
           ARIMONITOR → Monitora chamadas → Sistema Web
```

---

## **HISTÓRICO DA SESSÃO 2025-08-12** 📋

### **TRABALHO REALIZADO**:
1. ✅ **Análise completa arimonitor.py** - Inicialização e transferência de controle
2. ✅ **Análise ws.py** - Core WebSocket e loop de eventos
3. ✅ **Explicação @gen.coroutine + yield** - Programação assíncrona Tornado
4. ✅ **Mapeamento eventos ARI** - ChannelCreated → ChannelDestroyed
5. ✅ **Análise JSONs MQ** - Estrutura completa dos eventos 8000
6. ✅ **Conexão com infraestrutura** - Clientes 112,113,114 e ramais 1121,1122...
7. ✅ **Clarificação ARIMONITOR vs MANAGER** - Papéis distintos

### **PRÓXIMOS PASSOS**:
- Análise módulos restantes: ari_events.py, send_mq.py, recovery.py, etc.
- Análise MANAGER v2.7 (após ARIMONITOR)
- Migração Python 3 com sistema híbrido

### **CONHECIMENTO CONSOLIDADO**:
- Entendimento completo do fluxo de chamadas VoIP
- Arquitetura assíncrona com Tornado
- Integração RabbitMQ e sistema web Tecnofy
- Diferenciação entre monitoramento e gerenciamento
- **ARQUITETURA CÓDIGOS**: Cliente=3 dígitos, Endpoint=7 dígitos (Cliente+4)
- **MELHORIAS PYTHON 3**: 
  - UUID v7: Python 3.12+ tem `uuid.uuid7()` nativo (substituir generate_uuidv7())
  - Manter documentação em comentários do código-fonte
  - Combinar comentários + informações CLI para análise completa

## **CHANNEL_CREATED - LÓGICA DETALHADA** 📋

### **VALIDAÇÕES ESTRATÉGICAS**:
1. **exten="s"**: DESCARTA - Só quer o evento do ramal CHAMADOR (controla toda relação)
2. **caller.number vazio**: DESCARTA - Não é chamada válida
3. **"OutgoingSpoolFailed"**: DESCARTA - Erro PJSIP (investigar no ClickToCall)

### **ARQUITETURA DE RASTREAMENTO**:
- **creationtime**: CHAVE ÚNICA para correlacionar TODOS eventos da chamada
- **Canal chamador**: Controla toda a sequência de eventos
- **Canal atendente**: Descoberto depois em outros eventos e atualizado no JSON

### **DOIS DICIONÁRIOS EM MEMÓRIA**:
- **CALLS{}**: Chamadas ATIVAS (limpas ao encerrar)
- **XCallnumber{}**: HISTÓRICO completo em memória (todas as chamadas)

### **ESTRATÉGIA ANTI-SPAM MQ**:
- **ChannelCreated**: Cria JSON mas NÃO ENVIA ao MQ ainda
- **Envio real**: Apenas após validação em eventos posteriores
- **Exceção IA**: Ramal 94 envia IMEDIATAMENTE (sistema confiável)

### **SISTEMA IA INTEGRADO**:
- **Ramal 94**: Entrada fixa no dialplan dinâmico para testes IA
- **Sistema IA**: Parte integrada do ecossistema Tecnofy (detalhar depois)

## **DIAL() - ATENDIMENTO CONFIRMADO** 📞

### **CORREÇÃO IMPORTANTE**:
- **NÃO é o primeiro MQ**: Existe evento "OPEN" enviado ANTES (ainda não analisado)
- **ONCALL**: Segundo evento MQ quando chamada é atendida
- **Sequência**: OPEN → ONCALL → CLOSED

### **RING-GROUPS INTELIGENTES**:
- **JSON inicial**: destiny = ring-group
- **Após ANSWER**: destiny = ramal que realmente atendeu
- **Resultado**: Rastreamento exato de quem atendeu

### **NOTA PYTHON 3**:
- **Código atual**: Funcional e robusto (anos em produção)
- **Meta Python 3**: Reimplementar com elegância moderna mantendo funcionalidade
- **Prioridade**: Funcionalidade > elegância (sempre!)

## **DIALEDPEERNUMBER() - PRIMEIRO ENVIO MQ** 📡

### **SEQUÊNCIA MQ COMPLETA**:
1. **OPEN**: `dialedpeernumber()` - callstatus=1, channelstatus="open" (tocando)
2. **ONCALL**: `dial()` - callstatus=2, channelstatus="oncall" (atendida)
3. **CLOSED**: `channel_destroyed()` - channelstatus="closed/unanswered" (finalizada)

### **ESTRATÉGIA ANTI-SPAM PERFEITA**:
- `channel_created()`: Cria JSON mas NÃO envia
- `dialedpeernumber()`: VALIDA chamada legítima → PRIMEIRO envio MQ
- Resultado: MQ recebe apenas chamadas reais, não ataques

## **ENDPOINT_TYPE - ARQUITETURA INTELIGENTE** 🏗️

### **TIPOS ESPECIAIS COM TRATAMENTO DIFERENCIADO**:
- **"ring-group"**: Grupos de ramais (tratamento especial ring-groups)
- **"ring-member"**: Membros de ring-groups
- **"ia"**: Ramais de Inteligência Artificial
- **"unidade"**: Apps smartphone (iOS/Android) via WebRTC

### **SISTEMA PUSH PARA SMARTPHONES** 📱:
- **Detecção**: Ramal "unidade" offline/não registrado
- **Ação**: Envia evento PUSH via MQ
- **Resultado**: Sistema web Tecnofy "acorda" app smartphone
- **Fluxo**: App registra → Chamada sai da fila → Prossegue normalmente
- **Tecnologia**: WebRTC para apps móveis (por isso WebRTC nos servers novos)

## **SEND_MQ.PY - PROCESSAMENTO FINAL** 📨

### **LIMPEZA DE LEGADO**:
- **"update"**: Status não usado na versão atual (legado Seventh)
- **REG730/REG700**: Nomenclatura antiga (não atual)

### **ARQUITETURA VHOSTS MQ** 🏗️:
- **vhost="voip"**: Filas para servidores VoIP (consumidas pelos SIP servers)
- **vhost="cliente"**: Um vhost por revenda/cliente
- **Envio**: Registros sempre vão para vhost da revenda
- **ClickToCall**: Usa filas do vhost "voip" (analisar depois)
- **Manager**: Também usa vhost "voip" (analisar depois)

### **PROCESSAMENTO DE ÁUDIO** 🎵:
- **FTPWAV="Y"**: SSH remoto (SCP + conversão no servidor de áudio)
- **FTPWAV="N"**: Local + FTP (LAME local + FTP do MP3)
- **URL final**: `http://audio.tecnofy.com.br/voip1/arquivo.mp3`

### **MELHORIA PYTHON 3** 🎯:
- **Teste Audio2**: Implementar envio para servidor audio2.tecnofy.com.br
- **Load balancing**: Distribuir áudios entre audio original + audio2
- **Configuração**: Via environment variables ou config híbrido

## **RECOVERY.PY - RECUPERAÇÃO DE CHAMADAS PERDIDAS** 🔄

### **FUNÇÃO PRINCIPAL**:
- **Recuperação automática**: Após reconexão WebSocket
- **Recuperação manual**: Via comando específico do console
- **Detecção inteligente**: Verifica se arquivo WAV ainda está em uso (`fuser`)

### **PROBLEMAS CRÍTICOS IDENTIFICADOS** ⚠️:

#### **1. FORMATO DE EVENTO OBSOLETO**:
- **Atual**: Usa `event["payload"]` (formato antigo)
- **Correto**: Deveria usar `event["data"]` (formato atual ARI)
- **Incompatibilidade**: Não funciona com eventos atuais do sistema

#### **2. PUBLISHERS NÃO INICIALIZADOS**:
```python
# ❌ ERRO: Publishers inexistentes
self.publisher_pnn.send_message(event)
self.publisher_auto.send_message(event)
# ✅ CORRETO: Usar publisher inicializado
self.publisher_any.send_message(event)
```

#### **3. CAMPOS DE STATUS INCORRETOS**:
- **channelStatus** → deveria ser **channelstatus** (minúsculo)
- **callstatus = 4** → status não existe no sistema atual (usar 1 ou 2)

### **FLUXO DE RECUPERAÇÃO**:
1. **Detecção**: Chamadas com `status = "lost"` em `Client.calls`
2. **Verificação**: Comando `fuser` para verificar se WAV ainda está ativo
3. **Processamento**: SSH + LAME remoto (mesmo padrão `send_mq.py`)
4. **Envio MQ**: Evento "FORCECLOSE" para sistema web
5. **Limpeza**: Remove de `Client.calls` e marca XCallnumber como "offrec"

### **INTEGRAÇÃO COM SISTEMA**:
- **Acionado por**: `ws.py` após reconexão WebSocket
- **Usa dados de**: `Client.calls` (memória compartilhada)
- **Atualiza**: `XCallnumber` e contadores `ARIMONITOR`

### **MELHORIAS PYTHON 3** 🎯:
- **Async/await**: Processamento assíncrono de múltiplas recuperações
- **Subprocess**: Substituir `os.system()` por `subprocess.run()`
- **Pathlib**: Manipulação moderna de caminhos de arquivo
- **Type hints**: Documentação explícita de tipos
- **Logging estruturado**: Melhor rastreabilidade de recuperações

## **SERVICE.PY - SERVIÇOS E INTEGRAÇÕES** ⚙️

### **FUNÇÃO PRINCIPAL**:
- **Loop de serviços**: Executa tarefas periódicas (WhiteList, ClickToCall, Snapshots)
- **Comunicação MANAGER**: Socket TCP 1101 para receber novos clientes/endpoints
- **Segurança automática**: Integração fail2ban para whitelist dinâmica

### **INTEGRAÇÕES CRÍTICAS** 🔗:
#### **1. SOCKET MANAGER → ARIMONITOR**:
- **Porta 1101**: Recebe comandos do MANAGER v2.7
- **Eventos processados**:
  - `manager-create-endpoint`: Adiciona endpoint dinamicamente
  - `manager-create-client`: Adiciona cliente dinamicamente
- **Sem reinicialização**: Sistema atualiza tabelas em runtime

#### **2. CLICKTOCALL SERVICE**:
```python
ARIMONITOR['ClicktoCall_Service_Alive'] = threading.Thread(target=self.clicktocall.run_once)
# Processa chamadas originadas via sistema web
```

#### **3. WHITELIST AUTOMÁTICA**:
```python
cmd='fail2ban-client set asterisk-iptables addignoreip '+ip
# Adiciona IPs de clientes à whitelist fail2ban gradualmente
```

### **PROBLEMAS IDENTIFICADOS** ⚠️:
- **ast.literal_eval()**: Risco de segurança (eval de código)
- **Socket sem tratamento**: Falta try/except para falhas de conexão
- **Logs verbosos**: Impacto na performance

## **MONITOR.PY - MONITORAMENTO E DEBUG** 📊

### **FUNÇÃO PRINCIPAL**:
- **Monitoramento ramais**: Eventos `PeerStatusChange` (Registered/Unreachable/etc.)
- **Sistema debug avançado**: Múltiplos modos de debug configuráveis
- **Auto-descoberta IPs**: Detecta automaticamente IPs de novos ramais

### **FILTRAGEM INTELIGENTE** 🎯:
#### **CONTROLE DE EVENTOS**:
```python
# DATA.event_types[event_type]:
# 'N' = descarta, 'Y' = processa, 'P' = print+processa, 'NP' = print+descarta
if DATA.event_types[event['type']] == 'N': return('continue')
```

#### **SUB-TIPOS ChannelVarset**:
```python
# DATA.channelvarset_subtypes[variable] para controle granular
if event["type"] == "ChannelVarset":
    varset = event["variable"]
    # Processa conforme configuração específica da variável
```

### **DESCOBERTA AUTOMÁTICA DE IPs** 🔍:
```python
if not ip in REMOTEIPS.keys():
    REMOTEIPS[ip] = {
        "dns": '',
        "whitelist": 'N', 
        "dateinserted": timestamp
    }
    # IP detectado será processado pelo service.py para whitelist
```

### **SISTEMA DEBUG AVANÇADO**:
- **'Y'**: JSON completo no console
- **'D'**: Log estruturado com rotação diária  
- **'R'**: Print resumido com campos relevantes
- **'S'**: Filtragem seletiva de eventos

### **LEGACY REG707** (comentado):
- Estrutura antiga de monitoramento MQ
- Não compatível com formato atual (8000-8014)
- Preparado para futura reativação se necessário

**STATUS**: ✅ Análise completa ARIMONITOR v2.7: arimonitor.py + ws.py + ari_events.py + send_mq.py + recovery.py + service.py + monitor.py - Sessão 2025-08-13