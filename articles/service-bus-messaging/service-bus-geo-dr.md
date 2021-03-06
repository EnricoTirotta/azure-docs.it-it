---
title: Ripristino di emergenza geografico per il bus di servizio di Azure | Microsoft Docs
description: Come usare le aree geografiche per il failover ed eseguire il ripristino di emergenza nel bus di servizio di Azure
services: service-bus-messaging
documentationcenter: 
author: christianwolf42
manager: timlt
editor: 
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2017
ms.author: sethm
ms.openlocfilehash: fdeb9ba55fc8eade95f6fca88f47dd12aa18a480
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/16/2017
---
# <a name="azure-service-bus-geo-disaster-recovery"></a>Azure Service Bus Geo-ripristino d'emergenza

Quando l'intero Data Center o aree di Azure (se non [zone disponibilità](../availability-zones/az-overview.md) vengono utilizzati) verificarsi tempi di inattività, è fondamentale per l'elaborazione dati continuare a operare in un'area diversa o un Data Center. Il *ripristino di emergenza geografico* e la *replica geografica* sono quindi funzionalità importanti per qualsiasi azienda. Service Bus di Azure supporta il ripristino di emergenza di area geografica e replica geografica, a livello di spazio dei nomi. 

La funzionalità di ripristino di emergenza di geografica è disponibile a livello globale per lo SKU Premium Bus di servizio. 

## <a name="outages-and-disasters"></a>Emergenze e interruzioni

È importante notare la distinzione tra "interruzioni" e "emergenza". Un'*interruzione* è l'indisponibilità temporanea del bus di servizio di Azure e può interessare alcuni componenti del servizio, ad esempio un archivio di messaggistica, oppure l'intero data center. Tuttavia, dopo la risoluzione del problema, Bus di servizio diventa nuovamente disponibile. In genere, un'interruzione non determina la perdita di messaggi o di altri dati. Un'interruzione può essere provocata ad esempio da un'interruzione dell'alimentazione nel data center. Alcuni interruzioni sono solo le perdite di connessione breve a causa di problemi di rete o temporaneo. 

Oggetto *emergenza* è definito come la perdita permanente o a lungo termine di una regione di Azure, datacenter o cluster di Service Bus. L'area o Data Center può o non diventi disponibile nuovamente o potrebbe essere inattivo per ore o giorni. Un'emergenza può essere causata, ad esempio, da un incendio, un'inondazione o un terremoto. Una situazione di emergenza diventa permanente potrebbe causare la perdita di alcuni messaggi, eventi o altri dati. Tuttavia, nella maggior parte dei casi non dovrebbe esserci perdita di dati e i messaggi possono essere ripristinati dopo aver eseguito il backup del data center.

La funzionalità di ripristino di emergenza geografico del bus di servizio di Azure è una soluzione di ripristino di emergenza. I concetti e il flusso di lavoro illustrati in questo articolo sono applicabili a scenari di emergenza, non a interruzioni temporanee. Per una descrizione dettagliata del ripristino di emergenza in Microsoft Azure, vedere [questo articolo](/azure/architecture/resiliency/disaster-recovery-azure-applications).   

## <a name="basic-concepts-and-terms"></a>Concetti e terminologia di base

La funzionalità di ripristino di emergenza implementa il ripristino di emergenza di metadati e si basa sugli spazi dei nomi ripristino di emergenza primario e secondario. Si noti che la funzionalità di ripristino di emergenza di geografica è disponibile per il [SKU Premium](service-bus-premium-messaging.md) solo. Non è necessario apportare modifiche alla stringa di connessione, perché la connessione viene effettuata tramite un alias.

In questo articolo viene usata la terminologia seguente:

-  *Alias*: il nome per una configurazione di ripristino di emergenza impostati. L'alias fornisce una singola stringa di connessione FQDN (nome di dominio completo) stabile. Le applicazioni usano questa stringa di connessione alias per connettersi a uno spazio dei nomi. 

-  *Spazio dei nomi primario o secondario*: gli spazi dei nomi che corrispondono all'alias. Spazio dei nomi primario è "attivo" e riceve messaggi (può essere uno spazio dei nomi esistente o nuova). Spazio dei nomi secondario è "passivo" e non riceve i messaggi. I metadati tra entrambi vengono sincronizzati, pertanto entrambi facilmente può accettare messaggi senza modifiche stringa di connessione o codice di applicazione. Per garantire che solo lo spazio dei nomi active riceve i messaggi, è necessario utilizzare l'alias. 

-  *Metadati*: entità, ad esempio code, argomenti e sottoscrizioni e le relative proprietà associate allo spazio dei nomi del servizio. Si noti che solo le entità e le relative impostazioni vengono replicate automaticamente. I messaggi non vengono replicati. 

-  *Failover*: processo di attivazione dello spazio dei nomi secondario.

## <a name="setup-and-failover-flow"></a>Flusso di programma di installazione e il failover

Nella sezione seguente viene fornita una panoramica del processo di failover e viene spiegato come configurare il failover iniziale. 

![1][]

### <a name="setup"></a>Configurazione

È prima di tutto creare o usare uno spazio dei nomi primario e un nuovo spazio dei nomi secondario, quindi associare i due. L'associazione fornisce un alias che può essere usato per la connessione. Poiché si usa un alias, non è necessario modificare le stringhe di connessione. È possibile aggiungere solo nuovi spazi dei nomi all'associazione di failover. Infine, è necessario aggiungere alcune attività di monitoraggio per rilevare se è necessario un failover. Nella maggior parte dei casi, il servizio è una parte di un ecosistema di grandi dimensioni, pertanto i failover automatici sono raramente possibili, molto spesso i failover devono essere eseguiti la sincronizzazione con il sottosistema o infrastruttura rimanente.

