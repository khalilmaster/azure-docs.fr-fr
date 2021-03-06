---
title: Qu’est-ce que le contrôle d’accès en fonction du rôle (RBAC) dans Azure ? | Microsoft Docs
description: Découvrez le contrôle d’accès en fonction du rôle (RBAC) dans Azure. Utilisez les attributions de rôles pour contrôler l’accès aux ressources dans Azure.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.assetid: 8f8aadeb-45c9-4d0e-af87-f1f79373e039
ms.service: role-based-access-control
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/02/2018
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: 4dcfb71e0adb05922603715e4dbcbdb243305927
ms.sourcegitcommit: e0834ad0bad38f4fb007053a472bde918d69f6cb
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37438194"
---
# <a name="what-is-role-based-access-control-rbac"></a>Qu’est-ce que le contrôle d’accès en fonction du rôle (RBAC) ?

Il est vital pour toute organisation qui utilise le cloud de pouvoir gérer les accès aux ressources situées dans cloud. Le contrôle d’accès basé sur un rôle (RBAC) permet de gérer les utilisateurs ayant accès aux ressources Azure, les modes d’utilisation des ressources par ces derniers et les zones auxquelles ils ont accès.

Le RBAC est un système d’autorisation basé sur [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) qui propose une gestion affinée des accès des ressources dans Azure. Avec le contrôle d’accès en fonction du rôle, vous pouvez séparer les tâches au sein de votre équipe et accorder aux utilisateurs uniquement les accès nécessaires pour accomplir leur travail. Plutôt que de donner à tous des autorisations illimitées au sein de votre abonnement ou de vos ressources Azure, vous pouvez autoriser uniquement certaines actions sur une étendue donnée.

## <a name="what-can-i-do-with-rbac"></a>Que puis-je faire avec le contrôle d’accès en fonction du rôle (RBAC) ?

Voici quelques exemples de ce que vous pouvez faire avec le contrôle d’accès en fonction du rôle (RBAC) :

- Autoriser un utilisateur à gérer des machines virtuelles dans un abonnement et un autre utilisateur à gérer des réseaux virtuels
- Permettre à un groupe d’administrateurs de base de données de gérer des bases de données SQL dans un abonnement
- Permettre à un utilisateur à gérer toutes les ressources dans un groupe de ressources, telles que des machines virtuelles, des sites Web et des sous-réseaux
- Permettre à une application d’accéder à toutes les ressources d’un groupe de ressources

## <a name="how-rbac-works"></a>Fonctionnement du le contrôle d’accès en fonction du rôle (RBAC)

Les attributions de rôles vous permettent de contrôler l’accès aux ressources à l’aide du contrôle d’accès en fonction du rôle (RBAC). Ce concept est essentiel à comprendre : il s’agit de la façon dont les autorisations sont appliquées. Une attribution de rôle se compose de trois éléments : un principal de sécurité, une définition de rôle et une étendue.

### <a name="security-principal"></a>Principal de sécurité

Un *principal de sécurité* est un objet qui représente un utilisateur, un groupe ou un principal de service demandant l’accès aux ressources Azure.

![Principe de sécurité d’une attribution de rôle](./media/overview/rbac-security-principal.png)

- Utilisateur : personne disposant d’un profil dans Azure Active Directory. Vous pouvez également attribuer des rôles aux utilisateurs dans les autres locataires. Pour plus d’informations sur les utilisateurs des autres organisations, consultez [Azure Active Directory B2B](/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b).
- Groupe : ensemble d’utilisateurs créés dans Azure Active Directory. Lorsque vous attribuez un rôle à un groupe, vous l’attribuez également à tous les utilisateurs de ce groupe. 
- Principal de service : identité de sécurité utilisée par des applications ou des services permettant d’accéder aux ressources Azure spécifiques. Vous pouvez la considérer comme une *identité utilisateur* (nom d’utilisateur, mot de passe ou certificat) pour une application.

### <a name="role-definition"></a>Définition de rôle

Une *définition de rôle* est une collection d’autorisations. On l’appelle parfois simplement *rôle*. Une définition de rôle répertorie les opérations qui peuvent être effectuées, telles que lire, écrire et supprimer. Les rôles peuvent être de haut niveau, comme propriétaire, ou spécifiques, comme lecteur de machines virtuelles.

![Définition de rôle pour une attribution de rôle](./media/overview/rbac-role-definition.png)

Azure inclut plusieurs [rôles intégrés](built-in-roles.md) que vous pouvez utiliser. Voici les quatre rôles fondamentaux intégrés : Les trois premiers s’appliquent à tous les types de ressources.

