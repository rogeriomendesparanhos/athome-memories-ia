# PREMISSAS BÁSICAS PARA NOVOS SERVIDORES

**Data**: 14/08/2025  
**Aplicável**: Todos os servidores Linux criados  

---

## 🔐 **1. CONFIGURAÇÃO SSH PADRÃO**

### Chave SSH Local
- **Localização**: `~/.ssh/id_ed25519.pub`
- **Comando**: `ssh-copy-id -i ~/.ssh/id_ed25519.pub root@[IP_SERVER]`
- **Objetivo**: Acesso direto sem KeyPair AWS

### Após Configuração
- **Acesso direto**: `ssh root@[IP_SERVIDOR]`
- **Sem necessidade** de `-i KeyPair_2024.pem`
- **Mais seguro** e **mais prático**

---

## 🌐 **2. CONFIGURAÇÃO DNS LOCAL**

### Arquivo Hosts Windows
- **Local**: `C:\Windows\System32\drivers\etc\hosts`
- **Formato**: `[IP] [NOME].tecnofy.com.br [NOME]`
- **Exemplo**: `56.124.7.66 voip3.tecnofy.com.br voip3`

### Requer Administrador
- **PowerShell Admin**: `Add-Content -Path "C:\Windows\..." -Value "[ENTRADA]"`
- **Ou Notepad** como Administrador

### Após Configuração
- **Acesso por nome**: `ssh root@voip3`
- **Ping funcional**: `ping voip3.tecnofy.com.br`
- **URLs locais**: `http://voip3:8088/ws`

---

## 📋 **3. PROCESSO PADRONIZADO**

### Para CADA servidor novo:

#### A. Configuração SSH
```bash
# 1. Copiar chave (manual ou automatizado)
ssh-copy-id -i ~/.ssh/id_ed25519.pub root@[IP]

# 2. Testar acesso
ssh root@[IP] "echo 'SSH OK' && hostname"
```

#### B. Configuração DNS
```powershell
# Como Admin no Windows
Add-Content -Path "C:\Windows\System32\drivers\etc\hosts" -Value "[IP] [NOME].tecnofy.com.br [NOME]"
```

#### C. Documentação
```bash
# Atualizar CREDENTIALS_MASTER.md
# Adicionar ao banco aws-servers se necessário
# Criar checkpoint se servidor crítico
```

---

## 🛠️ **4. SCRIPTS AUTOMATIZADOS**

### Script Padrão
- **Arquivo**: `/mnt/c/claudedoc/credentials/server_setup_standard.sh`
- **Uso**: `./server_setup_standard.sh [IP] [NOME] [SENHA]`
- **Exemplo**: `./server_setup_standard.sh 56.124.7.66 voip3 mdansk`

### Funcionalidades
- ✅ Verifica chave SSH local
- ✅ Copia chave para servidor
- ✅ Tenta atualizar hosts (precisa admin)
- ✅ Documenta credenciais
- ✅ Fornece instruções manuais

---

## 🎯 **5. BENEFÍCIOS PADRONIZAÇÃO**

### Acesso Simplificado
- **Antes**: `ssh -i ~/.ssh/voip3_key.pem ubuntu@56.124.7.66`
- **Depois**: `ssh root@voip3`

### URLs Funcionais
- **Antes**: `wss://56.124.7.66:8089/ws`
- **Depois**: `wss://voip3.tecnofy.com.br:8089/ws`

### Manutenção
- **IPs fixos** via Elastic IP
- **Nomes consistentes** 
- **Documentação centralizada**

---

## ⚠️ **6. REQUISITOS CRÍTICOS**

### Obrigatórios
- [x] **Elastic IP** alocado ANTES da configuração
- [x] **SSH root** habilitado com senha
- [x] **Chave local** `~/.ssh/id_ed25519` existente
- [x] **Privileges Admin** no Windows para hosts

### Recomendados
- [x] **Backup** das configurações
- [x] **Teste** de conectividade
- [x] **Documentação** atualizada
- [x] **Database** sincronizado

---

## 📞 **7. TROUBLESHOOTING**

### SSH não funciona sem keypar
```bash
# Verificar se chave foi copiada
ssh root@[IP] "cat ~/.ssh/authorized_keys | grep paranhos"
```

### DNS não resolve
```bash
# Verificar arquivo hosts
cat /mnt/c/Windows/System32/drivers/etc/hosts | grep [NOME]

# Testar resolução
nslookup [NOME].tecnofy.com.br
```

### Permissão negada no hosts
- **Solução**: Executar PowerShell/Notepad como Administrador
- **Alternativa**: Usar `servidor.tecnofy.local` no `/etc/hosts` do WSL

---

**ESTA PREMISSA DEVE SER SEGUIDA PARA TODOS OS SERVIDORES FUTUROS!**