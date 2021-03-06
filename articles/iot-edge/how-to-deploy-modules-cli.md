---
title: Déployer des modules Azure IoT Edge (CLI) | Microsoft Docs
description: Utiliser l’extension IoT pour Azure CLI 2.0 afin de déployer des modules sur un appareil IoT Edge
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 06/08/2018
ms.topic: conceptual
ms.reviewer: menchi
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 98a4be02188f7e0462979792a6061d535a64a18d
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37095971"
---
# <a name="deploy-azure-iot-edge-modules-with-azure-cli-20"></a>Déployer des modules Azure IoT Edge avec Azure CLI 2.0

Une fois que vous avez créé des modules IoT Edge avec votre logique métier, vous pouvez les déployer sur vos appareils afin qu’ils opèrent à la périphérie. Si plusieurs modules fonctionnent ensemble pour collecter et traiter les données, vous pouvez les déployer tous à la fois et déclarer les règles de routage qui les connectent. 

[Azure CLI 2.0](https://docs.microsoft.com/cli/azure?view=azure-cli-latest) est un outil à ligne de commande open source et multiplateforme, destiné à la gestion des ressources Azure telles que IoT Edge. Il vous permet de gérer les ressources Azure IoT Hub, les instances de service Device Provisioning et les hubs liés dès l’installation. La nouvelle extension IoT enrichit Azure CLI 2.0 avec des fonctionnalités telles que la gestion des périphériques et la fonctionnalité complète de IoT Edge.

Cet article explique comment créer un manifeste de déploiement JSON, puis utiliser ce fichier pour étendre le déploiement à un appareil IoT Edge. Pour plus d’informations sur la création d’un déploiement ciblant plusieurs appareils en fonction de leurs balises partagées, consultez [Déployer et surveiller des modules IoT Edge à grande échelle](how-to-deploy-monitor-cli.md).

## <a name="prerequisites"></a>Prérequis

* Un [hub IoT](../iot-hub/iot-hub-create-using-cli.md) dans votre abonnement Azure. 
* Un [appareil IoT Edge](how-to-register-device-cli.md) avec le runtime IoT Edge installé.
* [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) dans votre environnement. La version d’Azure CLI 2.0 doit être au minimum 2.0.24. Utilisez `az –-version` pour valider. Cette version prend en charge les commandes d’extension az et introduit l’infrastructure de la commande Knack. 
* [Extension IoT pour Azure CLI 2.0](https://github.com/Azure/azure-iot-cli-extension).

## <a name="configure-a-deployment-manifest"></a>Configurer un manifeste de déploiement

Un manifeste de déploiement est un document JSON qui décrit les modules à déployer, le flux des données entre les modules et les propriétés souhaitées des jumeaux de module. Pour plus d’informations sur le fonctionnement et la création des manifestes de déploiement, consultez [Comprendre comment les modules IoT Edge peuvent être utilisés, configurés et réutilisés](module-composition.md).

Pour déployer des modules à l’aide d’Azure CLI 2.0, enregistrez localement le manifeste de déploiement dans un fichier .txt. Vous utiliserez le chemin du fichier dans la section suivante au moment d’exécuter la commande permettant d’appliquer la configuration à votre appareil. 

Par exemple, voici un manifeste de déploiement de base comportant un seul module :

   ```json
   {
     "moduleContent": {
       "$edgeAgent": {
         "properties.desired": {
           "schemaVersion": "1.0",
           "runtime": {
             "type": "docker",
             "settings": {
               "minDockerVersion": "v1.25",
               "loggingOptions": "",
               "registryCredentials": {
                 "registryName": {
                   "username": "",
                   "password": "",
                   "address": ""
                 }
               }
           },
           "systemModules": {
             "edgeAgent": {
               "type": "docker",
               "settings": {
                 "image": "mcr.microsoft.com/azureiotedge-agent:1.0",
                 "createOptions": "{}"
               }
             },
             "edgeHub": {
               "type": "docker",
               "status": "running",
               "restartPolicy": "always",
               "settings": {
                 "image": "mcr.microsoft.com/azureiotedge-hub:1.0",
                 "createOptions": "{}"
               }
             }
           },
           "modules": {
             "tempSensor": {
               "version": "1.0",
               "type": "docker",
               "status": "running",
               "restartPolicy": "always",
               "settings": {
                 "image": "mcr.microsoft.com/azureiotedge-simulated-temperature-sensor:1.0",
                 "createOptions": "{}"
               }
             }
           }
         }
       },
       "$edgeHub": {
         "properties.desired": {
           "schemaVersion": "1.0",
           "routes": {
               "route": "FROM /* INTO $upstream"
           },
           "storeAndForwardConfiguration": {
             "timeToLiveSecs": 7200
           }
         }
       },
       "tempSensor": {
         "properties.desired": {}
       }
     }
   }
   ```

## <a name="deploy-to-your-device"></a>Déployer sur votre appareil

Vous déployez les modules sur votre appareil en appliquant le manifeste de déploiement que vous avez configuré avec les informations des modules. 

Pour appliquer la configuration à un appareil IoT Edge, utilisez la commande suivante :

   ```cli
   az iot hub apply-configuration --device-id [device id] --hub-name [hub name] --content [file path]
   ```

Le paramètre device-id respecte la casse. Le paramètre content pointe vers le fichier manifeste de déploiement que vous avez enregistré. 

## <a name="view-modules-on-your-device"></a>Afficher les modules sur votre appareil

Une fois les modules déployés sur votre appareil, vous pouvez les voir tous à l’aide de la commande suivante : 

Affichez les modules sur votre appareil IoT Edge :
    
   ```cli
   az iot hub module-identity list --device-id [device id] --hub-name [hub name]
   ```

Le paramètre device-id respecte la casse.

   ![Faire la liste des modules](./media/how-to-deploy-cli/list-modules.png)

## <a name="next-steps"></a>Étapes suivantes

Découvrez comment [déployer et surveiller des modules IoT Edge à grande échelle](how-to-deploy-monitor.md).