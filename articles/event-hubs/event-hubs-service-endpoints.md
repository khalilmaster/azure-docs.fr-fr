---
title: Points de terminaison de service et règles de réseau virtuel dans Azure Event Hubs | Microsoft Docs
description: Ajoutez un point de terminaison de service Microsoft.ServiceBus à un réseau virtuel.
services: event-hubs
documentationcenter: ''
author: clemensv
manager: timlt
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.date: 06/26/2018
ms.author: clemensv
ms.openlocfilehash: a23e5414cd3c60192badfee65b14c49cd5e96f4e
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37034505"
---
# <a name="use-virtual-network-service-endpoints-with-azure-event-hubs"></a>Utiliser des points de terminaison de service de réseau virtuel avec Azure Event Hubs

L’intégration d’Event Hubs à des [points de terminaison de service de réseau virtuel (VNet)][vnet-sep] permet de sécuriser l’accès aux fonctionnalités de messagerie à partir de charges de travail, notamment celles de machines virtuelles liées à des réseaux virtuels. Le chemin du trafic réseau est sécurisé aux deux extrémités. 

Une fois configuré pour être lié à au moins un point de terminaison de service de sous-réseau de réseau virtuel, l’espace de noms Event Hubs respectif n’accepte que le trafic provenant de réseaux virtuels autorisés. Du point de vue du réseau virtuel, la liaison d’un espace de noms Event Hubs à un point de terminaison de service configure un tunnel de mise en réseau isolé allant du sous-réseau de réseau virtuel au service de messagerie.

Il en résulte une relation privée et isolée entre les charges de travail liées au sous-réseau et l’espace de noms Event Hubs respectif, et ce malgré le fait que l’adresse réseau observable du point de terminaison du service de messagerie figure dans une plage d’adresses IP publique.

## <a name="advanced-security-scenarios-enabled-by-vnet-integration"></a>Scénarios de sécurité avancés pris en charge par l’intégration à VNet 

En général, les solutions nécessitant une sécurité étroite et compartimentée et dans lesquelles les sous-réseaux de réseau virtuel assurent la segmentation entre les services compartimentés ont toujours besoin de chemins de communication entre les services résidant dans ces compartiments.

Toute route IP immédiate entre les compartiments, notamment celles acheminant le trafic HTTPS sur TCP/IP, comporte un risque d’exploitation des vulnérabilités émanant de la couche réseau. Les services de messagerie fournissent des chemins de communication complètement isolés, les messages étant même écrits sur disque à mesure qu’ils passent d’une partie à une autre. Les charges de travail dans deux réseaux virtuels distincts qui sont tous les deux liés à la même instance d’Event Hubs peuvent communiquer de manière efficace et fiable au moyen de messages, sans aucun impact sur l’intégrité des limites d’isolement de chaque réseau.
 
Vos solutions cloud sensibles en matière de sécurité accèdent donc aux fonctionnalités de messagerie asynchrones de pointe d’Azure qui allient fiabilité et scalabilité. Vos solutions peuvent désormais utiliser la messagerie pour créer des chemins de communication entre des compartiments de solution sécurisés, ce qui offre intrinsèquement une meilleure sécurité que n’importe quel mode de communication pair à pair (notamment HTTPS et d’autres protocoles de sockets sécurisés par TLS).

## <a name="bind-event-hubs-to-virtual-networks"></a>Lier Event Hubs à des réseaux virtuels

Les *règles de réseau virtuel* sont une fonctionnalité de sécurité de pare-feu. Elles permettent de contrôler si votre serveur Azure Event Hubs doit accepter ou non les connexions d’un sous-réseau de réseau virtuel particulier.

La liaison d’un espace de noms Event Hubs à un réseau virtuel est un processus en deux étapes. Vous devez d’abord créer un **point de terminaison de service de réseau virtuel** sur un sous-réseau de réseau virtuel et l’activer pour « Microsoft.ServiceBus », comme expliqué dans la [vue d’ensemble des points de terminaison de service][vnet-sep]. Après avoir ajouté le point de terminaison de service, liez-le à l’espace de noms Event Hubs au moyen d’une *règle de réseau virtuel*.

La règle de réseau virtuel est une association nommée de l’espace de noms Event Hubs et d’un sous-réseau de réseau virtuel. Une fois la règle en place, toutes les charges de travail liées au sous-réseau sont autorisées à accéder à l’espace de noms Event Hubs. Event Hubs n’établit jamais de connexions sortantes, ne nécessite aucun accès et ne se voit donc jamais accorder l’accès à votre sous-réseau quand cette règle est activée.

### <a name="create-a-virtual-network-rule-with-azure-resource-manager-templates"></a>Créer une règle de réseau virtuel avec des modèles Azure Resource Manager

Le modèle Resource Manager suivant permet d’ajouter une règle de réseau virtuel à un espace de noms Event Hubs.

Paramètres du modèle :

* **namespaceName** : espace de noms Event Hubs.
* **vnetRuleName** : nom de la règle de réseau virtuel à créer.
* **virtualNetworkingSubnetId** : chemin complet de Resource Manager pour le sous-réseau de réseau virtuel, par exemple `subscriptions/{id}/resourceGroups/{rg}/providers/Microsoft.Network/virtualNetworks/{vnet}/subnets/default` pour le sous-réseau par défaut d’un réseau virtuel.

```json
{  
   "$schema":"http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
   "contentVersion":"1.0.0.0",
   "parameters":{     
          "namespaceName":{  
             "type":"string",
             "metadata":{  
                "description":"Name of the namespace"
             }
          },
          "vnetRuleName":{  
             "type":"string",
             "metadata":{  
                "description":"Name of the Authorization rule"
             }
          },
          "virtualNetworkSubnetId":{  
             "type":"string",
             "metadata":{  
                "description":"subnet Azure Resource Manager ID"
             }
          }
      },
    "resources": [
        {
            "apiVersion": "2018-01-01-preview",
            "name": "[concat(parameters('namespaceName'), '/', parameters('vnetRuleName'))]",
            "type":"Microsoft.EventHub/namespaces/VirtualNetworkRules",         
            "properties": {             
                "virtualNetworkSubnetId": "[parameters('virtualNetworkSubnetId')]"  
            }
        } 
    ]
}
```

Pour déployer le modèle, suivez les instructions pour [Azure Resource Manager][lnk-deploy].

## <a name="next-steps"></a>Étapes suivantes

Pour plus d’informations sur les réseaux virtuels, consultez les liens suivants :

- [Points de terminaison de service de réseau virtuel Azure][vnet-sep]
- [Filtrage IP Azure Event Hubs][ip-filtering]

[vnet-sep]: ../virtual-network/virtual-network-service-endpoints-overview.md
[lnk-deploy]: ../azure-resource-manager/resource-group-template-deploy.md
[ip-filtering]: event-hubs-ip-filtering.md