---
title: 'HDInsight : Clusters HDInsight joints à un domaine - Azure'
description: Découvrir...
services: hdinsight
author: omidm1
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 7dc6847d-10d4-4b5c-9c83-cc513cf91965
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 06/26/2018
ms.author: omidm
ms.openlocfilehash: 3fd3a4b8982fe2170726df03bdc884e658d0b0c2
ms.sourcegitcommit: 0fa8b4622322b3d3003e760f364992f7f7e5d6a9
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37019486"
---
# <a name="an-introduction-to-hadoop-security-with-domain-joined-hdinsight-clusters"></a>Introduction aux fonctions de sécurité Hadoop sur la base de clusters HDInsight joints à un domaine

Jusqu’à présent, Azure HDInsight ne prenait en charge qu’un seul administrateur local d’utilisateurs. Cela fonctionnait à merveille pour les petites équipes d’application ou les services. Avec le gain en popularité des charges de travail Hadoop dans le secteur de l’entreprise, les besoins en fonctionnalités de niveau entreprise, comme l’authentification basée sur Active Directory, la prise en charge multi-utilisateur et le contrôle d’accès en fonction du rôle sont devenus de plus en plus importants. À l’aide de clusters HDInsight joints à un domaine, vous pouvez créer un cluster HDInsight joint à un domaine Active Directory et configurer une liste des employés de l’entreprise qui peuvent s’authentifier via Azure Active Directory pour se connecter au cluster HDInsight. Toute personne en dehors de l’entreprise ne peut pas se connecter ni accéder au cluster HDInsight. L’administrateur d’entreprise peut configurer le contrôle d’accès en fonction du rôle pour la sécurité Hive à l’aide [d’Apache Ranger](http://hortonworks.com/apache/ranger/), limitant ainsi l’accès aux données de manière appropriée. Enfin, l’administrateur peut auditer l’accès aux données par les employés et les modifications apportées aux stratégies de contrôle d’accès, d’où un degré élevé de gouvernance des ressources de l’entreprise.

> [!NOTE]
> Les nouvelles fonctionnalités décrites dans cet article sont disponibles en préversion uniquement sur les types de cluster suivants : Hadoop, Spark et Interactive Query. Oozie est maintenant activé sur les clusters joints à un domaine. Pour accéder à l’interface utilisateur web d’Oozie, les utilisateurs doivent activer le [tunneling](../hdinsight-linux-ambari-ssh-tunnel.md).

## <a name="benefits"></a>Avantages
La sécurité d’entreprise est constituée de quatre piliers majeurs : sécurité du périmètre, authentification, autorisation et chiffrement.

![Piliers des avantages des clusters HDInsight joints à un domaine](./media/apache-domain-joined-introduction/hdinsight-domain-joined-four-pillars.png).

### <a name="perimeter-security"></a>Sécurité du périmètre
La sécurité du périmètre dans HDInsight est obtenue grâce aux réseaux virtuels et au service de passerelle. Aujourd’hui, un administrateur d’entreprise peut créer un cluster HDInsight à l’intérieur d’un réseau virtuel et utiliser les groupes de sécurité réseau (règles de pare-feu) pour restreindre l’accès au réseau virtuel. Seules les adresses IP définies dans les règles de pare-feu entrantes seront en mesure de communiquer avec le cluster HDInsight, fournissant ainsi la sécurité du périmètre. Le service de passerelle permet d’obtenir un autre niveau de sécurité du périmètre. La passerelle est le service qui agit en tant que première ligne de défense pour toutes les requêtes entrantes dans le cluster HDInsight. Elle accepte la requête, la valide et seulement ensuite autorise la requête à transiter sur les autres nœuds du cluster, offrant ainsi une sécurité du périmètre aux autres nœuds de nom et de données du cluster.

### <a name="authentication"></a>Authentification
Un administrateur d’entreprise peut créer un cluster HDInsight joint à un domaine, dans un [réseau virtuel](https://azure.microsoft.com/services/virtual-network/). Les nœuds du cluster HDInsight seront joints au domaine géré par l’entreprise. Ceci s’effectue grâce aux [Services de domaine Azure Active Directory](../../active-directory-domain-services/active-directory-ds-overview.md). Tous les nœuds du cluster sont joints à un domaine que l’entreprise gère. Avec cette configuration, les employés de l’entreprise peuvent se connecter aux nœuds du cluster à l’aide de leurs informations d’identification de domaine. Ils peuvent également utiliser leurs informations d’identification de domaine pour s’authentifier auprès d’autres points de terminaison approuvés comme les vues Ambari, ODBC, JDBC, PowerShell et les API REST pour interagir avec le cluster. L’administrateur dispose d’un contrôle total sur la limitation du nombre d’utilisateurs qui interagissent avec le cluster via ces points de terminaison.

### <a name="authorization"></a>Authorization
L’une des meilleures pratiques appliquées par la plupart des entreprises est que chaque employé n’a pas accès à toutes les ressources de l’entreprise. De même, avec cette version, l’administrateur peut définir des stratégies de contrôle d’accès en fonction du rôle pour les ressources du cluster. Par exemple, l’administrateur peut configurer [Apache Ranger](http://hortonworks.com/apache/ranger/) pour définir des stratégies de contrôle d’accès pour Hive. Cette fonctionnalité garantit que les employés seront en mesure d’accéder uniquement aux données nécessaires à leur travail. L’accès SSH au cluster est également limité uniquement à l’administrateur.

### <a name="auditing"></a>Audit
En plus de la protection des ressources du cluster HDInsight des utilisateurs non autorisés et de la sécurisation des données, l’audit d’accès aux ressources du cluster et aux données est nécessaire pour effectuer le suivi des accès non autorisés ou non intentionnels aux ressources. L’administrateur peut afficher et signaler tout accès aux données et aux ressources du cluster HDInsight. L’administrateur peut également afficher et signaler toutes les modifications des stratégies de contrôle d’accès effectuées dans les points de terminaison pris en charge par Apache Ranger. Un cluster HDInsight joint à un domaine utilise l’interface utilisateur familière d’Apache Ranger pour rechercher les journaux d’audit. Sur le serveur principal, Ranger utilise [Apache Solr](http://hortonworks.com/apache/solr/) pour le stockage et la recherche des journaux.

### <a name="encryption"></a>Chiffrement
La protection des données est importante pour respecter les exigences de conformité et de sécurité de l’organisation et, en plus de restreindre l’accès aux données contre les employés non autorisés, elle doit également être sécurisée par chiffrement. Les magasins de données pour les clusters HDInsight, le Stockage Blob Azure et Azure Data Lake Storage prennent en charge le [chiffrement des données](../../storage/common/storage-service-encryption.md) transparent côté serveur au repos. Les clusters HDInsight sécurisés fonctionnent sans problème avec cette fonctionnalité de chiffrement des données côté serveur au repos.

## <a name="next-steps"></a>Étapes suivantes
* [Planifier des clusters HDInsight joints à un domaine](apache-domain-joined-architecture.md).
* [Configurer des clusters HDInsight joints à un domaine](apache-domain-joined-configure.md).
* [Gérer des clusters HDInsight joints à un domaine](apache-domain-joined-manage.md).
* [Configurer des stratégies Hive pour des clusters HDInsight joints à un domaine](apache-domain-joined-run-hive.md).
* [Utiliser SSH avec HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md#domainjoined).

