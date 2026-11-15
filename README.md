
# Laboratório de Análise de Tráfego de Rede com Wireshark

![Status](https://img.shields.io/badge/Status-Concluido-success)
![Wireshark](https://img.shields.io/badge/Ferramenta-Wireshark-blue)
![Linux](https://img.shields.io/badge/Sistema-Ubuntu-orange)
![Categoria](https://img.shields.io/badge/Area-Ciberseguranca-blueviolet)

## Sobre o Projeto

Este repositório documenta um laboratório prático de análise de tráfego de rede utilizando o Wireshark em uma máquina virtual Ubuntu Linux.

O projeto tem como objetivo compreender o funcionamento dos principais protocolos de rede por meio da captura de pacotes, análise de comunicações e documentação técnica dos resultados.

Cada experimento apresenta os procedimentos realizados, as informações observadas e as evidências coletadas durante as atividades.

O laboratório também busca desenvolver conhecimentos fundamentais para as áreas de redes de computadores, suporte de TI e cibersegurança, especialmente para futuras atividades de monitoramento e análise de tráfego em ambientes SOC.

## Ambiente do Laboratório

| Componente | Tecnologia |
|---|---|
| Sistema operacional | Ubuntu Linux |
| Virtualização | Oracle VirtualBox |
| Analisador de pacotes | Wireshark |
| Ferramentas de terminal | ping, nslookup, curl |
| Interface de rede | enp0s3 |
| Formato das capturas | PCAPNG |
| Documentação | Markdown |
| Controle de versão | Git e GitHub |

Os experimentos foram realizados em um ambiente de estudos, utilizando tráfego de rede autorizado.

## Experimentos Realizados

### Experimento 01 — Análise de Tráfego ICMP

**Objetivo:** Compreender o funcionamento das mensagens ICMP Echo Request e Echo Reply por meio da captura e análise de pacotes.

**Conteúdos analisados:**

- ICMP Echo Request e Echo Reply
- ICMP Type 8 e Type 0
- Identifier e Sequence Number
- Inspeção de pacotes
- Testes básicos de conectividade

**Evidências:**

- [Documentação Técnica](docs/01-icmp-analysis.md)
- [Captura de Pacotes](captures/icmp-echo-request-reply.pcapng)

### Experimento 02 — Análise de Tráfego DNS

**Objetivo:** Analisar o funcionamento das consultas e respostas DNS durante o processo de resolução de nomes de domínio.

**Conteúdos analisados:**

- DNS Query e DNS Response
- Protocolo UDP e porta 53
- DNS Transaction ID
- Registros A e AAAA
- Resolução de nomes de domínio
- Inspeção de pacotes DNS

**Evidências:**

- [Documentação Técnica](docs/02-dns-analysis.md)
- [Captura de Pacotes](captures/dns-query-response.pcapng)

### Experimento 03 — Análise de Tráfego TCP

**Objetivo:** Compreender o estabelecimento de conexões TCP, a transmissão e confirmação de dados e o encerramento ordenado da comunicação.

**Conteúdos analisados:**

- TCP Three-Way Handshake
- Flags SYN, SYN-ACK e ACK
- Sequence Number (SEQ)
- Acknowledgment Number (ACK)
- Transmissão de dados HTTP
- TCP Segment Length
- Encerramento da conexão com FIN e ACK

**Evidências:**

- [Documentação Técnica](docs/03-tcp-analysis.md)
- [Captura de Pacotes](captures/tcp-three-way-handshake-http.pcapng)

### Experimento 04 — Análise de Tráfego HTTP e HTTPS

**Objetivo:** Analisar requisições e respostas HTTP e comparar uma comunicação sem criptografia com uma comunicação HTTPS protegida por TLS.

**Conteúdos analisados:**

- Requisições HTTP GET
- Códigos de status HTTP
- Cabeçalhos HTTP
- Inspeção de conteúdo HTML
- Follow TCP Stream
- TLS Client Hello e Server Hello
- TLS Application Data
- Comparação entre HTTP e HTTPS
- Análise de metadados em conexões criptografadas

**Evidências:**

- [Documentação Técnica](docs/04-http-analysis.md)
- [Captura HTTP](captures/http-request-response.pcapng)
- [Captura HTTPS](captures/https-tls-handshake.pcapng)

## Conhecimentos Desenvolvidos

Durante os experimentos, foram praticadas as seguintes habilidades:

- Captura e filtragem de tráfego utilizando Wireshark
- Identificação e interpretação de protocolos de rede
- Análise de cabeçalhos e campos dos pacotes
- Compreensão do estabelecimento de conexões TCP
- Investigação do processo de resolução DNS
- Análise de requisições e respostas HTTP
- Compreensão da utilização do TLS no HTTPS
- Identificação de metadados em conexões criptografadas
- Documentação de resultados técnicos
- Organização de evidências utilizando Git e GitHub

## Aplicação na Cibersegurança

A análise de tráfego de rede é uma habilidade importante na área de cibersegurança, especialmente em atividades realizadas por profissionais de Security Operations Center (SOC).

Compreender o comportamento normal dos protocolos permite desenvolver uma base para identificar padrões incomuns de comunicação e investigar possíveis incidentes de segurança.

Por meio deste laboratório, foi possível observar como diferentes protocolos se comportam durante comunicações legítimas.

Os experimentos realizados tiveram como foco os fundamentos de redes e a interpretação de pacotes. Não foram executados ataques ou simulações de atividades maliciosas.

## Estrutura do Repositório

    wireshark-network-traffic-lab/
    |
    |-- captures/
    |   |-- icmp-echo-request-reply.pcapng
    |   |-- dns-query-response.pcapng
    |   |-- tcp-three-way-handshake-http.pcapng
    |   |-- http-request-response.pcapng
    |   |-- https-tls-handshake.pcapng
    |
    |-- docs/
    |   |-- 01-icmp-analysis.md
    |   |-- 02-dns-analysis.md
    |   |-- 03-tcp-analysis.md
    |   |-- 04-http-analysis.md
    |
    |-- README.md

## Próximos Estudos

Com a conclusão dos quatro experimentos, os próximos estudos poderão abordar:

- Detecção de anomalias no tráfego de rede
- Investigação de conexões suspeitas
- Identificação de varreduras de portas TCP
- Análise de comportamentos suspeitos em consultas DNS
- Simulações controladas de monitoramento SOC
- Documentação de eventos de segurança

Essas atividades fazem parte do planejamento futuro e ainda não foram realizadas neste laboratório.

## Aviso

Todos os experimentos foram realizados para fins educacionais, utilizando tráfego de rede autorizado.

As capturas disponibilizadas têm como objetivo demonstrar o funcionamento dos protocolos e servir como evidências das análises técnicas realizadas.

---

**Áreas de estudo:** Redes de Computadores | Análise de Tráfego | Cibersegurança | SOC
