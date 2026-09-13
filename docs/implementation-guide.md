# Guia de Implementação Assistida por IA

## Propósito

Este documento define como transformar a arquitetura documentada em implementação sem permitir que detalhes ausentes sejam inventados silenciosamente.

## Ordem recomendada

1. Definir os modelos de estado do domínio.
2. Definir contratos de comandos.
3. Definir regras de validação e processamento.
4. Definir contratos de eventos.
5. Implementar o avanço do tempo lógico.
6. Criar testes unitários do núcleo.
7. Criar o adaptador entre apresentação e comandos.
8. Criar a atualização da apresentação a partir do estado/eventos.
9. Medir desempenho.
10. Otimizar somente os pontos demonstrados pelos testes/benchmarks.

## Regras para uma IA implementadora

### 1. Não inventar domínio

Se o requisito não disser como uma entidade funciona, a IA deve perguntar ou registrar uma decisão proposta. Não deve transformar uma suposição em regra definitiva.

### 2. Respeitar a fronteira do núcleo

Código de apresentação pode solicitar comandos e consumir resultados, mas não deve assumir a autoridade sobre as regras do mundo.

### 3. Manter dependências unidirecionais

A direção conceitual deve permanecer próxima de:

```text
Input → Commands → Simulation Core → State/Events → Presentation
```

Uma dependência inversa deve ser justificada e documentada.

### 4. Testar antes de otimizar

Uma implementação inicial simples é preferível a uma arquitetura de alta complexidade sem requisito que a justifique.

### 5. Registrar decisões novas

Se a implementação exigir uma escolha que não está documentada, registrar:

- contexto;
- alternativas consideradas;
- escolha;
- justificativa;
- consequências.

## Checklist antes de aceitar uma implementação

- [ ] O núcleo pode ser testado sem renderização.
- [ ] A UI não contém regra central de domínio.
- [ ] Ações externas são representadas como comandos.
- [ ] O estado oficial está claramente identificado.
- [ ] Eventos não são tratados como fonte de verdade.
- [ ] O tempo da simulação é independente do frame rate.
- [ ] Erros e comandos inválidos têm comportamento definido.
- [ ] Decisões novas estão documentadas.
- [ ] Não foram adicionadas otimizações sem evidência.
- [ ] Os diagramas continuam compatíveis com a implementação.

## O que ainda precisa de especificação antes de uma implementação completa

A documentação arquitetural já estabelece as fronteiras principais, mas ainda não fornece um contrato de implementação completo. Antes de construir um sistema real, devem ser especificados pelo menos:

- entidades do domínio;
- ciclo de vida das entidades;
- comandos concretos;
- eventos concretos;
- invariantes do estado;
- frequência e semântica dos ticks;
- persistência;
- tratamento de falhas;
- requisitos de desempenho;
- observabilidade e logging;
- estratégia de testes de integração.
