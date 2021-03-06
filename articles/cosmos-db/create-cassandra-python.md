---
title: 'Démarrage rapide : API Cassandra avec Python - Azure Cosmos DB | Microsoft Docs'
description: Ce guide de démarrage rapide montre comment utiliser l’API Apache Cassandra Azure Cosmos DB pour créer une application de profil avec Python
services: cosmos-db
author: SnehaGunda
manager: kfile
ms.service: cosmos-db
ms.component: cosmosdb-cassandra
ms.custom: quick start connect, mvc
ms.devlang: python
ms.topic: quickstart
ms.date: 11/15/2017
ms.author: sngun
ms.openlocfilehash: 0adabc3561ee989e0ce383a5d995a12c144b19b7
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38237786"
---
# <a name="quickstart-build-a-cassandra-app-with-python-and-azure-cosmos-db"></a>Démarrage rapide : Créer une application web Cassandra avec Python et Azure Cosmos DB

Ce guide de démarrage rapide montre comment utiliser Python et [l’API Cassandra](cassandra-introduction.md) Azure Cosmos DB pour créer une application de profil en clonant un exemple de GitHub. Ce guide de démarrage rapide vous montre également comment créer un compte Azure Cosmos DB en utilisant le portail web Azure.

Azure Cosmos DB est le service de base de données multi-modèle de Microsoft distribué à l’échelle mondiale. Vous pouvez rapidement créer et interroger des bases de données de documents, de tables, de paires clé/valeur et de graphes, lesquelles bénéficient toutes des fonctionnalités de distribution mondiale et de mise à l’échelle horizontale d’Azure Cosmos DB.   

