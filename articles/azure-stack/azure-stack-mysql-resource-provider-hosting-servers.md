---
title: Serveurs d’hébergement MySQL sur Azure Stack | Microsoft Docs
description: Comment ajouter des instances MySQL pour l’approvisionnement via le fournisseur de ressources de l’adaptateur MySQL
services: azure-stack
documentationCenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/02/2018
ms.author: jeffgilb
ms.reviewer: jeffgo
ms.openlocfilehash: bccc2dcad8e326cd29cfe031a95a7c2d0cf5ec7f
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38302310"
---
# <a name="add-hosting-servers-for-the-mysql-resource-provider"></a>Ajouter des serveurs d’hébergement pour le fournisseur de ressources MySQL

Vous pouvez héberger une instance MySQL sur une machine virtuelle dans [Azure Stack](azure-stack-poc.md), ou sur une machine virtuelle à l’extérieur de votre environnement Azure Stack, à condition que le fournisseur de ressources MySQL puisse se connecter à l’instance.

Vous pouvez utiliser les versions de MySQL 5.6, 5.7 et 8.0 pour vos serveurs d’hébergement. Le fournisseur de ressources MySQL ne prend pas en charge l’authentification caching_sha2_password ; celle-ci sera ajoutée dans la prochaine version. Les serveurs MySQL 8.0 doivent être configurés pour utiliser mysql_native_password. MariaDB est également pris en charge.

## <a name="connect-to-a-mysql-hosting-server"></a>Se connecter à un serveur d’hébergement MySQL

Assurez-vous de détenir les informations d’identification d’un compte disposant de privilèges d’administrateur système. Pour ajouter un serveur d’hébergement, procédez comme suit :

1. Connectez-vous au portail d’administration Azure Stack en tant qu’administrateur de service.
2. Sélectionnez **Plus de services**.
3. Sélectionnez **RESSOURCES ADMINISTRATIVES**  >  **Serveurs d’hébergement MySQL**  >  **+Ajouter**. Cette opération ouvre la boîte de dialogue **Ajouter un serveur d’hébergement MySQL**, illustrée dans la capture d’écran suivante.

   ![Configurer un serveur d’hébergement](./media/azure-stack-mysql-rp-deploy/mysql-add-hosting-server-2.png)

4. Spécifiez les détails de connexion de votre instance de MySQL Server.

   * Pour **Nom du serveur d’hébergement MySQL**, fournissez le nom de domaine complet (FQDN) ou une adresse IPv4 valide. N’utilisez pas le nom court de la machine virtuelle.
   * Une instance MySQL par défaut n’est pas fournie, donc vous devez spécifier la **taille du serveur d’hébergement en Go**. Entrez une taille qui est proche de la capacité du serveur de base de données.
   * Conservez la valeur par défaut pour **Abonnement**.
   * Pour **Groupe de ressources**, créez un groupe ou utilisez un groupe existant.

   > [!NOTE]
   > Si l’instance MySQL est accessible par le locataire et l’administrateur Azure Resource Manager, vous pouvez la placer sous le contrôle du fournisseur de ressources. Mais, l’instance MySQL **doit** être allouée exclusivement au fournisseur de ressources.

5. Sélectionnez **Références (SKU)** pour ouvrir la boîte de dialogue **Créer une référence (SKU)**.

   ![Créer une référence (SKU) MySQL](./media/azure-stack-mysql-rp-deploy/mysql-new-sku.png)

   Le **nom** de la référence SKU doit refléter les propriétés de la référence SKU pour que les utilisateurs puissent déployer leurs bases de données sur la référence SKU appropriée.

6. Sélectionnez **OK** pour créer la référence SKU.
> [!NOTE]
> Une heure entière peut être nécessaire avant que les références n’apparaissent dans le portail. Vous ne pouvez pas créer de base de données tant que la référence (SKU) n’a pas été déployée et exécutée.

7. Sous **Ajouter un serveur d’hébergement MySQL**, sélectionnez **Créer**.

À mesure que vous ajoutez des serveurs, vous devez les affecter à une référence (SKU) nouvelle ou existante pour différencier les offres de service. Par exemple, vous pouvez avoir une instance d’entreprise MySQL qui fournit des sauvegardes de base de données et automatiques améliorées. Vous pouvez réserver ce serveur hautes performances pour les différents services de votre organisation.

## <a name="security-considerations-for-mysql"></a>Considérations relatives à la sécurité pour MySQL

Les informations suivantes s’appliquent au fournisseur de ressources et aux serveurs d’hébergement MySQL :

* Vérifiez que tous les serveurs d’hébergement sont configurés pour la communication à l’aide de TLS 1.2. Consultez [Configuring MySQL to Use Encrypted Connections](https://dev.mysql.com/doc/refman/5.7/en/using-encrypted-connections.html) (Configuration de MySQL pour utiliser des connexions chiffrées).
* Utilisez [Transparent Data Encryption](https://dev.mysql.com/doc/mysql-secure-deployment-guide/5.7/en/secure-deployment-data-encryption.html).
* Le fournisseur de ressources MySQL ne prend pas en charge l’authentification caching_sha2_password.

## <a name="increase-backend-database-capacity"></a>Augmenter la capacité de base de données principale

Vous pouvez augmenter la capacité de base de données principale en déployant plus de serveurs MySQL dans le portail Azure Stack. Ajoutez ces serveurs à une référence (SKU) nouvelle ou existante. Si vous ajoutez un serveur à une référence (SKU) existante, assurez-vous que les caractéristiques du serveur sont les mêmes que pour les autres serveurs dans la référence (SKU).

## <a name="make-mysql-database-servers-available-to-your-users"></a>Mise à disposition des serveurs de base de données MySQL pour vos utilisateurs

Créez des plans et des offres pour mettre les serveurs de base de données MySQL à disposition des utilisateurs. Ajoutez le service Microsoft.MySqlAdapter au plan, puis créez un quota. MySQL n’autorise pas la limitation de la taille des bases de données.

## <a name="next-steps"></a>Étapes suivantes

[Création d’une base de données MySQL](azure-stack-mysql-resource-provider-databases.md)