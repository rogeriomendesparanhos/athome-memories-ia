# ANALISE COMPLETA DOS SISTEMAS EXISTENTES

**Data**: 10/11/2025
**Objetivo**: Entender 100% como cada sistema funciona HOJE antes de criar repo unificado

---

## SERVIDOR AUDIO (Python 2.7)

### Localizacao
`/root/util/`

### Arquivos Principais

#### 1. monitor_emails.py (v1.1)
**Linhas**: 766
**Data**: 31/10/2025
**Funcao**: Monitor IMAP + Sistema de Filas INTEGRADO

**Funcionalidades**:
- Conecta IMAP Gmail
- Busca emails nao lidos (UNSEEN)
- Valida remetente (whitelist)
- Parse subject: `RAMAIS=X,Y INICIO=DD/MM/YYYY HH:MM FIM=DD/MM/YYYY HH:MM`
- Valida parametros (limite ramais, range datas)
- Envia email confirmacao
- **ADICIONA NA FILA** (pending/)
- **PROCESSA FILA** no mesmo script (chama selecionar_audios_v3.4.py)
- Envia email resultado ou no-files

**Sistema de Filas**:
```python
def create_queue_structure():
    # Cria: pending/, processing/, completed/, failed/

def add_to_queue(from_email, params):
    # Adiciona JSON na pending/

def process_queue():
    # Processa todos requests pendentes sequencialmente
    # Para cada request:
    #   1. Move pending -> processing
    #   2. Executa: python selecionar_audios_v3.4.py --modo automatico ...
    #   3. Se exit=0: move processing -> completed
    #   4. Se exit=1: envia no-files email + move -> completed
    #   5. Se exit!=0,1: move processing -> failed
```

**Email CC System (v1.1)**:
- SEMPRE envia para admin (rogerio.paranhos@gmail.com)
- CC para solicitante (se diferente do admin)
- Funcao: `send_email_smtp(to_email, subject, body, cc_email=None)`

**Exit Codes do Processor**:
- 0: Sucesso (audios encontrados e enviados)
- 1: Nenhum arquivo encontrado (mas processamento OK)
- Outros: Erro

#### 2. selecionar_audios_v3.4.py
**Funcao**: Processa solicitacao de audios (HTML email MINIMALISTA)

**Parametros CLI**:
```bash
--modo automatico
--ramais 1121,1122
--inicio "27/10/2025 08:00"
--fim "28/10/2025 18:00"
--email rogerio.paranhos@gmail.com
```

**Funcionalidades**:
- Busca audios localmente em /gravacoes/
- Pattern: `internal-ORIGIN-DEST-YYYYMMDD-HHMMSS-*.wav`
- Cria ZIP com audios
- Gera presigned S3 URL (24h)
- Envia email HTML MINIMALISTA (v3.4)
- Exit codes: 0=sucesso, 1=no files, 2=params error, 3=processing error

**Email HTML v3.4**:
- Layout MINIMALISTA
- Mostra apenas: data/hora | origem -> destino
- Botao "Tocar" para cada audio (presigned URL)
- ZIP download no final
- Cor verde ATHome: RGB(1,132,127)
- Admin sempre em CC (implementado internamente v3.1)

### Execucao Atual (AUDIO)

**Systemd**:
- Service: `/etc/systemd/system/monitor_emails.service`
- Timer: `/etc/systemd/system/monitor_emails.timer`
- **Status**: Timer PARADO (inactive/dead)
- Intervalo: 3 minutos (OnUnitActiveSec=3min)

**Controle**:
- Script: `/root/util/controle_monitor.sh`
- Comandos: pausar, retomar, status
- Usa: `systemctl start/stop monitor_emails.timer`

### Configuracao AUDIO

**Arquivo**: `/root/util/monitor_config.txt`

Conteudo esperado:
```
EMAIL_SERVICE=conta@gmail.com
EMAIL_PASSWORD=senha_app
WHITELIST=email1@domain.com,email2@domain.com
SMTP_SERVER=smtp.gmail.com
SMTP_PORT=587
SMTP_FROM=conta@gmail.com
SMTP_PASSWORD=senha_app
LOG_FILE=/var/log/voip_audios_monitor.log
QUEUE_DIR=/root/sandbox/fila_audios
PROCESSOR_SCRIPT=/root/util/selecionar_audios_v3.4.py
MAX_RAMAIS=10
MAX_DAYS_RANGE=30
```

