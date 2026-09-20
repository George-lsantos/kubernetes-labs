# ServiceMonitors, PodMonitors e Alertas no Kubernetes

Este laboratório demonstra o mecanismo de descoberta declarativa de métricas do
Prometheus Operator via CRDs, e o ciclo completo de alertas — da regra até a
notificação em canal real.

## 🎯 Objetivo

Demonstrar domínio sobre a descoberta dinâmica de alvos de scraping (ServiceMonitor
e PodMonitor) e o pipeline de alertas (PrometheusRule → Alertmanager → Slack),
sem depender de edição manual de configuração do Prometheus.

## Tarefas Realizadas

- Deploy de aplicação de teste expondo métricas Prometheus (`/metrics`)
- Criação de `ServiceMonitor` selecionando o Service via labels, validado em Targets
- Criação de `PodMonitor` para descoberta direta via labels de Pod, sem Service
- Criação de `PrometheusRule` com regra de alerta (`ExampleAppDown`, `severity: critical`)
- Configuração do Alertmanager (`receivers`, `route`, `inhibit_rules`) para notificação real via Slack
- Teste do ciclo completo: queda simulada da app → alerta `firing` → notificação recebida → `resolved`

## Resultados Esperados

- Target aparecendo `UP` no Prometheus após criação do ServiceMonitor/PodMonitor
- Alerta transicionando corretamente `inactive` → `pending` → `firing` → `resolved`
- Notificação real recebida no canal configurado

## 📷 Evidências

| Componente                          | Screenshot                                    |
|--------------------------------------|------------------------------------------------|
| ServiceMonitor aplicado              | ![SM](evidencias/servicemonitor-yaml.png)      |
| Target `UP` no Prometheus            | ![Targets](evidencias/target-up.png)           |
| PodMonitor aplicado                  | ![PM](evidencias/podmonitor-yaml.png)          |
| PrometheusRule — alerta `firing`     | ![Firing](evidencias/alert-firing.png)         |
| Notificação recebida no Slack        | ![Slack](evidencias/slack-notification.png)    |
| Alerta `resolved` após correção      | ![Resolved](evidencias/alert-resolved.png)     |