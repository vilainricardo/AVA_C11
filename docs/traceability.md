# Matriz de Rastreabilidade

A matriz conecta requisitos, decisões e elementos dos diagramas.

| Requisito | Decisão relacionada | Elemento documentado |
|---|---|---|
| RF-01 | ADR-002 | Input |
| RF-02 | ADR-002 | Command Layer |
| RF-03 | ADR-001 / ADR-002 | Simulation Core |
| RF-04 | ADR-001 | World State |
| RF-05 | ADR-003 | Event Layer |
| RF-06 | ADR-001 | Presentation |
| RF-07 | ADR-003 | Jornada crítica — ramo de comando inválido |
| RF-08 | ADR-004 | Tempo lógico / Simulation Core |
| RNF-01 | ADR-001 | Fronteira Simulation Core / Presentation |
| RNF-02 | ADR-001 | Critério CA-01 |
| RNF-03 | ADR-001 | Tabela de responsabilidades |
| RNF-04 | ADR-005 | Guia de implementação |
| RNF-05 | ADR-006 | Decisões arquiteturais |

## Leitura da matriz

A matriz existe para evitar que a documentação seja apenas descritiva. Cada requisito relevante deve ter uma decisão, componente ou fluxo que permita verificar sua intenção.

Quando um requisito novo não possuir correspondente arquitetural, isso deve ser tratado como uma lacuna de documentação ou como uma nova decisão a ser registrada.
