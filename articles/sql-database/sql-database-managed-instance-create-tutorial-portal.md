---
title: 'Portail Azure : Créer une option Azure SQL Database Managed Instance | Microsoft Docs'
description: Créez une Azure SQL Database Managed Instance dans un réseau virtuel.
keywords: didacticiel sql database, créer une option sql database managed instance
services: sql-database
author: bonova
ms.reviewer: carlrab, srbozovi
ms.service: sql-database
ms.custom: managed instance
ms.topic: tutorial
ms.date: 05/09/2018
ms.author: bonova
manager: craigg
ms.openlocfilehash: e337a5c7c203e2e1048149dfeff71436a4d2752f
ms.sourcegitcommit: 3c3488fb16a3c3287c3e1cd11435174711e92126
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/07/2018
ms.locfileid: "34850608"
---
# <a name="create-an-azure-sql-database-managed-instance-in-the-azure-portal"></a>Créer une option Azure SQL Database Managed Instance dans le portail Azure

Ce didacticiel montre comment créer une Azure SQL Database Managed Instance (préversion) à l’aide du portail Azure dans un sous-réseau dédié d’un réseau virtuel puis se connecter à l’instance gérée à l’aide de SQL Server Management Studio sur une machine virtuelle dans le même réseau virtuel.

> [!div class="checklist"]
> * Ajouter votre abonnement à une liste verte
> * Configurer un réseau virtuel
> * Créer une table de routage et un itinéraire
> * Appliquer la table de routage au sous-réseau de l’instance gérée
> * Créer une option Managed Instance
> * Créer un sous-réseau dans le réseau virtuel pour une machine virtuelle
> * Créer une machine virtuelle dans le nouveau sous-réseau du réseau virtuel
> * Connexion à la machine virtuelle
> * Installer SSMS et se connecter à l’option Managed Instance


