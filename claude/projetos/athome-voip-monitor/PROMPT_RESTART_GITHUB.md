# PROMPT DE RESTART - PROJETO GITHUB UNIFIED

**Data da Analise**: 10/11/2025
**Objetivo**: Criar repositorio GitHub unificado para AUDIO (Python 2.7) e AUDIO2 (Python 3.x)

---

## CONTEXTO DO PROBLEMA

Tentamos criar este repo antes e FALHOU. Usuario ficou frustrado porque:
1. Nao entendi corretamente os requisitos
2. Implementei sem validar entendimento
3. Resultado nao bateu com o esperado
4. Fiquei pedindo explicacoes repetidas

**Nova abordagem acordada**:
- Criar entendimento passo a passo via perguntas/respostas
- NAO executar nada ate validar 100%
- Implementar UMA tarefa por vez
- Comitar no git
- Usuario testa e da retorno
- So depois proxima tarefa

---

## ANALISE COMPLETA JA FEITA

Li TODO o codigo existente em ambos servidores. Documento completo em:
`/mnt/c/claudedoc/ANALISE_COMPLETA_SISTEMAS_EXISTENTES.md`

### Codigo AUDIO (Python 2.7)
- Localizacao: `/root/util/`
- Monitor: `monitor_emails.py` (766 linhas, v1.1)
- Processor: `selecionar_audios_v3.4.py`
- Config: `/root/util/monitor_config.txt`
- Systemd: `monitor_emails.service` + `monitor_emails.timer` (PARADO)

### Codigo AUDIO2 (Python 3.x)
- Localizacao: `/root/maildrive/athome-voip-mail-driver-v3.6/`
- Monitor: `monitor_emails_v3.6.py` (578 linhas)
- Queue: `queue_processor.py` (293 linhas)
- Processor: `processor_audios_v3.5.py`
- Handlers: `ramais_handler.py`, `checkip_handler.py`, `macchange_handler.py`
- Config: `config/monitor_config.txt`
- Systemd: 2 timers (monitor + queue_processor)

---

## ENTENDIMENTO CHAVE

### 1. Sistema de Filas
**AMBOS JA TEM!**
- AUDIO: Integrado no monitor_emails.py (funcao process_queue)
- AUDIO2: Separado em queue_processor.py

Estrutura identica:
```
/queue_dir/
├── pending/      # Novos requests
├── processing/   # Em execucao
├── completed/    # Sucesso
└── failed/       # Erro
```

### 2. Dual-Mode (O QUE FALTOU NO REPO ANTERIOR)

**Requisito**: Ambos devem ter capacidade de monitorar emails

**Como funciona**:
- Config: `MODE=master` ou `MODE=slave`
- MODE=master: Monitor ativo (le emails do Gmail)
- MODE=slave: Monitor inativo (nao le emails)

**Default inicial**:
- AUDIO2: MODE=master
- AUDIO: MODE=slave

**Implementacao** (inicio do monitor):
```python
MODE = get_config('MODE', 'slave')
if MODE != 'master':
    log_message("INFO", "Modo SLAVE - monitor inativo")
    sys.exit(0)
# Continua apenas se master
```

**PROBLEMA**: AUDIO2 monitor_emails_v3.6.py NAO TEM isto implementado!

### 3. Queue Processor Sempre Ativo

**Requisito**: Fila processa SEMPRE, independente do MODE

**Logica**:
- Monitor pode estar ligado/desligado (master/slave)
- Queue processor SEMPRE roda

**AUDIO atual**: Tudo integrado - precisa SEPARAR
**AUDIO2 atual**: Ja separado - OK

### 4. Nao Mexer no Que Funciona

**Usuario enfatizou**: NAO mexer na logica de:
- Leitura de emails (parse, validacao)
- Busca de audios (local ou S3)

**AUDIO**:
- Parse: `RAMAIS=X,Y INICIO=DD/MM/YYYY HH:MM FIM=DD/MM/YYYY HH:MM`
- Busca: local em `/gravacoes/`
- Manter 100%!

**AUDIO2**:
- Parse: Token `[XXXXXXXX]` + multi-comando
- Busca: S3 bucket
- Manter 100%!

### 5. Campo audios_from

**Requisito**: Define ONDE processar comando RAMAIS

**Valores**:
- `audios_from=s3` → AUDIO2 processa (busca S3)
- `audios_from=audio` → AUDIO processa (busca local)

**Integracao via SCP**:
- Email chega com `audios_from=audio`
- AUDIO2 cria request JSON
- AUDIO2 faz SCP para AUDIO:`/root/sandbox/fila_audios/pending/`
- AUDIO processa
- AUDIO envia email resultado

**NAO IMPLEMENTADO** no repo anterior!

---

## O QUE PRECISA SER FEITO

### AUDIO (Python 2.7) - 4 mudancas

1. **Criar queue_processor.py**
   - Extrair funcao process_queue() do monitor_emails.py
   - Script separado que processa fila
   - Python 2.7 compativel

2. **Adicionar dual-mode no monitor_emails.py**
   - Check MODE no inicio
   - Exit se slave

3. **Remover process_queue() do monitor_emails.py**
   - Deixar apenas: le emails + enfileira
   - Nao processar mais

4. **Criar systemd timer para queue_processor**
   - queue_processor.service
   - queue_processor.timer

### AUDIO2 (Python 3.x) - 2 mudancas

1. **Adicionar dual-mode no monitor_emails_v3.6.py**
   - Check MODE no inicio
   - Exit se slave

