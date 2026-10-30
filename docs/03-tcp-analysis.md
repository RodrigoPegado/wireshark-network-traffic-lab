
# Experimento 03 — Análise de Tráfego TCP

## 1. Objetivo

O objetivo deste experimento é compreender, de maneira prática, o funcionamento do protocolo TCP por meio da análise de tráfego de rede no Wireshark.

Buscamos observar o estabelecimento de uma conexão utilizando o Three-Way Handshake (SYN, SYN-ACK e ACK), analisar a transmissão e a confirmação de dados HTTP e identificar o encerramento ordenado da conexão por meio das flags FIN e ACK.

O experimento também busca desenvolver conhecimentos fundamentais para a análise de tráfego de rede e futuras investigações em cibersegurança.

## 2. Metodologia

O experimento foi realizado em uma máquina virtual Ubuntu Linux, utilizando o VirtualBox e o Wireshark para capturar e analisar o tráfego de rede na interface `enp0s3`.

Para gerar uma conexão TCP, executamos o seguinte comando no terminal:

```bash
curl -4 --http1.1 --no-keepalive http://example.com
```

Durante a captura, utilizamos filtros de exibição para identificar os segmentos TCP e o estabelecimento da conexão.

Posteriormente, aplicamos o filtro:

```wireshark
tcp.stream == 0
```

Esse filtro permitiu isolar a comunicação TCP analisada, facilitando a identificação do Three-Way Handshake, dos números de sequência e confirmação, da transmissão HTTP e do encerramento da conexão.

Após a análise, foram exportados 11 pacotes filtrados para um arquivo `.pcapng`, preservando as evidências do experimento.

**Arquivo da captura:** `captures/tcp-three-way-handshake-http.pcapng`

## 3. Resultados

### 3.1. Estabelecimento da conexão TCP

Durante a análise do tráfego TCP, identificamos que o cliente Ubuntu iniciou o estabelecimento da conexão enviando um segmento SYN ao servidor remoto.

Em seguida, o servidor respondeu com SYN-ACK, reconhecendo a solicitação e enviando seu próprio SYN.

Por fim, o cliente enviou um ACK, confirmando o recebimento do SYN do servidor e concluindo o Three-Way Handshake.

Após essas três etapas, a conexão TCP estava estabelecida e pronta para a transmissão de dados HTTP.

**Informações observadas:**

| Campo | Valor |
|---|---|
| Cliente | Ubuntu Linux |
| Servidor remoto | 104.20.23.154 |
| Porta de destino | 80 |
| Protocolo de transporte | TCP |
| TCP Stream | 0 |
| Sequência do handshake | SYN → SYN-ACK → ACK |

### 3.2. Análise dos números de sequência e confirmação (SEQ e ACK)

Durante a análise do Three-Way Handshake, foram observados os campos *Sequence Number* (SEQ) e *Acknowledgment Number* (ACK), responsáveis pelo controle da sequência dos dados transmitidos e pela confirmação de seu recebimento no protocolo TCP.

Os seguintes valores foram identificados no Wireshark:

| Segmento | SEQ | ACK |
|---|---|---|
| SYN | 0 | 0* |
| SYN-ACK | 0 | 1 |
| ACK | 1 | 1 |

*No primeiro SYN, a flag ACK não está ativada. Portanto, o valor 0 exibido nesse campo não representa uma confirmação válida.*

Os números apresentados são relativos, pois o Wireshark utiliza essa representação para facilitar a interpretação dos segmentos TCP.

No primeiro segmento, o cliente Ubuntu enviou um SYN com SEQ=0, solicitando o estabelecimento da conexão.

Em seguida, o servidor respondeu com SYN-ACK, apresentando SEQ=0 e ACK=1. Esse valor de confirmação indica que o servidor reconheceu o SYN do cliente e espera o próximo número de sequência.

Por fim, o cliente enviou ACK com SEQ=1 e ACK=1, confirmando o SYN recebido do servidor e completando o Three-Way Handshake.

Essa análise demonstra como o TCP utiliza números de sequência independentes em cada direção da comunicação e confirmações para acompanhar os dados transmitidos, contribuindo para a entrega confiável e ordenada das informações.

### 3.3. Transmissão HTTP e confirmação dos dados

Durante a comunicação, o cliente Ubuntu enviou uma requisição HTTP utilizando o método GET.

O segmento TCP responsável pelo transporte da requisição apresentou Sequence Number (SEQ) igual a 1, Acknowledgment Number (ACK) igual a 1 e comprimento de 75 bytes.

**Valores observados:**

| Campo | Valor |
|---|---|
| Requisição | GET / HTTP/1.1 |
| Sequence Number | 1 |
| Acknowledgment Number | 1 |
| TCP Segment Len | 75 bytes |
| Confirmação do servidor | ACK=76 |
| Resposta HTTP | HTTP/1.1 200 OK |

Após receber os dados, o servidor remoto enviou uma confirmação com Acknowledgment Number (ACK) igual a 76.

Esse valor indica que os bytes anteriores foram recebidos em sequência e que o próximo número de sequência esperado era 76.

O cálculo observado foi:

```text
SEQ inicial + quantidade de bytes = próximo ACK esperado

1 + 75 = 76
```

Posteriormente, foi identificada a resposta `HTTP/1.1 200 OK`, indicando que a requisição HTTP foi processada com sucesso pelo servidor.

Essa análise demonstrou como o TCP utiliza os números de sequência e confirmação para acompanhar os bytes transmitidos durante uma comunicação de rede.

### 3.4. Encerramento da conexão TCP

Após a transmissão dos dados HTTP, observamos que o cliente Ubuntu iniciou o encerramento da conexão TCP enviando um segmento com as flags FIN e ACK.

A flag FIN indica que o dispositivo terminou de enviar dados naquela direção da comunicação, enquanto a flag ACK é utilizada para confirmar o recebimento de dados ou segmentos de controle.

O servidor remoto confirmou o FIN enviado pelo cliente e, posteriormente, enviou seu próprio FIN, indicando que também havia terminado de transmitir dados.

Por fim, o Ubuntu confirmou o FIN do servidor com um ACK.

**Eventos identificados:**

| Etapa | Origem | Evento |
|---|---|---|
| 1 | Cliente Ubuntu | FIN, ACK |
| 2 | Servidor remoto | Confirmação ACK |
| 3 | Servidor remoto | FIN |
| 4 | Cliente Ubuntu | Confirmação ACK |

Esse comportamento demonstrou um encerramento ordenado da conexão TCP, no qual ambos os dispositivos finalizaram suas respectivas direções de transmissão.

## 4. Conclusão

Neste experimento, aprendemos a analisar o funcionamento de uma conexão TCP por meio do Three-Way Handshake, identificando as flags SYN, SYN-ACK e ACK e compreendendo suas funções durante o estabelecimento da comunicação.

Por meio dos pacotes capturados no Wireshark, também observamos a transmissão de dados HTTP, os números de sequência e confirmação e o encerramento ordenado da conexão utilizando as flags FIN e ACK.

Essa prática foi importante para desenvolver habilidades de análise de tráfego de rede, fundamentais para identificar problemas de comunicação e investigar possíveis atividades suspeitas, como ataques SYN Flood.

Embora o experimento não tenha simulado um ataque, os conhecimentos adquiridos servirão de base para futuras análises de segurança de redes.
