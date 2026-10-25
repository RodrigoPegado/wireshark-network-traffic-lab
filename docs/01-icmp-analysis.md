### Objetivo

Este experimento tem como objetivo capturar e analisar pacotes do protocolo ICMP utilizando o Wireshark, a fim de compreender o funcionamento das mensagens Echo Request e Echo Reply.

A análise busca identificar os endereços IP de origem e destino, os tipos de mensagens ICMP e os campos utilizados na comunicação, além de observar possíveis comportamentos inesperados durante o experimento.

---

### Metodologia

O experimento foi realizado em uma máquina virtual com o sistema operacional Ubuntu Linux, utilizando o Wireshark para captura e análise do tráfego de rede.

Inicialmente, foi selecionada a interface de rede `enp0s3`, responsável pela comunicação da máquina virtual com a rede. Em seguida, foi aplicado o filtro de exibição `icmp` para visualizar somente os pacotes correspondentes a esse protocolo.

Para gerar o tráfego, foi executado o comando `ping -c 4 1.1.1.1`, responsável por enviar quatro mensagens ICMP Echo Request ao endereço de destino.

Após a captura, foram analisadas as mensagens Echo Request e Echo Reply, observando os endereços IP de origem e destino, os campos Type e Code, além dos identificadores e números de sequência utilizados na comunicação.

---

### Resultados

Durante o experimento, foram observados oito pacotes ICMP, sendo quatro mensagens Echo Request enviadas pela máquina virtual Ubuntu (`10.0.2.15`) ao endereço `1.1.1.1` e quatro mensagens Echo Reply recebidas como resposta.

A análise demonstrou que a comunicação ICMP entre os dois endereços foi bem-sucedida durante o teste, sem perda de respostas observada.

Entretanto, o sucesso dessa comunicação não garante que a rede esteja completamente segura, pois o experimento analisou apenas uma pequena amostra do tráfego ICMP, não sendo suficiente para identificar ou descartar possíveis ameaças e vulnerabilidades.

---

### Conclusão

Durante este experimento, aprendi como funciona a comunicação utilizando o protocolo ICMP, especialmente as mensagens Echo Request e Echo Reply, que permitem testar a comunicação entre dispositivos de rede.

Com o Wireshark, consegui observar os pacotes enviados e recebidos, analisar os endereços IP de origem e destino e compreender a função de campos como Type, Code, Identifier e Sequence Number.

Também compreendi que a análise de tráfego exige atenção, interpretação e raciocínio lógico, pois uma comunicação bem-sucedida não significa necessariamente que a rede está segura. É necessário investigar o comportamento dos pacotes e considerar outras evidências para identificar possíveis anomalias ou ameaças.