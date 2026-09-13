# Decisões Arquiteturais

Este documento registra decisões necessárias para interpretar a arquitetura de forma consistente.

## ADR-001 — Separar simulação e apresentação

**Status:** Aceita

### Contexto

Regras do mundo simulado não devem ficar acopladas ao mecanismo de renderização ou à interface.

### Decisão

Manter o `Simulation Core` independente da camada `Presentation`.

### Consequências

**Positivas**
- testes do núcleo sem renderização;
- menor acoplamento;
- possibilidade de trocar ou evoluir a apresentação;
- maior clareza de responsabilidades.

**Negativas**
- exige contratos entre as camadas;
- aumenta a quantidade de conceitos arquiteturais a documentar.

---

## ADR-002 — Ações externas entram como comandos

**Status:** Aceita

### Contexto

A interface precisa solicitar alterações sem manipular diretamente o estado do mundo.

### Decisão

Representar solicitações externas como comandos processados pelo `Simulation Core`.

### Consequências

- a intenção fica explícita;
- comandos podem ser validados e testados;
- a UI deixa de ser responsável pelas regras centrais.

---

## ADR-003 — Mudanças relevantes podem ser comunicadas por eventos

**Status:** Aceita

### Contexto

A apresentação e outros consumidores precisam reagir a mudanças ocorridas na simulação.

### Decisão

Utilizar uma camada de eventos para comunicar ocorrências relevantes.

### Consequências

- consumidores ficam menos acoplados ao produtor;
- a apresentação pode reagir sem conhecer toda a implementação interna;
- eventos não devem ser tratados como fonte de verdade do estado.

---

## ADR-004 — Tempo lógico separado do frame rate

**Status:** Aceita

### Contexto

Uma simulação precisa evoluir de maneira previsível mesmo quando a taxa de renderização varia.

### Decisão

O avanço do mundo deve ser governado por um relógio/tick lógico da simulação.

### Consequências

- regras temporais ficam mais previsíveis;
- renderização e simulação podem evoluir em ritmos diferentes;
- a política exata de frequência permanece em aberto.

---

## ADR-005 — Otimização incremental

**Status:** Aceita

### Contexto

Uma arquitetura excessivamente complexa pode ser introduzida sem evidência de necessidade.

### Decisão

Começar com uma solução simples e medir desempenho antes de introduzir paralelização, estruturas especializadas ou outras otimizações de maior complexidade.

### Consequências

- menor custo inicial;
- decisões de performance orientadas por evidências;
- algumas otimizações ficam deliberadamente para etapas futuras.

---

## ADR-006 — Decisões não documentadas não devem ser inventadas

**Status:** Aceita

### Contexto

Uma futura implementação, inclusive assistida por IA, pode preencher lacunas automaticamente e transformar uma suposição em requisito.

### Decisão

Quando um detalhe não estiver definido, ele deve ser marcado como decisão em aberto ou hipótese explícita antes de ser incorporado como regra arquitetural.

### Consequências

A documentação passa a funcionar também como limite contra decisões acidentais e facilita revisão arquitetural.
