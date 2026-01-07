\# Wazuh SIEM/XDR Lab — Windows Endpoint (VMware Workstation Pro)



Projeto de laboratório (uso educacional) para implementar e validar um ambiente de \*\*monitoramento, detecção e resposta\*\* usando \*\*Wazuh (SIEM/XDR)\*\* para um endpoint \*\*Windows (host)\*\*, com servidor Wazuh em VM \*\*Ubuntu Server 24.04\*\*.



> \*\*Escopo:\*\* 1 endpoint Windows (host) + 1 VM Linux (Wazuh all-in-one).  

> \*\*Objetivo:\*\* evidenciar visibilidade, geração de alertas, investigação (hunting) e mapeamento \*\*MITRE ATT\&CK\*\*.



---



\## 1) Declaração de Uso Ético

Este laboratório é \*\*controlado e autorizado\*\* (ativos próprios/educacionais).  

Não realizar testes em sistemas de terceiros sem autorização formal.



---



\## 2) Arquitetura (alto nível)



\- \*\*Host:\*\* Windows 11 (endpoint monitorado com Wazuh Agent).

\- \*\*VM:\*\* Ubuntu Server 24.04 (Wazuh Server + Indexer + Dashboard, single-node).

\- \*\*Hypervisor:\*\* VMware Workstation Pro.

\- \*\*Rede recomendada (segurança + praticidade):\*\*

&nbsp; - \*\*NAT\*\* para acesso à Internet e atualizações

&nbsp; - \*\*Host-Only\*\* para tráfego isolado do laboratório (Agente ↔ Wazuh)



O VMware Workstation Pro fornece os modos \*\*NAT\*\* e \*\*Host-Only\*\* com esses propósitos (NAT compartilha identidade de rede do host; Host-Only cria rede contida no host).  

📚 Referência: “Understanding Common Networking Configurations” (VMware Workstation Pro).  

\- NAT / Host-Only / Bridged: https://techdocs.broadcom.com/.../understanding-common-networking-configurations.html \[1](https://techdocs.broadcom.com/us/en/vmware-cis/desktop-hypervisors/workstation-pro/17-0/using-vmware-workstation-pro/configuring-network-connections/understanding-common-networking-configurations.html)



---



\## 3) Snapshots (pontos de restauração)



Snapshots preservam \*\*memória\*\*, \*\*configurações\*\* e o estado dos \*\*discos\*\*, permitindo reverter a um estado conhecido.  

📚 Referência: “Using Snapshots to Preserve Virtual Machine States”.  

\- Snapshots: https://techdocs.broadcom.com/.../using-snapshots-to-preserve-virtual-machine-states.html \[2](https://techdocs.broadcom.com/us/en/vmware-cis/desktop-hypervisors/workstation-pro/17-0/using-vmware-workstation-pro/using-virtual-machines-in-workstation-pro-user-guide/taking-snapshots-of-virtual-machines/using-snapshots-to-preserve-virtual-machine-states.html)



Recomendação de snapshots:

1\. `SNAP-0\_BASE\_OS` — Ubuntu 24.04 instalado e atualizado

2\. `SNAP-1\_WAZUH\_OK` — Wazuh instalado e Dashboard acessível

3\. `SNAP-2\_AGENT\_OK` — Agente do Windows conectado e enviando eventos



---



\## 4) Requisitos e dimensionamento sugerido



\### VM Wazuh (Ubuntu Server 24.04)

\- vCPU: \*\*4\*\*

\- RAM: \*\*10 GB\*\* (mínimo recomendado para estabilidade do stack no lab)

\- Disco: \*\*100 GB\*\* (thin provision)

\- Rede: 2 NICs (NAT + Host-Only)



Por que isso?

\- O \*\*Wazuh Indexer\*\* tem recomendação mínima por nó de \*\*4 GB RAM e 2 cores\*\* (recomendado 16 GB/8 cores), e o consumo de disco depende do volume/retention.  

📚 Referência: requisitos do Indexer.  

\- https://documentation.wazuh.com/current/installation-guide/wazuh-indexer/index.html 



\### Windows Host (endpoint)

\- Instalação do \*\*Wazuh Agent\*\* no Windows 11 (host).

\- O agente é multiplaforma e o guia destaca consumo médio leve (≈35 MB RAM em média, variando por recursos).  

📚 Referência: Wazuh Agent — Installation guide.  

\- https://documentation.wazuh.com/current/installation-guide/wazuh-agent/index.html 



---



\## 5) O que este projeto demonstra



\### 5.1 Pipeline SIEM (coleta → análise → alerta)

O Wazuh coleta logs/telemetria, processa via \*\*decoders\*\* e \*\*rules\*\*, gera alertas e os envia ao Indexer para visualização no Dashboard.  

📚 Referência: Data analysis / Ruleset.  

\- https://documentation.wazuh.com/current/user-manual/ruleset/index.html 



\### 5.2 MITRE ATT\&CK

O Wazuh possui módulo no Dashboard para mapear alertas a táticas/técnicas do \*\*MITRE ATT\&CK\*\*.  

📚 Referência: Wazuh MITRE ATT\&CK module.  

\- https://documentation.wazuh.com/current/user-manual/ruleset/mitre.html 



\### 5.3 (Opcional) Active Response

O Wazuh inclui Active Response para automatizar ações a partir de alertas, com \*\*alerta de cautela\*\* sobre riscos de má implementação.  

📚 Referência: Active Response.  

\- https://documentation.wazuh.com/current/user-manual/capabilities/active-response/index.html 



---



\## 6) Conteúdo do repositório



\- `RELATORIO\_WAZUH\_SIEM\_XDR.md` → Template do relatório (executivo + técnico + MITRE)

\- `EVIDENCIAS\_CHECKLIST.md` → Checklist mínimo de prints/evidências para nota

\- `01-arquitetura/` → diagrama e descrição do lab

\- `02-deploy/` → documentação de deploy (VMware + Wazuh + Agent)

\- `03-casos-de-uso/` → casos de uso (SIEM logs, FIM, SCA, MITRE)

\- `04-evidencias/` → prints e descrição das evidências

\- `05-anexos/` → timeline e referências



---



\## 7) Como reproduzir (alto nível)



> Este guia é propositalmente em alto nível para manter foco educacional e seguro.



1\) Criar VM Ubuntu Server 24.04 no VMware Workstation Pro (recursos conforme seção 4).  

2\) Configurar rede da VM com \*\*NAT\*\* (updates) + \*\*Host-Only\*\* (lab). \[1](https://techdocs.broadcom.com/us/en/vmware-cis/desktop-hypervisors/workstation-pro/17-0/using-vmware-workstation-pro/configuring-network-connections/understanding-common-networking-configurations.html)  

3\) Instalar Wazuh single-node (server/indexer/dashboard) e validar acesso ao Dashboard.  

4\) Instalar Wazuh Agent no Windows host e registrar no server.   

5\) Validar ingestão de eventos e gerar evidências no dashboard (Security Events / Agents / MITRE).   

6\) Organizar evidências e preencher o relatório com os achados e recomendações.



---



\## 8) Licenças e créditos

\- Wazuh é plataforma open source (usar docs oficiais como referência).

\- VMware Workstation Pro: utilizar conforme termos aplicáveis ao seu uso.

