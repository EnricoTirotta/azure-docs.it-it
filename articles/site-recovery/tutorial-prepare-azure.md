---
title: Creare risorse da usare con Azure Site Recovery | Microsoft Docs
description: Informazioni su come preparare Azure per la replica di computer locali usando il servizio Azure Site Recovery.
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: tutorial
ms.date: 01/16/2018
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: d1aadd6b44d64f0bdb35ea02d628bedfc366ad3c
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 02/21/2018
---
# <a name="prepare-azure-resources-for-replication-of-on-premises-machines"></a>Preparare le risorse di Azure per la replica dei computer locali

Il servizio [Azure Site Recovery](site-recovery-overview.md) contribuisce a realizzare la strategia di continuità aziendale e ripristino di emergenza (BCDR) mantenendo operative le app aziendali durante le interruzioni pianificate e non pianificate. Site Recovery gestisce e coordina il ripristino di emergenza di computer locali e macchine virtuali di Azure, incluse le operazioni di replica, failover e failback.

Questa esercitazione illustra come preparare i componenti di Azure quando si vuole eseguire la replica di VM locali (Hyper-V o VMware) o server fisici Windows/Linux in Azure. In questa esercitazione si apprenderà come:

> [!div class="checklist"]
> * Verificare che l'account abbia le autorizzazioni di replica
> * Creare un account di archiviazione di Azure
> * Impostare una rete di Azure. Le macchine virtuali di Azure create dopo il failover vengono aggiunte a questa rete di Azure.

Se non si ha una sottoscrizione di Azure, creare un [account gratuito](https://azure.microsoft.com/pricing/free-trial/) prima di iniziare.

## <a name="log-in-to-azure"></a>Accedere ad Azure

Accedere al portale di Azure all'indirizzo http://portal.azure.com.

## <a name="verify-account-permissions"></a>Verificare le autorizzazioni dell'account

Se è appena stato creato l'account Azure gratuito, si è amministratori della propria sottoscrizione. Se non si è l'amministratore della propria sottoscrizione, rivolgersi all'amministratore per l'assegnazione delle autorizzazioni necessarie. Per abilitare la replica per una nuova macchina virtuale, è necessario avere le autorizzazioni seguenti:

- Autorizzazione per la creazione di una macchina virtuale nel gruppo di risorse selezionato
- Autorizzazione per la creazione di una macchina virtuale nella rete virtuale selezionata
- Autorizzazione per la scrittura nell'account di archiviazione selezionato

Il ruolo predefinito 'Collaboratore Macchina virtuale' ha le autorizzazioni elencate. È necessario anche avere l'autorizzazione per la gestione delle operazioni di Azure Site Recovery. Il ruolo 'Collaboratore di Site Recovery' ha tutte le autorizzazioni necessarie per la gestione delle operazioni di Site Recovery in un insieme di credenziali dei servizi di ripristino.

## <a name="create-a-storage-account"></a>Creare un account di archiviazione

Le immagini delle macchine replicate sono archiviate nell'archiviazione di Azure. Le macchine virtuali di Azure vengono create dall'archiviazione quando si esegue il failover da locale ad Azure.

1. Nel menu del [portale di Azure](https://portal.azure.com) fare clic su **Nuovo** -> **Archiviazione** -> **Account di archiviazione**.
2. In **Crea account di archiviazione** immettere un nome per l'account. Per queste esercitazioni viene usato il nome **contosovmsacct1910171607**. Il nome deve essere univoco in Azure, avere una lunghezza compresa tra 3 e 24 caratteri e contenere solo numeri e lettere minuscole.
3. Usare il modello di distribuzione di **Resource Manager**.
4. Selezionare **Utilizzo generico** > **Standard**. Non selezionare l'archivio BLOB.
5. Selezionare il valore predefinito **RA-GRS** per la ridondanza di archiviazione.
6. Selezionare la sottoscrizione in cui creare il nuovo account di archiviazione.
7. Specificare un nuovo gruppo di risorse. Un gruppo di risorse di Azure è un contenitore logico in cui le risorse di Azure vengono distribuite e gestite. Per queste esercitazioni viene usato il nome **ContosoRG**.
8. Selezionare la posizione geografica dell'account di archiviazione. L'account di archiviazione deve trovarsi nella stessa area dell'insieme di credenziali dei servizi di ripristino. Per queste esercitazioni viene usata l'area **Europa occidentale**.

   ![create-storageacct](media/tutorial-prepare-azure/create-storageacct.png)

9. Fare clic su **Crea** per creare l'account di archiviazione.

## <a name="create-a-vault"></a>Creare un insieme di credenziali

1. Nel portale di Azure fare clic su **Crea una risorsa** > **Monitoraggio e gestione** > **Backup e Site Recovery**.
2. In **Nome**specificare un nome descrittivo per identificare l'insieme di credenziali. Per questa esercitazione viene usato **ContosoVMVault**.
3. Selezionare il gruppo di risorse esistente denominato **contosoRG**.
4. Specificare l'area di Azure **Europa occidentale**, usata in questo set di esercitazioni.
5. Per accedere rapidamente all'insieme di credenziali dal dashboard, fare clic su **Aggiungi al dashboard** > **Crea**.

   ![Nuovo insieme di credenziali](./media/tutorial-prepare-azure/new-vault-settings.png)

   Il nuovo insieme di credenziali verrà visualizzato in **Dashboard** > **Tutte le risorse** e nella pagina **Insiemi di credenziali dei servizi di ripristino** principale.

## <a name="set-up-an-azure-network"></a>Configurare una rete di Azure

Le macchine virtuali di Azure create dall'archiviazione dopo il failover vengono aggiunte a questa rete.

1. Nel [portale di Azure](https://portal.azure.com) fare clic su **Crea una risorsa** >  **Rete** > **Rete virtuale**.
2. Lasciare selezionato **Resource Manager** come modello di distribuzione. Resource Manager è il modello di distribuzione consigliato.
   - Specificare un nome di rete. Il nome deve essere univoco all'interno del gruppo di risorse di Azure. Viene usato il nome **ContosoASRnet**
   - Usare il gruppo di risorse esistente **contosoRG**.
   - Specificare l'intervallo di indirizzi di rete 10.0.0.0/24.
   - Per questa esercitazione non è necessaria una subnet.
   - Selezionare la sottoscrizione in cui creare la rete.
   - Selezionare la posizione **Europa occidentale**. La rete deve trovarsi nella stessa area dell'insieme di credenziali di Servizi di ripristino.
3. Fare clic su **Crea**.

   ![create-network](media/tutorial-prepare-azure/create-network.png)

   La creazione della rete virtuale richiede qualche secondo. La rete creata viene visualizzata nel dashboard del portale di Azure.

## <a name="next-steps"></a>Passaggi successivi

> [!div class="nextstepaction"]
> [Preparare l'infrastruttura VMware locale per il ripristino di emergenza in Azure](tutorial-prepare-on-premises-vmware.md)
