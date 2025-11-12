# INSTRUÇÕES PARA ATUALIZAR HOSTS DO WINDOWS

**Arquivo**: `C:\Windows\System32\drivers\etc\hosts`  
**Requer**: Privilégios de Administrador  

## 🔧 COMO FAZER:

### Método 1: PowerShell como Administrador
```powershell
# 1. Abrir PowerShell como Administrador
# 2. Executar:
Add-Content -Path "C:\Windows\System32\drivers\etc\hosts" -Value "56.124.7.66    voip3.tecnofy.com.br    voip3"
```

### Método 2: Notepad como Administrador
1. **Botão direito** no menu Iniciar → **Windows Terminal (Admin)**
2. Execute: `notepad C:\Windows\System32\drivers\etc\hosts`
3. **Adicione no final do arquivo**:
```
56.124.7.66    voip3.tecnofy.com.br    voip3
```
4. **Salve** o arquivo (Ctrl+S)

## ✅ TESTE APÓS ATUALIZAR:
```bash
# No WSL, teste:
ping voip3.tecnofy.com.br
ssh root@voip3
```

## 📋 PADRÃO PARA FUTUROS SERVIDORES:
```
[IP_SERVIDOR]    [NOME].tecnofy.com.br    [NOME]
```

**Exemplo**:
- VOIP1: `54.207.156.75    voip1.tecnofy.com.br    voip1`
- VOIP3: `56.124.7.66     voip3.tecnofy.com.br    voip3`