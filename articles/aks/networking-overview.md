---
title: Configuration réseau dans Azure Kubernetes Service (AKS)
description: En savoir plus sur la configuration réseau de base et avancée dans Azure Kubernetes Service (AKS).
services: container-service
author: mmacy
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 06/15/2018
ms.author: marsma
ms.openlocfilehash: 207accc30e10c4e2bed5b713fc59e2f9ad86a876
ms.sourcegitcommit: 638599eb548e41f341c54e14b29480ab02655db1
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/21/2018
ms.locfileid: "36311093"
---
# <a name="network-configuration-in-azure-kubernetes-service-aks"></a>Configuration réseau dans Azure Kubernetes Service (AKS)

Lorsque vous créez un cluster Azure Kubernetes Service (AKS), vous pouvez sélectionner deux options de mise en réseau : **de base** ou **avancée**.

## <a name="basic-networking"></a>Mise en réseau de base

L’option de mise en réseau **de base** est la configuration par défaut de la création de cluster AKS. La configuration réseau du cluster et de ses pods est gérée intégralement par Azure, et est appropriée pour les déploiements qui ne nécessitent pas de configuration personnalisée de réseau virtuel. Avec la mise en réseau de base, vous ne disposez d’aucun contrôle sur la configuration réseau, par exemple sur les sous-réseaux ou les plages d’adresses IP affectées au cluster.

Les nœuds d’un cluster AKS configurés pour la mise en réseau de base utilisent le plug-in Kubernetes [kubenet][kubenet].

## <a name="advanced-networking"></a>Mise en réseau avancée

La mise en réseau **avancée** place vos pods dans un réseau virtuel Azure que vous configurez, en leur octroyant une connectivité automatique aux ressources de réseau virtuel et une intégration avec le riche ensemble de fonctionnalités offert par les réseaux virtuels.
La mise en réseau avancée est disponible en cas de déploiement de clusters AKS avec le [Portail Azure][portal], Azure CLI ou un modèle Resource Manager.

Les nœuds d’un cluster AKS configurés pour la mise en réseau avancée utilisent le plug-in Kubernetes[Azure Container Networking Interface (CNI)][cni-networking].

![Diagramme représentant 2 nœuds avec des ponts les reliant chacun à un réseau virtuel Azure][advanced-networking-diagram-01]

## <a name="advanced-networking-features"></a>Fonctions de la mise en réseau avancée

La mise en réseau avancée procure les avantages suivants :

* Déployez votre cluster AKS dans un réseau virtuel existant ou créez un nouveau réseau virtuel et un sous-réseau pour votre cluster.
* Chaque pod du cluster se voit affecter une adresse IP dans le réseau virtuel et peut communiquer directement avec les autres pods du cluster et les autres nœuds du réseau virtuel.
* Un pod peut se connecter à d’autres services dans un réseau virtuel appairé, et à d’autres réseaux locaux via des connexions ExpressRoute et VPN site à site (S2S). Les pods sont également accessibles en local.
* Exposez un service Kubernetes en interne ou en externe via Azure Load Balancer. Également une fonction de la mise en réseau de base.
* Les pods dans un sous-réseau disposant de points de terminaison de service activés peuvent se connecter en toute sécurité aux services Azure (Stockage et SQL Database, par exemple).
* Utilisez les itinéraires définis par l’utilisateur pour acheminer le trafic des pods vers une appliance virtuelle réseau.
* Les pods peuvent accéder aux ressources sur les réseaux Internet publics. Également une fonction de la mise en réseau de base.

> [!IMPORTANT]
> Chacun des nœuds d’un cluster AKS configuré avec le Portail Azure pour la mise en réseau avancée peut héberger un maximum de **30 pods**.  Le seul moyen possible de changer la valeur maximale consiste à modifier la propriété maxPods lors du déploiement d’un cluster avec un modèle Resource Manager. Chaque réseau virtuel configuré pour une utilisation avec le plug-in Azure CNI est limité à **4 096 adresses IP**.

## <a name="advanced-networking-prerequisites"></a>Conditions préalables de mise en réseau avancée

* Le réseau virtuel du cluster AKS doit autoriser la connectivité Internet sortante.
* Ne créez pas plus d’un cluster AKS dans le même sous-réseau.
* La mise en réseau avancée pour AKS ne prend pas en charge les réseaux virtuels qui utilisent les zones DNS privées Azure.
* Les clusters AKS ne peuvent pas utiliser `169.254.0.0/16`, `172.30.0.0/16` ou `172.31.0.0/16` pour la plage d’adresses de service Kubernetes.
* Le principal de service utilisé pour le cluster AKS doit avoir des autorisations `Contributor` pour le groupe de ressources qui contient le réseau virtuel existant.

## <a name="plan-ip-addressing-for-your-cluster"></a>Planifier l’adressage IP pour votre cluster

