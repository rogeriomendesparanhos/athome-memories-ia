# Sessão de Teste - Screen + Claude

## Contexto
- Usuário: paranhos
- Testando uso do Claude dentro de sessões do screen
- Objetivo: manter capacidade de rolagem na interface do Claude

## O que fizemos
1. Criamos arquivo `~/.screenrc` com configurações básicas
2. Habilitamos scrollback com 10000 linhas
3. Configuramos suporte para rolagem

## Próximos passos para testar
1. Executar `screen -S teste`
2. Dentro do screen, executar `claude`
3. Testar rolagem com `Ctrl+A` + `[`
4. Verificar se a interface do Claude funciona adequadamente

## Como retomar
Diga: "Voltei para testar o screen com Claude" e eu lembrarei do contexto.

# Protocolo de Modo Conversa

## Ativação do Modo Conversa
Quando o usuário disser:
- "quero conversar sobre [assunto]"
- "vamos entrar no modo conversa" 
- "modo conversa sobre [tema]"

## Comportamento no Modo Conversa
- **Perguntas e respostas rápidas e diretas**
- **Análise de pontos específicos sem executar atividades**
- **Discussão conceitual antes da implementação**
- **Vale para questionamentos do usuário E do Claude**
- **Foco em entendimento mútuo do assunto**

## Objetivo
Permitir análise e discussão de assuntos sem partir direto para execução, criando um espaço para alinhamento e esclarecimentos antes de agir.

# Progresso ARIMONITOR v3.0

## Status Atual (15/08/2025)
- ✅ **Console estruturada** - 11 comandos + help contextual
- ✅ **Parser endpoints** - 70 endpoints carregados corretamente
- ✅ **Verificação ARI** - check status via REST API
- ✅ **Shutdown graceful** - quit funcionando limpo
- ✅ **95% pronto para produção**

## Próxima Sessão ARIMONITOR
**Foco**: Ajustes no módulo arimonitor.py principal
- Consolidar funcionalidades base antes do WebSocket
- Melhorias e validações necessárias
- Preparar base sólida para análise WebSocket posterior
- Status atual: 95% pronto, console funcionando, 70 endpoints carregados

## Comandos de Retomada
```bash
ssh root@voip3
cd /root/arimonitor_v3
python3.12 arimonitor.py
ARIMONITOR> help
ARIMONITOR> list 101
ARIMONITOR> endpoints
```

# Infraestrutura VoIP - Contexto Completo

## Visao Geral
- **130+ empresas de seguranca** (revendas)
- **1.500+ ramais totais** em 2 ambientes distintos
- **AWS sa-east-1** (Sao Paulo)
- **8 instancias EC2**, 11 volumes EBS (1.320GB total)

## Servidores e Ambientes

### PRODUCAO - SEVENTH (Legacy CHAN-SIP)
**READ-ONLY - NUNCA ALTERAR OU ATIVAR DEBUG**

- **GV1P** (18.228.54.15) - Overflow: 305 endpoints, 5 revendas
- **GV2P** (52.67.72.145) - Principal: 410+ endpoints, 125 revendas
- **GV3P** (18.228.236.165) - Dedicado: 573 endpoints, 1 cliente
- **Total**: 1.378 ramais configurados, 813+ ativos

### PRODUCAO - TECNOFY / CIENCIA DIGITAL (Moderno PJSIP + WebRTC)
**READ-ONLY - NUNCA ALTERAR OU ATIVAR DEBUG**

- **VOIP1** (54.207.156.75) - Producao: Asterisk 20.11.1, WebRTC
- **Total**: 180+ ramais ativos, 81 grupos de toque, 5 clientes operacionais

### AUDIO (Compartilhado)
- **AUDIO** (54.232.201.186) - Serve AMBOS ambientes (SEVENTH + TECNOFY)
- Nao vinculado a nenhum cliente especifico
- 500GB gravacoes compartilhadas

### DESENVOLVIMENTO E TESTES
**Ambiente seguro para testes e desenvolvimento**

- **VOIP3** - Ambiente de testes criado 100% via claude-code + AWS CLI
  - Instalacao Asterisk + MariaDB
  - Mesmo nivel funcional do VOIP1
  - **MODELO PARA EVOLUCAO FUTURA**
  - Host do ARIMONITOR v3.0 (95% pronto)

- **VOIP2** (54.232.235.128) - Standby (parado)
- **PILOTO** (172.31.14.5) - Testes (parado)
- **CLAUDE-CODE** (52.67.178.193) - Desenvolvimento (parado)

## Modelo de Evolucao (VOIP1/VOIP3)

**Roadmap**:
1. VOIP3 criado do zero via AWS CLI
2. ARIMONITOR migrando para Python 3 (95% pronto)
3. Implementar **dual-mode sipserver**:
   - **Modo Atual**: Configuracao via arquivos texto
   - **Modo Futuro**: Realtime via MariaDB (todos recursos no banco)

## Regras Criticas de Seguranca

### Acesso SSH
- Sempre via `ssh root@servidor`
- **NUNCA mexer nas chaves SSH sem permissao**
- Senha root universal: `mdansk`

### Ambientes de Producao (READ-ONLY)
Servidores **SEVENTH**: gv1p, gv2p, gv3p
Servidores **TECNOFY**: voip1

**Proibido**:
- Alterar configuracoes
- Ativar debug
- Executar comandos destrutivos
- Modificar arquivos

**Permitido**:
- Leitura de logs
- Consultas read-only
- Coleta de informacoes

## Credenciais e Acessos

**Localizacao**: `/mnt/c/claudedoc/credentials/`

**Arquivos Principais**:
- `CREDENTIALS_MASTER.md` - Todas as credenciais
- `mariadb_credentials.txt` - Banco local
- `sipservers_asterisk_db_credentials.txt` - Asterisk DBs

**Banco Local (Inventario)**:
```bash
mysql -u claude_aws -pclaudepswd aws-servers
```

**AWS CLI**:
- Access Key: AKIAXZOSOZL4MGKGXF4X
- Region: sa-east-1

## Documentacao Chave

**Mapeamentos Completos**:
- `/mnt/c/claudedoc/aws-infrastructure-milestone-1.md`
- `/mnt/c/claudedoc/VOIP1_COMPLETE_INVENTORY.md`
- `/mnt/c/claudedoc/seventh-environment-consolidated.md`
- `/mnt/c/claudedoc/gv2p-complete-mapping.md`
- `/mnt/c/claudedoc/voip1-complete-mapping.md`

**Procedimentos**:
- `/mnt/c/claudedoc/SSL-LETSENCRYPT-STANDARD.md`
- `/mnt/c/claudedoc/credentials/PREMISSAS_BASICAS_SERVIDORES.md`

**Lista de Clientes**:
- `/mnt/c/claudedoc/arimonitor_v2.7/s3aws/revendas.txt`

## Padroes de Documentacao

**SEMPRE**:
- Documentacao em portugues SEM acentuacao ou caracteres especiais
- Respeitar ambientes READ-ONLY
- Pedir permissao antes de alteracoes em producao
- Testar em VOIP3 antes de aplicar em producao