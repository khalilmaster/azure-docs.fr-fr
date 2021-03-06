---
title: Détecter des problèmes d’appareils dans la solution de surveillance à distance Azure | Microsoft Docs
description: Ce tutoriel montre comment utiliser des règles et des actions pour détecter automatiquement les problèmes d’appareils liés au seuil dans la solution de surveillance à distance.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 06/08/2018
ms.topic: tutorial
ms.custom: mvc
ms.openlocfilehash: 1e3eaeec1d2eae3c36f285a3e4c536657504cbb8
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37098479"
---
# <a name="tutorial-detect-issues-with-devices-connected-to-your-monitoring-solution"></a>Tutoriel : Détecter des problèmes liés aux appareils connectés à votre solution de surveillance

Dans ce tutoriel, vous configurez l’accélérateur de solution de surveillance à distance pour détecter les problèmes liés à vos appareils IoT connectés. Pour détecter les problèmes liés à vos appareils, vous ajoutez des règles qui génèrent des alertes sur le tableau de bord de la solution.

Pour présenter les règles et alertes, le tutoriel utilise un condenseur simulé. L’appareil de refroidissement est géré par une entreprise appelée Contoso et est connecté à l’accélérateur de solution de surveillance à distance. Contoso a déjà une règle qui génère une alerte critique lorsque la pression signalée par un appareil de refroidissement dépasse 298 psi. En tant qu’opérateur chez Contoso, vous souhaitez identifier les appareils de refroidissement avec des capteurs défectueux en recherchant des pics de pression initiale. Pour identifier ces appareils, vous ajoutez une règle qui génère une alerte d’avertissement lorsque la pression dans le refroidisseur dépasse 150 psi.

Vous devez également créer une alerte critique pour un refroidisseur lorsque, au cours des cinq dernières minutes, l’humidité moyenne dans l’appareil était supérieure à 80 % et la température de l’appareil était supérieure à 75 degrés Fahrenheit (24 degrés Celsius).

Dans ce tutoriel, vous avez appris à effectuer les opérations suivantes :

>[!div class="checklist"]
> * Afficher les règles dans votre solution
> * Créer une règle
> * Créer une règle avec plusieurs conditions
> * Modifier une règle existante
> * Activer et désactiver les règles de commutateur

## <a name="prerequisites"></a>Prérequis

Pour suivre ce tutoriel, vous avez besoin d’une instance déployée de l’accélérateur de solution de surveillance à distance dans votre abonnement Azure.

Si vous n’avez pas encore déployé l’accélérateur de solution de surveillance à distance, vous devez suivre le tutoriel [Déployer l’accélérateur de solution de surveillance à distance](quickstart-remote-monitoring-deploy.md).

## <a name="view-the-existing-rules"></a>Afficher les règles existantes

La page **Règles** de l’accélérateur de solution affiche une liste de toutes les règles actuellement proposées :