Les clusters configurés avec la mise en réseau avancée nécessitent une planification supplémentaire. La taille de votre réseau virtuel et de son sous-réseau doit être suffisante pour prendre en charge le nombre de pods que vous envisagez d’exécuter, ainsi que le nombre de nœuds du cluster.

Les adresses IP des pods et des nœuds de cluster sont affectées à partir du sous-réseau spécifié du réseau virtuel. Chaque nœud est configuré avec une adresse IP primaire, qui est l’adresse IP du nœud, et 30 adresses IP supplémentaires préconfigurées par Azure CNI, qui sont affectées aux pods planifiés sur le nœud. Lorsque vous faites monter en charge votre cluster, chaque nœud est configuré de manière similaire avec des adresses IP du sous-réseau.

Le plan d’adressage IP pour un cluster AKS se compose d’un réseau virtuel, au moins un sous-réseau pour les nœuds et les pods, et une plage d’adresses de service Kubernetes.

| Plage Azure/ressource Azure | Limites et tailles |
| --------- | ------------- |
| Réseau virtuel | Un réseau virtuel Azure peut avoir un volume maximal de /8, mais peut uniquement avoir 4 096 adresses IP configurées. |
| Sous-réseau | Doit être suffisamment grand pour contenir les nœuds et les pods. Pour calculer la taille minimale de votre sous-réseau : (nombre de nœuds) + (nombre de nœuds * pods par nœud). Pour un cluster à 50 nœuds : (50) + (50 * 30) = 1 550 ; votre sous-réseau doit être /21 ou plus grand. |
| Plage d’adresses de service Kubernetes | Cette plage ne doit être utilisée par aucun élément réseau sur ce réseau virtuel ou connectée à celui-ci. Le CIDR d’adresse du service doit être inférieur à /12. |
| Adresse IP du service DNS Kubernetes | Adresse IP dans la plage d’adresses de service Kubernetes, qui sera utilisée par la détection de service de cluster (kube-dns). |
| Adresse de pont Docker | Adresse IP (en notation CIDR) utilisée en tant qu’adresse IP de pont Docker sur les nœuds. Valeur par défaut : 172.17.0.1/16. |

Comme indiqué précédemment, chaque réseau virtuel configuré pour une utilisation avec le plug-in Azure CNI est limité à **4 096 adresses IP**. Chaque nœud de cluster configuré pour la mise en réseau avancée peut héberger un maximum de **30 pods**.

## <a name="deployment-parameters"></a>Paramètres de déploiement

Lors de la création d’un cluster AKS, les paramètres suivants sont configurables pour la mise en réseau avancée :

**Réseau virtuel** : le réseau virtuel dans lequel vous souhaitez déployer le cluster Kubernetes. Si vous souhaitez créer un réseau virtuel pour votre cluster, sélectionnez *Créer un nouveau*, puis suivez la procédure décrite dans la section *Créer un réseau virtuel*.

**Sous-réseau** : le sous-réseau du réseau virtuel dans lequel vous souhaitez déployer le cluster. Si vous souhaitez créer un nouveau sous-réseau dans le réseau virtuel pour votre cluster, sélectionnez *Créer un nouveau*, puis exécutez la procédure décrite dans la section *Créer un sous-réseau*.

**Plage d’adresses des services Kubernetes** : la *plage d’adresses des services Kubernetes* correspond à la plage à partir de laquelle les adresses IP sont affectées aux services Kubernetes du cluster (pour plus d’informations sur les services Kubernetes, voir [Services][services] dans la documentation de Kubernetes).

La plage d’adresses IP des services Kubernetes :

* doit être située en dehors de la plage d’adresses IP du réseau virtuel de votre cluster ;
* ne doit comporter aucun élément en commun avec les autres réseaux virtuels pour lesquelles il existe un lien de peering avec le réseau virtuel du cluster ;
* ne doit avoir aucun élément en commun avec des adresses IP locales ;

Tout chevauchement entre des plages d’adresses IP est susceptible d’entraîner des comportements imprévisibles. Par exemple, si un pod tente d’accéder à une adresse IP à l’extérieur du cluster, qui se trouve également être une adresse IP de service, des comportements imprévisibles et des défaillances risquent de se produire.

**Adresse IP du service DNS Kubernetes** :l’adresse IP du service DNS du cluster. Cette adresse doit se situer dans la *plage d’adresses du service Kubernetes*.

**Adresse du pont docker** : l’adresse IP et le masque réseau à affecter au pont docker. Cette adresse IP doit être située en dehors de la plage d’adresses IP du réseau virtuel de votre cluster.

## <a name="configure-networking---cli"></a>Configurer la mise en réseau – Interface de ligne de commande

Lors de la création d’un cluster AKS avec Azure CLI, il est également possible de configurer la mise en réseau avancée. Utilisez les commandes suivantes pour créer un nouveau cluster AKS en activant les fonctionnalités de mise en réseau avancée.

