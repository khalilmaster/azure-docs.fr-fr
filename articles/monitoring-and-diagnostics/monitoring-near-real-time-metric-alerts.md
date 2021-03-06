---
title: Ressources prises en charge pour les alertes de métrique Azure Monitor plus récentes
description: Référence sur les métriques et les journaux prise en charge pour des alertes métriques Azure plus récentes quasiment en temps réel.
author: snehithm
services: monitoring
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 06/29/2018
ms.author: snmuvva
ms.component: alerts
ms.openlocfilehash: 4d51b099532d3052acc190231ec4be17765a427e
ms.sourcegitcommit: f606248b31182cc559b21e79778c9397127e54df
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38971021"
---
# <a name="introduction"></a>Introduction
Azure Monitor prend désormais en charge un [nouveau type d’alerte de métrique](monitoring-overview-unified-alerts.md) qui présente d’importants avantages par rapport aux anciennes [alertes de métrique classiques](insights-alerts-portal.md). Des métriques sont disponibles pour une [longue liste de services Azure](monitoring-supported-metrics.md). Les alertes plus récentes prennent en charge un sous-ensemble (croissant) des types de ressource. Cet article répertorie ce sous-ensemble. 

Vous pouvez également utiliser des alertes métriques plus récentes sur des journaux Log Analytics populaires extraits en tant que métriques dans le cadre des métriques de journaux (préversion).  
- [Les compteurs de performance](../log-analytics/log-analytics-data-sources-performance-counters.md) pour les machines Windows et Linux
- [Enregistrements de pulsations pour Agent Health](../operations-management-suite/oms-solution-agenthealth.md)
- Enregistrements de la [gestion des mises à jour](../operations-management-suite/oms-solution-update-management.md)
- Journaux sur les [données d’événement](../log-analytics/log-analytics-data-sources-windows-events.md)
 
> [!NOTE]
> La métrique et/ou la dimension spécifique ne s’affichera que si des données correspondantes existent pour la période choisie. Ces métriques sont disponibles pour les clients qui ont des espaces de travail Log Analytics dans les régions USA Est, USA Centre-Ouest et Europe Ouest. Les métriques Log Analytics sont actuellement en préversion et susceptibles d’être modifiées.

