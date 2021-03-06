---
title: Fichier Include
description: Fichier Include
services: cognitive-services
author: chliang
manager: bix
ms.service: cognitive-services
ms.technology: anomaly-finder
ms.topic: include
ms.date: 04/13/2018
ms.author: chliang
ms.custom: include file
ms.openlocfilehash: df7326cb8e671d0f71924e813a1354dfef1e20c7
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35369121"
---
Les données sont retournées avec la valeur attendue et les marges supérieure et inférieure par défaut. Dans la pratique, vous pouvez définir un paramètre [sensitivity], puis utiliser (ExpectedValue + sensitivity * UpperMargin) comme limite supérieure et (ExpectedValue - sensitivity * LowerMargin) comme limite inférieure pour régler le point d’anomalie par vous-même. La valeur de [sensitivity] doit être supérieure à 1. Vous trouverez ci-dessous des diagrammes pour le réglage.

> [!NOTE]
> Les diagrammes ne sont pas générés par l’exemple d’application. Ils sont créés par un outil distinct avec l’exemple d’application.

![Réglage : sensitivity = 1.0](../media/sensitivity_1.png)
![Réglage : sensitivity = 1.5](../media/sensitivity_1.5.png)
![Réglage : sensitivity = 2](../media/sensitivity_2.png)
![Réglage : sensitivity = 3.5](../media/sensitivity_3.5.png)