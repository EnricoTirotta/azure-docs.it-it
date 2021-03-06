---
title: 'Esercitazione: Integrazione di Azure Active Directory con Zoom | Microsoft Docs'
description: Informazioni su come configurare l'accesso Single Sign-On tra Azure Active Directory e Zoom.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 0ebdab6c-83a8-4737-a86a-974f37269c31
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/28/2017
ms.author: jeedes
ms.openlocfilehash: 5a6d9ea9de1035bf9c84cf3c451cc1121f04a82a
ms.sourcegitcommit: 3cdc82a5561abe564c318bd12986df63fc980a5a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/05/2018
---
# <a name="tutorial-azure-active-directory-integration-with-zoom"></a>Esercitazione: Integrazione di Azure Active Directory con Zoom

Questa esercitazione descrive come integrare Zoom con Azure Active Directory (Azure AD).

L'integrazione di Zoom con Azure AD offre i vantaggi seguenti:

- È possibile controllare in Azure AD chi può accedere a Zoom.
- È possibile abilitare gli utenti per l'accesso automatico a Zoom (Single Sign-On) con gli account Azure AD personali.
- È possibile gestire gli account in un'unica posizione centrale: il portale di Azure.

Per altre informazioni sull'integrazione di app SaaS con Azure AD, vedere [Informazioni sull'accesso alle applicazioni e Single Sign-On con Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Prerequisiti

Per configurare l'integrazione di Azure AD con Zoom, sono necessari gli elementi seguenti:

- Sottoscrizione di Azure AD
- Sottoscrizione di Zoom abilitata per l'accesso Single Sign-On

> [!NOTE]
> Non è consigliabile usare un ambiente di produzione per testare i passaggi di questa esercitazione.

A questo scopo, è consigliabile seguire le indicazioni seguenti:

