# AVA_C11

## Descrição

Este repositório apresenta a documentação arquitetural produzida para a atividade da Unidade III.

O sistema analisado representa, de forma simplificada, um jogo/simulador com um mundo persistente. A ideia central é separar o **núcleo de simulação** da **apresentação visual**.

O `Simulation Core` é responsável pelas regras e pelo estado do mundo. A Unity fica responsável por entrada, apresentação e renderização. Os dois lados possuem ciclos independentes e trocam informações por meio de comandos, estado e eventos.

## Diagramas

Os diagramas foram escritos em Markdown com Mermaid e estão organizados por finalidade.

### Arquitetura / contêineres

Mostra os principais blocos do sistema, suas responsabilidades e as relações entre a frente de simulação e a frente de apresentação.

![Diagrama de arquitetura](docs/diagrams/architecture.md)

Código-fonte: [`docs/diagrams/architecture.md`](docs/diagrams/architecture.md)

### Modelo de execução

Mostra que o ciclo de simulação e o ciclo de renderização são independentes. Um novo frame da Unity não significa automaticamente uma nova execução do `Simulation Core`.

![Modelo de execução](docs/diagrams/execution-model.md)

Código-fonte: [`docs/diagrams/execution-model.md`](docs/diagrams/execution-model.md)

### Jornada crítica

Mostra uma ação do usuário atravessando a fronteira entre apresentação e simulação, até que o resultado esteja disponível para ser representado visualmente.

![Jornada crítica](docs/diagrams/critical-journey.md)

Código-fonte: [`docs/diagrams/critical-journey.md`](docs/diagrams/critical-journey.md)

> **Observação:** os arquivos dos diagramas contêm o código Mermaid. A renderização visual pode ser visualizada pelo suporte de Mermaid do GitHub ou por um visualizador compatível.

## Decisões e ajustes realizados

Durante a elaboração, o modelo gerou algumas estruturas que precisaram ser revistas. Esses ajustes fazem parte do resultado da atividade.

### 1. Apresentação visual inicialmente colocada no fluxo do Core

Na primeira versão, a apresentação aparecia como uma etapa posterior ao `Simulation Core`, em uma leitura semelhante a:

```text
Input → Command → Simulation Core → Estado/Eventos → Presentation → Renderização
```

Essa representação não deixava clara a independência entre os ciclos e podia levar à interpretação de que o `Simulation Core` processava e depois chamava a apresentação como parte do mesmo fluxo.

O diagrama foi corrigido para representar duas frentes independentes:

```text
FRENTE DE SIMULAÇÃO              FRENTE DE APRESENTAÇÃO

Command → Simulation Core       Input → Presentation → Renderização
             │                         ▲
             ├→ World State ───────────┤
             └→ Eventos ───────────────┘
```

A apresentação consome informações produzidas pela simulação, mas não é uma etapa interna do processamento do Core.

### 2. Redundância entre os diagramas

Também foi identificado que o nível de contêineres e o diagrama de separação entre simulação e renderização estavam explicando praticamente o mesmo conceito.

A solução foi separar as responsabilidades:

- **Architecture:** quais são os principais blocos e suas responsabilidades;
- **Execution Model:** como os ciclos de simulação e renderização funcionam ao longo do tempo;
- **Critical Journey:** o que acontece quando uma ação provoca uma alteração no mundo.

Assim, cada diagrama responde a uma pergunta diferente.

### 3. Correção do conceito de gatilho da simulação

A expressão inicial de que o Core "só processa eventos" foi considerada imprecisa. Eventos também podem ser uma saída produzida pelo próprio Core.

A formulação adotada passou a ser:

> O `Simulation Core` processa quando existe um **gatilho válido da simulação**, como um comando, um evento interno agendado, um avanço do tick lógico ou outra alteração válida do mundo.

Portanto, um frame da Unity não é, por si só, um gatilho para executar novamente as regras do núcleo.

### 4. Complexidade maior que a necessária

Outro ajuste importante foi reconhecer que o sistema estava sendo descrito de forma mais complexa do que o exercício exigia.

Como a arquitetura foi inicialmente construída tendo como referência os princípios de um projeto real, o modelo trouxe espontaneamente elementos comuns de documentação de projetos de software, como **RF (requisitos funcionais)**, **RNF (requisitos não funcionais)** e uma **matriz de rastreabilidade**.

Esses elementos foram inicialmente criados, embora de forma extremamente simplificada, mas **não faziam parte do que havia sido solicitado na atividade**. Por isso, foram removidos do repositório.

O mesmo princípio foi aplicado ao próprio README. Inicialmente, ele foi tratado como se precisasse conter todos os elementos normalmente encontrados em um README clássico de projeto. Isso também aumentou desnecessariamente o escopo da documentação.

Foi necessário restringir o README ao que o exercício efetivamente pede: **a descrição, os diagramas renderizados e as decisões/ajustes realizados sobre o que o modelo gerou**.

Esse ajuste é importante porque uma arquitetura adequada não é necessariamente a mais complexa; ela deve ser proporcional ao problema e ao objetivo da atividade.

## Estrutura atual

```text
AVA_C11/
├── README.md
└── docs/
    ├── architecture-decisions.md
    └── diagrams/
        ├── architecture.md
        ├── execution-model.md
        └── critical-journey.md
```

## Fonte dos diagramas

Os diagramas são mantidos como código em Mermaid dentro dos arquivos Markdown. Isso permite que as decisões arquiteturais permaneçam versionadas junto com sua representação visual.
