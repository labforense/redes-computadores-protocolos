# Laboratório 06 — Wireshark e troubleshooting

## Objetivo

Aplicar a leitura de pacotes e a lógica de diagnóstico em cenários reais de rede.

## Cenário

Você está analisando o acesso a um site e percebe que ele demora muito para abrir ou não responde. Seu objetivo é verificar se o problema está em:

- DNS
- TCP
- roteamento
- firewall
- servidor web

---

## Parte 1 — Captura

1. Abra o Wireshark.
2. Inicie a captura em sua interface ativa.
3. Acesse um site conhecido, como exemplo.com ou uma página local.
4. Pare a captura quando o fluxo terminar.

---

## Parte 2 — Filtros

Use os filtros abaixo para separar o tráfego:

- `dns`
- `http`
- `tcp`
- `ip.addr == 8.8.8.8`
- `tcp.flags.syn == 1`
- `http.response.code == 200`

---

## Parte 3 — Análise

Identifique os seguintes elementos:

1. Qual pacote representa a consulta DNS?
2. Houve uma resolução correta do domínio?
3. O handshake TCP foi concluído?
4. O cliente enviou uma requisição HTTP?
5. O servidor respondeu com código HTTP 200?
6. Houve retransmissão ou timeout?
7. O problema parece estar no DNS, no transporte ou na aplicação?

---

## Parte 4 — Diagnóstico

### Perguntas orientadoras

- Se não houve resposta DNS, o problema é de quê?
- Se o SYN foi enviado e não houve SYN-ACK, o problema está onde?
- Se o TCP conectou, mas a resposta foi 500, onde está a falha?
- Se a página ficou carregando para sempre, o que pode estar acontecendo?

---

## Parte 5 — Relatório final

Escreva um pequeno relatório contendo:

- cenário observado
- filtros usados
- pacotes importantes
- conclusão técnica
- hipótese de causa

---

## Critérios de avaliação

Seu laboratório será bem avaliado se você conseguir:

- identificar corretamente a sequência do fluxo
- interpretar DNS, TCP e HTTP
- diferenciar falha de roteamento, DNS e aplicação
- formular uma conclusão coerente com os dados capturados

---

## Gabarito orientativo

### Possíveis evidências

- `dns` → consulta de resolução de nomes
- `tcp.flags.syn == 1` → tentativa de conexão
- `tcp.flags.syn == 1 and tcp.flags.ack == 1` → resposta do servidor
- `http.request.method == "GET"` → requisição da página
- `http.response.code == 200` → resposta bem-sucedida

### Interpretação correta

Se a sequência for DNS → TCP → HTTP → 200, então a rede e o serviço estão funcionando. Se houver uma interrupção antes de HTTP, a falha é de infraestrutura ou transporte. Se houver HTTP mas resposta 500, o problema está na aplicação ou no backend.
