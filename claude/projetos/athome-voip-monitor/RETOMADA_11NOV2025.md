# RETOMADA DE PROJETO - ATHome VoIP Monitor
**Data de criacao:** 11/11/2025 - 21:30
**Ultima atualizacao:** 11/11/2025 - 22:30
**Status:** Pronto para ajustes finais

---

## CONTEXTO RAPIDO

Sistema de monitoramento VoIP com 2 servidores:
- **AUDIO (Python 2.7):** Processa audios locais, modo SLAVE
- **AUDIO2 (Python 3.8):** Processa S3 + roteia para AUDIO, modo MASTER

**O QUE FOI FEITO HOJE:**
1. ✅ AUDIO2: Roteamento baseado em `audios_from` (audio vs s3)
2. ✅ AUDIO2: 3 comandos testados (RAMAIS, CHECKIP, MACCHANGE)
3. ✅ AUDIO2: Systemd timers ativos (producao)
4. ✅ AUDIO: Token validation implementada e testada
5. ✅ AUDIO: 10 tokens sincronizados com AUDIO2

---

## PROXIMOS PASSOS (PARA PROXIMA SESSAO)

### 0. PRIMEIRO: Criar Pasta .claudedoc no Repositorio
**PRIORIDADE MAXIMA - FAZER ANTES DE QUALQUER OUTRA COISA**

**Objetivo:** Criar pasta `.claudedoc/` no repositorio Git para documentacao exclusiva de IA

**Estrutura proposta:**
```
athome-voip-monitor/
├── python2/              # Codigo AUDIO
├── python3/              # Codigo AUDIO2
├── docs/                 # Documentacao tecnica (usuarios, admins)
│   ├── README.md
│   ├── INSTALACAO.md
│   └── COMANDOS.md
│
├── .claudedoc/          # ← CONTEXTO PARA IA (exclusivo)
│   ├── PROJETO.md       # Overview completo do projeto
│   ├── SESSOES/         # Historico de sessoes com IA
│   │   ├── 2025-11-10_audio_python2.md
│   │   ├── 2025-11-11_audio2_roteamento.md
│   │   └── 2025-11-11_token_validation.md
│   ├── ARQUITETURA/     # Decisoes tecnicas e contexto
│   │   ├── tokens.md
│   │   ├── roteamento.md
│   │   └── dual_mode.md
│   └── RETOMADA.md      # Arquivo principal de retomada
│
└── .gitignore           # Ignora credentials mas NAO .claudedoc
```

**Vantagens:**
- ✅ Separacao clara: `.claudedoc/` = contexto IA | `docs/` = docs usuario
- ✅ Nome obvio: Qualquer IA sabe que e para ela
- ✅ Git-friendly: Tudo versionado e sincronizado
- ✅ Portavel: Clone o repo = tem todo contexto
- ✅ Nao polui: Pasta oculta (comeca com `.`)

**Tarefas:**
1. Criar pasta `.claudedoc/` no repo local
2. Mover documentacao atual para `.claudedoc/SESSOES/`
3. Criar `.claudedoc/RETOMADA.md` (este arquivo)
4. Criar `.claudedoc/PROJETO.md` (overview)
5. Criar `.claudedoc/ARQUITETURA/` com decisoes tecnicas
6. Commitar e push
7. Pull nos servidores AUDIO e AUDIO2

**Resultado esperado:**
Qualquer pessoa (ou IA) que clonar o repositorio tera acesso imediato a todo contexto e historico do projeto.

---

### 1. Ajustes de Layout de Emails
- Melhorar formatacao dos emails enviados
- Considerar usar HTML em vez de plain text
- Adicionar branding ATHome
- Melhorar apresentacao de resultados

### 2. Gerenciamento Dinamico de Whitelist
- Implementar comandos para adicionar/remover emails autorizados
- Formato: `[TOKEN] WHITELIST ADD email@example.com`
- Formato: `[TOKEN] WHITELIST REMOVE email@example.com`
- Formato: `[TOKEN] WHITELIST LIST`

---

## ARQUIVOS DE DOCUMENTACAO

### Documento Principal (1418 linhas)
**Caminho:** `/mnt/c/claudedoc/SESSAO_COMPLETA_AUDIO2_PYTHON3_ROTEAMENTO_11NOV2025.md`

**Conteudo:**
- Fase 1-4: AUDIO2 implementacao e roteamento
- Fase 5: Testes dos 3 comandos
- Fase 6: Ativacao systemd services
- Fase 7: Token validation no AUDIO (completa)
- Proximos passos detalhados

### Documento AUDIO (Python2)
**Caminho:** `/mnt/c/claudedoc/SESSAO_COMPLETA_AUDIO_PYTHON2_11NOV2025.md`

**Conteudo:**
- Implementacao AUDIO modo SLAVE
- Sistema de filas FIFO
- Integracao com AUDIO2

