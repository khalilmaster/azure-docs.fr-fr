---
title: 'Tutoriel : Créer des clusters Hadoop à la demande dans Azure HDInsight à l’aide de Data Factory | Microsoft Docs'
description: Découvrez comment créer des clusters Hadoop à la demande dans HDInsight avec Azure Data Factory.
services: hdinsight
documentationcenter: ''
tags: azure-portal
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 1f3b3a78-4d16-4d99-ba6e-06f7bb185d6a
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: conceptual
ms.date: 05/07/2018
ms.author: nitinme
ms.openlocfilehash: 9d54d3481176b36a0d13a9b8af2fad03349b81be
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36229251"
---
# <a name="tutorial-create-on-demand-hadoop-clusters-in-hdinsight-using-azure-data-factory"></a>Tutoriel : Créer des clusters Hadoop à la demande dans HDInsight avec Azure Data Factory
[!INCLUDE [selector](../../includes/hdinsight-create-linux-cluster-selector.md)]

Dans cet article, vous allez apprendre à créer un cluster Hadoop, à la demande, dans Azure HDInsight à l’aide d’Azure Data Factory. Ensuite, vous utiliserez des pipelines de données dans Azure Data Factory pour exécuter des travaux Hive et supprimer le cluster. À la fin de ce tutoriel, vous saurez rendre opérationnelle l’exécution d’un travail Big Data, où la création du cluster, l’exécution du travail et la suppression du cluster sont effectuées selon une planification.

Ce didacticiel décrit les tâches suivantes : 

> [!div class="checklist"]
> * Créer un compte de stockage Azure
> * Comprendre l’activité Azure Data Factory
> * Créer une fabrique de données à l’aide du portail Azure
> * Créez des services liés
> * Créer un pipeline
> * Déclencher un pipeline
> * Surveiller un pipeline
> * Vérifier la sortie

