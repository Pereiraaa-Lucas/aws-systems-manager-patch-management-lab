AWS Systems Manager: Enterprise Patch Management & Systems Hardening Lab

🚀 Visão Geral do Projeto

Este repositório documenta a execução prática e o entendimento técnico avançado obtido no laboratório de Fortalecimento de Sistemas com o Gerenciador de Patches (Patch Manager) através do AWS Systems Manager (SSM).

Em ambientes de computação em nuvem em larga escala — que englobam centenas ou milhares de instâncias híbridas —, garantir a conformidade de segurança e a aplicação automatizada de patches sem intervenção manual direta é um pilar crítico do modelo de responsabilidade compartilhada da AWS e das melhores práticas do AWS Well-Architected Framework (Specifically, the Security Pillar).

🛠️ Arquitetura e Escopo do Laboratório

O laboratório simulou um ambiente de produção heterogêneo composto por 6 instâncias Amazon EC2:

3 instâncias Linux (destinadas ao ambiente LinuxProd).

3 instâncias Windows Server 2019 (destinadas ao ambiente WindowsProd).

O objetivo central foi implementar um ciclo de vida de gerenciamento de patches automatizado, separando estratégias padrão para Linux e criando políticas personalizadas e restritivas para cargas de trabalho Windows.

📋 Passo a Passo Executado

1. Governança e Aplicação de Patches em Linux (Padrão)

Utilização de Baselines Padrão: Emprego da lista de referência pré-configurada pela AWS (AWS-AmazonLinux2DefaultPatchBaseline).

Automação via Tags: Direcionamento da operação de digitalização e instalação (Scan and Install) utilizando o seletor de tags baseada em chave-valor (Patch Group = LinuxProd).

2. Criação de Linha de Base Personalizada para Windows (Custom Baseline)

Criação da Custom Baseline: Desenvolvimento da lista de referência WindowsServerSecurityUpdates voltada para o WindowsServer2019.

Regras de Aprovação Granular:

Regra 1 (Crítica): Aprovação automática em 3 dias para atualizações de segurança (SecurityUpdates) com severidade Crítica, gerando relatórios de conformidade estritos.

Regra 2 (Importante): Aprovação automática em 3 dias para atualizações de segurança com severidade Importante, com relatórios de conformidade em nível Alto.

3. Tagueamento de Recursos e Vinculação de Grupos de Patches

Associação de Grupo: Vínculo da baseline personalizada WindowsServerSecurityUpdates ao grupo de patches WindowsProd.

Resource Tagging no EC2: Atribuição da tag corporativa Patch Group = WindowsProd em todas as instâncias do Windows (Windows-1, Windows-2, Windows-3) por meio do console do EC2 e Fleet Manager.

Execução Dirigida: Disparo do comando PatchBaselineOperations via Run Command para garantir conformidade imediata.

4. Auditoria e Validação de Conformidade

Painel de Conformidade (Patch Manager Dashboard): Monitoramento em tempo real do estado de conformidade das instâncias, atingindo com sucesso a métrica de Compliant: 6 (100% de conformidade corporativa).

Inspeção de Nós: Auditoria detalhada de histórico de instalação de pacotes e relatórios de conformidade por nó individual.

🧠 Entendimento de Alto Nível & Insights de Segurança

Gestão Centralizada em Escala (SSM Fleet Manager): O uso de agentes SSM (Amazon SSM Agent) elimina a necessidade de abertura de portas de gerência direta (como SSH/RDP expostas à internet), canalizando comandos administrativos de forma segura por meio de endpoints criptografados da API da AWS.

Separação de Ambientes via Patch Groups: A tag Patch Group atua como um seletor lógico fundamental. Ela desacopla a infraestrutura física da política de remediação, permitindo que equipes apliquem diferentes janelas de manutenção e criticidades a ambientes de desenvolvimento, homologação e produção de forma isolada.

Redução de Superfície de Ataque (Vulnerability Management): A automação rigorosa de patches de segurança previne explorações conhecidas (CVEs), mitigando riscos inerentes à lentidão ou esquecimento de atualizações manuais em parques de servidores corporativos.

📂 Estrutura do Repositório

aws-systems-manager-patch-management-lab/
├── README.md               # Documentação técnica e entendimento do projeto
├── docs/                   # Evidências e guias complementares em PDF
└── scripts/                # Referências de automação e comandos SSM
