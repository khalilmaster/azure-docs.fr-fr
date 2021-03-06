---
title: Utilisation de profils de version d’API avec Ruby dans Azure Stack | Microsoft Docs
description: En savoir plus sur l’utilisation de profils de version d’API avec Ruby dans Azure Stack.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: B82E4979-FB78-4522-B9A1-84222D4F854B
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2018
ms.author: mabrigg
ms.reviewer: sijuman
ms.openlocfilehash: 4d62c192b4e74980fc8cd8a671a702ba2ddfdbcb
ms.sourcegitcommit: ab3b2482704758ed13cccafcf24345e833ceaff3
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/06/2018
ms.locfileid: "37866591"
---
# <a name="use-api-version-profiles-with-ruby-in-azure-stack"></a>Utiliser des profils de version d’API avec Ruby dans Azure Stack

*S’applique à : systèmes intégrés Azure Stack et Kit de développement Azure Stack*

## <a name="ruby-and-api-version-profiles"></a>Ruby et les profils de version d’API

Le Kit de développement logiciel (SDK) Ruby pour Azure Stack Resource Manager fournit des outils pour vous aider à créer et gérer votre infrastructure. Les fournisseurs de ressources dans le Kit de développement logiciel (SDK) incluent le calcul, les réseaux virtuels et le stockage avec le langage Ruby. Les profils d’API dans le Kit de développement logiciel (SDK) Ruby permettent le développement du cloud hybride en vous offrant la possibilité de basculer entre les ressources Azure globales et les ressources sur Azure Stack.

Un profil d’API est une combinaison de fournisseurs de ressources et de versions de service. Vous pouvez utiliser un profil d’API pour combiner différents types de ressources.

 - Pour utiliser les versions les plus récentes de tous les services, utilisez le profil **Le plus récent** de la gemme cumulative du Kit de développement logiciel (SDK) Azure.
 - Pour utiliser les services compatibles avec Azure Stack, utilisez le profil **V2017_03_09** de la gemme cumulative du Kit de développement logiciel (SDK) Azure.
 - Pour utiliser la dernière version d’API d’un service, utilisez le profil **Le plus récent** de la gemme spécifique. Par exemple, si vous souhaitez utiliser la dernière version d’API d’un service de calcul autonome, utilisez le profil **Le plus récent** de la gemme **Calcul**.
 - Pour utiliser une version d’API spécifique d’un service, utilisez les versions d’API spécifiques définies dans la gemme.

> [!Note]   
> Vous pouvez combiner toutes les options dans la même application.

