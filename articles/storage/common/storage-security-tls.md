---
title: Activer le protocole TLS sécurisé pour le client du stockage Azure | Microsoft Docs
description: Découvrez comment activer TLS 1.2 dans le client du stockage Azure.
services: storage
documentationcenter: na
author: fhryo-msft
manager: cbrooks
editor: fhryo-msft
ms.assetid: ''
ms.service: storage
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage
ms.date: 06/25/2018
ms.author: fryu
ms.openlocfilehash: 6c313b6015a8a6dcc4ca5befb5fef70b047d0410
ms.sourcegitcommit: ab3b2482704758ed13cccafcf24345e833ceaff3
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/06/2018
ms.locfileid: "37866523"
---
# <a name="enable-secure-tls-for-azure-storage-client"></a>Activer le protocole TLS sécurisé pour le client du stockage Azure

Quand vous devez auditer vos services par rapport à leur utilisation du stockage Azure selon les dernières exigences en matière de conformité et de sécurité, SSL 1.0, 2.0, 3.0 et TLS 1.0 sont reconnus comme des protocoles de communication non conformes.

SSL 1.0, 2.0 et 3.0 ont été déclarés comme étant vulnérables. Ils ont été interdits par un document RFC. TLS 1.0 est non sécurisé en raison de l’utilisation du chiffrement par blocs (DES CBC et RC2 CBC) et du chiffrement en continu (RC4) non sécurisés. Le conseil des normes de sécurité PCI a également suggéré la migration vers des versions ultérieures de TLS. Pour plus de détails, consultez [TLS (Transport Layer Security)](https://en.wikipedia.org/wiki/Transport_Layer_Security#SSL_1.0.2C_2.0_and_3.0).

Le stockage Azure a arrêté SSL 3.0 depuis 2015 et utilise TLS 1.2 sur les points de terminaison HTTP publics, mais TLS 1.0 et TLS 1.1 sont toujours pris en charge à des fins de compatibilité descendante.

Afin de garantir une connexion sécurisée et conforme au stockage Azure, vous devez activer TLS 1.2 côté client avant d’envoyer des demandes pour activer le service de stockage Azure.

## <a name="enable-tls-12-in-net-client"></a>Activer TLS 1.2 dans le client .NET

Pour que le client puisse négocier TLS 1.2, les versions du système d’exploitation et du .NET Framework doivent toutes deux prendre en charge TLS 1.2. Pour plus de détails, consultez [Prise en charge de TLS 1.2](https://docs.microsoft.com/en-us/dotnet/framework/network-programming/tls#support-for-tls-12).

L’exemple suivant montre comment activer TLS 1.2 dans votre client .NET.

```csharp

    static void EnableTls12()
    {
        // Enable TLS 1.2 before connecting to Azure Storage
        System.Net.ServicePointManager.SecurityProtocol = System.Net.SecurityProtocolType.Tls12;

        // Connect to Azure Storage
        CloudStorageAccount storageAccount = CloudStorageAccount.Parse("DefaultEndpointsProtocol=https;AccountName={yourstorageaccount};AccountKey={yourstorageaccountkey};EndpointSuffix=core.windows.net");
        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

        CloudBlobContainer container = blobClient.GetContainerReference("foo");
        container.CreateIfNotExists();
    }

```

## <a name="enable-tls-12-in-powershell-client"></a>Activer TLS 1.2 dans le client PowerShell

L’exemple suivant montre comment activer TLS 1.2 dans votre client PowerShell.

```powershell

# Enable TLS 1.2 before connecting to Azure Storage
[System.Net.ServicePointManager]::SecurityProtocol = [System.Net.SecurityProtocolType]::Tls12;

$resourceGroup = "{YourResourceGropuName}"
$storageAccountName = "{YourStorageAccountNme}"
$prefix = "foo"

# Connect to Azure Storage
$storageAccount = Get-AzureRmStorageAccount -ResourceGroupName $resourceGroup -Name $storageAccountName
$ctx = $storageAccount.Context
$listOfContainers = Get-AzureStorageContainer -Context $ctx -Prefix $prefix
$listOfContainers

```

## <a name="verify-tls-12-connection"></a>Vérifier la connexion TLS 1.2

Vous pouvez recourir à Fiddler pour vérifier si TLS 1.2 est réellement utilisé. Ouvrez Fiddler pour commencer à capturer le trafic réseau du client, puis exécutez l’exemple ci-dessus. Vous pouvez ensuite trouver la version TLS dans la connexion que l’exemple établit.

La capture d’écran suivante est un exemple de la vérification.

![capture d’écran de vérification de la version TLS dans Fiddler](./media/storage-security-tls/storage-security-tls-verify-in-fiddler.png)

## <a name="see-also"></a>Voir aussi

* [TLS (Transport Layer Security)](https://en.wikipedia.org/wiki/Transport_Layer_Security#SSL_1.0.2C_2.0_and_3.0)
* [Activer TLS dans le client Java](https://www.java.com/en/configure_crypto.html)