---

## SERVIDOR AUDIO2 (Python 3.x)

### Localizacao
`/root/maildrive/athome-voip-mail-driver-v3.6/`

### Arquivos Principais

#### 1. monitor_emails_v3.6.py
**Linhas**: 578
**Data**: 09/11/2025
**Funcao**: Monitor IMAP com FILAS (NAO processa, apenas enfileira)

**Funcionalidades**:
- Conecta IMAP Gmail
- Busca emails nao lidos (UNSEEN)
- Valida remetente (whitelist)
- **EXTRAI E VALIDA TOKEN**: `[XXXXXXXX] COMANDO ...`
- Identifica comando: RAMAIS, CHECKIP, MACCHANGE
- Parse basico parametros
- **ADICIONA NA FILA** (pending/)
- Envia email confirmacao (posicao na fila)
- **NAO PROCESSA** - delegado ao queue_processor.py

**Diferenca Chave vs AUDIO**:
- AUDIO: Um unico script (monitor + processa fila)
- AUDIO2: Dois scripts separados (monitor enfileira, processor executa)

**Token System**:
```python
def extract_token_from_subject(subject):
    # Pattern: [XXXXXXXX] resto do subject
    # Token deve ser 8 digitos hexadecimais

def validate_token(token):
    # Valida token no arquivo security/valid_tokens.txt
```

**Comandos Suportados**:
1. **RAMAIS**: Busca audios (via S3)
   - `RAMAIS=1121,1122 INICIO=... FIM=...`
2. **CHECKIP**: Verifica/desbloqueia IP
   - `CHECKIP=IP SERVER=gv1p`
3. **MACCHANGE**: Altera MAC address
   - `MACCHANGE=ramal MACATUAL=xxx MACFUTURO=yyy SERVER=gv1p`

#### 2. queue_processor.py
**Linhas**: 293
**Funcao**: Processa fila FIFO chamando handlers

**Funcionalidades**:
- Carrega requests de pending/
- Para cada request:
  1. Move pending -> processing
  2. Atualiza timestamp_processing
  3. Identifica comando (RAMAIS/CHECKIP/MACCHANGE)
  4. Chama handler apropriado
  5. Handler retorna {success: bool, message: str}
  6. Move processing -> completed ou failed
  7. Atualiza status final
- **Crash Recovery**: Detecta processing stale (timeout 30min)

**Handlers**:
- `commands/ramais_handler.py`: Processa comando RAMAIS
- `commands/checkip_handler.py`: Processa comando CHECKIP
- `commands/macchange_handler.py`: Processa comando MACCHANGE

#### 3. processor_audios_v3.5.py
**Funcao**: Processador de audios para comando RAMAIS (via S3)

**Diferenca vs AUDIO**:
- AUDIO: Busca local em /gravacoes/
- AUDIO2: Busca em S3 bucket

**Sistema Multi-Comando**:
- AUDIO: Apenas RAMAIS
- AUDIO2: RAMAIS + CHECKIP + MACCHANGE

### Execucao Atual (AUDIO2)

**Systemd**:

Service 1: `monitor_emails_v3.6.service`
- Executa: `monitor_emails_v3.6.py`
- Timer: `monitor_emails_v3.6.timer`
- Intervalo: A definir (provavelmente 3min)

Service 2: `queue_processor.service`
- Executa: `queue_processor.py`
- Timer: `queue_processor.timer`
- Intervalo: A definir (provavelmente 1-2min)

**Controle**:
- Provavelmente existe controle_monitor.sh similar ao AUDIO

### Configuracao AUDIO2

**Arquivo**: `config/monitor_config.txt`

Conteudo esperado (similar ao AUDIO + novos):
```
EMAIL_SERVICE=...
EMAIL_PASSWORD=...
WHITELIST=...
SMTP_SERVER=...
SMTP_PORT=...
SMTP_FROM=...
SMTP_PASSWORD=...
LOG_FILE=/var/log/voip_monitor_v3.6.log
QUEUE_DIR=/var/spool/voip_queue/
QUEUE_PROCESSOR_LOG=/var/log/voip_queue_processor.log
PROCESSING_TIMEOUT=30
MAX_RETRIES=3
TOKEN_FILE=/root/maildrive/athome-voip-mail-driver-v3.6/security/valid_tokens.txt
```