- [Propriétaire](built-in-roles.md#owner) : dispose d’un accès total à toutes les ressources, ainsi que le droit de déléguer l’accès à d’autres personnes.
- [Contributeur](built-in-roles.md#contributor) : peut créer et gérer tous les types de ressources Azure, mais ne peut pas accorder l’accès à d’autres personnes.
- [Lecteur](built-in-roles.md#reader) : peut consulter les ressources Azure existantes.
- [Administrateur de l’accès utilisateur](built-in-roles.md#user-access-administrator) : vous permet de gérer l’accès des utilisateurs aux ressources Azure.

Les autres rôles intégrés permettent de gérer des ressources Azure spécifiques. Par exemple, le rôle de [contributeur de machine virtuelle](built-in-roles.md#virtual-machine-contributor) permet à l’utilisateur de créer et gérer des machines virtuelles. Si les rôles intégrés ne répondent pas aux besoins spécifiques de votre organisation, vous pouvez créer vos propres [rôles personnalisés](custom-roles.md).

Azure propose des opérations de données (actuellement en version préliminaire) qui vous permettent d’accorder l’accès aux données au sein d’un objet. Par exemple, si un utilisateur dispose d’un accès en lecture aux données d’un compte de stockage, il peut lire les objets blob ou les messages de ce compte de stockage. Pour plus d’informations, consultez [Comprendre les définitions de rôle](role-definitions.md).

### <a name="scope"></a>Étendue

*Étendue* constitue la limite à laquelle l’accès s’applique. Lorsque vous attribuez un rôle, vous pouvez restreindre les actions autorisées en définissant une étendue. Cette possibilité s’avère utile si vous voulez par exemple attribuer le rôle de [contributeur de site web](built-in-roles.md#website-contributor) à quelqu’un, mais seulement pour un groupe de ressources.

Dans Azure, vous pouvez spécifier une étendue à plusieurs niveaux : abonnement, groupe de ressources ou ressource. Les étendues sont structurées dans une relation parent-enfant dans laquelle chaque enfant n’aura qu’un seul parent.

![Étendue pour une attribution de rôle](./media/overview/rbac-scope.png)

L’accès que vous attribuez dans une étendue parente est hérité dans les étendues enfants. Par exemple :

- Si vous affectez le rôle de [lecteur](built-in-roles.md#reader) à un groupe au niveau de l’étendue de l’abonnement, les membres de ce groupe peuvent afficher chaque groupe de ressources et la ressource dans l’abonnement.
- Si vous affectez le rôle de [contributeur](built-in-roles.md#contributor) à une application au niveau du groupe de ressources, il peut gérer tous les types de ressources dans ce groupe de ressources, mais aucun groupe de ressources dans l’abonnement.

Azure comprend également une étendue au-dessus des abonnements appelée [groupes de gestion](../azure-resource-manager/management-groups-overview.md), disponible en version préliminaire. Les groupes de gestion sont un moyen de gérer plusieurs abonnements. Lorsque vous spécifiez l’étendue de la fonctionnalité RBAC, vous pouvez spécifier un groupe de gestion ou un abonnement, un groupe de ressources ou une hiérarchie des ressources.

### <a name="role-assignment"></a>Attribution de rôle

Une *attribution de rôle* est le processus de liaison d’une définition de rôle à un utilisateur, un groupe ou un principal de service au niveau d’une étendue spécifique pour accorder des accès. La création d’une attribution de rôle permet d’accorder un accès, qui peut être révoqué par la suppression d’une attribution de rôle.

Le diagramme suivant montre un exemple d’attribution de rôle. Dans cet exemple, le rôle de [contributeur](built-in-roles.md#contributor) a été attribué au groupe Marketing pour le groupe de ressources pharma-sales. Cela signifie que les utilisateurs du groupe Marketing peuvent créer ou gérer n’importe quelle ressource Azure dans le groupe de ressources pharma-sales. Les utilisateurs Marketing n’ont pas accès aux ressources en dehors du groupe de ressources pharma-sales, sauf si elles font partie d’une autre attribution de rôle.

![Attribution de rôle pour contrôler les accès](./media/overview/rbac-overview.png)

Vous pouvez créer des attributions de rôles à l’aide du Portail Azure, d’Azure CLI, d’Azure PowerShell, des kits de développement logiciel (SDK) Azure ou d’API REST. Vous pouvez avoir jusqu’à 2000 attributions de rôles dans chaque abonnement. Pour créer et supprimer des attributions de rôles, les utilisateurs doivent disposer de l’autorisation `Microsoft.Authorization/roleAssignments/*`. Cette autorisation est accordée par le biais des rôles [Propriétaire](built-in-roles.md#owner) ou [Administrateur de l’accès utilisateur](built-in-roles.md#user-access-administrator).

## <a name="next-steps"></a>Étapes suivantes

- [Démarrage rapide : accorder l’accès à un utilisateur avec RBAC et le Portail Azure](quickstart-assign-role-user-portal.md)
- [Manage access using RBAC and the Azure portal](role-assignments-portal.md) (Gérer les accès à l’aide du contrôle d’accès en fonction du rôle et du Portail Azure)
- [Comprendre les différents rôles dans Azure](rbac-and-directory-admin-roles.md)
