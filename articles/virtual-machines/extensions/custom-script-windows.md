---
title: Extension de script personnalisé Azure pour Windows | Microsoft Docs
description: Automatiser les tâches de configuration de machine virtuelle Windows à l’aide de l’extension de script personnalisé
services: virtual-machines-windows
documentationcenter: ''
author: danielsollondon
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: f4181fee-7a9d-4a1c-b517-52956f5b7fa1
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 04/24/2018
ms.author: danis
ms.openlocfilehash: 80f9ecd40c5b9504a6554b95bf374046d8253933
ms.sourcegitcommit: 6cf20e87414dedd0d4f0ae644696151e728633b6
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/06/2018
ms.locfileid: "34809775"
---
# <a name="custom-script-extension-for-windows"></a>Extension de script personnalisé pour Windows

L’extension de script personnalisé télécharge et exécute des scripts sur des machines virtuelles Azure. Cette extension est utile pour la configuration post-déploiement, l’installation de logiciels ou toute autre tâche de configuration ou de gestion. Des scripts peuvent être téléchargés à partir de Stockage Azure ou de GitHub, ou fournis dans le portail Azure lors de l’exécution de l’extension. L’extension de script personnalisé s’intègre aux modèles Azure Resource Manager et peut être exécutée à l’aide de l’interface de ligne de commande Azure, de PowerShell, du portail Azure ou de l’API REST de machine virtuelle Azure.

Ce document explique en détail l’utilisation de l’extension de script personnalisé à l’aide du module Azure PowerShell, des modèles Azure Resource Manager, et détaille également les étapes de résolution de problèmes sur les systèmes Windows.

## <a name="prerequisites"></a>Prérequis

> [!NOTE]  
> N’utilisez pas l’extension de script personnalisé pour exécuter Update-AzureRmVM avec la même machine virtuelle en tant que paramètre, car elle s’attendra elle-même.  
>   
> 

### <a name="operating-system"></a>Système d’exploitation