Si vous n’avez pas d’abonnement Azure, créez un compte [gratuit](https://azure.microsoft.com/free/) avant de commencer.

> [!IMPORTANT]
> Pour obtenir une liste de régions dans lesquelles Managed Instance est actuellement disponible, consultez [Migrate your databases to a fully managed service with Azure SQL Database Managed Instance](https://azure.microsoft.com/blog/migrate-your-databases-to-a-fully-managed-service-with-azure-sql-database-managed-instance/).
 
## <a name="log-in-to-the-azure-portal"></a>Se connecter au portail Azure.

Connectez-vous au [portail Azure](https://portal.azure.com/#create/Microsoft.SQLManagedInstance).

## <a name="whitelist-your-subscription"></a>Ajouter votre abonnement à une liste verte

Dans un premier temps, l’option Managed Instance est sortie en tant que préversion publique contrôlée, ce qui requiert que votre abonnement soit ajouté à une liste verte. Si ce n’est pas déjà le cas, suivez la procédure ci-après pour consulter et accepter les conditions d’utilisation de la préversion et envoyer une demande d’ajout à une liste verte.

1. Cliquez sur **Créer une ressource** en haut à gauche du portail Azure.
2. Recherchez **Managed Instance**, puis sélectionnez **Azure SQL Database Managed Instance (preview)** (Azure SQL Database Managed Instance (préversion)).
3. Cliquez sur **Créer**.

   ![créer l’option managed instance](./media/sql-database-managed-instance-tutorial/managed-instance-create.png)

3. Sélectionnez votre abonnement, cliquez sur **Conditions d’utilisation de la version Preview**, puis fournissez les informations demandées.

   ![conditions d’utilisation de la préversion de l’option managed instance](./media/sql-database-managed-instance-tutorial/preview-terms.png)

5. Lorsque vous avez terminé, cliquez sur **OK**.

   ![conditions d’utilisation de la préversion de l’option managed instance](./media/sql-database-managed-instance-tutorial/preview-approval-pending.png)

   > [!NOTE]
   > En attendant la validation de la préversion, vous pouvez continuer et terminer les sections suivantes de ce didacticiel.

## <a name="configure-a-virtual-network-vnet"></a>Configurer un réseau virtuel

Les étapes suivantes vous montrent comment créer un réseau virtuel [Azure Resource Manager](../azure-resource-manager/resource-manager-deployment-model.md), destiné à être utilisé par votre option Managed Instance. Pour plus d’informations sur la configuration du réseau virtuel, consultez [Configure a VNet for Azure SQL Database Managed Instance](sql-database-managed-instance-vnet-configuration.md) (Configurer un réseau virtuel pour une option Azure SQL Database Managed Instance).

1. Cliquez sur **Créer une ressource** en haut à gauche du portail Azure.
2. Recherchez **Réseau virtuel**, puis cliquez dessus. Vérifiez ensuite que **Resource Manager** est sélectionné en tant que mode de déploiement, puis cliquez sur **Créer**.

   ![créer un réseau virtuel](./media/sql-database-managed-instance-tutorial/virtual-network-create.png)

3. Remplissez le formulaire de réseau virtuel avec les informations demandées, en utilisant les informations du tableau suivant :

   | Paramètre| Valeur suggérée | Description |
   | ------ | --------------- | ----------- |
   |**Name**|Nom valide|Pour connaître les noms valides, consultez [Conventions d’affectation de noms](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).|
   |**Espace d’adressage**|Plage d’adresses valide, telle que 10.14.0.0/24|Nom d’adresse du réseau virtuel en notation CIDR.|
   |**Abonnement**|Votre abonnement|Pour plus d’informations sur vos abonnements, consultez [Abonnements](https://account.windowsazure.com/Subscriptions).|
   |**Groupe de ressources**|Groupe de ressources valide (nouveau ou existant)|Pour connaître les noms valides, consultez [Conventions d’affectation de noms](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).|
   |**Lieu**|Emplacement valide| Pour plus d’informations sur les régions, consultez [Régions Azure](https://azure.microsoft.com/regions/).|
   |**Nom du sous-réseau**|Nom de sous-réseau valide, tel que mi_subnet|Pour connaître les noms valides, consultez [Conventions d’affectation de noms](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).|
   |**Plage d’adresses de sous-réseau**|Adresse de sous-réseau valide, telle que 10.14.0.0/28. Utilisez un espace d’adressage de sous-réseau plus petit que l’espace d’adressage lui-même pour pouvoir créer d’autres sous-réseaux dans le même réseau virtuel, comme un sous-réseau pour l’hébergement des applications de test/client ou des sous-réseaux de passerelle pour se connecter à partir d’un emplacement local ou d’autres réseaux virtuels.|Plage d’adresses du sous-réseau en notation CIDR. Il doit être contenu dans l’espace d’adressage du réseau virtuel|
   |**Points de terminaison de service**|Désactivé|Activez un ou plusieurs points de terminaison de service pour ce sous-réseau|
   ||||

   ![formulaire de création de réseau virtuel](./media/sql-database-managed-instance-tutorial/virtual-network-create-form.png)

4. Cliquez sur **Créer**.

## <a name="create-new-route-table-and-a-route"></a>Créer une table de routage et un itinéraire

La procédure suivante explique comment créer un itinéraire Internet de tronçon suivant 0.0.0.0/0.

1. Cliquez sur **Créer une ressource** en haut à gauche du portail Azure.
2. Recherchez **Table de routage**, puis cliquez dessus. Cliquez ensuite sur **Créer** depuis la page Table de routage. 

   ![créer une table de routage](./media/sql-database-managed-instance-tutorial/route-table-create.png)

3. Remplissez le formulaire de table de routage avec les informations demandées, en utilisant les données du tableau suivant :

   | Paramètre| Valeur suggérée | Description |
   | ------ | --------------- | ----------- |
   |**Name**|Nom valide|Pour connaître les noms valides, consultez [Conventions d’affectation de noms](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).|
   |**Abonnement**|Votre abonnement|Pour plus d’informations sur vos abonnements, consultez [Abonnements](https://account.windowsazure.com/Subscriptions).|
   |**Groupe de ressources**|Sélectionnez le groupe de ressources que vous avez créé lors de la procédure précédente|Pour connaître les noms valides, consultez [Conventions d’affectation de noms](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).|
   |**Lieu**|Sélectionnez l’emplacement que vous avez spécifié lors de la procédure précédente| Pour plus d’informations sur les régions, consultez [Régions Azure](https://azure.microsoft.com/regions/).|
   |**Disable BCP route propagation** (Désactiver la propagation d’itinéraire BCP)|activé||
   ||||

   ![formulaire de création de table de routage](./media/sql-database-managed-instance-tutorial/route-table-create-form.png)

4. Cliquez sur **Créer**.
5. Une fois la table de routage créée, ouvrez-la.

   ![table de routage](./media/sql-database-managed-instance-tutorial/route-table.png)

6. Cliquez sur **Itinéraires**, puis sur **Ajouter**.

   ![ajouter une table de routage](./media/sql-database-managed-instance-tutorial/route-table-add.png)

7.  Ajoutez **itinéraire Internet de tronçon suivant 0.0.0.0/0** comme itinéraire **unique**, en utilisant les informations du tableau suivant :

    | Paramètre| Valeur suggérée | Description |
    | ------ | --------------- | ----------- |
    |**Nom de l’itinéraire**|Nom valide|Pour connaître les noms valides, consultez [Conventions d’affectation de noms](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).|
    |**Préfixe de l’adresse**|0.0.0.0/0|Adresse IP de destination en notation CIDR à laquelle cet itinéraire s’applique.|
    |**Type de tronçon suivant**|Internet|Tronçon suivant qui gère les paquets correspondants pour cet itinéraire|
    |||

    ![itinéraire](./media/sql-database-managed-instance-tutorial/route.png)

8. Cliquez sur **OK**.

## <a name="apply-the-route-table-to-the-managed-instance-subnet"></a>Appliquer la table de routage au sous-réseau de l’instance gérée

Les étapes suivantes montrent comment définir la nouvelle table de routage sur le sous-réseau de l’option Managed Instance.

1. Pour définir la table de routage sur le sous-réseau de l’option Managed Instance, ouvrez le réseau virtuel que vous avez créé précédemment.
2. Cliquez sur **Sous-réseaux**, puis sur le sous-réseau de l’option Managed Instance (**mi_subnet** dans la capture d’écran ci-dessous).

    ![sous-réseau](./media/sql-database-managed-instance-tutorial/subnet.png)

11. Cliquez sur **Table de routage**, puis sélectionnez **myMI_route_table**.

    ![définir une table de routage](./media/sql-database-managed-instance-tutorial/set-route-table.png)

12. Cliquez sur **Enregistrer**.

    ![définir une table de routage - enregistrer](./media/sql-database-managed-instance-tutorial/set-route-table-save.png)

## <a name="create-a-managed-instance"></a>Créer une option Managed Instance

Les étapes suivantes vous montrent comment créer votre option Managed Instance après la validation de votre préversion.

1. Cliquez sur **Créer une ressource** en haut à gauche du portail Azure.
2. Recherchez **Managed Instance**, puis sélectionnez **Azure SQL Database Managed Instance (preview)** (Azure SQL Database Managed Instance (préversion)).
3. Cliquez sur **Créer**.

   ![créer l’option managed instance](./media/sql-database-managed-instance-tutorial/managed-instance-create.png)

4. Sélectionnez votre abonnement et vérifiez que les conditions d’utilisation de la préversion sont définies sur l’état **Accepté**.

   ![conditions d’utilisation de la préversion de l’option managed instance acceptées](./media/sql-database-managed-instance-tutorial/preview-accepted.png)

5. Remplissez le formulaire de l’option Managed Instance avec les informations demandées, en utilisant les données du tableau suivant :

   | Paramètre| Valeur suggérée | Description |
   | ------ | --------------- | ----------- |
   |**Nom de l’instance managée**|Nom valide|Pour connaître les noms valides, consultez [Conventions d’affectation de noms](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).|
   |**Connexion administrateur de l’instance managée**|Nom d’utilisateur non valide|Pour connaître les noms valides, consultez [Conventions d’affectation de noms](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions). N’utilisez pas « serveradmin », car il s’agit d’un rôle réservé au niveau du serveur.| 
   |**Mot de passe**|Mot de passe valide|Le mot de passe doit contenir au moins 16 caractères et satisfaire aux [exigences de complexité définies](../virtual-machines/windows/faq.md#what-are-the-password-requirements-when-creating-a-vm).|
   |**Groupe de ressources**|Le groupe de ressources que vous avez créé précédemment||
   |**Lieu**|L’emplacement que vous avez précédemment sélectionné|Pour plus d’informations sur les régions, consultez [Régions Azure](https://azure.microsoft.com/regions/).|
   |**Réseau virtuel**|Le réseau virtuel que vous avez créé précédemment|

   ![formulaire de création de l’option managed instance](./media/sql-database-managed-instance-tutorial/managed-instance-create-form.png)

6. Cliquez sur **Niveau de tarification** pour dimensionner les ressources de calcul et de stockage, ainsi que pour examiner les options de niveau tarifaire. Par défaut, votre instance bénéficie de 32 Go d’espace de stockage gratuitement, ce qui peut être insuffisant pour vos applications.
7. Utilisez les curseurs ou zones de texte pour spécifier la quantité de stockage et le nombre de v-cores. 
   ![niveau tarifaire d’instance gérée](./media/sql-database-managed-instance-tutorial/managed-instance-pricing-tier.png)

8. Lorsque vous avez terminé, cliquez sur **Appliquer** pour enregistrer votre sélection.  
9. Cliquez sur **Créer** pour déployer l’option Managed Instance.
10. Cliquez sur l’icône **Notifications** pour afficher l’état du déploiement.
 
   ![progression du déploiement](./media/sql-database-managed-instance-tutorial/deployment-progress.png)

11. Cliquez sur **Déploiement en cours** pour ouvrir la fenêtre de l’option Managed Instance et surveiller de façon précise la progression du déploiement.
 
   ![progression du déploiement 2](./media/sql-database-managed-instance-tutorial/managed-instance.png)

Pendant le déploiement, passez à la procédure suivante.

> [!IMPORTANT]
> Pour la première instance dans un sous-réseau, la durée du déploiement est généralement plus importante que celle des instances suivantes. 24 heures peuvent parfois être nécessaires. N’annulez pas l’opération de déploiement car elle dure plus longtemps que prévu. Il s’agit d’une situation temporaire, propre au déploiement de votre première instance. Prévoyez une réduction significative du temps de déploiement peu après le début de la préversion publique.

## <a name="create-a-new-subnet-in-the-vnet-for-a-virtual-machine"></a>Créer un sous-réseau dans le réseau virtuel pour une machine virtuelle

Les étapes suivantes vous montrent comment créer un deuxième sous-réseau dans le réseau virtuel pour une machine virtuelle dans laquelle vous installez SQL Server Management Studio et vous connectez à votre option Managed Instance.

1. Ouvrez votre ressource de réseau virtuelle.
 
   ![Réseau virtuel](./media/sql-database-managed-instance-tutorial/vnet.png)

2. Cliquez sur **Sous-réseaux**, puis cliquez sur **+Sous-réseau**.
 
   ![ajouter un sous-réseau](./media/sql-database-managed-instance-tutorial/add-subnet.png)

3. Remplissez le formulaire de sous-réseau avec les informations demandées, en utilisant les données du tableau suivant :

   | Paramètre| Valeur suggérée | Description |
   | ------ | --------------- | ----------- |
   |**Name**|Nom valide|Pour connaître les noms valides, consultez [Conventions d’affectation de noms](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).|
   |**Plage d’adresses (bloc CIDR)**|Plage d’adresses valide dans le réseau virtuel (utilisez la valeur par défaut)||
   |**Groupe de sécurité réseau**|Aucun||
   |**Table de routage**|Aucun||
   |**Points de terminaison de service**|Aucun||

   ![détails de sous-réseau de machine virtuelle](./media/sql-database-managed-instance-tutorial/vm-subnet-details.png)

4. Cliquez sur **OK**.

## <a name="create-a-virtual-machine-in-the-new-subnet-in-the-vnet"></a>Créer une machine virtuelle dans le nouveau sous-réseau du réseau virtuel

Les étapes suivantes vous montrent comment créer une machine virtuelle dans le même réseau virtuel que celui dans lequel l’option Managed Instance est créée. 

1. Cliquez sur **Créer une ressource** en haut à gauche du portail Azure.
2. Sélectionnez **Compute**, puis **Windows Server 2016 Datacenter** ou **Windows 10**. Cette section du didacticiel utilise Windows Server. La configuration de Windows 10 est en grande partie similaire. 

   ![compute](./media/sql-database-managed-instance-tutorial/compute.png)

3. Remplissez le formulaire de machine virtuelle avec les informations demandées, en utilisant les données du tableau suivant :

   | Paramètre| Valeur suggérée | Description |
   | ------ | --------------- | ----------- |
   |**Name**|Nom valide|Pour connaître les noms valides, consultez [Conventions d’affectation de noms](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).|
   | **Type de disque de machine virtuelle**|SSD|Les disques SSD offrent le meilleur compromis entre prix et performances.|   
   |**Nom d'utilisateur**|Nom d’utilisateur non valide|Pour connaître les noms valides, consultez [Conventions d’affectation de noms](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).| 
   |**Mot de passe**|Mot de passe valide|Le mot de passe doit contenir au moins 12 caractères et satisfaire aux [exigences de complexité définies](../virtual-machines/windows/faq.md#what-are-the-password-requirements-when-creating-a-vm).| 
   |**Abonnement**|Votre abonnement|Pour plus d’informations sur vos abonnements, consultez [Abonnements](https://account.windowsazure.com/Subscriptions).|
   |**Groupe de ressources**|Le groupe de ressources que vous avez créé précédemment||
   |**Lieu**|L’emplacement que vous avez précédemment sélectionné||
   |**Already have a Windows license** (Vous disposez déjà d’une licence Windows)|Non |Si vous avez des licences Windows avec Software Assurance actif, utilisez Azure Hybrid Benefit pour réduire vos coûts de calcul|
   ||||

   ![formulaire de création de machine virtuelle](./media/sql-database-managed-instance-tutorial/virtual-machine-create-form.png)

4. Cliquez sur **OK**.
5. Choisissez la taille de la machine virtuelle. Pour voir plus de tailles, sélectionnez **Afficher tout** ou modifiez le filtre **Type de disque pris en charge**. Pour ce didacticiel, vous avez uniquement besoin d’une petite machine virtuelle.

    ![Tailles de machine virtuelle](./media/sql-database-managed-instance-tutorial/virtual-machine-size.png)  

6. Cliquez sur **Sélectionner**.
7. Sur le formulaire **Paramètres**, cliquez sur **Sous-réseau**, puis sélectionnez **vm_subnet**. Ne choisissez pas le sous-réseau dans lequel l’option Managed Instance est approvisionnée, mais plutôt un autre sous-réseau dans le même réseau virtuel.

    ![Paramètres de machine virtuelle](./media/sql-database-managed-instance-tutorial/virtual-machine-settings.png)  

8. Cliquez sur **OK**.
9. Sur la page de résumé, passez en revue les détails de l’offre, puis cliquez sur **Créer** pour démarrer le déploiement de la machine virtuelle.
 
## <a name="connect-to-virtual-machine"></a>Connexion à la machine virtuelle

Les étapes suivantes vous montrent comment vous connecter à votre machine virtuelle nouvellement créée à l’aide d’une connexion Bureau à distance.

1. Une fois le déploiement terminé, accédez à la ressource de machine virtuelle.

    ![Machine virtuelle](./media/sql-database-managed-instance-tutorial/vm.png)  

2. Cliquez sur le bouton **Connexion** dans les propriétés de la machine virtuelle. Un fichier de protocole Remote Desktop Protocol (fichier .rdp) est créé et téléchargé.
3. Pour vous connecter à votre machine virtuelle, ouvrez le fichier RDP téléchargé. 
4. À l’invite, cliquez sur **Se connecter**. Sur un Mac, vous avez besoin d’un client RDP similaire à ce [Client Bureau à distance](https://itunes.apple.com/us/app/microsoft-remote-desktop/id715768417?mt=12) disponible sur le Mac App Store.

5. Entrez le nom d’utilisateur et le mot de passe spécifiés lors de la création de la machine virtuelle, puis cliquez sur **OK**.

6. Un avertissement de certificat peut s’afficher pendant le processus de connexion. Cliquez sur **Oui** ou **Continuer** pour continuer le processus de connexion.

Vous êtes connecté à votre machine virtuelle dans le tableau de bord du Gestionnaire de serveur.

> [!IMPORTANT]
> Ne poursuivez tant que votre option Managed Instance n’est pas correctement approvisionnée. Une fois configurée, récupérez le nom d’hôte de votre instance dans le champ **Instance gérée** de l’onglet **Vue d’ensemble** de votre option Managed Instance. Le nom est similaire à ce qui suit : **drfadfadsfd.tr23.westus1-a.worker.database.windows.net**.

## <a name="install-ssms-and-connect-to-the-managed-instance"></a>Installer SSMS et se connecter à l’option Managed Instance

Les étapes suivantes vous montrent comment télécharger et installer SSMS, puis comment se connecter à votre option Managed Instance.

1. Dans le Gestionnaire de serveur, cliquez sur **Serveur local** dans le volet gauche.

    ![propriétés du gestionnaire de serveur](./media/sql-database-managed-instance-tutorial/server-manager-properties.png)  

2. Dans le volet **Propriétés**, cliquez sur **On** (Activé) pour modifier la Configuration de sécurité renforcée d’Internet Explorer.
3. Dans la boîte de dialogue **Configuration de sécurité renforcée d’Internet Explorer**, cliquez sur **Désactivé** dans la section Administrateurs de la boîte de dialogue, puis sur **OK**.

    ![configuration de sécurité renforcée d’Internet Explorer](./media/sql-database-managed-instance-tutorial/internet-explorer-security-configuration.png)  
4. Ouvrez **Internet Explorer** à partir de la barre des tâches.
5. Sélectionnez **Utiliser les paramètres de sécurité et de compatibilité recommandés**, puis cliquez sur **OK** pour terminer la configuration d’Internet Explorer 11.
6. Entrez https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms dans la zone d’adresse d’URL et cliquez sur **Entrée**. 
7. Téléchargez la version la plus récente de SQL Server Management Studio et cliquez sur **Exécuter** quand vous y êtes invité.
8. À l’invite, cliquez sur **Installer** pour commencer.
9. Une fois l’installation terminée, cliquez sur **Fermer**.
10. Ouvrez SSMS.
11. Dans la boîte de dialogue **Connexion au serveur**, entrez le **nom d’hôte* de votre option Managed Instance dans la zone **Nom du serveur**, sélectionnez **Authentification SQL Server**, indiquez votre identifiant et mot de passe, puis cliquez sur **Se connecter**.

    ![connecter ssms](./media/sql-database-managed-instance-tutorial/ssms-connect.png)  

Après vous être connecté, vous pouvez voir votre système et vos bases de données utilisateur dans le nœud Bases de données, ainsi que divers objets dans les nœuds Sécurité, Objets serveur, Réplication, Gestion, SQL Server Agent et XEvent Profiler.

> [!NOTE]
> Pour restaurer une base de données SQL existante dans une instance gérée, vous pouvez utiliser [Azure Database Migration Service (DMS) pour la migration](../dms/tutorial-sql-server-to-managed-instance.md) afin de restaurer à partir d’un fichier de sauvegarde de base de données, la [commande T-SQL RESTORE](sql-database-managed-instance-restore-from-backup-tutorial.md) afin de restaurer à partir d’un fichier de sauvegarde de base de données ou [importer à partir d’un fichier BACPAC](sql-database-import.md).

## <a name="next-steps"></a>Étapes suivantes

Dans ce didacticiel, vous avez appris à créer une Azure SQL Database Managed Instance (préversion) à l’aide du portail Azure dans un sous-réseau dédié d’un réseau virtuel puis à se connecter à l’instance gérée à l’aide de SQL Server Management Studio sur une machine virtuelle dans le même réseau virtuel.  Vous avez appris à effectuer les actions suivantes : 

> [!div class="checklist"]
> * Ajouter votre abonnement à une liste verte
> * Configurer un réseau virtuel
> * Créer une table de routage et un itinéraire
> * Appliquer la table de routage au sous-réseau de l’instance gérée
> * Créer une option Managed Instance
> * Créer un sous-réseau dans le réseau virtuel pour une machine virtuelle
> * Créer une machine virtuelle dans le nouveau sous-réseau du réseau virtuel
> * Connexion à la machine virtuelle
> * Installer SSMS et se connecter à l’option Managed Instance

Passez au didacticiel suivant pour apprendre à restaurer une sauvegarde de base de données dans Azure SQL Database Managed Instance.

> [!div class="nextstepaction"]
>[Restaurer une sauvegarde de base de données dans Azure SQL Database Managed Instance](sql-database-managed-instance-restore-from-backup-tutorial.md)
