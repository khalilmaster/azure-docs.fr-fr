---
title: Démarrage rapide (C#) pour envoyer des données de télémétrie à Azure IoT Hub | Microsoft Docs
description: Dans ce démarrage rapide, vous exécutez deux exemples d’applications C# pour envoyer des données de télémétrie simulées à un IoT Hub et lire les données de télémétrie provenant de l’IoT Hub à traiter dans le cloud.
author: dominicbetts
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.devlang: csharp
ms.topic: quickstart
ms.custom: mvc
ms.date: 06/20/2018
ms.author: dobett
ms.openlocfilehash: 501a8f2dad0537f0288dfd6a6d7bedcac054ab86
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38619299"
---
# <a name="quickstart-send-telemetry-from-a-device-to-an-iot-hub-and-read-the-telemetry-from-the-hub-with-a-back-end-application-c"></a>Démarrage rapide : envoyer des données de télémétrie à partir d’un appareil à un IoT Hub et lire les données de télémétrie depuis le concentrateur avec une application back-end (C#)

[!INCLUDE [iot-hub-quickstarts-1-selector](../../includes/iot-hub-quickstarts-1-selector.md)]

IoT Hub est un service Azure qui vous permet de traiter de gros volumes de données de télémétrie envoyées par vos appareils IoT dans le cloud à des fins de stockage ou de traitement. Dans ce guide de démarrage rapide, vous envoyez des données de télémétrie à partir d’une application d’appareil simulé, via IoT Hub, à une application back-end pour traitement.

Ce démarrage rapide utilise deux applications C# prédéfinies, une pour envoyer des données de télémétrie et l’autre pour lire ces données de télémétrie provenant du concentrateur. Avant d’exécuter ces deux applications, vous créez un IoT Hub et inscrivez un appareil auprès du concentrateur.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Si vous n’avez pas d’abonnement Azure, créez un [compte gratuit](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) avant de commencer.

## <a name="prerequisites"></a>Prérequis

Les deux exemples d’applications que vous exécutez dans ce guide de démarrage rapide sont écrits à l’aide de C#. Votre machine de développement doit disposer du Kit SDK .NET Core 2.1.0 ou version ultérieure.

Vous pouvez télécharger le Kit SDK .NET Core pour plusieurs plateformes sur [.NET](https://www.microsoft.com/net/download/all).

Vous pouvez vérifier la version actuelle de C# sur votre machine de développement à l’aide de la commande suivante :

```cmd/sh
dotnet --version
```

Téléchargez l’exemple de projet C# à partir de https://github.com/Azure-Samples/azure-iot-samples-csharp/archive/master.zip et extrayez l’archive ZIP.

## <a name="create-an-iot-hub"></a>Créer un hub IoT

[!INCLUDE [iot-hub-quickstarts-create-hub](../../includes/iot-hub-quickstarts-create-hub.md)]

## <a name="register-a-device"></a>Inscrire un appareil

Un appareil doit être inscrit dans votre hub IoT pour pouvoir se connecter. Dans ce démarrage rapide, vous utilisez Azure CLI pour inscrire un appareil simulé.

1. Ajoutez l’extension CLI d’IoT Hub et créez l’identité de l’appareil. Remplacez `{YourIoTHubName}` par le nom que vous avez choisi pour votre IoT Hub :

    ```azurecli-interactive
    az extension add --name azure-cli-iot-ext
    az iot hub device-identity create --hub-name {YourIoTHubName} --device-id MyDotnetDevice
    ```

    Si vous choisissez un autre nom pour votre appareil, mettez à jour le nom de l’appareil dans les exemples d’applications avant de les exécuter.

2. Exécutez la commande suivante pour obtenir la _chaîne de connexion_ à l’appareil que vous venez d’inscrire :

    ```azurecli-interactive
    az iot hub device-identity show-connection-string --hub-name {YourIoTHubName} --device-id MyDotnetDevice --output table
    ```

    Notez la chaîne de connexion à l’appareil, du type `Hostname=...=`. Vous utiliserez cette valeur plus loin dans ce démarrage rapide.

3. Vous avez aussi besoin du _point de terminaison compatible Event Hubs_, du _chemin d’accès compatible Event Hubs_ et de la _clé principale iothubowner_ à partir de votre IoT Hub pour permettre à l’application back-end de se connecter à votre IoT Hub et de récupérer les messages. Les commandes suivantes extraient ces valeurs pour votre IoT Hub :

    ```azurecli-interactive
    az iot hub show --query properties.eventHubEndpoints.events.endpoint --name {YourIoTHubName}

    az iot hub show --query properties.eventHubEndpoints.events.path --name {YourIoTHubName}

    az iot hub policy show --name iothubowner --query primaryKey --hub-name {your IoT Hub name}
    ```

    Prenez note de ces trois valeurs, vous les utiliserez plus loin dans ce démarrage rapide.

## <a name="send-simulated-telemetry"></a>Envoyer des données de télémétrie simulée

L’application d’appareil simulé se connecte à un point de terminaison spécifique de l’appareil sur votre IoT Hub et envoie les données de télémétrie simulée (température et humidité).

1. Dans une fenêtre de terminal, accédez au dossier racine de l’exemple de projet C#. Ensuite, accédez au dossier **iot-hub\Quickstarts\simulated-device**.

2. Utilisez un éditeur de texte pour ouvrir le fichier **SimulatedDevice.cs**.

    Remplacez la valeur de la variable `s_connectionString` par la chaîne de connexion d’appareil que vous avez notée précédemment. Puis, enregistrez les modifications apportées au fichier **SimulatedDevice.cs**.

3. Dans la fenêtre de terminal, exécutez les commandes suivantes pour installer les packages requis pour l’application d’appareil simulé :

    ```cmd/sh
    dotnet restore
    ```

4. Dans la fenêtre de terminal, exécutez la commande suivante pour générer et exécuter l’application d’appareil simulé :

    ```cmd/sh
    dotnet run
    ```

    La capture d’écran suivante présente la sortie lorsque l’application d’appareil simulé envoie des données de télémétrie à votre IoT Hub :

    ![Exécuter l’appareil simulé](media/quickstart-send-telemetry-dotnet/SimulatedDevice.png)

## <a name="read-the-telemetry-from-your-hub"></a>Lire les données de télémétrie envoyées par votre hub

L’application back-end se connecte au point de terminaison **Événements** du service sur votre IoT Hub. L’application reçoit les messages appareil-à-cloud envoyés à partir de votre appareil simulé. Une application back-end IoT Hub s’exécute généralement dans le cloud pour recevoir et traiter les messages appareil-à-cloud.

1. Dans une autre fenêtre de terminal, accédez au dossier racine de l’exemple de projet C#. Puis, accédez au dossier **iot-hub\Quickstarts\read-d2c-messages**.

2. Ouvrez le fichier **ReadDeviceToCloudMessages.cs** dans l’éditeur de texte de votre choix. Mettez à jour les variables suivantes et enregistrez vos modifications dans le fichier.

    | Variable | Valeur |
    | -------- | ----------- |
    | `s_eventHubsCompatibleEndpoint` | Remplacez la valeur de la variable par le point de terminaison compatible Event Hubs que vous avez noté précédemment. |
    | `s_eventHubsCompatiblePath`     | Remplacez la valeur de la variable par le chemin d’accès compatible Event Hubs que vous avez noté précédemment. |
    | `s_iotHubSasKey`                | Remplacez la valeur de la variable par la clé principale iothubowner que vous avez notée précédemment. |

3. Dans la fenêtre de terminal, exécutez les commandes suivantes pour installer les bibliothèques requises pour l’application back-end :

    ```cmd/sh
    dotnet restore
    ```

4. Dans la fenêtre de terminal, exécutez les commandes suivantes pour générer et exécuter l’application back-end :

    ```cmd/sh
    dotnet run
    ```

    La capture d’écran suivante présente la sortie lorsque l’application back-end reçoit les données de télémétrie envoyées par l’appareil simulé au concentrateur :

    ![Exécuter l’application back-end](media/quickstart-send-telemetry-dotnet/ReadDeviceToCloud.png)

## <a name="clean-up-resources"></a>Supprimer les ressources

[!INCLUDE [iot-hub-quickstarts-clean-up-resources](../../includes/iot-hub-quickstarts-clean-up-resources.md)]

## <a name="next-steps"></a>Étapes suivantes

Dans ce guide de démarrage rapide, vous avez configuré un IoT Hub, inscrit un appareil, envoyé des données de télémétrie simulée au concentrateur à l’aide d’une application C# et lu les données de télémétrie à partir du concentrateur à l’aide d’une application back-end simple.

Pour savoir comment contrôler votre appareil simulé à partir d’une application back-end, passez au démarrage rapide suivant.

> [!div class="nextstepaction"]
> [Démarrage rapide : contrôler un appareil connecté à un IoT Hub](quickstart-control-device-dotnet.md)