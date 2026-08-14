# Laboratório 10 — SNMP, Syslog e monitoramento

## Objetivo

Configurar monitoramento de rede com SNMP, Syslog e responder a alertas em tempo real.

---

## Parte 1 — Configuração SNMP

### Passo 1: Habilitar SNMP em um roteador/switch

```bash
# Cisco router
Router(config)# snmp-server community public RO
Router(config)# snmp-server community private RW
Router(config)# snmp-server location "São Paulo - Filial A"
Router(config)# snmp-server contact "network-ops@company.com"
Router(config)# end
```

### Passo 2: Testar query SNMP

```bash
# Do seu computador
snmpget -v 2c -c public 192.168.1.1 1.3.6.1.2.1.1.3.0
```

**Resultado esperado**:
```
iso.org.dod.internet.mgmt.mib-2.system.sysUpTime.0 = Timeticks: (123456789) X days, X:XX:XX.XX
```

### Passo 3: Registre informações coletadas

```
Uptime do roteador: _______________
Descrição do sistema: _______________
Contato: _______________
Local: _______________
```

---

## Parte 2 — Coleta de métricas críticas

### Use SNMP para consultar:

```bash
# CPU
snmpget -v 2c -c public 192.168.1.1 1.3.6.1.4.1.9.2.1.58.0

# Memória
snmpget -v 2c -c public 192.168.1.1 1.3.6.1.4.1.9.2.1.56.0

# Interface status
snmpwalk -v 2c -c public 192.168.1.1 1.3.6.1.2.1.2.2.1.5
```

### Registre os valores:

```
CPU: _____ %
Memória: _____ %
Interface 1: (up/down)
Interface 2: (up/down)
```

---

## Parte 3 — Configuração Syslog

### Passo 1: Habilitar logging

```bash
# Cisco router
Router(config)# logging 192.168.1.50
Router(config)# logging trap informational
Router(config)# logging source-interface GigabitEthernet0/0
Router(config)# end

# Salve config
Router# write memory
```

### Passo 2: Gerar um evento de log

```bash
# Derrubar e subir uma interface
Router(config)# interface Ethernet0/0
Router(config-if)# shutdown
Router(config-if)# no shutdown
Router(config-if)# end
```

### Passo 3: Verifique no servidor Syslog

Se estiver rodando rsyslog:

```bash
tail -f /var/log/syslog | grep "192.168.1.1"
```

**Resultado esperado:**
```
Aug 14 10:30:45 router-01 %LINK-3-UPDOWN: Interface Ethernet0/0, changed state to down
Aug 14 10:30:47 router-01 %LINK-3-UPDOWN: Interface Ethernet0/0, changed state to up
```

---

## Parte 4 — Interpretação de alertas

Para cada cenário, qual é o nível de severidade?

### Cenário A
```
Log: Interface Ethernet1/0 down
Mensagem: %LINK-3-UPDOWN: Interface Ethernet1/0, changed state to down
Severidade: [ ] Emergency [ ] Alert [ ] Critical [x] Error [ ] Warning
Ação recomendada: _______________
```

### Cenário B
```
Log: CPU 95%
Severidade: [ ] Emergency [x] Alert [ ] Critical [ ] Error [ ] Warning
Ação recomendada: _______________
```

### Cenário C
```
Log: Memória 65%
Severidade: [ ] Emergency [ ] Alert [ ] Critical [ ] Error [x] Warning
Ação recomendada: _______________
```

---

## Parte 5 — Análise de tráfego com NetFlow

Se disponível, configure NetFlow:

```bash
# Roteador Cisco
Router(config)# flow-sampler-map fsm-1
Router(config-sampler)# sampling-mode packet 1 out-of 100
Router(config)# interface GigabitEthernet0/0
Router(config-if)# ip flow ingress
Router(config-if)# ip flow-sampler fsm-1 ingress
```

Depois analise em ntopng ou Wireshark.

---

## Parte 6 — Criando um procedimento de resposta

Você recebe este alerta às 14h30:

```
ALERT: Interface GigabitEthernet0/1 down
Timestamp: 2024-08-14 14:30:23
Device: router-filial-b
Severity: Critical
```

### Seu workflow:

1. **Reconhecimento** (T+0)
   - [ ] acknowledge alert
   - [ ] anote timestamp

2. **Investigação** (T+5 min)
   - [ ] qual interface caiu?
   - [ ] qual é a função dessa interface?
   - [ ] qual é o impacto?
   - [ ] o roteador de backup está online?

3. **Diagnóstico** (T+15 min)
   - [ ] interface foi resetada?
   - [ ] há erro de configuração?
   - [ ] há problema de hardware?
   - [ ] há problema de cabo?

4. **Resolução** (T+30 min)
   - [ ] reiniciar interface
   - [ ] se não funcionar, ativar link backup
   - [ ] se não funcionar, escalate para on-site

5. **Documentação** (T+60 min)
   - [ ] escreva ticket com causa raiz
   - [ ] registre mudanças
   - [ ] comunique stakeholders

---

## Parte 7 — Relatório final

Escreva um relatório contendo:

1. **Configuração realizada**:
   - SNMP versão e community string
   - Syslog servidor e porta
   - NetFlow (se disponível)

2. **Métricas coletadas**:
   - CPU, memória
   - Status de interfaces
   - Uptime

3. **Eventos observados**:
   - quais eventos foram coletados?
   - qual foi a severidade?
   - como você responderia?

4. **Recomendações**:
   - qual é o intervalo ideal de poll SNMP?
   - quais são os limites de alerta?
   - qual é o plano de resposta?

---

## Gabarito orientativo

### Resposta esperada para CPU 95%

Severidade: Critical (ou Alert)
Ação: 
1. investigar processo que está usando CPU
2. se possível, parar processo
3. se não, rebootar roteador
4. se problema persiste, substituir hardware

### Resposta esperada para interface down

Se for um link redundante: ativo link backup
Se for o link principal: escalate imediatamente, impacto crítico
