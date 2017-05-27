---
title: Esempio di script dell&quot;interfaccia della riga di comando di Azure - Creare una VM da uno snapshot | Microsoft Docs
description: Esempio di script dell&quot;interfaccia della riga di comando di Azure - Creare una VM da uno snapshot
services: virtual-machines-linux
documentationcenter: virtual-machines
author: ramankum
manager: kavithag
editor: ramankum
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/10/2017
ms.author: ramankum
ms.translationtype: Human Translation
ms.sourcegitcommit: e7da3c6d4cfad588e8cc6850143112989ff3e481
ms.openlocfilehash: b5e33d7dcee5b5241de796d632a9b6069b991f7b
ms.contentlocale: it-it
ms.lasthandoff: 05/16/2017

---

# <a name="create-a-virtual-machine-from-a-snapshot-with-cli"></a>Creare una macchina virtuale da uno snapshot con l'interfaccia della riga di comando

Questo script crea una macchina virtuale da uno snapshot di un disco del sistema operativo.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Script di esempio

[!code-azurecli[main](../../../cli_scripts/virtual-machine/create-vm-from-snapshot/create-vm-from-snapshot.sh "Creare una VM da uno snapshot")]

## <a name="clean-up-deployment"></a>Pulire la distribuzione 

Eseguire questo comando per rimuovere il gruppo di risorse, la macchina virtuale e tutte le risorse correlate.

```azurecli
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Spiegazione dello script

Questo script usa i comandi seguenti per creare un disco gestito, una macchina virtuale e tutte le risorse correlate. Ogni comando della tabella include collegamenti alla documentazione specifica del comando.

| Comando | Note |
|---|---|
| [az snapshot show](https://docs.microsoft.com/cli/azure/snapshot#show) | Ottiene lo snapshot usando il nome dello snapshot e il nome del gruppo di risorse. La proprietà Id dell'oggetto restituito viene usata per creare un disco gestito.  |
| [az disk create](https://docs.microsoft.com/cli/azure/disk#create) | Crea dischi gestiti da uno snapshot usando l'ID dello snapshot, il nome del disco, il tipo di archiviazione e la dimensione  |
| [az vm create](https://docs.microsoft.com/cli/azure/vm#create) | Crea una VM utilizzando un disco del sistema operativo gestito |

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni sull'interfaccia della riga di comando di Azure, vedere la [documentazione sull'interfaccia della riga di comando di Azure](https://docs.microsoft.com/cli/azure/overview).

Altri esempi di script dell'interfaccia della riga di comando della macchina virtuale sono reperibili nella [documentazione della VM Linux di Azure](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