### <a name="example"></a>Esempio

Nell'esempio di questo scenario, considerare una soluzione di punto di vendita (POS) che genera i messaggi o gli eventi. Bus di servizio passa gli eventi per alcuni mapping o la formattazione di soluzione, che quindi inoltra i dati sottoposti a mapping a un altro sistema per un'ulteriore elaborazione. A questo punto, tutti questi sistemi potrebbe trovarsi nella stessa area di Azure. La decisione su quando e le parti per eseguire il failover dipende il flusso di dati nell'infrastruttura. 

È possibile automatizzare il failover con sistemi di controllo o con soluzioni di monitoraggio personalizzate. Tuttavia, tali automazione ha pianificazione aggiuntiva e lavoro, che non rientra nell'ambito di questo articolo.

### <a name="failover-flow"></a>Flusso di failover

Se si avvia il failover, sono necessari due passaggi:

1. Se si verifica un'interruzione di un altro, si desidera essere in grado di eseguire il failover. Pertanto, impostare un altro spazio dei nomi passivo e aggiornare l'associazione. 

2. Pull dei messaggi dallo spazio dei nomi primario precedente quando sarà nuovamente disponibile. Successivamente, utilizzare tale spazio dei nomi per la messaggistica regolare di fuori del programma di installazione geografica ripristino oppure eliminare lo spazio dei nomi primario precedente.

> [!NOTE]
> È supportata solo semantica di inoltro hanno esito negativo. In questo scenario, il failover e quindi associare nuovamente con un nuovo spazio dei nomi. Failback non è supportato. ad esempio, in un cluster SQL. 

![2][]

## <a name="management"></a>Gestione

Se si commette un errore; ad esempio, è abbinato le aree non corrette durante l'installazione iniziale, è possibile interrompere l'associazione degli spazi dei due nomi in qualsiasi momento. Se si desidera utilizzare gli spazi dei nomi associato come normali spazi dei nomi, è possibile eliminare l'alias.

## <a name="use-existing-namespace-as-alias"></a>Usa lo spazio dei nomi esistente come alias

Se si dispone di uno scenario in cui non è possibile modificare le connessioni di producer e consumer, è possibile riutilizzare il nome dello spazio dei nomi come il nome dell'alias. Vedere il [di esempio qui il codice su GitHub](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/GeoDR/SBGeoDR2/SBGeoDR_existing_namespace_name).

## <a name="samples"></a>Esempi

Il [esempi su GitHub](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/GeoDR/SBGeoDR2/SBGeoDR2) viene illustrato come configurare e avviare un failover. Questi esempi illustrano i concetti seguenti:

- Impostazioni in Azure Active Directory è necessarie utilizzare Gestione risorse di Azure con il Bus di servizio. 
- Passaggi necessari per eseguire il codice di esempio. 
- Inviare e ricevere dallo spazio dei nomi primario corrente. 
- Descrive come usare uno spazio dei nomi esistente come alias.

## <a name="considerations"></a>Considerazioni

Tenere presente le considerazioni da tenere presenti in questa versione seguenti:

1. Durante la pianificazione di failover, è consigliabile considerare il fattore tempo. Ad esempio, se si perde la connessione per più di 15-20 minuti, è possibile decidere avviare il failover. 
 
2. Il fatto che nessun dato venga replicato significa che attualmente sessioni attive non vengono replicate. Inoltre, il rilevamento dei duplicati e messaggi pianificati potrebbero non funzionare. Nuove sessioni e messaggi pianificati duplicati nuovo funzionerà. 

3. Failover di un'infrastruttura distribuita complessa deve essere [provare a implementarlo](/azure/architecture/resiliency/disaster-recovery-azure-applications#disaster-simulation) almeno una volta. 

4. La sincronizzazione delle entità può richiedere del tempo, circa il 50-100 entità al minuto. Contano anche le sottoscrizioni e regole come entità. 

## <a name="next-steps"></a>Passaggi successivi

- Per informazioni di riferimento sull'API REST per il ripristino di emergenza geografico, vedere [qui](/rest/api/servicebus/disasterrecoveryconfigs).
- Eseguire l'[esempio di GitHub](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/GeoDR/SBGeoDR2/SBGeoDR2) sul ripristino di emergenza geografico.
- Vedere l'[esempio che invia messaggi a un alias](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/GeoDR/TestGeoDR/ConsoleApp1) per il ripristino di emergenza geografico.

Per altre informazioni sulla messaggistica del bus di servizio, vedere gli articoli seguenti:

* [Dati fondamentali del bus di servizio](service-bus-fundamentals-hybrid-solutions.md)
* [Code, argomenti e sottoscrizioni del bus di servizio](service-bus-queues-topics-subscriptions.md)
* [Introduzione alle code del bus di servizio](service-bus-dotnet-get-started-with-queues.md)
* [Come usare gli argomenti e le sottoscrizioni del bus di servizio](service-bus-dotnet-how-to-use-topics-subscriptions.md)
* [API REST](/rest/api/servicebus/) 

[1]: ./media/service-bus-geo-dr/geo1.png
[2]: ./media/service-bus-geo-dr/geo2.png