---

## REPOSITORIO GIT

**URL:** `https://github.com/rogeriomendesparanhos/athome-voip-monitor.git`

**Credenciais:** `/mnt/c/claudedoc/credentials/github-credentials.txt`
```
Usuario: rogeriomendesparanhos
Token: ghp_XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
```

**Branch:** main

**Commits realizados (5 total):**
1. `f84c564` - feat(python3): popula v3.6 sem sufixos
2. `b1b0341` - feat(python3): roteamento audios_from
3. `5a9558b` - fix(python3): corrige path processor_audios
4. `20fe2c1` - feat(python3): systemd services AUDIO2
5. `98ccfae` - feat(python2): token validation AUDIO

**Estrutura do repositorio:**
```
athome-voip-monitor/
├── python2/             # AUDIO (Python 2.7, CentOS, local files)
│   ├── src/
│   │   ├── monitor_emails.py         (v1.2 - com token)
│   │   ├── queue_processor.py
│   │   ├── selecionar_audios.py
│   │   └── security/                 (NOVO)
│   │       ├── tokens.json           (10 tokens)
│   │       └── token_validator.py    (Python 2.7)
│   ├── config/
│   │   └── monitor_config.txt
│   └── scripts/
│       └── controle_monitor.sh
│
└── python3/             # AUDIO2 (Python 3.8, Ubuntu, S3)
    ├── monitor_emails.py
    ├── queue_processor.py
    ├── processor_audios.py
    ├── command_router.py
    ├── commands/
    │   ├── ramais_handler.py         (com roteamento)
    │   ├── checkip_handler.py
    │   └── macchange_handler.py
    ├── servers/
    │   └── servers_config.json       (campo audios_from)
    ├── security/
    │   ├── tokens.json               (10 tokens)
    │   └── token_validator.py
    ├── config/
    │   └── monitor_config.txt
    └── systemd/
        ├── monitor_emails.service
        ├── monitor_emails.timer
        ├── queue_processor.service
        └── queue_processor.timer
```

---

## SERVIDORES

### AUDIO (54.232.201.186)
**OS:** CentOS 7
**Python:** 2.7
**Path:** `/root/maildrive/athome-voip-monitor/python2/`
**Modo:** SLAVE (processa fila apenas)
**Fila:** `/root/sandbox/fila_audios/`

**Config:** `/root/maildrive/athome-voip-monitor/python2/config/monitor_config.txt`
```
MODE=slave
EMAIL_SERVICE=rogerio.paranhos@gmail.com
EMAIL_PASSWORD=yzulqmveitgevhnt
QUEUE_DIR=/root/sandbox/fila_audios
WHITELIST=rogerio.paranhos@gmail.com
```

**Services:**
```bash
systemctl status monitor_emails.timer    # dormindo (slave)
systemctl status queue_processor.timer   # ativo (3 min)
```

**Logs:**
- `/var/log/voip_audios_monitor.log` (monitor)
- `/var/log/voip_queue_processor.log` (queue)

**SSH:**
```bash
ssh root@audio
Senha: mdansk
```

### AUDIO2 (18.228.131.217)
**OS:** Ubuntu 22.04
**Python:** 3.8+
**Path:** `/root/maildrive/athome-voip-monitor/python3/`
**Modo:** MASTER (le emails + processa)
**Fila:** `/root/sandbox/email_commands_queue/`

**Config:** `/root/maildrive/athome-voip-monitor/python3/config/monitor_config.txt`
```
EMAIL_SERVICE=voip.audios.service@gmail.com
EMAIL_PASSWORD=uxinjspfqjnmisla
WHITELIST=rogerio.paranhos@gmail.com
QUEUE_DIR=/root/sandbox/email_commands_queue
```

**Services:**
```bash
systemctl status monitor_emails.timer    # ativo (3 min)
systemctl status queue_processor.timer   # ativo (3 min)
```

**Logs:**
- `/var/log/voip_multi_command_monitor.log` (monitor)
- `/var/log/voip_queue_processor.log` (queue)

**SSH:**
```bash
ssh root@audio2
Senha: mdansk
```

---

## TOKENS DE SEGURANCA

**Localizacao:**
- `python2/src/security/tokens.json`
- `python3/security/tokens.json`

**10 tokens identicos em ambos servidores:**
```json
{
  "tokens": [
    {"token": "A1B2C3D4", "description": "Token Master 1", "enabled": true},
    {"token": "FF00AA11", "description": "Token Master 2", "enabled": true},
    {"token": "12345678", "description": "Token Teste 1", "enabled": true},
    {"token": "DEADBEEF", "description": "Token Dev 1", "enabled": true},
    {"token": "CAFEBABE", "description": "Token Dev 2", "enabled": true},
    {"token": "FEDCBA98", "description": "Token Backup 1", "enabled": true},
    {"token": "AB12CD34", "description": "Token Prod 1", "enabled": true},
    {"token": "99887766", "description": "Token Prod 2", "enabled": true},
    {"token": "11223344", "description": "Token Temp 1", "enabled": true},
    {"token": "AABBCCDD", "description": "Token Temp 2", "enabled": true}
  ]
}
```

