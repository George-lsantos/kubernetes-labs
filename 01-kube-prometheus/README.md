# Observabilidade com Kube-Prometheus no Amazon EKS

Este laboratório demonstra a instalação e operação da stack completa de observabilidade
(Prometheus Operator + Grafana + Alertmanager) em um cluster gerenciado Amazon EKS,
cobrindo desde a instalação via manifests até a exposição segura dos dashboards.

## 🎯 Objetivo

Implantar uma stack de monitoramento production-grade no EKS usando o projeto
`kube-prometheus`, validando descoberta de métricas via CRDs, persistência de dados
e troubleshooting de problemas reais de recurso.

## Arquitetura

![Diagrama](evidencias/diagrama-kube-prometheus.png)

## Tarefas Realizadas

- Provisionamento dos CRDs (`manifests/setup/`) e verificação via `kubectl wait --for condition=Established`
- Instalação completa da stack: Prometheus Operator, Grafana, Alertmanager, node-exporter,
  kube-state-metrics, prometheus-adapter, blackbox-exporter
- Diagnóstico e resolução de `OOMKilled` no Grafana via ajuste de `resources.limits`
- Implementação de persistência via `StorageClass` (`local-path-provisioner`) e
  `PersistentVolumeClaim` no Prometheus (CR `storage.volumeClaimTemplate`) e Grafana
- Exposição segura via Ingress NGINX + Cloudflare Tunnel, contornando CGNAT
- Ajuste de `NetworkPolicy` para permitir tráfego do Ingress Controller até os componentes

## Resultados Esperados

- Stack de observabilidade íntegra e persistente a reinicializações
- Dashboards e métricas acessíveis externamente com segurança
- Zero perda de dados em restart de Pod ou reboot de nó

## 📷 Evidências

| Componente                     | Screenshot                              |
|---------------------------------|------------------------------------------|
| Pods da stack `Running`         | ![Pods](evidencias/pods-monitoring.png)  |
| PVCs `Bound`                     | ![PVC](evidencias/pvc-bound.png)         |
| Grafana acessível externamente  | ![Grafana](evidencias/grafana-live.png)  |
| Prometheus Targets              | ![Targets](evidencias/prometheus-targets.png) |