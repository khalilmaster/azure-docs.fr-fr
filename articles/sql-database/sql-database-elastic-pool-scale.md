---
title: Mettre à l’échelle un pool élastique - Azure SQL Database | Microsoft Docs
description: Cette page décrit les ressources de mise à l’échelle pour les pools élastiques dans Azure SQL Database.
services: sql-database
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.custom: DBs & servers
ms.topic: conceptual
ms.date: 06/20/2018
ms.author: carlrab
ms.openlocfilehash: 0d15382f413485ccc287bac945b3c88eb748a9f6
ms.sourcegitcommit: 638599eb548e41f341c54e14b29480ab02655db1
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/21/2018
ms.locfileid: "36311333"
---
# <a name="scale-elastic-pool-resources-in-azure-sql-database"></a>Mettre à l’échelle un pool élastique dans Azure SQL Database

Cet article décrit comment faire évoluer les ressources de calcul et de stockage disponibles pour les pools élastiques et les bases de données en pool dans Azure SQL Database. 

## <a name="vcore-based-purchasing-model-change-elastic-pool-storage-size"></a>Modèle d’achat vCore : modifier la taille de stockage de pool élastique

- Le stockage peut être approvisionné jusqu’à la limite de taille maximale : 
 - Pour le stockage Standard, augmenter ou diminuer la taille par incréments de 10 Go
 - Pour le stockage Premium, augmentez ou diminuez la taille par incréments de 250 Go
