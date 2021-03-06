---
title: Bien démarrer avec l’API Reconnaissance vocale de Microsoft en JavaScript | Microsoft Docs
description: L’API Reconnaissance vocale de Microsoft dans Cognitive Services permet de développer des applications qui convertissent en permanence du contenu vocal en texte.
services: cognitive-services
author: zhouwangzw
manager: wolfma
ms.service: cognitive-services
ms.component: bing-speech
ms.topic: article
ms.date: 12/21/2017
ms.author: zhouwang
ms.openlocfilehash: 56c41fd7f6a00d80bc6bccd61894654e057e926e
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35368380"
---
# <a name="get-started-with-the-speech-recognition-api-in-javascript"></a>Bien démarrer avec l’API Reconnaissance vocale en JavaScript

Vous pouvez développer des applications qui convertissent du contenu vocal en texte à l’aide de l’API Reconnaissance vocale. La bibliothèque cliente JavaScript utilise le [protocole WebSocket du service Speech](../API-Reference-REST/websocketprotocol.md), qui vous permet de communiquer et de recevoir un texte transcrit simultanément. Cet article vous aide à commencer avec l’API Reconnaissance vocale en JavaScript.

## <a name="prerequisites"></a>Prérequis

### <a name="subscribe-to-the-speech-recognition-api-and-get-a-free-trial-subscription-key"></a>S’abonner à l’API Reconnaissance vocale et obtenir une clé d’abonnement d’essai

L’API Microsoft Speech fait partie de Cognitive Services. Vous pouvez obtenir des clés d’abonnement d’essai à partir de la page [d’abonnement à Cognitive Services](https://azure.microsoft.com/try/cognitive-services/). Après avoir sélectionné l’API Microsoft Speech, sélectionnez **Obtenir la clé API** pour obtenir la clé. Cette opération retourne une clé principale et une clé secondaire. Les deux clés étant liées au même quota, vous pouvez utiliser l’une ou l’autre.

> [!IMPORTANT]
> Obtenez une clé d’abonnement. Pour pouvoir utiliser des bibliothèques clientes Speech, vous devez disposer d’une [clé d’abonnement](https://azure.microsoft.com/try/cognitive-services/).

## <a name="get-started"></a>Prise en main

Cette section vous indique comment charger un exemple de page HTML. L’exemple se trouve dans notre [dépôt github](https://github.com/Azure-Samples/SpeechToText-WebSockets-Javascript). Vous pouvez **ouvrir l’exemple directement** à partir du dépôt ou **ouvrir l’exemple à partir d’une copie locale** du dépôt. 

> [!NOTE]
> Certains navigateurs bloquent l’accès au microphone si l’origine n’est pas sécurisée. Ainsi, nous vous recommandons d’héberger l’exemple ou votre application sur https afin qu’il fonctionne sur tous les navigateurs pris en charge. 

### <a name="open-the-sample-directly"></a>Ouvrir l’exemple directement

Obtenez une clé d’abonnement comme décrit ci-dessus. Ouvrez le [lien vers l’exemple](https://htmlpreview.github.io/?https://github.com/Azure-Samples/SpeechToText-WebSockets-Javascript/blob/preview/samples/browser/Sample.html). La page est chargée dans votre navigateur par défaut (restituée à l’aide [htmlPreview](https://github.com/htmlpreview/htmlpreview.github.com)).

### <a name="open-the-sample-from-a-local-copy"></a>Ouvrir l’exemple à partir d’une copie locale

Pour tester l’exemple localement, clonez ce dépôt :

```
git clone https://github.com/Azure-Samples/SpeechToText-WebSockets-Javascript
```

Compilez les sources TypeScript et regroupez-les par le biais d’une opération bundle/browserify en un seul fichier JavaScript ([npm](https://www.npmjs.com/) doit être installé sur votre ordinateur). Accédez à la racine du dépôt cloné et exécutez les commandes :

```
cd SpeechToText-WebSockets-Javascript && npm run bundle
```

Ouvrez `samples\browser\Sample.html` dans le navigateur de votre choix.

## <a name="next-steps"></a>Étapes suivantes

Vous trouverez plus d’informations sur la façon d’inclure le SDK dans votre propre page web [ici](https://github.com/Azure-Samples/SpeechToText-WebSockets-Javascript).

## <a name="remarks"></a>Remarques

- L’API Reconnaissance vocale prend en charge trois [modes de reconnaissance](../concepts.md#recognition-modes). Vous pouvez changer de mode en mettant à jour la fonction **Setup()** qui se trouve dans le fichier Sample.html. L’exemple définit le mode sur `Interactive` par défaut. Pour changer de mode, mettez à jour le paramètre `SR.RecognitionMode.Interactive` vers le mode souhaité. Par exemple, définissez le paramètre sur `SR.RecognitionMode.Conversation`.
- Pour obtenir la liste complète des langues prises en charge, consultez [Langues prises en charge](../API-Reference-REST/supportedlanguages.md).

## <a name="related-topics"></a>Rubriques connexes

- [Dépôt d’un exemple d’API Reconnaissance vocale en JavaScript](https://github.com/Azure-Samples/SpeechToText-WebSockets-Javascript)
- [Bien démarrer avec l’API REST](GetStartedREST.md)
