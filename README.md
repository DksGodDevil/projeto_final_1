Relatório Técnico de Análise de Segurança

Autor: Wesley da Silva Pinheiro
Data: 27/07/2025

2. Sumário Executivo

Nossa análise de segurança mostra que a rede analisada parece segura, mas na verdade tem sérias falhas. Apesar de dividida em três partes (chamadas de corp, guest e infra), essas divisões são apenas nominais. Na prática, tudo está conectado sem barreiras reais, o que facilita o acesso indevido.

O maior problema encontrado é que existe um computador com acesso liberado a todas as partes da rede. Isso significa que, se alguém invadir a rede de visitantes, pode circular livremente e acessar sistemas importantes em poucos minutos.

Este relatório explica melhor essa e outras fragilidades, como o uso de programas que transmitem dados sem segurança (como o FTP) e sistemas de gerenciamento abertos demais (como o Zabbix). Também sugerimos medidas práticas para corrigir esses problemas e tornar a rede de fato protegida.

3. Objetivo

Analisar a arquitetura da rede para identificar a topologia, a exposição de serviços, falhas na segmentação e riscos operacionais de segurança que possam permitir movimentação lateral e acesso não autorizado a ativos críticos.

4. Escopo

O escopo desta análise compreende o ambiente Docker simulado, contendo múltiplos hosts distribuídos nas sub-redes 10.10.10.0/24, 10.10.50.0/24 e 10.10.30.0/24. A análise foi conduzida a partir da máquina de pivô (608e83b46a02), que possui conectividade com todas as redes mencionadas.

5. Metodologia

A análise foi realizada através de uma abordagem de coleta ativa de dados, seguida de análise manual. As seguintes ferramentas e técnicas foram empregadas:

Descoberta de Hosts: nmap -sn para identificar hosts ativos em cada sub-rede.

Mapeamento de Portas e Serviços: rustscan e nmap para varredura de portas TCP e identificação de serviços, versões e configurações.

Análise de Conectividade: Análise das interfaces de rede (ip a) e da tabela ARP (arp -a) para mapear a topologia e a conectividade entre as redes.

Documentação: Consolidação dos dados em um inventário de ativos e relatório técnico.


6. Diagnóstico: Identificando as Falhas na Defesa

Achado 1 - RISCO CRÍTICO: Fronteiras Inexistentes e Movimentação Lateral Irrestrita

Descrição: A arquitetura atual permite que um dispositivo com acesso a uma das redes se comunique livremente com as outras duas. A máquina de análise (34b8386d208b) serviu como prova de conceito.

Impacto: Possibilita escalada de privilégio e comprometimento de ativos sensíveis.

Evidência: Arquivo recon-redes.txt.

Achado 2 - RISCO ALTO: Painel de Gerenciamento Exposto

Descrição: A interface web do Zabbix está acessível internamente sem restrição de IP.

Impacto: Risco de acesso não autorizado e execução remota de comandos.

Evidência: Arquivo infra_net/infra_net_servico_zabbix.txt.

Achado 3 - RISCO MÉDIO: Vazamento de Credenciais por FTP

Descrição: O ftp-server utiliza FTP sem criptografia.

Impacto: Interceptação de credenciais e dados.

Evidência: Arquivo infra_net/infra_net_servico_ftp-anon.txt.

7. Recomendações Estratégicas

Para o Achado 1: Implementar segmentação com firewall

Para o Achado 2: Restringir acesso e fortalecer senhas do Zabbix

Para o Achado 3: Substituir FTP por SFTP ou FTPS

8. Plano de Ação Priorizado (Matriz 80/20)

Ação Corretiva

Impacto de Risco

Facilidade de Execução

Prioridade

Implementar regras de Firewall para segmentação

Altíssimo

Média

CRÍTICA

Alterar/Validar senha padrão do Zabbix

Alto

Alta

ALTA

Substituir FTP por SFTP/FTPS

Médio

Média

ALTA

Restringir acesso direto a BD e LDAP via firewall

Médio

Média

Média

9. Conclusão

A análise conclui que a infraestrutura de rede opera sob uma falsa premissa de segurança. A falta de controles de acesso efetivos torna as sub-redes meros rótulos lógicos, não barreiras reais.

Para evoluir de uma postura vulnerável para uma resiliente, é imperativo que as recomendações deste relatório, especialmente a implementação de regras de firewall, sejam tratadas com a mais alta prioridade. A verdadeira segurança só será alcançada com aplicação rigorosa de controles entre as redes.

10. Anexos

recon-redes.txt

infra_net/infra_net_servico_zabbix.txt

infra_net/infra_net_servico_ftp-anon.txt

