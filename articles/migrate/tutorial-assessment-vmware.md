---
title: Individuare e valutare le macchine virtuali VMware locali per la migrazione ad Azure con Azure Migrate | Microsoft Docs
description: Questo articolo descrive come individuare e valutare le macchine virtuali VMware locali per la migrazione ad Azure usando il servizio Azure Migrate.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: tutorial
ms.date: 06/02/2018
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: 0c82eeaeb17fb670b6d277d1b703b44b84343877
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 02/21/2018
---
# <a name="discover-and-assess-on-premises-vmware-vms-for-migration-to-azure"></a>Individuare e valutare le macchine virtuali VMware locali per la migrazione ad Azure

Il servizio [Azure Migrate](migrate-overview.md) valuta i carichi di lavoro locali per la migrazione ad Azure.

In questa esercitazione si apprenderà come:

> [!div class="checklist"]
> * Creare un progetto di Azure Migrate.
> * Configurare una macchina virtuale locale dell'agente di raccolta per individuare le macchine virtuali VMware locali per la valutazione.
> * Raggruppare le macchine virtuali e creare una valutazione.


Se non si ha una sottoscrizione di Azure, creare un [account gratuito](https://azure.microsoft.com/pricing/free-trial/) prima di iniziare.


## <a name="prerequisites"></a>prerequisiti

- **VMware**: le macchine virtuali che si prevede di migrare devono essere gestite tramite il server vCenter versione 5.5, 6.0 o 6.5. Inoltre, è necessario che sia in esecuzione l'host ESXi versione 5.0 o versione successiva per distribuire la macchina virtuale dell'agente di raccolta. 
 
> [!NOTE]
> Il supporto per Hyper-V è in programma e sarà disponibile a breve. 

- **Account server vCenter**: è necessario un account di sola lettura per accedere al server vCenter. Azure Migrate usa questo account per individuare le macchine virtuali.Azure Migrate usa questo account per individuare le macchine virtuali locali.
- **Autorizzazioni**: nel server vCenter è necessario avere le autorizzazioni per creare una macchina virtuale importando un file con estensione ova. 
- **Impostazioni delle statistiche**: prima di avviare la distribuzione, le statistiche del server vCenter devono essere impostate sul livello 3. Con un livello inferiore a 3, viene eseguita la valutazione, ma non vengono raccolti i dati sulle prestazioni per l'archiviazione e la rete. In questo caso le dimensioni consigliate verranno stabilite in base ai dati sulle prestazioni per i dati di CPU, memoria e configurazione per le schede disco e di rete. 

## <a name="log-in-to-the-azure-portal"></a>Accedere al Portale di Azure
Accedere al [Portale di Azure](https://portal.azure.com).

## <a name="create-a-project"></a>Creare un progetto

1. Nel portale di Azure fare clic su **Crea una risorsa**.
2. Cercare **Azure Migrate** e selezionare il servizio **Azure Migrate (preview)** nei risultati della ricerca. Fare quindi clic su **Crea**.
3. Specificare un nome di progetto e la sottoscrizione di Azure per il progetto.
4. Creare un nuovo gruppo di risorse.
5. Specificare il percorso in cui creare il progetto e quindi fare clic su **Crea**. Per questa anteprima è possibile creare un progetto di Azure Migrate solo nell'area Stati Uniti centro-occidentali, Tuttavia, è comunque possibile pianificare la migrazione per qualsiasi località di Azure di destinazione. Il percorso specificato per il progetto viene usato solo per archiviare i metadati raccolti da macchine virtuali locali. 

    ![Azure Migrate](./media/tutorial-assessment-vmware/project-1.png)
    


## <a name="download-the-collector-appliance"></a>Scaricare l'appliance dell'agente di raccolta

Azure Migrate crea una macchina virtuale locale definita appliance dell'agente di raccolta. Questa macchina virtuale individua le macchine virtuali VMware locali e invia i relativi metadati al servizio Azure Migrate. Per configurare l'appliance dell'agente di raccolta, si scarica un file con estensione ova e si importa il file nel server vCenter locale per creare la macchina virtuale.

1. Nel progetto di Azure Migrate fare clic su **Attività iniziali** > **Individua e valuta** > **Individua macchine virtuali**.
2. In **Individua macchine virtuali** fare clic su **Scarica** per scaricare il file con estensione ova.
3. In **Copiare le credenziali del progetto** copiare l'ID e la chiave del progetto. Queste informazioni sono necessarie per configurare l'agente di raccolta.

    ![Scaricare il file con estensione ova](./media/tutorial-assessment-vmware/download-ova.png)

### <a name="verify-the-collector-appliance"></a>Verificare l'appliance dell'agente di raccolta

Verificare che il file con estensione ova sia sicuro prima di distribuirlo.

1. Nel computer in cui è stato scaricato il file aprire una finestra di comando con privilegi di amministratore.
2. Eseguire il comando seguente per generare il valore hash per il file con estensione ova:
    - ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```
    - Esempio di utilizzo: ```C:\>CertUtil -HashFile C:\AzureMigrate\AzureMigrate.ova SHA256```
3. Il valore hash generato deve corrispondere a queste impostazioni.
    
    Per OVA versione 1.0.8.59

    **Algoritmo** | **Valore hash**
    --- | ---
    MD5 | 71139e24a532ca67669260b3062c3dad
    SHA1 | 1bdf0666b3c9c9a97a07255743d7c4a2f06d665e
    SHA256 | 6b886d23b24c543f8fc92ff8426cd782a77efb37750afac397591bda1eab8656  

    Per OVA versione 1.0.8.49:
    **Algoritmo** | **Valore hash**
    --- | ---
    MD5 | cefd96394198b92870d650c975dbf3b8 
    SHA1 | 4367a1801cf79104b8cd801e4d17b70596481d6f
    SHA256 | fda59f076f1d7bd3ebf53c53d1691cc140c7ed54261d0dc4ed0b14d7efef0ed9

    Per OVA versione 1.0.8.40:

    **Algoritmo** | **Valore hash**
    --- | ---
    MD5 | afbae5a2e7142829659c21fd8a9def3f
    SHA1 | 1751849c1d709cdaef0b02a7350834a754b0e71d
    SHA256 | d093a940aebf6afdc6f616626049e97b1f9f70742a094511277c5f59eacc41ad

## <a name="create-the-collector-vm"></a>Creare la macchina virtuale dell'agente di raccolta

Importare il file scaricato nel server vCenter.

1. Nella console di vSphere Client fare clic su **File** > **Deploy OVF Template** (Distribuisci modello OVF).

    ![Distribuire il modello OVF](./media/tutorial-assessment-vmware/vcenter-wizard.png)

2. In Deploy OVF Template Wizard (Distribuzione guidata del modello OVF) > **Source** (Origine) specificare il percorso del file con estensione ova.
3. In **Name** (Nome) e **Location** (Posizione) specificare un nome descrittivo per la macchina virtuale dell'agente di raccolta e l'oggetto dell'inventario in cui verrà ospitata la macchina virtuale.
5. In **Host/Cluster** specificare l'host o il cluster in cui verrà eseguita la macchina virtuale dell'agente di raccolta.
7. Nell'area relativa ai dati di archiviazione specificare la destinazione di archiviazione per la macchina virtuale dell'agente di raccolta.
8. In **Disk Format** (Formato disco) specificare il tipo e la dimensione del disco.
9. In **Network Mapping** (Mapping di rete) specificare la rete a cui si connetterà la macchina virtuale dell'agente di raccolta. La rete richiede la connettività Internet per l'invio dei metadati ad Azure. 
10. Rivedere e confermare le impostazioni e quindi fare clic su **Finish** (Fine).

## <a name="run-the-collector-to-discover-vms"></a>Eseguire l'agente di raccolta per individuare le macchine virtuali

1. Nella console di vSphere Client fare clic con il pulsante destro del mouse su VM > **Open Console** (Apri console).
2. Specificare le preferenze relative alla lingua, al fuso orario e alla password per l'appliance.
3. Sul desktop fare clic sul collegamento **Run collector** (Esegui agente di raccolta).
4. In Agente di raccolta di Azure Migrate aprire **Set up prerequisites** (Configura prerequisiti).
    - Accettare le condizioni di licenza e leggere le informazioni di terze parti.
    - L'agente di raccolta verifica che la macchina virtuale abbia accesso a Internet.
    - Se la macchina virtuale accede a Internet tramite un proxy, fare clic su **Proxy settings** (Impostazioni proxy) e specificare l'indirizzo e la porta di ascolto del proxy. Se il proxy richiede l'autenticazione, specificare le credenziali.

    > [!NOTE]
    > L'indirizzo proxy deve essere immesso nel formato http://ProxyIPAddress o http://ProxyFQDN. È supportato solo il proxy HTTP.

    - L'agente di raccolta verifica che il servizio dell'agente di raccolta sia in esecuzione. Il servizio è installato per impostazione predefinita nella macchina virtuale dell'agente di raccolta.
    - Scaricare e installare VMware PowerCLI.

5. In **Specify vCenter Server details** (Specificare i dettagli del Server vCenter) eseguire queste operazioni:
    - Specificare il nome completo o l'indirizzo IP del server vCenter.
    - In **User name** (Nome utente) e **Password** specificare le credenziali dell'account di sola lettura che verranno usate dall'agente di raccolta per individuare le macchine virtuali nel server vCenter.
    - In **Collection scope** (Ambito raccolta) selezionare un ambito per l'individuazione delle macchine virtuali. L'agente di raccolta può individuare solo le macchine virtuali all'interno dell'ambito specificato. L'ambito può essere impostato su una cartella, un data center o un cluster, ma non deve contenere più di 1000 macchine virtuali. 

6. In **Specify migration project** (Specificare il progetto di migrazione) specificare l'ID e la chiave del progetto di Azure Migrate copiati dal portale. Se questi valori non sono stati copiati, aprire il portale di Azure dalla macchina virtuale dell'agente di raccolta. Nella pagina **Panoramica** del progetto fare clic su **Individua macchine virtuali** e copiare i valori.  
7. In **View collection progress** (Visualizza stato raccolta) monitorare l'individuazione e verificare che i metadati raccolti dalle macchine virtuali siano inclusi nell'ambito. L'agente di raccolta indica un tempo di individuazione approssimativo.

> [!NOTE]
> Come lingua del sistema operativo e lingua dell'interfaccia dell'agente di raccolta è supportato solo l'inglese (Stati Uniti). Sarà presto disponibile il supporto per altre lingue.


### <a name="verify-vms-in-the-portal"></a>Verificare le macchine virtuali nel portale

Il tempo di individuazione dipende dal numero di macchine virtuali da individuare. In genere, per completare l'individuazione di 100 macchine virtuali, è necessario aspettare circa un'ora dopo l'esecuzione dell'agente di raccolta. 

1. Nel progetto di Azure Migrate fare clic su **Gestisci** > **Macchine virtuali**.
2. Verificare che le macchine virtuali da individuare siano visualizzate nel portale.


## <a name="create-and-view-an-assessment"></a>Creare e visualizzare una valutazione

Dopo aver individuato le macchine virtuali, è possibile raggrupparle e creare una valutazione. 

1. Nella pagina **Panoramica** del progetto fare clic su **+Crea valutazione**.
2. Fare clic su **Visualizza tutto** per rivedere le proprietà di valutazione.
3. Creare il gruppo e specificarne il nome.
4. Selezionare le macchine virtuali da aggiungere al gruppo.
5. Fare clic su **Crea valutazione** per creare il gruppo e la valutazione.
6. Dopo aver creato la valutazione, visualizzarla in **Panoramica** > **Dashboard**.
7. Fare clic su **Esporta valutazione** per scaricarla come file di Excel.

### <a name="assessment-details"></a>Dettagli valutazione

Una valutazione include informazioni sulla compatibilità o meno delle macchine virtuali locali per Azure, sulle dimensioni di macchina virtuale più appropriate in Azure e sui costi di Azure mensili stimati. 

![Report di valutazione](./media/tutorial-assessment-vmware/assessment-report.png)

#### <a name="azure-readiness"></a>Idoneità per Azure

La visualizzazione Idoneità per Azure nella valutazione mostra lo stato di idoneità di ogni macchina virtuale. A seconda delle proprietà della macchina virtuale, ogni macchina virtuale può essere contrassegnata come:
- Idonea per Azure
- Idonea con condizioni
- Non idonea per Azure
- Idoneità sconosciuta 

Per le macchine virtuali idonee, Azure Migrate offre un consiglio riguardo alle dimensioni delle macchine in Azure. L'indicazione relativa alle dimensioni fornita da Azure Migrate dipende dal criterio di ridimensionamento specificato nelle proprietà della valutazione. Se il criterio è il ridimensionamento in base alle prestazioni, l'indicazione relativa alle dimensioni prende in considerazione la cronologia delle prestazioni delle macchine virtuali. Se il criterio è il ridimensionamento come in locale, l'indicazione prende in considerazione la dimensione della macchina virtuale in locale (ridimensionamento così com'è). In questo caso, i dati sull'utilizzo non vengono considerati. [Altre informazioni](concepts-assessment-calculation.md) su come viene eseguito il ridimensionamento in Azure Migrate. 

Per le macchine virtuali non idonee o idonee con condizioni, Azure Migrate descrive il problema correlato e indica i passaggi correttivi da eseguire. 

Le macchine virtuali per cui Azure Migrate non è in grado di identificare l'idoneità per Azure (a causa della mancata disponibilità dei dati) sono contrassegnate con idoneità sconosciuta.

Oltre all'idoneità per Azure e al ridimensionamento, Azure Migrate consiglia anche gli strumenti che è possibile usare per la migrazione della macchina virtuale. Se la macchina virtuale è adatta alla migrazione in modalità lift-and-shift, viene consigliato di usare il servizio [Azure Site Recovery]. Se si tratta di una macchina virtuale di database, viene consigliato di usare Servizio Migrazione del database di Azure.

  ![Idoneità per la valutazione](./media/tutorial-assessment-vmware/assessment-suitability.png)  

#### <a name="monthly-cost-estimate"></a>Stima costo mensile

In questa visualizzazione viene mostrato il costo di calcolo e archiviazione totale dell'esecuzione delle macchine virtuali in Azure con i dettagli per ogni computer. Le stime dei costi vengono calcolate usando le impostazioni delle proprietà di valutazione e le dimensioni consigliate in base alle prestazioni per una macchina virtuale e i relativi dischi. 

> [!NOTE]
> La stima dei costi fornita da Azure Migrate si riferisce all'esecuzione delle macchine virtuali locali come macchine virtuali dell'infrastruttura distribuita come servizio (IaaS) di Azure. In Azure Migrate non vengono considerati i costi relativi alla piattaforma distribuita come servizio (PaaS, Platform as a Service) o al software come un servizio (SaaS, Software as a Service). 

I costi mensili stimati per il calcolo e l'archiviazione vengono aggregati per tutte le macchine virtuali del gruppo. 

![Valutazione dei costi delle macchine virtuali](./media/tutorial-assessment-vmware/assessment-vm-cost.png) 

#### <a name="confidence-rating"></a>Classificazione di attendibilità

Ogni valutazione in Azure Migrate è associata a una classificazione di attendibilità compresa tra 1 stella e 5 stelle, dove 1 stella corrisponde al livello inferiore e 5 stelle al livello superiore. La classificazione di attendibilità viene assegnata a una valutazione in base alla disponibilità dei punti dati necessari per calcolare la valutazione. La classificazione aiuta a stimare l'affidabilità delle indicazioni relative alle dimensioni fornite da Azure Migrate. 

La classificazione di attendibilità è utile quando si esegue un *ridimensionamento in base alle prestazioni*, in quanto non tutti i punti dati possono essere disponibili. Per un *ridimensionamento come in locale*, la classificazione di attendibilità è sempre 5 stelle, perché Azure Migrate ha a disposizione tutti i dati necessari per ridimensionare la macchina virtuale. 

Per un ridimensionamento in base alle prestazioni, Azure Migrate deve disporre dei dati sull'utilizzo di CPU e memoria. Per ogni disco collegato alla macchina virtuale, il servizio deve leggere le operazioni di I/O al secondo in lettura/scrittura e la velocità effettiva per applicare il ridimensionamento in base alle prestazioni. Analogamente, per ogni scheda di rete collegata alla macchina virtuale, Azure Migrate deve disporre dei dati sull'ingresso/uscita di rete per applicare il ridimensionamento in base alle prestazioni. Se una qualsiasi delle cifre sull'utilizzo indicate sopra non è disponibile nel server vCenter, l'indicazione relativa alle dimensioni fornita da Azure Migrate potrebbe non essere affidabile. Di seguito viene indicata la classificazione di attendibilità per la valutazione in base alla percentuale dei punti dati disponibili:

   **Disponibilità dei punti dati** | **Classificazione di attendibilità**
   --- | ---
   0%-20% | 1 stella
   21%-40% | 2 stelle
   41%-60% | 3 stelle
   61%-80% | 4 stelle
   81%-100% | 5 stelle

Una valutazione può non avere a disposizione tutti i punti dati a causa di uno dei motivi seguenti:
- L'impostazione delle statistiche nel server vCenter non è impostata sul livello 3 e la valutazione applica il ridimensionamento in base alle prestazioni come criterio di ridimensionamento. Se l'impostazione delle statistiche nel server vCenter è inferiore al livello 3, i dati sulle prestazioni per i dischi e la rete non vengono raccolti da vCenter Server. In questo caso, l'indicazione fornita da Azure Migrate per dischi e rete si basa solo sull'allocazione in locale. Per l'archiviazione, Azure Migrate consiglia dischi standard, in quanto non esiste alcun modo per identificare se il disco ha operazioni di I/O al secondo e velocità effettiva elevate e richiede quindi dischi Premium.
- L'impostazione delle statistiche nel server vCenter è stata impostata sul livello 3 per un breve periodo di tempo, prima di avviare il processo di individuazione. Ad esempio, se si modifica il livello di questa impostazione su 3 oggi e si avvia l'individuazione usando l'appliance dell'agente di raccolta domani (24 ore dopo) creando una valutazione per un giorno, sono disponibili tutti i punti dati. Se invece si modifica a un mese il periodo di tempo delle prestazioni nelle proprietà della valutazione, la classificazione di attendibilità diminuisce, in quanto i dati sulle prestazioni di dischi e rete per l'ultimo mese non sono disponibili. Per tenere conto dei dati sulle prestazioni dell'ultimo mese, è consigliabile mantenere l'impostazione delle statistiche del server vCenter sul livello 3 per un mese prima di avviare il processo di individuazione. 
- Durante il periodo per cui viene calcolata la valutazione sono state arrestate alcune macchine virtuali. Se una o più macchine virtuali sono state spente per un certo periodo di tempo, il server vCenter non avrà i dati sulle prestazioni per questo periodo. 
- All'interno del periodo per cui viene calcolata la valutazione sono state create alcune macchine virtuali. Questa situazione si verifica, ad esempio, se si crea una valutazione per la cronologia delle prestazioni dell'ultimo mese, ma solo una settimana prima sono state create alcune macchine virtuali nell'ambiente. In questi casi, la cronologia delle prestazioni delle nuove macchine virtuali non sarà disponibile per l'intero periodo.

> [!NOTE]
> Se la classificazione di attendibilità di qualsiasi valutazione è inferiore a 3 stelle, è consigliabile modificare le impostazioni delle statistiche del server vCenter sul livello 3, attendere per il periodo che deve essere considerato nella valutazione (1 giorno/1 settimana/1 mese) e quindi eseguire l'individuazione e la valutazione. Se non è possibile applicare la soluzione indicata sopra, il ridimensionamento in base alle prestazioni potrebbe non essere affidabile ed è consigliabile passare al *ridimensionamento come in locale* modificando le proprietà della valutazione.
 
## <a name="next-steps"></a>Passaggi successivi

- [Informazioni](how-to-scale-assessment.md) su come individuare e valutare un ambiente VMware di grandi dimensioni.
- Informazioni su come creare gruppi di valutazione con affidabilità elevata usando il [mapping delle dipendenze delle macchine virtuali](how-to-create-group-machine-dependencies.md)
- [Altre informazioni](concepts-assessment-calculation.md) sul modo in cui vengono calcolate le valutazioni.