---

## COMPARACAO FUNCIONAL

### Funcionalidades Existentes em CADA Lado

| Funcionalidade | AUDIO (2.7) | AUDIO2 (3.x) | Observacao |
|----------------|-------------|--------------|------------|
| **Monitor IMAP** | ✅ SIM | ✅ SIM | Ambos monitoram Gmail |
| **Validacao Whitelist** | ✅ SIM | ✅ SIM | Ambos validam remetente |
| **Sistema Token** | ❌ NAO | ✅ SIM | AUDIO2 tem, AUDIO nao |
| **Sistema de Filas** | ✅ SIM | ✅ SIM | **AMBOS TEM!** |
| **Processamento Filas** | ✅ SIM | ✅ SIM | AUDIO: integrado / AUDIO2: separado |
| **Comando RAMAIS** | ✅ SIM | ✅ SIM | Ambos processam audios |
| **Comando CHECKIP** | ❌ NAO | ✅ SIM | Apenas AUDIO2 |
| **Comando MACCHANGE** | ❌ NAO | ✅ SIM | Apenas AUDIO2 |
| **Busca Audios Local** | ✅ SIM | ❌ NAO | AUDIO: /gravacoes/ |
| **Busca Audios S3** | ❌ NAO | ✅ SIM | AUDIO2: bucket S3 |
| **Email CC System** | ✅ SIM (v1.1) | ✅ SIM | Ambos tem admin+CC |
| **Email HTML** | ✅ SIM (v3.4) | ✅ SIM | Ambos formatados |
| **Email No-Files** | ✅ SIM (v1.1) | ✅ SIM | Ambos notificam |
| **Systemd Service** | ✅ SIM | ✅ SIM | Ambos usam systemd |
| **Controle pausar/retomar** | ✅ SIM | ✅ SIM? | AUDIO confirmado |

### Arquitetura de Execucao

**AUDIO (Integrado)**:
```
monitor_emails.py (executa a cada 3min)
├─ Conecta IMAP
├─ Processa novos emails
├─ Adiciona na fila (pending/)
├─ Processa fila imediatamente
│  └─ Chama selecionar_audios_v3.4.py
└─ Finaliza
```

**AUDIO2 (Separado)**:
```
monitor_emails_v3.6.py (executa a cada 3min)
├─ Conecta IMAP
├─ Processa novos emails
├─ Valida token
├─ Adiciona na fila (pending/)
├─ Envia email confirmacao
└─ Finaliza

queue_processor.py (executa a cada 1-2min)
├─ Carrega pending/
├─ Para cada request:
│  ├─ Move -> processing
│  ├─ Chama handler (ramais/checkip/macchange)
│  └─ Move -> completed/failed
└─ Finaliza
```

---

## O QUE O USUARIO DISSE QUE QUER

### 1. Dual-Mode em AMBOS os Lados

**Requisito**: Ambos servidores devem ter capacidade de monitorar emails

**Como funciona**:
- Campo configuracao: `MODE=master` ou `MODE=slave`
- Se MODE=master: Monitor IMAP ativo (le emails)
- Se MODE=slave: Monitor IMAP desligado (nao le emails)

**Default inicial**:
- AUDIO2: MODE=master (le emails)
- AUDIO: MODE=slave (nao le emails)

**Implementacao**:
```python
# No inicio do monitor_emails
MODE = get_config('MODE', 'slave')  # Default: slave

if MODE != 'master':
    log_message("INFO", "Modo SLAVE - monitor inativo")
    sys.exit(0)

# Continua apenas se MODE=master
```

**PROBLEMA IDENTIFICADO**:
- ✅ AUDIO: Tem apenas um script - facil adicionar dual-mode
- ❌ AUDIO2: monitor_emails_v3.6.py NAO TEM dual-mode implementado

### 2. Sistema de Filas em AMBOS os Lados

**Requisito**: Sistema de filas deve funcionar SEMPRE, independente do MODE

**Logica**:
- Monitor pode estar ligado (master) ou desligado (slave)
- Mas processador de fila SEMPRE roda em ambos os lados

**AUDIO (Atual)**:
- Monitor + Processor no mesmo script
- Se adicionar dual-mode, precisa separar:
  - monitor_emails.py: le emails (se MODE=master) + enfileira
  - queue_processor.py: processa fila (SEMPRE)

