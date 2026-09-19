# ☸️ Laboratório Prático: Amazon EKS e Kubernetes

---

### Boas-vindas ao meu Portfólio de Kubernetes!

Este repositório reúne meus laboratórios práticos com Kubernetes, cobrindo dois cenários complementares: um cluster **gerenciado na nuvem** (Amazon EKS) e um cluster **self-managed** rodando no meu homelab (kubeadm), ambos usados para consolidar conceitos de orquestração de containers, redes, segurança e observabilidade — do jeito que se encontra em ambientes reais de produção.

A ideia por trás dos dois projetos é a mesma: sair da teoria e enfrentar os problemas que só aparecem na prática — limites de instância, DNS entre contas, CGNAT em rede residencial, gerenciamento de certificados, GitOps, monitoramento. Cada um dos labs documenta não só o que foi implementado, mas os desafios encontrados pelo caminho.

---

### 🚀 Visão Geral

Os projetos abordam os seguintes tópicos principais:

* **Orquestração de Containers:** Provisionamento e operação de clusters Kubernetes, tanto gerenciados (EKS) quanto self-managed (kubeadm).
* **Networking e Ingress:** Roteamento HTTP/HTTPS, Ingress NGINX Controller, integração de DNS multi-conta e superação de CGNAT via túnel.
* **Segurança e TLS:** Emissão e renovação automática de certificados com cert-manager (HTTP-01 e DNS-01).
* **Observabilidade:** Monitoramento de aplicações e infraestrutura com Grafana e Prometheus.
* **GitOps (em progresso):** Gerenciamento declarativo de deployments.

---

