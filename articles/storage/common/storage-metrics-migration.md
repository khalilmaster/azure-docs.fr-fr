---
title: Migration des métriques de Stockage Azure | Microsoft Docs
description: Découvrez plus d’informations sur la migration des anciennes métriques vers les nouvelles métriques gérées par Azure Monitor.
services: storage
documentationcenter: na
author: fhryo-msft
manager: cbrooks
editor: fhryo-msft
ms.assetid: ''
ms.service: storage
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage
ms.date: 03/30/2018
ms.author: fryu
ms.openlocfilehash: 9586d0c2b61fe97106378ec684c28be9143b5194
ms.sourcegitcommit: f606248b31182cc559b21e79778c9397127e54df
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38969382"
---
# <a name="azure-storage-metrics-migration"></a>Migration des métriques de Stockage Azure

En adéquation avec la stratégie d’unification de l’expérience de monitoring dans Azure, le Stockage Azure intègre des métriques à la plateforme Azure Monitor. À l’avenir, le service des anciennes métriques sera arrêté (avec notification préalable) conformément à la stratégie Azure. Si vous utilisez d’anciennes métriques de stockage, vous devez effectuer la migration avant la date de fin du service pour conserver vos informations de métriques.

Cet article explique comment migrer les anciennes métriques vers les nouvelles.

## <a name="understand-old-metrics-that-are-managed-by-azure-storage"></a>Présentation des anciennes métriques gérées par le Stockage Azure

Le Stockage Azure collecte d’anciennes valeurs de métrique, les regroupe et les stocke dans des tables $Metric du même compte de stockage. Vous pouvez utiliser le portail Azure pour configurer un graphique de monitoring. Vous pouvez aussi utiliser les SDK du Stockage Azure pour lire les données des tables $Metric qui sont basées sur le schéma. Pour plus d’informations, consultez [Storage Analytics](./storage-analytics.md).

Les anciennes métriques fournissent des métriques de capacité uniquement sur le stockage Blob Azure. Les anciennes métriques fournissent des métriques de transaction sur le stockage Blob, le stockage Table, Azure Files et le stockage File d’attente. 

Les anciennes métriques sont conçues dans un schéma plat. Cette conception ne donne pas de valeur de métrique quand les modèles de trafic ne déclenchent pas la métrique. Par exemple, la valeur **ServerTimeoutError** est définie sur 0 dans les tables $Metric, même si vous ne recevez aucune erreur d’expiration de délai d’attente de serveur de la part du trafic en direct sur le compte de stockage.

## <a name="understand-new-metrics-managed-by-azure-monitor"></a>Présentation des nouvelles métriques gérées par Azure Monitor

Pour les nouvelles métriques de stockage, le Stockage Azure envoie les données de métrique au backend Azure Monitor. Azure Monitor offre une expérience de monitoring unifiée, comprenant à la fois les données du portail et l’ingestion de données. Pour plus d’informations, consultez cet [article](../../monitoring-and-diagnostics/monitoring-overview-metrics.md).

Les nouvelles métriques fournissent des métriques de capacité et des métriques de transaction sur l’objet blob, la table, le fichier, la file d’attente et le stockage Premium.

La conception multidimension est une des fonctionnalités fournies par Azure Monitor. Le Stockage Azure adopte cette conception en définissant un nouveau schéma de métrique. Pour connaître les dimensions prises en charge sur les métriques, consultez [Métriques de Stockage Azure dans Azure Monitor](./storage-metrics-in-azure-monitor.md). La conception multidimension permet de rentabiliser la bande passante par l’ingestion, et la capacité par les métriques de stockage. Par conséquent, si votre trafic n’a pas déclenché de métriques connexes, les données de métrique associées ne sont pas générées. Par exemple, si votre trafic n’a pas déclenché d’erreur d’expiration de délai d’attente de serveur, Azure Monitor ne retourne aucune donnée quand vous interrogez la valeur de métrique **Transactions** avec la dimension **ResponseType** égale à **ServerTimeoutError**.

## <a name="metrics-mapping-between-old-metrics-and-new-metrics"></a>Mappage entre anciennes et nouvelles métriques

Si vous lisez les données de métriques par programmation, vous devez adopter le nouveau schéma de métriques dans vos programmes. Pour mieux comprendre les changements, vous pouvez consulter le mappage dans le tableau suivant :

**Métriques de capacité**

| Ancienne métrique | Nouvelle métrique |
| ------------------- | ----------------- |
| **Capacité**            | **BlobCapacity** avec la dimension **BlobType** égale à **BlockBlob** ou **PageBlob** |
| **ObjectCount**        | **BlobCount** avec la dimension **BlobType** égale à **BlockBlob** ou **PageBlob** |
| **ContainerCount**      | **ContainerCount** |

Les métriques suivantes sont de nouvelles offres non prises en charge par les anciennes métriques :
* **TableCapacity**
* **TableCount**
* **TableEntityCount**
* **QueueCapacity**
* **QueueCount**
* **QueueMessageCount**
* **FileCapacity**
* **FileCount**
* **FileShareCount**
* **UsedCapacity**

