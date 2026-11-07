
# Experimento 04 — Análise de Tráfego HTTP e HTTPS

## 1. Objetivo

O objetivo deste experimento é compreender, de maneira prática, o funcionamento dos protocolos HTTP e HTTPS por meio da captura e análise de tráfego de rede utilizando o Wireshark.

Buscamos identificar os métodos e cabeçalhos utilizados em requisições e respostas HTTP, observar o conteúdo transmitido sem criptografia e comparar esse comportamento com uma comunicação HTTPS protegida pelo protocolo TLS.

Além disso, o experimento tem como finalidade desenvolver habilidades de análise de tráfego de rede, identificando informações que podem auxiliar profissionais de cibersegurança na investigação de conexões e atividades suspeitas.

## 2. Metodologia

O experimento foi realizado em uma máquina virtual Ubuntu Linux, executada no VirtualBox, utilizando o Wireshark para capturar e analisar o tráfego de rede na interface `enp0s3`.

Inicialmente, realizamos uma requisição HTTP utilizando o seguinte comando:

```bash
curl -4 --http1.1 -v http://example.com
```

O comando utiliza IPv4, solicita uma comunicação HTTP/1.1 e apresenta informações detalhadas sobre a requisição e a resposta.

Durante a captura, utilizamos o filtro:

```wireshark
http
```

Esse filtro permitiu identificar a requisição GET e a resposta HTTP do servidor.

Posteriormente, utilizamos a funcionalidade **Follow TCP Stream** para acompanhar a comunicação e visualizar os cabeçalhos e o conteúdo HTML transmitido.

Na segunda etapa, realizamos uma requisição HTTPS:

```bash
curl -4 --http1.1 https://example.com
```

Utilizamos os filtros `tcp.port == 443` e `tls` para investigar a comunicação criptografada, identificando mensagens de negociação TLS e os metadados disponíveis.

As capturas HTTP e HTTPS foram realizadas separadamente e exportadas para arquivos `.pcapng`, com o objetivo de preservar as evidências do experimento.

**Arquivos das capturas:**

- `captures/http-request-response.pcapng`
- `captures/https-tls-handshake.pcapng`

## 3. Resultados

### 3.1. Análise da requisição HTTP

Durante a análise da comunicação HTTP, identificamos uma requisição enviada pelo cliente Ubuntu ao domínio `example.com`.

A requisição utilizou o método GET, responsável por solicitar um recurso ao servidor.

Os seguintes campos foram identificados:

| Campo | Valor observado |
|---|---|
| Método HTTP | GET |
| Versão HTTP | HTTP/1.1 |
| Host | example.com |
| User-Agent | curl/8.23.0 |
| Porta de destino | 80 |

O campo `Host` identificou o domínio solicitado pelo cliente, enquanto o campo `User-Agent` informou o software utilizado para realizar a requisição.

Também observamos os caracteres `\r\n`, utilizados para delimitar as linhas dos cabeçalhos no formato HTTP/1.1.

Essa análise permitiu compreender como uma requisição HTTP é estruturada e quais informações são transmitidas pelo cliente ao servidor.

### 3.2. Análise da resposta HTTP

Após a requisição GET, identificamos a resposta enviada pelo servidor remoto.

A resposta apresentou o código de status `200 OK`, indicando que a requisição foi processada com sucesso.

Os seguintes campos foram observados:

| Campo | Valor observado |
|---|---|
| Status Code | 200 OK |
| Server | cloudflare |
| Content-Type | text/html; charset=utf-8 |
| Content-Length | Não encontrado |
| HTTP File Data | 577 bytes |

O cabeçalho `Server` apresentou o valor `cloudflare`, indicando a identificação informada pelo servidor que respondeu à requisição.

O campo `Content-Type` informou que o conteúdo retornado estava no formato HTML, utilizando a codificação de caracteres UTF-8.

Não identificamos o cabeçalho `Content-Length` na resposta analisada. Sua ausência não significa necessariamente um erro, pois o HTTP permite diferentes mecanismos para determinar o tamanho do corpo da mensagem.

Durante a inspeção, identificamos 577 bytes no campo `HTTP File Data`, correspondente aos dados do conteúdo HTTP apresentados pelo Wireshark.

Essa etapa permitiu compreender como o servidor responde às requisições e quais informações podem ser obtidas por meio dos cabeçalhos HTTP.

### 3.3. Inspeção do conteúdo HTML

Para analisar o conteúdo transmitido, utilizamos a funcionalidade **Follow TCP Stream** do Wireshark.

Essa ferramenta permitiu acompanhar a comunicação entre o cliente Ubuntu e o servidor remoto, reunindo os dados transmitidos no fluxo TCP selecionado.

