---
title: Creare la prima app Web Java in Azure
description: Informazioni su come eseguire app Web nel servizio app mediante la distribuzione di un'app Java di base.
services: app-service\web
documentationcenter: 
author: rmcmurray
manager: routlaw
editor: 
ms.assetid: 8bacfe3e-7f0b-4394-959a-a88618cb31e1
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: quickstart
ms.date: 11/08/2017
ms.author: cephalin;robmcm
ms.custom: mvc, devcenter
ms.openlocfilehash: de72b97051daef675ceae7c7af08fb04e21cf849
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 02/01/2018
---
# <a name="create-your-first-java-web-app-in-azure"></a>Creare la prima app Web Java in Azure

Le [app Web](app-service-web-overview.md) di Azure forniscono un servizio di hosting Web ad alta scalabilità e con funzioni di auto-correzione. Questa guida introduttiva illustra come distribuire un'app Web Java nel servizio app usando [Eclipse IDE for Java EE Developers](http://www.eclipse.org/).

Al termine di questa guida introduttiva, l'applicazione visualizzata in un Web browser avrà un aspetto simile al seguente:

![App Web di esempio "Hello Azure" app Web di esempio](./media/app-service-web-get-started-java/browse-web-app-1.png)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>prerequisiti

Per completare questa guida introduttiva, installare:

* Lo strumento gratuito <a href="http://www.eclipse.org/downloads/" target="_blank">Eclipse IDE for Java EE Developers</a>. In questa guida introduttiva viene usato Eclipse Neon.
* <a href="/java/azure/eclipse/azure-toolkit-for-eclipse-installation" target="_blank">Azure Toolkit for Eclipse</a>.

## <a name="create-a-dynamic-web-project-in-eclipse"></a>Creare un progetto Web dinamico in Eclipse

In Eclipse selezionare **File** > **Nuovo** > **Dynamic Web Project** (Progetto Web dinamico).

Nella finestra di dialogo **New Dynamic Web Project** (Nuovo progetto Web dinamico) assegnare al progetto il nome **MyFirstJavaOnAzureWebApp** e selezionare **Fine**.
   
![Finestra di dialogo New Dynamic Web Project (Nuovo progetto Web dinamico)](./media/app-service-web-get-started-java/new-dynamic-web-project-dialog-box.png)

### <a name="add-a-jsp-page"></a>Aggiungere una pagina JSP

Se Esplora progetti non viene visualizzato, è necessario ripristinarlo.

![Area di lavoro Java EE per Eclipse](./media/app-service-web-get-started-java/pe.png)

In Esplora progetti espandere il progetto **MyFirstJavaOnAzureWebApp**.
Fare doppio clic su **WebContent** e quindi selezionare **Nuovo** > **JSP File** (File JSP).

![Menu per un nuovo file JSP in Esplora progetti](./media/app-service-web-get-started-java/new-jsp-file-menu.png)

Nella finestra di dialogo **New JSP File** (Nuovo file JSP):

* Assegnare al file il nome **index.jsp**.
* Selezionare **Fine**.

  ![Finestra di dialogo New JSP File (Nuovo file JSP)](./media/app-service-web-get-started-java/new-jsp-file-dialog-box-page-1.png)

Nel file index.jsp sostituire l'elemento `<body></body>` con il markup seguente:

```jsp
<body>
<h1><% out.println("Hello Azure!"); %></h1>
</body>
```

Salvare le modifiche.

## <a name="publish-the-web-app-to-azure"></a>Pubblicare l'app Web in Azure

In Esplora progetti fare clic con il pulsante destro del mouse sul progetto e quindi selezionare **Azure** > **Publish as Azure Web App** (Pubblica come app Web di Azure).

![Menu di scelta rapida Publish as Azure Web App (Pubblica come app Web di Azure)](./media/app-service-web-get-started-java/publish-as-azure-web-app-context-menu.png)

Nella finestra di dialogo **Accesso ad Azure** mantenere selezionata l'opzione **Interattivo** opzione e quindi selezionare **Accedi**.

Seguire le istruzioni di accesso.

### <a name="deploy-web-app-dialog-box"></a>Finestra di dialogo Distribuisci app Web

Dopo avere effettuato l'accesso all'account Azure, verrà visualizzata la finestra di dialogo **Distribuisci app Web**.

Selezionare **Create**.

![Finestra di dialogo Distribuisci app Web](./media/app-service-web-get-started-java/deploy-web-app-dialog-box.png)

### <a name="create-app-service-dialog-box"></a>Finestra di dialogo Crea servizio app

Viene visualizzata la finestra di dialogo **Crea servizio app** con i valori predefiniti. Il numero **170602185241** illustrato nell'immagine seguente è diverso da quello visualizzato nella finestra di dialogo reale.

![Finestra di dialogo Crea servizio app](./media/app-service-web-get-started-java/cas1.png)

Nella finestra di dialogo **Crea servizio app**:

