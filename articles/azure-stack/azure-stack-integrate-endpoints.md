---
title: 'Azure dello Stack di integrazione di datacenter: pubblicare endpoint'
description: Viene descritto come pubblicare endpoint dello Stack di Azure nel Data Center
services: azure-stack
author: jeffgilb
ms.service: azure-stack
ms.topic: article
ms.date: 02/16/2018
ms.author: jeffgilb
ms.reviewer: wamota
keywords: 
ms.openlocfilehash: 8af533147f3cc12f2334a43e7b672c69d0d25802
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/21/2018
---
# <a name="azure-stack-datacenter-integration---publish-endpoints"></a>Azure dello Stack di integrazione di datacenter: pubblicare endpoint
Stack di Azure consente di impostare vari virtuali indirizzi IP (VIP) per i ruoli di infrastruttura. Questi indirizzi VIP sono allocati dal pool di indirizzi IP pubblici. Ogni indirizzo IP virtuale è protetta con un elenco di controllo di accesso (ACL) nel livello di rete definita dal software. Gli ACL vengono usati anche tra i commutatori fisici (tori e BMC) per un'ulteriore protezione avanzata della soluzione. Una voce DNS viene creata per ogni endpoint nella zona DNS esterna che è stata specificata in fase di distribuzione.


Il diagramma dell'architettura seguente illustra i vari livelli della rete e gli ACL:

![Diagramma dell'architettura](media/azure-stack-integrate-endpoints/Integrate-Endpoints-01.png)

## <a name="ports-and-protocols-inbound"></a>Porte e protocolli (in ingresso)

Di seguito sono elencate gli indirizzi VIP infrastruttura necessari per una pubblicazione di endpoint Azure Stack a reti esterne. L'elenco Mostra ogni endpoint, la porta richiesta e protocollo. Gli endpoint necessari per i provider di risorse aggiuntive, come il provider di risorse SQL e altri, sono vedere la documentazione sulla distribuzione di provider di risorse specifico.

Gli indirizzi VIP infrastruttura interna che non sono elencate perché non sono necessarie per la pubblicazione dello Stack di Azure.

> [!NOTE]
> Gli indirizzi VIP utente sono dinamici, definiti dagli utenti stessi senza il controllo dall'operatore dello Stack di Azure.


|Endpoint (VIP)|Un record host DNS|Protocollo|Porte|
|---------|---------|---------|---------|
|AD FS|Adfs.*&lt;region>.&lt;fqdn>*|HTTPS|443|
|Portale (amministratore)|Adminportal.*&lt;region>.&lt;fqdn>*|HTTPS|443<br>12495<br>12499<br>12646<br>12647<br>12648<br>12649<br>12650<br>13001<br>13003<br>13010<br>13011<br>13020<br>13021<br>13026<br>30015|
|Gestione risorse di Azure (amministratore)|Adminmanagement.*&lt;region>.&lt;fqdn>*|HTTPS|443<br>30024|
|Portale (utente)|Portal.*&lt;region>.&lt;fqdn>*|HTTPS|443<br>12495<br>12649<br>13001<br>13010<br>13011<br>13020<br>13021<br>30015<br>13003|
|Gestione risorse di Azure (utente)|Gestione.  *&lt;area >.&lt; nome di dominio completo >*|HTTPS|443<br>30024|
|Grafico|Graph.*&lt;region>.&lt;fqdn>*|HTTPS|443|
|Elenco di revoche di certificati|Crl.*&lt;region>.&lt;fqdn>*|HTTP|80|
|DNS|&#42;.*&lt;region>.&lt;fqdn>*|TCP / UDP|53|
|Chiave dell'insieme di credenziali (utente)|&#42;.vault.*&lt;region>.&lt;fqdn>*|HTTPS|443|
|Chiave dell'insieme di credenziali (amministratore)|&#42;.adminvault.*&lt;region>.&lt;fqdn>*|HTTPS|443|
|Coda di archiviazione|&#42;.queue.*&lt;region>.&lt;fqdn>*|HTTP<br>HTTPS|80<br>443|
|Tabella di archiviazione|&#42;.table.*&lt;region>.&lt;fqdn>*|HTTP<br>HTTPS|80<br>443|
|Archiviazione BLOB|&#42;.blob.*&lt;region>.&lt;fqdn>*|HTTP<br>HTTPS|80<br>443|
|Provider di risorse SQL|sqladapter.dbadapter.*&lt;region>.&lt;fqdn>*|HTTPS|44300-44304|
|Provider di risorse MySQL|mysqladapter.dbadapter.*&lt;region>.&lt;fqdn>*|HTTPS|44300-44304|
|Servizio app|&#42;.appservice.*&lt;region>.&lt;fqdn>*|TCP|80 (HTTP)<br>443 (HTTPS)<br>8172 (MSDeploy)|
|  |&#42;.scm.appservice.*&lt;region>.&lt;fqdn>*|TCP|443 (HTTPS)|
|  |api.appservice.*&lt;region>.&lt;fqdn>*|TCP|443 (HTTPS)<br>44300 (Gestione risorse di azure)|
|  |ftp.appservice.*&lt;region>.&lt;fqdn>*|TCP, UDP|21, 1021, 10001-101000 (FTP)<br>990 (FTPS)|

## <a name="ports-and-urls-outbound"></a>Porte e gli URL (in uscita)

Stack di Azure supporta solo i server proxy trasparente. In una distribuzione in cui un uplink proxy trasparente per un server proxy tradizionali, è necessario consentire le seguenti porte e gli URL per le comunicazioni in uscita:


|Scopo|URL|Protocollo|Porte|
|---------|---------|---------|---------|
|Identità|login.windows.net<br>login.microsoftonline.com<br>graph.windows.net|HTTP<br>HTTPS|80<br>443|
|Diffusione di Marketplace|https://management.azure.com<br>https://&#42;.blob.core.windows.net<br>https://*.azureedge.net<br>https://&#42;.microsoftazurestack.com|HTTPS|443|
|Patch e aggiornamento|https://&#42;.azureedge.net|HTTPS|443|
|Registrazione|https://management.azure.com|HTTPS|443|
|Uso|https://&#42;.microsoftazurestack.com<br>https://*.trafficmanager.com|HTTPS|443|


## <a name="next-steps"></a>Passaggi successivi
[Requisiti di infrastruttura a chiave pubblica dello Stack di Azure](azure-stack-pki-certs.md)