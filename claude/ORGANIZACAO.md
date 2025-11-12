# Organizacao - ATHome Memories IA

**Documentacao exclusiva para Inteligencia Artificial**

Este documento define ONDE cada tipo de arquivo deve ser guardado no repositorio.

---

## Estrutura de Pastas

```
claude/
├── projetos/                    # Documentacao especifica de cada projeto
│   ├── arimonitor-v3/          # Tudo relacionado ao ARIMONITOR v3
│   ├── athome-voip-monitor/    # Tudo relacionado ao monitor de emails/audios
│   ├── voip-infrastructure/    # Infraestrutura VoIP geral (servidores, inventario)
│   └── outros/                 # Projetos menores ou experimentais
│
├── sessoes/                    # Historico de sessoes com IA por data
│   └── 2025/
│       ├── 08-agosto/
│       ├── 09-setembro/
│       ├── 10-outubro/
│       └── 11-novembro/
│
├── analises/                   # Analises tecnicas cross-project
│
├── referencias/                # Procedimentos, padroes, guias gerais
│
├── credentials/                # Credenciais sanitizadas (nao comitar senhas reais)
│
├── CLAUDE.md                   # Arquivo raiz - menu inicial de toda sessao
├── README.md                   # Explicacao do repositorio
└── ORGANIZACAO.md              # Este arquivo
```

---

## Regras de Classificacao

### 1. projetos/

**Quando usar:**
- Documentacao especifica de UM projeto
- Analise de codigo de um projeto
- Arquitetura/decisoes de um projeto especifico
- Mapeamento completo de um projeto

**Exemplos:**
- `projetos/arimonitor-v3/POINTER.md` (redirecionamento para repo)
- `projetos/athome-voip-monitor/PROJETO.md`
- `projetos/voip-infrastructure/GV2P_COMPLETE_MAPPING.md`
- `projetos/voip-infrastructure/VOIP1_COMPLETE_INVENTORY.md`

**Criterio:**
Se o arquivo menciona especificamente um projeto/servidor no titulo ou conteudo → vai para `projetos/`

**IMPORTANTE - PROJETOS COM REPOSITORIO GIT PROPRIO:**
Alguns projetos possuem repositorio Git proprio e mantem documentacao Claude dentro do repo (pasta `claudedoc/`). Nesses casos:
- A pasta `projetos/PROJETO/` contem apenas `POINTER.md` (redirecionamento)
- Documentacao antiga arquivada em `projetos/PROJETO/_archived/`
- Documentacao atual vive no repositorio do projeto

Projetos com claudedoc/ proprio:
- arimonitor-v3 → https://github.com/rogeriomendesparanhos/arimonitor-v3/claudedoc/
- athome-voip-monitor → https://github.com/rogeriomendesparanhos/athome-voip-monitor/.claudedoc/

---

### 2. sessoes/

**Quando usar:**
- Transcript de sessao completa com IA
- Resumo de sessao de trabalho
- Log de atividades realizadas em uma data

**Padrao de nome:**
`YYYY-MM-DD-projeto-assunto.md`

**Exemplos:**
- `sessoes/2025/11-novembro/2025-11-10-audio-python2.md`
- `sessoes/2025/11-novembro/2025-11-11-audio2-roteamento.md`
- `sessoes/2025/08-agosto/2025-08-15-arimonitor-v3-sessao.md`

**Criterio:**
Se tem data no nome OU e transcript de conversa → vai para `sessoes/YYYY/MM-mes/`

---

### 3. analises/

**Quando usar:**
- Analise tecnica que envolve MULTIPLOS projetos/servidores
- Comparativos entre sistemas
- Estudos de capacidade/performance cross-system
- Analises de seguranca gerais

**Exemplos:**
- `analises/ANALISE_COMPLETA_SISTEMAS_EXISTENTES.md`
- `analises/ARIMONITOR_V2.7_vs_V3.0_COMPARATIVA.md`
- `analises/CAPACIDADE_VOIP1_2025-09-17.md`
- `analises/FLAPS_PJSIP_VOIP1_2025-09-17.md`

**Criterio:**
Se analisa/compara multiplos sistemas OU e analise cross-project → vai para `analises/`

---

### 4. referencias/

**Quando usar:**
- Procedimentos padrao (nao especifico de um projeto)
- Guias de setup genericos
- Instrucoes de seguranca
- Premissas basicas de infraestrutura
- Padroes e boas praticas

**Exemplos:**
- `referencias/SSL-LETSENCRYPT-STANDARD.md`
- `referencias/PREMISSAS_BASICAS_SERVIDORES.md`
- `referencias/HOSTS_UPDATE_INSTRUCTIONS.md`
- `referencias/FAIL2BAN_STANDARD_SETUP.md`

**Criterio:**
Se e procedimento/padrao reutilizavel em multiplos contextos → vai para `referencias/`

---

### 5. credentials/

**Quando usar:**
- Informacoes de credenciais
- Tokens de API
- Chaves SSH
- Configuracoes de acesso

**IMPORTANTE:**
- NAO commitar credenciais reais/sensiveis
- Usar placeholders ou versoes sanitizadas
- Manter versoes reais apenas localmente

**Exemplos:**
- `credentials/CREDENTIALS_MASTER.md` (sanitizado)
- `credentials/github-credentials.txt.example`
- `credentials/aws-credentials.example`

**Criterio:**
Se contem ou referencia credenciais → vai para `credentials/` (e verificar .gitignore)

---

## Casos Especiais

### Arquivo se encaixa em multiplas categorias?
**Prioridade:**
1. Se e sessao com data → `sessoes/`
2. Se e especifico de 1 projeto → `projetos/PROJETO/`
3. Se e analise cross-project → `analises/`
4. Se e procedimento generico → `referencias/`

### Arquivo muito antigo ou obsoleto?
- Avaliar se ainda e relevante
- Se sim: organizar normalmente
- Se nao: pode descartar ou criar `projetos/outros/arquivados/`

### Documentacao de projeto que tem repo proprio?
- Manter documentacao `.md` de sessoes/analises AQUI
- Codigo vai no repo especifico do projeto
- Este repo e para CONTEXTO, nao codigo

---

## Workflow de Nova Documentacao

Quando criar novo documento `.md`:

1. **Identificar tipo:**
   - E sessao? → `sessoes/YYYY/MM-mes/`
   - E especifico de projeto? → `projetos/PROJETO/`
   - E analise geral? → `analises/`
   - E procedimento? → `referencias/`

2. **Nomear adequadamente:**
   - Sessoes: `YYYY-MM-DD-projeto-assunto.md`
   - Outros: `TOPICO_DESCRITIVO.md` ou `topico-descritivo.md`

3. **Commitar com mensagem clara:**
   - `docs(sessoes): adiciona sessao 2025-11-12 arimonitor`
   - `docs(projetos): atualiza arquitetura voip-monitor`
   - `docs(analises): adiciona analise capacidade servidores`

---

## Atualizacao deste Documento

Este arquivo deve ser atualizado quando:
- Nova categoria de pasta for criada
- Regra de classificacao mudar
- Novos padroes forem adotados

**Data ultima atualizacao:** 2025-11-12
**Versao:** 1.0
