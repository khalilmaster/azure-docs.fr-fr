---
title: Exemple json Azure Policy - appliquer un modèle de similitude | Microsoft Docs
description: Cet exemple de stratégie json oblige les ressources à respecter le modèle de similitude pour les conventions d’affectation de noms.
services: azure-policy
documentationcenter: ''
author: DCtheGeek
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: azure-policy
ms.devlang: ''
ms.topic: sample
ms.tgt_pltfrm: ''
ms.workload: ''
ms.date: 11/13/2017
ms.author: dacoulte
ms.custom: mvc
ms.openlocfilehash: 33f67cc96395d1e481618e75fccb535c92d2fe75
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34601766"
---
# <a name="enforce-like-pattern-for-naming-conventions"></a>Appliquer le modèle de similitude pour les conventions d’affectation de noms

Exigez que les noms de ressource soient conformes à un modèle de similitude pour les conventions d’affectation de noms. Spécifiez le modèle de similitude autorisé en tant que paramètre.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-template"></a>Exemple de modèle

[!code-json[main](../../../policy-templates/samples/TextPatterns/enforce-like-pattern/azurepolicy.json "enforce like pattern")]

Vous pouvez déployer ce modèle en utilisant le [portail Azure](#deploy-with-the-portal), avec [PowerShell](#deploy-with-powershell) ou avec [Azure CLI](#deploy-with-azure-cli).

## <a name="deploy-with-the-portal"></a>Déployer avec le portail

[![Déploiement sur Azure](http://azuredeploy.net/deploybutton.png)](https://portal.azure.com/?feature.customportal=false&microsoft_azure_policy=true&microsoft_azure_policy_policyinsights=true&feature.microsoft_azure_security_policy=true&microsoft_azure_marketplace_policy=true#blade/Microsoft_Azure_Policy/CreatePolicyDefinitionBlade/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-policy%2Fmaster%2Fsamples%2FTextPatterns%2Fenforce-like-pattern%2Fazurepolicy.json)

## <a name="deploy-with-powershell"></a>Déployer avec PowerShell

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

```powershell
$definition = New-AzureRmPolicyDefinition -Name "enforce-like-pattern" -DisplayName "Ensure resource names meet the like condition for a pattern." -description "Ensure resource names meet the like condition for a pattern." -Policy 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/TextPatterns/enforce-like-pattern/azurepolicy.rules.json' -Parameter 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/TextPatterns/enforce-like-pattern/azurepolicy.parameters.json' -Mode All
$definition
$assignment = New-AzureRMPolicyAssignment -Name <assignmentname> -Scope <scope> -PolicyDefinition $definition
$assignment
```

### <a name="clean-up-powershell-deployment"></a>Nettoyer un déploiement PowerShell

Exécutez la commande suivante pour supprimer le groupe de ressources, la machine virtuelle et toutes les ressources associées.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="deploy-with-azure-cli"></a>Déploiement avec l’interface de ligne de commande Azure

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

```azurecli
az policy definition create --name 'enforce-like-pattern' --display-name 'Ensure resource names meet the like condition for a pattern.' --description 'Ensure resource names meet the like condition for a pattern.' --rules 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/TextPatterns/enforce-like-pattern/azurepolicy.rules.json' --params 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/TextPatterns/enforce-like-pattern/azurepolicy.parameters.json' --mode All

az policy assignment create --name <assignmentname> --scope <scope> --policy "enforce-like-pattern" 
```

### <a name="clean-up-azure-cli-deployment"></a>Nettoyer un déploiement Azure CLI

Exécutez la commande suivante pour supprimer le groupe de ressources, la machine virtuelle et toutes les ressources associées.

```azurecli-interactive
az group delete --name myResourceGroup --yes
```

## <a name="next-steps"></a>Étapes suivantes

- Relisez plusieurs exemples dans [Exemples Azure Policy](../json-samples.md).