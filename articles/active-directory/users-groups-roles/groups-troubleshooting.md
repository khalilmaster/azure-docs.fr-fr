---
title: Résolution des problèmes liés à l’appartenance dynamique des groupes | Microsoft Docs
description: Conseils pour résoudre les problèmes d’appartenance dynamique à des groupes dans Azure AD.
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.component: users-groups-roles
ms.topic: article
ms.date: 08/28/2017
ms.author: curtand
ms.reviewer: krbain
ms.custom: it-pro
ms.openlocfilehash: aaded644635ab93cef9323ad38d1d150b15fe743
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37449015"
---
# <a name="troubleshooting-dynamic-memberships-for-groups"></a>Résolution des problèmes liés à l’appartenance dynamique à des groupes
**J’ai configuré une règle sur un groupe, mais aucune appartenance n’est mise à jour dans le groupe**<br/>Vérifiez les valeurs des attributs d’utilisateur sur la règle : des utilisateurs satisfont-ils à la règle ? Si tout semble correct, attendez quelque temps avant que le groupe se remplisse. Selon la taille de votre client, cela peut prendre jusqu'à 24 heures pour que le groupe soit rempli la première fois ou après une modification de la règle.

**J’ai configuré une règle, mais les membres existants sur celle-ci sont à présent supprimés**<br/>Ce comportement est normal. Lors de l’activation ou de la modification d’une règle, les membres existants du groupe sont supprimés. Les utilisateurs provenant de l’évaluation de la règle sont ajoutés en tant que membres du groupe.     

**Je ne vois pas instantanément de changement d’appartenance quand j’ajoute ou quand je modifie une règle. Pourquoi ?**<br/>L’évaluation de l’appartenance dédiée est effectuée régulièrement dans le cadre d’un processus asynchrone exécuté en arrière-plan. La durée de l’opération est déterminée par le nombre d’utilisateurs dans votre annuaire et par la taille du groupe créé à la suite de la règle. Généralement, les annuaires contenant peu d’utilisateurs constatent les changements d’appartenance au groupe en moins de quelques minutes. Le renseignement des annuaires contenant de nombreux utilisateurs peut prendre plus de 30 minutes.

### <a name="next-steps"></a>Étapes suivantes
Ces articles fournissent des informations supplémentaires sur Azure Active Directory.

* [Gestion de l’accès aux ressources avec les groupes Azure Active Directory](../fundamentals/active-directory-manage-groups.md)
* [Index d’articles pour la gestion des applications dans Azure Active Directory](../active-directory-apps-index.md)
* [Qu’est-ce qu’Azure Active Directory ?](../fundamentals/active-directory-whatis.md)
* [Intégration des identités locales dans Azure Active Directory](../connect/active-directory-aadconnect.md)
