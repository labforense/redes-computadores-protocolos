# Laboratório 08 — TLS/HTTPS e certificados

## Objetivo

Aprender a validar certificados, verificar informações de TLS e identificar problemas de segurança em conexões HTTPS.

---

## Parte 1 — Inspeção de certificado no navegador

### Passo 1: Acessar um site HTTPS

Acesse https://www.example.com (ou qualquer site HTTPS)

### Passo 2: Ver o certificado

Clique no cadeado próximo à URL e selecione "Ver certificado" (a opção varia por navegador).

### Passo 3: Registrar informações

Anote as seguintes informações:

- **Domínio (Subject CN)**: _______________
- **Data de validade (Not Before)**: _______________
- **Data de expiração (Not After)**: _______________
- **Issuer (CA que assinou)**: _______________
- **Algoritmo de assinatura**: _______________
- **Número de série**: _______________
- **Fingerprint SHA-256**: _______________

---

## Parte 2 — Validação de certificado

Responda as seguintes perguntas:

1. O certificado é válido hoje? Ele está expirado?
2. O domínio no certificado corresponde ao site que você acessou?
3. Qual é a CA que assinou o certificado? Você confia nela?
4. Há certificados intermediários? Quantos?

---

## Parte 3 — Verificação com linha de comando

### No Windows

```powershell
# Usando PowerShell
$cert = (Get-WebSslCertificate -Uri "https://www.example.com")
$cert | Format-List -Property *
```

### No Linux/Mac

```bash
# Usando OpenSSL
openssl s_client -connect www.example.com:443 -servername www.example.com
```

### Informações importantes

Procure por:
- `Issuer`
- `Subject`
- `Validity (Not Before / Not After)`
- `Public-Key: RSA (bits)`
- `Signature Algorithm`

---

## Parte 4 — Análise no Wireshark

### Passo 1: Capturar tráfego

1. Abra Wireshark
2. Inicie captura
3. Acesse https://www.example.com
4. Pare captura

### Passo 2: Filtrar por TLS

```
tls or ssl
```

### Passo 3: Observar o handshake

1. Encontre `Client Hello`
2. Encontre `Server Hello`
3. Encontre `Certificate`
4. Encontre `Client Key Exchange` (TLS 1.2) ou `Finished` (TLS 1.3)

### Passo 4: Analisar certificado

Clique na mensagem `Certificate` e expanda:
- Verifique qual certificado está sendo enviado
- Verifique se há intermediários
- Compare com o certificado do navegador

---

## Parte 5 — Identificando problemas

Para cada cenário, o que você veria em Wireshark?

### Cenário A: Certificado expirado

Ação esperada: o navegador mostra aviso, o TLS pode não estabelecer

**O que ver em Wireshark**:
- _______________

---

### Cenário B: Nome do domínio não corresponde

Ação esperada: o navegador mostra aviso

**O que ver em Wireshark**:
- _______________

---

### Cenário C: CA desconhecida

Ação esperada: o navegador mostra aviso de "autoridade desconhecida"

**O que ver em Wireshark**:
- _______________

---

### Cenário D: Handshake TLS bem-sucedido

Ação esperada: conexão segura estabelecida, dados criptografados

**O que ver em Wireshark**:
- ClientHello
- ServerHello
- Certificate
- ChangeCipherSpec + Finished
- Encrypted Application Data

---

## Parte 6 — Teste de algoritmos de criptografia

### Usando OpenSSL

```bash
# Verificar cipher suites suportados
openssl s_client -connect www.example.com:443 -cipher ALL
```

### Registrar

- Qual é o cipher suite negociado?
- Qual é o algoritmo de key exchange (ECDHE, DHE, etc)?
- Qual é a criptografia (AES-GCM, ChaCha20, etc)?
- Qual é o hash (SHA-256, SHA-384, etc)?

---

## Parte 7 — Relatório final

Escreva um relatório contendo:

1. **Certificado analisado**:
   - domínio
   - CA
   - data de expiração
   - algoritmo

2. **Validação**:
   - o certificado é válido?
   - o domínio corresponde?
   - há problemas óbvios?

3. **Handshake TLS**:
   - qual versão de TLS?
   - qual cipher suite?
   - quanto tempo levou o handshake?

4. **Segurança**:
   - os algoritmos são modernos?
   - há vulnerabilidades conhecidas?
   - há recomendações de melhoria?

---

## Gabarito orientativo

### Resposta esperada para Wireshark

**Cenário A (expirado)**: o certificado será visto, mas o navegador o rejeitará. No Wireshark, você veria o Certificate, mas o navegador mostraria erro.

**Cenário B (domínio não corresponde)**: no Wireshark você veria o Certificate com outro CN. O navegador mostraria erro de mismatch.

**Cenário C (CA desconhecida)**: o Wireshark mostraria o Certificate, mas não é assinado por uma CA conhecida.

**Cenário D**: sequência completa do handshake visível.

### Bom relatório inclui

- identificação correta do certificado
- validação baseada em fatos (datas, nomes, assinatura)
- entendimento do fluxo TLS
- recomendações de segurança reais
