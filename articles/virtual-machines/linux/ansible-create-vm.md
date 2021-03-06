---
title: Utiliser Ansible pour créer une machine virtuelle Linux de base dans Azure | Microsoft Docs
description: Découvrez comment utiliser Ansible pour créer et gérer une machine virtuelle Linux de base dans Azure
services: virtual-machines-linux
documentationcenter: virtual-machines
author: cynthn
manager: jeconnoc
editor: na
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/30/2018
ms.author: cynthn
ms.openlocfilehash: 35dfe8348718e0edf8683f7eeddf286831697d89
ms.sourcegitcommit: aa988666476c05787afc84db94cfa50bc6852520
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37931427"
---
# <a name="create-a-basic-virtual-machine-in-azure-with-ansible"></a>Créer une machine virtuelle Linux de base dans Azure avec Ansible
Ansible vous permet d’automatiser le déploiement et la configuration de ressources dans votre environnement. Vous pouvez utiliser Ansible pour gérer vos machines virtuelles dans Azure comme vous le feriez pour toute autre ressource. Cet article vous explique comment créer une machine virtuelle de base avec Ansible. Vous pouvez également découvrir comment [créer un environnement de machine virtuelle complète avec Ansible](ansible-create-complete-vm.md).


## <a name="prerequisites"></a>Prérequis
Pour gérer des ressources Azure avec Ansible, vous avez besoin des éléments suivants :

- Ansible et les modules du SDK Azure Python installés sur votre système hôte.
    - Installez Ansible sur [CentOS 7.4](ansible-install-configure.md#centos-74), [Ubuntu 16.04 LTS](ansible-install-configure.md#ubuntu-1604-lts) et [SLES 12 SP2](ansible-install-configure.md#sles-12-sp2)
- Les informations d’identification Azure, et Ansible configuré pour les utiliser.
    - [Créer des informations d’identification Azure et configurer Ansible](ansible-install-configure.md#create-azure-credentials)
- Azure CLI version 2.0.4 ou ultérieure. Exécutez `az --version` pour trouver la version. 
    - Si vous devez mettre à niveau, consultez [Installation d’Azure CLI 2.0]( /cli/azure/install-azure-cli). Vous pouvez également utiliser [Azure Cloud Shell](/azure/cloud-shell/quickstart) à partir de votre navigateur web.


## <a name="create-supporting-azure-resources"></a>Créer des ressources Azure de support
Dans cet exemple, vous créez un runbook qui déploie une machine virtuelle dans une infrastructure existante. Créez d’abord un groupe de ressources avec [az group create](/cli/azure/group#az-group-create). L’exemple suivant crée un groupe de ressources nommé *myResourceGroup* à l’emplacement *eastus* :

```azurecli
az group create --name myResourceGroup --location eastus
```

Créez un réseau virtuel pour votre machine virtuelle avec [az network vnet create](/cli/azure/network/vnet#az-network-vnet-create). L’exemple suivant permet de créer un réseau virtuel nommé *myVnet* et un sous-réseau nommé *mySubnet* :

```azurecli
az network vnet create \
  --resource-group myResourceGroup \
  --name myVnet \
  --address-prefix 10.0.0.0/16 \
  --subnet-name mySubnet \
  --subnet-prefix 10.0.1.0/24
```


## <a name="create-and-run-ansible-playbook"></a>Créer et exécuter un playbook Ansible
Créez un playbook Ansible nommé *azure_create_vm.yml* et collez le contenu suivant. Cet exemple crée une seule machine virtuelle et configure les informations d’identification SSH. Entrez vos données de clé publique complètes dans la paire *key_data* comme suit :

```yaml
- name: Create Azure VM
  hosts: localhost
  connection: local
  tasks:
  - name: Create VM
    azure_rm_virtualmachine:
      resource_group: myResourceGroup
      name: myVM
      vm_size: Standard_DS1_v2
      admin_username: azureuser
      ssh_password_enabled: false
      ssh_public_keys: 
        - path: /home/azureuser/.ssh/authorized_keys
          key_data: "ssh-rsa AAAAB3Nz{snip}hwhqT9h"
      image:
        offer: CentOS
        publisher: OpenLogic
        sku: '7.5'
        version: latest
```

Pour créer la machine virtuelle avec Ansible, exécutez le playbook comme suit :

```bash
ansible-playbook azure_create_vm.yml
```

La sortie est similaire à l’exemple suivant, qui montre que la machine virtuelle a été créée :

```bash
PLAY [Create Azure VM] ****************************************************

TASK [Gathering Facts] ****************************************************
ok: [localhost]

TASK [Create VM] **********************************************************
changed: [localhost]

PLAY RECAP ****************************************************************
localhost                  : ok=2    changed=1    unreachable=0    failed=0
```


## <a name="next-steps"></a>Étapes suivantes
Cet exemple crée une machine virtuelle dans un groupe de ressources existant avec un réseau virtuel déjà déployé. Pour obtenir un exemple plus détaillé de la façon d’utiliser Ansible pour créer des ressources de support, comme un réseau virtuel et des règles de groupe de sécurité réseau, consultez [Créer un environnement de machine virtuelle complète avec Ansible](ansible-create-complete-vm.md).
