# Exercícios práticos avançados

## Objetivo

Reforçar a compreensão real de redes e protocolos com exercícios baseados em cenários reais.

---

## Exercício 1 — caminho completo de uma página

Descreva, em ordem, o que acontece desde que você digita um domínio até a página ser exibida.

### Responda:

- quem resolve o nome?
- qual protocolo inicia a conexão?
- qual camada do modelo faz a entrega?
- como o servidor responde?

---

## Exercício 2 — comparação TCP x UDP

Crie uma tabela comparando:

- confiabilidade
- velocidade
- uso em streaming
- uso em web
- uso em DNS
- uso em vídeo chamada

---

## Exercício 3 — análise de fluxo DNS

Explique o que acontece quando o sistema tenta resolver:

- `github.com`
- `mail.google.com`
- `example.com`

Liste o que acontece em ordem.

---

## Exercício 4 — análise de pacote HTTP

Imagine que você acessa um site e captura o tráfego no Wireshark.

Identifique:

- requisição
- resposta
- status code
- headers relevantes
- payload

---

## Exercício 5 — diagnóstico de falha

Você não consegue acessar um site. Responda:

- o problema pode ser DNS?
- pode ser roteamento?
- pode ser firewall?
- pode ser porta bloqueada?
- pode ser serviço indisponível?

Descreva um plano de investigação.

---

## Exercício 6 — identificar protocolo no Wireshark

Seu objetivo é reconhecer qual protocolo está sendo usado em um fluxo de rede.

Procure distinguir:

- HTTP
- HTTPS
- DNS
- TCP
- UDP
- SMTP

---

## Exercício 7 — pensamento em camadas

Explique o processo de envio de uma mensagem em etapas:

- camada de aplicação
- transporte
- rede
- enlace
- física

Descreva o que cada camada adiciona ao pacote.

---

## Exercício 8 — analogia da cidade

Descreva a rede como se ela fosse uma cidade, usando os seguintes elementos:

- rua
- mapa
- correio
- prédio
- endereço
- entrega

---

## Desafio final

Crie uma narrativa completa descrevendo:

- como um usuário acessa um site
- como o domínio é resolvido
- como a conexão é aberta
- como a requisição é entregue
- como a resposta volta
- como o Wireshark observa o processo

Se você conseguir explicar isso sem olhar o material, você já dominou o fluxo principal da rede.
