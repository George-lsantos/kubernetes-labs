# Autoscaling no Kubernetes com HPA, Metrics-Server e Locust

Este laboratório demonstra o funcionamento do Horizontal Pod Autoscaler (HPA)
usando métricas de CPU e memória, com geração de carga controlada via Locust
para validar o comportamento de scale-up/scale-down em tempo real.

## 🎯 Objetivo

Configurar autoscaling automático baseado em múltiplas métricas de recurso,
validar o comportamento sob carga real e mensurável (não sintética), e
demonstrar controle fino de velocidade de escala via `behavior`, evitando
oscilação (flapping).

## Arquitetura

![Diagrama](evidencias/diagrama-hpa-locust.png)

## Tarefas Realizadas

- Configuração de `resources.requests/limits` como pré-requisito de cálculo do HPA
- Criação de HPA (`autoscaling/v2`) com métricas combinadas de CPU e memória
- Deploy do **Locust** como gerador de carga controlado, com interface web
  para configurar número de usuários simulados e taxa de spawn
- Execução de teste de carga progressivo (rampa de usuários) e observação
  do HPA reagindo em tempo real
- Configuração de `behavior.scaleUp`/`scaleDown` com `stabilizationWindowSeconds`,
  `policies` (`Pods` e `Percent`) e `selectPolicy` (`Max`/`Min`)
- Validação da assimetria intencional: scale-up imediato sob carga,
  scale-down conservador após 5 minutos de estabilidade

## Resultados Esperados

- HPA escalando corretamente com base em CPU e memória combinadas
- Correlação visível entre o aumento de RPS no Locust e o aumento de réplicas
- Scale-down gradual e controlado após o fim da carga, sem flapping

## 📷 Evidências

| Componente                                | Screenshot                                     |
|---------------------------------------------|--------------------------------------------------|
| HPA com métricas CPU + memória (`describe`)  | ![HPA](evidencias/hpa-describe.png)             |
| Dashboard do Locust — carga em andamento     | ![Locust](evidencias/locust-dashboard.png)      |
| RPS e usuários simulados subindo             | ![Locust Chart](evidencias/locust-charts.png)   |
| `kubectl get hpa -w` — réplicas escalando    | ![Scaling](evidencias/hpa-scaling-live.png)     |
| `behavior` configurado (YAML)                | ![Behavior](evidencias/hpa-behavior.png)        |
| Scale-down gradual após fim do teste         | ![ScaleDown](evidencias/scale-down.png)         |