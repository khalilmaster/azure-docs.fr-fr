---
title: Qu’est ce qu’une protection de base dans l’accès conditionnel Azure Active Directory ? - préversion | Microsoft Docs
description: Découvrez comment la protection de base garantit qu’au moins le niveau de sécurité de base est activé dans votre environnement Azure Active Directory.
services: active-directory
keywords: accès conditionnel aux applications, accès conditionnel à Azure AD, accès sécurisé aux ressources d’entreprise, stratégies d’accès conditionnel
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: ''
ms.assetid: 8c1d978f-e80b-420e-853a-8bbddc4bcdad
ms.service: active-directory
ms.component: protection
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/02/2018
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: 1e7eb3a0098dc27b6f3c47d8d4848b2b9b5f7e61
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37447538"
---
# <a name="what-is-baseline-protection---preview"></a>Qu’est ce qu’une protection de base ? - préversion  

Au cours de l’année écoulée, les attaques d’identité ont augmenté de 300 %. Pour protéger votre environnement contre les attaques en augmentation constante, Azure Active Directory (Azure AD) introduit une nouvelle fonctionnalité appelée « protection de base ». La protection de base est un ensemble de [stratégies d’accès conditionnel](active-directory-conditional-access-azure-portal.md) prédéfinies. L’objectif de ces stratégies est de garantir qu’au moins le niveau de sécurité de base est activé dans toutes les éditions d’Azure AD. 

Cet article vous offre un aperçu de la protection de base dans Azure Active Directory.


 
## <a name="require-mfa-for-admins"></a>Demander l’authentification multifacteur pour les administrateurs

Les utilisateurs ayant accès à des comptes privilégiés ont accès sans restriction à votre environnement. En raison de l’importance de ces comptes, vous devez leur accorder une attention particulière. Une méthode courante pour améliorer la protection de comptes privilégiés consiste à demander une forme de vérification de compte plus sévère lorsqu’ils sont utilisés pour se connecter. Dans Azure Active Directory, vous pouvez obtenir une vérification plus sévère des comptes en demandant une authentification multifacteur (MFA).  

**Demander l’authentification multifacteur pour les administrateurs** est une stratégie de base qui requiert l’authentification multifacteur pour les rôles d’annuaire suivants : 

- Administrateur général  

- Administrateur SharePoint  

- Administrateur Exchange  

- Administrateur de l’accès conditionnel  

- Administrateur de sécurité  


![Azure Active Directory](./media/active-directory-conditional-access-baseline-protection/01.png)

Cette stratégie de base vous offre la possibilité d’exclure des utilisateurs et des groupes. Vous pouvez vouloir exclure un *[compte d’administration de l’accès d’urgence](users-groups-roles/directory-emergency-access.md)* pour garantir que votre accès au locataire n’est pas verrouillé.


## <a name="enable-a-baseline-policy"></a>Activer une stratégie de base 

Lorsque des stratégies de base sont en préversion, par défaut, elles ne sont pas activées. Si vous souhaitez activer une stratégie, vous devrez le faire manuellement. Dès que la disponibilité de cette fonctionnalité se généralise, les stratégies sont activées par défaut. Le changement de comportement prévu est la raison pour laquelle vous devez, par ailleurs, activer et désactiver une troisième option pour définir l’état d’une stratégie : **Activer automatiquement la stratégie à l'avenir**. En sélectionnant cette option, vous laissez à Microsoft le soin de décider quand activer une stratégie.      


**Pour activer une stratégie de base :**  

1. Connectez-vous au [portail Azure](https://portal.azure.com) en tant qu’administrateur général, administrateur de sécurité ou administrateur de l’accès conditionnel.

2. Dans la barre de navigation gauche du **portail Azure**, cliquez sur **Azure Active Directory**.

    ![Azure Active Directory](./media/active-directory-conditional-access-baseline-protection/02.png)

3. Dans la page **Azure Active Directory**, dans la section **Sécurité**, cliquez sur **Accès conditionnel**.

    ![Accès conditionnel](./media/active-directory-conditional-access-baseline-protection/05.png)

4. Dans la liste des stratégies, cliquez sur une stratégie commençant par **Stratégie de base :**. 

5. Pour activer la stratégie, cliquez sur **Utiliser la stratégie immédiatement**.

6. Cliquez sur **Enregistrer**. 
 


  
 

## <a name="what-you-should-know"></a>Ce que vous devez savoir 

Alors que la gestion de stratégies d’accès conditionnelles personnalisées nécessite une licence Azure AD Premium license, les stratégies de base sont disponibles dans toutes les éditions d’Azure AD.     

Les rôles d’annuaire inclus dans la stratégie de base sont les rôles Azure AD les plus privilégiés. 

Si vous disposez de comptes privilégiés qui sont utilisés dans vos scripts, vous devriez les remplacer par [Managed Service Identity (MSI)](./managed-service-identity/overview.md) ou par des [principaux de service avec certificats](../azure-resource-manager/resource-group-authenticate-service-principal.md). Pour contourner provisoirement le problème, vous pouvez exclure des comptes d’utilisateurs spécifiques de la stratégie de base. 

Les stratégies de base s’appliquent à des flux d’authentification hérités comme POP, IMAP ou un client Office pour ordinateur de bureau plus ancien. 




## <a name="next-steps"></a>Étapes suivantes

Pour savoir comment configurer une stratégie d’accès conditionnel, consultez [Prise en main de l’accès conditionnel dans Azure Active Directory](active-directory-conditional-access-azure-portal-get-started.md).

Si vous êtes prêt à configurer des stratégies d’accès conditionnel pour votre environnement, consultez les [Meilleures pratiques pour l’accès conditionnel dans Azure Active Directory](active-directory-conditional-access-best-practices.md). 