**Funcionamento:**
- Token e validado APENAS na leitura do email
- Token e removido do subject antes do processamento
- JSON da fila NAO contem token
- Formato: `[XXXXXXXX]` (8 digitos hexadecimais)
- Exemplo: `[A1B2C3D4] RAMAIS=1121,1122 INICIO=... FIM=...`

---

## COMANDOS DISPONIVEIS

### 1. RAMAIS
**Formato:**
```
[TOKEN] RAMAIS=1121,1122 SERVER=voip3 INICIO=01/11/2025 00:00 FIM=05/11/2025 23:59
```

**Servidores:**
- `gv1p`, `gv2p`, `gv3p` → audios_from=audio (roteia para AUDIO)
- `voip1`, `voip3` → audios_from=s3 (processa localmente)

**Resultado:** ZIP com audios + email com link

### 2. CHECKIP
**Formato:**
```
[TOKEN] CHECKIP=3.3.3.3
```

**Acao:**
- Verifica em 4 servidores (gv1p, gv2p, gv3p, voip1)
- Desban se necessario
- Adiciona na whitelist fail2ban

**Resultado:** Email com status de cada servidor

### 3. MACCHANGE
**Formato:**
```
[TOKEN] MACCHANGE=3010 MACATUAL=010203040506 MACFUTURO=010203040599 SERVER=gv2p
```

**Restricao:** Apenas servidores gv1p, gv2p, gv3p

**Acao:**
- Valida MAC atual
- Cria backup
- Altera em 2 arquivos
- Notifica arimonitor

**Resultado:** Email com confirmacao da alteracao

---

## ROTEAMENTO (audios_from)

**Arquivo:** `python3/servers/servers_config.json`

```json
{
  "sip_servers": [
    {"id": "gv1p", "host": "18.228.54.15", "audios_from": "audio"},
    {"id": "gv2p", "host": "52.67.72.145", "audios_from": "audio"},
    {"id": "gv3p", "host": "18.228.236.165", "audios_from": "audio"},
    {"id": "voip1", "host": "54.207.156.75", "audios_from": "s3"},
    {"id": "voip3", "host": "56.124.7.66", "audios_from": "s3"}
  ]
}
```

**Fluxo RAMAIS:**
1. AUDIO2 recebe email `[TOKEN] RAMAIS=... SERVER=gv2p`
2. AUDIO2 valida token
3. AUDIO2 verifica `audios_from` de gv2p → "audio"
4. AUDIO2 cria JSON e envia via SCP para AUDIO
5. AUDIO processa fila e retorna resultado

**Fluxo alternativo (S3):**
1. AUDIO2 recebe email `[TOKEN] RAMAIS=... SERVER=voip3`
2. AUDIO2 valida token
3. AUDIO2 verifica `audios_from` de voip3 → "s3"
4. AUDIO2 processa localmente com boto3
5. AUDIO2 envia resultado

---

## TESTES REALIZADOS

### Teste 1: Token Invalido
**Email:** `[A1B2C3DX] CHECKIP=3.3.3.3`
**Resultado:** ❌ Rejeitado (9 caracteres, precisa 8)
**Email enviado:** "Erro - Token Invalido"

### Teste 2: Token Valido + RAMAIS
**Email:** `[A1B2C3D4] RAMAIS=1013010,1013015 SERVER=voip3 INICIO=04/11/2025 12:00 FIM=04/11/2025 18:00`
**Resultado:** ✅ Sucesso
- Token validado
- Processado via S3 (voip3)
- 81 arquivos encontrados
- Email com resultado enviado

### Teste 3: CHECKIP
**Email:** `[A1B2C3D4] CHECKIP=3.3.3.3`
**Resultado:** ✅ Sucesso
- IP desbanido em GV3P
- Adicionado whitelist em 4 servidores

### Teste 4: MACCHANGE (3 operacoes)
**Emails:**
1. Trocar: `MACATUAL=010203040506 MACFUTURO=010203040599`
2. Remover: `MACATUAL=010203040599 MACFUTURO=`
3. Adicionar: `MACATUAL= MACFUTURO=010203040588`

**Resultado:** ✅ Todos com sucesso

---

## COMANDOS UTEIS

### Git Local (WSL)
```bash
cd /mnt/c/claudedoc/athome-voip-monitor

# Ver status
git status

# Pull
git pull

# Commit
git add .
git commit -m "mensagem"
git push
```