- Le stockage pour un pool élastique peut être configuré en augmentant ou diminuant sa taille maximale.
- Le prix du stockage pour un pool élastique est égal au volume de stockage multiplié par le prix unitaire du stockage pour le niveau de service. Pour plus d’informations sur le prix du stockage supplémentaire, consultez [Tarification des bases de données SQL](https://azure.microsoft.com/pricing/details/sql-database/).

## <a name="vcore-based-purchasing-model-change-elastic-pool-compute-resources-vcores"></a>Modèle d’achat vCore : modifier les ressources de calcul de pool élastique (vCores)

Vous pouvez augmenter ou diminuer le niveau de performance pour un pool élastique en fonction des besoins de la ressource à l’aide du [portail Azure](sql-database-elastic-pool-manage.md#azure-portal-manage-elastic-pools-and-pooled-databases), de [PowerShell](/powershell/module/azurerm.sql/set-azurermsqlelasticpool), [d’Azure CLI](/cli/azure/sql/elastic-pool#az_sql_elastic_pool_update) ou de [l’API REST](/rest/api/sql/elasticpools/update).

- Lors de la remise à l’échelle des vCore du pool, les connexions de base de données sont brièvement interrompues. Il s’agit du même comportement que celui qui se produit lors de la remise à l’échelle des DTU pour une base de données (qui n’est pas dans un pool). Pour plus d’informations sur la durée et l’impact des pertes de connexion pour une base de données lors des opérations de remise à l’échelle, consultez [Remise à l’échelle des DTU pour une base de données unique](#single-database-change-storage-size). 
- La durée de remise à l’échelle des vCore du pool peut dépendre de la quantité totale d’espace de stockage utilisée par toutes les bases de données dans le pool. En règle générale, la latence de remise à l’échelle moyenne est de 90 minutes ou moins pour 100 Go. Par exemple, si l’espace total utilisé par toutes les bases de données du pool est égal à 200 Go, une opération de redimensionnement du pool prend 3 heures au maximum. Dans certains cas dans le niveau Standard ou De base, la latence de remise à l’échelle peut être de moins de cinq minutes, quelle que soit la quantité d’espace utilisé.
- En général, la durée de modification du nombre minimal de vCore par base de données ou du nombre maximal de vCore par base de données prend cinq minutes au maximum.
- Lors de la réduction des vCore d’un pool, l’espace utilisé par le pool doit être inférieur à la taille maximale autorisée du niveau de service cible et des vCore maximum du pool.

## <a name="dtu-based-purchasing-model-change-elastic-pool-storage-size"></a>Modèle d’achat DTU : modifier la taille de stockage de pool élastique

- Le prix des eDTU pour un pool élastique inclut une certaine quantité de stockage sans coût supplémentaire. Un espace de stockage en plus du volume inclus peut être approvisionné pour un coût supplémentaire jusqu’à la limite de taille par incréments de 250 Go jusqu’à 1 To, puis par incréments de 256 Go au-delà de 1 To. Pour les quantités de stockage et limites de taille maximale incluses, consultez [Pool élastique : tailles de stockage et niveaux de performance](#elastic-pool-storage-sizes-and-performance-levels).
- Vous pouvez configurer du stockage supplémentaire pour un pool élastique en augmentant sa taille maximale à l’aide du [portail Azure](sql-database-elastic-pool-scale.md#azure-portal-manage-elastic-pools-and-pooled-databases), de [PowerShell](/powershell/module/azurerm.sql/set-azurermsqlelasticpool), [d’Azure CLI](/cli/azure/sql/elastic-pool#az_sql_elastic_pool_update) ou de [l’API REST](/rest/api/sql/elasticpools/update).
- Le prix de l’espace de stockage supplémentaire pour un pool élastique est égal au volume de stockage supplémentaire multiplié par le prix unitaire du stockage supplémentaire pour le niveau de service. Pour plus d’informations sur le prix du stockage supplémentaire, consultez [Tarification des bases de données SQL](https://azure.microsoft.com/pricing/details/sql-database/).

## <a name="dtu-based-purchasing-model-change-elastic-pool-compute-resources-edtus"></a>Modèle d’achat DTU : modifier les ressources de calcul de pool élastique (eDTU)

Vous pouvez augmenter ou diminuer les ressources disponibles pour un pool élastique en fonction des besoins de la ressource à l’aide du [portail Azure](sql-database-elastic-pool-scale.md#azure-portal-manage-elastic-pools-and-pooled-databases), de [PowerShell](/powershell/module/azurerm.sql/set-azurermsqlelasticpool), [d’Azure CLI](/cli/azure/sql/elastic-pool#az_sql_elastic_pool_update) ou de [l’API REST](/rest/api/sql/elasticpools/update).

- Lors de la remise à l’échelle des eDTU du pool, les connexions de base de données sont brièvement supprimées. Il s’agit du même comportement que celui qui se produit lors de la remise à l’échelle des DTU pour une base de données (qui n’est pas dans un pool). Pour plus d’informations sur la durée et l’impact des pertes de connexion pour une base de données lors des opérations de remise à l’échelle, consultez [Remise à l’échelle des DTU pour une base de données unique](#single-database-change-storage-size). 
- La durée de remise à l’échelle des eDTU de pool peut dépendre de la quantité totale d’espace de stockage utilisée par toutes les bases de données dans le pool. En règle générale, la latence de remise à l’échelle moyenne est de 90 minutes ou moins pour 100 Go. Par exemple, si l’espace total utilisé par toutes les bases de données du pool est égal à 200 Go, une opération de redimensionnement du pool prend 3 heures au maximum. Dans certains cas dans le niveau Standard ou De base, la latence de remise à l’échelle peut être de moins de cinq minutes, quelle que soit la quantité d’espace utilisé.
- En général, la durée de modification du nombre minimal d’eDTU par base de données ou du nombre maximal d’eDTU par base de données prend cinq minutes au maximum.
- Lors de la réduction des eDTU d’un pool, l’espace utilisé par le pool doit être inférieur à la taille et au niveau de performance autorisés et aux eDTU maximales du pool.
- Lors de la remise à l’échelle des eDTU du pool, un coût de stockage supplémentaire s’applique si (1) la taille de stockage maximale du pool est prise en charge par le pool cible, et (2) la taille maximale de stockage dépasse la quantité de stockage incluse dans le pool cible. Par exemple, si un pool Standard de 100 eDTU avec une taille maximale de 100 Go est rétrogradé en pool Standard de 50 eDTU, un coût de stockage supplémentaire s’applique, car le pool cible prend en charge une taille maximale de 100 Go et la quantité de stockage incluse est de seulement 50 Go. Par conséquent, la quantité d’espace de stockage supplémentaire est de 100 Go - 50 Go = 50 Go. Pour la tarification du stockage supplémentaire, consultez [Tarification des bases de données SQL](https://azure.microsoft.com/pricing/details/sql-database/). Si la quantité réelle d’espace utilisé est inférieure à la quantité de stockage incluse, ce coût supplémentaire peut être évité en réduisant la taille maximale de la base de données à la quantité incluse. 

## <a name="next-steps"></a>Étapes suivantes

Pour les limites de ressources globales, consultez [SQL Database vCore-based resource limits - elastic pools](sql-database-vcore-resource-limits-elastic-pools.md) (limites de ressource vCore SQL Database - pools élastiques) et [SQL Database DTU-based resource limits - elastic pools](sql-database-dtu-resource-limits-elastic-pools.md) (limites de ressource DTU SQL Database - pools élastiques).
