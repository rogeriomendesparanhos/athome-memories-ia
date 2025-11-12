# CREDENTIALS MASTER - Sanitized Version

**AVISO:** Este arquivo contem apenas estrutura e placeholders.
**Credenciais reais ficam apenas localmente em `/mnt/c/claudedoc/credentials/`**

---

## GitHub

**Usuario:** rogeriomendesparanhos
**Token:** ghp_XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX

**Clone URL:**
```bash
git clone https://<TOKEN>@github.com/rogeriomendesparanhos/<REPO>.git
```

---

## AWS

**Access Key ID:** AKIAXXXXXXXXXXXX
**Secret Access Key:** (local only)
**Region:** sa-east-1 (Sao Paulo)

**Config:**
```bash
aws configure
# Access Key: [local only]
# Secret Key: [local only]
# Region: sa-east-1
# Format: json
```

---

## SSH Servidores

**Usuario:** root
**Senha:** [local only]
**KeyPair:** KeyPair_2024.pem (local only)

**Hosts:**
- gv1p (18.228.54.15)
- gv2p (52.67.72.145)
- gv3p (18.228.236.165)
- voip1 (54.207.156.75)
- voip3 (56.124.7.66)
- audio (54.232.201.186)
- audio2 (18.228.131.217)

---

## Gmail Service Accounts

### voip.audios.service@gmail.com
**Email:** voip.audios.service@gmail.com
**App Password:** [local only]
**Uso:** AUDIO2 monitor de emails

### rogerio.paranhos@gmail.com
**Email:** rogerio.paranhos@gmail.com
**App Password:** [local only]
**Uso:** AUDIO monitor (dormindo), destinatario principal

---

## MariaDB

### Banco Local (Inventario)
**Host:** localhost
**Database:** aws-servers
**Usuario:** claude_aws
**Senha:** [local only]

### Asterisk Databases (Servidores)
**Banco:** asterisk
**Usuario:** asteriskuser
**Senha:** [local only]

---

## AMI Credentials

### VOIP3
**Usuario:** claudeami
**Secret:** [local only]
**Port:** 5038

### Paranhos User
**Usuario:** paranhos
**Secret:** [local only]

---

## Tokens de Seguranca

### AUDIO/AUDIO2 Token Validation
**Localizacao:** `security/tokens.json` (em cada projeto)
**Formato:** 8 digitos hexadecimais
**Exemplo:** [A1B2C3D4]

**Tokens ativos:** 10 tokens sincronizados

---

## Localizacao dos Arquivos Reais

**Path local:** `/mnt/c/claudedoc/credentials/`

**Arquivos principais:**
- CREDENTIALS_MASTER.md (completo, local only)
- github-credentials.txt
- aws_credentials
- mariadb_credentials.txt
- sipservers_asterisk_db_credentials.txt
- voip_audios_service_gmail.txt
- voip3_ami_claudeami_user1.txt
- paranhos-ami-credentials.txt

---

**NUNCA commitar credenciais reais no Git!**
**Versao sanitizada:** Para versionamento
**Versao real:** Apenas local