[![Page de règles](./media/iot-accelerators-remote-monitoring-automate/rulesactions_v2-inline.png)](./media/iot-accelerators-remote-monitoring-automate/rulesactions_v2-expanded.png#lightbox)

Pour afficher uniquement les règles qui s’appliquent aux appareils de refroidissement, appliquez un filtre :

[![Filtrer la liste des règles](./media/iot-accelerators-remote-monitoring-automate/rulesactionsfilter_v2-inline.png)](./media/iot-accelerators-remote-monitoring-automate/rulesactionsfilter_v2-expanded.png#lightbox)

Vous pouvez afficher plus d’informations sur une règle et modifier celle-ci lorsque vous la sélectionnez dans la liste :

[![Afficher les détails de la règle](./media/iot-accelerators-remote-monitoring-automate/rulesactionsdetail_v2-inline.png)](./media/iot-accelerators-remote-monitoring-automate/rulesactionsdetail_v2-expanded.png#lightbox)

Pour désactiver, activer ou supprimer une ou plusieurs règles, sélectionnez des règles dans la liste :

[![Sélectionner plusieurs règles](./media/iot-accelerators-remote-monitoring-automate/rulesactionsmultiselect_v2-inline.png)](./media/iot-accelerators-remote-monitoring-automate/rulesactionsmultiselect_v2-expanded.png#lightbox)

## <a name="create-a-rule"></a>Créer une règle

Pour ajouter une nouvelle règle qui génère un avertissement lorsque la pression dans un appareil de refroidissement dépasse 150 psi, choisissez **Nouvelle règle**. Utilisez les valeurs suivantes pour créer la règle :

| Paramètre          | Valeur                                 |
| ---------------- | ------------------------------------- |
| Nom de la règle        | Avertissement de refroidissement                       |
| Description      | Pression de refroidissement supérieure à 150 psi |
| Groupe d’appareils     | Groupe d’appareils de **refroidissement**             |
| Calcul      | Immédiat                               |
| Champ de condition 1| pression                              |
| Opérateur de condition 1 | Supérieur à                      |
| Valeur de condition 1    | 150                               |
| Niveau de gravité  | Avertissement                               |

[![Créer la règle d’avertissement](./media/iot-accelerators-remote-monitoring-automate/rulesactionsnewrule_v2-inline.png)](./media/iot-accelerators-remote-monitoring-automate/rulesactionsnewrule_v2-expanded.png#lightbox)

Pour enregistrer la nouvelle règle, cliquez sur **Appliquer**.

Vous pouvez voir le moment où la règle est déclenchée dans la page **Règles** ou la page **Tableau de bord** :

[![Règle d’avertissement déclenchée](./media/iot-accelerators-remote-monitoring-automate/warningruletriggered-inline.png)](./media/iot-accelerators-remote-monitoring-automate/warningruletriggered-expanded.png#lightbox)

## <a name="create-a-rule-with-multiple-conditions"></a>Créer une règle avec plusieurs conditions

Pour créer une règle avec plusieurs conditions qui génère une alerte critique quand l’humidité moyenne de l’appareil de refroidissement des cinq dernières minutes est supérieure à 80 % et que la température moyenne de l’appareil de refroidissement est supérieure à 75 degrés Fahrenheit (24 degrés Celsius), choisissez Nouvelle règle. Utilisez les valeurs suivantes pour créer la règle :

| Paramètre          | Valeur                                 |
| ---------------- | ------------------------------------- |
| Nom de la règle        | Humidité et température critiques de l’appareil de refroidissement    |
| Description      | L’humidité et la température sont critiques |
| Groupe d’appareils     | Groupe d’appareils de **refroidissement**             |
| Calcul      | Moyenne                               |
| Période      | 5                                     |
| Champ de condition 1| humidité                              |
| Opérateur de condition 1 | Supérieur à                      |
| Valeur de condition 1    | 80                                |
| Niveau de gravité  | Critique                              |

[![Créer une règle à plusieurs conditions règle - première partie](./media/iot-accelerators-remote-monitoring-automate/rulesactionsnewrule_mult_v2-inline.png)](./media/iot-accelerators-remote-monitoring-automate/rulesactionsnewrule_mult_v2-expanded.png#lightbox)

Pour ajouter la seconde condition, cliquez sur « + Ajouter une condition ». Utilisez les valeurs suivantes pour la nouvelle condition :

| Paramètre          | Valeur                                 |
| ---------------- | ------------------------------------- |
| Champ de condition 2| température                           |
| Opérateur de condition 2 | Supérieur à                      |
| Valeur de condition 2    | 75                                |

[![Créer une règle à plusieurs conditions règle - deuxième partie](./media/iot-accelerators-remote-monitoring-automate/rulesactionsnewrule_mult_cond2_v2-inline.png)](./media/iot-accelerators-remote-monitoring-automate/rulesactionsnewrule_mult_cond2_v2-expanded.png#lightbox)

Pour enregistrer la nouvelle règle, cliquez sur **Appliquer**.

Vous pouvez voir le moment où la règle est déclenchée dans la page **Règles** ou la page **Tableau de bord** :

[![Déclenchement d’une règle à plusieurs conditions](./media/iot-accelerators-remote-monitoring-automate/criticalruletriggered-inline.png)](./media/iot-accelerators-remote-monitoring-automate/criticalruletriggered-expanded.png#lightbox)

## <a name="edit-an-existing-rule"></a>Modifier une règle existante

Pour modifier une règle existante, sélectionnez-la dans la liste des règles et cliquez sur **Modifier** :

[![Modifier une règle](./media/iot-accelerators-remote-monitoring-automate/rulesactionsedit_v2-inline.png)](./media/iot-accelerators-remote-monitoring-automate/rulesactionsedit_v2-expanded.png#lightbox)

## <a name="disable-a-rule"></a>Désactiver une règle

Pour désactiver temporairement une règle, vous pouvez la désactiver dans la liste des règles. Sélectionnez la règle à désactiver, puis choisissez **Désactiver**. L’**état** de la règle dans la liste change pour indiquer que la règle est désactivée. Vous pouvez réactiver une règle que vous avez précédemment désactivée à l’aide de la même procédure.

[![Désactiver une règle](./media/iot-accelerators-remote-monitoring-automate/rulesactionsdisable-inline.png)](./media/iot-accelerators-remote-monitoring-automate/rulesactionsdisable-expanded.png#lightbox)

Vous pouvez activer et désactiver plusieurs règles en même temps en sélectionnant plusieurs règles dans la liste.

<!-- ## Delete a rule

To permanently delete a rule, choose the rule in the list of rules and then choose **Delete**.

You can delete multiple rules at the same time if you select multiple rules in the list.-->

## <a name="clean-up-resources"></a>Supprimer des ressources

Si vous envisagez de passer à l’étape suivante du tutoriel, laissez l’accélérateur de solution de surveillance à distance déployé. Pour réduire les coûts d’exécution de l’accélérateur de solution pendant que vous ne l’utilisez pas, vous pouvez arrêter les appareils simulés dans le panneau des paramètres :

[![Suspendre les données de télémétrie](./media/iot-accelerators-remote-monitoring-automate/togglesimulation-inline.png)](./media/iot-accelerators-remote-monitoring-automate/togglesimulation-expanded.png#lightbox)

Vous pouvez redémarrer les appareils simulés lorsque vous êtes prêt à commencer le tutoriel suivant.

Si vous n’avez plus besoin l’accélérateur de solution, supprimez-le à partir de la page [Solutions approvisionnées](https://www.azureiotsolutions.com/Accelerators#dashboard) :

![Supprimer la solution](media/iot-accelerators-remote-monitoring-automate/deletesolution.png)

## <a name="next-steps"></a>Étapes suivantes

Ce tutoriel vous a montré comment utiliser la page **Règles** dans l’accélérateur de solution de surveillance à distance pour créer et gérer des règles qui déclenchent des alertes dans la solution. Pour découvrir comment utiliser l’accélérateur de solution pour gérer et configurer vos appareils connectés, passez au tutoriel suivant.

> [!div class="nextstepaction"]
> [Configurer et gérer les appareils connectés à votre solution de surveillance](iot-accelerators-remote-monitoring-manage.md)