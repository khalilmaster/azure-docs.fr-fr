---
title: Exécuter un package SSIS avec l’activité Exécuter le Package SSIS - Azure | Microsoft Docs
description: Cet article décrit comment exécuter un package SQL Server Integration Services (SSIS) dans un pipeline Azure Data Factory à l’aide de l’activité Exécuter le Package SSIS.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: ''
ms.devlang: powershell
ms.topic: conceptual
ms.date: 07/09/2018
author: swinarko
ms.author: sawinark
ms.reviewer: douglasl
manager: craigg
ms.openlocfilehash: db5941528eedd10cf252607dbe2160bd498a70de
ms.sourcegitcommit: a1e1b5c15cfd7a38192d63ab8ee3c2c55a42f59c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37951965"
---
# <a name="run-an-ssis-package-with-the-execute-ssis-package-activity-in-azure-data-factory"></a>Exécuter un package SSIS avec l’activité Exécuter le Package SSIS dans Azure Data Factory
Cet article décrit comment exécuter un package SSIS dans un pipeline Azure Data Factory à l’aide d’une activité Exécuter le Package SSIS. 

## <a name="prerequisites"></a>Prérequis

### <a name="azure-sql-database"></a>Azure SQL Database 
La procédure pas à pas dans cet article utilise une base de données Azure SQL qui héberge le catalogue SSIS. Vous pouvez également utiliser Azure SQL Managed Instance (préversion).

## <a name="create-an-azure-ssis-integration-runtime"></a>Créer un runtime d’intégration Azure-SSIS
Créez un runtime d’intégration Azure-SSIS si vous n’en avez pas en suivant les instructions pas à pas fournies dans le [Didacticiel : Déployer des packages SSIS](tutorial-create-azure-ssis-runtime-portal.md).

## <a name="data-factory-ui-azure-portal"></a>Interface utilisateur de Data Factory (portail Azure)
Dans cette section, vous utilisez l’interface utilisateur de Data Factory pour créer un pipeline Data Factory avec une activité Exécuter le Package SSIS qui exécute un package SSIS.

### <a name="create-a-data-factory"></a>Créer une fabrique de données
La première étape consiste à créer une fabrique de données à l’aide du portail Azure. 

