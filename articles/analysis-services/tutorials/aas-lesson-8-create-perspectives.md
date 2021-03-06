---
title: 'Leçon 8 du didacticiel Azure Analysis Services : Créer des perspectives | Microsoft Docs'
description: Explique comment créer des perspectives dans le projet du didacticiel Azure Analysis Services.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 07/03/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: f72c1048a4bf2cb0a2f42db99bb35cf66563ae0f
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37442098"
---
# <a name="create-perspectives"></a>Créer des perspectives

Dans cette leçon, vous allez créer une perspective de ventes sur Internet. Une perspective correspond à un sous-ensemble visualisable d’un modèle qui fournit des points de vue précis, spécifiques à l’entreprise ou à l’application. Lorsqu’un utilisateur se connecte à un modèle à l’aide d’une perspective, il voit uniquement les objets de modèle (tables, colonnes, mesures, hiérarchies et KPI) comme des champs définis dans cette perspective. Pour plus d’informations, consultez [Perspectives](https://docs.microsoft.com/sql/analysis-services/tabular-models/perspectives-ssas-tabular).
  
La perspective Ventes sur Internet que vous créez dans cette leçon exclut l’objet de la table DimCustomer. Lorsque vous créez une perspective qui exclut certains objets, ces objets ne sont pas supprimés du modèle. Cependant, ils ne s’affichent pas dans la liste de champs du client de création de rapports. Les colonnes et les mesures calculées, qu’elles soient ou non incluses dans une perspective, peuvent toujours effectuer des calculs à partir des données des objets exclus.  
  
L’objectif de cette leçon est de décrire comment créer des perspectives et de vous familiariser avec les outils de création de modèles tabulaires. Si vous décidez plus tard d’ajouter des tables à ce modèle, vous pourrez créer des perspectives supplémentaires pour définir les différents points de vue du modèle, tels que l’inventaire et les ventes.  
  
Durée estimée pour suivre cette leçon : **5 minutes**  
  
## <a name="prerequisites"></a>Prérequis  
Cette rubrique fait partie d’un didacticiel de modélisation tabulaire, qui doit être suivi dans l’ordre prévu. Avant d’effectuer les tâches de cette leçon, vous devez avoir terminé la leçon précédente : [Leçon 7 : Créer des indicateurs de performance clés](../tutorials/aas-lesson-7-create-key-performance-indicators.md).  
  
## <a name="create-perspectives"></a>Créer des perspectives  
  
#### <a name="to-create-an-internet-sales-perspective"></a>Pour créer une perspective de ventes sur Internet  
  
1.  Cliquez sur le menu **Modèle** > **Perspectives** > **Créer et gérer**.  
  
2.  Dans la boîte de dialogue **Perspectives**, cliquez sur **Nouvelle perspective**.  
  
3.  Double-cliquez sur l’en-tête de colonne **Nouvelle perspective**, puis renommez-le **Ventes sur Internet**.  
  
4.  Sélectionnez toutes les tables *sauf* **DimCustomer**.  
  
    ![aas-lesson8-perspectives](../tutorials/media/aas-lesson8-perspectives.png)
  
    Dans une leçon ultérieure, vous allez utiliser la fonctionnalité Analyser dans Excel pour tester cette perspective. La liste de champs du tableau croisé dynamique Excel inclut toutes les tables, à l’exception de la table DimCustomer.  

## <a name="whats-next"></a>Et ensuite ?
[Leçon 9 : Créer des hiérarchies](../tutorials/aas-lesson-9-create-hierarchies.md).
  
  
  
  
