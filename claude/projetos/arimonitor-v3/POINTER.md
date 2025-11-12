# ARIMONITOR-V3 - POINTER PARA DOCUMENTACAO OFICIAL

**Data:** 2025-11-12
**Status:** REDIRECIONAMENTO

---

## ⚠️ IMPORTANTE: DOCUMENTACAO MOVIDA

A documentacao oficial do projeto **arimonitor-v3** agora reside **dentro do proprio repositorio do projeto**.

---

## 📍 LOCALIZACAO ATUAL

### **Repositorio GitHub:**
https://github.com/rogeriomendesparanhos/arimonitor-v3

### **Documentacao Claude (claudedoc/):**
```
arimonitor-v3/
└── claudedoc/
    ├── README.md              # Proposito e estrutura
    ├── context/
    │   └── status-atual.md    # Estado atual do projeto ⭐
    ├── sessions/              # Historico de sessoes
    ├── decisions/             # Decisoes tecnicas
    ├── references/            # Referencias e exemplos
    └── planning/              # Planejamentos futuros
```

### **Documentacao Tecnica (docs/):**
```
arimonitor-v3/
└── docs/
    ├── ARIMONITOR_V3.7_PLANNING.md        # Planejamento v3.7
    ├── ARIMONITOR_V3.6_RELEASE_FINAL.md   # Release v3.6
    └── ARIMONITOR_V3.6_ENCERRAMENTO.md    # Encerramento v3.6
```

---

## 🔄 COMO ACESSAR

### **Local (WSL):**
```bash
cd /mnt/c/claudedoc/arimonitor-v3/claudedoc/
cat context/status-atual.md
```

### **Via Git:**
```bash
git clone https://github.com/rogeriomendesparanhos/arimonitor-v3.git
cd arimonitor-v3/claudedoc/
```

---

## 📦 ARQUIVOS ARQUIVADOS

Os seguintes arquivos foram **ARQUIVADOS** (desatualizados) em `_archived/`:
- `CONTEXT-MEMORIA.md` - Contexto antigo (analise v2.7)
- `PYTHON3_ARCHITECTURE.md` - Arquitetura de migracao (nao mais relevante)

**Motivo:** Esses documentos eram de uma fase anterior do projeto (analise do v2.7 e planejamento de migracao Python 2→3) que ja foi superada. A versao v3.6 ja esta pronta e funcionando em Python 3!

---

## 🎯 PARA CLAUDE (IA)

Quando precisar do contexto do **arimonitor-v3**:

1. ✅ Acessar `/mnt/c/claudedoc/arimonitor-v3/claudedoc/context/status-atual.md`
2. ✅ Ler sessoes em `/mnt/c/claudedoc/arimonitor-v3/claudedoc/sessions/`
3. ✅ Consultar planejamento em `/mnt/c/claudedoc/arimonitor-v3/docs/`

**NAO** usar os arquivos arquivados em `_archived/` - estao desatualizados!

---

## 💡 RAZAO DA MUDANCA

**Antes:** Documentacao espalhada entre `athome-memories-ia` e repositorio do projeto

**Agora:** Documentacao vive junto com o codigo (Single Source of Truth)

**Beneficios:**
- ✅ Contexto sempre sincronizado com codigo
- ✅ Uma unica fonte da verdade
- ✅ Facil de versionar junto com o projeto
- ✅ Colaboradores acessam tudo em um lugar

---

## 📋 ESTRUTURA athome-memories-ia

Esta pasta agora contem apenas:
- `POINTER.md` (este arquivo) - Redirecionamento para repo
- `_archived/` - Documentos historicos (nao usar)

---

**Ultima atualizacao:** 2025-11-12
**Responsavel:** Claude + Paranhos
**Versao atual projeto:** v3.6 (main), v3.7 (em desenvolvimento)