### AUDIO2 - Pull e Restart
```bash
ssh root@audio2
cd /root/maildrive/athome-voip-monitor
git pull

# Restart services
systemctl restart monitor_emails.timer
systemctl restart queue_processor.timer

# Ver logs
tail -50 /var/log/voip_multi_command_monitor.log
tail -50 /var/log/voip_queue_processor.log
```

### AUDIO - Pull e Restart
```bash
ssh root@audio
cd /root/maildrive/athome-voip-monitor
git pull

# Restart services
systemctl restart queue_processor.timer

# Ver logs
tail -50 /var/log/voip_queue_processor.log
```

### Teste Manual AUDIO2
```bash
ssh root@audio2
cd /root/maildrive/athome-voip-monitor/python3

# Monitor
python3 monitor_emails.py

# Queue
python3 queue_processor.py

# Ver fila
ls -lh /root/sandbox/email_commands_queue/pending/
ls -lh /root/sandbox/email_commands_queue/completed/
```

### Teste Manual AUDIO
```bash
ssh root@audio
cd /root/maildrive/athome-voip-monitor/python2/src

# Queue apenas (modo SLAVE)
python queue_processor.py

# Ver fila
ls -lh /root/sandbox/fila_audios/pending/
ls -lh /root/sandbox/fila_audios/completed/
```

---

## CREDENCIAIS

**Localizacao:** `/mnt/c/claudedoc/credentials/`

### GitHub
**Arquivo:** `github-credentials.txt`
```
Usuario: rogeriomendesparanhos
Token: ghp_XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
```

### Email AUDIO2 (voip.audios.service@gmail.com)
**Arquivo:** `voip_audios_service_gmail.txt`
```
Email: voip.audios.service@gmail.com
Senha App: uxinjspfqjnmisla
```

### Email AUDIO (rogerio.paranhos@gmail.com)
```
Email: rogerio.paranhos@gmail.com
Senha App: yzulqmveitgevhnt
```

### SSH Servidores
```
Usuario: root
Senha: mdansk
```

---

## PROMPT DE RETOMADA

**Use este prompt para retomar o projeto:**

```
Ola Claude!

Estou retomando o projeto ATHome VoIP Monitor apos reboot do sistema.

Por favor, leia o arquivo de retomada em:
/mnt/c/claudedoc/RETOMADA_PROJETO_AUDIO_11NOV2025.md

E tambem a documentacao completa em:
/mnt/c/claudedoc/SESSAO_COMPLETA_AUDIO2_PYTHON3_ROTEAMENTO_11NOV2025.md

STATUS ATUAL:
- AUDIO2 (Python3): 100% em producao, timers ativos
- AUDIO (Python2): Token validation implementada e testada
- Git: 5 commits, tudo sincronizado
- Repositorio: https://github.com/rogeriomendesparanhos/athome-voip-monitor

PROXIMOS PASSOS:
0. PRIMEIRO: Criar pasta .claudedoc/ no repositorio (PRIORIDADE)
1. Ajustes de layout de emails
2. Gerenciamento dinamico de whitelist

Preciso que voce:
1. Confirme que entendeu o contexto
2. Verifique se os servidores estao operacionais
3. CRIAR a estrutura .claudedoc/ conforme documentado
4. Preparar-se para implementar os ajustes finais
```

---

## CHECKLIST PRE-RETOMADA

Apos reboot, verificar:

- [ ] WSL esta funcionando
- [ ] Pasta `/mnt/c/claudedoc/` acessivel
- [ ] SSH para AUDIO funciona: `ssh root@audio`
- [ ] SSH para AUDIO2 funciona: `ssh root@audio2`
- [ ] Git local esta no main: `cd /mnt/c/claudedoc/athome-voip-monitor && git status`
- [ ] Timers AUDIO2 ativos: `ssh root@audio2 'systemctl status monitor_emails.timer'`
- [ ] Timer AUDIO ativo: `ssh root@audio 'systemctl status queue_processor.timer'`

---

## OBSERVACOES IMPORTANTES

1. **Token e porta de entrada apenas**
   - Extraido e validado na leitura do email
   - Removido antes do processamento
   - NAO vai para fila ou resultado

2. **Ambos servidores validam token**
   - AUDIO2: quando le emails (modo MASTER)
   - AUDIO: quando le emails (futuro modo MASTER)
   - Processadores de fila NAO validam token

3. **Roteamento e transparente**
   - AUDIO2 decide baseado em `audios_from`
   - AUDIO sempre processa localmente
   - Usuario nao precisa saber onde foi processado

4. **Whitelist atual e estatica**
   - Arquivo texto com emails separados por virgula
   - Precisa editar arquivo e reiniciar para alterar
   - Proximo passo: tornar dinamica via comandos

---

**PROJETO 100% FUNCIONAL E DOCUMENTADO!**
**PRONTO PARA AJUSTES FINAIS!**
