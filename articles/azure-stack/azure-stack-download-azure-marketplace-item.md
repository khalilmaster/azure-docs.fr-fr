---
title: Télécharger des éléments de la Place de marché à partir d’Azure | Microsoft Docs
description: L’opérateur de cloud peut télécharger des éléments de la Place de marché à partir d’Azure dans mon déploiement Azure Stack.
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/16/2018
ms.author: brenduns
ms.reviewer: jeffgo
ms.openlocfilehash: 5d403f7c1d0fff466f6c0fb9942ec777ab820eab
ms.sourcegitcommit: 680964b75f7fff2f0517b7a0d43e01a9ee3da445
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34604530"
---
# <a name="download-marketplace-items-from-azure-to-azure-stack"></a>Télécharger des éléments de la Place de marché à partir d’Azure dans Azure Stack

*S’applique à : systèmes intégrés Azure Stack et Kit de développement Azure Stack*

En tant qu’opérateur cloud, téléchargez des éléments à partir de la Place de marché Azure et rendez-les disponibles dans Azure Stack. Les éléments que vous pouvez choisir font partie d’une liste d’éléments de la Place de marché Azure testés au préalable et prenant en charge le travail avec Azure Stack. De nouveaux éléments sont fréquemment ajoutés à cette liste, consultez-la régulièrement pour voir de nouveaux contenus. 

Il existe deux scénarios pour la connexion à la Place de marché Azure : 

- **Un scénario connecté** : il demande que votre environnement Azure Stack soit connecté à internet. Vous utilisez le portail Azure Stack pour localiser et télécharger des éléments. 
- **Un scénario déconnecté ou partiellement connecté** : il vous oblige à accéder à internet à l’aide de l’outil de syndication Place de marché pour télécharger des éléments de la Place de marché. Ensuite, vous transférez vos téléchargements pour votre installation déconnectée de Azure Stack. Ce scénario utilise PowerShell.

Consultez [Éléments Place de marché Azure pour Azure Stack](azure-stack-marketplace-azure-items.md) pour obtenir la liste des éléments de la Place de marché que vous pouvez télécharger.


## <a name="connected-scenario"></a>Scénario connecté
Si Azure Stack se connecte à internet, vous pouvez utiliser le portail d’administration pour télécharger des éléments de la Place de marché.

### <a name="prerequisites"></a>Prérequis
Votre déploiement Azure Stack doit être connecté à internet et être [inscrit auprès d’Azure](azure-stack-register.md).

### <a name="use-the-portal-to-download-marketplace-items"></a>Utiliser le portail pour télécharger les éléments de la Place de marché  
1. Connectez-vous au portail d’administration d’Azure Stack.

