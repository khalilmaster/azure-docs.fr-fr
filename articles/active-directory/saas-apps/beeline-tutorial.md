---
title: 'Didacticiel : Intégration d’Azure Active Directory à BeeLine | Microsoft Docs'
description: Découvrez comment configurer l’authentification unique entre Azure Active Directory et BeeLine.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 0726859d-1dac-44a0-810b-da56d89039ee
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: a36161e5fcdcd6e015a585f7ce08b88101ca1628
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36214801"
---
# <a name="tutorial-azure-active-directory-integration-with-beeline"></a>Didacticiel : Intégration d’Azure Active Directory à Beeline

Dans ce didacticiel, vous allez apprendre à intégrer BeeLine à Azure Active Directory (Azure AD).

L’intégration de BeeLine à Azure AD vous offre les avantages suivants :

- Dans Azure AD, vous pouvez contrôler qui a accès à BeeLine
- Vous pouvez autoriser vos utilisateurs à se connecter automatiquement à BeeLine (par le biais de l’authentification unique) avec leur compte Azure AD
- Vous pouvez gérer vos comptes à partir d’un emplacement central : le portail Azure.

Pour en savoir plus sur l’intégration des applications SaaS avec Azure AD, consultez [Qu’est-ce que l’accès aux applications et l’authentification unique avec Azure Active Directory ?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Prérequis

Pour configurer l’intégration d’Azure AD avec BeeLine, vous avez besoin des éléments suivants :

- Un abonnement Azure AD
- Un abonnement BeeLine pour lequel l’authentification unique est activée

> [!NOTE]
> Pour tester les étapes de ce didacticiel, nous déconseillons l’utilisation d’un environnement de production.

Vous devez en outre suivre les recommandations ci-dessous :

- N’utilisez pas votre environnement de production, sauf si cela est nécessaire.
- Si vous n’avez pas d’environnement d’essai Azure AD, vous pouvez obtenir un essai d’un mois [ici](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Description du scénario
Dans ce didacticiel, vous testez l’authentification unique Azure AD dans un environnement de test. Le scénario décrit dans ce didacticiel se compose des deux sections principales suivantes :

1. Ajout de BeeLine à partir de la galerie
2. Configuration et test de l’authentification unique Azure AD

## <a name="adding-beeline-from-the-gallery"></a>Ajout de BeeLine à partir de la galerie
Pour configurer l’intégration de BeeLine avec Azure AD, vous devez ajouter BeeLine, à partir de la galerie, à votre liste d’applications SaaS gérées.

**Pour ajouter BeeLine à partir de la galerie, effectuez les étapes suivantes :**

1. Dans le volet de navigation gauche du **[portail Azure](https://portal.azure.com)**, cliquez sur l’icône **Azure Active Directory**. 

    ![Active Directory][1]

2. Accédez à **Applications d’entreprise**. Accédez ensuite à **Toutes les applications**.

    ![APPLICATIONS][2]
    
3. Pour ajouter l’application, cliquez sur le bouton **Nouvelle application** en haut de la boîte de dialogue.

    ![APPLICATIONS][3]

4. Dans la zone de recherche, tapez **BeeLine**.

    ![Création d’un utilisateur de test Azure AD](./media/beeline-tutorial/tutorial_beeline_search.png)

5. Dans le volet de résultats, sélectionnez **BeeLine**, puis cliquez sur **Ajouter** pour ajouter l’application.

    ![Création d’un utilisateur de test Azure AD](./media/beeline-tutorial/tutorial_beeline_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configuration et test de l’authentification unique Azure AD
Dans cette section, vous allez configurer et tester l’authentification unique Azure AD avec BeeLine avec un utilisateur de test appelé « Britta Simon ».

Pour que l’authentification unique fonctionne, Azure AD doit savoir qui est l’utilisateur BeeLine équivalent dans Azure AD. En d’autres termes, une relation entre un utilisateur Azure AD et un utilisateur BeeLine associé doit être établie.

Dans BeeLine, affectez la valeur de **nom d’utilisateur** dans Azure AD comme valeur de **Nom d’utilisateur** pour établir la relation.

Pour configurer et tester l’authentification unique Azure AD avec BeeLine, vous devez suivre les indications des sections suivantes :

1. **[Configuration de l’authentification unique Azure AD](#configuring-azure-ad-single-sign-on)** pour permettre à vos utilisateurs d’utiliser cette fonctionnalité.
2. **[Création d’un utilisateur de test Azure AD](#creating-an-azure-ad-test-user)** pour tester l’authentification unique Azure AD avec Britta Simon.
3. **[Création d’un utilisateur de test BeeLine](#creating-a-beeline-test-user)** pour avoir un équivalent de Britta Simon dans BeeLine lié à la représentation Azure AD associée.
4. **[Affectation de l’utilisateur de test Azure AD](#assigning-the-azure-ad-test-user)** : permet à Britta Simon d’utiliser l’authentification unique Azure AD.
5. **[Testing Single Sign-On](#testing-single-sign-on)** pour vérifier si la configuration fonctionne.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuration de l’authentification unique Azure AD

Dans cette section, vous allez activer l’authentification unique Azure AD dans le portail Azure et configurer l’authentification unique dans votre application BeeLine.

**Pour configurer l’authentification unique Azure AD avec BeeLine, effectuez les étapes suivantes :**

1. Dans le portail Azure, dans la page d’intégration de l’application **BeeLine**, cliquez sur **Authentification unique**.

    ![Configure Single Sign-On][4]

2. Dans la boîte de dialogue **Authentification unique**, pour le **Mode**, sélectionnez **Authentification basée sur SAML** pour activer l’authentification unique.
 
    ![Configure Single Sign-On](./media/beeline-tutorial/tutorial_beeline_samlbase.png)

3. Dans la section **Domaine et URL BeeLine**, effectuez les étapes suivantes :

    ![Configure Single Sign-On](./media/beeline-tutorial/tutorial_beeline_url.png)

    a. Dans la zone de texte **Identificateur**, tapez une URL au format suivant : `https://projects.beeline.net/<instancename>`

    b. Dans la zone de texte **URL de réponse** , tapez une URL au format suivant :
    | |
    |--|
    | `https://projects.beeline.net/<instancename>/SSO_External.ashx`|
    | `https://projects.beeline.net/<companyname>/SSO_External.ashx` |

    > [!NOTE] 
    > Il ne s’agit pas de valeurs réelles. Mettez à jour ces valeurs avec l’identificateur et l’URL de réponse réels. Pour obtenir ces valeurs, contactez [l’équipe de support technique de BeeLine](https://www.beeline.com/contact-us/).
 
4. Dans la section **Certificat de signature SAML**, cliquez sur **Métadonnées XML** puis enregistrez le fichier de métadonnées sur votre ordinateur.

    ![Configure Single Sign-On](./media/beeline-tutorial/tutorial_beeline_certificate.png) 

5. Votre application Beeline attend les assertions SAML dans un format spécifique. Collaborez avec l’[équipe de support technique de BeeLine](https://www.beeline.com/contact-us/) pour identifier tout d’abord l’identificateur d’utilisateur correct qui sera mappé à l’application. Suivez également les instructions de l’[équipe de support technique de BeeLine](https://www.beeline.com/contact-us/) concernant l’attribut à utiliser pour ce mappage. Vous pouvez gérer la valeur de cet attribut à partir de l’onglet **Attributs utilisateur** de l’application. La capture d’écran suivante montre un exemple : Ici, nous avons mis en correspondance la revendication **Identificateur d’utilisateur** avec l’attribut **userprincipalname**, qui fournit l’ID utilisateur unique qui sera envoyé à l’application BeeLine dans chaque réponse SAML correcte.

    ![Configure Single Sign-On](./media/beeline-tutorial/tutorial_attribute.png)    

6. Cliquez sur le bouton **Enregistrer** .

    ![Configure Single Sign-On](./media/beeline-tutorial/tutorial_general_400.png)

7. Dans la section **Configuration de BeeLine**, cliquez sur **Configurer BeeLine** pour ouvrir la fenêtre **Configurer l’authentification**. Copiez **l’URL de déconnexion** et l’**ID d’entité SAML** à partir de la **section Référence rapide**.

    ![Configure Single Sign-On](./media/beeline-tutorial/tutorial_beeline_configure.png) 

8. Pour configurer l’authentification unique côté **BeeLine**, vous devez envoyer le **XML de métadonnées** téléchargé, l’**ID d’entité SAML** et l’**URL de déconnexion** à l’[équipe de support technique BeeLine](https://www.beeline.com/contact-us/).

> [!TIP]
> Vous pouvez maintenant lire une version concise de ces instructions dans le [portail Azure](https://portal.azure.com), pendant que vous configurez l’application.  Après avoir ajouté cette application à partir de la section **Active Directory > Applications d’entreprise**, cliquez simplement sur l’onglet **Authentification unique** et accédez à la documentation incorporée par le biais de la section **Configuration** en bas. Vous pouvez en savoir plus sur la fonctionnalité de documentation incorporée ici : [Documentation incorporée Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Création d’un utilisateur de test Azure AD
L’objectif de cette section est de créer un utilisateur de test appelé Britta Simon dans le portail Azure.

![Créer un utilisateur Azure AD][100]

**Pour créer un utilisateur de test dans Azure AD, procédez comme suit :**

1. Dans le panneau de navigation gauche du **portail Azure**, cliquez sur l’icône **Azure Active Directory**.

    ![Création d’un utilisateur de test Azure AD](./media/beeline-tutorial/create_aaduser_01.png) 

2. Pour afficher la liste des utilisateurs, accédez à **Utilisateurs et groupes**, puis cliquez sur **Tous les utilisateurs**.
    
    ![Création d’un utilisateur de test Azure AD](./media/beeline-tutorial/create_aaduser_02.png) 

3. Pour ouvrir la boîte de dialogue **Utilisateur**, cliquez sur **Ajouter** en haut de la boîte de dialogue.
 
    ![Création d’un utilisateur de test Azure AD](./media/beeline-tutorial/create_aaduser_03.png) 

4. Dans la boîte de dialogue **Utilisateur**, procédez comme suit :
 
    ![Création d’un utilisateur de test Azure AD](./media/beeline-tutorial/create_aaduser_04.png) 

    a. Dans la zone de texte **Nom**, entrez **BrittaSimon**.

    b. Dans la zone de texte **Nom d’utilisateur**, tapez **l’adresse e-mail** de Britta Simon.

    c. Sélectionnez **Afficher le mot de passe** et notez la valeur du **mot de passe**.

    d. Cliquez sur **Créer**.
 
### <a name="creating-a-beeline-test-user"></a>Création d’un utilisateur de test BeeLine

Dans cette section, vous allez créer un utilisateur appelé Britta Simon dans Beeline. Tous les utilisateurs de BeeLine doivent être approvisionnés dans cette application avant de procéder à l’authentification unique. Vous devez donc collaborer avec l’[équipe de support technique de Beeline](https://www.beeline.com/contact-us/) pour approvisionner ces utilisateurs dans l’application. 

### <a name="assigning-the-azure-ad-test-user"></a>Affectation de l’utilisateur de test Azure AD

Dans cette section, vous allez autoriser Britta Simon à utiliser l’authentification unique Azure en lui accordant l’accès à BeeLine.

![Affecter des utilisateurs][200] 

**Pour affecter Britta Simon à BeeLine, effectuez les étapes suivantes :**

1. Dans le portail Azure, ouvrez la vue des applications, accédez à la vue des répertoires, accédez à **Applications d’entreprise**, puis cliquez sur **Toutes les applications**.

    ![Affecter des utilisateurs][201] 

2. Dans la liste des applications, sélectionnez **BeeLine**.

    ![Configure Single Sign-On](./media/beeline-tutorial/tutorial_beeline_app.png) 

3. Dans le menu de gauche, cliquez sur **Utilisateurs et groupes**.

    ![Affecter des utilisateurs][202] 

4. Cliquez sur le bouton **Ajouter**. Ensuite, sélectionnez **Utilisateurs et groupes** dans la boîte de dialogue **Ajouter une affectation**.

    ![Affecter des utilisateurs][203]

5. Dans la boîte de dialogue **Utilisateurs et groupes**, sélectionnez **Britta Simon** dans la liste des utilisateurs.

6. Cliquez sur le bouton **Sélectionner** dans la boîte de dialogue **Utilisateurs et groupes**.

7. Cliquez sur le bouton **Affecter** dans la boîte de dialogue **Ajouter une affectation**.
    
### <a name="testing-single-sign-on"></a>Test de l’authentification unique

Dans cette section, vous allez tester la configuration de l’authentification unique Azure AD à l’aide du volet d’accès. Lorsque vous cliquez sur la mosaïque Beeline dans le volet d’accès, vous devez être connecté automatiquement à votre application Beeline.

## <a name="additional-resources"></a>Ressources supplémentaires

* [Liste de didacticiels sur l’intégration d’applications SaaS avec Azure Active Directory](tutorial-list.md)
* [Qu’est-ce que l’accès aux applications et l’authentification unique avec Azure Active Directory ?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/beeline-tutorial/tutorial_general_01.png
[2]: ./media/beeline-tutorial/tutorial_general_02.png
[3]: ./media/beeline-tutorial/tutorial_general_03.png
[4]: ./media/beeline-tutorial/tutorial_general_04.png

[100]: ./media/beeline-tutorial/tutorial_general_100.png

[200]: ./media/beeline-tutorial/tutorial_general_200.png
[201]: ./media/beeline-tutorial/tutorial_general_201.png
[202]: ./media/beeline-tutorial/tutorial_general_202.png
[203]: ./media/beeline-tutorial/tutorial_general_203.png

