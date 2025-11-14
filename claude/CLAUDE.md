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
- Ler `/mnt/c/claudedoc/athome-memories-ia/claude/ORGANIZACAO.md`
- Ler `/mnt/c/claudedoc/athome-memories-ia/claude/CLAUDE.md`
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
- Buscar documentacao em `athome-memories-ia/claude/projetos/PROJETO/`
- Ler arquivos relevantes do projeto
- Se projeto tem repo git proprio, buscar `claudedoc/` dele
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

## Diretrizes de Interface

### REGRA #1: OBJETIVIDADE ABSOLUTA
**NUNCA mostre codigo ou resultado de codigo a menos que explicitamente solicitado.**

**Comportamento correto:**
- Executou comando? Diga apenas "Feito" ou resultado relevante
- Leu arquivo? Diga apenas o que encontrou de importante
- Criou/editou codigo? Diga apenas "Arquivo X atualizado"
- Erro? Mostre APENAS a mensagem de erro, nao o codigo completo

**Proibido:**
- Exibir blocos de codigo sem solicitacao
- Mostrar output completo de comandos
- Copiar conteudo de arquivos na resposta
- Encher tela com informacao desnecessaria

### REGRA #2: RESPOSTA PONTUAL
- Uma pergunta = Uma resposta direta
- NAO liste possibilidades
- NAO antecipe necessidades
- NAO sugira "proximos passos" sem ser perguntado

### REGRA #3: MODO CONVERSA
Quando usuario entra em modo conversa:
- Responda APENAS o que foi perguntado
- NAO liste alternativas ou caminhos possiveis
- Deixe usuario conduzir o raciocinio
- Foco: construir conceito mutuo passo a passo

### REGRA #4: PREVENCAO DE AUTO-COMPACT
- Tela limpa = contexto preservado
- Verbosidade excessiva causa perda de memoria
- Respostas curtas mantem historico acessivel
- Trabalho importante nao pode ser perdido

**LEMBRE-SE:** Usuario sabe o que quer. Apenas responda o solicitado.

---

## Repositorio de Memoria (athome-memories-ia)

**Localizacao Local:** `/mnt/c/claudedoc/athome-memories-ia/`
**GitHub:** https://github.com/rogeriomendesparanhos/athome-memories-ia

**Estrutura:**
```
athome-memories-ia/
└── claude/
    ├── CLAUDE.md              # Menu inicial
    ├── ORGANIZACAO.md         # Regras de organizacao
    ├── projetos/              # Docs especificos por projeto
    │   ├── arimonitor-v3/
    │   ├── athome-voip-monitor/
    │   ├── voip-infrastructure/
    │   └── outros/
    ├── sessoes/               # Historico de sessoes por data
    │   └── 2025/
    ├── analises/              # Analises cross-project
    ├── referencias/           # Procedimentos e padroes
    └── credentials/           # Credenciais sanitizadas
```

**Para consultar documentacao completa:**
Ler `athome-memories-ia/claude/ORGANIZACAO.md`

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
- Repo: https://github.com/rogeriomendesparanhos/arimonitor-v3

**athome-voip-monitor:**
- Sistema de processamento de audios via email
- Dual-mode: audio (Python 2.7) + audio2 (Python 3.8)
- Roteamento inteligente (local vs S3)
- Status: 100% funcional em producao
- Repo: https://github.com/rogeriomendesparanhos/athome-voip-monitor

**voip-infrastructure:**
- Mapeamento completo de todos servidores
- Inventarios de ramais, trunks, rotas
- Infraestrutura AWS EC2
- 130+ empresas, 1500+ ramais

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
- Senha universal: mdansk

### Documentacao
- Sempre em portugues SEM acentuacao
- Respeitar ambientes READ-ONLY
- Pedir permissao antes de alteracoes em producao
- Testar em voip3/audio/audio2 antes de aplicar em producao

---

## Repositorios Git Ativos

**athome-memories-ia:**
- Contexto geral e historico
- Documentacao cross-project
- https://github.com/rogeriomendesparanhos/athome-memories-ia

**arimonitor-v3:**
- Console de monitoramento
- Pasta `claudedoc/` com docs especificos
- https://github.com/rogeriomendesparanhos/arimonitor-v3

**athome-voip-monitor:**
- Sistema de processamento audios
- Pasta `.claudedoc/` com docs especificos
- https://github.com/rogeriomendesparanhos/athome-voip-monitor

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

**Data ultima atualizacao:** 2025-11-12
**Versao:** 2.0
**Repositorio memoria:** athome-memories-ia
