---
title: Configuration d’environnements de science des données dans Azure | Microsoft Docs
description: Configurer des environnements de science des données dans Azure à utiliser dans le processus TDSP (Team Data Science Process).
services: machine-learning
documentationcenter: ''
author: deguhath
manager: cgronlun
editor: cgronlun
ms.assetid: 481cfa6a-7ea3-46ac-b0f9-2e3982c37153
ms.service: machine-learning
ms.component: team-data-science-process
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/29/2017
ms.author: deguhath
ms.openlocfilehash: 46dd8b84202e6f534456afa4c92a8decced895e8
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/07/2018
ms.locfileid: "34836985"
---
# <a name="set-up-data-science-environments-for-use-in-the-team-data-science-process"></a>Configuration d’environnements de science des données à utiliser dans le processus TDSP (Team Data Science Process)
Le processus TDSP (Team Data Science Process) utilise différents environnements de science de données pour le stockage, le traitement et l’analyse de données. Il peut s’agir par exemple de Stockage Blob Azure, de plusieurs types de machines virtuelles Azure, de clusters HDInsight (Hadoop) et d’espaces de travail Azure Machine Learning. Le choix de l’environnement à utiliser dépend du type et de la quantité des données à modéliser, ainsi que de la destination cible de ces données dans le cloud. 

* Pour plus d’informations sur les questions à prendre en compte lors de cette décision, consultez la page [Planifier votre environnement de science des données Azure Machine Learning](plan-your-environment.md). 
* Pour accéder à un catalogue de scénarios que vous pouvez rencontrer lorsque vous effectuez des analyses avancées, consultez la page [Scénarios du processus TDSP (Team Data Science Process)](plan-sample-scenarios.md)

Ce menu pointe vers des rubriques qui décrivent comment configurer les différents environnements de science de données utilisés par le processus TDSP (Team Data Science Process).

[!INCLUDE [data-science-environment-setup](../../../includes/cap-setup-environments.md)]

La **machine virtuelle pour la science des données de Microsoft (DSVM)** est également disponible sous forme d’image de machine virtuelle Azure. Cette machine virtuelle est préinstallée et configurée avec plusieurs outils couramment utilisés dans le cadre de l’analyse de données et de l’apprentissage automatique. La machine virtuelle DSVM est disponible sur Windows et Linux. Pour plus d’informations, consultez [Présentation de la machine virtuelle Science des données basée sur le cloud pour Linux et Windows](../data-science-virtual-machine/overview.md).

