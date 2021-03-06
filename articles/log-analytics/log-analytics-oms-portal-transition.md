---
title: Migration du portail OMS vers Azure | Microsoft Docs
description: Le portail OMS devient caduque et toutes ses fonctionnalités migrent vers le portail Azure. Cet article fournit des détails sur cette transition.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/11/2018
ms.author: bwren
ms.component: na
ms.openlocfilehash: e47e8cbd209ea34317ca9b176a2c4b0fef10a2b2
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37133407"
---
# <a name="oms-portal-moving-to-azure"></a>Migration du portail OMS vers Azure
Merci d’utiliser le portail OMS. Votre soutien nous encourage et nous continuons d’investir fortement dans nos services de monitoring et de gestion. Un des commentaires fréquents des clients concerne le besoin d’une expérience utilisateur unique pour surveiller et gérer à la fois localement et les charges de travail Azure. Vous savez probablement le portail Azure est le hub de tous les services Azure et propose une riche expérience de gestion avec des fonctionnalités telles que des tableaux de bord pour épingler les ressources, la recherche intelligente pour rechercher des ressources et le balisage pour gérer les ressources. Pour consolider et rationaliser le flux de travail de monitoring et de gestion, nous avons commencé à ajouter les fonctionnalités du portail OMS dans le portail Azure. Nous sommes heureux d’annoncer que la plupart des fonctionnalités du portail OMS font désormais partie du portail Azure. En fait, certaines des nouvelles fonctionnalités comme Traffic Manager sont disponibles uniquement dans le portail Azure. Il existe encore quelques lacunes, celle ayant le plus d’impact concerne cinq solutions toujours en cours de migration vers le portail Azure. Si vous n’utilisez pas ces fonctionnalités, vous pourrez effectuer tout ce que vous faisiez dans le portail OMS avec le portail Azure, et bien plus encore. Si ce n’est déjà fait, nous vous recommandons de commencer à utiliser le portail Azure aujourd’hui ! 

Nous prévoyons de combler les lacunes restantes entre les deux portails d’ici le mois d’août 2018. Conformément aux commentaires des clients, nous communiquerons la chronologie de caducité du portail OMS. Nous sommes heureux de migrer vers le portail Azure et pensons que la transition sera facile. Mais nous comprenons que les changements sont difficiles et peuvent entraîner des perturbations. Envoyez vos questions, commentaires ou problèmes à LAUpgradeFeedback@microsoft.com. Le reste de cet article passe en revue les scénarios clés, les lacunes actuelles et la feuille de route pour cette transition. 


## <a name="what-will-change"></a>Qu’est-ce qui changera ? 
Les modifications suivantes sont annoncées avec l’abandon du portail OMS. Chacun de ces changements est décrit en détail dans les sections ci-dessous.

- La nouvelle expérience de gestion des alertes remplacera la solution Alert Management.
- La gestion de l’accès utilisateur s’effectuera dans le portail Azure avec le contrôle d’accès en fonction du rôle Azure.
- Application Insights Connector n’est plus nécessaire dans la mesure où la même fonctionnalité peut être activée via les requêtes portant sur plusieurs espaces de travail.
- L’application mobile OMS sera déconseillée. 
- La solution NSG est remplacée par une fonctionnalité améliorée disponible via la solution Traffic Analytics.