## <a name="portal-powershell-cli-rest-support"></a>Portail, PowerShell, CLI, prise en charge de REST
À l’heure actuelle, il n’est possible de créer des alertes métriques plus récentes que sur le Portail Azure, [l’API REST](https://docs.microsoft.com/rest/api/monitor/metricalerts/createorupdate) ou les [modèles Resource Manager](monitoring-create-metric-alerts-with-templates.md). La prise en charge de la configuration d’alertes plus récentes en utilisant PowerShell et l’interface de ligne de commande Azure (Azure CLI 2.0) est bientôt disponible.

## <a name="metrics-and-dimensions-supported"></a>Métriques et dimensions prises en charge
Les alertes métriques plus récentes prennent en charge la génération d’alertes pour les métriques qui utilisent des dimensions. Vous pouvez utiliser les dimensions pour filtrer votre métrique au niveau approprié. Toutes les métriques prises en charge, ainsi que les dimensions applicables, peuvent être examinées et visualisés à partir d’[Azure Monitor – Metrics Explorer (préversion)](monitoring-metric-charts.md).

Voici la liste complète des sources de métrique d’Azure Monitor prises en charge par les alertes plus récentes :

|Type de ressource  |Dimensions prises en charge  | Mesures disponibles|
|---------|---------|----------------|
|Microsoft.ApiManagement/service     | OUI        | [Gestion des API](monitoring-supported-metrics.md#microsoftapimanagementservice)|
|Microsoft.Automation/automationAccounts     |     OUI   | [Comptes Automation](monitoring-supported-metrics.md#microsoftautomationautomationaccounts)|
|Microsoft.Batch/batchAccounts | N/A| [Comptes Batch](monitoring-supported-metrics.md#microsoftbatchbatchaccounts)|
|Microsoft.Cache/Redis     |    N/A     |[Cache Redis](monitoring-supported-metrics.md#microsoftcacheredis)|
|Microsoft.CognitiveServices/accounts     |    N/A     | [Cognitive Services](monitoring-supported-metrics.md#microsoftcognitiveservicesaccounts)|
|Microsoft.Compute/virtualMachines     |    N/A     | [Machines virtuelles](monitoring-supported-metrics.md#microsoftcomputevirtualmachines)|
|Microsoft.Compute/virtualMachineScaleSets     |   N/A      |[Groupe de machines virtuelles identiques](monitoring-supported-metrics.md#microsoftcomputevirtualmachinescalesets)|
|Microsoft.ContainerInstance/containerGroups | OUI| [Groupes de conteneur](monitoring-supported-metrics.md#microsoftcontainerinstancecontainergroups)|
|Microsoft.ContainerService/managedClusters | OUI | [Clusters managés](monitoring-supported-metrics.md#microsoftcontainerservicemanagedclusters)|
|Microsoft.DataFactory/datafactories| OUI| [Fabriques de données V1](monitoring-supported-metrics.md#microsoftdatafactorydatafactories)|
|Microsoft.DataFactory/factories     |   OUI     |[Fabriques de données V2](monitoring-supported-metrics.md#microsoftdatafactoryfactories)|
|Microsoft.DBforMySQL/servers     |   N/A      |[Base de données pour MySQL](monitoring-supported-metrics.md#microsoftdbformysqlservers)|
|Microsoft.DBforPostgreSQL/servers     |    N/A     | [Base de données pour PostgreSQL](monitoring-supported-metrics.md#microsoftdbforpostgresqlservers)|
|Microsoft.EventHub/namespaces     |  OUI      |[Concentrateurs d'événements](monitoring-supported-metrics.md#microsofteventhubnamespaces)|
|Microsoft.KeyVault/vaults| Non  | [Coffres](monitoring-supported-metrics.md#microsoftkeyvaultvaults)|
|Microsoft.Logic/workflows     |     N/A    |[Logic Apps](monitoring-supported-metrics.md#microsoftlogicworkflows) |
|Microsoft.Network/applicationGateways     |    N/A     | [Application Gateways](monitoring-supported-metrics.md#microsoftnetworkapplicationgateways) |
|Microsoft.Network/expressRouteCircuits | N/A |  [Circuits Express Route](monitoring-supported-metrics.md#microsoftnetworkexpressroutecircuits) |
|Microsoft.Network/dnsZones | N/A| [Zones DNS](monitoring-supported-metrics.md#microsoftnetworkdnszones) |
|Microsoft.Network/loadBalancers (uniquement pour les références SKU Standard)| OUI| [Équilibreurs de charge](monitoring-supported-metrics.md#microsoftnetworkloadbalancers) |
|Microsoft.Network/publicipaddresses     |  N/A       |[Adresses IP publiques](monitoring-supported-metrics.md#microsoftnetworkpublicipaddresses)|
|Microsoft.PowerBIDedicated/capacities | N/A | [Capacités](monitoring-supported-metrics.md#microsoftpowerbidedicatedcapacities)|
|Microsoft.Network/trafficManagerProfiles | OUI | [Profils Traffic Manager](monitoring-supported-metrics.md#microsoftnetworktrafficmanagerprofiles) |
|Microsoft.Search/searchServices     |   N/A      |[Services Recherche](monitoring-supported-metrics.md#microsoftsearchsearchservices)|
|Microsoft.ServiceBus/namespaces     |  OUI       |[Service Bus](monitoring-supported-metrics.md#microsoftservicebusnamespaces)|
|Microsoft.Storage/storageAccounts     |    OUI     | [Comptes de stockage](monitoring-supported-metrics.md#microsoftstoragestorageaccounts)|
|Microsoft.Storage/storageAccounts/services     |     OUI    | [Services blob](monitoring-supported-metrics.md#microsoftstoragestorageaccountsblobservices), [Services de fichiers](monitoring-supported-metrics.md#microsoftstoragestorageaccountsfileservices), [Services de file d’attente](monitoring-supported-metrics.md#microsoftstoragestorageaccountsqueueservices) et [Services de table](monitoring-supported-metrics.md#microsoftstoragestorageaccountstableservices)|
|Microsoft.StreamAnalytics/streamingjobs     |  N/A       | [Stream Analytics](monitoring-supported-metrics.md#microsoftstreamanalyticsstreamingjobs)|
| Microsoft.Web/serverfarms | OUI | [Plans App Service](monitoring-supported-metrics.md#microsoftwebserverfarms)  |
|Microsoft.OperationalInsights/workspaces (Préversion) | OUI|[Espaces de travail Log Analytics](monitoring-supported-metrics.md#microsoftoperationalinsightsworkspaces)|



## <a name="payload-schema"></a>Schéma de la charge utile

L’opération POST contient le schéma et la charge utile JSON ci-après pour toutes les alertes plus récentes basées sur des métriques lorsqu’un [groupe d’action](monitoring-action-groups.md) configuré de manière appropriée est utilisé :

```json
{"schemaId":"AzureMonitorMetricAlert","data":
    {
    "version": "2.0",
    "status": "Activated",
    "context": {
    "timestamp": "2018-02-28T10:44:10.1714014Z",
    "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/Contoso/providers/microsoft.insights/metricAlerts/StorageCheck",
    "name": "StorageCheck",
    "description": "",
    "conditionType": "SingleResourceMultipleMetricCriteria",
    "condition": {
      "windowSize": "PT5M",
      "allOf": [
        {
          "metricName": "Transactions",
          "dimensions": [
            {
              "name": "AccountResourceId",
              "value": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/Contoso/providers/Microsoft.Storage/storageAccounts/diag500"
            },
            {
              "name": "GeoType",
              "value": "Primary"
            }
          ],
          "operator": "GreaterThan",
          "threshold": "0",
          "timeAggregation": "PT5M",
          "metricValue": 1.0
        },
      ]
    },
    "subscriptionId": "00000000-0000-0000-0000-000000000000",
    "resourceGroupName": "Contoso",
    "resourceName": "diag500",
    "resourceType": "Microsoft.Storage/storageAccounts",
    "resourceId": "/subscriptions/1e3ff1c0-771a-4119-a03b-be82a51e232d/resourceGroups/Contoso/providers/Microsoft.Storage/storageAccounts/diag500",
    "portalLink": "https://portal.azure.com/#resource//subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/Contoso/providers/Microsoft.Storage/storageAccounts/diag500"
  },
        "properties": {
                "key1": "value1",
                "key2": "value2"
        }
    }
}
```

## <a name="next-steps"></a>Étapes suivantes

* En savoir plus sur la nouvelle [expérience Alertes](monitoring-overview-unified-alerts.md).
* En savoir plus sur les [alertes de journal dans Azure](monitor-alerts-unified-log.md).
* En savoir plus sur les [alertes dans Azure](monitoring-overview-alerts.md).
