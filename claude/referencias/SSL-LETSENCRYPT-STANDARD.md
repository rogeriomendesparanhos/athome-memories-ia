# PADRÃO SSL LETSENCRYPT - PROCEDIMENTO OBRIGATÓRIO

**APLICAR EM TODOS OS SERVIDORES COM CERTIFICADOS SSL**

---

## **PROCEDIMENTO PADRÃO COMPLETO** 🔐

### **1. INSTALAÇÃO**:
```bash
sudo apt update
sudo apt install -y certbot python3-certbot-apache
```

### **2. CONFIGURAÇÃO DO CERTIFICADO**:
```bash
sudo certbot --apache -d DOMINIO.COM \
  --non-interactive \
  --agree-tos \
  --email roger@tecnofy.com.br
```

### **3. VERIFICAÇÃO DA CONFIGURAÇÃO AUTOMÁTICA**:
```bash
# Verificar timer ativo
sudo systemctl status certbot.timer

# Verificar próxima execução
sudo systemctl list-timers certbot*

# Testar renovação
sudo certbot renew --dry-run
```

---

## **CONFIGURAÇÃO AUTOMÁTICA GARANTIDA** ⚙️

### **Timer Systemd** (Configurado automaticamente):
- **Arquivo**: `/lib/systemd/system/certbot.timer`
- **Frequência**: 2x por dia (00h e 12h)
- **Randomização**: Até 12h de delay aleatório
- **Persistente**: Sim (executa se sistema estava offline)

### **Serviço de Renovação**:
- **Arquivo**: `/lib/systemd/system/certbot.service` 
- **Comando**: `/usr/bin/certbot -q renew`
- **Tipo**: oneshot (executa e finaliza)

### **Logs com Rotação**:
- **Arquivo**: `/etc/logrotate.d/certbot`
- **Localização**: `/var/log/letsencrypt/letsencrypt.log`
- **Rotação**: Semanal, 12 backups comprimidos
- **Configuração**: Automática (não requer intervenção)

---

## **COMANDOS DE MONITORAMENTO** 📊

### **Status do Sistema**:
```bash
# Verificar certificados ativos
sudo certbot certificates

# Status do timer
sudo systemctl status certbot.timer

# Logs recentes
sudo tail -50 /var/log/letsencrypt/letsencrypt.log

# Filtrar renovações
sudo grep -i "renew\|success\|error" /var/log/letsencrypt/letsencrypt.log | tail -10
```

### **Teste Manual**:
```bash
# Teste completo (dry-run)
sudo certbot renew --dry-run

# Renovação forçada (se necessário)
sudo certbot renew --force-renewal
```

### **Logs do Systemd**:
```bash
# Histórico do timer
sudo journalctl -u certbot.timer

# Histórico do serviço
sudo journalctl -u certbot.service
```

---

## **ESTRUTURA DE ARQUIVOS** 📂

### **Certificados**:
```
/etc/letsencrypt/live/DOMINIO.COM/
├── fullchain.pem    ← Certificado completo
├── privkey.pem      ← Chave privada
├── cert.pem         ← Certificado público
└── chain.pem        ← Cadeia de certificação
```

### **Logs**:
```
/var/log/letsencrypt/
├── letsencrypt.log          ← Log principal (atual)
├── letsencrypt.log.1.gz     ← Semana anterior
├── letsencrypt.log.2.gz     ← 2 semanas atrás
└── ... (até 12 rotações)
```

### **Configuração Apache**:
```
/etc/apache2/sites-available/
├── 000-default.conf         ← Site HTTP (porta 80)
└── 000-default-le-ssl.conf  ← Site HTTPS (porta 443)
```

---

## **VALIDAÇÃO FINAL OBRIGATÓRIA** ✅

### **Checklist pós-instalação**:
- [ ] **HTTPS funcionando**: `curl -I https://DOMINIO.COM`
- [ ] **Timer ativo**: `systemctl is-enabled certbot.timer`
- [ ] **Dry-run OK**: `certbot renew --dry-run`
- [ ] **Logs funcionando**: Log em `/var/log/letsencrypt/letsencrypt.log`
- [ ] **Rotação configurada**: Arquivo `/etc/logrotate.d/certbot` presente

### **Intervalos de Verificação**:
- **Imediato**: Após instalação
- **Semanal**: Verificar logs de renovação
- **Mensal**: Confirmar certificados válidos
- **Trimestral**: Teste dry-run manual

---

## **HISTÓRICO DE IMPLEMENTAÇÕES** 📋

### **Servidores com SSL Padrão**:
1. **VOIP1** (Tecnofy): ✅ Implementado via SNAP (verificado 14/10/2025)
2. **Audio2** (Primeiro IA): ✅ Implementado 2025-08-12

### **Próximos Servidores**:
- **Audio2 - Prediotech**: ⏳ Aguardando DNS
- **Novos servidores**: Aplicar este padrão obrigatoriamente

### **Observação Importante - VOIP1**:
O VOIP1 usa **certbot via SNAP** ao invés de APT:
- Timer: `snap.certbot.renew.timer` (ao invés de `certbot.timer`)
- Timer systemd tradicional está masked (normal e correto)
- Renovação automática funcionando 2x/dia
- Última verificação: 14/10/2025 - Status: ✅ 100% funcional
- Documento detalhado: `VOIP1_SSL_CERTIFICADOS_VERIFICACAO_2025-10-14.md`

---

**IMPORTANTE**: Este documento deve ser consultado SEMPRE que configurar SSL em qualquer servidor.

**Atualizado**: 2025-08-12  
**Responsável**: Claude IA + Roger  
**Status**: ✅ Padrão estabelecido e documentado