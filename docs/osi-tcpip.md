# Modelo OSI e TCP/IP

## Modelo OSI

O modelo OSI divide a comunicação em sete camadas:

1. Camada Física
2. Camada de Enlace
3. Camada de Rede
4. Camada de Transporte
5. Camada de Sessão
6. Camada de Apresentação
7. Camada de Aplicação

## Modelo TCP/IP

O modelo TCP/IP é o mais usado na prática e tem quatro camadas principais:

1. Acesso à rede
2. Internet
3. Transporte
4. Aplicação

## Diferença prática

- OSI é mais didático e conceitual
- TCP/IP reflete a arquitetura usada na Internet
- Os protocolos realistas geralmente são explicados pelo modelo TCP/IP

## Encapsulamento

Uma aplicação gera dados que passam por cada camada e recebem cabeçalhos específicos antes de sair pela rede.

Exemplo:

- Aplicação: HTTP
- Transporte: TCP
- Rede: IP
- Enlace: Ethernet

A cada camada, o dado recebe informações que ajudam a entrega e o controle.

## Conceitos importantes

- MAC address
- IP address
- Portas
- Segmentos, pacotes e quadros
- Encapsulamento e desencapsulamento
- Roteamento

## Dica de estudo

Tente responder sempre:

- O que essa camada faz?
- Qual protocolo está trabalhando nela?
- Qual é o dado que ela adiciona?
- Como o pacote é entregue ao destino?
