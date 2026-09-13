# AVA_C11

Documentação arquitetural para a atividade da Unidade III.

> Este repositório é um artefato acadêmico autocontido. Ele apresenta um sistema fictício inspirado em princípios de arquitetura de software, sem expor código, assets, nomes internos ou decisões proprietárias de qualquer projeto privado.

## 1. Objetivo

O objetivo é documentar uma arquitetura de software de forma suficientemente clara para apoiar análise, discussão e futura implementação, usando diagramas como código e uma jornada crítica do sistema.

O sistema de referência representa um jogo/simulador com mundo persistente. O núcleo de simulação é responsável pelas regras e pelo estado do mundo, enquanto a Unity mantém o ciclo de apresentação e renderização.

## 2. Escopo

### Incluído

- núcleo de simulação independente da apresentação;
- entidades e estado do mundo;
- passagem de tempo por ticks lógicos;
- entrada de ações como comandos;
- saída de mudanças relevantes como eventos;
- camada Unity de apresentação/renderização;
- sincronização da apresentação com o estado mais recente;
- preocupação explícita com desempenho e evolução incremental;
- documentação arquitetural em Markdown e Mermaid.

### Fora do escopo

- implementação de gameplay;
- código-fonte de um jogo real;
- assets gráficos, áudio ou modelos 3D;
- persistência definitiva em banco de dados;
- multiplayer;
- escolha de infraestrutura de produção;
- detalhes de implementação que ainda não foram decididos.

## 3. Visão arquitetural

A arquitetura separa **dois ciclos independentes**:

- **Simulação:** processa comandos, ticks e alterações internas do mundo. É a autoridade sobre o estado.
- **Apresentação Unity:** captura entrada, consulta o estado mais recente e mantém o ciclo de renderização.

A apresentação **não é uma etapa do processamento do Simulation Core**. Ela recebe/consulta informações produzidas pela simulação e decide como representá-las visualmente.

Um novo frame da Unity não significa necessariamente que o núcleo de simulação deva executar novamente suas regras. Da mesma forma, uma mudança na simulação pode ocorrer sem que a renderização seja o mecanismo que a causou.

## 4. Responsabilidades

| Componente | Responsabilidade | Ciclo |
|---|---|---|
| Input | Capturar intenção do usuário e gerar solicitações. | Unity |
| Presentation | Consumir estado/eventos e manter a representação visual. | Unity |
| Command Layer | Representar solicitações de alteração do mundo. | Simulação |
| Simulation Core | Aplicar regras e produzir novos estados/eventos. | Simulação |
| World State | Manter a fonte de verdade do estado simulado. | Simulação |
| Event Layer | Comunicar mudanças relevantes. | Simulação |
| Renderização / UI | Desenhar a representação atual. | Unity |

## 5. Princípios arquiteturais

1. **Simulation Core independente da apresentação** — as regras da simulação não dependem de APIs visuais.
2. **Estado com fonte de verdade única** — o estado simulado pertence ao núcleo.
3. **Comandos para entrada** — ações externas chegam ao núcleo como intenções explícitas.
4. **Eventos para comunicação de mudanças** — acontecimentos relevantes podem sinalizar consumidores interessados.
5. **Tempo lógico** — a evolução do mundo usa uma noção de tempo/tick da simulação, não simplesmente o frame rate.
6. **Renderização independente** — a Unity pode renderizar usando o último estado conhecido sem executar novamente as regras do mundo em cada frame.
7. **Baixo acoplamento** — os ciclos de simulação e apresentação se comunicam por contratos claros.
8. **Evolução incremental** — otimizações complexas devem ser introduzidas quando houver evidência de necessidade.

## 6. Modelo de execução

O modelo temporal foi separado do diagrama estrutural para evitar que os dois expressem a mesma informação.

O **diagrama de arquitetura** mostra os contêineres e suas responsabilidades. O **modelo de execução** mostra como os ciclos funcionam ao longo do tempo.

Ver: [`docs/diagrams/execution-model.md`](docs/diagrams/execution-model.md).

## 7. Sincronização

Quando não existe alteração relevante no mundo, a Unity pode continuar renderizando a partir do último estado conhecido. Isso evita associar artificialmente o processamento do domínio à taxa de FPS.

Quando o `Simulation Core` produz uma mudança, o novo estado e/ou um evento relevante pode sinalizar à apresentação que existe informação nova para representar.

O mecanismo concreto de snapshot, versionamento, dirty state ou outra estratégia de sincronização ainda é uma decisão de implementação em aberto.

## 8. Decisões e restrições conhecidas

### Decisões

