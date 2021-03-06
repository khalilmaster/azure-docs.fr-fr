---
title: Collaborer avec d’autres contributeurs sur les applications LUIS dans Azure | Microsoft Docs
description: Découvrez comment collaborer avec d’autres contributeurs sur les applications Language Understanding (LUIS).
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 03/05/2018
ms.author: v-geberr
ms.openlocfilehash: c0451f7621a3c18dbf365f3a03934924c030092f
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35369925"
---
# <a name="collaborate-with-others-on-language-understanding-luis-apps"></a>Collaborer avec d’autres utilisateurs sur les applications Language Understanding (LUIS)  

Vous pouvez collaborer avec d’autres utilisateurs sur votre application LUIS. 

## <a name="owner-and-collaborators"></a>Propriétaire et collaborateurs
Une application a un seul propriétaire, mais peut avoir de nombreux collaborateurs. 

## <a name="add-collaborator"></a>Ajouter un collaborateur

Pour autoriser des collaborateurs à modifier votre application LUIS, sur la page **Paramètres** de votre application LUIS, entrez l’adresse électronique du collaborateur et cliquez sur **Add collaborator** (Ajouter un collaborateur).

![Ajouter un collaborateur](./media/luis-how-to-collaborate/add-collaborator.png)

* Les collaborateurs peuvent se connecter et modifier votre application LUIS en même temps que vous travaillez sur l’application. <!--If a collaborator edits the LUIS app, you see a notification at the top of the browser.-->
* Les collaborateurs ne peuvent pas ajouter d’autres collaborateurs.

## <a name="set-application-as-public"></a>Définir une application comme publique
Consultez [Accès au point de terminaison d’application publique](luis-concept-security.md#public-app-endpoint-access) pour plus d’informations.

### <a name="transfer-of-ownership"></a>Transfert de propriété
Bien que LUIS ne prenne actuellement pas en charge le transfert de propriété, vous pouvez exporter votre application, et un autre utilisateur LUIS peut l’importer. Il peut y avoir des différences mineures de scores LUIS entre les deux applications. 
