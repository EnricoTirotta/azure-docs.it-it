---
title: Creare il nome di dominio completo (FQDN) per una macchina virtuale Windows nel portale di Azure | Microsoft Docs
description: Informazioni su come creare un nome di dominio completo o FQDN per un Gestore risorse basato su macchina virtuale nel portale di Azure.
services: virtual-machines-windows
documentationcenter: 
author: iainfoulds
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: a2ae5887-76df-485e-ae19-0efd96df8600
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 12/13/2017
ms.author: iainfou
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 8ebc1ef89b24a9aa21f39e5b05051c16351f08cd
ms.sourcegitcommit: 0e4491b7fdd9ca4408d5f2d41be42a09164db775
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/14/2017
---
# <a name="create-a-fully-qualified-domain-name-in-the-azure-portal-for-a-windows-vm"></a>Creare un nome di dominio completo nel portale di Azure per una macchina virtuale Windows

Quando si crea una macchina virtuale nel [portale di Azure](https://portal.azure.com), il portale crea automaticamente una risorsa IP pubblica per la macchina virtuale. Usare questo indirizzo IP per accedere in remoto alla VM. Anche se il portale non crea un [nome di dominio completo](https://en.wikipedia.org/wiki/Fully_qualified_domain_name), o FQDN (Fully Qualified Domain Name), è possibile crearne uno dopo aver creato la VM. Questo articolo illustra i passaggi per creare un nome DNS o un FQDN.

## <a name="create-a-fqdn"></a>Creare un nome di dominio completo
In questo articolo si presuppone che sia già stata creata una VM. Se necessario, è possibile [creare una macchina virtuale nel portale](quick-create-portal.md) o [con Azure Powershell](quick-create-powershell.md). Dopo che la macchina virtuale è operativa, seguire questi passaggi:

[!INCLUDE [virtual-machines-common-portal-create-fqdn](../../../includes/virtual-machines-common-portal-create-fqdn.md)]

È ora possibile connettersi in modalità remota alla macchina virtuale usando questo nome DNS, ad esempio per Remote Desktop Protocol (RDP).

## <a name="next-steps"></a>Passaggi successivi
Quando la VM ha un indirizzo IP pubblico e un nome DNS, è possibile distribuire framework applicazioni o servizi comuni, ad esempio IIS, SQL o SharePoint.

Per suggerimenti sulla creazione di distribuzioni di Azure, vedere la pagina relativa all'[uso di Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md).

