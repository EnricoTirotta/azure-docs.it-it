---
title: "Usare probe personalizzati del servizio di bilanciamento del carico per monitorare lo stato di integrità | Microsoft Docs"
description: Informazioni su come usare probe personalizzati per il servizio di bilanciamento del carico di Azure per monitorare le relative istanze
services: load-balancer
documentationcenter: na
author: KumudD
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 46b152c5-6a27-4bfc-bea3-05de9ce06a57
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: kumud
ms.openlocfilehash: 266132d8cbb6f9922ce7b49759981132c2c17f47
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/11/2017
---
# <a name="understand-load-balancer-probes"></a>Informazioni sui probe del servizio di bilanciamento del carico

[!INCLUDE [load-balancer-basic-sku-include.md](../../includes/load-balancer-basic-sku-include.md)]

Il servizio di bilanciamento del carico di Azure consente di monitorare l'integrità delle istanze del server tramite probe. Se un probe non risponde, il servizio di bilanciamento del carico interrompe l'invio di nuove connessioni all'istanza non integra. Le connessioni esistenti non sono interessate, mentre quelle nuove vengono inviate alle istanze integre.

I ruoli del servizio cloud, ovvero i ruoli di lavoro e i ruoli Web, usano un agente guest per il monitoraggio probe. I probe TCP o HTTP personalizzati devono essere configurati quando si usano macchine virtuali monitorate tramite Load Balancer.

## <a name="understand-probe-count-and-timeout"></a>Informazioni su conteggio e timeout dei probe

Il comportamento dei probe dipende dagli elementi seguenti:

* Numero di probe riusciti che consentono di etichettare un'istanza come attiva.
* Numero di probe non riusciti che fanno sì che un'istanza sia etichettata come inattiva.

I valori di timeout e frequenza impostati in SuccessFailCount determinano se un'istanza è confermata come in esecuzione o non in esecuzione. Nel portale di Azure il timeout è impostato sul doppio del valore della frequenza.

La configurazione dei probe deve essere la stessa in tutte le istanze con bilanciamento del carico per un endpoint, ovvero un set con bilanciamento del carico. Non è possibile avere una configurazione dei probe diversa per ogni istanza del ruolo o macchina virtuale nello stesso servizio ospitato per una combinazione di endpoint specifica. Ad esempio, ogni istanza deve avere porte locali e i timeout identici.

> [!IMPORTANT]
> Un probe del servizio di bilanciamento del carico usa l'indirizzo IP 168.63.129.16. Questo indirizzo IP pubblico facilita la comunicazione con le risorse interne della piattaforma per lo scenario Rete virtuale di Azure che prevede l'uso di un IP personale ("bring your own IP"). L'indirizzo IP pubblico virtuale 168.63.129.16 viene usato in tutte le aree e non cambia. È consigliabile consentire questo indirizzo IP in tutti i criteri firewall locali. Non deve essere considerato come un rischio per la sicurezza, perché solo la piattaforma Azure interna può generare un messaggio da questo indirizzo. Se non si consente questo indirizzo IP nei criteri del firewall, è possibile riscontrare comportamenti imprevisti in un'ampia gamma di scenari. I comportamenti imprevisti possono includere la configurazione dello stesso intervallo di indirizzi IP di 168.63.129.16 e la duplicazione degli indirizzi IP.

## <a name="learn-about-the-types-of-probes"></a>Informazioni sui tipi di probe

### <a name="guest-agent-probe"></a>Probe dell'agente guest

Un probe dell'agente guest è disponibile solo per i servizi cloud di Azure. Il servizio di bilanciamento del carico usa l'agente guest all'interno della macchina virtuale. L'agente guest è quindi in ascolto e risponde con HTTP 200 OK solo quando l'istanza è in stato Pronto. Gli altri stati sono Occupato, Riciclo in corso o Arresto.

