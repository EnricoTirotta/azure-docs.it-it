---
title: 'Esercitazione su Kubernetes in Azure: distribuire un cluster'
description: Esercitazione sul servizio contenitore di Azure - Distribuire un cluster
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: tutorial
ms.date: 11/15/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: e0d5bd57a40fca837ead42e691e1fa0c802dc013
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 02/09/2018
---
# <a name="deploy-an-azure-container-service-aks-cluster"></a>Distribuire un cluster del servizio contenitore di Azure

Kubernetes fornisce una piattaforma distribuita per applicazioni in contenitori. Con il servizio contenitore di Azure, il provisioning di un cluster Kubernetes pronto per la produzione è semplice e rapido. In questa esercitazione, la terza parte di otto, viene distribuito un cluster di Kubernetes nel servizio contenitore di Azure. I passaggi completati comprendono:

> [!div class="checklist"]
> * Distribuzione di un cluster del servizio contenitore di Azure Kubernetes
> * Installazione dell'interfaccia della riga di comando Kubernetes (kubectl)
> * Configurazione di kubectl

Nelle esercitazioni successive, l'applicazione Azure Vote viene distribuita nel cluster, ridimensionata, aggiornata e Operations Management Suite viene configurato per monitorare il cluster Kubernetes.

## <a name="before-you-begin"></a>Prima di iniziare

Nelle esercitazioni precedenti, un'immagine del contenitore è stata creata e caricata in un'istanza di Registro contenitori di Azure. Se questi passaggi non sono stati ancora eseguiti e si vuole procedere, tornare a [Tutorial 1 – Create container images][aks-tutorial-prepare-app] (Esercitazione 1: Creare immagini del contenitore).

## <a name="enabling-aks-preview-for-your-azure-subscription"></a>Abilitazione dell'anteprima AKS per la sottoscrizione di Azure
Mentre AKS è disponibile in anteprima, per creare nuovi cluster è necessario un flag funzionalità per la sottoscrizione. È possibile richiedere questa funzionalità per tutte le sottoscrizioni da usare. Usare il comando `az provider register` per registrare il provider AKS:

```azurecli-interactive
az provider register -n Microsoft.ContainerService
```

Dopo la registrazione è possibile Creare un cluster Kubernetes con AKS.

## <a name="create-kubernetes-cluster"></a>Creare un cluster Kubernetes

Nell'esempio seguente viene creato un cluster denominato `myAKSCluster` nel gruppo di risorse denominato `myResourceGroup`. Nell'[esercitazione precedente][aks-tutorial-prepare-acr] è stato creato questo gruppo di risorse.

```azurecli
az aks create --resource-group myResourceGroup --name myAKSCluster --node-count 1 --generate-ssh-keys
```

Dopo alcuni minuti, la distribuzione viene completata e restituisce le informazioni in formato JSON sulla distribuzione del servizio contenitore di Azure.

## <a name="install-the-kubectl-cli"></a>Installare l'interfaccia della riga di comando di kubectl

Per connettersi al cluster Kubernetes dal computer client, usare [kubectl][kubectl], il client da riga di comando di Kubernetes.

Se si usa Azure CloudShell, kubectl è già installato. Per installarlo in locale, eseguire il comando seguente:

```azurecli
az aks install-cli
```

## <a name="connect-with-kubectl"></a>Connettersi con kubectl

Per configurare kubectl per la connessione al cluster Kubernetes, eseguire il comando seguente:

```azurecli
az aks get-credentials --resource-group=myResourceGroup --name=myAKSCluster
```

Per verificare la connessione al cluster, eseguire il comando [kubectl get nodes][kubectl-get].

```azurecli
kubectl get nodes
```

Output:

```
NAME                          STATUS    AGE       VERSION
k8s-myAKSCluster-36346190-0   Ready     49m       v1.7.7
```

Al termine dell'esercitazione, sarà disponibile un cluster del servizio contenitore di Azure pronto per i carichi di lavoro. Nelle esercitazioni successive, in questo cluster viene distribuita un'applicazione multi-contenitore, quindi viene scalata orizzontalmente, aggiornata e monitorata.

## <a name="next-steps"></a>Passaggi successivi

In questa esercitazione è stato distribuito un cluster Kubernetes nel servizio contenitore di Azure. Sono stati completati i passaggi seguenti:

> [!div class="checklist"]
> * Distribuzione di un cluster Kubernetes nel servizio contenitore di Azure
> * Installazione dell'interfaccia della riga di comando di Kubernetes (kubectl)
> * Configurazione di kubectl

Passare all'esercitazione successiva per apprendere come eseguire l'applicazione nel cluster.

> [!div class="nextstepaction"]
> [Distribuire un'applicazione in Kubernetes][aks-tutorial-deploy-app]

<!-- LINKS - external -->
[kubectl]: https://kubernetes.io/docs/user-guide/kubectl/
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get

<!-- LINKS - internal -->
[aks-tutorial-deploy-app]: ./tutorial-kubernetes-deploy-application.md
[aks-tutorial-prepare-acr]: ./tutorial-kubernetes-prepare-acr.md
[aks-tutorial-prepare-app]: ./tutorial-kubernetes-prepare-app.md