**Métriques de transaction**

| Ancienne métrique | Nouvelle métrique |
| ------------------- | ----------------- |
| **AnonymousAuthorizationError** | Transactions avec la dimension **ResponseType** égale à **AuthorizationError** |
| **AnonymousClientOtherError** | Transactions avec la dimension **ResponseType** égale à **ClientOtherError** |
| **AnonymousClientTimeoutError** | Transactions avec la dimension **ResponseType** égale à **ClientTimeoutError** |
| **AnonymousNetworkError** | Transactions avec la dimension **ResponseType** égale à **NetworkError** |
| **AnonymousServerOtherError** | Transactions avec la dimension **ResponseType** égale à **ServerOtherError** |
| **AnonymousServerTimeoutError** | Transactions avec la dimension **ResponseType** égale à **ServerTimeoutError** |
| **AnonymousSuccess** | Transactions avec la dimension **ResponseType** égale à **Success** |
| **AnonymousThrottlingError** | Transactions avec la dimension **ResponseType** égale à **ClientThrottlingError** ou **ServerBusyError** |
| **AuthorizationError** | Transactions avec la dimension **ResponseType** égale à **AuthorizationError** |
| **Disponibilité** | **Disponibilité** |
| **AverageE2ELatency** | **SuccessE2ELatency** |
| **AverageServerLatency** | **SuccessServerLatency** |
| **ClientOtherError** | Transactions avec la dimension **ResponseType** égale à **ClientOtherError** |
| **ClientTimeoutError** | Transactions avec la dimension **ResponseType** égale à **ClientTimeoutError** |
| **NetworkError** | Transactions avec la dimension **ResponseType** égale à **NetworkError** |
| **PercentAuthorizationError** | Transactions avec la dimension **ResponseType** égale à **AuthorizationError** |
| **PercentClientOtherError** | Transactions avec la dimension **ResponseType** égale à **ClientOtherError** |
| **PercentNetworkError** | Transactions avec la dimension **ResponseType** égale à **NetworkError** |
| **PercentClientOtherError** | Transactions avec la dimension **ResponseType** égale à **ServerOtherError** |
| **PercentSuccess** | Transactions avec la dimension **ResponseType** égale à **Success** |
| **PercentThrottlingError** | Transactions avec la dimension **ResponseType** égale à **ClientThrottlingError** ou **ServerBusyError** |
| **PercentTimeoutError** | Transactions avec la dimension **ResponseType** égale à **ServerTimeoutError** ou **ResponseType** égale à **ClientTimeoutError** |
| **SASAuthorizationError** | Transactions avec la dimension **ResponseType** égale à **AuthorizationError** |
| **SASClientOtherError** | Transactions avec la dimension **ResponseType** égale à **ClientOtherError** |
| **SASClientTimeoutError** | Transactions avec la dimension **ResponseType** égale à **ClientTimeoutError** |
| **SASNetworkError** | Transactions avec la dimension **ResponseType** égale à **NetworkError** |
| **SASServerOtherError** | Transactions avec la dimension **ResponseType** égale à **ServerOtherError** |
| **SASServerTimeoutError** | Transactions avec la dimension **ResponseType** égale à **ServerTimeoutError** |
| **SASSuccess** | Transactions avec la dimension **ResponseType** égale à **Success** |
| **SASThrottlingError** | Transactions avec la dimension **ResponseType** égale à **ClientThrottlingError** ou **ServerBusyError** |
| **ServerOtherError** | Transactions avec la dimension **ResponseType** égale à **ServerOtherError** |
| **ServerTimeoutError** | Transactions avec la dimension **ResponseType** égale à **ServerTimeoutError** |
| **Success** | Transactions avec la dimension **ResponseType** égale à **Success** |
| **ThrottlingError** | **Transactions** avec la dimension **ResponseType** égale à **ClientThrottlingError** ou **ServerBusyError**|
| **TotalBillableRequests** | **Transactions** |
| **TotalEgress** | **Sortie** |
| **TotalIngress** | **Entrée** |
| **TotalRequests** | **Transactions** |

## <a name="faq"></a>Forum Aux Questions

### <a name="how-should-i-migrate-existing-alert-rules"></a>Comment migrer des règles d’alerte existantes ?

Si vous avez créé des règles d’alerte classiques basées sur des anciennes métriques de stockage, vous devez créer des règles d’alerte basées sur le nouveau schéma de métriques.

### <a name="is-new-metric-data-stored-in-the-same-storage-account-by-default"></a>Les nouvelles données de métrique sont-elles stockées dans le même compte de stockage par défaut ?

Non. Pour archiver les données de métrique dans un compte de stockage, utilisez [L’API Paramètre de diagnostic Azure Monitor](https://docs.microsoft.com/rest/api/monitor/diagnosticsettings/createorupdate).

## <a name="next-steps"></a>Étapes suivantes

* [Azure Monitor](../../monitoring-and-diagnostics/monitoring-overview.md)
* [Métriques de stockage dans Azure Monitor](./storage-metrics-in-azure-monitor.md)