L’extension de script personnalisé pour Linux s’exécute sur les systèmes d’exploitation pris en charge par l’extension. Pour en savoir plus, voir cet [article](https://support.microsoft.com/en-us/help/4078134/azure-extension-supported-operating-systems).

### <a name="script-location"></a>Emplacement du script

L’extension vous permet d’utiliser vos informations d’identification de stockage d’objets blob Azure pour accéder au stockage d’objets blob Azure. Par ailleurs, le script peut être placé n’importe où, tant que la machine virtuelle peut effectuer le routage vers ce point de terminaison, par exemple GitHub, un serveur de fichiers interne, etc.


### <a name="internet-connectivity"></a>Connectivité Internet
Si vous devez télécharger un script en externe, par exemple à partir de GitHub ou du stockage Azure, vous devez ouvrir des ports de pare-feu/de groupe de sécurité réseau supplémentaires. Par exemple, si votre script se trouve dans le Stockage Azure, vous pouvez en autoriser l’accès à l’aide de balises de service du groupe de sécurité réseau Azure pour le [Stockage](https://docs.microsoft.com/azure/virtual-network/security-overview#service-tags).

Si votre script se trouve sur un serveur local, vous devrez peut-être encore ouvrir des ports de pare-feu/de groupe de sécurité réseau supplémentaires.

### <a name="tips-and-tricks"></a>Astuces et conseils
* Le taux d’échec le plus élevé de cette extension est dû aux erreurs de syntaxe contenues dans le script. Testez les exécutions de script sans erreur et insérez également une journalisation supplémentaire dans le script pour trouver plus facilement l’emplacement de l’échec.
* Écrivez des scripts idempotents ; s’ils sont exécutés plusieurs fois par erreur, ils n’entraîneront pas de modification du système.
* Vérifiez que l’exécution des scripts ne nécessite pas d’entrée utilisateur.
* L’exécution du script est autorisée pendant 90 minutes. Toute exécution d’une durée supérieure entraîne l’échec du provisionnement de l’extension.
* N’insérez pas de redémarrages dans le script, car cela entraîne des problèmes avec d’autres extensions en cours d’installation : après un redémarrage, l’extension ne poursuit pas son exécution. 
* Si l’un de vos scripts provoque un redémarrage, installez les applications, puis exécutez des scripts, etc. Vous devez planifier le redémarrage à l’aide d’une tâche planifiée Windows, ou d’outils tels que des extensions DSC, Chef ou Puppet.
* L’extension n’exécute un script qu’une seule fois. Si vous voulez exécuter un script à chaque démarrage, vous devez utiliser l’extension pour créer une tâche planifiée Windows.
* Si vous souhaitez planifier le moment de l’exécution d’un script, vous devez utiliser l’extension pour créer une tâche planifiée Windows. 
* Lors de l’exécution du script, vous voyez seulement l’état de l’extension « transition en cours » dans le portail Azure ou Azure CLI. Si vous souhaitez que les mises à jour de l’état d’un script en cours d’exécution soient plus fréquentes, vous devez créer votre propre solution.
* L’extension de script personnalisé ne prend pas en charge les serveurs proxy en mode natif, mais vous pouvez utiliser un outil de transfert de fichiers prenant en charge les serveurs proxy dans votre script, par exemple *Curl*. 
* Tenez compte des emplacements de répertoire autres que par défaut, et qui sont susceptibles d’être utilisés pour vos scripts ou commandes. Gérez cette situation de façon logique.


## <a name="extension-schema"></a>Schéma d’extensions

La configuration de l’extension de script personnalisé spécifie des éléments tels que l’emplacement du script et la commande à exécuter. Cette configuration peut être stockée dans des fichiers de configuration, ou spécifiée en ligne de commande ou dans un modèle Azure Resource Manager. 

Les données sensibles peuvent être stockées dans une configuration protégée qui n’est chiffrée et déchiffrée qu’à l’intérieur de la machine virtuelle. La configuration protégée est utile lorsque la commande d’exécution comprend des secrets tels qu’un mot de passe.

Ces éléments doivent être traités comme des données sensibles et spécifiés dans la configuration du paramètre de protection des extensions. Les données du paramètre de protection de l’extension de machine virtuelle Azure sont chiffrées et ne sont déchiffrées que sur la machine virtuelle cible.

```json
{
    "apiVersion": "2015-06-15",
    "type": "Microsoft.Compute/virtualMachines/extensions",
    "name": "config-app",
    "location": "[resourceGroup().location]",
    "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'),copyindex())]",
        "[variables('musicstoresqlName')]"
    ],
    "tags": {
        "displayName": "config-app"
    },
    "properties": {
        "publisher": "Microsoft.Compute",
        "type": "CustomScriptExtension",
        "typeHandlerVersion": "1.9",
        "autoUpgradeMinorVersion": true,
        "settings": {
            "fileUris": [
                "script location"
            ]
        },
        "protectedSettings": {
            "commandToExecute": "myExecutionCommand",
            "storageAccountName": "myStorageAccountName",
            "storageAccountKey": "myStorageAccountKey"
        }
    }
}
```
**Remarque** : il n’est possible d’installer qu’une seule version d’une extension sur une machine virtuelle à un moment donné. Spécifier deux fois un script personnalisé dans le même modèle Azure Resource Manager pour une même machine virtuelle aboutira à un échec. 

### <a name="property-values"></a>Valeurs de propriétés

| NOM | Valeur/Exemple | Type de données |
| ---- | ---- | ---- |
| apiVersion | 2015-06-15 | date |
| publisher | Microsoft.Compute | chaîne |
| Type | CustomScriptExtension | chaîne |
| typeHandlerVersion | 1.9 | int |
| fileUris (p. ex.) | https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-windows/scripts/configure-music-app.ps1 | array |
| commandToExecute (p. ex.) | powershell -ExecutionPolicy Unrestricted -File configure-music-app.ps1 | chaîne |
| storageAccountName (p. ex.) | examplestorageacct | chaîne |
| storageAccountKey (p. ex.) | TmJK/1N3AbAZ3q/+hOXoi/l73zOqsaxXDhqa9Y83/v5UpXQp2DQIBuv2Tifp60cE/OaHsJZmQZ7teQfczQj8hg== | chaîne |

>[!NOTE]
>Ces noms de propriétés respectent la casse. Pour éviter des problèmes de déploiement, utilisez les noms présentés ici.

#### <a name="property-value-details"></a>Détails des valeurs de propriété
 * `commandToExecute` : (**obligatoire**, chaîne) script de point d’entrée à exécuter. Utilisez plutôt ce champ si votre commande contient des secrets tels que des mots de passe ou si vos URI de fichier sont sensibles.
* `fileUris` : (facultatif, tableau de chaînes) URL des fichiers à télécharger.
* `storageAccountName` : (facultatif, chaîne) nom du compte de stockage. Si vous spécifiez des informations d’identification de stockage, toutes les propriétés `fileUris` doivent être des URL d’objets blob Azure.
* `storageAccountKey` : (facultatif, chaîne) clé d’accès du compte de stockage.

Les valeurs suivantes peuvent être définies dans les paramètres publics ou protégés. L’extension rejette les configurations dans lesquelles les valeurs indiquées ci-dessous sont définies à la fois dans les paramètres publics et protégés.
* `commandToExecute`

L’utilisation des paramètres publics peut être utile pour le débogage, mais il est vivement recommandé d’utiliser les paramètres protégés.

Les paramètres publics sont envoyés en texte clair à la machine virtuelle sur laquelle le script est exécuté.  Les paramètres protégés sont chiffrés à l’aide d’une clé connue uniquement d’Azure et de la machine virtuelle. Les paramètres sont enregistrés sur la machine virtuelle tels qu’ils ont été envoyés. Autrement dit, si les paramètres ont été chiffrés, ils sont enregistrés chiffrés sur la machine virtuelle. Le certificat utilisé pour déchiffrer les valeurs chiffrées est stocké sur la machine virtuelle et permet de déchiffrer les paramètres (si nécessaire) lors de l’exécution.

## <a name="template-deployment"></a>Déploiement de modèle

Les extensions de machines virtuelles Azure peuvent être déployées avec des modèles Azure Resource Manager. Le schéma JSON détaillé dans la section précédente peut être utilisé dans un modèle Azure Resource Manager pour exécuter l’extension de script personnalisé pendant un déploiement de modèle Azure Resource Manager. Un exemple de modèle qui inclut l’extension de script personnalisé est disponible ici, [GitHub](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-windows).

## <a name="powershell-deployment"></a>Déploiement PowerShell

Vous pouvez utiliser la commande `Set-AzureRmVMCustomScriptExtension` pour ajouter l’extension de script personnalisé sur une machine virtuelle existante. Pour plus d’informations, consultez [Set-AzureRmVMCustomScriptExtension](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.1.0/set-azurermvmcustomscriptextension).
```powershell
Set-AzureRmVMCustomScriptExtension -ResourceGroupName myResourceGroup `
    -VMName myVM `
    -Location myLocation `
    -FileUri myURL `
    -Run 'myScript.ps1' `
    -Name DemoScriptExtension
```
## <a name="further-examples"></a>Autres exemples

### <a name="using-multiple-script"></a>Utilisation de plusieurs scripts
Dans cet exemple, vous avez trois scripts qui sont utilisés pour construire votre serveur. Le paramètre « commandToExecute » appelle le premier script, puis vous avez des options concernant la façon dont les autres scripts sont appelés. Par exemple, vous pouvez avoir un script principal qui contrôle l’exécution, avec la gestion des erreurs, la journalisation et la gestion d’état appropriées.

```powershell

$fileUri = @("https://xxxxxxx.blob.core.windows.net/buildServer1/1_Add_Tools.ps1",
"https://xxxxxxx.blob.core.windows.net/buildServer1/2_Add_Features.ps1",
"https://xxxxxxx.blob.core.windows.net/buildServer1/3_CompleteInstall.ps1")

$Settings = @{"fileUris" = $fileUri};

$storageaccname = "xxxxxxx"
$storagekey = "1234ABCD"
$ProtectedSettings = @{"storageAccountName" = $storageaccname; "storageAccountKey" = $storagekey; "commandToExecute" = "powershell -ExecutionPolicy Unrestricted -File 1_Add_Tools.ps1"};

#run command
Set-AzureRmVMExtension -ResourceGroupName myRG `
    -Location myLocation ` 
    -VMName myVM ` 
    -Name "buildserver1" ` 
    -Publisher "Microsoft.Compute" ` 
    -ExtensionType "CustomScriptExtension" ` 
    -TypeHandlerVersion "1.9" ` 
    -Settings $Settings ` 
    -ProtectedSettings $ProtectedSettings `
```

### <a name="running-scripts-from-a-local-share"></a>Exécution de scripts à partir d’un partage local
Dans cet exemple, vous pouvez utiliser un serveur SMB local pour votre emplacement de script. Notez cependant que vous n’avez pas besoin de transmettre d’autres paramètres, sauf *commandToExecute*.

```powershell
$ProtectedSettings = @{"commandToExecute" = "powershell -ExecutionPolicy Unrestricted -File \\filesvr\build\serverUpdate1.ps1"};
 
Set-AzureRmVMExtension -ResourceGroupName myRG 
    -Location myLocation ` 
    -VMName myVM ` 
    -Name "serverUpdate" 
    -Publisher "Microsoft.Compute" `
    -ExtensionType "CustomScriptExtension" ` 
    -TypeHandlerVersion "1.9" `
    -ProtectedSettings $ProtectedSettings

```

### <a name="how-to-run-custom-script-more-than-once-with-cli"></a>Comment exécuter un script personnalisé plusieurs fois avec l’interface de ligne de commande
Si vous souhaitez exécuter plusieurs fois l’extension de script personnalisé, vous ne pouvez le faire que dans les conditions suivantes :
1. Le paramètre « Name » de l’extension est le même que pour le déploiement précédent de celle-ci.
2. Vous devez mettre à jour la configuration, sans quoi la commande n’est pas ré-exécutée. Par exemple, vous pouvez ajouter à la commande une propriété dynamique, telle que timestamp. 

## <a name="troubleshoot-and-support"></a>Dépannage et support technique

### <a name="troubleshoot"></a>Résolution des problèmes

Vous pouvez récupérer les données sur l’état des déploiements d’extension à partir du portail Azure et à l’aide du module Azure PowerShell. Pour voir l’état du déploiement des extensions pour une machine virtuelle donnée, exécutez la commande suivante :

```powershell
Get-AzureRmVMExtension -ResourceGroupName myResourceGroup -VMName myVM -Name myExtensionName
```

La sortie de l’exécution de l’extension est enregistrée dans les fichiers qui se trouvent dans le répertoire suivant sur la machine virtuelle.
```cmd
C:\WindowsAzure\Logs\Plugins\Microsoft.Compute.CustomScriptExtension
```

Les fichiers spécifiés sont téléchargés dans le répertoire suivant sur la machine virtuelle cible.
```cmd
C:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\1.*\Downloads\<n>
```
où `<n>` est un entier décimal, qui peut changer d’une exécution de l’extension à l’autre.  La valeur `1.*` correspond à la valeur`typeHandlerVersion` actuelle et réelle de l’extension.  Par exemple, le répertoire réel peut être `C:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\1.8\Downloads\2`.  

Lors de l’exécution de la commande `commandToExecute`, l’extension définit ce répertoire (par exemple, `...\Downloads\2`) comme répertoire de travail actif. Cela permet d’utiliser des chemins d’accès relatifs pour localiser les fichiers téléchargés par le biais de la propriété `fileURIs`. Consultez le tableau ci-dessous pour en voir des exemples.

Étant donné que le chemin de téléchargement absolu peut varier au fil du temps, il est préférable d’opter pour des chemins d’accès au script/fichier relatifs dans la chaîne `commandToExecute`, si possible. Par exemple : 
```json
    "commandToExecute": "powershell.exe . . . -File \"./scripts/myscript.ps1\""
```

Les informations de chemin d’accès après le premier segment d’URI sont conservées pour les fichiers téléchargés par le biais de la liste de propriétés `fileUris`.  Comme l’indique le tableau ci-dessous, les fichiers téléchargés sont mappés avec des sous-répertoires de téléchargement afin de refléter la structure des valeurs `fileUris`.  

#### <a name="examples-of-downloaded-files"></a>Exemples de fichiers téléchargés

| URI dans fileUris | Emplacement téléchargé relatif | Emplacement téléchargé absolu* |
| ---- | ------- |:--- |
| `https://someAcct.blob.core.windows.net/aContainer/scripts/myscript.ps1` | `./scripts/myscript.ps1` |`C:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\1.8\Downloads\2\scripts\myscript.ps1`  |
| `https://someAcct.blob.core.windows.net/aContainer/topLevel.ps1` | `./topLevel.ps1` | `C:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\1.8\Downloads\2\topLevel.ps1` |

\* Comme ci-dessus, les chemins absolus d’accès au répertoire évoluent pendant la durée de vie de la machine virtuelle, mais pas au sein d’une même exécution de l’extension CustomScript.

### <a name="support"></a>Support

Si vous avez besoin d’une aide supplémentaire à quelque étape que ce soit dans cet article, vous pouvez contacter les experts Azure sur les [forums MSDN Azure et Stack Overflow](https://azure.microsoft.com/support/forums/). Vous pouvez également signaler un incident au support Azure. Accédez au [site du support Azure](https://azure.microsoft.com/support/options/) , puis cliquez sur Obtenir un support. Pour plus d’informations sur l’utilisation du support Azure, lisez le [FAQ du support Microsoft Azure](https://azure.microsoft.com/support/faq/).