- Non usare l'ambiente di produzione a meno che non sia necessario.
- Se non è disponibile un ambiente di valutazione di Azure AD, è possibile [ottenere una versione di valutazione di un mese](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrizione dello scenario
In questa esercitazione viene eseguito il test dell'accesso Single Sign-On di Azure AD in un ambiente di test. Lo scenario descritto in questa esercitazione prevede le due fasi fondamentali seguenti:

1. Aggiunta di Zoom dalla raccolta
2. Configurazione e test dell'accesso Single Sign-On di Azure AD

## <a name="adding-zoom-from-the-gallery"></a>Aggiunta di Zoom dalla raccolta
Per configurare l'integrazione di Zoom in Azure AD, è necessario aggiungere Zoom dalla raccolta all'elenco di app SaaS gestite.

**Per aggiungere Zoom dalla raccolta, seguire questa procedura:**

1. Nel **[portale di Azure](https://portal.azure.com)** fare clic sull'icona di **Azure Active Directory** nel riquadro di spostamento sinistro. 

    ![Pulsante Azure Active Directory][1]

2. Passare ad **Applicazioni aziendali**. Andare quindi a **Tutte le applicazioni**.

    ![Pannello Applicazioni aziendali][2]
    
3. Fare clic sul pulsante **Nuova applicazione** nella parte superiore della finestra di dialogo per aggiungere una nuova applicazione.

    ![Pulsante Nuova applicazione][3]

4. Nella casella di ricerca digitare **Zoom**, selezionare **Zoom** nel pannello dei risultati e quindi fare clic sul pulsante **Aggiungi** per aggiungere l'applicazione.

    ![Zoom nell'elenco risultati](./media/active-directory-saas-zoom-tutorial/tutorial_zoom_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurare e testare l'accesso Single Sign-On di Azure AD

In questa sezione viene configurato e testato l'accesso Single Sign-On di Azure AD con Zoom usando un utente di test di nome "Britta Simon".

Per il funzionamento dell'accesso Single Sign-On, Azure AD deve conoscere l'utente controparte di Zoom corrispondente a un utente di Azure AD. In altre parole, deve essere stabilita una relazione di collegamento tra un utente di Azure AD e l'utente correlato in Zoom.

Per stabilire la relazione di collegamento, in Zoom assegnare il valore del **nome utente** in Azure AD come valore di **Username** (Nome utente).

Per configurare e testare l'accesso Single Sign-On di Azure AD con Zoom, è necessario completare le procedure di base seguenti:

1. **[Configurare l'accesso Single Sign-On di Azure AD](#configure-azure-ad-single-sign-on)**: per consentire agli utenti di usare questa funzionalità.
2. **[Creare un utente di test di Azure AD](#create-an-azure-ad-test-user)**: per testare l'accesso Single Sign-On di Azure AD con l'utente Britta Simon.
3. **[Creare un utente di test di Zoom](#create-a-zoom-test-user)**: per avere una controparte di Britta Simon in Zoom collegata alla rappresentazione dell'utente in Azure AD.
4. **[Assegnare l'utente test di Azure AD](#assign-the-azure-ad-test-user)**: per abilitare Britta Simon all'uso dell'accesso Single Sign-On di Azure AD.
5. **[Testare l'accesso Single Sign-On](#test-single-sign-on)** per verificare se la configurazione funziona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurare l'accesso Single Sign-On di Azure AD

In questa sezione viene abilitato l'accesso Single Sign-On di Azure AD nel portale di Azure e viene configurato l'accesso Single Sign-On nell'applicazione Zoom.

**Per configurare l'accesso Single Sign-On di Azure AD con Zoom, seguire questa procedura:**

1. Nella pagina di integrazione dell'applicazione **Zoom** del portale di Azure fare clic su **Single Sign-On**.

    ![Collegamento Configura accesso Single Sign-On][4]

2. Nella finestra di dialogo **Single Sign-On** selezionare **Accesso basato su SAML** per **Modalità** per abilitare l'accesso Single Sign-On.
 
    ![Finestra di dialogo Single Sign-On](./media/active-directory-saas-zoom-tutorial/tutorial_zoom_samlbase.png)

3. Nella sezione **URL e dominio Zoom** seguire questa procedura:

    ![Informazioni su URL e dominio per l'accesso Single Sign-On di Zoom](./media/active-directory-saas-zoom-tutorial/tutorial_zoom_url.png)

    a. Nella casella di testo **URL di accesso** digitare l'URL usando il modello seguente: `https://<companyname>.zoom.us`

    b. Nella casella di testo **Identificatore** digitare l'URL adottando il modello seguente: `<companyname>.zoom.us`

    > [!NOTE] 
    > Poiché questi non sono i valori reali, Aggiornare questi valori con l'identificatore e l'URL di accesso effettivi. Per ottenere questi valori, contattare il [team di supporto clienti di Zoom](https://support.zoom.us/hc).

4. L'applicazione dello Zoom prevede le asserzioni SAML in un formato specifico, che è necessario aggiungere mapping di attributi personalizzati alla configurazione degli attributi del token SAML. Configurare le attestazioni seguenti per questa applicazione. È possibile gestire i valori di questi attributi dalla sezione "**Attributi utente**" nella pagina di integrazione dell'applicazione. 

    ![Configure Single Sign-On](./media/active-directory-saas-Zoom-tutorial/tutorial_attribute.png)

5. Nella sezione **Attributi utente** della finestra di dialogo **Single Sign-On** configurare l'attributo del token SAML come indicato nell'immagine precedente ed eseguire i passaggi descritti di seguito:
    
    | Nome attributo | Valore attributo | Valore di Namespace |
    | ------------------- | -----------|--------- |    
    | Indirizzo di posta elettronica | user.mail | `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/mail`|
    | Nome | user.givenname | `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname`|
    | Cognome | user.surname | `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname `|
    | Numero di telefono | User. telephoneNumber | `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/phone`|
    | department | user.department | `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/department`|

    a. Fare clic su **Aggiungi attributo** per aprire la finestra di dialogo **Aggiungi attributo**.

    ![Configure Single Sign-On](./media/active-directory-saas-Zoom-tutorial/tutorial_attribute_04.png)

    ![Configure Single Sign-On](./media/active-directory-saas-Zoom-tutorial/tutorial_attribute_05.png)

    b. Nella casella di testo **Nome** digitare il nome dell'attributo indicato per la riga.

    c. Nell'elenco **Valore** digitare il valore dell'attributo indicato per la riga.

    d. Nel **Namespace** casella di testo, digitare il valore dello spazio dei nomi indicato per la riga.
    
    e. Fare clic su **OK**. 
 
6. Nella sezione **Certificato di firma SAML** fare clic su **Certificato (Base64)** e quindi salvare il file del certificato nel computer.

    ![Collegamento di download del certificato](./media/active-directory-saas-zoom-tutorial/tutorial_zoom_certificate.png)

7. Fare clic sul pulsante **Salva** .

    ![Pulsante Salva per la configurazione dell'accesso Single Sign-On](./media/active-directory-saas-zoom-tutorial/tutorial_general_400.png)

8. Nella sezione **Configurazione di Zoom** fare clic su **Configura Zoom** per aprire la finestra **Configura accesso**. Copiare l'**URL di disconnessione, l'ID di entità SAML e l'URL del servizio Single Sign-On SAML** dalla sezione **Riferimento rapido.**

    ![Configurazione di Zoom](./media/active-directory-saas-zoom-tutorial/tutorial_zoom_configure.png)

9. In un'altra finestra del Web browser accedere al sito aziendale di Zoom come amministratore.

10. Fare clic sulla scheda **Single Sign-On** .
   
    ![Scheda Single Sign-On](./media/active-directory-saas-zoom-tutorial/IC784700.png "Single Sign-On")

11. Fare clic sulla scheda **Security Control** (Controllo di sicurezza) e quindi passare alle impostazioni **Single Sign-On**.

12. Nella sezione Single Sign-On seguire questa procedura:
   
    ![Sezione Single Sign-On](./media/active-directory-saas-zoom-tutorial/IC784701.png "Single Sign-On")
   
    a. Nella casella di testo **URL pagina di accesso** incollare il valore dell'**URL del servizio Single Sign-On SAML** copiato dal portale di Azure.
   
    b. Nella casella di testo **URL pagina di disconnessione** incollare il valore dell'**URL di disconnessione** copiato dal portale di Azure.
     
    c. Aprire il certificato con codifica base 64 nel blocco note, copiarne il contenuto negli appunti e quindi incollarlo nella casella di testo **Identity provider certificate** .

    d. Nella casella di testo **Autorità emittente** incollare il valore dell'**ID di entità SAML** copiato dal portale di Azure. 

    e. Fare clic su **Save**.

    > [!NOTE] 
    > Per altre informazioni, vedere la documentazione di zoom [https://zoomus.zendesk.com/hc/en-us/articles/115005887566](https://zoomus.zendesk.com/hc/en-us/articles/115005887566)

> [!TIP]
> Un riepilogo delle istruzioni è disponibile all'interno del [portale di Azure](https://portal.azure.com) durante la configurazione dell'app.  Dopo aver aggiunto l'app dalla sezione **Active Directory > Applicazioni aziendali** è sufficiente fare clic sulla scheda **Single Sign-On** e accedere alla documentazione incorporata tramite la sezione **Configurazione** nella parte inferiore. Altre informazioni sulla funzione di documentazione incorporata sono disponibili in [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985) (Documentazione incorporata di Azure AD).
> 

### <a name="create-an-azure-ad-test-user"></a>Creare un utente test di Azure AD

Questa sezione descrive come creare un utente test denominato Britta Simon nel portale di Azure.

   ![Creare un utente test di Azure AD][100]

**Per creare un utente test in Azure AD, eseguire la procedura seguente:**

1. Nel portale di Azure fare clic sul pulsante **Azure Active Directory** nel riquadro sinistro.

    ![Pulsante Azure Active Directory](./media/active-directory-saas-zoom-tutorial/create_aaduser_01.png)

2. Per visualizzare l'elenco di utenti, passare a **Utenti e gruppi** e quindi fare clic su **Tutti gli utenti**.

    ![Collegamenti "Utenti e gruppi" e "Tutti gli utenti"](./media/active-directory-saas-zoom-tutorial/create_aaduser_02.png)

3. Per aprire la finestra di dialogo **Utente** fare clic su **Aggiungi** nella parte superiore della finestra di dialogo **Tutti gli utenti**.

    ![Pulsante Aggiungi](./media/active-directory-saas-zoom-tutorial/create_aaduser_03.png)

4. Nella finestra di dialogo **Utente** seguire questa procedura:

    ![Finestra di dialogo Utente](./media/active-directory-saas-zoom-tutorial/create_aaduser_04.png)

    a. Nella casella **Nome** digitare **BrittaSimon**.

    b. Nella casella **Nome utente** digitare l'indirizzo di posta elettronica dell'utente Britta Simon.

    c. Selezionare la casella di controllo **Mostra password** e quindi prendere nota del valore visualizzato nella casella **Password**.

    d. Fare clic su **Crea**.
 
### <a name="create-a-zoom-test-user"></a>Creare un utente di test di Zoom

Per consentire agli utenti di Azure AD di accedere a Zoom, è necessario effettuarne il provisioning in Zoom. Nel caso di Zoom, il provisioning è un'attività manuale.

### <a name="to-provision-a-user-account-perform-the-following-steps"></a>Per eseguire il provisioning di un account utente, seguire questa procedura:

1. Accedere al sito aziendale di **Zoom** come amministratore.
 
2. Fare clic sulla scheda **Gestione account** e quindi su **Gestione utente**.

3. Nella sezione Gestione utente fare clic su **Aggiungi utenti**.
   
    ![Gestione degli utenti](./media/active-directory-saas-zoom-tutorial/IC784703.png "Gestione degli utenti")

4. Nella pagina **Aggiungi utenti** seguire la procedura seguente:
   
    ![Aggiungere utenti](./media/active-directory-saas-zoom-tutorial/IC784704.png "Aggiungere utenti")
   
    a. In **Tipo utente** selezionare **Basic**.

    b. Nella casella di testo **Emails** (Messaggi di posta elettronica) digitare l'indirizzo e-mail di un account Azure AD di cui si vuole effettuare il provisioning.

    c. Fare clic su **Aggiungi**.

> [!NOTE]
> È possibile usare qualsiasi altro strumento o API di creazione di account utente di Zoom per effettuare il provisioning degli account utente di Azure Active Directory.

### <a name="assign-the-azure-ad-test-user"></a>Assegnare l'utente test di Azure AD

In questa sezione Britta Simon viene abilitata per l'uso dell'accesso Single Sign-On di Azure concedendole l'accesso a Zoom.

![Assegnare il ruolo utente][200] 

**Per assegnare Britta Simon a Zoom, seguire questa procedura:**

1. Nel portale di Azure aprire la visualizzazione delle applicazioni e quindi la visualizzazione delle directory e passare ad **Applicazioni aziendali**, quindi fare clic su **Tutte le applicazioni**.

    ![Assegna utente][201] 

2. Nell'elenco delle applicazioni selezionare **Zoom**.

    ![Collegamento di Zoom nell'elenco delle applicazioni](./media/active-directory-saas-zoom-tutorial/tutorial_zoom_app.png)  

3. Scegliere **Utenti e gruppi** dal menu a sinistra.

    ![Collegamento "Utenti e gruppi"][202]

4. Fare clic sul pulsante **Aggiungi**. Selezionare quindi **Utenti e gruppi** nella finestra di dialogo **Aggiungi assegnazione**.

    ![Riquadro Aggiungi assegnazione][203]

5. Nella finestra di dialogo **Utenti e gruppi** selezionare **Britta Simon** nell'elenco Utenti.

6. Fare clic sul pulsante **Seleziona** nella finestra di dialogo **Utenti e gruppi**.

7. Fare clic sul pulsante **Assegna** nella finestra di dialogo **Aggiungi assegnazione**.
    
### <a name="test-single-sign-on"></a>Testare l'accesso Single Sign-On

Questa sezione descrive come testare la configurazione dell'accesso Single Sign-On di Azure AD usando il pannello di accesso.

Quando si fa clic sul riquadro Zoom nel pannello di accesso, si dovrebbe accedere automaticamente all'applicazione Zoom.

## <a name="additional-resources"></a>Risorse aggiuntive

* [Elenco di esercitazioni sulla procedura di integrazione delle app SaaS con Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Informazioni sull'accesso alle applicazioni e Single Sign-On con Azure Active Directory](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-zoom-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-zoom-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-zoom-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-zoom-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-zoom-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-zoom-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-zoom-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-zoom-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-zoom-tutorial/tutorial_general_203.png

