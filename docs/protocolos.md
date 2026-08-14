# Protocolos essenciais

Nesta seção, você vai entender os protocolos mais importantes da prática real da rede.

## Visão geral

Os protocolos de aplicação são como linguagens diferentes que os sistemas usam para conversar entre si.

Cada protocolo responde a uma pergunta:

- HTTP: como o navegador pede uma página?
- DNS: como o nome do site vira endereço IP?
- SSH: como um administrador entra de forma segura em um servidor?
- FTP: como transferir arquivos?
- SMB: como compartilhar arquivos e impressões na rede local?
- SMTP: como o e-mail sai do remetente para o destino?

---

## Tabela comparativa dos protocolos principais

| Protocolo | Função | Porta comum | Tipo de comunicação | Observação |
|---|---|---:|---|---|
| HTTP | Navegação web | 80 | Texto simples | Sem criptografia |
| HTTPS | Navegação segura | 443 | Criptografado | Usa TLS |
| DNS | Resolução de nomes | 53 | Consulta/Resposta | Traduz domínio em IP |
| SSH | Acesso remoto seguro | 22 | Terminal remoto | Criptografado |
| FTP | Transferência de arquivos | 21 | Controle + dados | Pode ser inseguro |
| SMB | Compartilhamento de arquivos | 445 | Rede local | Muito usado em Windows |
| SMTP | Envio de e-mail | 25 / 587 | Mensagem | Base do e-mail |

---

## HTTP/HTTPS

### HTTP

O HTTP é o protocolo da web. Quando você acessa uma página, seu navegador envia uma requisição e recebe uma resposta.

Fluxo básico:

1. cliente envia uma requisição
2. servidor recebe a solicitação
3. servidor responde com conteúdo
4. cliente exibe a página

### HTTPS

HTTPS é o HTTP com segurança. Ele usa TLS para criptografar a comunicação.

### Pilares do HTTP

- método: GET, POST, PUT, DELETE
- status code: 200, 404, 500
- headers: informaçõesExtras
- cookies: manter sessão

### Analogía

HTTP é como pedir um cardápio em um restaurante. HTTPS é pedir esse cardápio com a conversa protegida por um cofre de segurança.

---

## DNS

O DNS converte nomes legíveis em endereços IP.

Exemplo:

- você digita `google.com`
- o sistema consulta o DNS
- o DNS responde com o IP correspondente
- o cliente conecta ao servidor correto

### Registros importantes

- A: nome -> IPv4
- AAAA: nome -> IPv6
- MX: e-mail
- CNAME: apelido
- NS: servidor DNS
- TXT: textos e validações

### Analogía

DNS é como a agenda telefônica da Internet. Sem ele, você teria que memorizar números IP complicados.

---

## SSH

O SSH permite acesso remoto seguro a servidores e dispositivos.

Ele é usado para:

- administração remota
- transferências seguras
- execução de comandos em servidores

### Vantagens

- criptografia
- autenticação forte
- ideal para infraestrutura e automação

### Analogía

SSH é como ter uma porta segura para entrar em um servidor sem abrir uma janela para qualquer pessoa ver o que você está fazendo.

---

## FTP

O FTP foi criado para enviar arquivos entre sistemas.

Ele se divide em:

- canal de controle
- canal de dados

### Observação importante

FTP tradicional é inseguro porque o conteúdo pode ser transmitido sem criptografia. Hoje muitos ambientes usam SFTP ou FTPS.

### Analogía

FTP é como enviar um arquivo em uma mala sem proteção. SFTP é como enviar o mesmo arquivo dentro de uma mala com cadeado e blindagem.

---

## SMB

SMB é usado principalmente em redes locais para compartilhar arquivos, pastas e impressoras.

Muito comum em:

- redes Windows
- servidores de arquivos
- ambientes empresariais

### Analogía

SMB é como ter uma sala de arquivos compartilhada na empresa. Todos acessam os documentos em um mesmo espaço, de acordo com permissões.

---

## SMTP

SMTP é o protocolo usado para envio de e-mails.

Ele faz parte do fluxo de mensagem:

- cliente envia e-mail ao servidor SMTP
- servidor entrega ou encaminha para outro servidor
- destinatário recebe via POP3 ou IMAP

### Observação

SMTP não é o único protocolo do e-mail. Ele trabalha junto com POP3 e IMAP.

### Analogía

SMTP é como o correio que leva a carta até o destino. POP3 e IMAP escolhem e organizam a correspondência para o destinatário ler depois.

---

## Como estudar cada protocolo

Quando você estudar qualquer protocolo, responda estas perguntas:

- qual é a função principal?
- qual porta ele usa?
- como ocorre a troca de mensagens?
- quando ele falha?
- como identificar esse protocolo em Wireshark?

Essas perguntas fazem você aprender profundamente, e não apenas decorar nomes.

---

## Conclusão

Os protocolos de aplicação são a camada que faz a rede “falar com as pessoas”.

Sem eles, não haveria:

- páginas web
- e-mails
- acesso remoto
- compartilhamento de arquivos
- resolução de nomes

A rede deixa de ser um mistério quando você entende a função de cada protocolo e como eles se encaixam no fluxo total da comunicação.