![Badge Kubernetes](https://img.shields.io/badge/Kubernetes-Prático-326CE5?style=for-the-badge&logo=kubernetes)
![Badge EKS](https://img.shields.io/badge/Amazon%20EKS-Gerenciado-orange?style=for-the-badge&logo=amazonaws)
![Badge Homelab](https://img.shields.io/badge/Homelab-kubeadm-blue?style=for-the-badge&logo=linux)
![Badge GitOps](https://img.shields.io/badge/GitOps-In%20Progress-623CE4?style=for-the-badge&logo=argo)

---

### 📂 Projetos e Laboratórios

| Nº | Projeto | Serviços/Ferramentas Principais | Descrição do Lab | 🔗 Acesso |
|:---|:---|:---|:---|:---:|
| **01** | Kubernetes na AWS com EKS | Amazon EKS, eksctl, Ingress NGINX, cert-manager, Route 53 | Provisionamento de um cluster Kubernetes gerenciado (EKS) via eksctl, com exposição de aplicação através de Ingress Controller, TLS automatizado via cert-manager e DNS configurado entre contas AWS distintas. | [Acessar](./lab-01-aws-eks-ingress-cert-manager) |
| **02** | Cluster Kubernetes Self-Managed (Homelab) | kubeadm, containerd, Calico, ArgoCD, Ingress NGINX, cert-manager, Cloudflare Tunnel, Prometheus, Grafana | Cluster Kubernetes "puro" construído do zero com kubeadm em ambiente homelab, expondo uma aplicação de microsserviços (Online Boutique) publicamente através de Cloudflare Tunnel, com TLS via DNS-01 e stack completa de observabilidade. | [Acessar](./lab-02-homelab-kubeadm-showcase) |

---

### ☁️ Lab 01 — Kubernetes na AWS com EKS

**Objetivo:** validar o fluxo completo de um cluster Kubernetes gerenciado na AWS, desde o provisionamento até uma aplicação acessível via HTTPS com domínio próprio.

**Arquitetura e implementação:**

O cluster EKS foi provisionado via **eksctl**, na região `us-east-1`, com nodegroup baseado em instâncias `t3.small` (ajustado a partir de `t3.micro`, após esbarrar no limite de pods por nó permitido pelo tipo de instância). Dentro do cluster, foi implantada a aplicação **giropops-senhas** (do curso Descomplicando o Kubernetes, LINUXtips) junto com um banco **Redis**, formando um cenário simples para testar roteamento e persistência.

A exposição pública foi feita através do **Ingress NGINX Controller**, instalado via Helm como serviço `LoadBalancer`, o que provisiona automaticamente um Elastic Load Balancer na AWS. O domínio `k8s.tecnolcloud.com.br` está hospedado em uma conta AWS diferente da conta onde o cluster roda — um exercício real de conectividade multi-conta, resolvido apontando um CNAME no Route 53 para o ELB do Ingress.

Por fim, o **cert-manager** (v1.21.1) foi configurado para emissão e renovação automática de certificados TLS.

**Desafios e aprendizados:**
- Custo como variável de design: o cluster é derrubado e recriado diariamente, reforçando a automação de reinstalação dos componentes (Ingress + cert-manager) a cada ciclo.
- Limites de instância impactando diretamente a quantidade de pods por nó.
- DNS entre contas AWS distintas, cenário comum em empresas com múltiplas contas.

---

### 🏠 Lab 02 — Cluster Kubernetes Self-Managed (Homelab)

**Objetivo:** construir um projeto Kubernetes completo, aplicando boas práticas de mercado, para demonstrar domínio de conceitos que vão além do "hello world" — voltado para portfólio e divulgação profissional.

**Arquitetura e implementação:**

O cluster foi construído do zero com **kubeadm** (Kubernetes "puro", em vez de k3s), rodando em uma topologia de `k8s-master` (2 vCPU/4GB) + 2 workers (2 vCPU/6GB cada) em um notebook homelab. O node master tem IP fixo reservado por MAC na rede local. A stack de container runtime usa **containerd**, com `kubeadm`/`kubelet`/`kubectl` na v1.36.4 e rede de pods via **Calico** (instalado através do Tigera Operator).

A aplicação de demonstração é o **Online Boutique** (GoogleCloudPlatform/microservices-demo), um conjunto de microsserviços que exercita bem cenários de service mesh, comunicação interna e observabilidade.

Para exposição externa, optou-se pelo **Ingress NGINX tradicional** via Helm (em vez do NGINX Gateway Fabric/Gateway API, testado mas descartado por ser menos cobrado em entrevistas de emprego na área). O TLS é emitido via **cert-manager** usando desafio **DNS-01** no Route 53 (IAM user dedicado, `ClusterIssuer letsencrypt-prod`).

O maior desafio do projeto foi expor o cluster publicamente: a internet residencial usa **CGNAT**, o que inviabiliza port-forward direto. A solução foi um **Cloudflare Tunnel** (`cloudflared` rodando no próprio cluster), que exige que o domínio tenha os nameservers na Cloudflare para publicar o registro DNS do túnel corretamente. Como o domínio principal (`tecnolcloud.com.br`) não estava com nameservers na Cloudflare, foi adquirido um domínio dedicado para o projeto (`projetoaws.online`), migrado para nameservers 100% Cloudflare.

A observabilidade é feita com **Prometheus** e **Grafana** (namespace `monitoring`), expostos via `grafana.projetoaws.online` e `prometheus.projetoaws.online`.

**Desafios e aprendizados:**
- CGNAT em rede residencial exigindo uma solução de túnel em vez de port-forward tradicional.
- Delegação de DNS via Cloudflare só funciona com a zona inteira nos nameservers da Cloudflare — o que levou à decisão de usar um domínio de teste dedicado em vez de migrar o domínio principal.
- Escolha consciente entre Gateway API e Ingress tradicional, priorizando o que é mais relevante no mercado de trabalho.
- Reconstrução do cluster do zero após conflito de IP/DHCP, reforçando a importância de IPs reservados por MAC em ambientes homelab.

---

### 📝 Próximos Passos

* **GitOps com ArgoCD:** gerenciamento declarativo dos deployments em ambos os clusters.
* **Migração do Lab 01 para Terraform:** unificar a criação do EKS com o projeto de infraestrutura modular já em andamento, usando VPC CNI customizada.
* **Segurança:** políticas de rede (NetworkPolicies), RBAC refinado e scanning de imagens.

---

### 🌐 Contato

* **Portfólio:** [www.tecnolcloud.com.br](https://www.tecnolcloud.com.br)
* **LinkedIn:** [linkedin.com/in/george--luis](https://www.linkedin.com/in/george--luis)
* **GitHub:** [github.com/George-lsantos](https://github.com/George-lsantos)