Tout d’abord, récupérez l’ID de la ressource du sous-réseau auquel le cluster AKS sera joint :

```console
$ az network vnet subnet list --resource-group myVnet --vnet-name myVnet --query [].id --output tsv

/subscriptions/d5b9d4b7-6fc1-46c5-bafe-38effaed19b2/resourceGroups/myVnet/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/default
```

Utilisez la commande [az aks create][az-aks-create] avec l’argument `--network-plugin azure` pour créer un cluster avec mise en réseau avancée. Remplacez la valeur `--vnet-subnet-id` par l’ID du sous-réseau recueilli à l’étape précédente :

```azurecli
az aks create --resource-group myAKSCluster --name myAKSCluster --network-plugin azure --vnet-subnet-id <subnet-id> --docker-bridge-address 172.17.0.1/16 --dns-service-ip 10.2.0.10 --service-cidr 10.2.0.0/24
```

## <a name="configure-networking---portal"></a>Configurer la mise en réseau – Portail

La capture d’écran suivante de votre portail Azure représente un exemple de configuration de ces paramètres durant la création du cluster AKS :

![Configuration de la mise en réseau avancée dans le portail Azure][portal-01-networking-advanced]

## <a name="frequently-asked-questions"></a>Questions fréquentes (FAQ)

La série suivante de questions-réponses s’applique à la configuration réseau **avancée**.

* *Puis-je configurer la mise en réseau avancée avec l’interface Azure CLI ?*

  Non. La mise en réseau avancée est actuellement disponible uniquement lors du déploiement de clusters AKS dans le portail Azure ou avec un modèle Resource Manager.

* *Puis-je déployer des machines virtuelles dans le sous-réseau de mon cluster ?*

  Non. Le déploiement des machines virtuelles dans le sous-réseau utilisé par votre cluster Kubernetes n’est pas pris en charge. Les machines virtuelles peuvent être déployées dans le même réseau virtuel, mais dans un sous-réseau différent.

* *Puis-je configurer des stratégies de réseau spécifiques aux pods ?*

  Non. Les stratégies de réseau spécifiques aux pods ne sont actuellement pas prises en charge.

* *Le nombre maximal de pods pouvant être déployés sur un nœud peut-il être configuré ?*

  Par défaut, chaque nœud peut héberger 30 pods au maximum. Pour modifier la valeur maximale, vous pouvez uniquement modifier la propriété `maxPods` lors du déploiement d’un cluster avec un modèle Resource Manager.

* *Comment configurer des propriétés supplémentaires pour le sous-réseau développé lors de la création du cluster AKS ? Par exemple, des points de terminaison du service.*

  La liste complète des propriétés pour le réseau virtuel et les sous- réseaux développés durant la création du cluster AKS peut être configurée dans la page de configuration du réseau virtuel standard du portail Azure.

## <a name="next-steps"></a>Étapes suivantes

### <a name="networking-in-aks"></a>Mise en réseau dans AKS

Pour plus d’informations sur la mise en réseau dans AKS, consultez les articles suivants :

[Utiliser une adresse IP statique avec l’équilibrage de charge d’Azure Kubernetes Service (AKS)](static-ip.md)

[Entrée HTTPS sur Azure Container Service (AKS)](ingress.md)

[Utiliser un équilibreur de charge interne avec Azure Container Service (AKS)](internal-lb.md)

### <a name="acs-engine"></a>Moteur ACS

Le [moteur Azure Container Service (moteur ACS)][acs-engine] est un projet open source générant des modèles Azure Resource Manager que vous pouvez utiliser pour déployer des clusters Docker sur Azure. Kubernetes, DC/OS, le mode Swarm et les orchestrateurs Swarm peuvent être déployés avec le moteur ACS.

Les clusters Kubernetes créés avec le moteur ACS prennent en charge les plug-ins [kubenet][kubenet] et [Azure CNI][cni-networking]. En l’état, les scénarios de mise en réseau de base et avancée sont pris en charge par le moteur ACS.

<!-- IMAGES -->
[advanced-networking-diagram-01]: ./media/networking-overview/advanced-networking-diagram-01.png
[portal-01-networking-advanced]: ./media/networking-overview/portal-01-networking-advanced.png

<!-- LINKS - External -->
[acs-engine]: https://github.com/Azure/acs-engine
[cni-networking]: https://github.com/Azure/azure-container-networking/blob/master/docs/cni.md
[kubenet]: https://kubernetes.io/docs/concepts/cluster-administration/network-plugins/#kubenet
[services]: https://kubernetes.io/docs/concepts/services-networking/service/
[portal]: https://portal.azure.com

<!-- LINKS - Internal -->
[az-aks-create]: /cli/azure/aks?view=azure-cli-latest#az-aks-create
[aks-ssh]: aks-ssh.md