Si vous ne disposez pas d’abonnement Azure, créez un [compte gratuit](https://azure.microsoft.com/free/) avant de commencer.

## <a name="prerequisites"></a>Prérequis

* Azure PowerShell. Pour obtenir des instructions, consultez la rubrique [Installation et configuration d'Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-5.7.0).

* Un principal de service Azure Active Directory. Une fois que vous avez créé le principal de service, n’oubliez pas de récupérer **l’ID d’application** et la **clé d’authentification** en suivant les instructions dans l’article dont le lien est indiqué ci-après. Vous aurez besoin de ces valeurs plus loin dans ce didacticiel. En outre, vérifiez que ce principal de service est membre du rôle *Contributeur* de l’abonnement ou du groupe de ressources dans lequel le cluster est créé. Pour savoir comment récupérer les valeurs requises et attribuer les rôles adéquats, consultez [Créer un principal de service Azure Active Directory](../azure-resource-manager/resource-group-create-service-principal-portal.md).

## <a name="create-an-azure-storage-account"></a>Créer un compte de stockage Azure

Dans cette section, vous créez un compte de stockage qui fera office de stockage par défaut pour le cluster HDInsight que vous créez à la demande. Ce compte de stockage contient également l’exemple de script HiveQL (**hivescript.hql**) qui vous permet de simuler un exemple de travail Hive qui s’exécute sur le cluster.

Cette section utilise un script Azure PowerShell pour créer le compte de stockage et y copier les fichiers requis. L’exemple de script Azure PowerShell de cette section exécute les tâches suivantes :

1. Se connecte à Azure.
2. Crée un groupe de ressources Azure.
3. Crée un compte de stockage Azure.
4. Crée un conteneur d’objets blob dans le compte de stockage.
5. Copie l’exemple de script HiveQL (**hivescript.hql**) dans le conteneur d’objets blob. Le script est disponible à l’adresse [https://hditutorialdata.blob.core.windows.net/adfv2hiveactivity/hivescripts/hivescript.hql](https://hditutorialdata.blob.core.windows.net/adfhiveactivity/script/partitionweblogs.hql). L’exemple de script est déjà disponible dans un autre conteneur d’objets blob public. Le script PowerShell ci-dessous crée une copie de ces fichiers dans le compte de stockage Azure qu’il crée.


**Pour créer un compte de stockage et copier les fichiers à l’aide d’Azure PowerShell :**
> [!IMPORTANT]
> Spécifiez des noms pour le groupe de ressources Azure et le compte de stockage Azure qui seront créés par le script.
> Notez le **nom du groupe de ressources**, le **nom du compte de stockage** et la **clé du compte de stockage** générés en sortie par le script. Vous aurez besoin de ces informations dans la section suivante.

```powershell
$resourceGroupName = "<Azure Resource Group Name>"
$storageAccountName = "<Azure Storage Account Name>"
$location = "East US 2"

$sourceStorageAccountName = "hditutorialdata"  
$sourceContainerName = "adfv2hiveactivity"

$destStorageAccountName = $storageAccountName
$destContainerName = "adfgetstarted" # don't change this value.

####################################
# Connect to Azure
####################################
#region - Connect to Azure subscription
Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
Login-AzureRmAccount
#endregion

####################################
# Create a resource group, storage, and container
####################################

#region - create Azure resources
Write-Host "`nCreating resource group, storage account and blob container ..." -ForegroundColor Green

New-AzureRmResourceGroup -Name $resourceGroupName -Location $location
New-AzureRmStorageAccount `
    -ResourceGroupName $resourceGroupName `
    -Name $destStorageAccountName `
    -type Standard_LRS `
    -Location $location

$destStorageAccountKey = (Get-AzureRmStorageAccountKey `
    -ResourceGroupName $resourceGroupName `
    -Name $destStorageAccountName)[0].Value

$sourceContext = New-AzureStorageContext `
    -StorageAccountName $sourceStorageAccountName `
    -Anonymous
$destContext = New-AzureStorageContext `
    -StorageAccountName $destStorageAccountName `
    -StorageAccountKey $destStorageAccountKey

New-AzureStorageContainer -Name $destContainerName -Context $destContext
#endregion

####################################
# Copy files
####################################
#region - copy files
Write-Host "`nCopying files ..." -ForegroundColor Green

$blobs = Get-AzureStorageBlob `
    -Context $sourceContext `
    -Container $sourceContainerName

$blobs|Start-AzureStorageBlobCopy `
    -DestContext $destContext `
    -DestContainer $destContainerName

Write-Host "`nCopied files ..." -ForegroundColor Green
Get-AzureStorageBlob -Context $destContext -Container $destContainerName
#endregion

Write-host "`nYou will use the following values:" -ForegroundColor Green
write-host "`nResource group name: $resourceGroupName"
Write-host "Storage Account Name: $destStorageAccountName"
write-host "Storage Account Key: $destStorageAccountKey"

Write-host "`nScript completed" -ForegroundColor Green
```

**Pour vérifier la création du compte de stockage**

1. Connectez-vous au [Portail Azure](https://portal.azure.com).
2. Sélectionnez **Groupes de ressources** dans le volet de gauche.
3. Double-cliquez sur le nom du groupe de ressources que vous avez créé dans votre script PowerShell. Utilisez le filtre si la liste des groupes de ressources est trop longue.
4. Dans la vignette **Ressources**, vous voyez une ressource, sauf si vous partagez le groupe de ressources avec d’autres projets. Cette ressource correspond au compte de stockage avec le nom que vous avez spécifié précédemment. Sélectionnez le nom du compte de stockage.
5. Sélectionnez la vignette **Objets Blob**.
6. Sélectionnez le conteneur **adfgetstarted**. Vous voyez un dossier appelé **hivescripts**.
7. Ouvrez le dossier et vérifiez qu’il contient l’exemple de fichier de script, **hivescript.hql**.

## <a name="understand-the-azure-data-factory-activity"></a>Comprendre l’activité Azure Data Factory

[Azure Data Factory](../data-factory/introduction.md) gère et automatise le déplacement et la transformation des données. Azure Data Factory peut créer un cluster Hadoop HDInsight juste-à-temps pour traiter une tranche de données d’entrée et supprimer le cluster à l’issue du traitement. 

Dans Azure Data Factory, une fabrique de données peut comporter un ou plusieurs pipelines de données. Un pipeline de données comprend une ou plusieurs activités. Il existe deux types d’activité :

* [Activités de déplacement des données](../data-factory/copy-activity-overview.md) Vous utilisez les activités de déplacement des données pour déplacer des données d’une banque de données source vers une banque de données de destination.
* [Activités de transformation des données](../data-factory/transform-data.md). Vous utilisez les activités de transformation des données pour transformer/traiter les données. L’activité Hive HDInsight est l’une des activités de transformation prises en charge par Data Factory. Dans ce didacticiel, vous utilisez l’activité de transformation Hive.

Dans cet article, vous configurez l’activité Hive pour créer un cluster HDInsight Hadoop à la demande. Quand l’activité s’exécute pour traiter des données, le déroulement des opérations est le suivant :

1. Un cluster Hadoop HDInsight est automatiquement créé juste-à-temps à votre intention pour traiter la tranche. 

2. Les données d’entrée sont traitées par l’exécution d’un script HiveQL sur le cluster. Dans ce didacticiel, le script HiveQL associé à l’activité Hive effectue les opérations suivantes :

    * Il utilise la table existante (*hivesampletable*) pour créer une autre table **HiveSampleOut**.
    * Il remplit la table **HiveSampleOut** uniquement avec des colonnes spécifiques issues de la table d’origine *hivesampletable*.

3. Le cluster Hadoop HDInsight est supprimé à l’issue du traitement et reste inactif pendant l’intervalle de temps configuré (paramètre timeToLive). Si la tranche de données suivante peut être traitée au cours de cette durée d’inactivité timeToLive, elle est traitée à l’aide du même cluster.  

## <a name="create-a-data-factory"></a>Créer une fabrique de données

1. Connectez-vous au [portail Azure](https://portal.azure.com/).

2. Dans le portail Azure, sélectionnez **Créer une ressource** > **Données + Analytique** > **Data Factory**.

    ![Azure Data Factory sur le portail](./media/hdinsight-hadoop-create-linux-clusters-adf/data-factory-azure-portal.png "Azure Data Factory sur le portail")

2. Entrez ou sélectionnez les valeurs comme indiqué dans la capture d’écran suivante :

    ![Créer une fabrique de données Azure Data Factory à l’aide du portail Azure](./media/hdinsight-hadoop-create-linux-clusters-adf/create-data-factory-portal.png "Créer une fabrique de données Azure Data Factory à l’aide du portail Azure")

    Entrez ou sélectionnez les valeurs suivantes :
    
    |Propriété  |Description  |
    |---------|---------|
    |**Name** |  Entrez un nom pour la fabrique de données. Ce nom doit être globalement unique.|
    |**Abonnement**     |  Sélectionnez votre abonnement Azure. |
    |**Groupe de ressources**     | Sélectionnez **Utiliser l’existant**, puis sélectionnez le groupe de ressources que vous avez créé à l’aide du script PowerShell. |
    |**Version**     | Sélectionnez **V2 (préversion)**. |
    |**Lieu**     | L’emplacement est automatiquement défini sur l’emplacement que vous avez spécifié au moment de la création du groupe de ressources. Pour ce tutoriel, l’emplacement est défini sur **Est des États-Unis 2**. |
    

3. Sélectionnez **Épingler au tableau de bord**, puis sélectionnez **Créer**. Une nouvelle vignette intitulée **Soumission du déploiement en cours** apparaît sur le tableau de bord du Portail. La création d’une fabrique de données peut prendre de 2 à 4 minutes.

    ![Déploiement du modèle en cours](./media/hdinsight-hadoop-create-linux-clusters-adf/deployment-progress-tile.png "Déploiement du modèle en cours") 
 
4. Une fois la fabrique de données créée, le portail affiche la vue d’ensemble de la fabrique de données.

    ![Vue d’ensemble de la fabrique de données Azure Data Factory](./media/hdinsight-hadoop-create-linux-clusters-adf/data-factory-portal-overview.png "Vue d’ensemble de la fabrique de données Azure Data Factory")

5. Sélectionnez **Créer & surveiller** pour lancer le portail de création et de surveillance Azure Data Factory.

## <a name="create-linked-services"></a>Créez des services liés

Dans cette section, vous créez deux services liés au sein de votre fabrique de données.

- Un **service lié au stockage Azure** relie un compte de stockage Azure à la fabrique de données. Ce stockage est utilisé par le cluster HDInsight à la demande. Il contient également le script Hive qui est exécuté sur le cluster.
- Un **service lié HDInsight à la demande**. Azure Data Factory crée un cluster HDInsight et exécute le script Hive automatiquement. Il supprime ensuite le cluster HDInsight une fois que le cluster inactif pendant une période préconfigurée.

###  <a name="create-an-azure-storage-linked-service"></a>Créer un service lié Stockage Azure

1. Dans le volet gauche de la page **Prise en main**, sélectionnez l’icône **Modifier**.

    ![Créer un service lié Azure Data Factory](./media/hdinsight-hadoop-create-linux-clusters-adf/data-factory-edit-tab.png "Créer un service lié Azure Data Factory")

2. Sélectionnez **Connexions** dans le coin inférieur gauche de la fenêtre, puis sélectionnez **+ Nouveau**.

    ![Créer des connexions dans Azure Data Factory](./media/hdinsight-hadoop-create-linux-clusters-adf/data-factory-create-new-connection.png "Créer des connexions dans Azure Data Factory")

3. Dans la boîte de dialogue **Nouveau service lié**, sélectionnez **Stockage Blob Azure**, puis sélectionnez **Continuer**.

    ![Créer un service lié Stockage Azure pour Data Factory](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-storage-linked-service.png "Créer un service lié Stockage Azure pour Data Factory")

4. Fournissez un nom pour le service lié de stockage, sélectionnez le compte Stockage Azure que vous avez créé dans le cadre du script PowerShell, puis sélectionnez **Terminer**.

    ![Fournir un nom pour le service lié Stockage Azure](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-storage-linked-service-details.png "Fournir un nom pour le service lié Stockage Azure")

### <a name="create-an-on-demand-hdinsight-linked-service"></a>Créer un service lié HDInsight à la demande

1. Cliquez de nouveau sur le bouton **+ Nouveau** pour créer un nouveau service lié.

2. Dans la fenêtre **Nouveau service lié**, sélectionnez **Calcul** > **Azure HDInsight**, puis cliquez sur **Continuer**.

    ![Créer un service lié HDInsight pour Azure Data Factory](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-linked-service.png "Créer un service lié HDInsight pour Azure Data Factory")

3. Dans la fenêtre **Nouveau service lié**, fournissez les valeurs requises.

    ![Fournir des valeurs pour le service lié HDInsight](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-linked-service-details.png "Fournir des valeurs pour le service lié HDInsight")

    Entrez les valeurs suivantes et conservez les autres valeurs par défaut.

    | Propriété | Description |
    | --- | --- |
    | NOM | Entrez un nom pour le service lié HDInsight. |
    | type | Sélectionnez **HDInsight à la demande**. |
    | Service lié Stockage Azure | Sélectionnez le service lié Stockage que vous avez préalablement créé. |
    | Type de cluster | Sélectionnez **hadoop**. |
    | Durée de vie | Indiquez la durée pendant laquelle le cluster HDInsight doit être disponible avant d’être automatiquement supprimé.|
    | ID de principal de service | Indiquez l’ID d’application du principal de service Azure Active Directory que vous avez créé dans le cadre des prérequis. |
    | Clé de principal de service | Indiquez la clé d’authentification pour le principal de service Azure Active Directory. |
    | Préfixe du nom du cluster | Indiquez une valeur qui fera office de préfixe pour tous les types de cluster créés par la fabrique de données. |
    | Groupe de ressources | Sélectionnez le groupe de ressources que vous avez créé dans le cadre du script PowerShell utilisé précédemment.| 
    | Nom d’utilisateur SSH du cluster | Entrez un nom d’utilisateur SSH. |
    | Mot de passe SSH du cluster | Indiquez un mot de passe pour l’utilisateur SSH. |

    Sélectionnez **Terminer**.

## <a name="create-a-pipeline"></a>Créer un pipeline

1. Cliquez sur le bouton **+** (plus), puis sélectionnez **Pipeline**.

    ![Créer un pipeline dans Azure Data Factory](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-create-pipeline.png "Créer un pipeline dans Azure Data Factory")

2. Dans la boîte à outils **Activités**, développez l’activité **HDInsight** et faites glisser l’activité **Hive** vers la surface du concepteur de pipeline. Sous l’onglet **Général**, fournissez un nom pour l’activité.

    ![Ajouter des activités à un pipeline Data Factory](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-add-hive-pipeline.png "Ajouter des activités à un pipeline Data Factory")

3. Vérifiez que l’activité Hive est sélectionnée, sélectionnez l’onglet **Cluster HDI** puis, à partir de la liste déroulante **Service lié HDInsight**, sélectionnez le service lié que vous avez créé précédemment pour HDInsight.

    ![Indiquer les détails du cluster HDInsight pour le pipeline](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-hive-activity-select-hdinsight-linked-service.png "Indiquer les détails du cluster HDInsight pour le pipeline")

4. Sélectionnez l’onglet **Script**, puis effectuez les étapes suivantes :

    a. Pour **Service lié au script**, sélectionnez **HDIStorageLinkedService**. Cette valeur est le service lié de stockage que vous avez préalablement créé.

    b. Pour **Chemin d’accès du fichier**, sélectionnez **Parcourir le stockage** et accédez à l’emplacement de l’exemple de script Hive. Si vous avez exécuté le script PowerShell précédemment, cet emplacement doit être `adfgetstarted/hivescripts/hivescript.hql`.

    ![Fournir les détails du script Hive pour le pipeline](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-provide-script-path.png "Fournir les détails du script Hive pour le pipeline")

    c. Sous **Avancé** > **Paramètres**, sélectionnez **Remplir automatiquement à partir du script**. Cette option recherche tous les paramètres dans le script Hive qui requièrent des valeurs à l’exécution. Le script que vous utilisez (**hivescript.hql**) a un paramètre **Output**. Indiquez la valeur dans le format `wasb://<Container>@<StorageAccount>.blob.core.windows.net/outputfolder/` pour pointer vers un dossier existant de votre stockage Azure. Le chemin respecte la casse. Il s’agit du chemin où sera stockée la sortie du script.

    ![Fournir des paramètres pour le script Hive](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-provide-script-parameters.png "Fournir des paramètres pour le script Hive")

5. Sélectionnez **Valider** pour valider le pipeline. Cliquez sur le bouton **>>** flèche droite (>>) pour fermer la fenêtre de validation.

    ![Valider le pipeline Azure Data Factory](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-validate-all.png "Valider le pipeline Azure Data Factory")

5. Enfin, sélectionnez **Tout publier** pour publier les artefacts sur Azure Data Factory.

    ![Publier le pipeline Azure Data Factory](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-publish-pipeline.png "Publier le pipeline Azure Data Factory")

## <a name="trigger-a-pipeline"></a>Déclencher un pipeline

1. Dans la barre d’outils sur la surface du concepteur, sélectionnez **Déclencheur** > **Déclencher maintenant**.

    ![Déclencher le pipeline Azure Data Factory](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-trigger-pipeline.png "Déclencher le pipeline Azure Data Factory")

2. Sélectionnez **Terminer** dans la barre latérale contextuelle.

## <a name="monitor-a-pipeline"></a>Surveiller un pipeline

1. Basculez vers l’onglet **Surveiller** sur la gauche. Vous voyez une exécution du pipeline dans la liste **Exécutions du pipeline**. Notez l’état de l’exécution dans la colonne **État**.

    ![Surveiller le pipeline Azure Data Factory](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-monitor-pipeline.png "Surveiller le pipeline Azure Data Factory")

2. Sélectionnez **Actualiser** pour actualiser l’état.

3. Vous pouvez également sélectionner l’icône **Afficher les exécutions d’activités** pour voir l’exécution d’activité associée au pipeline. Dans la capture d’écran ci-après, vous ne voyez qu’une seule exécution d’activité, car il n’y a qu’une seule activité dans le pipeline que vous avez créé. Pour revenir à la vue précédente, sélectionnez **Pipelines** en haut de la page.

    ![Surveiller l’activité du pipeline Azure Data Factory](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-monitor-pipeline-activity.png "Surveiller l’activité du pipeline Azure Data Factory")


## <a name="verify-the-output"></a>Vérifier la sortie

1. Pour vérifier la sortie, dans le portail Azure, accédez au compte de stockage que vous avez utilisé pour ce tutoriel. Vous devez voir les dossiers ou conteneurs suivants :

    - Vous voyez un emplacement **adfgerstarted/outputfolder** qui contient la sortie du script Hive qui a été exécuté dans le cadre du pipeline.

    - Vous voyez un conteneur **adfhdidatafactory-\<nom_service_lié>-\<horodatage>**. Ce conteneur est l’emplacement de stockage par défaut du cluster HDInsight qui a été créé dans le cadre de l’exécution du pipeline.

    - Vous voyez un conteneur **adfjobs** qui contient les journaux des tâches Azure Data Factory.  

        ![Vérifier la sortie du pipeline Azure Data Factory](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-verify-output.png "Vérifier la sortie du pipeline Azure Data Factory")


## <a name="clean-up-the-tutorial"></a>Nettoyage du didacticiel

Avec la création de cluster HDInsight à la demande, vous n’avez pas besoin de supprimer explicitement le cluster HDInsight. Le cluster est supprimé en fonction de la configuration que vous avez fournie durant la création du pipeline. Toutefois, la suppression du cluster n’entraîne pas celle des comptes de stockage associés à celui-ci. Ce comportement par défaut permet de préserver vos données. Toutefois, si vous ne souhaitez pas conserver les données, vous pouvez supprimer le compte de stockage que vous avez créé.

Vous pouvez également supprimer le groupe de ressources entier que vous avez créé pour ce tutoriel. Cette opération supprime le compte de stockage et la fabrique de données Azure Data Factory que vous avez créés.

### <a name="delete-the-resource-group"></a>Supprimer le groupe de ressources

1. Connectez-vous au [Portail Azure](https://portal.azure.com).
2. Sélectionnez **Groupes de ressources** dans le volet de gauche.
3. Sélectionnez le nom du groupe de ressources que vous avez créé dans votre script PowerShell. Utilisez le filtre si la liste des groupes de ressources est trop longue. Le groupe de ressources s’ouvre.
4. Dans la mosaïque **Ressources**, vous devez voir le compte de stockage par défaut et la fabrique de données, sauf si vous partagez le groupe de ressources avec d’autres projets.
5. Sélectionnez **Supprimer le groupe de ressources**. Ce faisant, vous supprimez le compte de stockage et les données stockées dans ce dernier.

    ![Supprimer le groupe de ressources](./media/hdinsight-hadoop-create-linux-clusters-adf/delete-resource-group.png "Supprimer le groupe de ressources")

6. Entrez le nom du groupe de ressources pour confirmer la suppression, puis sélectionnez **Supprimer**.


## <a name="next-steps"></a>Étapes suivantes
Dans cet article, vous avez appris à utiliser Azure Data Factory pour créer un cluster HDInsight à la demande et exécuter des travaux Hive. Passez au prochain article pour apprendre à créer des clusters HDInsight avec une configuration personnalisée.

> [!div class="nextstepaction"]
>[Créer des clusters Azure HDInsight avec une configuration personnalisée](hdinsight-hadoop-provision-linux-clusters.md)