- A apresentação visual é separada do núcleo de simulação.
- O núcleo é tratado como a autoridade sobre o estado do mundo.
- Comandos representam solicitações de mudança.
- Eventos comunicam mudanças relevantes aos consumidores.
- A arquitetura permite testes do núcleo sem exigir renderização.
- A renderização possui ciclo próprio e não determina a frequência da simulação.
- Performance deve ser tratada de forma incremental e orientada por medição.

### Restrições

- O documento não pressupõe uma implementação específica para todos os detalhes.
- Não se deve introduzir tecnologia adicional apenas por preferência arquitetural.
- A apresentação não deve conter a regra central da simulação.
- Um frame de renderização não deve ser tratado como gatilho automático para reprocessar o mundo.

## 9. Decisões ainda em aberto

Para que uma implementação futura não precise inventar decisões importantes, ainda seria necessário definir:

- formato concreto das entidades e componentes de estado;
- catálogo de comandos e seus parâmetros;
- catálogo de eventos;
- política de frequência dos ticks;
- estratégia de sincronização entre estado e apresentação;
- estratégia de snapshot/versionamento;
- estratégia de persistência;
- tratamento de erros e comandos inválidos;
- estratégia de serialização;
- requisitos quantitativos de desempenho;
- estratégia de paralelização, caso necessária;
- contratos detalhados entre simulação e apresentação.

## 10. Diagramas

Os diagramas foram separados por finalidade, evitando repetir a mesma explicação em várias figuras.

### 10.1 Arquitetura / contêineres

Responde: **quais são os principais blocos do sistema e como eles se relacionam?**

Ver: [`docs/diagrams/architecture.md`](docs/diagrams/architecture.md).

### 10.2 Modelo de execução

Responde: **como os ciclos independentes de simulação e renderização funcionam ao longo do tempo?**

Ver: [`docs/diagrams/execution-model.md`](docs/diagrams/execution-model.md).

### 10.3 Jornada crítica

Responde: **o que acontece quando uma ação do usuário provoca uma alteração no mundo?**

Ver: [`docs/diagrams/critical-journey.md`](docs/diagrams/critical-journey.md).

## 11. O que foi inferido vs. o que foi definido

### Inferido a partir dos princípios arquiteturais

- a simulação deve ser isolável da camada visual;
- a apresentação deve possuir ciclo independente;
- comandos são uma fronteira adequada para entrada;
- eventos são uma fronteira adequada para comunicar mudanças;
- o tempo lógico deve ser tratado explicitamente;
- a Unity não deve ser a autoridade temporal do domínio;
- testes e benchmarks podem ser executados sem depender da renderização;
- otimizações devem ser introduzidas conforme evidências.

### Definido neste artefato

- nomes dos blocos apresentados nos diagramas;
- separação explícita dos ciclos de simulação e apresentação;
- jornada crítica escolhida para a atividade;
- escopo e limites documentados neste README;
- itens que permanecem deliberadamente em aberto.

Nenhuma decisão em aberto deve ser considerada automaticamente definida por este documento.

## 12. Como uma IA deve usar esta documentação

Uma IA que receba este repositório deve:

1. tratar o `Simulation Core` como autoridade do estado simulado;
2. manter simulação e apresentação como ciclos independentes;
3. representar ações externas como comandos;
4. usar eventos para comunicar mudanças relevantes;
5. evitar executar regras de domínio apenas porque ocorreu um novo frame;
6. evitar criar detalhes de domínio que não estejam documentados;
7. marcar explicitamente qualquer decisão nova necessária para implementação;
8. preferir soluções simples antes de introduzir otimizações ou infraestrutura complexa;
9. manter os diagramas atualizados quando uma decisão arquitetural for alterada.

## 13. Estrutura do repositório

```text
AVA_C11/
├── README.md
└── docs/
    ├── architecture-decisions.md
    ├── implementation-guide.md
    ├── requirements.md
    ├── traceability.md
    └── diagrams/
        ├── architecture.md
        ├── execution-model.md
        └── critical-journey.md
```

## 14. Ferramentas

- Markdown para documentação;
- Mermaid para diagramas como código;
- Git/GitHub para versionamento e colaboração.

## 15. Critério de qualidade

A documentação é considerada adequada quando outra pessoa consegue compreender:

- quais são os principais componentes;
- qual responsabilidade pertence a cada componente;
- quais são os ciclos independentes de simulação e apresentação;
- como uma ação atravessa a fronteira entre os ciclos;
- onde o estado é mantido;
- quando a simulação precisa processar uma alteração;
- como a apresentação obtém o estado mais recente;
- quais decisões já foram tomadas;
- quais decisões ainda precisam ser tomadas;
- quais pontos não devem ser inventados durante uma implementação.