## <a name="current-known-gaps"></a>Lacunes actuelles connues 
Il existe actuellement des lacunes des fonctionnalités qui vous obligent à toujours utiliser le portail OMS. Ces lacunes sont en passe d’être comblées, et ce document sera mis à jour en conséquence. Vous devez également vous reporter à [Mises à jour Azure](https://azure.microsoft.com/updates/?product=log-analytics) pour consulter les annonces sur les extensions et les modifications.

- Les solutions suivantes ne sont pas encore totalement opérationnelles dans le portail Azure. Vous devez continuer à utiliser ces solutions dans le portail classique jusqu’à ce qu’elles soient mises à jour.

    - Solutions d’analytique Windows ([Upgrade Readiness](https://technet.microsoft.com/itpro/windows/deploy/upgrade-readiness-get-started), [Device Health](https://docs.microsoft.com/windows/deployment/update/device-health-monitor) et [Update Compliance](https://technet.microsoft.com/itpro/windows/manage/update-compliance-get-started))
    - [DNS Analytics](log-analytics-dns.md) 
    - [Surface Hub](log-analytics-surface-hubs.md)

-  Pour accéder à la ressource Log Analytics dans Azure, l’utilisateur doit se voir accorder l’accès via [l’accès en fonction du rôle Azure](#user-access-and-role-migration).
- Les planifications de mise à jour qui ont été créées avec le portail OMS ne seront pas reportées dans les déploiements de mise à jour planifiée ou l’historique de travail de mise à jour du tableau de bord de gestion Mise à jour dans le portail Azure. Cette lacune devrait être comblée à la fin du mois de juin 2018.
- La fonctionnalité d’aperçu des journaux personnalisés ne peut être activée que par le biais du portail OMS. À la fin du mois de juin 2018, cette fonctionnalité sera automatiquement activée pour tous les espaces de travail.
 
## <a name="what-should-i-do-now"></a>Que dois-je savoir ?  
Vous devez vous référer à [Questions courantes sur la transition du portail OMS vers le portail Azure pour les utilisateurs Log Analytics](../log-analytics/log-analytics-oms-portal-faq.md) pour plus d’informations sur la transition vers le portail Azure. Si les [lacunes décrites ci-dessus](#current-known-gaps) ne s’appliquent pas à votre environnement, vous devriez alors envisager d’utiliser le portail Azure pour votre expérience principale. Envoyez vos commentaires, questions ou problèmes à LAUpgradeFeedback@microsoft.com.

## <a name="changes-to-alerts"></a>Changements dans les alertes 

### <a name="alert-extension"></a>Extension d’alerte  
Les alertes sont en train d’être [étendues dans le portail Azure](../monitoring-and-diagnostics/monitoring-alerts-extend.md). Une fois cette opération terminée, les actions de gestion des alertes seront disponibles uniquement dans le portail Azure. Les alertes existantes continueront à être répertoriées dans le portail OMS. Si vous accédez aux alertes par programme à l’aide de l’API REST Log Analytics Alert ou du modèle Log Analytics Alert Resource, vous devrez utiliser des groupes d’actions à la place des actions dans vos appels d’API, modèles Azure Resource Manager et commandes PowerShell.

### <a name="alert-management-solution"></a>solution de gestion des alertes
Au lieu de la [solution de gestion des alertes](log-analytics-solution-alert-management.md), vous pouvez utiliser [l’interface d’alerte unifiée d’Azure Monitor](../monitoring-and-diagnostics/monitoring-overview-unified-alerts.md) pour visualiser et gérer vos alertes. Cette nouvelle expérience regroupe les alertes à partir de plusieurs sources dans Azure, notamment les alertes de journal de Log Analytics. Vous pouvez voir les distributions de vos alertes, tirer parti du regroupement automatique des alertes associées via des groupes intelligents et afficher les alertes entre plusieurs abonnements lors de l’application de filtres riches. Toutes ces fonctionnalités sont disponibles en préversion à partir du 4 juin 2018. La solution de gestion des alertes ne sera pas disponible dans le portail Azure. 

Les données collectées par la solution Alert Management (enregistrements avec un type d’alerte) se trouvent toujours dans Log Analytics tant que la solution est installée pour l’espace de travail. À partir du mois d’août 2018, le streaming des alertes à partir de la génération d’alertes unifiée dans les espaces de travail sera activée, remplaçant cette fonctionnalité. Certaines modifications du schéma sont attendues et seront annoncées à une date ultérieure.

## <a name="user-access-and-role-migration"></a>Accès utilisateur et migration du rôle
La gestion de l’accès au portail Azure est plus riche et plus puissante que la gestion de l’accès dans le portail OMS, mais elle nécessite quelques conversions. Consultez [Gérer les espaces de travail](log-analytics-manage-access.md#manage-accounts-and-users) pour plus d’informations sur la gestion de l’accès dans Log Analytics.

À partir du 25 juin et durant le mois de juillet, la conversion automatique des autorisations du contrôle d’accès à partir du portail OMS vers les autorisations du portail Azure débutera. Une fois la conversion terminée, la section de gestion utilisateur du portail OMS acheminera les utilisateurs vers le contrôle d’accès (IAM) dans Azure. 

Lors de la conversion, le système vérifiera chaque utilisateur ou groupe de sécurité qui dispose d’autorisations dans le portail OMS et déterminera s’il peut avoir le même niveau ou les mêmes autorisations dans Azure. Si des autorisations sont manquantes, il affectera les rôles suivants pour les solutions et les espaces de travail appropriés.

| Autorisation dans le portail OMS | Rôle Azure |
|:---|:---|
| Lecture seule | Lecteur Log Analytics |
| Contributeur | Contributeur Log Analytics |
| Administrateur | Propriétaire |

Pour s’assurer qu’aucune autorisation excessive n’est attribuée aux utilisateurs, le système n’affectera pas automatiquement ces autorisations au niveau du groupe de ressources. Par conséquent, les administrateurs de l’espace de travail doivent s’attribuer manuellement le rôle _propriétaire_ ou _collaborateur_ au niveau du groupe de ressources ou de l’abonnement pour effectuer les actions suivantes.

- Ajouter ou supprimer des solutions
- Définir de nouvelles vues personnalisées
- Gérer les alertes 

Dans certains cas, la conversion automatique ne peut pas appliquer d’autorisation et invite l’administrateur à attribuer manuellement des autorisations.

## <a name="oms-mobile-app"></a>Application mobile OMS
L’application mobile OMS deviendra caduque, ainsi que le portail OMS. Au lieu de l’application mobile OMS, pour accéder aux informations sur votre infrastructure informatique, tableaux de bord et requêtes enregistrées, vous pouvez accéder au portail Azure directement à partir de votre navigateur sur votre appareil mobile. Pour obtenir des alertes, vous devez configurer des [groupes d’actions Azure](../monitoring-and-diagnostics/monitoring-action-groups.md) pour recevoir des notifications sous la forme d’un appel vocal ou d’un SMS

## <a name="application-insights-connector-and-solution"></a>Application Insights Connector et solution
[Application Insights Connector](log-analytics-app-insights-connector.md) permet d’importer des données Application Insights dans un espace de travail Log Analytics. Cette duplication des données était nécessaire pour activer la visibilité sur les données d’infrastructure et d’application.

Avec la prise en charge des [requêtes sur plusieurs ressources](log-analytics-cross-workspace-search.md), il n’est plus nécessaire de dupliquer les données. Par conséquent, la solution Application Insights existante sera déconseillée. À partir du mois de juillet, vous ne pourrez plus lier de nouvelles ressources Application Insights aux espaces de travail Log Analytics. Les liens et les tableaux de bord existants continueront de fonctionner jusqu’en novembre 2018.


## <a name="azure-network-security-group-analytics"></a>Azure Network Security Group Analytics
La [solution Azure Network Security Group Analytics](log-analytics-azure-networking-analytics.md#azure-network-security-group-analytics-solution-in-log-analytics) sera remplacée par [Traffic Analytics](https://azure.microsoft.com/en-in/blog/traffic-analytics-in-preview/) récemment lancé qui offre une visibilité sur l’activité des utilisateurs et des applications sur les réseaux cloud. Traffic Analytics vous permet d’auditer l’activité réseau de votre organisation, de sécuriser les applications et les données, d’optimiser les performances de la charge de travail et de rester en conformité. 

Cette solution analyse les journaux de flux NSG et fournit des insights sur les éléments suivants.

- Les flux de trafic sur vos réseaux entre Azure et Internet, les régions de cloud public, les réseaux virtuels et les sous-réseaux.
- Les applications et les protocoles sur votre réseau, sans renifleur ni appliance de collection de flux dédiée.
- Les principaux consommateurs, les applications bavardes, les conversations de machine virtuelle dans le cloud, les zones réactives du trafic.
- Les sources et destinations du trafic entre les réseaux virtuels, les relations entre les services métier critiques et les applications.
- La sécurité, comme le trafic malveillant, les ports ouverts sur Internet, les applications ou machines virtuelles tentant d’accéder à Internet.
- L’utilisation de la capacité, qui vous aide à éliminer les problèmes de sur-provisionnement ou de sous-utilisation.

Vous pouvez continuer à utiliser les Paramètres de diagnostics pour envoyer les journaux NSG à Log Analytics, afin que vos recherches enregistrées, alertes, tableaux de bord existants continuent de fonctionner. Les clients qui ont déjà installé la solution peuvent continuer à l’utiliser jusqu’à nouvel ordre. À partir du 20 juin, la solution d’analytique Network Security Group sera supprimée de la place de marché et mise à disposition par le biais de la Communauté sous forme de [modèle de démarrage rapide Azure](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Operationalinsights).

## <a name="next-steps"></a>Étapes suivantes
- Consultez [Questions courantes sur la transition du portail OMS vers le portail Azure pour les utilisateurs Log Analytics](log-analytics-oms-portal-faq.md) pour obtenir des conseils sur la transition depuis le portail OMS vers le portail Azure.