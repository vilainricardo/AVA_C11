# Requisitos do Sistema de Estudo

Este documento transforma a visão arquitetural em requisitos mínimos, sem inventar regras específicas de domínio.

## Requisitos funcionais

| ID | Requisito |
|---|---|
| RF-01 | O sistema deve receber uma intenção de ação do usuário. |
| RF-02 | O sistema deve representar a intenção como um comando. |
| RF-03 | O núcleo deve validar o comando conforme as regras disponíveis. |
| RF-04 | Um comando válido deve poder alterar o estado da simulação. |
| RF-05 | O sistema deve comunicar mudanças relevantes por eventos. |
| RF-06 | A apresentação deve refletir o estado relevante da simulação. |
| RF-07 | Comandos inválidos devem produzir um resultado tratável pela apresentação. |
| RF-08 | A simulação deve avançar de acordo com uma noção de tempo lógico. |

## Requisitos não funcionais

| ID | Requisito |
|---|---|
| RNF-01 | O núcleo de simulação deve possuir baixo acoplamento com a apresentação. |
| RNF-02 | O núcleo deve ser testável sem depender da renderização. |
| RNF-03 | As responsabilidades dos componentes devem ser explícitas. |
| RNF-04 | A arquitetura deve permitir evolução incremental de desempenho. |
| RNF-05 | Decisões arquiteturais devem ser documentadas para evitar suposições não rastreadas. |

## Critérios de aceitação arquitetural

### CA-01 — Separação

É possível executar testes das regras centrais sem inicializar a camada visual.

### CA-02 — Comandos

Uma ação externa chega ao núcleo como uma solicitação explícita, e não como alteração direta de estado.

### CA-03 — Estado

Existe uma fonte de verdade claramente identificada para o estado da simulação.

### CA-04 — Eventos

Uma mudança relevante pode ser comunicada à apresentação sem que a apresentação conheça a implementação interna da regra.

### CA-05 — Tempo

A evolução da simulação não depende diretamente do número de frames renderizados.

### CA-06 — Lacunas

Decisões não definidas permanecem identificadas como abertas e não são apresentadas como requisitos.
