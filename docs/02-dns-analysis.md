# Experimento 02 — Análise de Tráfego DNS

## 1. Objetivo

O objetivo deste experimento é analisar o funcionamento do protocolo DNS (Domain Name System), utilizando o comando `nslookup` para realizar uma consulta e identificar o endereço IPv4 associado ao domínio `example.com`.

Por meio do Wireshark, buscamos observar os pacotes de consulta (Query) e resposta (Response), investigando os registros DNS, o Transaction ID e as portas de origem e destino utilizadas na comunicação.

---

## 2. Metodologia

O experimento foi realizado em uma máquina virtual com o sistema operacional Ubuntu Linux, utilizando o VirtualBox e a ferramenta Wireshark para captura e análise do tráfego de rede.

Inicialmente, foi selecionada a interface de rede `enp0s3` no Wireshark. Em seguida, foi executado o comando:

```bash
nslookup example.com 1.1.1.1
```

O comando realizou uma consulta ao servidor DNS `1.1.1.1`, solicitando informações sobre o domínio `example.com`.

Para identificar os pacotes DNS, utilizamos o filtro de exibição:

```text
dns
```

Durante a análise, investigamos o Transaction ID, os registros DNS, os endereços IP e as portas UDP de origem e destino.

Para isolar os pacotes relacionados ao domínio consultado, utilizamos o filtro:

```text
dns && dns.qry.name == "example.com"
```

Por fim, os pacotes exibidos foram exportados pelo Wireshark utilizando a opção **File → Export Specified Packets → Displayed**, gerando o arquivo `dns-query-response.pcapng`.

---

## 3. Resultados

Durante a captura, foram identificadas mensagens DNS de consulta (Query) e resposta (Response) relacionadas ao domínio `example.com`.

O comando `nslookup` retornou o endereço IPv4 `172.66.147.243`, associado ao domínio consultado.

Na análise de uma das transações DNS, identificamos o Transaction ID `0x7c1a`, presente na consulta e na resposta correspondente. Esse identificador auxilia na associação entre as mensagens.

Também observamos uma resposta DNS com código `No error (0)` e dois registros na seção Answers, incluindo registros do tipo A.

---

### Análise das portas UDP

A comunicação analisada utilizou o protocolo UDP, apresentando os seguintes valores:

| Campo | Query | Response |
|---|---|---|
| Porta de origem | 49170 | 53 |
| Porta de destino | 53 | 49170 |
| Transaction ID | 0x7c1a | 0x7c1a |
| Protocolo de transporte | UDP | UDP |

Na consulta, a máquina virtual utilizou a porta de origem `49170` para enviar uma solicitação à porta `53` do servidor DNS.

Na resposta, o servidor utilizou a porta de origem `53` e enviou os dados para a porta `49170` da máquina virtual.

Essa análise permitiu compreender como as portas UDP são utilizadas para encaminhar as mensagens entre cliente e servidor.

---

### Relevância para a segurança

A análise do tráfego DNS tradicional permite observar informações como nomes de domínio consultados, endereços IP e servidores DNS utilizados.

Essas informações podem auxiliar profissionais de cibersegurança na identificação de padrões incomuns de comunicação e na investigação de possíveis atividades suspeitas.

Entretanto, uma consulta DNS isolada não é suficiente para confirmar a existência de uma ameaça.

---

## 4. Conclusão

Neste experimento, foi possível compreender como funciona uma consulta DNS e como um nome de domínio pode ser resolvido para um endereço IP.

Por meio do Wireshark, analisamos a comunicação entre cliente e servidor, identificando portas de origem e destino, Transaction ID e registros DNS.

Também compreendemos a importância do protocolo UDP nesse tipo de comunicação e como as respostas são encaminhadas de volta à aplicação cliente.

Esses conhecimentos são importantes para redes e cibersegurança, pois auxiliam na interpretação do tráfego de rede e na investigação de possíveis comunicações suspeitas.