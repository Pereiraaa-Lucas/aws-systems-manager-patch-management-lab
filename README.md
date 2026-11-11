# 🛡️ AWS Systems Manager: Patch Management & Automated Compliance Lab

<p align="center">
  <img src="https://img.shields.io/badge/AWS-Systems%20Manager-FF9900?style=for-the-badge&logo=amazonaws&logoColor=white" alt="AWS Systems Manager">
  <img src="https://img.shields.io/badge/Cloud-Security-%23232F3E?style=for-the-badge&logo=security&logoColor=white" alt="Cloud Security">
  <img src="https://img.shields.io/badge/Compliance-Automation-007396?style=for-the-badge&logo=awslambda&logoColor=white" alt="Compliance">
</p>

Este repositório documenta a implementação prática de uma estratégia centralizada de **Gerenciamento de Patches (Patch Management)** e **Governança de Conformidade** utilizando o **AWS Systems Manager (SSM)** em um ambiente híbrido simulado contendo instâncias Linux (**Amazon Linux 2**) e **Windows Server (Windows Server 2019)**.

O projeto demonstra a capacidade de **automatizar o ciclo de vida de atualizações de segurança**, aplicar listas de referência (**Patch Baselines**) personalizadas e assegurar **auditoria contínua de conformidade** em larga escala.

---

## 🏗️ Arquitetura e Cenário do Laboratório

O ambiente provisionado simula um cenário corporativo com múltiplos sistemas operacionais gerenciados de forma centralizada através do agente do SSM (**ssm-agent**), utilizando perfis de IAM com permissões de privilégio mínimo e tagueamento baseado em metadados (**Patch Group**).

* **Frota Gerenciada:** **6 instâncias Amazon EC2** (3 com Linux e 3 com Windows Server 2019).
* **Camada de Orquestração:** **AWS Systems Manager** (Fleet Manager, Patch Manager, State Manager e Run Command).
* **Segurança e Isolamento:** Controle de acesso baseado em tags e aplicação de políticas de segurança customizadas.

---

## ⚙️ Principais Componentes Implementados

### 1. Automação de Patches para Linux (Listas Padrão)
* **Baseline Utilizada:** `AWS-AmazonLinux2DefaultPatchBaseline` pré-configurada pela AWS.
* **Segmentação:** Aplicação direcionada por meio de tags de grupo (`Patch Group = LinuxProd`).
* **Ações Executadas:** Verificação (**Scan**) e instalação automatizada (**Install**) de pacotes críticos e de segurança sem intervenção manual.

### 2. Governança Avançada para Windows (Baselines Personalizadas)
Desenvolvimento de uma **Lista de Referência de Patches Customizada** (`WindowsServerSecurityUpdates`) para atender a requisitos estritos de conformidade do Windows Server 2019:
* **Regra 1 (Atualizações Críticas):** Aprovação automática em **3 dias** para pacotes de segurança classificados com severidade **Crítica**.
* **Regra 2 (Atualizações Importantes):** Aprovação automática em **3 dias** para pacotes classificados com severidade **Importante**.
* **Relatórios de Conformidade Integrados:** Classificação de criticidade configurada para auditorias automatizadas.

### 3. Tagueamento Estratégico e Grupos de Patches (`Patch Groups`)
* Associação dinâmica de instâncias do EC2 ao grupo lógico **`WindowsProd`** por meio de metadados de tags (`Patch Group = WindowsProd`).
* Desacoplamento entre a infraestrutura de servidores e as regras de patching, permitindo escalabilidade horizontal automática.

### 4. Verificação de Conformidade e Auditoria (`Compliance Reporting`)
* Utilização do painel do **Patch Manager** e do **State Manager** para monitoramento em tempo real do estado de conformidade (**Compliant vs. Non-Compliant**).
* Inspeção detalhada de nós individuais para validação de histórico de instalação e relatórios de auditoria de pacotes ausentes.

---

## 🚀 Passo a Passo da Implementação

### Passo 1: Aplicação de Patches em Linux (Padrão)
1. Acessar o **AWS Systems Manager > Gerenciador de correções** (Patch Manager).
2. Selecionar **Aplicar patch agora** (**Patch Now**).
3. Configurar os parâmetros:
   * **Operação:** `Verificar e instalar`
   * **Destino:** Instâncias especificadas por tags (`Patch Group` = `LinuxProd`)
   * **Opção de reinicialização:** `Reinicializar se necessário`

### Passo 2: Criação da Baseline Personalizada para Windows
1. No Patch Manager, navegar até a aba **Listas de referência de patches** e clicar em **Criar linha de base de patch**.
2. Definir o nome `WindowsServerSecurityUpdates` para o sistema operacional **Windows**.
3. Configurar as regras de aprovação para `WindowsServer2019` com classificação `SecurityUpdates` e severidades **Crítica** e **Importante** (janela de aprovação de 3 dias).
4. Associar a baseline criada ao grupo de patches `WindowsProd`.

### Passo 3: Tagueamento de Instâncias no Amazon EC2
1. Acessar o console do **Amazon EC2 > Instâncias**.
2. Selecionar as instâncias do Windows (`Windows-1`, `Windows-2`, `Windows-3`).
3. Gerenciar Tags e adicionar:
   * **Chave:** `Patch Group`
   * **Valor:** `WindowsProd`

### Passo 4: Execução e Validação de Conformidade
1. Disparar a operação de patching no Systems Manager direcionando para o grupo `WindowsProd`.
2. Validar o **Resumo da Conformidade** no painel central para garantir o status **`Compliant: 6`** em toda a frota.

---

## 🛠️ Tecnologias e Ferramentas Utilizadas

* **AWS EC2:** Infraestrutura de computação em nuvem (Linux & Windows).
* **AWS Systems Manager (SSM):** Gerenciamento operacional e automação de frotas.
* **IAM (Identity and Access Management):** Gestão segura de permissões e papéis de execução.
* **Cloud Infrastructure & Security Operations:** Práticas de hardening e gerenciamento de vulnerabilidades.

---

## 👨‍💻 Autor

**Lucas dos Santos Pereira**  
*Estudante de Análise e Desenvolvimento de Sistemas | Focado em Engenharia de Nuvem, Infraestrutura e Automação.*
