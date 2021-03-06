---
title: 'Esercitazione: Integrazione di Azure Active Directory con ClickTime | Documentazione Microsoft'
description: Informazioni su come configurare l'accesso Single Sign-On tra Azure Active Directory e ClickTime.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: d437b5ab-4d71-4c13-96d0-79018cebbbd4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/10/2017
ms.author: jeedes
ms.openlocfilehash: f19e1968c736cb21a2a80b9807fa86461e05ee42
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-clicktime"></a>Esercitazione: Integrazione di Azure Active Directory con ClickTime

Questa esercitazione descrive come integrare ClickTime con Azure Active Directory (Azure AD).

L'integrazione di ClickTime con Azure AD offre i vantaggi seguenti:

- È possibile controllare in Azure AD chi può accedere a ClickTime
- È possibile abilitare gli utenti per l'accesso automatico a ClickTime (Single Sign-On) con i propri account Azure AD
- È possibile gestire gli account in un'unica posizione centrale: il portale di Azure.

Per altre informazioni sull'integrazione di app SaaS con Azure AD, vedere [Informazioni sull'accesso alle applicazioni e Single Sign-On con Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>prerequisiti

Per configurare l'integrazione di Azure AD con ClickTime, sono necessari gli elementi seguenti:

- Sottoscrizione di Azure AD
- Sottoscrizione di ClickTime abilitata per l'accesso Single Sign-On.

> [!NOTE]
> Non è consigliabile usare un ambiente di produzione per testare i passaggi di questa esercitazione.

A questo scopo, è consigliabile seguire le indicazioni seguenti:

