---
title: Pilote Azure Blob File System pour Azure Data Lake Storage Gen2 Preview
description: Pilote ABFS pour Hadoop FileSystem
services: storage
keywords: ''
author: jamesbak
manager: jahogg
ms.topic: article
ms.author: jamesbak
ms.date: 06/27/2018
ms.service: storage
ms.component: data-lake-storage-gen2
ms.openlocfilehash: e92c4efba29f1c40f6d4cb155974ca3a896796e5
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37114331"
---
# <a name="the-azure-blob-filesystem-driver-abfs-a-dedicated-azure-storage-driver-for-hadoop"></a>Pilote ABFS (Azure Blob File System) : pilote de stockage Azure dédié pour Hadoop

[Hadoop FileSystem](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-common/filesystem/index.html) constitue l’une des principales méthodes d’accès aux données dans Azure Data Lake Storage Gen2 Preview. Azure Data Lake Storage Gen2 propose un pilote associé, le pilote `ABFS` (Azure Blob File System). ABFS fait partie d’Apache Hadoop et est inclus dans la plupart des distributions commerciales de Hadoop. Grâce à ce pilote, de nombreuses applications et infrastructures peuvent accéder aux données dans Data Lake Storage Gen2 sans nécessiter de code faisant explicitement référence au service Data Lake Storage Gen2.

## <a name="prior-capability-the-windows-azure-storage-blob-driver"></a>Fonctionnalité préalable : pilote Windows Azure Storage Blob

Au départ, c’est le [pilote WASB](https://hadoop.apache.org/docs/current/hadoop-azure/index.html) (Windows Azure Storage Blob) qui prenait en charge Azure Storage Blob. Il avait pour tâche complexe de mapper la sémantique du système de fichiers (conformément à l’interface Hadoop FileSystem) à celle de l’interface de style « magasin d’objets » exposée par Stockage Blob Azure. Ce pilote continue à prendre en charge ce modèle et fournit un accès très performant aux données stockées dans les objets blob. Il est toutefois difficile de le tenir à jour, car la quantité de code nécessaire au mappage est très importante. Par ailleurs, quand certaines opérations comme [FileSystem.rename()](http://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-common/filesystem/filesystem.html#boolean_renamePath_src_Path_d) et [FileSystem.delete()](http://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-common/filesystem/filesystem.html#boolean_deletePath_p_boolean_recursive) sont appliquées à des répertoires, le pilote doit effectuer un grand nombre d’opérations (les magasins d’objet ne prenant pas en charge les répertoires), ce qui aboutit souvent à une dégradation des performances.

Pour remédier aux insuffisances inhérentes à la conception de WASB, le nouveau service Azure Data Lake Storage a donc été implémenté pour pouvoir être pris en charge par le nouveau pilote ABFS.

## <a name="the-azure-blob-file-system-driver"></a>Pilote Azure Blob File System

[L’interface REST Azure Data Lake Storage](https://docs.microsoft.com/en-us/rest/api/storageservices/data-lake-storage-gen2) est conçue pour prendre en charge la sémantique du système de fichiers sur Stockage Blob Azure. Étant donné que Hadoop FileSystem est conçu pour prendre en charge la même sémantique, aucun mappage complexe ne doit avoir lieu dans le pilote. Le pilote Azure Blob File System (ou ABFS) est donc un simple shim client pour l’API REST.

Le pilote doit toutefois effectuer certaines opérations :

### <a name="uri-scheme-to-reference-data"></a>Schéma d’URI pour référencer les données

Conformément à d’autres implémentations de FileSystem dans Hadoop, le pilote ABFS définit son propre schéma d’URI pour que les ressources (fichiers et répertoires) puissent être adressées distinctement. Le schéma d’URI est documenté dans [Utiliser l’URI Azure Data Lake Storage Gen2](./introduction-abfs-uri.md). L’URI a la structure suivante : `abfs[s]://file_system@account_name.dfs.core.windows.net/<path>/<path>/<file_name>`

Grâce au format d’URI ci-dessus, des outils et des frameworks Hadoop standard peuvent être utilisés pour référencer ces ressources :

```bash
hdfs dfs -mkdir -p abfs://fileanalysis@myanalytics.dfs.core.windows.net/tutorials/flightdelays/data 
hdfs dfs -put flight_delays.csv abfs://fileanalysis@myanalytics.dfs.core.windows.net/tutorials/flightdelays/data/ 
```

En interne, le pilote ABFS traduit la ou les ressources spécifiées dans l’URI en fichiers et répertoires, puis effectue des appels à l’API REST Azure Data Lake Storage avec ces références.

### <a name="authentication"></a>Authentification

Le pilote ABFS prend actuellement en charge l’authentification Shared Key. L’application Hadoop peut donc accéder de manière sécurisée aux ressources contenues dans Data Lake Storage Gen2. La clé est chiffrée et stockée dans la configuration Hadoop.

### <a name="configuration"></a>Configuration

La configuration du pilote ABFS est entièrement stockée dans le fichier de configuration <code>core-site.xml</code>. Sur les distributions Hadoop proposant [Ambari](http://ambari.apache.org/), la configuration peut également être gérée à l’aide du portail web ou de l’API REST Ambari.

Les détails de toutes les entrées de configuration prises en charge sont spécifiés dans la [documentation Hadoop officielle](http://hadoop.apache.org/docs/current/hadoop-azure/index.html).

### <a name="hadoop-documentation"></a>Documentation Hadoop

Le pilote ABFS est présenté au complet dans la [documentation Hadoop officielle](http://hadoop.apache.org/docs/current/hadoop-azure/index.html).

## <a name="next-steps"></a>Étapes suivantes

- [Configurer des clusters HDInsight](./quickstart-create-connect-hdi-cluster.md)
- [Créer un cluster Azure Databricks](./quickstart-create-databricks-account.md)
- [Utiliser l’URI Azure Data Lake Storage Gen2](./introduction-abfs-uri.md)