Per altre informazioni, vedere [Configure the service definition file (csdef) for health probes](https://msdn.microsoft.com/library/azure/ee758710.aspx) (Configurare il file csdef per probe di integrità) o [Get started by creating a public load balancer for cloud services](load-balancer-get-started-internet-classic-cloud.md#check-load-balancer-health-status-for-cloud-services) (Introduzione alla creazione di un servizio di bilanciamento del carico pubblico per i servizi cloud).

### <a name="what-makes-a-guest-agent-probe-mark-an-instance-as-unhealthy"></a>Perché un probe dell'agente guest contrassegna un'istanza come non integra

Se l'agente guest non risponde con un messaggio HTTP 200 OK, il servizio di bilanciamento del carico contrassegna l'istanza come istanza che non risponde. Il servizio smette quindi di inviare traffico all'istanza. Il servizio di bilanciamento del carico continua a eseguire il ping dell'istanza. Se l'agente guest risponde con un messaggio HTTP 200, il servizio di bilanciamento del carico continua a inviare il traffico a tale istanza.

Quando si usa un ruolo Web, il codice del sito Web viene in genere eseguito in w3wp.exe, che non è monitorato dall'infrastruttura di Azure o dall'agente guest. Gli errori in w3wp.exe, ad esempio le risposte HTTP 500, non vengono segnalati all'agente guest. Di conseguenza, il servizio di bilanciamento del carico non rimuove l'istanza dalla rotazione.

### <a name="http-custom-probe"></a>Probe HTTP personalizzato

Il probe HTTP personalizzato esegue l'override del probe dell'agente guest predefinito. È possibile creare logica personalizzata per determinare l'integrità dell'istanza del ruolo. Per impostazione predefinita, il servizio di bilanciamento del carico esegue regolarmente probe sull'endpoint ogni 15 secondi. L'istanza viene considerata inclusa nella rotazione del servizio di bilanciamento del carico se risponde con un messaggio HTTP 200 entro il periodo di timeout. Per impostazione predefinita, il periodo di timeout è 31 secondi.

Un probe HTTP personalizzato può essere utile se si vuole implementare logica personalizzata per rimuovere le istanze dalla rotazione del servizio di bilanciamento del carico. Ad esempio, si potrebbe scegliere di rimuovere un'istanza se indica oltre il 90% di utilizzo della CPU e restituisce uno stato diverso da 200. Per i ruoli Web che usano w3wp.exe, si ottiene anche il monitoraggio automatico del sito Web. Gli errori nel codice del sito Web restituiscono uno stato diverso da 200 al probe del servizio di bilanciamento del carico.

> [!NOTE]
> Il probe HTTP personalizzato supporta solo percorsi relativi e il protocollo HTTP. Il protocollo HTTPS non è supportato.

### <a name="what-makes-an-http-custom-probe-mark-an-instance-as-unhealthy"></a>Perché un probe HTTP personalizzato contrassegna un'istanza come non integra

* L'applicazione HTTP restituisce un codice di risposta HTTP diverso da 200, ad esempio 403, 404 o 500. Questo acknowledgment positivo comunica di rimuovere immediatamente l'istanza dell'applicazione dal servizio.
* Il server HTTP non invia alcuna risposta dopo il periodo di timeout. A seconda del valore di timeout impostato, più richieste di probe potrebbero non ricevere risposta prima che il probe venga contrassegnato come non in esecuzione, ovvero prima dell'invio di probe SuccessFailCount.
* Il server chiude la connessione tramite l'invio di TCP Reset.

### <a name="tcp-custom-probe"></a>Probe TCP personalizzato

I probe TCP personalizzati avviano una connessione tramite l'esecuzione di un handshake a tre livelli con la porta definita.

### <a name="what-makes-a-tcp-custom-probe-mark-an-instance-as-unhealthy"></a>Perché un probe TCP personalizzato contrassegna un'istanza come non integra

* Il server TCP non invia alcuna risposta dopo il periodo di timeout. Il probe viene contrassegnato come non in esecuzione in base alla configurazione del numero di richieste di probe non riuscite che possono rimanere senza risposta prima che il probe sia contrassegnato come non in esecuzione.
* Il probe riceve un TCP Reset dall'istanza del ruolo.

Per altre informazioni su come configurare un probe di integrità HTTP o un probe TCP, vedere [Creazione di un servizio di bilanciamento del carico pubblico in Resource Manager con PowerShell](load-balancer-get-started-internet-arm-ps.md).

## <a name="add-healthy-instances-back-into-the-load-balancer-rotation"></a>Aggiungere di nuovo le istanze integre nella rotazione del servizio di bilanciamento del carico

I probe TCP e HTTP sono considerati integri e contrassegnano come integra l'istanza del ruolo quando:

* Il servizio di bilanciamento del carico ottiene un probe positivo al primo avvio della macchina virtuale.
* Il numero di SuccessFailCount, descritto in precedenza, definisce il valore dei probe riusciti necessari per contrassegnare l'istanza del ruolo come integra. Se un'istanza del ruolo è stata rimossa, il numero di probe successivi riusciti deve essere uguale o maggiore del valore di SuccessFailCount per contrassegnare l'istanza del ruolo come in esecuzione.

> [!NOTE]
> Se l'integrità di un'istanza del ruolo varia, il servizio di bilanciamento del carico attende più a lungo prima di ripristinarne lo stato di integrità. Questo tempo di attesa aggiuntivo protegge l'utente e l'infrastruttura ed è un criterio voluto.

## <a name="use-log-analytics-for-a-load-balancer"></a>Usare Log Analytics per il servizio di bilanciamento del carico

È possibile usare [Log Analytics](load-balancer-monitor-log.md) per verificare lo stato di integrità dei probe del servizio di bilanciamento del carico e il conteggio dei probe. La registrazione può essere usata con Power BI o Azure Operational Insights per fornire statistiche sullo stato di integrità del servizio di bilanciamento del carico.
