# Laboratório 07 — Firewall e ACL

## Objetivo

Entender como regras de firewall funcionam e aplicar ACL em um cenário prático.

---

## Cenário

Você é o administrador de uma pequena rede corporativa. Precisa configurar um firewall para:

1. permitir SSH apenas da rede administrativa
2. permitir HTTP e HTTPS de qualquer lugar
3. permitir DNS apenas para servidores específicos
4. bloquear tudo mais

---

## Parte 1 — Análise de tráfego

Use Wireshark para capturar o tráfego e identificar:

1. Qual protocolo você vê?
2. Qual é a porta?
3. Qual é o endereço de origem?
4. Qual é o endereço de destino?
5. A regra de firewall deveria permitir ou bloquear?

---

## Parte 2 — Escrevendo regras

Escreva as regras de firewall em linguagem clara:

### Regra 1

```
Protocolo: TCP
Porta: 22
Origem: 10.0.0.0/24
Destino: qualquer
Ação: PERMITIR
Razão: SSH apenas da rede administrativa
```

### Regra 2

```
Protocolo: TCP
Porta: 80
Origem: qualquer
Destino: qualquer
Ação: PERMITIR
Razão: HTTP para todos
```

### Regra 3

```
Protocolo: TCP
Porta: 443
Origem: qualquer
Destino: qualquer
Ação: PERMITIR
Razão: HTTPS para todos
```

### Regra 4

```
Protocolo: UDP
Porta: 53
Origem: qualquer
Destino: 8.8.8.8
Ação: PERMITIR
Razão: DNS apenas para servidores de nome
```

### Regra 5 (padrão)

```
Protocolo: qualquer
Origem: qualquer
Destino: qualquer
Ação: BLOQUEAR
Razão: Tudo mais é bloqueado (deny by default)
```

---

## Parte 3 — Simulação de tráfego

Para cada cenário abaixo, responda: a regra permite ou bloqueia?

### Cenário A
- Protocolo: TCP
- Porta: 22
- Origem: 10.0.0.50 (rede administrativa)
- Destino: 192.168.0.5 (servidor)

**Sua resposta**: _______________

---

### Cenário B
- Protocolo: TCP
- Porta: 22
- Origem: 200.1.2.3 (internet)
- Destino: 192.168.0.5 (servidor)

**Sua resposta**: _______________

---

### Cenário C
- Protocolo: TCP
- Porta: 443
- Origem: qualquer
- Destino: 192.168.0.5

**Sua resposta**: _______________

---

### Cenário D
- Protocolo: UDP
- Porta: 53
- Origem: 192.168.0.2 (computador local)
- Destino: 8.8.8.8 (Google DNS)

**Sua resposta**: _______________

---

### Cenário E
- Protocolo: TCP
- Porta: 3306
- Origem: 200.1.2.3 (internet)
- Destino: 192.168.0.10 (MySQL)

**Sua resposta**: _______________

---

## Parte 4 — Considerações de NAT

Se sua rede usa NAT, responda:

1. Um computador local (192.168.0.50) acessa um site HTTPS. Como o firewall vê esse pacote?
2. Um atacante de fora tenta conectar via SSH na porta 22. O que o firewall faz?
3. Se você quer expor um servidor local via SSH de fora, o que precisa fazer com NAT?

---

## Parte 5 — Relatório

Escreva um pequeno relatório contendo:

- cenário escolhido
- regras de firewall propostas
- explicação de cada regra
- ordem das regras (qual é verificada primeiro)
- casos de bloqueio e permissão
- possíveis falhas na configuração

---

## Gabarito orientativo

### Respostas esperadas

**Cenário A**: PERMITIR (origem está na rede permitida)

**Cenário B**: BLOQUEAR (origem não está na rede permitida)

**Cenário C**: PERMITIR (HTTPS de qualquer lugar é permitido)

**Cenário D**: PERMITIR (DNS para Google é permitido)

**Cenário E**: BLOQUEAR (MySQL não está nas regras, cai no padrão BLOQUEAR)

### Considerações de NAT

1. O roteador traduz 192.168.0.50 para um IP público, mas a regra de saída geralmente permite tudo
2. O firewall bloqueia (SSH não é permitido da internet)
3. Usar Static NAT ou PAT com port forwarding para mapear porta externa para o servidor interno