## <a name="prerequisites"></a>Prérequis

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)] Vous pouvez également [essayer Azure Cosmos DB gratuitement](https://azure.microsoft.com/try/cosmosdb/) sans abonnement Azure, ni frais ni engagement.

Accédez au programme d’évaluation de l’API Cassandra Azure Cosmos DB. Si vous n’avez pas encore demandé l’accès, [inscrivez-vous maintenant](cassandra-introduction.md#sign-up-now).

Par ailleurs :
* [Python](https://www.python.org/downloads/) version v2.7.14
* [Git](http://git-scm.com/)
* [Pilote Python pour Apache Cassandra](https://github.com/datastax/python-driver)

## <a name="create-a-database-account"></a>Création d’un compte de base de données

Pour pouvoir créer une base de données de documents, vous devez créer un compte Cassandra avec Azure Cosmos DB.

[!INCLUDE [cosmos-db-create-dbaccount-cassandra](../../includes/cosmos-db-create-dbaccount-cassandra.md)]

## <a name="clone-the-sample-application"></a>Clonage de l’exemple d’application

Nous allons maintenant cloner une application API Cassandra à partir de GitHub, configurer la chaîne de connexion et l’exécuter. Vous pouvez constater à quel point il est facile de travailler par programmation avec des données. 

1. Ouvrez une invite de commandes, créez un nouveau dossier nommé git-samples, puis fermez l’invite de commandes.

    ```bash
    md "C:\git-samples"
    ```

2. Ouvrez une fenêtre de terminal git comme Git Bash et utilisez la commande `cd` pour accéder au nouveau dossier d’installation pour l’exemple d’application.

    ```bash
    cd "C:\git-samples"
    ```

3. Exécutez la commande suivante pour cloner l’exemple de référentiel : Cette commande crée une copie de l’exemple d’application sur votre ordinateur.

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-cassandra-python-getting-started.git
    ```

## <a name="review-the-code"></a>Vérifier le code

Cette étape est facultative. Pour savoir comment les ressources de base de données sont créées dans le code, vous pouvez examiner les extraits de code suivants. Tous les extraits de code sont tirés du fichier pyquickstart.py. Sinon, vous pouvez passer à l’étape [Mise à jour de votre chaîne de connexion](#update-your-connection-string). 

* Le nom d’utilisateur et le mot de passe sont définis à l’aide de la page de chaîne de connexion dans le portail Azure. Remplacez la valeur path\to\cert par le chemin de votre certificat X509.

   ```python
    ssl_opts = {
            'ca_certs': 'path\to\cert',
            'ssl_version': ssl.PROTOCOL_TLSv1_2
            }
    auth_provider = PlainTextAuthProvider( username=cfg.config['username'], password=cfg.config['password'])
    cluster = Cluster([cfg.config['contactPoint']], port = cfg.config['port'], auth_provider=auth_provider, ssl_options=ssl_opts)
    session = cluster.connect()
   
   ```

* Le `cluster` est initialisé avec les informations contactPoint. Le contactPoint est récupéré à partir du portail Azure.

    ```python
   cluster = Cluster([cfg.config['contactPoint']], port = cfg.config['port'], auth_provider=auth_provider)
    ```

* Le `cluster` se connecte à l’API Cassandra Azure Cosmos DB.

    ```python
    session = cluster.connect()
    ```

* Un espace de clés est créé.

    ```python
   session.execute('CREATE KEYSPACE IF NOT EXISTS uprofile WITH replication = {\'class\': \'NetworkTopologyStrategy\', \'datacenter1\' : \'1\' }')
    ```

* Une table est créée.

   ```
   session.execute('CREATE TABLE IF NOT EXISTS uprofile.user (user_id int PRIMARY KEY, user_name text, user_bcity text)');
   ```

* Des entités clé/valeur sont insérées.

    ```Python
    insert_data = session.prepare("INSERT INTO  uprofile.user  (user_id, user_name , user_bcity) VALUES (?,?,?)")
    batch = BatchStatement()
    batch.add(insert_data, (1, 'LyubovK', 'Dubai'))
    batch.add(insert_data, (2, 'JiriK', 'Toronto'))
    batch.add(insert_data, (3, 'IvanH', 'Mumbai'))
    batch.add(insert_data, (4, 'YuliaT', 'Seattle'))
    ....
    session.execute(batch)
    ```

* Requête pour obtenir toutes les valeurs de clé.

    ```Python
    rows = session.execute('SELECT * FROM uprofile.user')
    ```  
    
* Requête pour obtenir une paire clé-valeur.

    ```Python
    
    rows = session.execute('SELECT * FROM uprofile.user where user_id=1')
    ```  

## <a name="update-your-connection-string"></a>Mise à jour de votre chaîne de connexion

Maintenant, retournez dans le portail Azure afin d’obtenir les informations de votre chaîne de connexion et de les copier dans l’application. Cette opération permet à votre application de communiquer avec votre base de données hébergée.

1. Dans le [portail Azure](http://portal.azure.com/), cliquez sur **Chaîne de connexion**. 

    Utilisez le ![bouton Copier](./media/create-cassandra-python/copy.png) à droite de l’écran pour copier la valeur supérieure, c’est-à-dire le POINT DE CONTACT.

    ![Affichez et copiez un nom d’utilisateur, un mot de passe et un point de contact dans le panneau de chaîne de connexion du portail Azure](./media/create-cassandra-python/keys.png)

2. Ouvrez le fichier `config.py` . 

3. Collez la valeur POINT DE CONTACT à partir du portail sur `<FILLME>` à la ligne 10.

    La ligne 10 doit maintenant ressembler à 

    `'contactPoint': 'cosmos-db-quickstarts.cassandra.cosmosdb.azure.com:10350'`

4. Copiez la valeur NOM D’UTILISATEUR à partir du portail et collez-la sur `<FILLME>` à la ligne 6.

    La ligne 6 doit maintenant ressembler à 

    `'username': 'cosmos-db-quickstart',`
    
5. Copiez la valeur MOT DE PASSE à partir du portail et collez-la sur `<FILLME>` à la ligne 8.

    La ligne 8 doit maintenant ressembler à

    `'password' = '2Ggkr662ifxz2Mg==`';`

6. Enregistrez le fichier config.py.
    
## <a name="use-the-x509-certificate"></a>Utiliser le certificat X509

1. Si vous devez ajouter le certificat racine Baltimore CyberTrust, son numéro de série est 02:00:00:b9 et son empreinte digitale SHA1 est d4:de:20:d0:5e:66:fc:53:fe:1a:50:88:2 c: 78:db:28:52:ca:e4:74. Vous pouvez le télécharger à partir de https://cacert.omniroot.com/bc2025.crt et l’enregistrer dans un fichier local avec l'extension .cer

2. Ouvrez pyquickstart.py et changez « path\to\cert » pour pointer vers votre nouveau certificat.

3. Enregistrez pyquickstart.py.

## <a name="run-the-app"></a>Exécution de l'application

1. Utilisez la commande cd dans le terminal git pour accéder au dossier azure-cosmos-db-cassandra-python-getting-started. 

2. Exécutez les commandes suivantes pour installer les modules nécessaires :

    ```python
    python -m pip install cassandra-driver
    python -m pip install prettytable
    python -m pip install requests
    python -m pip install pyopenssl
    ```

2. Exécutez la commande suivante pour démarrer votre application Node :

    ```
    python pyquickstart.py
    ```

3. Vérifiez que les résultats sont corrects à partir de la ligne de commande.

    Appuyez sur CTRL + C pour arrêter l’exécution du programme et fermer la fenêtre de console. 

    ![Consulter et vérifier la sortie](./media/create-cassandra-python/output.png)
    
    Vous pouvez maintenant ouvrir l’Explorateur de données dans le portail Azure pour voir la requête, modifier ces nouvelles données et les utiliser. 

    ![Afficher les données dans l’Explorateur de données](./media/create-cassandra-python/data-explorer.png)

## <a name="review-slas-in-the-azure-portal"></a>Vérification des contrats SLA dans le portail Azure

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Supprimer des ressources

[!INCLUDE [cosmosdb-delete-resource-group](../../includes/cosmos-db-delete-resource-group.md)]

## <a name="next-steps"></a>Étapes suivantes

Dans ce démarrage rapide, vous avez appris à créer un compte Azure Cosmos DB, à créer un conteneur à l’aide de l’Explorateur de données, et à exécuter une application. Vous pouvez maintenant importer des données supplémentaires à votre compte Cosmos DB. 

> [!div class="nextstepaction"]
> [Importer des données Cassandra dans Azure Cosmos DB](cassandra-import-data.md)