2.  Vérifiez l’espace de stockage disponible avant de télécharger des éléments de la Place de marché. Plus tard, lorsque vous sélectionnez des éléments pour le téléchargement, vous pouvez comparer la taille du téléchargement avec votre capacité de stockage disponible. Si la capacité est limitée, envisagez des options pour [gérer l’espace disponible](azure-stack-manage-storage-shares.md#manage-available-space). 

    Pour consulter l’espace disponible dans **Gestion de la région**, sélectionnez la région que vous souhaitez explorer, puis accédez à **Fournisseurs de ressources** > **Stockage**.

    ![Revoir l’espace de stockage](media/azure-stack-download-azure-marketplace-item/storage.png)  

    
3. Ouvrez Azure Stack Marketplace et connectez-vous à Azure. Pour ce faire, sélectionnez **Gestion de la Place de marché**, puis sélectionnez **Ajouter à partir d’Azure**.

    ![Ajout à partir d’Azure](media/azure-stack-download-azure-marketplace-item/marketplace.png)

    Le portail affiche la liste des éléments disponibles en téléchargement à partir de la Place de marché Azure. Vous pouvez cliquer sur chaque élément pour afficher sa description et les informations supplémentaires le concernant, y compris sa taille de téléchargement. 

    ![Liste de la Place de marché](media/azure-stack-download-azure-marketplace-item/image03.png)

4. Sélectionnez les éléments que vous souhaitez, puis sélectionnez **Télécharger**. Le temps de téléchargement varie.

    ![Message de téléchargement](media/azure-stack-download-azure-marketplace-item/image04.png)

    Une fois le téléchargement terminé, vous pouvez déployer le nouvel élément de Place de marché en tant qu’opérateur ou utilisateur Azure Stack.

5. Pour déployer l’élément téléchargé, sélectionnez **+ Nouveau** puis recherchez le nouvel élément de Place de marché parmi les catégories. Cliquez ensuite sur l’élément pour commencer le processus de déploiement. Le processus varie pour les différents éléments de la Place de marché. 

## <a name="disconnected-or-a-partially-connected-scenario"></a>Scénario déconnecté ou partiellement connecté

Si Azure Stack est en mode déconnecté et sans connexion internet, vous utilisez PowerShell et l’*outil de syndication Place de marché* pour télécharger les éléments de la Place de marché sur une machine avec une connexion internet. Vous transférez alors les éléments vers votre environnement Azure Stack. Dans un environnement déconnecté, vous ne pouvez pas télécharger les éléments de la Place de marché en utilisant le portail Azure Stack. 

L’outil de syndication Place de marché peut également être utilisé dans un scénario connecté. 

Ce scénario comporte deux parties :
- **Partie 1 :** télécharger à partir de la Place de marché Azure. Sur l’ordinateur avec un accès internet, vous configurez PowerShell, téléchargez l’outil de syndication et puis vous téléchargez les éléments à partir de la Place de marché Azure.  
- **Partie 2 :** télécharger et publier dans la Place de marché Azure Stack. Vous déplacez les fichiers que vous avez téléchargés dans votre environnement Azure Stack, vous les importez dans Azure Stack, puis vous les publiez dans la Place de marché Azure.  


### <a name="prerequisites"></a>Prérequis
- Votre déploiement Azure Stack doit être [inscrit auprès d’Azure](azure-stack-register.md).  

- L’ordinateur qui possède une connexion internet doit disposer du **module PowerShell pour Azure Stack version 1.2.11** ou une version ultérieure. S’il est déjà présent, [installez des modules PowerShell spécifiques à Azure Stack](azure-stack-powershell-install.md).  

- Pour activer l’importation d’un élément de la Place de marché téléchargé, l’[environnement PowerShell pour l’opérateur Azure Stack](azure-stack-powershell-configure-admin.md) doit être configuré.  

- Vous devez disposer d’un [compte de stockage](azure-stack-manage-storage-accounts.md) dans Azure Stack disposant d’un conteneur accessible publiquement (qui est un objet blob de stockage). Vous utilisez le conteneur comme un stockage temporaire pour les fichiers de la galerie des éléments de la Place de marché. Si vous n’êtes pas familiarisé avec les comptes de stockage et les conteneurs, consultez [Travailler avec des objets blob - portail Azure](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-portal) dans la documentation Azure.

- L’outil de syndication Place de marché est téléchargé lors de la première procédure. 

### <a name="use-the-marketplace-syndication-tool-to-download-marketplace-items"></a>Utiliser l’outil de syndication Place de marché pour télécharger les éléments de la Place de marché

1. Sur un ordinateur avec une connexion Internet, ouvrez une console PowerShell en tant qu’administrateur.

2. Ajoutez le compte Azure que vous avez utilisé pour inscrire Azure Stack. Pour ajouter le compte, exécutez `Add-AzureRmAccount` sans aucun paramètre dans PowerShell. Vous êtes invité à entrer vos informations d’identification de compte Azure et vous devrez peut-être utiliser l’authentification à 2 facteurs en fonction de la configuration de votre compte.

3. Si vous avez plusieurs abonnements, exécutez la commande suivante pour sélectionner celui que vous avez utilisé pour l’inscription :  

   ```PowerShell  
   Get-AzureRmSubscription -SubscriptionID '<Your Azure Subscription GUID>' | Select-AzureRmSubscription
   $AzureContext = Get-AzureRmContext
   ```

4. Téléchargez la dernière version de l’outil de syndication de la Place de marché à l’aide du script suivant :  

   ```PowerShell
   # Download the tools archive.
   [Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12 
   invoke-webrequest https://github.com/Azure/AzureStack-Tools/archive/master.zip `
     -OutFile master.zip

   # Expand the downloaded files.
   expand-archive master.zip `
     -DestinationPath . `
     -Force

   # Change to the tools directory.
   cd .\AzureStack-Tools-master

   ```

5. Importez le module de syndication et lancez l’outil en exécutant le script suivant. Remplacez le *chemin d’accès du dossier de destination* avec un emplacement pour stocker les fichiers que vous téléchargez à partir de la Place de marché Azure.   

   ```PowerShell  
   Import-Module .\Syndication\AzureStack.MarketplaceSyndication.psm1

   Sync-AzSOfflineMarketplaceItem `
     -destination "Destination folder path" `
     -AzureTenantID $AzureContext.Tenant.TenantId `
     -AzureSubscriptionId $AzureContext.Subscription.Id  
   ```

6. Une fois l’outil exécuté, vous êtes invité à entrer les informations d’identification de votre compte Azure. Connectez-vous au compte Azure que vous avez utilisé pour inscrire Azure Stack. Une fois connecté, vous devriez voir un écran similaire à l’image suivante, avec la liste des éléments de la Place de marché disponibles.  

   ![Fenêtre contextuelle des éléments de la Place de marché Azure](media/azure-stack-download-azure-marketplace-item/image05.png)

7. Sélectionnez l’élément que vous souhaitez télécharger, puis notez la *version*. (Vous pouvez sélectionner plusieurs images en maintenant la touche *Ctrl* enfoncée.) Vous allez référencer la *version* lorsque vous importez l’élément dans la procédure suivante. 
   
   Vous pouvez également filtrer la liste des images à l’aide de l’option **Ajouter des critères**.

8. Sélectionnez **OK**, puis lisez et acceptez les conditions juridiques. 

9. Le temps de téléchargement dépend de la taille de l’élément. Une fois le téléchargement terminé, l’élément est disponible dans le dossier que vous avez spécifié dans le script. Le téléchargement comprend un fichier VHD (pour les machines virtuelles) ou un fichier .ZIP (pour les extensions de machine virtuelle). Il inclut également un package de galerie au format *.azpkg*. (un package *.azpkg* est un fichier *.zip*.)
 

### <a name="import-the-download-and-publish-to-azure-stack-marketplace"></a>Importer le téléchargement et le publier sur la Place de marché Azure Stack
1. Les fichiers pour les images de machine virtuelle ou les modèles de solution que vous avez [précédemment téléchargés](#use-the-marketplace-syndication-tool-to-download-marketplace-items) doivent être accessibles localement dans votre environnement Azure Stack.  

2. Importer des fichiers .VHD dans Azure Stack. Pour importer une image de machine virtuelle, vous devez disposer des informations suivantes sur la machine virtuelle :
   - La *version*, comme indiquée à l’étape 7 de la procédure précédente.
   - Les valeurs de *publisher*, *offer* et *sku* de la machine virtuelle. Pour obtenir ces valeurs, renommer une copie du fichier **.azpkg** pour modifier son extension de fichier en **.zip**. Vous pouvez ensuite utiliser un éditeur de texte pour ouvrir **DeploymentTemplates\CreateUiDefinition.json**. Dans le fichier .json, recherchez la section *imageReference* qui contient ces valeurs pour l’élément de la Place de marché. L’exemple suivant illustre l’affichage de ces informations :

     ```json  
     "imageReference": {  
        "publisher": "MicrosoftWindowsServer",  
        "offer": "WindowsServer",  
        "sku": "2016-Datacenter-Server-Core"  
      }
     ```  

   Importez l’image dans Azure Stack à l’aide de l’applet de commande **Add-AzsPlatformimage**. Lorsque vous utilisez cette cmdlet, veillez à remplacer les valeurs *publisher*, *offer* et les autres valeurs de paramètre par les valeurs de l’image que vous importez. Vous pouvez obtenir les valeurs *publisher*, *offer*, et *sku* de l’image à partir du fichier texte téléchargé avec le fichier AZPKG et stocké dans l’emplacement de destination. 

   Dans l’exemple de script suivant, les valeurs pour la machine virtuelle Server Core (Windows Server 2016 Datacenter) sont utilisées. 

   ```PowerShell  
   Add-AzsPlatformimage `
    -publisher "MicrosoftWindowsServer" `
    -offer "WindowsServer" `
    -sku "2016-Datacenter-Server-Core" `
    -osType Windows `
    -Version "2016.127.20171215" `
    -OsDiskLocalPath "C:\AzureStack-Tools-master\Syndication\Windows-Server-2016-DatacenterCore-20171215-en.us-127GB.vhd" `
   ```
   **À propos des modèles de solution :** certains modèles peuvent inclure un petit fichier VHD d’environ 3 Mo portant le nom **fixed3.vhd**. Vous n’avez pas besoin d’importer ce fichier dans Azure Stack. Fixed3.vhd.  Ce fichier est inclus avec certains modèles de solution afin de répondre aux exigences de publication pour Place de marché Azure.

   Consultez la description des modèles et téléchargez puis importez des exigences supplémentaires comme des disques durs virtuels requis pour fonctionner avec le modèle de solution.

3. Utilisez le portail d’administration pour télécharger le package d’élément de Place de marché (fichier .azpkg) pour le stockage d’objets Blob d’Azure Stack. Le téléchargement du package le rend disponible pour Azure Stack afin que vous puissiez publier l’élément ultérieurement sur la Place de marché Azure Stack.

   Le téléchargement nécessite un compte de stockage avec un conteneur publiquement accessible (voir la configuration requise pour ce scénario)   
   1. Dans le portail administrateur d’Azure Stack, allez dans **Plus de services** > **Comptes de stockage**.  
   
   2. Sélectionnez un compte de stockage de votre abonnement, puis sous **SERVICE BLOB**, sélectionnez **Conteneurs**.  
      ![Service Blob](media/azure-stack-download-azure-marketplace-item/blob-service.png)  
   
   3. Sélectionnez le conteneur que vous souhaitez utiliser, puis sélectionnez **Télécharger** pour ouvrir le volet **Télécharger des objets blob**.  
      ![Conteneur](media/azure-stack-download-azure-marketplace-item/container.png)  
   
   4. Dans le volet Télécharger des objets blob, recherchez les fichiers que vous souhaitez charger dans le stockage, puis sélectionnez **Télécharger**.  
      ![upload](media/azure-stack-download-azure-marketplace-item/upload.png)  

   5. Les fichiers que vous téléchargez apparaissent dans le volet du conteneur. Sélectionnez un fichier, puis copiez l’URL à partir du volet **Propriétés d’objets blob**. Vous allez utiliser cette URL à l’étape suivante lorsque vous importez l’élément de Place de marché dans Azure Stack.  Dans l’image suivante, le conteneur est *blob-test-storage* et le fichier est *Microsoft.WindowsServer2016DatacenterServerCore-ARM.1.0.801.azpkg*.  L’URL du fichier est *https://testblobstorage1.blob.local.azurestack.external/blob-test-storage/Microsoft.WindowsServer2016DatacenterServerCore-ARM.1.0.801.azpkg*.  
      ![Propriétés d'objets blob](media/azure-stack-download-azure-marketplace-item/blob-storage.png)  

4.  Utilisez PowerShell pour publier l’élément de Place de marché sur Azure Stack en utilisant l’applet de commande **Add-AzsGalleryItem**. Par exemple :   
    ```PowerShell  
    Add-AzsGalleryItem `
     -GalleryItemUri "https://mystorageaccount.blob.local.azurestack.external/cont1/Microsoft.WindowsServer2016DatacenterServerCore-ARM.1.0.801.azpkg" `
     –Verbose
    ```
5. Une fois que vous publiez un élément de la galerie, vous pouvez l’afficher depuis **Plus de services** > **Place de marché**.  Si vous avez téléchargé un modèle de solution, veillez également à ajouter toute image VHD dépendante pour ce modèle solution.  
  ![Afficher la Place de marché](media/azure-stack-download-azure-marketplace-item/view-marketplace.png)  

> [!NOTE]
> Avec la version 1.3.0 de Azure Stack PowerShell, vous pouvez maintenant ajouter des extensions de machine virtuelle.

Par exemple : 

````PowerShell
Add-AzsVMExtension -Publisher "Microsoft" -Type "MicroExtension" -Version "0.1.0" -ComputeRole "IaaS" -SourceBlob "https://github.com/Microsoft/PowerShell-DSC-for-Linux/archive/v1.1.1-294.zip" -SupportMultipleExtensions -VmOsType "Linux"
````

## <a name="next-steps"></a>Étapes suivantes
[Créer et publier un élément de Place de Marché](azure-stack-create-and-publish-marketplace-item.md)