## <a name="install-the-azure-ruby-sdk"></a>Installer le Kit de développement logiciel (SDK) Azure pour Ruby

 - Suivez les instructions officielles pour installer [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git).
 - Suivez les instructions officielles pour installer [Ruby](https://www.ruby-lang.org/en/documentation/installation/).
    - Lors de l’installation, choisissez **Add Ruby to PATH variable** (Ajouter Ruby à la variable PATH)
    - Installez le kit de développement pendant l’installation de Ruby lorsque vous y êtes invité.
    - Ensuite, installez le programme d’installation à l’aide de la commande suivante :  
      `Gem install bundler`
 - Si vous n’en avez pas de disponible, créez un abonnement et enregistrez l’ID d’abonnement pour une utilisation ultérieure. Les instructions pour créer un abonnement se trouvent [ici](https://docs.microsoft.com/azure/azure-stack/azure-stack-subscribe-plan-provision-vm). 
 - Créez un principal de service et enregistrez son ID et son secret. Les instructions pour créer un principal de service pour Azure Stack se trouvent [ici](https://docs.microsoft.com/azure/azure-stack/azure-stack-create-service-principals). 
 - Vérifiez que votre principal de service a le rôle de contributeur/propriétaire sur votre abonnement. Les instructions pour assigner un rôle au principal de service se trouvent [ici](https://docs.microsoft.com/azure/azure-stack/azure-stack-create-service-principals).

## <a name="install-the-rubygem-packages"></a>Installer les packages rubygem

Vous pouvez installer les packages rubygem Azure directement.

````Ruby  
gem install azure_mgmt_compute
gem install azure_mgmt_storage
gem install azure_mgmt_resources
gem install azure_mgmt_network
Or use them in your Gemfile.
gem 'azure_mgmt_storage'
gem 'azure_mgmt_compute'
gem 'azure_mgmt_resources'
gem 'azure_mgmt_network'
````

N’oubliez pas que le Kit de développement logiciel (SDK) Azure Resource Manager pour Ruby est en version préliminaire et que son interface sera donc probablement très différente pour les prochaines versions. Un nombre plus élevé dans la version mineure peut indiquer ces modifications importantes.

## <a name="usage-of-the-azuresdk-gem"></a>Utilisation de la gemme azure_sdk

La gemme, azure_sdk, est un cumul de toutes les gemmes prises en charge dans le Kit de développement logiciel (SDK) Ruby. Cette gemme se compose du profil **Le plus récent**, qui prend en charge la version la plus récente de tous les services. Il présente un profil **V2017_03_09** avec des versions gérées, qui est conçu pour Azure Stack.

Vous pouvez installer la gemme cumulative azure_sdk avec la commande suivante :  

````Ruby  
  gem install 'azure_sdk
````

## <a name="prerequisite"></a>Configuration requise

Pour pouvoir utiliser le Kit de développement logiciel (SDK) Azure pour Ruby avec Azure Stack, vous devez fournir les valeurs suivantes et définir les valeurs avec des variables d’environnement. Consultez les instructions après le tableau pour définir les variables d’environnement en fonction de votre système d’exploitation. 

| Valeur | Variables d’environnement | Description | 
| --- | --- | --- | --- |
| ID client | AZURE_TENANT_ID | La valeur de votre [ID de locataire](https://docs.microsoft.com/azure/azure-stack/azure-stack-identity-overview) Azure Stack. |
| ID client | AZURE_CLIENT_ID | L’ID d’application du principal du service enregistré lors de la création du principal de service dans la section précédente de ce document.  |
| Identifiant d’abonnement | AZURE_SUBSCRIPTION_ID | [L’ID d’abonnement](https://docs.microsoft.com/azure/azure-stack/azure-stack-plan-offer-quota-overview#subscriptions) correspond à la façon dont vous accédez à des offres dans Azure Stack. |
| Clé secrète client | AZURE_CLIENT_SECRET | Le secret d’application du principal de service enregistré lors de la création du principal de service. |
| Point de terminaison Resource Manager | ARM_ENDPOINT | Consultez la section [Point de terminaison Azure Stack Resource Manager](#The-azure-stack-resource-manager-endpoint).  |

### <a name="the-azure-stack-resource-manager-endpoint"></a>Point de terminaison Azure Stack Resource Manager

Azure Resource Manager est une infrastructure de gestion qui permet aux administrateurs de déployer, gérer et surveiller les ressources Azure. Azure Resource Manager peut gérer ces tâches en groupe, plutôt qu’individuellement, dans une seule opération.

Vous pouvez obtenir les informations de métadonnées du point de terminaison Resource Manager. Le point de terminaison renvoie un fichier JSON avec les informations requises pour exécuter votre code.

  > [!Note]  
  > Le **ResourceManagerUrl** dans le Kit de développement Azure Stack (ASDK) est : `https://management.local.azurestack.external/`  
  > Le **ResourceManagerUrl** dans les systèmes intégrés est : `https://management.<location>.ext-<machine-name>.masd.stbtest.microsoft.com/`  
  > Pour extraire les métadonnées requises : `<ResourceManagerUrl>/metadata/endpoints?api-version=1.0`
  
  Exemple de fichier JSON :

  ```json
  { "galleryEndpoint": "https://portal.local.azurestack.external:30015/",  
    "graphEndpoint": "https://graph.windows.net/",  
    "portal Endpoint": "https://portal.local.azurestack.external/", 
    "authentication": {
      "loginEndpoint": "https://login.windows.net/", 
      "audiences": ["https://management.<yourtenant>.onmicrosoft.com/3cc5febd-e4b7-4a85-a2ed-1d730e2f5928"]
    }
  }
  ```

### <a name="set-environmental-variables"></a>Définir des variables d’environnement

**Microsoft Windows**  
Pour définir les variables d’environnement, dans l’invite de commandes Windows, utilisez le format suivant :  
`set AZURE_TENANT_ID=<YOUR_TENANT_ID>`

**Systèmes Unix, Linux et macOS**  
Dans les systèmes Unix, vous pouvez utiliser la commande suivante :  
`export AZURE_TENANT_ID=<YOUR_TENANT_ID>`

## <a name="existing-api-profiles"></a>Profils d’API existants

La gemme cumulative azure_sdk comprend les deux profils suivants :

1. **V2017_03_09**  
  Profil créé pour Azure Stack. Utilisez ce profil pour que les services soient davantage compatibles avec Azure Stack.
2. **La plus récente**  
  Profil composé des versions les plus récentes de tous les services. Utilisez les dernières versions de tous les services.

Pour plus d’informations sur les profils d’API et Azure Stack, consultez la section [Résumé des profils d’API](azure-stack-version-profiles.md#summary-of-api-profiles).

## <a name="azure-ruby-sdk-api-profile-usage"></a>Utilisation du profil d’API du Kit de développement logiciel (SDK) Azure pour Ruby

Les lignes suivantes doivent être utilisées pour instancier un client de profil. Ce paramètre est uniquement requis pour Azure Stack ou d’autres clouds privés. Azure global a déjà ces paramètres par défaut.

````Ruby  
active_directory_settings = get_active_directory_settings(ENV['ARM_ENDPOINT'])

provider = MsRestAzure::ApplicationTokenProvider.new(
    ENV['AZURE_TENANT_ID'],
    ENV['AZURE_CLIENT_ID'],
    ENV['AZURE_CLIENT_SECRET'],
    active_directory_settings
)
credentials = MsRest::TokenCredentials.new(provider)
options = {
    credentials: credentials,
    subscription_id: subscription_id,
    active_directory_settings: active_directory_settings,
    base_url: ENV['ARM_ENDPOINT']
}

# Target profile built for Azure Stack
client = Azure::Resources::Profiles::V2017_03_09::Mgmt::Client.new(options)
````

Le client de profil peut être utilisé pour accéder aux fournisseurs de ressources individuels, tels que le calcul, le stockage et le réseau.

````Ruby  
# To access the operations associated with Compute
profile_client.compute.virtual_machines.get 'RESOURCE_GROUP_NAME', 'VIRTUAL_MACHINE_NAME'

# Option 1: To access the models associated with Compute
purchase_plan_obj = profile_client.compute.model_classes.purchase_plan.new

# Option 2: To access the models associated with Compute
# Notice Namespace: Azure::Profiles::<Profile Name>::<Service Name>::Mgmt::Models::<Model Name>
purchase_plan_obj = Azure::Profiles::V2017_03_09::Compute::Mgmt::Models::PurchasePlan.new
````

## <a name="define-azurestack-environment-setting-functions"></a>Définir des fonctions de définition d’environnement Azure Stack

Pour authentifier le principal de service à l’environnement Azure Stack, définissez les points de terminaison à l’aide de **get_active_directory_settings()**. Cette méthode utilise la variable d’environnement **ARM_Endpoint** que vous définissez lors de l’établissement de vos variables d’environnement.

````Ruby  
# Get Authentication endpoints using Arm Metadata Endpoints
def get_active_directory_settings(armEndpoint)
    settings = MsRestAzure::ActiveDirectoryServiceSettings.new
    response = Net::HTTP.get_response(URI("#{armEndpoint}/metadata/endpoints?api-version=1.0"))
    status_code = response.code
    response_content = response.body
    unless status_code == "200"
        error_model = JSON.load(response_content)
        fail MsRestAzure::AzureOperationError.new("Getting Azure Stack Metadata Endpoints", response, error_model)
    end
    result = JSON.load(response_content)
    settings.authentication_endpoint = result['authentication']['loginEndpoint'] unless result['authentication']['loginEndpoint'].nil?
    settings.token_audience = result['authentication']['audiences'][0] unless result['authentication']['audiences'][0].nil?
    settings
end
````

## <a name="samples-using-api-profiles"></a>Exemples à l’aide de profils d’API

Vous pouvez utiliser les exemples suivants se trouvant dans les référentiels GitHub en tant que référence de création de solutions avec des profils d’API Ruby et Azure Stack :

 - [Manage Azure resources and resource groups with Ruby (Gérer des ressources et des groupes de ressources Azure avec Ruby)](https://github.com/Azure-Samples/resource-manager-ruby-resources-and-groups/tree/master/Hybrid)
 - [Manage virtual machines using Ruby](https://github.com/Azure-Samples/compute-ruby-manage-vm/tree/master/Hybrid) (Gérer des machines virtuelles à l’aide de Ruby)
 - [Deploy an SSH Enabled VM with a Template in Ruby (Déployer une machine virtuelle compatible SSH à l’aide d’un modèle dans Ruby)](https://github.com/Azure-Samples/resource-manager-ruby-template-deployment/tree/master/Hybrid)

### <a name="sample-resource-manager-and-groups"></a>Exemple Resource Manager et groupes

Pour exécuter l’exemple, assurez-vous que vous avez installé Ruby. Si vous utilisez Visual Studio Code, téléchargez le Kit de développement logiciel (SDK) Ruby en tant qu’extension également. 

> [!Note]  
> Vous pouvez obtenir le référentiel pour l’exemple sur la page « [Manage Azure resources and resource groups with Ruby](https://github.com/Azure-Samples/resource-manager-ruby-resources-and-groups/tree/master/Hybrid) » (Gérer des ressources Azure et des groupes de ressources avec Ruby).

1. Clonez le référentiel.

    ````Bash
    git clone https://github.com/Azure-Samples/resource-manager-ruby-resources-and-groups.git
    ````

2. Installez les dépendances à l’aide du bundle.

    ````Bash
    cd resource-manager-ruby-resources-and-groups\Hybrid\
    bundle install
    ````

3. Créez un principal de service Azure à l’aide de PowerShell et récupérez les valeurs requises. 

  Pour obtenir des instructions sur la création d’un principal de service, consultez [Fournir l’accès des applications à Azure Stack](https://docs.microsoft.com/azure/azure-stack/azure-stack-create-service-principals).

  Les valeurs requises sont :
  - ID client
  - ID client
  - Clé secrète client
  - Identifiant d’abonnement
  - Point de terminaison Resource Manager

  Définissez les variables d’environnement suivantes en utilisant les informations que vous avez récupérées à partir du principal de service que vous avez créé.

  - export AZURE_TENANT_ID={votre id de locataire}
  - export AZURE_CLIENT_ID={votre id de client}
  - export AZURE_CLIENT_SECRET={votre secret de client}
  - export AZURE_SUBSCRIPTION_ID={votre id d’abonnement}
  - export ARM_ENDPOINT={votre url Azure Stack Resource Manager}

  > [!Note]  
  > Sous Windows, utilisez « définition» plutôt que « export »

4. Assurez-vous que la variable d’emplacement est définie sur votre emplacement Azure Stack. Par exemple, LOCAL = « local »

5. Ajoutez la ligne de code suivante si vous utilisez Azure Stack ou d’autres clouds privés pour cibler les points de terminaison Active Directory appropriés.

  ````Ruby  
  active_directory_settings = get_active_directory_settings(ENV['ARM_ENDPOINT'])
  ````

6. À l’intérieur de la variable d’options, ajoutez les paramètres Active Directory et l’URL de base pour utiliser Azure Stack. 

  ````Ruby  
  options = {
    credentials: credentials,
    subscription_id: subscription_id,
    active_directory_settings: active_directory_settings,
    base_url: ENV['ARM_ENDPOINT']
  }
  ````

7. Créez un client de profil qui cible le profil Azure Stack :

  ````Ruby  
    client = Azure::Resources::Profiles::V2017_03_09::Mgmt::Client.new(options)
  ````

8. Pour authentifier le principal de service avec Azure Stack, les points de terminaison doivent être définis à l’aide de **get_active_directory_settings()**. Cette méthode utilise la variable d’environnement **ARM_Endpoint** que vous définissez lors de l’établissement de vos variables d’environnement.

  ````Ruby  
  def get_active_directory_settings(armEndpoint)
    settings = MsRestAzure::ActiveDirectoryServiceSettings.new
    response = Net::HTTP.get_response(URI("#{armEndpoint}/metadata/endpoints?api-version=1.0"))
    status_code = response.code
    response_content = response.body
    unless status_code == "200"
      error_model = JSON.load(response_content)
      fail MsRestAzure::AzureOperationError.new("Getting Azure Stack Metadata Endpoints", response, error_model)
    end
    result = JSON.load(response_content)
    settings.authentication_endpoint = result['authentication']['loginEndpoint'] unless result['authentication']['loginEndpoint'].nil?
    settings.token_audience = result['authentication']['audiences'][0] unless result['authentication']['audiences'][0].nil?
    settings
  end
  ````

9. Exécutez l’exemple.

  ````Ruby
    bundle exec ruby example.rb
  ````

## 

## <a name="next-steps"></a>Étapes suivantes

* [Installer PowerShell pour Azure Stack](azure-stack-powershell-install.md)
* [Configurez l’environnement PowerShell de l’utilisateur Azure Stack.](azure-stack-powershell-configure-user.md)  
