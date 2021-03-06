---
title: Limites QnA Maker - Azure Cognitive Services | Microsoft Docs
description: Limites QnA Maker
services: cognitive-services
author: nstulasi
manager: sangitap
ms.service: cognitive-services
ms.component: QnAMaker
ms.topic: article
ms.date: 05/07/2018
ms.author: saneppal
ms.openlocfilehash: 4d2bafad08ffab76743cb802733a5d2f01a97f06
ms.sourcegitcommit: c722760331294bc8532f8ddc01ed5aa8b9778dec
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/04/2018
ms.locfileid: "35370924"
---
# <a name="qna-maker-limits"></a>Limites QnA Maker
Liste complète des limites dans QnA Maker.

## <a name="knowledge-bases"></a>Bases de connaissances

* Nombre maximal de bases de connaissances selon les [limites de niveau de la Recherche Azure](https://docs.microsoft.com/en-us/azure/search/search-limits-quotas-capacity)

|**Niveau de la Recherche Azure** | **Gratuit** | **De base** |**S1** | **S2**| **S3** |**S3 HD**|
|---|---|---|---|---|---|----|
|Nombre maximal de bases de connaissances publiées autorisées (index max -- 1 (réservé pour le test)|2|14|49|199|199|2999|

## <a name="extraction-limits"></a>Limites d’extraction
* Nombre maximal de fichiers qui peuvent être extraits et taille de fichier maximale : consultez [Tarification QnAMaker](https://azure.microsoft.com/en-in/pricing/details/cognitive-services/qna-maker/)
* Nombre maximal de liens ciblés qui peuvent être analysés pour l’extraction de QnA à partir des pages HTML de FAQ : 20

## <a name="metadata-limits"></a>Limites de métadonnées
* Nombre maximal de champs de métadonnées par base de connaissances selon les [limites de niveau de la Recherche Azure](https://docs.microsoft.com/en-us/azure/search/search-limits-quotas-capacity)

|**Niveau de la Recherche Azure** | **Gratuit** | **De base** |**S1** | **S2**| **S3** |**S3 HD**|
|---|---|---|---|---|---|----|
|Nombre maximal de champs de métadonnées par service QnA Maker (pour tous les Kbits/s)|1 000|100*|1 000|1 000|1 000|1 000|

## <a name="knowledge-base-content-limits"></a>Limites de contenu de la base de connaissances
Limites globales sur le contenu de la base de connaissances :
* Longueur du texte de la réponse : 250 000
* Longueur du texte de la question : 1 000
* Longueur de la clé de métadonnée/du texte de valeur : 100
* Caractères pris en charge pour le nom des métadonnées : lettres, chiffres et _  
* Caractères pris en charge pour le nom des métadonnées : tout sauf : et | 
* Longueur du nom de fichier : 200
* Formats de fichier pris en charge : « .tsv », « .pdf », « .txt », « .docx », « .xlsx ».
* Nombre maximal d’autres questions : 100
* Nombre maximal de paires question-réponse : dépend du [niveau de Recherche Azure](https://docs.microsoft.com/en-in/azure/search/search-limits-quotas-capacity#document-limits) choisi 

## <a name="create-knowledge-base-call-limits"></a>Créer des limites d’appel de base de connaissances :
Elles représentent les limites pour chaque action de création d’une base de connaissances, autrement dit, pour chaque clic sur *Créer une base de connaissances* ou pour chaque appel de l’API CreateKnowledgeBase.
* Nombre maximal d’autres questions par réponse : 100
* Nombre maximal d’URL : 10
* Nombre maximal de fichiers : 10

## <a name="update-knowledge-base-call-limits"></a>Limites d’appel de mise à jour de la base de connaissances
Elles représentent les limites pour chaque action de mise à jour, autrement dit, pour chaque clic sur *Save and train* (Enregistrer et former) ou pour chaque appel de l’API UpdateKnowledgeBase.
* Longueur de chaque nom source : 300
* Nombre maximal d’autres questions ajoutées ou supprimées : 100
* Nombre maximal de champs de métadonnées ajoutés ou supprimés : 10
* Nombre maximal d’URL qui peuvent être actualisées : 5
