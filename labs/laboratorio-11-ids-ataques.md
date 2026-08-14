# Laboratório 11 — Detecção de ataques com IDS

## Objetivo

Capturar e analisar tráfego malicioso, detectar padrões de ataque e responder.

---

## Parte 1 — Simulação de ataque: Port scanning

### Passo 1: Fazer port scan

```bash
# Linux/Mac
nmap 192.168.0.0/24 -p 1-1000

# Isso envia pacotes SYN para descobrir portas abertas
```

### Passo 2: Capturar em Wireshark

Use filtro:
```
tcp.flags.syn == 1 and not tcp.flags.ack == 1
```

### Passo 3: Analisar padrão

Registre:
- IP origem do scan: _______________
- Portas alvo: _______________
- Intervalo de tempo: _______________
- Quantos pacotes por segundo: _______________

**Pergunta**: Isso é ataque ou administrativo?
- [ ] Ataque (origem desconhecida, sem autorização)
- [ ] Administrativo (origem conhecida, autorizado)

---

## Parte 2 — Simulação de ataque: SYN flood

### Passo 1: Entenda o ataque

O atacante envia centenas de SYN sem completar handshake, lotando a fila do servidor.

```
Atacante: SYN, SYN, SYN, SYN, SYN... (mas nunca envia ACK)
Servidor: fica esperando ACK, fila cheia
Usuário legítimo: conexão recusada
```

### Passo 2: Detectar em Wireshark

Filtro:
```
tcp.flags.syn == 1 and tcp.flags.ack == 0
```

### Passo 3: Registre padrão suspeito

```
Protocolo: TCP
Flags: SYN (nunca ACK)
Origem: vários IPs? [ ] sim [ ] não
Destino: mesma porta? [ ] sim [ ] não
Taxa: pacotes por segundo: _______
Indicador de ataque: [x] sim [ ] não
```

---

## Parte 3 — Simulação de ataque: DNS amplification

### Conceito

Atacante envia query DNS falsificando IP origen (vítima). Servidor DNS responde em volume grande para vítima.

---

### Passo 1: Monitorar DNS

Filtro Wireshark:
```
dns.response.code == 0
```

### Passo 2: Detectar anomalia

Pergunte-se:
- [ ] há muito DNS para o mesmo IP destino?
- [ ] o tamanho da resposta é anormalmente grande?
- [ ] muitos IPs distintos enviando DNS?

### Passo 3: Classificar

```
Origem: _______________
Destino: _______________
Taxa de query DNS: _____ por segundo
Taxa de resposta: _____ bytes por segundo
Classificação: [ ] normal [ ] suspeito [x] ataque
```

---

## Parte 4 — Simulação de ataque: SQL Injection

### Passo 1: Entenda o ataque

URL legítima:
```
http://app.example.com/login.php?user=admin&password=senha
```

Ataque SQL Injection:
```
http://app.example.com/login.php?user=admin' OR '1'='1&password=qualquer
```

### Passo 2: Capturar HTTP

Filtro:
```
http.request.uri contains "union" or
http.request.uri contains "select" or
http.request.uri contains "drop" or
http.request.uri contains "--"
```

### Passo 3: Analisar

Registre:
```
URL alvo: _______________
Parâmetro suspeito: _______________
Payload SQL: _______________
Origem: _______________
Frequência: _______________
```

---

## Parte 5 — Análise com Wireshark

### Passo 1: Capture tráfego suspeito

```bash
tcpdump -i eth0 -w suspicious.pcap "tcp port 80 or tcp port 443 or tcp port 22"
```

### Passo 2: Abra em Wireshark

Procure por:
```
Conexões múltiplas do mesmo IP para portas diferentes
Requisições HTTP com padrões estranhos
TLS handshake incompleto (possível scan)
Retransmissões anormais (possível DoS)
```

### Passo 3: Extraia evidência

Quais são as 3 coisas mais suspeitas que você vê?

1. _______________
2. _______________
3. _______________

---

## Parte 6 — Plano de resposta

Se você detectasse esse ataque em produção, qual seria seu plano?

```
T+0 min: Recebeu alerta
         [ ] notificar SOC
         [ ] abrir ticket

T+5 min: Investigação
         [ ] confirmar ataque
         [ ] identificar origem
         [ ] medir impacto

T+15 min: Contenção
         [ ] bloquear IP origem no firewall
         [ ] ativar WAF
         [ ] isolar servidor afetado

T+30 min: Erradicação
         [ ] patcher vulnerabilidade
         [ ] mudar passwords
         [ ] verificar se há backdoor

T+60 min: Recuperação
         [ ] restaurar serviço
         [ ] validar normalidade
         [ ] comunicar usuários

T+1440 min: Post-mortem
         [ ] documentar causa raiz
         [ ] atualizar procedures
         [ ] treinar equipe
```

---

## Parte 7 — Relatório de investigação

Crie um relatório formal contendo:

1. **Resumo do incidente**:
   - tipo de ataque
   - quando foi detectado
   - qual foi o impacto

2. **Timeline**:
   - T+0: alerta
   - T+X: ação
   - T+Y: resolução

3. **Técnica de ataque**:
   - como funciona
   - qual é o objetivo
   - como foi detectado

4. **Evidência (Wireshark)**:
   - screenshot do padrão suspeito
   - filtro usado
   - interpretação

5. **Causa raiz**:
   - por que foi possível?
   - qual é a vulnerabilidade?
   - como prevenir?

6. **Recomendações**:
   - IDS rule para detectar novamente
   - patch de segurança
   - mudança de arquitetura
   - treinamento de pessoal

---

## Gabarito orientativo

### Detecção de port scan

Padrão: múltiplos SYN para portas diferentes, intervalo curto, sem completar conexão
Ação: adicionar à blacklist, investigar origem

### Detecção de SYN flood

Padrão: centenas de SYN da mesma origem, nenhum ACK, mesma porta destino
Ação: ativar SYN cookies, rate limiting, bloquear IP

### Detecção de SQL Injection

Padrão: URL com "UNION SELECT" ou "1'=" ou "-- " ou "DROP"
Ação: ativar WAF, patcher aplicação, muda senha DB, investigar se foi explorado

### Resposta esperada

Qualquer ataque detectado → bloquear → investigar → corrigir vulnerabilidade
