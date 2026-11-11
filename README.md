# 🛡️ AWS Systems Manager Patch Management Lab

<p align="center">
  <img src="https://img.shields.io/badge/AWS-Systems%20Manager-orange?style=for-the-badge&logo=amazonaws&logoColor=white" alt="AWS Systems Manager">
  <img src="https://img.shields.io/badge/Compliance-Automated-brightgreen?style=for-the-badge&logo=security&logoColor=white" alt="Compliance">
  <img src="https://img.shields.io/badge/OS-Linux%20%2F%20Windows-blue?style=for-the-badge&logo=linux&logoColor=white" alt="Multi-OS">
  <img src="https://img.shields.io/badge/Status-Completed-success?style=for-the-badge" alt="Status Completed">
</p>

## 📋 Visão Geral do Projeto

Em ambientes corporativos de nuvem com dezenas, centenas ou milhares de instâncias EC2, a gestão manual de atualizações e conformidade de segurança torna-se inviável. Este laboratório demonstra a implementação prática de uma estratégia unificada de **Patch Management** utilizando o **AWS Systems Manager (SSM)** em um ambiente híbrido contendo instâncias **Linux (Amazon Linux 2)** e **Windows Server 2019**.

O objetivo central é garantir a postura de segurança contínua por meio de baselines de patches padrões e customizadas, grupos lógicos baseados em tags e relatórios automatizados de conformidade.

---

## 🎯 Objetivos de Aprendizagem

* **Automação Linux:** Aplicação de patches em instâncias Linux utilizando listas de referência (*Patch Baselines*) nativas da AWS.
* **Customização de Baselines:** Criação de listas de referência de patches personalizadas para atender a requisitos rigorosos de segurança em servidores Windows.
* **Segmentação por Grupos de Patches:** Associação de baselines customizadas a frotas específicas utilizando *Patch Groups* e tags do Amazon EC2.
* **Auditoria e Conformidade:** Monitoramento em tempo real do estado de conformidade (*Compliance Reporting*) de toda a infraestrutura gerenciada.

---

## 🏗️ Arquitetura do Ambiente

O laboratório simulou um ambiente multi-sistema operacional composto por:
* **6 instâncias Amazon EC2** gerenciadas via SSM Agent (3 instâncias Linux e 3 instâncias Windows Server 2019).
* **Perfis do IAM (Instance Profiles)** pré-configurados para comunicação segura com o AWS Systems Manager.
* **AWS Systems Manager (Fleet Manager, Patch Manager, Run Command e State Manager)** como plano de controle centralizado.

---

## 🛠️ Passo a Passo da Implementação

### Tarefa 1: Aplicar Patches em Instâncias Linux (Baseline Padrão)
O objetivo desta etapa foi utilizar as baselines pré-definidas da AWS para aplicar atualizações críticas em servidores Linux de produção.

1. Acesso ao console do **AWS Systems Manager** e validação dos nós no **Fleet Manager**.
2. Configuração do **Patch Manager** utilizando a baseline padrão `AWS-AmazonLinux2DefaultPatchBaseline`.
3. Definição do escopo de alvo baseada em tags (`Patch Group: LinuxProd`).
4. Execução da operação de varredura e instalação (*Scan and Install*), monitorando o progresso via `AWS-PatchNowAssociation`.

### Tarefa 2: Criar Baseline Personalizada para Windows Server
Para ambientes Windows, criou-se uma política de atualização customizada com regras granulares de severidade e aprovação automática.

1. Criação da baseline `WindowsServerSecurityUpdates` voltada para o SO **Windows**.
2. Configuração de regras de aprovação automática:
   * **Regra 1:** Produtos `WindowsServer2019`, Severidade **Crítica**, Classificação `SecurityUpdates`, Aprovação em `3 dias` (Relatório de Conformidade: *Crítica*).
   * **Regra 2:** Produtos `WindowsServer2019`, Severidade **Importante**, Classificação `SecurityUpdates`, Aprovação em `3 dias` (Relatório de Conformidade: *Alta*).
3. Vinculação da nova baseline ao grupo de patches `WindowsProd`.

### Tarefa 3: Tagueamento e Aplicação de Patches no Windows
Utilização do Amazon EC2 e do Systems Manager para direcionar a nova política aos servidores Windows.

1. Tagueamento das instâncias do EC2 (`Windows-1`, `Windows-2` e `Windows-3`) com a Chave `Patch Group` e o Valor `WindowsProd`.
2. Execução da ferramenta **Aplicar patch agora** no Systems Manager mapeando o grupo `WindowsProd`.
3. Inspeção dos bastidores da execução utilizando o **Run Command** e o documento SSM `PatchBaselineOperations` para rastrear o sucesso das operações.

### Tarefa 4: Verificação de Conformidade e Auditoria
Auditoria final do estado de saúde e segurança de toda a frota computacional.

1. Navegação até a aba **Painel** do Gerenciador de patches para confirmar o resumo de conformidade: `Compliant: 6`.
2. Inspeção detalhada na aba **Relatórios de conformidade**, validando métricas individuais por nó.
3. Checagem individual dos patches aplicados e horários de instalação na aba **Patch** de nós específicos do Windows.

---

## 🚀 Conclusão e Resultados

Este laboratório validou com sucesso a capacidade de:
* Padronizar a aplicação de atualizações de segurança em ambientes heterogêneos.
* Reduzir o esforço operacional através de automação orientada por tags.
* Manter visibilidade e governança total sobre a conformidade de sistemas operacionais na nuvem AWS.

---
<p align="center">Desenvolvido com 💙 por Lucas dos Santos Pereira</p>
