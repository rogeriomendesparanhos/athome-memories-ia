# CLAUDE.md - Arquivo Raiz de Contexto

**ATHome VoIP - Documentacao Exclusiva para IA**

---

## Menu Inicial - Todas as Sessoes

Sempre que iniciar uma nova sessao comigo, apresente estas 4 opcoes:

```
Ola! Como posso ajudar hoje?

1) Acessar contexto geral (repo athome-memories-ia)
2) Listar repositorios em atividade
3) Trabalhar em projeto especifico
4) Sessao simples (apenas memoria deste arquivo)

Escolha uma opcao ou descreva diretamente o que precisa.
```

---

## Comportamento de Cada Opcao

### Opcao 1: Acessar contexto geral
**Acao:**
- Ler `ORGANIZACAO.md` para entender estrutura
- Ler `README.md` para overview do sistema
- Listar pastas de projetos disponiveis
- Informar ao usuario o que esta disponivel

**Quando usar:**
- Usuario quer visao geral
- Precisa lembrar de projetos existentes
- Quer acessar historico de sessoes

---

### Opcao 2: Listar repositorios em atividade
**Acao:**
- Verificar pasta `/mnt/c/claudedoc/`
- Listar diretorios que sao repositorios git
- Mostrar status de cada um (branch, commits pendentes, etc)
- Informar quais tem pasta `claudedoc/` com documentacao

**Quando usar:**
- Usuario quer saber em que esta trabalhando
- Precisa escolher um repo para trabalhar
- Quer ver status geral dos projetos

---

### Opcao 3: Trabalhar em projeto especifico
**Acao:**
- Perguntar qual projeto
- Buscar documentacao em `projetos/PROJETO/`
- Ler arquivos relevantes do projeto
- Se projeto tem repo git, buscar `claudedoc/` dele
- Apresentar resumo do estado atual do projeto

**Quando usar:**
- Usuario ja sabe em que quer trabalhar
- Precisa contexto especifico de um projeto
- Vai implementar/debugar algo especifico

---

### Opcao 4: Sessao simples
**Acao:**
- Usar apenas contexto deste arquivo (CLAUDE.md)
- Nao ler outros arquivos
- Sessao leve para conversas rapidas

**Quando usar:**
- Pergunta simples/rapida
- Nao precisa de contexto de projetos
- Usuario quer apenas conversar

---

## Protocolo de Modo Conversa

Quando usuario disser:
- "quero conversar sobre [assunto]"
- "vamos entrar no modo conversa"
- "modo conversa sobre [tema]"

**Comportamento:**
- Perguntas e respostas rapidas e diretas
- Analise de pontos especificos SEM executar atividades
- Discussao conceitual antes de implementacao
- Vale para questionamentos do usuario E do Claude
- Foco em entendimento mutuo do assunto

---

## Contexto Rapido - Infraestrutura ATHome

### Ambientes

**PRODUCAO (READ-ONLY - NUNCA ALTERAR):**
- Seventh (legacy chan-sip): gv1p, gv2p, gv3p
- Tecnofy/Ciencia Digital (moderno pjsip+webrtc): voip1

**DESENVOLVIMENTO:**
- voip3 (modelo para evolucao futura)
- audio (compartilhado, processamento local)
- audio2 (compartilhado, processamento S3)

### Projetos Principais

**arimonitor-v3:**
- Console de monitoramento ARI
- Python 3.12
- Em desenvolvimento no voip3
- Status: 95% pronto

**athome-voip-monitor:**
- Sistema de processamento de audios via email
- Dual-mode: audio (Python 2.7) + audio2 (Python 3.8)
- Roteamento inteligente (local vs S3)
- Status: 100% funcional em producao

**voip-infrastructure:**
- Mapeamento completo de todos servidores
- Inventarios de ramais, trunks, rotas
- Credenciais e procedimentos
- Infraestrutura AWS EC2

### Credenciais

**Localizacao local:** `/mnt/c/claudedoc/credentials/`

**Principais:**
- GitHub: rogeriomendesparanhos
- AWS: sa-east-1 (Sao Paulo)
- SSH: root@servidor (senha: mdansk)
- Gmail: voip.audios.service@gmail.com

**IMPORTANTE:** Credenciais reais ficam apenas local. No repo Git ficam sanitizadas.

---

## Regras Importantes

### Ambientes de Producao (READ-ONLY)
**Servidores:** gv1p, gv2p, gv3p, voip1

**Proibido:**
- Alterar configuracoes
- Ativar debug
- Executar comandos destrutivos
- Modificar arquivos

**Permitido:**
- Leitura de logs
- Consultas read-only
- Coleta de informacoes

### Acesso SSH
- Sempre via `ssh root@servidor`
- NUNCA mexer nas chaves SSH sem permissao
- Senha universal: `mdansk`

### Documentacao
- Sempre em portugues SEM acentuacao
- Respeitar ambientes READ-ONLY
- Pedir permissao antes de alteracoes em producao
- Testar em voip3/audio/audio2 antes de aplicar em producao

---

## Repositorios Git

**athome-memories-ia:**
- Este repositorio
- Contexto geral e historico
- Documentacao cross-project

**arimonitor-v3:**
- https://github.com/rogeriomendesparanhos/arimonitor-v3
- Console de monitoramento
- Pasta `claudedoc/` com docs especificos

**athome-voip-monitor:**
- https://github.com/rogeriomendesparanhos/athome-voip-monitor
- Sistema de processamento audios
- Pasta `.claudedoc/` com docs especificos

---

## Como Usar Este Arquivo

**Para Claude (IA):**
- Ler este arquivo no inicio de TODA sessao
- Apresentar menu de 4 opcoes
- Aguardar escolha do usuario
- Seguir comportamento definido para opcao escolhida

**Para Paranhos (usuario):**
- Este arquivo e para IA, nao precisa ler
- Apenas diga o que quer fazer
- IA vai seguir menu automaticamente

---

**Data criacao:** 2025-11-12
**Versao:** 1.0
**Repositorio:** athome-memories-ia
