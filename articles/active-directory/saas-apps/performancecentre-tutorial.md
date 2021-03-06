---
title: 'Didacticiel : Intégration d’Azure Active Directory à PerformanceCentre | Microsoft Docs'
description: Découvrez comment configurer l’authentification unique entre Azure Active Directory et PerformanceCentre.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 65288c32-f7e6-4eb3-a6dc-523c3d748d1c
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2017
ms.author: jeedes
ms.openlocfilehash: 1a9935dbb0ec43c1eb2ec78fdd7caa92b69e3784
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36214159"
---
# <a name="tutorial-azure-active-directory-integration-with-performancecentre"></a>Didacticiel : Intégration d’Azure Active Directory à PerformanceCentre

Dans ce didacticiel, vous apprenez à intégrer PerformanceCentre à Azure Active Directory (Azure AD).

L’intégration de PerformanceCentre dans Azure AD vous offre les avantages suivants :

- Dans Azure AD, vous pouvez contrôler qui a accès à PerformanceCentre.
- Vous pouvez autoriser les utilisateurs à se connecter automatiquement à PerformanceCentre (via l’authentification unique) avec leur compte Azure AD.
- Vous pouvez gérer vos comptes à partir d’un emplacement central : le portail Azure.

Pour en savoir plus sur l’intégration des applications SaaS avec Azure AD, consultez [Qu’est-ce que l’accès aux applications et l’authentification unique avec Azure Active Directory ?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Prérequis

Pour configurer l’intégration d’Azure AD avec PerformanceCentre, vous avez besoin des éléments suivants :

- Un abonnement Azure AD
- Un abonnement PerformanceCentre pour lequel l’authentification unique est activée

> [!NOTE]
> Pour tester les étapes de ce didacticiel, nous déconseillons l’utilisation d’un environnement de production.

Vous devez en outre suivre les recommandations ci-dessous :

- N’utilisez pas votre environnement de production, sauf si cela est nécessaire.
- Si vous n’avez pas d’environnement d’essai Azure AD, vous pouvez obtenir un essai d’un mois [ici](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Description du scénario
Dans ce didacticiel, vous testez l’authentification unique Azure AD dans un environnement de test. Le scénario décrit dans ce didacticiel se compose des deux sections principales suivantes :

1. Ajout de PerformanceCentre à partir de la galerie
2. Configuration et test de l’authentification unique Azure AD

## <a name="adding-performancecentre-from-the-gallery"></a>Ajout de PerformanceCentre à partir de la galerie
Pour configurer l’intégration de PerformanceCentre avec Azure AD, vous devez ajouter PerformanceCentre à partir de la galerie à votre liste d’applications SaaS gérées.

**Pour ajouter PerformanceCentre à partir de la galerie, procédez comme suit :**

1. Dans le volet de navigation gauche du **[portail Azure](https://portal.azure.com)**, cliquez sur l’icône **Azure Active Directory**. 

    ![Active Directory][1]

2. Accédez à **Applications d’entreprise**. Accédez ensuite à **Toutes les applications**.

    ![APPLICATIONS][2]
    
3. Pour ajouter l’application, cliquez sur le bouton **Nouvelle application** en haut de la boîte de dialogue.

    ![APPLICATIONS][3]

4. Dans la zone de recherche, entrez **PerformanceCentre**.

    ![Création d’un utilisateur de test Azure AD](./media/performancecentre-tutorial/tutorial_performancecentre_search.png)

5. Dans le volet de résultats, sélectionnez **PerformanceCentre**, puis cliquez sur le bouton **Ajouter** pour ajouter l’application.

    ![Création d’un utilisateur de test Azure AD](./media/performancecentre-tutorial/tutorial_performancecentre_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configuration et test de l’authentification unique Azure AD
Dans cette section, vous configurez et testez l’authentification unique Azure AD avec PerformanceCentre au moyen d’un utilisateur de test appelé « Britta Simon ».

Pour que l’authentification unique fonctionne, Azure AD a besoin de savoir qui est l’utilisateur PerformanceCentre équivalent dans Azure AD. En d’autres termes, une relation entre un utilisateur Azure AD et l’utilisateur PerformanceCentre associé doit être établie.

Dans PerformanceCentre, affectez la valeur du **nom d’utilisateur** indiquée dans Azure AD comme valeur du **nom d’utilisateur** pour établir la relation.

Pour configurer et tester l’authentification unique Azure AD avec PerformanceCentre, vous avez besoin de suivre les indications des sections suivantes :

1. **[Configuration de l’authentification unique Azure AD](#configuring-azure-ad-single-sign-on)** pour permettre à vos utilisateurs d’utiliser cette fonctionnalité.
2. **[Création d’un utilisateur de test Azure AD](#creating-an-azure-ad-test-user)** pour tester l’authentification unique Azure AD avec Britta Simon.
3. **[Création d’un utilisateur de test PerformanceCentre](#creating-a-performancecentre-test-user)** pour avoir dans PerformanceCentre un équivalent de Britta Simon lié à la représentation Azure AD associée.
4. **[Affectation de l’utilisateur de test Azure AD](#assigning-the-azure-ad-test-user)** : permet à Britta Simon d’utiliser l’authentification unique Azure AD.
5. **[Testing Single Sign-On](#testing-single-sign-on)** pour vérifier si la configuration fonctionne.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuration de l’authentification unique Azure AD

Dans cette section, vous activez l’authentification unique Azure AD dans le portail Azure et configurez l’authentification unique dans votre application PerformanceCentre.

**Pour configurer l’authentification unique Azure AD avec PerformanceCentre, procédez comme suit :**

1. Dans le portail Azure, sur la page d’intégration de l’application **PerformanceCentre** , cliquez sur **Authentification unique**.

    ![Configure Single Sign-On][4]

2. Dans la boîte de dialogue **Authentification unique**, pour le **Mode**, sélectionnez **Authentification basée sur SAML** pour activer l’authentification unique.
 
    ![Configure Single Sign-On](./media/performancecentre-tutorial/tutorial_performancecentre_samlbase.png)

3. Dans la section **Domaine et URL PerformanceCentre**, procédez comme suit :

    ![Configure Single Sign-On](./media/performancecentre-tutorial/tutorial_performancecentre_url.png)

    a. Dans la zone de texte **URL de connexion**, tapez une URL au format suivant : `http://companyname.performancecentre.com/saml/SSO`

    b. Dans la zone de texte **Identificateur**, tapez une URL au format suivant : `http://companyname.performancecentre.com`

    > [!NOTE] 
    > Il ne s’agit pas de valeurs réelles. Mettez à jour ces valeurs avec l’URL de connexion et l’identificateur réels. Pour obtenir ces valeurs, contactez l’[équipe du support technique de PerformanceCentre](https://www.performancecentre.com/contact-us/). 

4. Dans la section **Certificat de signature SAML**, cliquez sur **Métadonnées XML** puis enregistrez le fichier de métadonnées sur votre ordinateur.

    ![Configure Single Sign-On](./media/performancecentre-tutorial/tutorial_performancecentre_certificate.png) 

5. Cliquez sur le bouton **Enregistrer** .

    ![Configure Single Sign-On](./media/performancecentre-tutorial/tutorial_general_400.png)

6. Dans la section **Configuration PerformanceCentre**, cliquez sur **Configurer PerformanceCentre** pour ouvrir la fenêtre **Configurer l’authentification**. Copiez l’**ID d’entité SAML et l’URL du service d’authentification unique SAML** à partir de la **section Référence rapide**.

    ![Configure Single Sign-On](./media/performancecentre-tutorial/tutorial_performancecentre_configure.png) 

7. Connectez-vous à votre site d’entreprise **PerformanceCentre** en tant qu’administrateur.

8. Dans l’onglet sur le côté gauche, cliquez sur **Configure**.
   
    ![Authentification unique Azure AD][10]

9. Dans l’onglet sur le côté gauche, cliquez sur **Miscellaneous**, puis cliquez sur **Single Sign On**.
   
    ![Authentification unique Azure AD][11]

10. Pour **Protocol**, sélectionnez **SAML**.
   
    ![Authentification unique Azure AD][12]

11. Ouvrez votre fichier de métadonnées téléchargé dans le Bloc-notes, copiez son contenu, collez-le dans la zone de texte **Identity Provider Metadata**, puis cliquez sur **Save**.
   
    ![Authentification unique Azure AD][13]

12. Vérifiez que les valeurs **Entity Base URL** et **Entity ID URL** sont correctes.
    
     ![Authentification unique Azure AD][14]

> [!TIP]
> Vous pouvez maintenant lire une version concise de ces instructions dans le [portail Azure](https://portal.azure.com), pendant que vous configurez l’application.  Après avoir ajouté cette application à partir de la section **Active Directory > Applications d’entreprise**, cliquez simplement sur l’onglet **Authentification unique** et accédez à la documentation incorporée par le biais de la section **Configuration** en bas. Vous pouvez en savoir plus sur la fonctionnalité de documentation incorporée ici : [Documentation incorporée Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Création d’un utilisateur de test Azure AD
L’objectif de cette section est de créer un utilisateur de test appelé Britta Simon dans le portail Azure.

![Créer un utilisateur Azure AD][100]

**Pour créer un utilisateur de test dans Azure AD, procédez comme suit :**

1. Dans le panneau de navigation gauche du **portail Azure**, cliquez sur l’icône **Azure Active Directory**.

    ![Création d’un utilisateur de test Azure AD](./media/performancecentre-tutorial/create_aaduser_01.png) 

2. Pour afficher la liste des utilisateurs, accédez à **Utilisateurs et groupes**, puis cliquez sur **Tous les utilisateurs**.
    
    ![Création d’un utilisateur de test Azure AD](./media/performancecentre-tutorial/create_aaduser_02.png) 

3. Pour ouvrir la boîte de dialogue **Utilisateur**, cliquez sur **Ajouter** en haut de la boîte de dialogue.
 
    ![Création d’un utilisateur de test Azure AD](./media/performancecentre-tutorial/create_aaduser_03.png) 

4. Dans la boîte de dialogue **Utilisateur**, procédez comme suit :
 
    ![Création d’un utilisateur de test Azure AD](./media/performancecentre-tutorial/create_aaduser_04.png) 

    a. Dans la zone de texte **Nom**, entrez **BrittaSimon**.

    b. Dans la zone de texte **Nom d’utilisateur**, tapez **l’adresse e-mail** de Britta Simon.

    c. Sélectionnez **Afficher le mot de passe** et notez la valeur du **mot de passe**.

    d. Cliquez sur **Créer**.
 
### <a name="creating-a-performancecentre-test-user"></a>Création d’un utilisateur de test PerformanceCentre

L’objectif de cette section est de créer un utilisateur appelé Britta Simon dans PerformanceCentre.

**Pour créer un utilisateur appelé Britta Simon dans PerformanceCentre, procédez comme suit :**

1. Connectez-vous à votre site d’entreprise PerformanceCentre en tant qu’administrateur.

2. Dans le menu de gauche, cliquez sur **Interrelate**, puis cliquez sur **Create Participant**.
   
    ![Create User][400]

3. Dans la boîte de dialogue **Interrelate - Create Participant** , effectuez les opérations suivantes :
   
    ![Create User][401]
    
    a. Entrez les attributs requis pour Britta Simon dans les zones de texte correspondantes.
    
    >[!IMPORTANT]
    >L’attribut de nom d’utilisateur de Britta dans PerformanceCentre doit être le même que le nom d’utilisateur dans Azure AD.
    
    b. Sélectionnez **Client Administrator** pour **Choose Role**.
    
    c. Cliquez sur **Enregistrer**. 

### <a name="assigning-the-azure-ad-test-user"></a>Affectation de l’utilisateur de test Azure AD

Dans cette section, vous autorisez Britta Simon à utiliser l’authentification unique Azure en accordant l’accès à PerformanceCentre.

![Affecter des utilisateurs][200] 

**Pour attribuer Britta Simon à PerformanceCentre, procédez comme suit :**

1. Dans le portail Azure, ouvrez la vue des applications, accédez à la vue des répertoires, accédez à **Applications d’entreprise**, puis cliquez sur **Toutes les applications**.

    ![Affecter des utilisateurs][201] 

2. Dans la liste des applications, sélectionnez **PerformanceCentre**.

    ![Configure Single Sign-On](./media/performancecentre-tutorial/tutorial_performancecentre_app.png) 

3. Dans le menu de gauche, cliquez sur **Utilisateurs et groupes**.

    ![Affecter des utilisateurs][202] 

4. Cliquez sur le bouton **Ajouter**. Ensuite, sélectionnez **Utilisateurs et groupes** dans la boîte de dialogue **Ajouter une affectation**.

    ![Affecter des utilisateurs][203]

5. Dans la boîte de dialogue **Utilisateurs et groupes**, sélectionnez **Britta Simon** dans la liste des utilisateurs.

6. Cliquez sur le bouton **Sélectionner** dans la boîte de dialogue **Utilisateurs et groupes**.

7. Cliquez sur le bouton **Affecter** dans la boîte de dialogue **Ajouter une affectation**.
    
### <a name="testing-single-sign-on"></a>Test de l’authentification unique

L’objectif de cette section est de tester la configuration de l’authentification unique Azure AD à l’aide du volet d’accès.  

Lorsque vous cliquez sur la vignette PerformanceCentre dans le volet d’accès, vous devez être connecté automatiquement à votre application PerformanceCentre.

## <a name="additional-resources"></a>Ressources supplémentaires

* [Liste de didacticiels sur l’intégration d’applications SaaS avec Azure Active Directory](tutorial-list.md)
* [Qu’est-ce que l’accès aux applications et l’authentification unique avec Azure Active Directory ?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/performancecentre-tutorial/tutorial_general_01.png
[2]: ./media/performancecentre-tutorial/tutorial_general_02.png
[3]: ./media/performancecentre-tutorial/tutorial_general_03.png
[4]: ./media/performancecentre-tutorial/tutorial_general_04.png
[10]: ./media/performancecentre-tutorial/tutorial_performancecentre_06.png
[11]: ./media/performancecentre-tutorial/tutorial_performancecentre_07.png
[12]: ./media/performancecentre-tutorial/tutorial_performancecentre_08.png
[13]: ./media/performancecentre-tutorial/tutorial_performancecentre_09.png
[14]: ./media/performancecentre-tutorial/tutorial_performancecentre_10.png

[100]: ./media/performancecentre-tutorial/tutorial_general_100.png

[200]: ./media/performancecentre-tutorial/tutorial_general_200.png
[201]: ./media/performancecentre-tutorial/tutorial_general_201.png
[202]: ./media/performancecentre-tutorial/tutorial_general_202.png
[203]: ./media/performancecentre-tutorial/tutorial_general_203.png
[400]: ./media/performancecentre-tutorial/tutorial_performancecentre_11.png
[401]: ./media/performancecentre-tutorial/tutorial_performancecentre_12.png

