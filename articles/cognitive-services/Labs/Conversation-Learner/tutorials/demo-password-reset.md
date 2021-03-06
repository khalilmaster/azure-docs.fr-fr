---
title: Application Apprenant de conversation de démonstration, réinitialisation de mot de passe – Microsoft Cognitive Services | Microsoft Docs
titleSuffix: Azure
description: Découvrez comment créer une application Apprenant de conversation de démonstration.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 24d61787a79ee1a1a9737c417aa966cc8fd75930
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35369584"
---
# <a name="demo-password-reset"></a>Démonstration : réinitialisation de mot de passe
Cette démonstration illustre un robot de support technique simple qui peut aider à effectuer des réinitialisations de mot de passe. 

Il montre comment Apprenant de conversation peut apprendre des flux de dialogue non triviaux, des séquences à plusieurs tours, y compris une classe hors domaine. Cette démonstration n’utilise pas de code ni d’entités.

## <a name="requirements"></a>Configuration requise
Ce didacticiel requiert l’exécution du robot de réinitialisation de mot de passe.

    npm run demo-password

### <a name="open-the-demo"></a>Ouvrir la démonstration

Dans la liste des applications de l’interface utilisateur web, cliquez sur le didacticiel de démonstration Réinitialisation de mot de passe. 

### <a name="actions"></a>Actions

Nous avons créé un ensemble d’actions où l’utilisateur recherche de l’aide concernant son mot de passe. Des solutions y sont également proposées.

![](../media/tutorial_pw_reset_actions.PNG)

### <a name="training-dialogs"></a>Dialogues d’apprentissage

Il existe un certain nombre de dialogues d’apprentissage. Il existe également des démonstrations d’une procédure de classe hors domaine--par exemple, des demandes d’utilisateurs telles que « itinéraire » sont hors domaine ; le robot a reçu des exemples de quelques demandes hors domaine et peut répondre avec « Je ne peux pas vous aider pour cette demande ».

![](../media/tutorial_pw_reset_entities.PNG)

À titre d’exemple, nous allons tester une session d’apprentissage.

1. Cliquez sur Dialogues d’apprentissage, puis sur Nouveau dialogue d’apprentissage.
1. Entrez « J’ai perdu mon mot de passe ».
2. Cliquez sur Action de score.
3. Cliquez pour sélectionner « Est-ce que cela concerne votre compte local ou votre compte Microsoft ? ».
4. Entrez « compte local ».
5. Cliquez sur Actions de score.
3. Cliquez pour sélectionner « Quelle version de Windows avez-vous ? ».
4. Entrez « Windows 8 ».
5. Cliquez sur Actions de score.
6. Sélectionnez « SOLUTION : comment réinitialiser le mot de passe sur Windows 8 ».
4. Cliquez sur Apprentissage terminé.

Essayons de voir comment le robot peut apprendre une classe hors domaine.

1. Cliquez sur Dialogues d’apprentissage, puis sur Nouveau dialogue d’apprentissage.
1. Entrez « recherche web ».
    - Il s’agit d’un exemple de classe hors domaine. 
2. Cliquez sur Action de score.
3. Cliquez pour sélectionner « Je ne peux pas vous aider pour cette demande ».
    - Notez que le score pour cette option est actuellement faible. Mais avec un peu plus d’apprentissage, le score sera plus élevé.
4. Cliquez sur Apprentissage terminé.

Vous savez maintenant comment créer une démonstration de support technique de base, et vous avez appris de quelle façon elle peut apprendre à fournir des solutions et également gérer des requêtes différentes des exemples.

## <a name="next-steps"></a>Étapes suivantes

> [!div class="nextstepaction"]
> [Démonstration : commande de pizza](./demo-pizza-order.md)