**AUDIO2 (Atual)**:
- Ja separado! monitor_emails_v3.6.py + queue_processor.py
- Precisa apenas adicionar dual-mode no monitor

**PROBLEMA IDENTIFICADO**:
- ✅ AUDIO: Tem fila integrada - precisa SEPARAR em 2 scripts
- ✅ AUDIO2: Ja separado - OK

### 3. Nao Mexer no Que Ja Funciona

**Requisito**: Logica de leitura de emails e busca de audios deve ser mantida

**AUDIO**:
- Logica leitura emails: Manter parse `RAMAIS=X INICIO=Y FIM=Z`
- Logica busca audios: Manter busca local em /gravacoes/

**AUDIO2**:
- Logica leitura emails: Manter validacao token + multi-comando
- Logica busca audios: Manter busca em S3

**IMPORTANTE**: Usuario deixou claro que NAO quer mexer nisso!

### 4. Campo audios_from

**Requisito**: Define onde processar comando RAMAIS

**Valores**:
- `audios_from=s3`: Processa no AUDIO2 (busca S3)
- `audios_from=audio`: Processa no AUDIO (busca local /gravacoes/)

**Como funciona**:
- Email chega: `RAMAIS=1121 ... audios_from=audio`
- Se audios_from=audio:
  - AUDIO2 cria request JSON
  - AUDIO2 faz SCP do JSON para AUDIO:/root/sandbox/fila_audios/pending/
  - AUDIO processa (busca local)
  - AUDIO envia email resultado
- Se audios_from=s3:
  - AUDIO2 processa localmente (busca S3)
  - AUDIO2 envia email resultado

**NAO FICOU CLARO NO REPO**:
- Como implementar SCP entre servidores
- Onde configurar credenciais SSH
- Como sincronizar filas

---

## O QUE PRECISA SER FEITO

### AUDIO (Python 2.7)

**Mudancas Necessarias**:

1. ✅ **Criar queue_processor.py separado**
   - Copiar logica process_queue() do monitor_emails.py
   - Manter 100% igual (nao mexer)

2. ✅ **Adicionar dual-mode no monitor_emails.py**
   - Adicionar check MODE no inicio
   - Se slave: exit(0)
   - Se master: continua

3. ✅ **Remover process_queue() do monitor_emails.py**
   - Deixar apenas: le emails + enfileira
   - Processamento delegado ao queue_processor.py

4. ✅ **Criar systemd timer para queue_processor**
   - queue_processor.service
   - queue_processor.timer (executa a cada 2min)

5. ✅ **Adicionar suporte audios_from (opcional)**
   - Se audios_from=s3: criar JSON e SCP para AUDIO2
   - Se audios_from=audio: processar local (default)

**Arquivo de Config (adicionar)**:
```
# Novo em monitor_config.txt
MODE=slave           # master ou slave
AUDIOS_FROM=audio   # audio ou s3 (default: audio)
```

### AUDIO2 (Python 3.x)

**Mudancas Necessarias**:

1. ✅ **Adicionar dual-mode no monitor_emails_v3.6.py**
   - Adicionar check MODE no inicio
   - Se slave: exit(0)
   - Se master: continua

2. ✅ **Manter queue_processor.py como esta**
   - Ja funciona perfeitamente
   - Nao mexer!

3. ✅ **Adicionar suporte audios_from no ramais_handler**
   - Se audios_from=audio: criar JSON e SCP para AUDIO
   - Se audios_from=s3: processar local (default)

**Arquivo de Config (adicionar)**:
```
# Novo em monitor_config.txt
MODE=master          # master ou slave
AUDIOS_FROM=s3      # audio ou s3 (default: s3)
```

---

## ESTRUTURA REPO GITHUB UNIFICADO

### Estrutura Proposta

