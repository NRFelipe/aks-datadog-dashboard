# Projeto: Monitoramento com Azure AKS + Datadog

## ✨ Visão Geral
Este projeto demonstra a criação de um cluster Kubernetes no Azure (AKS) com integração de monitoramento via Datadog. Utilizamos Helm para instalar o agente do Datadog e observamos métricas em tempo real da infraestrutura.

## ⚙️ Tecnologias Utilizadas
- **Azure Kubernetes Service (AKS)**: Orquestração de containers
- **Datadog**: Plataforma de observabilidade (infra, métricas, logs)
- **Helm**: Gerenciador de pacotes para Kubernetes
- **kubectl + Azure CLI**: Para gerenciamento do cluster

## 🔄 Passo a Passo

### 1. Criar Cluster AKS (Azure CLI)
az aks create \
  --resource-group aks-free-rg \
  --name aks-free-cluster \
  --node-count 1 \
  --enable-addons monitoring \
  --generate-ssh-keys \
  --enable-managed-identity \
  --tier free \
  --location eastus

### 2. Conectar-se ao cluster
az aks get-credentials --resource-group aks-free-rg --name aks-free-cluster

### 3. Instalar o repositório Helm da Datadog
helm repo add datadog https://helm.datadoghq.com
helm repo update

### 4. Criar Secret com a API Key do Datadog
kubectl create secret generic datadog-secret \
  --from-literal=api-key=SEU_API_KEY \
  -n default

### 5. Instalar o agente Datadog com Helm
helm install datadog-agent -f datadog-values.yaml datadog/datadog

> ✉️ O agente foi instalado no namespace `default`. Caso deseje usar `datadog`, é necessário desinstalar primeiro a versão existente:

helm uninstall datadog-agent -n default
kubectl create namespace datadog
helm install datadog-agent -n datadog -f datadog-values.yaml datadog/datadog

## 🔢 Métricas Observadas
- Uso de CPU (%)
- Uso de Memória (breakdown e por processo)
- Uso de Disco e Latência
- Tráfego de Rede
- Cargas médias (load averages)

## 📅 Resultado
Após a implantação, os agentes Datadog foram iniciados em cada nó do cluster. As métricas apareceram no painel de infraestrutura do Datadog em poucos minutos, permitindo visão clara do desempenho do cluster AKS.

## 🔗 Próximos passos
- Configurar alertas com base nas métricas
- Implementar escalonamento automático com KEDA
- Integrar com GitHub Actions para CI/CD automatizado

---

**Autor:** Felipe Rodriguez  
**Data:** Julho 2025