* Lasciare invariato il nome generato per l'app Web. Il nome deve essere univoco in Azure ed è incluso nell'indirizzo URL relativo all'app Web. Se, ad esempio, il nome dell'app Web è **MyJavaWebApp**, l'URL è *myjavawebapp.azurewebsites.net*.
* Mantenere il contenitore Web predefinito.
* Selezionare una sottoscrizione di Azure.
* Nella scheda **Piano di servizio app**:

  * **Crea nuovo**: mantenere il valore predefinito, ovvero il nome del piano di servizio app.
  * **Località**: selezionare **Europa occidentale** o un'area geografica nelle vicinanze.
  * **Piano tariffario**: selezionare l'opzione gratuita. Per le funzionalità, vedere [Prezzi di Servizio app](https://azure.microsoft.com/pricing/details/app-service/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).

   ![Finestra di dialogo Crea servizio app](./media/app-service-web-get-started-java/create-app-service-dialog-box.png)

[!INCLUDE [app-service-plan](../../includes/app-service-plan.md)]

### <a name="resource-group-tab"></a>Scheda Gruppo di risorse

Selezionare la scheda **Gruppo di risorse**. Mantenere il valore predefinito generato per il gruppo di risorse.

![Scheda Gruppo di risorse](./media/app-service-web-get-started-java/create-app-service-resource-group.png)

[!INCLUDE [resource-group](../../includes/resource-group.md)]

Selezionare **Create**.

<!--
### The JDK tab

Select the **JDK** tab. Keep the default, and then select **Create**.

![Create App Service plan](./media/app-service-web-get-started-java/create-app-service-specify-jdk.png)
-->

Azure Toolkit crea l'app Web e visualizza una finestra di dialogo di avanzamento.

![Finestra di dialogo di avanzamento Crea servizio app](./media/app-service-web-get-started-java/create-app-service-progress-bar.png)

### <a name="deploy-web-app-dialog-box"></a>Finestra di dialogo Distribuisci app Web

Nella finestra di dialogo **Distribuisci app Web** selezionare **Deploy to root** (Distribuisci nella radice). Se si ha un servizio app all'indirizzo *wingtiptoys.azurewebsites.net* e non si esegue la distribuzione nella radice, l'app Web denominata **MyFirstJavaOnAzureWebApp** verrà distribuita in *wingtiptoys.azurewebsites.net/MyFirstJavaOnAzureWebApp*.

![Finestra di dialogo Distribuisci app Web](./media/app-service-web-get-started-java/deploy-web-app-to-root.png)

Nella finestra di dialogo vengono visualizzate le selezioni del contenitore Web, di Azure e di JDK.

Selezionare **Distribuisci** per pubblicare l'app Web in Azure.

Al termine del processo di pubblicazione, selezionare il collegamento **Pubblicato** nella finestra di dialogo **Log attività di Azure**.

![Finestra di dialogo Log attività di Azure](./media/app-service-web-get-started-java/aal.png)

Congratulazioni! L'app Web è stata distribuita in Azure. 

![App Web di esempio "Hello Azure" app Web di esempio](./media/app-service-web-get-started-java/browse-web-app-1.png)

## <a name="update-the-web-app"></a>Aggiornare l'app Web

Modificare il codice JSP di esempio in un messaggio diverso.

```jsp
<body>
<h1><% out.println("Hello again Azure!"); %></h1>
</body>
```

Salvare le modifiche.

In Esplora progetti fare clic con il pulsante destro del mouse sul progetto e quindi selezionare **Azure** > **Publish as Azure Web App** (Pubblica come app Web di Azure).

Viene visualizzata la finestra di dialogo **Distribuisci app Web** in cui viene illustrato il servizio app creato in precedenza. 

> [!NOTE] 
> Selezionare **Deploy to root** (Distribuisci nella radice) ogni volta che si esegue una pubblicazione. 
> 

Selezionare l'app Web e fare clic su **Distribuisci** per pubblicare le modifiche.

Quando viene visualizzato il collegamento **Pubblicazione**, selezionarlo per passare all'app Web e visualizzare le modifiche.

## <a name="manage-the-web-app"></a>Gestire l'app Web

Accedere al <a href="https://portal.azure.com" target="_blank">portale di Azure</a> per visualizzare l'app Web creata.

Nel menu di sinistra selezionare **Gruppi di risorse**.

![Accesso ai gruppi di risorse nel portale](media/app-service-web-get-started-java/rg.png)

Selezionare il gruppo di risorse. Nella pagina vengono visualizzate le risorse create in questa guida introduttiva.

![Gruppo di risorse myResourceGroup](media/app-service-web-get-started-java/rg2.png)

Selezionare l'app Web (**webapp 170602193915** nell'immagine precedente).

Viene visualizzata la pagina **Panoramica**, che offre una visualizzazione dello stato dell'app. Qui è possibile eseguire attività di gestione di base come l'esplorazione, l'arresto, l'avvio, il riavvio e l'eliminazione dell'app. Le schede sul lato sinistro della pagina mostrano le diverse configurazioni che è possibile aprire. 

![Pagina del servizio app nel portale di Azure](media/app-service-web-get-started-java/web-app-blade.png)

[!INCLUDE [clean-up-section-portal-web-app](../../includes/clean-up-section-portal-web-app.md)]

## <a name="next-steps"></a>Passaggi successivi

> [!div class="nextstepaction"]
> [Eseguire il mapping di un dominio personalizzato](app-service-web-tutorial-custom-domain.md)