```
athome-voip-monitor/
├── python2/                          # Codigo AUDIO (Python 2.7)
│   ├── src/
│   │   ├── monitor_emails.py         # Com dual-mode + enfileira apenas
│   │   ├── queue_processor.py        # NOVO - processa fila
│   │   └── selecionar_audios_v3.4.py # Atual - NAO MEXER
│   ├── systemd/
│   │   ├── monitor_emails.service
│   │   ├── monitor_emails.timer
│   │   ├── queue_processor.service   # NOVO
│   │   └── queue_processor.timer     # NOVO
│   ├── config/
│   │   └── monitor_config.txt.example
│   └── scripts/
│       └── controle_monitor.sh       # Atual
│
├── python3/                          # Codigo AUDIO2 (Python 3.x)
│   ├── src/
│   │   ├── monitor_emails_v3.6.py    # Com dual-mode ADICIONADO
│   │   ├── queue_processor.py        # Atual - NAO MEXER
│   │   ├── processor_audios_v3.5.py  # Atual - NAO MEXER
│   │   └── command_router.py         # Atual - NAO MEXER
│   ├── commands/
│   │   ├── ramais_handler.py         # Com suporte audios_from
│   │   ├── checkip_handler.py        # Atual - NAO MEXER
│   │   └── macchange_handler.py      # Atual - NAO MEXER
│   ├── queue_lib/
│   │   └── queue_manager.py          # Atual - NAO MEXER
│   ├── security/
│   │   ├── token_validator.py        # Atual - NAO MEXER
│   │   └── valid_tokens.txt.example
│   ├── servers/
│   │   ├── ssh_manager.py            # Atual - NAO MEXER
│   │   └── servers_config.json       # Atual - NAO MEXER
│   ├── systemd/
│   │   ├── monitor_emails_v3.6.service
│   │   ├── monitor_emails_v3.6.timer
│   │   ├── queue_processor.service
│   │   └── queue_processor.timer
│   └── config/
│       └── monitor_config.txt.example
│
├── docs/
│   ├── ARCHITECTURE.md
│   ├── DUAL_MODE_GUIDE.md           # Como configurar master/slave
│   ├── QUEUE_SYSTEM.md
│   └── AUDIOS_FROM_INTEGRATION.md   # Como funciona SCP entre servidores
│
├── deploy/
│   ├── install_audio.sh             # Deploy completo no AUDIO
│   ├── install_audio2.sh            # Deploy completo no AUDIO2
│   └── README_DEPLOY.md
│
├── common/
│   └── utils.py                     # Funcoes compartilhadas (se houver)
│
└── README.md                        # Overview geral
```

### Configuracoes Default

**AUDIO (monitor_config.txt)**:
```
MODE=slave              # Default: NAO le emails
AUDIOS_FROM=audio      # Default: processa local
QUEUE_DIR=/root/sandbox/fila_audios
```

**AUDIO2 (monitor_config.txt)**:
```
MODE=master            # Default: LE emails
AUDIOS_FROM=s3        # Default: processa S3
QUEUE_DIR=/var/spool/voip_queue
```

---

## RESUMO DOS PROBLEMAS DO REPO ANTERIOR

### 1. Python3 NAO tinha dual-mode
- monitor_emails_v3.6.py nao checava MODE
- Sempre executava, nao tinha como desligar

### 2. Sistema de filas incompleto no Python2
- AUDIO atual: tudo integrado no monitor_emails.py
- Repo precisava: queue_processor.py separado no Python2

### 3. Integracao audios_from nao implementada
- Usuario mencionou mas nao estava no repo
- Faltava: SCP entre servidores, configuracao SSH

### 4. Confusao sobre "nao mexer"
- Usuario disse: nao mexer na logica emails/audios
- Eu acabei mexendo ou ficou confuso o que foi mantido

### 5. Default configs errados
- Usuario: AUDIO2=master, AUDIO=slave
- Repo: nao tinha configs default claros

---

## PROXIMOS PASSOS

1. ✅ **Criar queue_processor.py para Python 2.7**
   - Baseado na logica atual do monitor_emails.py
   - 100% compativel Python 2.7

2. ✅ **Adicionar dual-mode em ambos monitores**
   - Python 2.7: monitor_emails.py
   - Python 3.x: monitor_emails_v3.6.py

3. ✅ **Implementar suporte audios_from**
   - ramais_handler.py: detectar audios_from
   - Se =audio: SCP para AUDIO
   - Se =s3: processar local

4. ✅ **Criar scripts de deploy**
   - install_audio.sh
   - install_audio2.sh

5. ✅ **Documentacao completa**
   - Como funciona dual-mode
   - Como trocar master/slave
   - Como funciona audios_from

6. ❌ **VALIDAR COM USUARIO ANTES DE IMPLEMENTAR**
   - Mostrar este documento
   - Confirmar entendimento 100%
   - So depois criar repo

---

**FIM DA ANALISE**
