---
title: 'Didacticiel : intégration d’Azure Active Directory à Envi MMIS | Microsoft Docs'
description: Découvrez comment configurer l’authentification unique entre Azure Active Directory et Envi MMIS.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: ab89f8ee-2507-4625-94bc-b24ef3d5e006
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/03/2018
ms.author: jeedes
ms.openlocfilehash: 70066f1c29849b77c67710eb908ef2a340cdc45f
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/14/2018
ms.locfileid: "39047658"
---
# <a name="tutorial-azure-active-directory-integration-with-envi-mmis"></a>Didacticiel : intégration d’Azure Active Directory à Envi MMIS

Dans ce didacticiel, vous allez apprendre à intégrer Envi MMIS à Azure Active Directory (Azure AD).

L’intégration d’Envi MMIS à Azure AD vous offre les avantages suivants :

- Dans Azure AD, vous pouvez contrôler qui a accès à Envi MMIS.
- Vous pouvez autoriser vos utilisateurs à se connecter automatiquement à Envi MMIS (par le biais de l’authentification unique) avec leur compte Azure AD.
- Vous pouvez gérer vos comptes dans un emplacement central : le portail Azure

Pour en savoir plus sur l’intégration des applications SaaS avec Azure AD, consultez [Qu’est-ce que l’accès aux applications et l’authentification unique avec Azure Active Directory ?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Prérequis

Pour configurer l’intégration d’Azure AD à Envi MMIS, vous devez disposer des éléments suivants :

- Un abonnement Azure AD
- Un abonnement Envi MMIS pour lequel l’authentification unique est activée

> [!NOTE]
> Pour tester les étapes de ce didacticiel, nous déconseillons l’utilisation d’un environnement de production.

Vous devez en outre suivre les recommandations ci-dessous :

- N’utilisez pas votre environnement de production, sauf si cela est nécessaire.
- Si vous n’avez pas d’environnement d’essai Azure AD, vous pouvez [obtenir un essai d’un mois](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Description du scénario
Dans ce didacticiel, vous testez l’authentification unique Azure AD dans un environnement de test. Le scénario décrit dans ce didacticiel se compose des deux sections principales suivantes :

1. Ajout d’Envi MMIS à partir de la galerie
2. Configuration et test de l’authentification unique Azure AD

## <a name="adding-envi-mmis-from-the-gallery"></a>Ajout d’Envi MMIS à partir de la galerie
Pour configurer l’intégration d’Envi MMIS à Azure AD, vous devez ajouter Envi MMIS à partir de la galerie à votre liste d’applications SaaS gérées.

**Pour ajouter Envi MMIS à partir de la galerie, procédez comme suit :**

1. Dans le volet de navigation gauche du **[portail Azure](https://portal.azure.com)**, cliquez sur l’icône **Azure Active Directory**. 

    ![Bouton Azure Active Directory][1]

2. Accédez à **Applications d’entreprise**. Accédez ensuite à **Toutes les applications**.

    ![Panneau Applications d’entreprise][2]
    
3. Pour ajouter l’application, cliquez sur le bouton **Nouvelle application** en haut de la boîte de dialogue.

    ![Bouton Nouvelle application][3]

4. Dans la zone de recherche, tapez **Envi MMIS**, sélectionnez **Envi MMIS** dans le panneau de résultats, puis cliquez sur le bouton **Ajouter** pour ajouter l’application.

    ![Envi MMIS dans la liste des résultats](./media/envimmis-tutorial/tutorial_envimmis_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurer et tester l’authentification unique Azure AD

Dans cette section, vous allez configurer et tester l’authentification unique Azure AD avec Envi MMIS et un utilisateur de test appelé « Britta Simon ».

Pour que l’authentification unique fonctionne, Azure AD doit savoir qui est l’utilisateur Envi MMIS équivalent dans Azure AD. En d’autres termes, une relation entre un utilisateur Azure AD et un utilisateur Envi MMIS associé doit être établie.

Pour configurer et tester l’authentification unique Azure AD avec Envi MMIS, suivez les indications des sections ci-après :

1. **[Configurer l’authentification unique Azure AD](#configure-azure-ad-single-sign-on)** pour permettre à vos utilisateurs d’utiliser cette fonctionnalité.
2. **[Créer un utilisateur de test Azure AD](#create-an-azure-ad-test-user)** pour tester l’authentification unique Azure AD avec Britta Simon.
3. **[Créer un utilisateur de test Envi MMIS](#create-an-envi-mmis-test-user)** pour avoir un équivalent de Britta Simon dans Envi MMIS, lié à la représentation Azure AD associée.
4. **[Affecter l’utilisateur de test Azure AD](#assign-the-azure-ad-test-user)** pour permettre à Britta Simon d’utiliser l’authentification unique Azure AD.
5. **[Tester l’authentification unique](#test-single-sign-on)** : pour vérifier si la configuration fonctionne.

### <a name="configure-azure-ad-single-sign-on"></a>Configurer l’authentification unique Azure AD

Dans cette section, vous allez activer l’authentification unique Azure AD dans le Portail Azure et configurer l’authentification unique dans votre application Envi MMIS.

**Pour configurer l’authentification unique Azure AD avec Envi MMIS, procédez comme suit :**

1. Dans le Portail Azure, sur la page d’intégration de l’application **Envi MMIS**, cliquez sur **Authentification unique**.

    ![Lien Configurer l’authentification unique][4]

2. Dans la boîte de dialogue **Authentification unique**, pour le **Mode**, sélectionnez **Authentification basée sur SAML** pour activer l’authentification unique.
 
    ![Boîte de dialogue Authentification unique](./media/envimmis-tutorial/tutorial_envimmis_samlbase.png)

3. Dans la section **Domaine et URL Envi MMIS**, suivez les étapes ci-dessous pour configurer l’application en mode initié par **IDP** :

    ![Informations d’authentification unique dans la section Domaine et URL Envi MMIS](./media/envimmis-tutorial/tutorial_envimmis_url.png)

    a. Dans la zone de texte **Identificateur**, tapez une URL au format suivant : `https://www.<CUSTOMER DOMAIN>.com/Account`

    b. Dans la zone de texte **URL de réponse** , tapez une URL au format suivant : `https://www.<CUSTOMER DOMAIN>.com/Account/Acs`

4. Si vous souhaitez configurer l’application en **mode démarré par le fournisseur de service**, cochez **Afficher les paramètres d’URL avancés**, puis effectuez les étapes suivantes :

    ![Informations d’authentification unique dans la section Domaine et URL Envi MMIS](./media/envimmis-tutorial/tutorial_envimmis_url1.png)

    Dans la zone de texte **URL de connexion**, tapez une URL au format suivant : `https://www.<CUSTOMER DOMAIN>.com/Account`
     
    > [!NOTE]
    > Il ne s’agit pas de valeurs réelles. Mettez à jour ces valeurs avec l’identificateur, l’URL de réponse et l’URL de connexion réels. Pour obtenir ces valeurs, contactez [l’équipe du support technique d’Envi MMIS](mailto:support@ioscorp.com).

5. Dans la section **Certificat de signature SAML**, cliquez sur **Métadonnées XML** puis enregistrez le fichier de métadonnées sur votre ordinateur.

    ![Lien Téléchargement de certificat](./media/envimmis-tutorial/tutorial_envimmis_certificate.png) 

6. Cliquez sur le bouton **Enregistrer** .

    ![Bouton Enregistrer de la page Configurer l’authentification unique](./media/envimmis-tutorial/tutorial_general_400.png)

7. Dans une autre fenêtre de navigateur web, connectez-vous à votre site Envi MMIS en tant qu’administrateur.

8. Cliquez sur l’onglet **My Domain** (Mon domaine).

    ![Bouton Enregistrer de la page Configurer l’authentification unique](./media/envimmis-tutorial/configure1.png)

9. Cliquez sur **Modifier**.

    ![Bouton Enregistrer de la page Configurer l’authentification unique](./media/envimmis-tutorial/configure2.png)

10. Cochez la case **Use remote authentication** (Utiliser l’authentification distante), puis sélectionnez **HTTP Redirect** (Redirection HTTP) dans la liste déroulante **Authentication Type** (Type d’authentification).

    ![Bouton Enregistrer de la page Configurer l’authentification unique](./media/envimmis-tutorial/configure3.png)

11. Sélectionnez l’onglet **Resources** (Ressources), puis cliquez sur **Upload Metadata** (Charger les métadonnées).

    ![Bouton Enregistrer de la page Configurer l’authentification unique](./media/envimmis-tutorial/configure4.png)

12. Dans la section **Upload Metadata** (Charger les métadonnées), procédez comme suit :

    ![Bouton Enregistrer de la page Configurer l’authentification unique](./media/envimmis-tutorial/configure5.png)

    a. Dans la liste déroulante **Upload From** (Charger à partir de), sélectionnez **File** (Fichier).

    b. Chargez le fichier de métadonnées téléchargé à partir du Portail Azure en sélectionnant **l’icône de choix de fichier**.

    c. Cliquez sur **OK**.

13. Une fois que vous avez chargé le fichier de métadonnées téléchargé, les champs sont automatiquement renseignés. Cliquez sur **Mettre à jour**

    ![Bouton Enregistrer de la page Configurer l’authentification unique](./media/envimmis-tutorial/configure6.png)

### <a name="create-an-azure-ad-test-user"></a>Créer un utilisateur de test Azure AD

L’objectif de cette section est de créer un utilisateur de test appelé Britta Simon dans le portail Azure.

   ![Créer un utilisateur de test Azure AD][100]

**Pour créer un utilisateur de test dans Azure AD, procédez comme suit :**

1. Dans le volet gauche du Portail Azure, cliquez sur le bouton **Azure Active Directory**.

    ![Bouton Azure Active Directory](./media/envimmis-tutorial/create_aaduser_01.png)

2. Pour afficher la liste des utilisateurs, accédez à **Utilisateurs et groupes**, puis cliquez sur **Tous les utilisateurs**.

    ![Liens « Utilisateurs et groupes » et « Tous les utilisateurs »](./media/envimmis-tutorial/create_aaduser_02.png)

3. Pour ouvrir la boîte de dialogue **Utilisateur**, cliquez sur **Ajouter** en haut de la boîte de dialogue **Tous les utilisateurs**.

    ![Bouton Ajouter](./media/envimmis-tutorial/create_aaduser_03.png)

4. Dans la boîte de dialogue **Utilisateur**, procédez comme suit :

    ![Boîte de dialogue Utilisateur](./media/envimmis-tutorial/create_aaduser_04.png)

    a. Dans la zone **Nom**, tapez **BrittaSimon**.

    b. Dans la zone **Nom d’utilisateur** , tapez l’adresse e-mail de l’utilisateur Britta Simon.

    c. Cochez la case **Afficher le mot de passe**, puis notez la valeur affichée dans le champ **Mot de passe**.

    d. Cliquez sur **Créer**.
 
### <a name="create-an-envi-mmis-test-user"></a>Créer un utilisateur de test Envi MMIS

Pour se connecter à Envi MMIS, les utilisateurs Azure AD doivent être approvisionnés dans Envi MMIS.  
Dans le cas d’Envi MMIS, l’approvisionnement est une tâche manuelle.

**Pour approvisionner un compte d’utilisateur, procédez comme suit :**

1. Connectez-vous à votre site d’entreprise Envi MMIS en tant qu’administrateur.

2. Cliquez sur l’onglet **User List** (Liste d’utilisateurs).

    ![Ajouter un employé](./media/envimmis-tutorial/user1.png)

3. Cliquez sur le bouton **Add User** (Ajouter un utilisateur).

    ![Ajouter un employé](./media/envimmis-tutorial/user2.png)

4. Dans la section **Add User** , procédez comme suit :

    ![Ajouter un employé](./media/envimmis-tutorial/user3.png)

    a. Dans la zone de texte **User Name** (Nom d’utilisateur), tapez le nom d’utilisateur du compte de Britta Simon, comme **brittasimon@contoso.com**.
    
    b. Dans la zone de texte **First Name** (Prénom), tapez le prénom de Britta Simon, tel que **Britta**.

    c. Dans la zone de texte **Last Name** (Nom de famille), tapez le nom de Britta Simon, par exemple **Simon**.

    d. Dans la zone de texte **Title** (Titre), entrez le titre de l’utilisateur.
    
    e. Dans la zone de texte **Email Address** (Adresse e-mail), tapez l’adresse e-mail du compte de Britta Simon, comme **brittasimon@contoso.com**.

    f. Dans la zone de texte **SSO User Name** (Nom d’utilisateur SSO), tapez le nom d’utilisateur du compte de Britta Simon, comme **brittasimon@contoso.com**.

    g. Cliquez sur **Enregistrer**.

### <a name="assign-the-azure-ad-test-user"></a>Affecter l’utilisateur de test Azure AD

Dans cette section, vous allez autoriser Britta Simon à utiliser l’authentification unique Azure en lui accordant l’accès à Envi MMIS.

![Attribuer le rôle utilisateur][200] 

**Pour assigner Britta Simon à Envi MMIS, procédez comme suit :**

1. Dans le portail Azure, ouvrez la vue des applications, accédez à la vue des répertoires, accédez à **Applications d’entreprise**, puis cliquez sur **Toutes les applications**.

    ![Affecter des utilisateurs][201] 

2. Dans la liste des applications, sélectionnez **Envi MMIS**.

    ![Lien Envi MMIS dans la liste des applications](./media/envimmis-tutorial/tutorial_envimmis_app.png)  

3. Dans le menu de gauche, cliquez sur **Utilisateurs et groupes**.

    ![Lien « Utilisateurs et groupes »][202]

4. Cliquez sur le bouton **Ajouter**. Ensuite, sélectionnez **Utilisateurs et groupes** dans la boîte de dialogue **Ajouter une affectation**.

    ![Volet Ajouter une attribution][203]

5. Dans la boîte de dialogue **Utilisateurs et groupes**, sélectionnez **Britta Simon** dans la liste des utilisateurs.

6. Cliquez sur le bouton **Sélectionner** dans la boîte de dialogue **Utilisateurs et groupes**.

7. Cliquez sur le bouton **Affecter** dans la boîte de dialogue **Ajouter une affectation**.
    
### <a name="test-single-sign-on"></a>Tester l’authentification unique

Dans cette section, vous allez tester la configuration de l’authentification unique Azure AD à l’aide du volet d’accès.

Lorsque vous cliquez sur la vignette Envi MMIS dans le volet d’accès, vous devriez être automatiquement connecté à votre application Envi MMIS.
Pour plus d’informations sur le panneau d’accès, consultez [Présentation du panneau d’accès](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ressources supplémentaires

* [Liste de didacticiels sur l’intégration d’applications SaaS avec Azure Active Directory](tutorial-list.md)
* [Qu’est-ce que l’accès aux applications et l’authentification unique avec Azure Active Directory ?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/envimmis-tutorial/tutorial_general_01.png
[2]: ./media/envimmis-tutorial/tutorial_general_02.png
[3]: ./media/envimmis-tutorial/tutorial_general_03.png
[4]: ./media/envimmis-tutorial/tutorial_general_04.png

[100]: ./media/envimmis-tutorial/tutorial_general_100.png

[200]: ./media/envimmis-tutorial/tutorial_general_200.png
[201]: ./media/envimmis-tutorial/tutorial_general_201.png
[202]: ./media/envimmis-tutorial/tutorial_general_202.png
[203]: ./media/envimmis-tutorial/tutorial_general_203.png

