# ATHome Memories IA

**Repositorio de contexto e memoria para Inteligencia Artificial**

Este repositorio contem toda documentacao historica, analises tecnicas e contexto necessario para IAs trabalharem nos projetos ATHome VoIP.

---

## Objetivo

Fornecer contexto completo e organizado para qualquer IA (Claude, ChatGPT, etc) que precise:
- Retomar trabalho em projetos existentes
- Acessar historico de decisoes tecnicas
- Entender infraestrutura VoIP completa
- Consultar sessoes anteriores de desenvolvimento

---

## Estrutura

```
athome-memories-ia/
├── claude/                         # Documentacao para IA
│   ├── projetos/                   # Docs especificos de cada projeto
│   │   ├── arimonitor-v3/
│   │   ├── athome-voip-monitor/
│   │   ├── voip-infrastructure/
│   │   └── outros/
│   │
│   ├── sessoes/                    # Historico de sessoes por data
│   │   └── 2025/
│   │       ├── 08-agosto/
│   │       ├── 09-setembro/
│   │       ├── 10-outubro/
│   │       └── 11-novembro/
│   │
│   ├── analises/                   # Analises tecnicas cross-project
│   ├── referencias/                # Procedimentos e padroes gerais
│   ├── credentials/                # Credenciais (sanitizadas)
│   │
│   ├── CLAUDE.md                   # Arquivo raiz - menu inicial
│   ├── README.md                   # Este arquivo
│   └── ORGANIZACAO.md              # Regras de organizacao
│
└── README.md                       # Overview do repositorio
```

---

## Para IAs

### Primeira Vez no Repositorio
1. Ler `claude/CLAUDE.md` - Arquivo raiz com menu inicial
2. Ler `claude/ORGANIZACAO.md` - Entender estrutura
3. Apresentar menu de 4 opcoes ao usuario

### Retomando Trabalho
1. Ler `claude/CLAUDE.md`
2. Apresentar menu de opcoes
3. Aguardar escolha do usuario

### Adicionando Nova Documentacao
1. Consultar `claude/ORGANIZACAO.md` para saber onde guardar
2. Seguir padroes de nomenclatura
3. Commitar com mensagem descritiva

---

## Para Humanos

Este repositorio e **exclusivamente para IAs**.

Se voce e um desenvolvedor humano procurando documentacao de usuario, veja:
- Docs de usuario estao na pasta `docs/` de cada projeto especifico
- Este repo contem apenas contexto historico e tecnico para IAs

---

## Projetos Relacionados

**Repositorios Git:**
- [arimonitor-v3](https://github.com/rogeriomendesparanhos/arimonitor-v3) - Console monitoramento ARI
- [athome-voip-monitor](https://github.com/rogeriomendesparanhos/athome-voip-monitor) - Sistema processamento audios

**Cada projeto tem sua pasta `claudedoc/` ou `.claudedoc/` com documentacao especifica.**

---

## Infraestrutura

### Servidores de Producao
- **gv1p, gv2p, gv3p:** Seventh (legacy chan-sip) - READ ONLY
- **voip1:** Tecnofy/Ciencia Digital (pjsip+webrtc) - READ ONLY

### Servidores de Desenvolvimento
- **voip3:** Desenvolvimento e testes
- **audio:** Processamento local de audios
- **audio2:** Processamento S3 + roteamento

### AWS
- **Regiao:** sa-east-1 (Sao Paulo)
- **8 instancias EC2**
- **130+ empresas**, 1500+ ramais

---

## Seguranca

### Credenciais Neste Repo
- Apenas versoes **sanitizadas** (placeholders)
- Credenciais reais ficam **apenas localmente**
- Verificar `.gitignore` antes de commitar

### Regras de Acesso
- Producao: **READ-ONLY** (gv1p, gv2p, gv3p, voip1)
- Desenvolvimento: Acesso completo (voip3, audio, audio2)
- SSH: `root@servidor` (senha padrao universal)

---

## Versionamento

### Commits
Seguir convencao:
- `docs(sessoes): adiciona sessao YYYY-MM-DD`
- `docs(projetos): atualiza arquitetura projeto`
- `docs(analises): adiciona analise capacidade`
- `docs(referencias): adiciona procedimento padrao`

### Branches
- `main` - Branch principal (estavel)
- `dev` - Desenvolvimento (se necessario)

---

## Manutencao

### Quando Atualizar Documentacao

**Apos cada sessao de trabalho:**
- Adicionar arquivo em `claude/sessoes/YYYY/MM-mes/`

**Quando projeto evolui:**
- Atualizar docs em `claude/projetos/PROJETO/`

**Nova analise tecnica:**
- Adicionar em `claude/analises/`

**Novo procedimento:**
- Adicionar em `claude/referencias/`

---

## Contato

**Repositorio:** https://github.com/rogeriomendesparanhos/athome-memories-ia
**Empresa:** ATHome
**Data Criacao:** 2025-11-12
**Versao:** 1.0

---

**Este repositorio e a memoria viva dos projetos ATHome VoIP.**
