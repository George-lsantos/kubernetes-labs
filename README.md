# ☸️ Labs Práticos em Kubernetes e Observabilidade

---

### Boas-vindas ao meu Portfólio de Projetos!

Este repositório contém laboratórios práticos de Kubernetes, organizados de forma progressiva — desde a infraestrutura base do cluster até observabilidade, alertas e autoscaling avançado.

Cada laboratório demonstra conceitos importantes utilizados em ambientes reais de produção, com foco em Kubernetes "puro" (kubeadm, sem cloud managed), CNCF landscape e boas práticas de SRE/DevOps.

---

### 🚀 Visão Geral

Meus laboratórios abordam os seguintes tópicos principais:

* **Infraestrutura de Cluster:** Provisionamento bare-metal via kubeadm, CNI, exposição segura.
* **Observabilidade:** Stack completa Prometheus + Grafana + Alertmanager (kube-prometheus).
* **Descoberta Dinâmica de Métricas:** ServiceMonitors, PodMonitors e regras de alerta declarativas.
* **Autoscaling:** HPA com múltiplas métricas, tuning avançado de comportamento e testes de carga reais.

---

![Badge Kubernetes](https://img.shields.io/badge/Kubernetes-Prático-326CE5?style=for-the-badge&logo=kubernetes)
![Badge Prometheus](https://img.shields.io/badge/Prometheus-Observabilidade-E6522C?style=for-the-badge&logo=prometheus)
![Badge Grafana](https://img.shields.io/badge/Grafana-Dashboards-F46800?style=for-the-badge&logo=grafana)
![Badge CKA](https://img.shields.io/badge/CKA-Em Progresso-blue?style=for-the-badge&logo=linuxfoundation)

---

### 📂 Projetos e Laboratórios Concluídos

| Nº | Projeto | Serviços Principais | Descrição do Lab | 🔗 Acesso |
|:---|:---|:---|:---|:---:|
| **00** | Homelab Kubernetes — Infraestrutura Base | kubeadm, Calico, Ingress NGINX, cert-manager, Cloudflare Tunnel | Cluster Kubernetes bare-metal em Proxmox, com exposição pública segura contornando CGNAT via Cloudflare Tunnel. | [Acessar](https://github.com/George-lsantos/kubernetes-labs/tree/main/00-kube-homelab-k8s) |
| **01** | Observabilidade com Kube-Prometheus | Prometheus Operator, Grafana, Alertmanager, kube-state-metrics | Instalação e operação da stack completa de observabilidade, incluindo persistência via PVC e troubleshooting de recursos. | [Acessar](https://github.com/George-lsantos/kubernetes-labs/tree/main/01-kube-prometheus) |
| **02** | ServiceMonitors, PodMonitors e Alertas | ServiceMonitor, PodMonitor, PrometheusRule, Alertmanager | Descoberta declarativa de métricas via CRDs e ciclo completo de alertas, da regra até a notificação em Slack. | [Acessar](https://github.com/George-lsantos/kubernetes-labs/tree/main/02-servicemonitors-podmonitors-alertas) |
| **03** | Autoscaling com HPA, Metrics-Server e Locust | HPA (autoscaling/v2), Metrics-Server, Locust | Autoscaling baseado em CPU e memória, com teste de carga controlado via Locust e tuning de `behavior` para evitar flapping. | [Acessar](https://github.com/George-lsantos/kubernetes-labs/tree/main/03-autoscaling-hpa-metrics-server) |

---

### 📝 Labs Futuros e em Desenvolvimento

Aqui estão alguns projetos que já estão no meu radar:

* **GitOps com ArgoCD:** Deploy declarativo e sincronização automática de manifests via Git.
* **Segurança de Secrets:** Sealed Secrets / External Secrets Operator integrado a Vault ou AWS Secrets Manager.
* **Long-term Storage de Métricas:** Thanos ou VictoriaMetrics para retenção estendida do Prometheus.
* **Troubleshooting Avançado:** Cenários de SRE (CrashLoopBackOff, etcd backup/restore, kubeadm cluster management) rumo à certificação CKA.

---

### 🌐 Contato

* **Portfólio:** [www.tecnolcloud.com.br](https://www.tecnolcloud.com.br)
* **LinkedIn:** [linkedin.com/in/george--luis](https://www.linkedin.com/in/george--luis)
* **GitHub:** [github.com/George-lsantos](https://github.com/George-lsantos)