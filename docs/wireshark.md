# Wireshark e análise de tráfego

## O que é Wireshark

O Wireshark é uma ferramenta de captura e análise de pacotes de rede. Ela permite visualizar o conteúdo real que trafega em uma interface de rede.

## O que você consegue fazer

- Ver pacotes capturados
- Filtrar eventos de rede
- Observar DNS, HTTP, TCP, UDP e outros protocolos
- Diagnosticar falhas
- Entender o comportamento de aplicações e serviços

## Filtros importantes

- ip.addr
- tcp.port
- udp.port
- http
- dns
- arp
- smtp
- ssl

## Processo básico

1. Capturar tráfego
2. Filtrar por protocolo ou conversa
3. Selecionar pacote
4. Inspecionar cabeçalhos e payload
5. Relacionar a comunicação com a camada de aplicação

## Casos práticos

- Requisição HTTP para um site
- Consulta DNS para um domínio
- Handshake TCP entre cliente e servidor
- Falha de conexão por porta bloqueada
- Análise de e-mail SMTP

## Dica importante

A leitura de pacotes exige entender a arquitetura da rede. Sem conhecer OSI, TCP/IP e protocolos, o Wireshark fica apenas um visualizador de bytes.
