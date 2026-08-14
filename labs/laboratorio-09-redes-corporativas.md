# Laboratório 09 — Redes corporativas e VLAN

## Objetivo

Projetar e documentar uma rede corporativa segura com VLANs, roteamento dinâmico e redundância.

---

## Cenário

Você é engenheiro de rede de uma empresa com 3 filiais:

- **Filial A (São Paulo)**: 200 funcionários
- **Filial B (Rio de Janeiro)**: 100 funcionários
- **Filial C (Brasília)**: 50 funcionários

Precisa conectar as filiais mantendo:
- isolamento por departamento (vendas, TI, RH)
- redundância de link crítico
- failover automático
- performance de VoIP
- segurança entre filiais

---

## Parte 1 — Projeto de VLAN

Para cada filial, defina as VLANs necessárias:

### Filial A (São Paulo)

```
VLAN 10: Vendas
Endereço de rede: 10.0.10.0/24
Gateway: 10.0.10.1
Broadcast: 10.0.10.255
Dispositivos: 50 computadores + 10 telefones

VLAN 20: TI
Endereço de rede: 10.0.20.0/24
Dispositivos: 15 administradores + servidores

VLAN 30: RH/Financeiro
Endereço de rede: 10.0.30.0/24
Dispositivos: 20 computadores

VLAN 40: Convidados
Endereço de rede: 10.0.40.0/24
Dispositivos: acesso limitado
```

### Sua tarefa

Defina VLANs para Filiais B e C:

**Filial B:**
```
VLAN ___:  Departamento:  Rede:  Dispositivos:
```

**Filial C:**
```
VLAN ___:  Departamento:  Rede:  Dispositivos:
```

---

## Parte 2 — Configuração de trunk

Desenhe como você conectaria os switches e roteadores:

```
[Switch Filial A]
├── Porta 1-8: VLAN 10 (acesso)
├── Porta 9-16: VLAN 20 (acesso)
├── Porta 17-24: VLAN 30 (acesso)
├── Porta 25: VLAN 40 (acesso)
└── Porta 26: Trunk para roteador (todas VLANs)

[Roteador A]
├── Subinterface para VLAN 10
├── Subinterface para VLAN 20
├── Subinterface para VLAN 30
└── Subinterface para VLAN 40
```

---

## Parte 3 — Segmentação de rede

Defina quem pode falar com quem:

```
Vendas (VLAN 10) pode acessar:
- Internet (sim/não)
- Servidor de dados (sim/não)
- RH (sim/não)

TI (VLAN 20) pode acessar:
- Todas as VLANs (sim/não)
- Internet (sim/não)

Convidados (VLAN 40) podem acessar:
- Internet (sim/não)
- Internas (sim/não)
```

---

## Parte 4 — Redundância

Desenhe como você criaria um link redundante entre Filial A e Filial B:

```
[Filial A] ===== [Filial B]  (link principal)
   |                |
   +---- ISP 2 ----+        (link de backup)
```

Qual protocolo usaria para failover automático?
- [ ] HSRP
- [ ] VRRP
- [ ] GLBP

---

## Parte 5 — Roteamento dinâmico

Se usar OSPF, qual seria o custo de cada link?

```
Filial A - Filial B (1Gbps): Custo = 100.000.000 / 1.000.000.000 = 100
Filial A - Filial B (100Mbps): Custo = 100.000.000 / 100.000.000 = 1.000
Filial B - Filial C (10Mbps): Custo = 100.000.000 / 10.000.000 = 10.000
```

Qual é a rota preferida de Filial A para Filial C?
- [ ] A → B → C
- [ ] A → ISP → C

---

## Parte 6 — QoS

Configure prioridades para:

```
VoIP (porta 5060-5061):    Prioridade:  Banda garantida:
Vídeo conferência (5004):  Prioridade:  Banda garantida:
E-mail (25, 110, 143):     Prioridade:  Banda garantida:
Backup (2049):             Prioridade:  Banda garantida:
Dados gerais:              Prioridade:  Banda garantida:
```

---

## Parte 7 — Documentação

Você precisa documentar a rede para que outros engenheiros entendam. Crie:

- diagrama físico
- diagrama lógico (VLANs)
- tabela de roteamento esperada
- policy de firewall entre VLANs
- procedimento de failover

---

## Gabarito orientativo

### VLAN esperado para Filial B (100 pessoas)

```
VLAN 20: Vendas (40 pessoas)
Rede: 10.0.20.0/24

VLAN 21: TI (10 pessoas)
Rede: 10.0.21.0/24

VLAN 22: RH (30 pessoas)
Rede: 10.0.22.0/24
```

### Resposta esperada para roteamento

A → B → C é melhor porque:
- 100 + 1000 = 1100
- vs A → ISP → C que seria desconhecido/externo

### Resposta esperada para redundância

Usar VRRP ou HSRP para ambos os roteadores compartilharem um IP virtual.