2. **Adicionar suporte audios_from no ramais_handler.py**
   - Se `audios_from=audio`: SCP para AUDIO
   - Se `audios_from=s3`: processar local (default)

### Configs

**AUDIO monitor_config.txt** (adicionar):
```
MODE=slave
AUDIOS_FROM=audio
```

**AUDIO2 monitor_config.txt** (adicionar):
```
MODE=master
AUDIOS_FROM=s3
```

---

## ARQUIVOS LIDOS (para referencia rapida)

### AUDIO
- `/root/util/monitor_emails.py` - 766 linhas
- `/root/util/selecionar_audios_v3.4.py`
- `/root/util/monitor_config.txt`
- `/root/util/controle_monitor.sh`
- `/etc/systemd/system/monitor_emails.service`
- `/etc/systemd/system/monitor_emails.timer`

### AUDIO2
- `/root/maildrive/athome-voip-mail-driver-v3.6/monitor_emails_v3.6.py` - 578 linhas
- `.../queue_processor.py` - 293 linhas
- `.../processor_audios_v3.5.py`
- `.../commands/ramais_handler.py`
- `.../commands/checkip_handler.py`
- `.../commands/macchange_handler.py`
- `.../config/monitor_config.txt`
- `.../systemd/*.service` e `*.timer`

**Documentacao ja existente**:
- `/mnt/c/claudedoc/ANALISE_COMPLETA_SISTEMAS_EXISTENTES.md`
- `/mnt/c/claudedoc/PROBLEMAS_REPO_GITHUB.md`
- `/mnt/c/claudedoc/Claude_screen(monitor-emails).txt` (sessao anterior completa)

---

## ESTRUTURA REPO PROPOSTA

```
athome-voip-monitor/
├── python2/
│   ├── src/
│   │   ├── monitor_emails.py         # Com dual-mode
│   │   ├── queue_processor.py        # NOVO
│   │   └── selecionar_audios_v3.4.py # NAO MEXER
│   ├── systemd/
│   │   ├── monitor_emails.service
│   │   ├── monitor_emails.timer
│   │   ├── queue_processor.service   # NOVO
│   │   └── queue_processor.timer     # NOVO
│   └── config/
│       └── monitor_config.txt.example
│
├── python3/
│   ├── src/
│   │   ├── monitor_emails_v3.6.py    # Com dual-mode
│   │   ├── queue_processor.py        # NAO MEXER
│   │   └── processor_audios_v3.5.py  # NAO MEXER
│   ├── commands/
│   │   ├── ramais_handler.py         # Com audios_from
│   │   ├── checkip_handler.py        # NAO MEXER
│   │   └── macchange_handler.py      # NAO MEXER
│   ├── systemd/
│   └── config/
│
├── docs/
│   ├── DUAL_MODE_GUIDE.md
│   └── AUDIOS_FROM_GUIDE.md
│
└── README.md
```

---

## ABORDAGEM ACORDADA

1. **Fase 1: Entendimento** (sem executar nada)
   - Usuario faz perguntas simples
   - Respondo apenas conceitos
   - Validamos entendimento mútuo

2. **Fase 2: Implementacao** (tarefa por tarefa)
   - Implemento UMA mudanca
   - Comito no git
   - Usuario testa
   - Da retorno OK ou problema
   - So depois proxima tarefa

3. **NAO fazer**:
   - Executar comandos antes de validar
   - Criar multiplas mudancas de uma vez
   - Assumir que entendi sem confirmar

---

## PONTOS DE ATENCAO

1. **Python 2.7 vs Python 3**
   - Sintaxe diferente (print, strings)
   - Testar compatibilidade

2. **Paths absolutos**
   - AUDIO: `/root/util/`, `/root/sandbox/fila_audios/`
   - AUDIO2: `/root/maildrive/athome-voip-mail-driver-v3.6/`, `/var/spool/voip_queue/`

3. **Email sempre para admin**
   - ADMIN_EMAIL = "rogerio.paranhos@gmail.com"
   - CC para solicitante

4. **Exit codes importantes**
   - 0: Sucesso
   - 1: Nenhum arquivo (mas OK)
   - Outros: Erro

5. **Systemd timers**
   - Monitor: 3 minutos
   - Queue processor: 2 minutos (ou 1 min)

---

## QUANDO RETOMAR

1. Usuario dira: "vamos retomar o projeto github unified"
2. Ler este documento
3. Perguntar: "Entendi que vamos trabalhar em modo pergunta/resposta primeiro. Qual sua primeira pergunta?"
4. NAO executar nada ate usuario pedir explicitamente

---

## REFERENCIAS

- Sessao anterior completa: `/mnt/c/claudedoc/Claude_screen(monitor-emails).txt`
- Analise sistemas: `/mnt/c/claudedoc/ANALISE_COMPLETA_SISTEMAS_EXISTENTES.md`
- Problemas identificados: `/mnt/c/claudedoc/PROBLEMAS_REPO_GITHUB.md`
- Relatorio tentativa anterior: `/mnt/c/claudedoc/RELATORIO_GITHUB_UNIFIED.md`

---

**IMPORTANTE**: Usuario estava frustrado. Preciso recuperar confianca mostrando que ENTENDO de verdade antes de fazer qualquer coisa.

**Frase chave do usuario**: "nao e nada disso" - significa que nao entendi. Preciso parar e perguntar ate entender 100%.

---

**FIM DO PROMPT DE RESTART**
