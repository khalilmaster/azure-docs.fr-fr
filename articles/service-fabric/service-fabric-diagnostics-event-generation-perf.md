---
title: Suivi des performances d’Azure Service Fabric | Microsoft Docs
description: En savoir plus sur les compteurs de performances pour le suivi et le diagnostic des clusters Azure Service Fabric.
services: service-fabric
documentationcenter: .net
author: srrengar
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/16/2018
ms.author: srrengar
ms.openlocfilehash: 9e740dd3acce842f888e5994fe8f46222477adc1
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/16/2018
ms.locfileid: "34208231"
---
# <a name="performance-metrics"></a>Mesures de performances

Les mesures doivent être collectées pour comprendre les performances de votre cluster, et des applications qu’il prend en charge. Pour les clusters Service Fabric, nous vous recommandons de collecter les compteurs de performances suivants.

## <a name="nodes"></a>Nœuds

Pour les ordinateurs de votre cluster, veuillez collecter les compteurs de performances suivants afin de mieux comprendre la charge de chaque ordinateur et de prendre les meilleures décisions quant à la mise à l’échelle des clusters.

| Catégorie de compteur | Nom de compteur |
| --- | --- |
| PhysicalDisk(per Disk) | Avg. de file d’attente lecture disque |
| PhysicalDisk(per Disk) | Avg. de file d’attente écriture disque |
| PhysicalDisk(per Disk) | Avg. Disk sec/Read |
| PhysicalDisk(per Disk) | Avg. Disk sec/Write |
| PhysicalDisk(per Disk) | Nb d’opérations de lectures de disque/s  |
| PhysicalDisk(per Disk) | Nb d’octets de lecture de disque/s  |
| PhysicalDisk(per Disk) | Nb d’opération d’écriture de disque/s |
| PhysicalDisk(per Disk) | Nb d’octets d’écriture de disque/s |
| Mémoire | Nombre d’octets disponibles |
| PagingFile | % utilisation |
| Processor(Total) | % temps processeur |
| Processus (par service) | % temps processeur |
| Processus (par service) | Traitement ID |
| Processus (par service) | Octets privés |
| Processus (par service) | Nombre de threads |
| Processus (par service) | Octets virtuels |
| Processus (par service) | Plage de travail |
| Processus (par service) | Plage de travail - Privée |
| Network Interface(all-instances) | Longueur de la file d’attente de sortie |
| Network Interface(all-instances) | Paquets sortants rejetés |
| Network Interface(all-instances) | Paquets reçus et rejetés |
| Network Interface(all-instances) | Paquets sortants, erreurs |
| Network Interface(all-instances) | Paquets reçus, erreurs |

## <a name="net-applications-and-services"></a>Applications et services .NET

Collectez les compteurs suivants si vous déployez des services .NET dans votre cluster. 

| Catégorie de compteur | Nom de compteur |
| --- | --- |
| .NET CLR Memory (par service) | ID du processus |
| .NET CLR Memory (par service) | Nombre total d’octets dédiés |
| .NET CLR Memory (par service) | Nombre total d’octets réservés |
| .NET CLR Memory (par service) | Nombre d’octets dans tous les tas |
| .NET CLR Memory (par service) | Nombre de collections de génération 0 |
| .NET CLR Memory (par service) | Nombre de collections de génération 1 |
| .NET CLR Memory (par service) | Nombre de collections de génération 2 |
| .NET CLR Memory (par service) | % de temps dans GC |

### <a name="service-fabrics-custom-performance-counters"></a>Compteurs de performances personnalisés de Service Fabric

Service Fabric génère une quantité importante de compteurs de performance personnalisés. Si le kit de développement SDK est installé, vous pouvez voir la liste complète sur votre ordinateur Windows dans l’Analyseur de performances (Démarrer > Analyseur de performances). 

Si vous utilisez Reliable Actors, dans les applications que vous déployez dans votre cluster, ajoutez des compteurs des catégories `Service Fabric Actor` et `Service Fabric Actor Method` (consultez [Service Fabric Reliable Actors Diagnostics](service-fabric-reliable-actors-diagnostics.md)) (Diagnostics Reliable Actors de Service Fabric).

Si vous utilisez Reliable Services, nous disposons également de catégories `Service Fabric Service` et `Service Fabric Service Method` desquelles vous devez collecter des compteurs. 

Si vous utilisez Reliable Collections, nous vous recommandons d’ajouter `Avg. Transaction ms/Commit` depuis le `Service Fabric Transactional Replicator` pour collecter la latence moyenne de validation par mesure de transaction.


## <a name="next-steps"></a>Étapes suivantes

* En savoir plus sur la [génération d’événements au niveau de la plateforme](service-fabric-diagnostics-event-generation-infra.md) dans Service Fabric
* Collecter des métriques de performance via [l’Agent OMS](service-fabric-diagnostics-oms-agent.md)