1. Lancez le navigateur web **Microsoft Edge** ou **Google Chrome**. L’interface utilisateur de Data Factory n’est actuellement prise en charge que par les navigateurs web Microsoft Edge et Google Chrome.
2. Accédez au [portail Azure](https://portal.azure.com). 
3. Cliquez sur **Nouveau** dans le menu de gauche, puis sur **Données + analyse** et sur **Data Factory**. 
   
   ![Nouveau -> DataFactory](./media/how-to-invoke-ssis-package-stored-procedure-activity/new-azure-data-factory-menu.png)
2. Dans la page **Nouvelle fabrique de données**, entrez **ADFTutorialDataFactory** comme **nom**. 
      
     ![Page de nouvelle fabrique de données](./media/how-to-invoke-ssis-package-stored-procedure-activity/new-azure-data-factory.png)
 
   Le nom de la fabrique de données Azure doit être un nom **global unique**. Si l’erreur suivante s’affiche pour le champ du nom, changez le nom de la fabrique de données (par exemple, votrenomADFTutorialDataFactory). Consultez l’article [Data Factory - Règles d’affectation des noms](naming-rules.md) pour savoir comment nommer les artefacts Data Factory.
  
     ![Nom indisponible - erreur](./media/how-to-invoke-ssis-package-stored-procedure-activity/name-not-available-error.png)
3. Sélectionnez l’**abonnement** Azure dans lequel vous voulez créer la fabrique de données. 
4. Pour le **groupe de ressources**, effectuez l’une des opérations suivantes :
     
      - Sélectionnez **Utiliser l’existant**, puis sélectionnez un groupe de ressources existant dans la liste déroulante. 
      - Sélectionnez **Créer**, puis entrez le nom d’un groupe de ressources.   
         
    Pour plus d’informations sur les groupes de ressources, consultez [Utilisation des groupes de ressources pour gérer vos ressources Azure](../azure-resource-manager/resource-group-overview.md).  
4. Sélectionnez **V2** pour la **version**.
5. Sélectionnez **l’emplacement** de la fabrique de données. Seuls les emplacements pris en charge par Data Factory sont affichés dans la liste déroulante. Les magasins de données (Stockage Azure, Azure SQL Database, etc.) et les services de calcul (HDInsight, etc.) utilisés par la fabrique de données peuvent se trouver dans d’autres emplacements.
6. Sélectionnez **Épingler au tableau de bord**.     
7. Cliquez sur **Créer**.
8. Sur le tableau de bord, vous voyez la mosaïque suivante avec l’état : **Déploiement de fabrique de données**. 

    ![mosaïque déploiement de fabrique de données](media//how-to-invoke-ssis-package-stored-procedure-activity/deploying-data-factory.png)
9. Une fois la création terminée, la page **Data Factory** s’affiche comme sur l’image.
   
    ![Page d’accueil Data Factory](./media/how-to-invoke-ssis-package-stored-procedure-activity/data-factory-home-page.png)
10. Cliquez sur la vignette **Créer et surveiller** pour lancer l’application d’interface utilisateur (IU) d’Azure Data Factory dans un onglet séparé. 

### <a name="create-a-pipeline-with-an-execute-ssis-package-activity"></a>Créer un pipeline avec une activité Exécuter le Package SSIS
Lors de cette étape, vous utilisez l’interface utilisateur de Data Factory pour créer un pipeline. Vous ajoutez une activité Exécuter le Package SSIS au pipeline et la configurez pour exécuter le package SSIS. 

1. Dans la page de prise en main, cliquez sur **Créer un pipeline** : 

    ![Page de prise en main](./media/how-to-invoke-ssis-package-stored-procedure-activity/get-started-page.png)
2. Dans la boîte à outils **Activités**, développez **Général**, puis faites un glisser-déplacer de l’activité **Exécuter le package SSIS** vers la surface du concepteur de pipeline. 

   ![Faire glisser l’activité SSIS vers la surface du concepteur](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-designer.png) 

3. Sous l’onglet **Général** des propriétés de l’activité Exécuter le Package SSIS, fournissez un nom et une description pour l’activité. Définissez les valeurs de délai d’attente et de nouvelle tentative facultatives.

    ![Définissez les propriétés sous l’onglet Général](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-general.png)

4. Sous l’onglet **Paramètres** des propriétés de l’activité Exécuter le Package SSIS, sélectionnez le runtime d’intégration Azure-SSIS associé à la base de données `SSISDB` sur laquelle le package est déployé. Fournissez le chemin du package dans la base de données `SSISDB` en utilisant le format `<folder name>/<project name>/<package name>.dtsx`. Si vous le souhaitez, vous pouvez spécifier une exécution 32 bits et un niveau de journalisation prédéfini ou personnalisé et fournir un chemin d’environnement en utilisant le format `<folder name>/<environment name>`.

    ![Définir les propriétés sous l’onglet Paramètres](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-settings.png)

5. Pour valider la configuration du pipeline, cliquez sur **Valider** dans la barre d’outils. Pour fermer le **Rapport de validation de pipeline**, cliquez sur **>>**.

6. Publiez le pipeline dans Data Factory en cliquant sur le bouton **Publier tout**. 

### <a name="optionally-parameterize-the-activity"></a>Le cas échéant, paramétrer l’activité

Si vous le souhaitez, vous pouvez attribuer des valeurs, des expressions ou des fonctions pouvant faire référence à des variables système Data Factory, aux paramètres de votre projet ou de votre package, au format JSON. Pour cela, utilisez le bouton **Afficher le code source** situé au bas de la fenêtre d’activité Exécuter le package SSIS ou le bouton **Code** en haut à droite de la zone de pipeline. Par exemple, vous pouvez affecter des paramètres de pipeline Data Factory aux paramètres de votre projet SSIS ou de votre package, comme indiqué dans les captures d’écran suivantes :

![Modifier le script JSON pour l’activité Exécuter le package SSIS](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-parameters.png)

![Ajouter des paramètres à l’activité Exécuter le Package SSIS](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-parameters2.png)

### <a name="run-and-monitor-the-pipeline"></a>Exécuter et surveiller le pipeline
Dans cette section, vous déclenchez une exécution du pipeline puis vous la surveillez. 

1. Pour déclencher une exécution de pipeline, cliquez sur **Déclencher** dans la barre d’outils, puis sur **Déclencher maintenant**. 

    ![Déclencher maintenant](./media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-trigger.png)

2. Dans la fenêtre **Exécution du pipeline**, sélectionnez **Terminer**. 

3. Basculez vers l’onglet **Surveiller** sur la gauche. Vous voyez l’exécution de pipeline et son état, ainsi que d’autres informations (telles que l’heure de début d’exécution). Pour actualiser la vue, cliquez sur **Actualiser**.

    ![Exécutions de pipeline](./media/how-to-invoke-ssis-package-stored-procedure-activity/pipeline-runs.png)

4. Cliquez sur le lien **Afficher les exécutions d’activités** dans la colonne **Actions**. Une seule exécution d’activité est affichée, étant donné que le pipeline n’a qu’une seule activité (l’activité Exécuter le Package SSIS).

    ![Exécutions d’activités](./media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-runs.png)

5. Vous pouvez exécuter la **requête** suivante par rapport à la base de données SSISDB dans votre serveur SQL Azure pour vérifier que le package s’est exécuté. 

    ```sql
    select * from catalog.executions
    ```

    ![Vérifier les exécutions de package](./media/how-to-invoke-ssis-package-stored-procedure-activity/verify-package-executions.png)

6. Vous pouvez également obtenir l’ID de l’exécution SSISDB à partir de la sortie de l’exécution de l’activité de pipeline, et utiliser l’ID pour consulter des journaux d’exécution et des messages d’erreur plus complets dans SSMS.

    ![Obtenez l’ID de l'exécution.](media/how-to-invoke-ssis-package-ssis-activity/get-execution-id.png)

> [!NOTE]
> Vous pouvez également créer un déclencheur planifié pour votre pipeline afin que le pipeline s’exécute selon une planification (horaire, quotidienne, et ainsi de suite). Pour obtenir un exemple, consultez [Créer une fabrique de données - Interface utilisateur de Data Factory](quickstart-create-data-factory-portal.md#trigger-the-pipeline-on-a-schedule).

## <a name="azure-powershell"></a>Azure PowerShell
Dans cette section, vous utilisez Azure PowerShell pour créer un pipeline Data Factory avec une activité SSIS qui exécute un package SSIS. 

Installez les modules Azure PowerShell les plus récents en suivant les instructions décrites dans [Comment installer et configurer Azure PowerShell](/powershell/azure/install-azurerm-ps). 

### <a name="create-a-data-factory"></a>Créer une fabrique de données
Vous pouvez utiliser la fabrique de données qui a le runtime d’intégration Azure-SSIS ou créer une fabrique de données distincte. La procédure suivante décrit les étapes permettant de créer une fabrique de données. Vous créez un pipeline avec une activité SSIS dans cette fabrique de données. L’activité SSIS exécute votre package SSIS. 

1. Définissez une variable pour le nom du groupe de ressources que vous utiliserez ultérieurement dans les commandes PowerShell. Copiez le texte de commande suivant dans PowerShell, spécifiez un nom pour le [groupe de ressources Azure](../azure-resource-manager/resource-group-overview.md) entre des guillemets doubles, puis exécutez la commande. Par exemple : `"adfrg"`. 
   
     ```powershell
    $resourceGroupName = "ADFTutorialResourceGroup";
    ```

    Si le groupe de ressources existe déjà, vous pouvez ne pas le remplacer. Affectez une valeur différente à la variable `$ResourceGroupName` et exécutez à nouveau la commande
2. Pour créer le groupe de ressources Azure, exécutez la commande suivante : 

    ```powershell
    $ResGrp = New-AzureRmResourceGroup $resourceGroupName -location 'eastus'
    ``` 
    Si le groupe de ressources existe déjà, vous pouvez ne pas le remplacer. Affectez une valeur différente à la variable `$ResourceGroupName` et exécutez à nouveau la commande. 
3. Définissez une variable pour le nom de la fabrique de données. 

    > [!IMPORTANT]
    >  Mettez à jour le nom de la fabrique de données afin qu’il soit globalement unique. 

    ```powershell
    $DataFactoryName = "ADFTutorialFactory";
    ```

5. Pour créer la fabrique de données, exécutez l’applet de commande suivante **Set-AzureRmDataFactoryV2**, à l’aide des propriétés Location et ResourceGroupName à partir de la variable $ResGrp : 
    
    ```powershell       
    $DataFactory = Set-AzureRmDataFactoryV2 -ResourceGroupName $ResGrp.ResourceGroupName `
                                            -Location $ResGrp.Location `
                                            -Name $dataFactoryName 
    ```

Notez les points suivants :

* Le nom de la fabrique de données Azure doit être un nom global unique. Si vous recevez l’erreur suivante, changez le nom, puis réessayez.

    ```
    The specified Data Factory name 'ADFv2QuickStartDataFactory' is already in use. Data Factory names must be globally unique.
    ```
* Pour créer des instances de fabrique de données, le compte d’utilisateur que vous utilisez pour vous connecter à Azure doit être un membre des rôles **contributeur** ou **propriétaire**, ou un **administrateur** de l’abonnement Azure.
* Pour obtenir la liste des régions Azure dans lesquelles Data Factory est actuellement disponible, sélectionnez les régions qui vous intéressent sur la page suivante, puis développez **Analytique** pour localiser **Data Factory** : [Disponibilité des produits par région](https://azure.microsoft.com/global-infrastructure/services/). Les magasins de données (Stockage Azure, Azure SQL Database, etc.) et les services de calcul (HDInsight, etc.) utilisés par la fabrique de données peuvent se trouver dans d’autres régions.

### <a name="create-a-pipeline-with-an-ssis-activity"></a>Créer un pipeline avec une activité SSIS 
Dans cette étape, vous allez créer un pipeline avec une activité SSIS. L’activité exécute votre package SSIS. 

1. Créez un fichier JSON nommé **RunSSISPackagePipeline.json** dans le dossier **C:\ADF\RunSSISPackage** avec un contenu similaire à l’exemple suivant :

    > [!IMPORTANT]
    > Remplacez les noms d’objets, les descriptions et les chemins, ainsi que les valeurs de propriété et de paramètre, les mots de passe et autres valeurs de variable avant d’enregistrer le fichier. 

    ```json
    {
        "name": "RunSSISPackagePipeline",
        "properties": {
            "activities": [{
                "name": "mySSISActivity",
                "description": "My SSIS package/activity description",
                "type": "ExecuteSSISPackage",
                "typeProperties": {
                    "connectVia": {
                        "referenceName": "myAzureSSISIR",
                        "type": "IntegrationRuntimeReference"
                    },
                    "runtime": "x64",
                    "loggingLevel": "Basic",
                    "packageLocation": {
                        "packagePath": "FolderName/ProjectName/PackageName.dtsx"            
                    },
                    "environmentPath":   "FolderName/EnvironmentName",
                    "projectParameters": {
                        "project_param_1": {
                            "value": "123"
                        }
                    },
                    "packageParameters": {
                        "package_param_1": {
                            "value": "345"
                        }
                    },
                    "projectConnectionManagers": {
                        "MyAdonetCM": {
                            "userName": {
                                "value": "sa"
                            },
                            "passWord": {
                                "value": {
                                    "type": "SecureString",
                                    "value": "abc"
                                }
                            }
                        }
                    },
                    "packageConnectionManagers": {
                        "MyOledbCM": {
                            "userName": {
                                "value": "sa"
                            },
                            "passWord": {
                                "value": {
                                    "type": "SecureString",
                                    "value": "def"
                                }
                            }
                        }
                    },
                    "propertyOverrides": {
                        "\\PackageName.dtsx\\MaxConcurrentExecutables ": {
                            "value": 8,
                            "isSensitive": false
                        }
                    }
                },
                "policy": {
                    "timeout": "0.01:00:00",
                    "retry": 0,
                    "retryIntervalInSeconds": 30
                }
            }]
        }
    }
    ```

2.  Dans Azure PowerShell, accédez au dossier `C:\ADF\RunSSISPackage`.

3. Pour créer le pipeline **RunSSISPackagePipeline**, exécutez l’applet de commande **Set-AzureRmDataFactoryV2Pipeline**.

    ```powershell
    $DFPipeLine = Set-AzureRmDataFactoryV2Pipeline -DataFactoryName $DataFactory.DataFactoryName `
                                                   -ResourceGroupName $ResGrp.ResourceGroupName `
                                                   -Name "RunSSISPackagePipeline"
                                                   -DefinitionFile ".\RunSSISPackagePipeline.json"
    ```

    Voici l’exemple de sortie :

    ```
    PipelineName      : Adfv2QuickStartPipeline
    ResourceGroupName : <resourceGroupName>
    DataFactoryName   : <dataFactoryName>
    Activities        : {CopyFromBlobToBlob}
    Parameters        : {[inputPath, Microsoft.Azure.Management.DataFactory.Models.ParameterSpecification], [outputPath, Microsoft.Azure.Management.DataFactory.Models.ParameterSpecification]}
    ```

### <a name="create-a-pipeline-run"></a>Créer une exécution du pipeline
Utilisez l’applet de commande **Invoke-AzureRmDataFactoryV2Pipeline** pour exécuter le pipeline. L’applet de commande renvoie l’ID d’exécution du pipeline pour permettre une surveillance ultérieure.

```powershell
$RunId = Invoke-AzureRmDataFactoryV2Pipeline -DataFactoryName $DataFactory.DataFactoryName `
                                             -ResourceGroupName $ResGrp.ResourceGroupName `
                                             -PipelineName $DFPipeLine.Name
```

### <a name="monitor-the-pipeline-run"></a>Surveiller l’exécution du pipeline

Exécutez le script PowerShell suivant afin de vérifier continuellement l’état de l’exécution du pipeline jusqu’à la fin de la copie des données. Copiez/collez le script suivant dans la fenêtre PowerShell et appuyez sur ENTRÉE. 

```powershell
while ($True) {
    $Run = Get-AzureRmDataFactoryV2PipelineRun -ResourceGroupName $ResGrp.ResourceGroupName `
                                               -DataFactoryName $DataFactory.DataFactoryName `
                                               -PipelineRunId $RunId

    if ($Run) {
        if ($run.Status -ne 'InProgress') {
            Write-Output ("Pipeline run finished. The status is: " +  $Run.Status)
            $Run
            break
        }
        Write-Output  "Pipeline is running...status: InProgress"
    }

    Start-Sleep -Seconds 10
}   
```

Vous pouvez également surveiller le pipeline à l’aide du portail Azure. Pour obtenir des instructions pas à pas, consultez [Surveiller le pipeline](quickstart-create-data-factory-resource-manager-template.md#monitor-the-pipeline).

### <a name="create-a-trigger"></a>Créer un déclencheur
À l’étape précédente, vous avez exécuté le pipeline à la demande. Vous pouvez également créer un déclencheur de planification pour exécuter le pipeline d’après une planification (horaire, quotidienne, etc.).

1. Créez un fichier JSON nommé **MyTrigger.json** dans le dossier **C:\ADF\RunSSISPackage** avec le contenu suivant : 

    ```json
    {
        "properties": {
            "name": "MyTrigger",
            "type": "ScheduleTrigger",
            "typeProperties": {
                "recurrence": {
                    "frequency": "Hour",
                    "interval": 1,
                    "startTime": "2017-12-07T00:00:00-08:00",
                    "endTime": "2017-12-08T00:00:00-08:00"
                }
            },
            "pipelines": [{
                    "pipelineReference": {
                        "type": "PipelineReference",
                        "referenceName": "RunSSISPackagePipeline"
                    },
                    "parameters": {}
                }
            ]
        }
    }    
    ```
2. Dans **Azure PowerShell**, basculez vers le dossier **C:\ADF\RunSSISPackage**.
3. Exécutez l’applet de commande **Set-AzureRmDataFactoryV2Trigger** pour créer le déclencheur. 

    ```powershell
    Set-AzureRmDataFactoryV2Trigger -ResourceGroupName $ResGrp.ResourceGroupName `
                                    -DataFactoryName $DataFactory.DataFactoryName `
                                    -Name "MyTrigger" -DefinitionFile ".\MyTrigger.json"
    ```
4. Par défaut, le déclencheur est arrêté. Démarrez-le avec l’applet de commande **Start-AzureRmDataFactoryV2Trigger**. 

    ```powershell
    Start-AzureRmDataFactoryV2Trigger -ResourceGroupName $ResGrp.ResourceGroupName `
                                      -DataFactoryName $DataFactory.DataFactoryName `
                                      -Name "MyTrigger" 
    ```
5. Vérifiez que le déclencheur est démarré en exécutant l’applet de commande **Get-AzureRmDataFactoryV2Trigger**. 

    ```powershell
    Get-AzureRmDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName `
                                    -DataFactoryName $DataFactoryName `
                                    -Name "MyTrigger"     
    ```    
6. Exécutez la commande ci-dessous après l’heure suivante. Par exemple, si l’heure actuelle est 15h25 UTC, exécutez la commande à 16h00 UTC. 
    
    ```powershell
    Get-AzureRmDataFactoryV2TriggerRun -ResourceGroupName $ResourceGroupName `
                                       -DataFactoryName $DataFactoryName `
                                       -TriggerName "MyTrigger" `
                                       -TriggerRunStartedAfter "2017-12-06" `
                                       -TriggerRunStartedBefore "2017-12-09"
    ```

    Vous pouvez exécuter la requête suivante par rapport à la base de données SSISDB dans votre serveur Azure SQL pour vérifier que le package s’est exécuté. 

    ```sql
    select * from catalog.executions
    ```


## <a name="next-steps"></a>Étapes suivantes
Consultez le billet de blog suivant :
-   [Moderniser et étendre vos flux de travail ETL/ELT avec des activités SSIS dans des pipelines ADF](https://blogs.msdn.microsoft.com/ssis/2018/05/23/modernize-and-extend-your-etlelt-workflows-with-ssis-activities-in-adf-pipelines/)