- Non usare l'ambiente di produzione a meno che non sia necessario.
- Se non è disponibile un ambiente di valutazione di Azure AD, è possibile [ottenere una versione di valutazione di un mese](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrizione dello scenario
In questa esercitazione viene eseguito il test dell'accesso Single Sign-On di Azure AD in un ambiente di test. Lo scenario descritto in questa esercitazione prevede le due fasi fondamentali seguenti:

1. Aggiunta di ClickTime dalla raccolta
2. Configurazione e test dell'accesso Single Sign-On di Azure AD

## <a name="adding-clicktime-from-the-gallery"></a>Aggiunta di ClickTime dalla raccolta
Per configurare l'integrazione di ClickTime in Azure AD, è necessario aggiungere ClickTime dalla raccolta al proprio elenco delle app SaaS gestite.

**Per aggiungere ClickTime dalla raccolta, seguire questa procedura:**

1. Nel **[portale di Azure](https://portal.azure.com)** fare clic sull'icona di **Azure Active Directory** nel riquadro di spostamento sinistro. 

    ![Pulsante Azure Active Directory][1]

2. Passare ad **Applicazioni aziendali**. Andare quindi a **Tutte le applicazioni**.

    ![Pannello Applicazioni aziendali][2]
    
3. Fare clic sul pulsante **Nuova applicazione** nella parte superiore della finestra di dialogo per aggiungere una nuova applicazione.

    ![Pulsante Nuova applicazione][3]

4. Nella casella di ricerca digitare **ClickTime**, selezionare **ClickTime** dal pannello dei risultati e quindi fare clic sul pulsante **Aggiungi** per aggiungere l'applicazione.

    ![ClickTime nell'elenco risultati](./media/active-directory-saas-clicktime-tutorial/tutorial_clicktime_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurare e testare l'accesso Single Sign-On di Azure AD

In questa sezione viene configurato e testato l'accesso Single Sign-On di Azure AD con ClickTime in base a un utente test di nome "Britta Simon".

Per il funzionamento dell'accesso Single Sign-On, Azure AD deve conoscere qual è l'utente di ClickTime che corrisponde a un utente di Azure AD. In altre parole, deve essere stabilita una relazione di collegamento tra un utente di Azure AD e l'utente correlato in ClickTime.

Per stabilire la relazione di collegamento, in ClickTime assegnare il valore di **nome utente** in Azure AD come valore di **Username** (Nome utente).

Per configurare e testare l'accesso Single Sign-On di Azure AD con ClickTime, è necessario completare i blocchi predefiniti seguenti:

1. **[Configurare l'accesso Single Sign-On di Azure AD](#configure-azure-ad-single-sign-on)**: per consentire agli utenti di usare questa funzionalità.
2. **[Creare un utente di test di Azure AD](#create-an-azure-ad-test-user)**: per testare l'accesso Single Sign-On di Azure AD con l'utente Britta Simon.
3. **[Creare un utente test di ClickTime](#create-a-clicktime-test-user)**: per avere una controparte di Britta Simon in ClickTime collegata alla relativa rappresentazione in Azure AD.
4. **[Assegnare l'utente test di Azure AD](#assign-the-azure-ad-test-user)**: per abilitare Britta Simon all'uso dell'accesso Single Sign-On di Azure AD.
5. **[Testare l'accesso Single Sign-On](#test-single-sign-on)** per verificare se la configurazione funziona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurare l'accesso Single Sign-On di Azure AD

In questa sezione viene abilitato l'accesso Single Sign-On di Azure AD nel portale di Azure e viene configurato l'accesso Single Sign-On nell'applicazione ClickTime.

**Per configurare Single Sign-On di Azure AD con ClickTime, seguire questa procedura:**

1. Nella pagina di integrazione dell'applicazione **ClickTime** del portale di Azure fare clic su **Single Sign-On**.

    ![Collegamento Configura accesso Single Sign-On][4]

2. Nella finestra di dialogo **Single Sign-On** selezionare **Accesso basato su SAML** per **Modalità** per abilitare l'accesso Single Sign-On.
 
    ![Finestra di dialogo Single Sign-On](./media/active-directory-saas-clicktime-tutorial/tutorial_clicktime_samlbase.png)

3. Nella sezione **URL e dominio ClickTime** seguire questa procedura:

    ![Informazioni su URL e dominio per l'accesso Single Sign-On di ClickTime](./media/active-directory-saas-clicktime-tutorial/tutorial_clicktime_url.png)

    a. Nella casella di testo **Identificatore** digitare un URL come indicato di seguito: `https://app.clicktime.com/sp/`
    
    b. Nella casella di testo **URL di risposta** digitare l'URL usando i modelli seguenti: 

    | |
    |--|
    | `https://app.clicktime.com/Login/` |
    | `https://app.clicktime.com/App/Login/Consume.aspx` |

4. Nella sezione **Certificato di firma SAML** fare clic su **Certificato (Base64)** e quindi salvare il file del certificato nel computer.

    ![Collegamento di download del certificato](./media/active-directory-saas-clicktime-tutorial/tutorial_clicktime_certificate.png) 

5. Fare clic sul pulsante **Salva** .

    ![Pulsante Salva per la configurazione dell'accesso Single Sign-On](./media/active-directory-saas-clicktime-tutorial/tutorial_general_400.png)

6. Nella sezione **Configurazione di ClickTime** fare clic su **Configura ClickTime** per aprire la finestra **Configura accesso**. Copiare l'**URL servizio Single Sign-On SAML** dalla **sezione Riferimento rapido.**

    ![Configurazione di ClickTime](./media/active-directory-saas-clicktime-tutorial/tutorial_clicktime_configure.png) 

7. In un'altra finestra del Web browser accedere al sito aziendale di ClickTime come amministratore.

8. Nella barra degli strumenti in alto fare clic su **Preferences** (Preferenze) e quindi su **Security Settings** (Impostazioni di sicurezza).

9. Nella sezione di configurazione **Preferenza Single Sign-On** seguire questa procedura:
   
    ![Impostazioni di sicurezza](./media/active-directory-saas-clicktime-tutorial/tic777280.png "Impostazioni di sicurezza")
   
    a.  Selezionare **Allow** sign-in using Single Sign-On (SSO) **Azure AD** (Consenti l'accesso Single Sign-On (SSO) con Azure AD).
   
    b. Nella casella di testo **Identity Provider Endpoint** (Endpoint del provider di identità) incollare il valore di **SAML Single Sign-On Service URL** (URL servizio Single Sign-On SAML) copiato dal portale di Azure.
   
    c.  Aprire il **certificato con codifica Base 64** scaricato dal portale di Azure in **Blocco note**, copiarne il contenuto e quindi incollarlo nella casella di testo **X.509 Certificate** (Certificato X.509).
   
    d.  Fare clic su **Save**.

> [!TIP]
> Un riepilogo delle istruzioni è disponibile all'interno del [portale di Azure](https://portal.azure.com) durante la configurazione dell'app.  Dopo aver aggiunto l'app dalla sezione **Active Directory > Applicazioni aziendali** è sufficiente fare clic sulla scheda **Single Sign-On** e accedere alla documentazione incorporata tramite la sezione **Configurazione** nella parte inferiore. Altre informazioni sulla funzione di documentazione incorporata sono disponibili in [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985) (Documentazione incorporata di Azure AD).

### <a name="create-an-azure-ad-test-user"></a>Creare un utente test di Azure AD
Questa sezione descrive come creare un utente test denominato Britta Simon nel portale di Azure.

![Creare un utente test di Azure AD][100]

**Per creare un utente test in Azure AD, eseguire la procedura seguente:**

1. Nel portale di Azure fare clic sul pulsante **Azure Active Directory** nel riquadro sinistro.

    ![Pulsante Azure Active Directory](./media/active-directory-saas-clicktime-tutorial/create_aaduser_01.png) 

2. Per visualizzare l'elenco di utenti, passare a **Utenti e gruppi** e quindi fare clic su **Tutti gli utenti**.
    
    ![Collegamenti "Utenti e gruppi" e "Tutti gli utenti"](./media/active-directory-saas-clicktime-tutorial/create_aaduser_02.png) 

3. Per aprire la finestra di dialogo **Utente** fare clic su **Aggiungi** nella parte superiore della finestra di dialogo **Tutti gli utenti**.
 
    ![Pulsante Aggiungi](./media/active-directory-saas-clicktime-tutorial/create_aaduser_03.png) 

4. Nella finestra di dialogo **Utente** seguire questa procedura:
 
    ![Finestra di dialogo Utente](./media/active-directory-saas-clicktime-tutorial/create_aaduser_04.png) 

    a. Nella casella di testo **Nome** digitare **BrittaSimon**.

    b. Nella casella di testo **Nome utente** digitare l'**indirizzo di posta elettronica** di BrittaSimon.

    c. Selezionare **Mostra password** e prendere nota del valore della **Password**.

    d. Fare clic su **Crea**.
 
### <a name="create-a-clicktime-test-user"></a>Creare un utente test di ClickTime

Per consentire agli utenti di Azure AD di accedere a ClickTime, è necessario eseguirne il provisioning in ClickTime.  
Nel caso di ClickTime, il provisioning è un'attività manuale.

> [!NOTE]
> È possibile usare qualsiasi altro strumento o API di creazione di account utente fornita da ClickTime per effettuare il provisioning degli account utente di Azure AD.

**Per eseguire il provisioning di un account utente, seguire questa procedura:**
1. Accedere al tenant **ClickTime** .
2. Nel barra degli strumenti in alto fare clic su **Company** (Azienda) e quindi su **People** (Persone).
   
    ![Persone](./media/active-directory-saas-clicktime-tutorial/tic777282.png "Persone")
3. Fare clic su **Add Person**.
   
    ![Aggiungi persona](./media/active-directory-saas-clicktime-tutorial/tic777283.png "Aggiungi persona")
4. Nella sezione New Person seguire questa procedura:
   
    ![Persone](./media/active-directory-saas-clicktime-tutorial/tic777284.png "Persone")
   
    a.  Nella casella di testo **full name** (Nome completo) digitare il nome e cognome dell'utente, ad esempio **Britta Simon**. 
  
    b.  Nella casella di testo **email address** (Indirizzo di posta elettronica) digitare l'indirizzo di posta elettronica dell'utente come **brittasimon@contoso.com**.
       
    > [!NOTE]
    > È anche possibile impostare altre proprietà dell'oggetto new person.
   
    c.  Fare clic su **Save**.

### <a name="assign-the-azure-ad-test-user"></a>Assegnare l'utente test di Azure AD

In questa sezione Britta Simon viene abilitata per l'uso dell'accesso Single Sign-On di Azure, tramite la concessione dell'accesso a ClickTime.

![Assegnare il ruolo utente][200] 

**Per assegnare Britta Simon a ClickTime, seguire questa procedura:**

1. Nel portale di Azure aprire la visualizzazione delle applicazioni e quindi la visualizzazione delle directory e passare ad **Applicazioni aziendali**, quindi fare clic su **Tutte le applicazioni**.

    ![Assegna utente][201] 

2. Nell'elenco delle applicazioni, selezionare **ClickTime**.

    ![Collegamento di ClickTime nell'elenco delle applicazioni](./media/active-directory-saas-clicktime-tutorial/tutorial_clicktime_app.png) 

3. Scegliere **Utenti e gruppi** dal menu a sinistra.

    ![Collegamento "Utenti e gruppi"][202] 

4. Fare clic sul pulsante **Aggiungi**. Selezionare quindi **Utenti e gruppi** nella finestra di dialogo **Aggiungi assegnazione**.

    ![Riquadro Aggiungi assegnazione][203]

5. Nella finestra di dialogo **Utenti e gruppi** selezionare **Britta Simon** nell'elenco Utenti.

6. Fare clic sul pulsante **Seleziona** nella finestra di dialogo **Utenti e gruppi**.

7. Fare clic sul pulsante **Assegna** nella finestra di dialogo **Aggiungi assegnazione**.
    
### <a name="test-single-sign-on"></a>Testare l'accesso Single Sign-On

In questa sezione viene testata la configurazione dell'accesso Single Sign-On di Azure AD usando il pannello di accesso.

Quando si fa clic sul riquadro ClickTime nel pannello di accesso, si accederà automaticamente all'applicazione ClickTime.
Per altre informazioni sul pannello di accesso, vedere [Introduzione al Pannello di accesso](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Risorse aggiuntive

* [Elenco di esercitazioni sulla procedura di integrazione delle app SaaS con Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Informazioni sull'accesso alle applicazioni e Single Sign-On con Azure Active Directory](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_203.png

