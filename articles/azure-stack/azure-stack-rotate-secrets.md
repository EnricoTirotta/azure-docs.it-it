---
title: Ruotare i segreti nello Stack di Azure | Documenti Microsoft
description: Informazioni su come eseguire la rotazione i segreti nello Stack di Azure.
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
editor: 
ms.assetid: 49071044-6767-4041-9EDD-6132295FA551
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/08/2018
ms.author: mabrigg
ms.openlocfilehash: 0a4118a8927e4261fafa307af5b9c29623ce5c3f
ms.sourcegitcommit: e19f6a1709b0fe0f898386118fbef858d430e19d
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/13/2018
---
# <a name="rotate-secrets-in-azure-stack"></a>Ruotare i segreti nello Stack di Azure

*Si applica a: Azure Stack integrate di sistemi*

Aggiornare le password per i componenti dello Stack di Azure a un ritmo regolare.

## <a name="updating-the-passwords-for-the-baseboard-management-controller-bmc"></a>Aggiornare le password per baseboard management controller (BMC)

I baseboard management controller (BMC) monitorare lo stato dei server fisico. Le istruzioni su come aggiornare la password del BMC specifiche variano in base al fornitore dell'hardware (OEM) original equipment manufacturer.

1. Aggiornare il BMC su server fisici di Stack Azure seguendo le istruzioni OEM. La password per ciascun BMC nell'ambiente in uso deve essere lo stesso.
2. Aprire un endpoint con privilegi in Azure Stack sessioni. Per istruzioni, vedere [utilizzando l'endpoint con privilegi in Azure Stack](azure-stack-privileged-endpoint.md).
3. Dopo il PowerShell Prompt dei comandi è stato modificato in **[nome indirizzo IP o VM ERCS]: PS >** o **[azs ercs01]: PS >**, a seconda dell'ambiente, eseguire `Set-BmcPassword` eseguendo `invoke-command`. Passare la variabile di sessione con privilegi endpoint come parametro.  
Ad esempio: 
    ```powershell
    $PEPSession = New-PSSession -ComputerName <ERCS computer name> -Credential <CloudAdmin credential> -ConfigurationName "PrivilegedEndpoint"  
    
    Invoke-Command -Session $PEPSession -ScriptBlock {
        param($password)
        set-bmcpassword -bmcpassword $password
    } -ArgumentList (<LatestPassword as a SecureString>) 
    ```

## <a name="next-steps"></a>Passaggi successivi

Per ulteriori informazioni sullo Stack di Azure e la sicurezza, vedere [condizioni di sicurezza dell'infrastruttura di Azure Stack](azure-stack-security-foundations.md).