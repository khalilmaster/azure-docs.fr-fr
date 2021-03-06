---
title: Guide pratique pour configurer des identités affectées par le système et l’utilisateur sur une machine virtuelle Azure à l’aide de REST
description: Cet article contient des instructions pas à pas pour la configuration d’identités affectées par le système et l’utilisateur sur une machine virtuelle Azure à l’aide de CURL afin d’effectuer des appels d’API REST.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/25/2018
ms.author: daveba
ms.openlocfilehash: 9cc645d91bebf07ed7e657ed39c2454254958959
ms.sourcegitcommit: d551ddf8d6c0fd3a884c9852bc4443c1a1485899
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/07/2018
ms.locfileid: "37902885"
---
# <a name="configure-managed-identity-on-an-azure-vm-using-rest-api-calls"></a>Configurer une identité managée sur une machine virtuelle Azure à l’aide d’appels d’API REST

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

L’identité managée fournit aux services Azure une identité système managée automatiquement dans Azure Active Directory. Vous pouvez utiliser cette identité pour vous authentifier sur n’importe quel service prenant en charge l’authentification Azure AD, sans avoir d’informations d’identification dans votre code. 

Dans cet article, découvrez comment effectuer les opérations suivantes sur des identités managées sur une machine virtuelle Azure, en utilisant CURL pour effectuer des appels vers le point de terminaison REST Azure Resource Manager :

- Activer et désactiver l’identité attribuée au système sur une machine virtuelle Azure
- Ajouter et supprimer une identité attribuée à l’utilisateur sur une machine virtuelle Azure

## <a name="prerequisites"></a>Prérequis

