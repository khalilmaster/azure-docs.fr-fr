---
title: Problèmes lors de la création d’une application de proxy d’application | Microsoft Docs
description: Comment résoudre les problèmes de création d’applications de proxy d’application dans le portail d’administration Azure AD
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/21/2018
ms.author: barbkess
ms.reviewer: asteen
ms.openlocfilehash: 97795593ecc83a0b15f018861f5cd9139d26443b
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/23/2018
ms.locfileid: "36335106"
---
# <a name="problem-creating-an-application-proxy-application"></a>Problèmes lors de la création d’une application de proxy d’application 

Cet article aborde certains problèmes courants auxquels sont confrontés les utilisateurs lors de la création d’une application de proxy d’application.

## <a name="recommended-documents"></a>Documents recommandés 

Pour en savoir plus sur la création d’une application de proxy d’application par le biais du portail d’administration, consultez [Publier des applications avec le Proxy d’application Azure AD](manage-apps/application-proxy-publish-azure-portal.md).

Si vous suivez les étapes décrites dans ce document et si vous obtenez une erreur lors de création de l’application, consultez les détails de l’erreur pour en savoir plus et découvrir des suggestions afin de corriger l’application. La plupart des messages d’erreur incluent la suggestion d’un correctif. 

## <a name="specific-things-to-check"></a>Éléments spécifiques à vérifier

Pour éviter les erreurs courantes, vérifiez les points suivants :

-   Vous êtes un administrateur autorisé à créer une application de proxy d’application

-   L’URL interne est unique

-   L’URL externe est unique

-   Les URL commencent par http ou https et se terminent par un signe « / »

-   L’URL doit être un nom de domaine et non une adresse IP

Le message d’erreur doit s’afficher dans le coin supérieur droit lorsque vous créez l’application. Vous pouvez également sélectionner l’icône de notification pour afficher les messages d’erreur.

   ![Invite de notification](./media/application-proxy-config-problem/error-message.png)

## <a name="next-steps"></a>Étapes suivantes
[Activer le proxy d’application dans le portail Azure](manage-apps/application-proxy-enable.md)
