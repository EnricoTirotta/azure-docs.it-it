---
title: Esempio di rimozione di script dell'interfaccia della riga di comando di Azure Service Fabric
description: Rimuovere un'applicazione da un cluster di Azure Service Fabric usando l'interfaccia della riga di comando di Azure Service Fabric
services: service-fabric
documentationcenter: 
author: thraka
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: service-fabric
ms.workload: multiple
ms.devlang: na
ms.topic: sample
ms.date: 12/06/2017
ms.author: adegeo
ms.custom: mvc
ms.openlocfilehash: 95a6bac7ee79e3490584305e78d410104363b780
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/08/2017
---
# <a name="remove-an-application-from-a-service-fabric-cluster"></a>Rimuovere un'applicazione da un cluster di Service Fabric

Questo script di esempio elimina un'istanza di applicazione di Service Fabric in esecuzione, annulla la registrazione di un tipo di applicazione e della versione dal cluster.  L'eliminazione dell'istanza dell'applicazione elimina anche tutte le istanze del servizio associate a questa applicazione. I file dell'applicazione vengono quindi eliminati dall'archivio immagini. 

Se necessario, installare l'[interfaccia della riga di comando di Service Fabric](../service-fabric-cli.md).

## <a name="sample-script"></a>Script di esempio

[!code-sh[main](../../../cli_scripts/service-fabric/remove-application/remove-application.sh "Remove an application from a cluster")]

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni, vedere la [documentazione sull'interfaccia della riga di comando di Service Fabric](../service-fabric-cli.md).

Altri esempi dell'interfaccia della riga di comando di Service Fabric per Azure Service Fabric sono disponibili negli [esempi dell'interfaccia della riga di comando di Service Fabric](../samples-cli.md).