- Si vous ne connaissez pas MSI, consultez la [section Vue d’ensemble](overview.md). **Veillez à lire [la différence entre les identités attribuées au système et celles attribuées à l’utilisateur](overview.md#how-does-it-work)**.
- Si vous n’avez pas encore de compte Azure, [inscrivez-vous à un essai gratuit](https://azure.microsoft.com/free/) avant de continuer.
- Si vous utilisez Windows, installez le [sous-système Windows pour Linux](https://msdn.microsoft.com/commandline/wsl/about) ou utilisez [Azure Cloud Shell](../../cloud-shell/overview.md) dans le portail Azure.
- [Installez la console locale Azure CLI](/azure/install-azure-cli), si vous utilisez le [sous-système Windows pour Linux](https://msdn.microsoft.com/commandline/wsl/about) ou un [système d’exploitation de distribution Linux](/cli/azure/install-azure-cli-apt?view=azure-cli-latest).
- Si vous utilisez la console locale Azure CLI, connectez-vous à Azure à l’aide de la commande `az login` avec un compte associé à l’abonnement Azure sous lequel vous souhaitez gérer les identités affectées par le système ou l’utilisateur.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="system-assigned-identity"></a>Identité attribuée au système

Dans cette section, découvrez comment activer et désactiver une identité affectée par le système sur une machine virtuelle Azure, en utilisant CURL pour effectuer des appels vers le point de terminaison REST Azure Resource Manager.

### <a name="enable-system-assigned-identity-during-creation-of-an-azure-vm"></a>Activer une identité attribuée au système lors de la création d’une machine virtuelle Azure

Pour créer une machine virtuelle Azure sur laquelle une identité affectée par le système est activée, créez une machine virtuelle et récupérez un jeton d’accès pour utiliser CURL afin d’appeler le point de terminaison Resource Manager avec la valeur de type d’identité affectée par le système.

1. Créez un [groupe de ressources](../../azure-resource-manager/resource-group-overview.md#terminology) pour l’imbrication et le déploiement de votre machine virtuelle et de ses ressources connexes, à l’aide de la commande [az group create](/cli/azure/group/#az_group_create). Vous pouvez ignorer cette étape si vous possédez déjà le groupe de ressources que vous souhaitez utiliser à la place :

   ```azurecli-interactive 
   az group create --name myResourceGroup --location westus
   ```

2. Créez une [interface réseau](/cli/azure/network/nic?view=azure-cli-latest#az-network-nic-create) pour votre machine virtuelle :

   ```azurecli-interactive
    az network nic create -g myResourceGroup --vnet-name myVnet --subnet mySubnet -n myNic
   ```

3.  Récupérez un jeton d’accès de porteur, que vous allez utiliser à l’étape suivante dans l’en-tête d’autorisation pour créer votre machine virtuelle avec une identité managée affectée par le système.

   ```azurecli-interactive
   az account get-access-token
   ``` 

4. Créez une machine virtuelle à l’aide de CURL pour appeler le point de terminaison REST Azure Resource Manager. L’exemple suivant crée une machine virtuelle nommée *myVM* avec une identité affectée par le système, comme identifié dans le corps de demande par la valeur `"identity":{"type":"SystemAssigned"}`. Remplacez `<ACCESS TOKEN>` par la valeur que vous avez reçue à l’étape précédente lorsque vous avez demandé un jeton d’accès du porteur et la valeur `<SUBSCRIPTION ID>` adaptée à votre environnement.
 
    ```bash
    curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2017-12-01' -X PUT -d '{"location":"westus","name":"myVM","identity":{"type":"SystemAssigned"},"properties":{"hardwareProfile":{"vmSize":"Standard_D2_v2"},"storageProfile":{"imageReference":{"sku":"2016-Datacenter","publisher":"MicrosoftWindowsServer","version":"latest","offer":"WindowsServer"},"osDisk":{"caching":"ReadWrite","managedDisk":{"storageAccountType":"Standard_LRS"},"name":"TestVM3osdisk","createOption":"FromImage"},"dataDisks":[{"diskSizeGB":1023,"createOption":"Empty","lun":0},{"diskSizeGB":1023,"createOption":"Empty","lun":1}]},"osProfile":{"adminUsername":"azureuser","computerName":"myVM","adminPassword":"myPassword12"},"networkProfile":{"networkInterfaces":[{"id":"/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myNic","properties":{"primary":true}}]}}}' -H "Content-Type: application/json" -H "Authorization: Bearer <ACCESS TOKEN>"
    ```

### <a name="enable-system-assigned-identity-on-an-existing-azure-vm"></a>Activer une identité attribuée au système sur une machine virtuelle Azure existante

Pour activer l’identité affectée par le système sur une machine virtuelle existante, obtenez un jeton d’accès, puis utilisez CURL pour appeler le point de terminaison REST Resource Manager afin de mettre à jour le type d’identité.

1. Récupérez un jeton d’accès de porteur, que vous allez utiliser à l’étape suivante dans l’en-tête d’autorisation pour créer votre machine virtuelle avec une identité managée affectée par le système.

   ```azurecli-interactive
   az account get-access-token
   ```

2. Utilisez la commande CURL suivante pour appeler le point de terminaison REST Azure Resource Manager afin d’activer l’identité affectée par le système sur votre machine virtuelle, comme identifié dans le corps de la demande par la valeur `{"identity":{"type":"SystemAssigned"}` pour une machine virtuelle nommée *myVM*.  Remplacez `<ACCESS TOKEN>` par la valeur que vous avez reçue à l’étape précédente lorsque vous avez demandé un jeton d’accès du porteur et la valeur `<SUBSCRIPTION ID>` adaptée à votre environnement.
   
   > [!IMPORTANT]
   > Pour éviter de supprimer des identités managées affectées par l’utilisateur existantes qui sont attribuées à la machine virtuelle, répertoriez les identités affectées par l’utilisateur à l’aide de la commande CURL suivante : `curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.Compute/virtualMachines/<VM NAME>?api-version=2017-12-01' -H "Authorization: Bearer <ACCESS TOKEN>"`. Si vous avez attribué des identités affectées par l’utilisateur à la machine virtuelle, comme identifié dans la valeur `identity` de la réponse, passez à l’étape 3, qui montre comment conserver les identités affectées par l’utilisateur, tout en activant l’identité affectée par le système sur votre machine virtuelle.

   ```bash
    curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2017-12-01' -X PATCH -d '{"identity":{"type":"SystemAssigned"}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

3. Pour activer l’identité affectée par le système sur une machine virtuelle avec des identités affectées par l’utilisateur existantes, vous devez ajouter `SystemAssigned` à la valeur `type`.  
   
   Par exemple, si votre machine virtuelle a des identités affectées par l’utilisateur `ID1` et `ID2` qui lui sont attribuées, et que vous souhaitez ajouter l’identité affectée par le système à la machine virtuelle, utilisez l’appel CURL suivant. Remplacez `<ACCESS TOKEN>` et `<SUBSCRIPTION ID>` par les valeurs adaptées à votre environnement.
   
   ```bash
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/TestVM?api-version=2017-12-01' -X PATCH -d '{"identity":{"type":"SystemAssigned","UserAssigned", "identityIds":["/subscriptions/<<SUBSCRIPTION ID>>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1","/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID2"]}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

### <a name="disable-system-assigned-identity-from-an-azure-vm"></a>Désactiver l’identité affectée par le système à partir d’une machine virtuelle Azure

Pour désactiver une identité affectée par le système sur une machine virtuelle existante, obtenez un jeton d’accès, puis utilisez CURL pour appeler le point de terminaison REST Resource Manager afin de mettre à jour le type d’identité en le définissant sur `None`.

1. Récupérez un jeton d’accès de porteur, que vous allez utiliser à l’étape suivante dans l’en-tête d’autorisation pour créer votre machine virtuelle avec une identité managée affectée par le système.

   ```azurecli-interactive
   az account get-access-token
   ```

2. Mettez à jour la machine virtuelle à l’aide de CURL pour appeler le point de terminaison REST Azure Resource Manager afin de désactiver l’identité affectée par le système.  L’exemple suivant désactive une identité affectée par le système, comme identifié dans le corps de demande par la valeur `{"identity":{"type":"None"}}` d’une machine virtuelle nommée *myVM*.  Remplacez `<ACCESS TOKEN>` par la valeur que vous avez reçue à l’étape précédente lorsque vous avez demandé un jeton d’accès du porteur et la valeur `<SUBSCRIPTION ID>` adaptée à votre environnement.

   > [!IMPORTANT]
   > Pour éviter de supprimer des identités managées affectées par l’utilisateur existantes qui sont attribuées à la machine virtuelle, répertoriez les identités affectées par l’utilisateur à l’aide de la commande CURL suivante : `curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.Compute/virtualMachines/<VM NAME>?api-version=2017-12-01' -H "Authorization: Bearer <ACCESS TOKEN>"`. Si vous avez attribué des identités affectées par l’utilisateur à la machine virtuelle, comme identifié dans la valeur `identity` de la réponse, passez à l’étape 3, qui montre comment conserver les identités affectées par l’utilisateur, tout en désactivant l’identité affectée par le système sur votre machine virtuelle.

   ```bash
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2017-12-01' -X PATCH -d '{"identity":{"type":"None"}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

3. Pour supprimer l’identité affectée par le système d’une machine virtuelle ayant des identités affectées par l’utilisateur, supprimez `SystemAssigned` de la valeur `{"identity":{"type:" "}}`, tout en conservant la valeur `UserAssigned` et le tableau `identityIds` qui définit quelles identités affectées par l’utilisateur sont affectées à la machine virtuelle.

## <a name="user-assigned-identity"></a>Identité attribuée par l’utilisateur

Dans cette section, découvrez comment ajouter et supprimer une identité affectée par l’utilisateur dans une machine virtuelle Azure, en utilisant CURL pour effectuer des appels vers le point de terminaison REST Azure Resource Manager.

### <a name="assign-a-user-assigned-identity-during-the-creation-of-an-azure-vm"></a>Attribuer une identité attribuée à l’utilisateur lors de la création d’une machine virtuelle Azure

1. Récupérez un jeton d’accès de porteur, que vous allez utiliser à l’étape suivante dans l’en-tête d’autorisation pour créer votre machine virtuelle avec une identité managée affectée par le système.

   ```azurecli-interactive
   az account get-access-token
   ```

2. Créez une [interface réseau](/cli/azure/network/nic?view=azure-cli-latest#az-network-nic-create) pour votre machine virtuelle :

   ```azurecli-interactive
    az network nic create -g myResourceGroup --vnet-name myVnet --subnet mySubnet -n myNic
   ```

3.  Récupérez un jeton d’accès de porteur, que vous allez utiliser à l’étape suivante dans l’en-tête d’autorisation pour créer votre machine virtuelle avec une identité managée affectée par le système.

   ```azurecli-interactive
   az account get-access-token
   ``` 

4. Créez une identité affectée par l’utilisateur à l’aide des instructions disponibles ici : [Créer une identité managée affectée par l’utilisateur](how-to-manage-ua-identity-rest.md#create-a-user-assigned-managed-identity).

5. Créez une machine virtuelle à l’aide de CURL pour appeler le point de terminaison REST Azure Resource Manager. L’exemple suivant crée une machine virtuelle nommée *myVM* dans le groupe de ressources *myResourceGroup* avec une identité affectée par l’utilisateur `ID1`, comme identifié dans le corps de demande par la valeur `"identity":{"type":"UserAssigned"}`. Remplacez `<ACCESS TOKEN>` par la valeur que vous avez reçue à l’étape précédente lorsque vous avez demandé un jeton d’accès du porteur et la valeur `<SUBSCRIPTION ID>` adaptée à votre environnement.
 
   ```bash   
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2017-12-01' -X PUT -d '{"location":"westus","name":"myVM",{"identity":{"type":"UserAssigned", "identityIds":["/subscriptions/80c696ff-5efa-4909-a64d-f1b616f423ca/resourcegroups/TestRG/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1"]}},"properties":{"hardwareProfile":{"vmSize":"Standard_D2_v2"},"storageProfile":{"imageReference":{"sku":"2016-Datacenter","publisher":"MicrosoftWindowsServer","version":"latest","offer":"WindowsServer"},"osDisk":{"caching":"ReadWrite","managedDisk":{"storageAccountType":"Standard_LRS"},"name":"TestVM3osdisk","createOption":"FromImage"},"dataDisks":[{"diskSizeGB":1023,"createOption":"Empty","lun":0},{"diskSizeGB":1023,"createOption":"Empty","lun":1}]},"osProfile":{"adminUsername":"azureuser","computerName":"myVM","adminPassword":"myPassword12"},"networkProfile":{"networkInterfaces":[{"id":"/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myNic","properties":{"primary":true}}]}}}' -H "Content-Type: application/json" -H "Authorization: Bearer <ACCESS TOKEN>"
   ```

### <a name="assign-a-user-assigned-identity-to-an-existing-azure-vm"></a>Attribuer une identité attribuée à l’utilisateur à une machine virtuelle Azure existante

1. Récupérez un jeton d’accès de porteur, que vous allez utiliser à l’étape suivante dans l’en-tête d’autorisation pour créer votre machine virtuelle avec une identité managée affectée par le système.

   ```azurecli-interactive
   az account get-access-token
   ```

2.  Créez une identité affectée par l’utilisateur à l’aide des instructions disponibles ici : [Créer une identité managée affectée par l’utilisateur](how-to-manage-ua-identity-rest.md#create-a-user-assigned-managed-identity).

3.  Pour éviter de supprimer des identités managées affectées par l’utilisateur ou le système existantes qui sont attribuées à la machine virtuelle, vous devez répertorier les types d’identité affectés à la machine virtuelle à l’aide de la commande CURL suivante. Si vous avez affecté des identités managées au groupe de machines virtuelles identiques, celles-ci sont répertoriées sous la valeur `identity`.

    ```bash
    curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.Compute/virtualMachines/<VM NAME>?api-version=2017-12-01' -H "Authorization: Bearer <ACCESS TOKEN>" 
    ```

    Si vous avez des identités affectées par l’utilisateur ou le système qui sont attribuées à la machine virtuelle, comme identifié dans la valeur `identity` de la réponse, passez à l’étape 5, qui montre comment conserver les identités affectées par l’utilisateur, tout en activant l’identité affectée par le système sur votre machine virtuelle.

4. Si vous n’avez aucune identité affectée par l’utilisateur attribuée à votre machine virtuelle, utilisez la commande CURL suivante pour appeler le point de terminaison REST Azure Resource Manager afin d’attribuer la première identité affectée par l’utilisateur à la machine virtuelle.  Si vous avez des identités affectées par l’utilisateur qui sont attribuées à la machine virtuelle, passez à l’étape suivante qui vous montre comment ajouter plusieurs identités affectées par l’utilisateur à une machine virtuelle.

   L’exemple suivant attribue l’identité affectée par l’utilisateur `ID1` à une machine virtuelle nommée *myVM* dans le groupe de ressources *myResourceGroup*.  Remplacez `<ACCESS TOKEN>` par la valeur que vous avez reçue à l’étape précédente lorsque vous avez demandé un jeton d’accès du porteur et la valeur `<SUBSCRIPTION ID>` adaptée à votre environnement.

   ```bash
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2017-12-01' -X PATCH -d '{"identity":{"type":"userAssigned", "identityIds":["/subscriptions/<SUBSCRIPTION ID>/resourcegroups/TestRG/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1"]}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

5. Si vous avez des identités affectées par l’utilisateur ou le système qui sont attribuées à votre machine virtuelle, vous devez ajouter la nouvelle identité affectée par l’utilisateur au tableau `identityIDs`, tout en conservant les identités affectées par le système et l’utilisateur qui sont actuellement attribuées à la machine virtuelle.

   Par exemple, si votre machine virtuelle a l’identité affectée par le système et l’identité affectée par l’utilisateur `ID1` qui lui sont attribuées, et que vous souhaitez ajouter l’identité affectée par l’utilisateur `ID2` à celle-ci, utilisez la commande CURL suivante. Remplacez `<ACCESS TOKEN>` par la valeur que vous avez reçue à l’étape précédente lorsque vous avez demandé un jeton d’accès du porteur et la valeur `<SUBSCRIPTION ID>` adaptée à votre environnement.

   ```bash
   curl  'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/TestVM?api-version=2017-12-01' -X PATCH -d '{"identity":{"type":"SystemAssigned","UserAssigned", "identityIds":["/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1","/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID2"]}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

### <a name="remove-a-user-assigned-identity-from-an-azure-vm"></a>Supprimer une identité attribuée à l’utilisateur d’une machine virtuelle Azure

1. Récupérez un jeton d’accès de porteur, que vous allez utiliser à l’étape suivante dans l’en-tête d’autorisation pour créer votre machine virtuelle avec une identité managée affectée par le système.

   ```azurecli-interactive
   az account get-access-token
   ```

2. Pour éviter de supprimer des identités managées affectées par l’utilisateur existantes que vous souhaitez conserver sur la machine virtuelle ou de supprimer l’identité affectée par le système, vous devez répertorier les identités managées à l’aide de la commande CURL suivante : 
 
   ```bash
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.Compute/virtualMachines/<VM NAME>?api-version=2017-12-01' -H "Authorization: Bearer <ACCESS TOKEN>"
   ```
 
   Si vous avez affecté des identités managées à la machine virtuelle, celles-ci sont répertoriées dans la réponse sous la valeur `identity`.

   Par exemple, si vous avez les identités affectées par l’utilisateur `ID1` et `ID2` qui ont été attribuées à votre machine virtuelle et que vous voulez uniquement conserver l’identité `ID1` affectée et l’identité affectée par le système, utilisez la même commande CURL que lorsque vous affectez une identité managée affectée par l’utilisateur à une machine virtuelle, en conservant uniquement la valeur `ID1` et conservez la valeur `SystemAssigned`. Cette opération supprime l’identité affectée par l’utilisateur `ID2` de la machine virtuelle, tout en conservant l’identité affectée par le système.

   ```bash
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2017-12-01' -X PATCH -d '{"identity":{"type":"SystemAssigned","UserAssigned", "identityIds":["/subscriptions/80c696ff-5efa-4909-a64d-f1b616f423ca/resourcegroups/TestRG/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1"]}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

Si votre machine virtuelle dispose à la fois d’identités affectées par l’utilisateur et d’identités affectées par le système, vous pouvez supprimer toutes les identités affectées par l’utilisateur en choisissant de n’utiliser que des identités affectées par le système en utilisant la commande suivante :

```bash
curl 'https://management.azure.com/subscriptions/80c696ff-5efa-4909-a64d-f1b616f423ca/resourceGroups/TestRG/providers/Microsoft.Compute/virtualMachines/TestVM?api-version=2017-12-01' -X PATCH -d '{"identity":{"type":"SystemAssigned"}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
```
    
Si votre machine virtuelle a uniquement des identités affectées par l’utilisateur et que vous souhaitez les supprimer, utilisez la commande suivante :

```bash
curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2017-12-01' -X PATCH -d '{"identity":{"type":"None"}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
```
## <a name="next-steps"></a>Étapes suivantes

Pour plus d’informations sur la façon de créer, de lister ou de supprimer des identités affectées par l’utilisateur à l’aide de REST, consultez :

- [Créer, lister ou supprimer une identité affectée par l’utilisateur à l’aide d’appels d’API REST](how-to-manage-ua-identity-rest.md)