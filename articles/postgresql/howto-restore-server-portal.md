---
title: Come ripristinare un server nel Database di Azure per PostgreSQL | Microsoft Docs
description: In questo articolo viene descritta la procedura per ripristinare un server nel Database di Azure per PostgreSQL usando il portale di Azure.
services: postgresql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 11/03/2017
ms.openlocfilehash: 903fd2ff446e1963ab5cfcec745766188b74efcf
ms.sourcegitcommit: 38c9176c0c967dd641d3a87d1f9ae53636cf8260
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/06/2017
---
# <a name="how-to-backup-and-restore-a-server-in-azure-database-for-postgresql-using-the-azure-portal"></a>Come eseguire la procedura di backup e ripristino di un server in Database di Azure per PostgreSQL usando il portale di Azure

## <a name="backup-happens-automatically"></a>Il backup viene eseguito automaticamente
Quando si usa Database di Azure per PostgreSQL, il servizio di database esegue automaticamente il backup del server ogni 5 minuti. 

La disponibilità dei backup è di 7 giorni per il livello Basic e 35 giorni per il livello Standard. Per altre informazioni, vedere [Livelli di servizio del Database di Azure per PostgreSQL](concepts-service-tiers.md)

L'uso di questa funzionalità di backup automatico permette di ripristinare il server e tutti i suoi database in un nuovo server a un precedente punto temporizzato.

## <a name="restore-in-the-azure-portal"></a>Ripristino nel portale di Azure
Il Database di Azure per PostgreSQL consente di ripristinare il server a un punto temporizzato e in una nuova copia del server. È possibile usare questo nuovo server per ripristinare i dati. 

Ad esempio, se oggi una tabella è stata accidentalmente eliminata a mezzogiorno, è possibile eseguire il ripristino a un'ora prima di mezzogiorno per recuperare la tabella e i dati mancanti dalla nuova copia del server.

La procedura seguente consente di ripristinare il server di esempio ad un punto temporizzato.
1. Accedere al [portale di Azure](https://portal.azure.com/)
2. Individuare il Database di Azure per il server PostgreSQL. Nel portale di Azure fare clic su **Tutte le risorse** nel menu a sinistra e digitare il nome del server, ad esempio **mypgserver-20170401**, per cercare il server esistente. Fare clic sul nome del server elencato nei risultati della ricerca. Si apre la pagina **Panoramica** relativa al server, con le opzioni per un'ulteriore configurazione.

   ![Portale di Azure - Effettuare la ricerca del server](media/postgresql-howto-restore-server-portal/1-locate.png)

3. Nella barra degli strumenti della pagina di panoramica del server fare clic su **Ripristina**. Verrà visualizzata la pagina Ripristina.

   ![Database di Azure per PostgreSQL - Panoramica - Pulsante Ripristino](./media/postgresql-howto-restore-server-portal/2_server.png)

4. Compilare il modulo Ripristina con le informazioni obbligatorie:

   ![Database di Azure per PostgreSQL - Informazioni di ripristino ](./media/postgresql-howto-restore-server-portal/3_restore.png)
  - **Punto di ripristino**: selezionare un punto nel tempo precedente la modifica del server.
  - **Server di destinazione**: fornire un nuovo nome di server in cui si vuole eseguire il ripristino.
  - **Percorso**: non è possibile selezionare l'area. Per impostazione predefinita è uguale al server di origine.
  - **Piano tariffario**: non è possibile modificare questo valore quando si ripristina un server. È uguale al server di origine. 

5. Fare clic su **OK** per ripristinare il server al punto di ripristino temporizzato. 

6. Al termine del ripristino, individuare il nuovo server creato per verificare che il ripristino dei dati sia avvenuto come previsto.

## <a name="next-steps"></a>Passaggi successivi
- [Raccolte connessioni per il Database di Azure per PostgreSQL](concepts-connection-libraries.md)