Durante a inspeção, conseguimos visualizar os cabeçalhos HTTP e o conteúdo HTML retornado pelo servidor.

Entre os elementos identificados, encontramos o título:

`Example Domain`

A visualização do conteúdo demonstrou que, em uma comunicação HTTP tradicional, os dados são transmitidos sem a proteção criptográfica do TLS.

Consequentemente, um observador com acesso ao tráfego pode conseguir visualizar informações transmitidas pela aplicação, incluindo cabeçalhos e conteúdo das mensagens.

Essa característica representa um risco quando informações sensíveis são transmitidas por HTTP.

### 3.4. Comparação entre HTTP e HTTPS

Na segunda parte do experimento, realizamos uma comunicação HTTPS com o domínio `example.com`.

Diferentemente da comunicação HTTP, não conseguimos visualizar diretamente o conteúdo HTML transmitido pelo servidor.

Durante a captura, identificamos as seguintes mensagens TLS:

| Mensagem | Função |
|---|---|
| Client Hello | Inicia a negociação TLS e apresenta parâmetros suportados pelo cliente |
| Server Hello | Informa os parâmetros selecionados pelo servidor |
| Application Data | Transporta dados protegidos por TLS, podendo também encapsular mensagens criptografadas do handshake |
| Change Cipher Spec | Mensagem relacionada à transição criptográfica em versões anteriores do TLS ou utilizada por compatibilidade no TLS 1.3 |

A presença dessas mensagens permitiu observar etapas da comunicação TLS.

A principal diferença identificada foi a possibilidade de visualizar o conteúdo HTML na comunicação HTTP, enquanto na comunicação HTTPS esse conteúdo permaneceu protegido pela criptografia TLS.

O HTTPS utiliza TLS para fornecer confidencialidade, integridade e autenticação do servidor durante a comunicação, conforme a configuração e a validação dos certificados.

**Comparação dos resultados:**

| Característica | HTTP | HTTPS |
|---|---|---|
| Porta observada | 80 | 443 |
| Proteção TLS | Não | Sim |
| Conteúdo HTML diretamente legível | Sim | Não |
| Cabeçalhos HTTP diretamente legíveis | Sim | Não |
| IPs e portas visíveis | Sim | Sim |

Essa comparação demonstrou a importância da criptografia na proteção dos dados transmitidos entre clientes e servidores web.

### 3.5. Análise dos metadados HTTPS

Mesmo com o conteúdo da comunicação HTTPS protegido por TLS, conseguimos identificar informações relacionadas à conexão.

Os seguintes valores foram observados:

| Campo | Valor observado |
|---|---|
| IP de destino | 172.66.147.243 |
| Porta de destino | 443 |
| Protocolo de transporte | TCP |
| Protocolo de segurança | TLS |
| SNI | Não encontrado durante a análise |

O endereço IP e a porta de destino permitiram identificar o servidor remoto com o qual o cliente estabeleceu comunicação.

Durante a inspeção do Client Hello, não conseguimos localizar o campo Server Name Indication (SNI).

Essa observação não comprova que o SNI estava ausente da comunicação, pois sua visibilidade depende de fatores como a configuração TLS, o uso de ECH e a interpretação dos pacotes pelo Wireshark.

Também compreendemos que, mesmo sem acesso ao conteúdo criptografado, um analista SOC pode investigar padrões de comunicação por meio de informações como endereços IP, portas, horários, frequência e volume de tráfego.

Esses metadados podem auxiliar na identificação de comportamentos incomuns, embora não sejam suficientes, isoladamente, para comprovar uma atividade maliciosa.

## 4. Conclusão

Neste experimento, aprendemos a analisar requisições e respostas HTTP utilizando o Wireshark, identificando métodos, cabeçalhos, códigos de status e conteúdo transmitido entre cliente e servidor.

Por meio da funcionalidade Follow TCP Stream, conseguimos visualizar o conteúdo HTML retornado pelo servidor em uma comunicação HTTP, demonstrando a ausência de criptografia nesse protocolo.

Posteriormente, realizamos uma comparação com HTTPS, identificando mensagens TLS e observando que o conteúdo da aplicação não estava diretamente legível na captura.

Também verificamos que, mesmo em conexões criptografadas, informações como endereços IP, portas e padrões de comunicação continuam disponíveis para análise.

Essa prática contribuiu para o desenvolvimento de conhecimentos fundamentais em análise de tráfego de rede e cibersegurança, especialmente para futuras investigações de incidentes e atividades suspeitas em ambientes de monitoramento SOC.

O experimento não envolveu simulações de ataques, mas forneceu uma base prática para compreender a estrutura das comunicações web e a importância da criptografia na proteção das informações.
