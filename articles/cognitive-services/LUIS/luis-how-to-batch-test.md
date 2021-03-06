---
title: Tester votre application LUIS par lot - Azure | Microsoft Docs
description: Utiliser les tests par lot Language Understanding (LUIS) pour rechercher des énoncés avec des intentions et des entités incorrectes.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 03/14/2018
ms.author: v-geberr
ms.openlocfilehash: 822fb1e2d5b13941527d242e8501b423bd6b81cb
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36265511"
---
# <a name="batch-testing"></a>Test par lot
 Le test par lot est un test complet sur votre modèle formé en cours afin d’en mesurer les performances dans LUIS. 

<a name="batch-testing"></a>
## <a name="import-a-dataset-file-for-batch-testing"></a>Importer un fichier de jeu de données pour le test par lot

1. Sélectionnez **Test** dans la partie supérieure de la barre et sélectionnez le **panneau Test par lot**.

    ![Lien Test par lot](./media/luis-how-to-batch-test/batch-testing-link.png)

2. Sélectionnez **Importer le jeu de données**. La boîte de dialogue **Importer le nouveau jeu de données** s’affiche. Choisissez **Sélectionner un fichier** et recherchez le fichier [JSON](luis-concept-batch-test.md#batch-file-format) qui ne contient *pas plus de 1 000* énoncés à tester.

    ![Importer le fichier de jeu de données](./media/luis-how-to-batch-test/batchtest-importset.png)

    Les erreurs d’importation sont signalées dans une barre de notification rouge en haut du navigateur. En cas d’erreur dans l’importation, aucun jeu de données n’est créé. Pour plus d’informations, consultez [Erreurs courantes](luis-concept-batch-test.md#common-errors-importing-a-batch).

3. Dans le champ **Nom du jeu de données**, entrez un nom pour votre fichier de jeu de données. Le fichier de jeu de données inclut un **tableau d’énoncés**, notamment *l’intention étiquetée* et les *entités*. Examinez [l’exemple de fichier par lot](luis-concept-batch-test.md#batch-file-format) pour connaître la syntaxe. 

4. Sélectionnez **Terminé**. Le fichier de jeu de données est ajouté.

## <a name="run-rename-export-or-delete-dataset"></a>Exécuter, renommer, exporter ou supprimer le jeu de données
Pour exécuter, renommer, exporter ou supprimer le jeu de données, utilisez les points de suspension (**...**) à la fin de la ligne du jeu de données.

![Actions du jeu de données](./media/luis-how-to-batch-test/batch-testing-options.png)

## <a name="run-a-batch-test-on-your-trained-app"></a>Exécuter un test par lot sur votre application formée

Pour exécuter le test, sélectionnez le nom du jeu de données. Lorsque le test est terminé, cette ligne affiche le résultat du test du jeu de données.

![Résultat du test par lot](./media/luis-how-to-batch-test/run-test.png)

Le jeu de données téléchargeable est le même fichier que celui chargé pour le test par lot.

|État|Signification|
|--|--|
|![Icône cercle vert pour test réussi](./media/luis-how-to-batch-test/batch-test-result-green.png)|Tous les énoncés sont réussis.|
|![Icône x rouge pour test ayant échoué](./media/luis-how-to-batch-test/batch-test-result-red.png)|Au moins une intention de l’énoncé ne correspond pas à la prédiction.|
|![Icône Prêt à tester](./media/luis-how-to-batch-test/batch-test-result-blue.png)|Le test est prêt à s’exécuter.|

<a name="access-batch-test-result-details-in-a-visualized-view"></a>
## <a name="view-batch-test-results"></a>Afficher les résultats du test par lot 
Pour passer en revue les résultats du test par lot, sélectionnez **Afficher les résultats**.

![Résultats du test par lot](./media/luis-how-to-batch-test/run-test-results.png)

<!--
 Select the **See results** link that appears after you run the test. A scatter graph known as an error matrix displays. The data points represent the utterances in the dataset. 

Green points indicate correct prediction, and red ones indicate incorrect prediction.

The filtering panel on the right side of the screen displays a list of all intents and entities in the app, with a green point for intents/entities that were predicted correctly in all dataset utterances, and a red point for those items with errors. Also, for each intent/entity, you can see the number of correct predictions out of the total utterances.

-->


<a name="filter-chart-results-by-intent-or-entity"></a>## Filtrer les résultats du graphique

Pour filtrer le graphique en fonction d’une intention ou d’une entité spécifique, sélectionnez l’intention ou l’entité sur le côté droit du panneau de filtrage. Les points de données et leur répartition sont mis à jour dans le graphique en fonction de votre sélection. 
 
![Résultat du test par lot visualisé](./media/luis-how-to-batch-test/filter-by-entity.png) 

<!--
## Investigate false sections
Data points on the **[False Positive][false-positive]** and **[False Negative][false-negative]** sections indicate errors, which should be investigated. If all data points are on the **[True Positive][true-positive]** and **[True Negative][true-negative]** sections, then your application's performance is perfect on this dataset.


The graph indicates [F-measure][f-measure], [recall][recall], and [precision][precision].  
-->
## <a name="view-single-point-utterance-data"></a>Afficher les données d’énoncé d’un point unique
Dans le graphique, pointez sur un point de données pour afficher le score de certitude de la prédiction. Sélectionnez un point de données pour récupérer l’énoncé correspondant dans la liste des énoncés au bas de la page. 

![Énoncé sélectionné](./media/luis-how-to-batch-test/selected-utterance.png)


<a name="relabel-utterances-and-retrain"></a>
<a name="false-test-results"></a>
## <a name="view-section-data"></a>Afficher les données de section
Dans le graphique à quatre sections, sélectionnez le nom de section, comme **Faux positif** en haut à droite du graphique. Sous le graphique, tous les énoncés dans cette section s’affichent dans une liste sous le graphique. 

![Énoncés sélectionnés par section](./media/luis-how-to-batch-test/selected-utterances-by-section.png)

Dans l’image précédente, l’énoncé `switch on` est étiqueté avec l’intention TurnAllOn, mais a reçu la prédiction d’intention None. Cela indique que l’intention TurnAllOn a besoin de davantage d’exemples d’énoncés pour rendre la prédiction attendue. 

Les deux sections du graphique en rouge indiquent les énoncés ne correspondant pas à la prédiction attendue. Cela indique les énoncés pour lesquels LUIS a besoin de davantage de formation. 

Les deux sections du graphique en vert correspondent à la prédiction attendue.

## <a name="next-steps"></a>Étapes suivantes

Si le test indique que votre application LUIS ne reconnaît pas les entités et les intentions correctes, vous pouvez travailler à l’amélioration des performances de votre application LUIS en étiquetant plus d’énoncés ou en ajoutant des fonctionnalités. 

* [Étiqueter des énoncés suggérés avec LUIS](Label-Suggested-Utterances.md) 
* [Utiliser les fonctionnalités pour améliorer les performances de votre application LUIS](luis-how-to-add-features.md) 
* [Comprendre le test par lot avec ce tutoriel](luis-tutorial-batch-testing.md)
* [Découvrir les concepts du test par lot](luis-concept-batch-test.md).

[true-positive]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-glossary#true-positive
[true-negative]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-glossary#true-negative
[false-positive]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-glossary#false-positive
[false-negative]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-glossary#false-negative
[f-measure]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-glossary#f-measure
[recall]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-glossary#recall
[precision]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-glossary#precision

