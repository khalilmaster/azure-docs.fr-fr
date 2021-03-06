---
title: Installer Visual Studio et se connecter à Azure Stack | Microsoft Docs
description: Découvrez les étapes nécessaires pour installer Visual Studio et se connecter à Azure Stack.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/08/2018
ms.author: mabrigg
ms.reviewer: unknown
ms.openlocfilehash: cbd5e5dbcdd2565e8066b0721f45863569bfd90a
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35295028"
---
# <a name="install-visual-studio-and-connect-to-azure-stack"></a>Installer Visual Studio et se connecter à Azure Stack

*S’applique à : systèmes intégrés Azure Stack et Kit de développement Azure Stack*

Vous pouvez utiliser Visual Studio pour écrire et déployer des [modèles](azure-stack-arm-templates.md) Azure Resource Manager dans Azure Stack. Les étapes décrites dans cet article vous guident dans l’installation de Visual Studio sur [Azure Stack](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-remote-desktop), ou sur un ordinateur externe si vous envisagez d’utiliser Azure Stack par le biais du [VPN](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn).

## <a name="install-visual-studio"></a>Installation de Visual Studio

1. Téléchargez et exécutez [Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx).  

2. Ouvrez **Microsoft Web Platform Installer**.

3. Recherchez **Visual Studio Community 2015 avec Kit de développement logiciel (SDK) Microsoft Azure - 2.9.6**. Cliquez sur **Ajouter** et **Installer**.

4. Désinstallez la version de **Microsoft Azure PowerShell** installée dans le cadre du SDK Azure.

    ![Capture d’écran des étapes d’installation de WebPI](./media/azure-stack-install-visual-studio/image1.png) 

5. [Installer PowerShell pour Azure Stack](azure-stack-powershell-install.md)

6. Redémarrez le système d’exploitation après l’installation.

## <a name="connect-to-azure-stack-with-azure-ad"></a>Se connecter à Azure Stack avec Azure AD

1. Lancez Visual Studio.

2. Dans le menu **Affichage**, sélectionnez **Cloud Explorer**.

3. Dans le nouveau volet, sélectionnez **Ajouter un compte** et connectez-vous avec vos informations d’identification Azure Active Directory (Azure AD).  

    ![Capture d’écran de Cloud Explorer une fois connecté et connecté à Azure Stack](./media/azure-stack-install-visual-studio/image2.png)

Une fois connecté, vous pouvez [déployer des modèles](azure-stack-deploy-template-visual-studio.md) ou parcourir les types de ressources et les groupes de ressources disponibles pour créer vos propres modèles.  

## <a name="connect-to-azure-stack-with-ad-fs"></a>Se connecter à Azure Stack avec AD FS

1. Lancez Visual Studio.

2. Dans **Outils**, sélectionnez **Options**.

3. Développez **Environnement** dans le **volet de navigation** et sélectionnez **Comptes**.

4. Sélectionnez **Ajouter**, puis entrez le point de terminaison Azure Resource Manager utilisateur.  
  Pour le kit de développement Azure Stack, l’URL est : `https://management.local.azurestack/external`.  
  Pour les systèmes Azure Stack intégrés, l’URL est : `https://management.[Region}.[External FQDN]`.

    ![X](./media/azure-stack-install-visual-studio/image5.png)

5. Sélectionnez **Ajouter**.  

    Visual Studio appelle Azure Resource Manager et découvre les points de terminaison, notamment le point de terminaison d’authentification pour les services AD FS (Active Directory Federated Services).

    ![Capture d’écran de Cloud Explorer une fois connecté et connecté à Azure Stack](./media/azure-stack-install-visual-studio/image6.png)

6. Dans le menu **Affichage**, sélectionnez **Cloud Explorer**.
7. Sélectionnez **Ajouter un compte** et connectez-vous avec vos informations d’identification AD FS.  

    ![X](./media/azure-stack-install-visual-studio/image7.png)

    Cloud Explorer interroge les abonnements disponibles. Vous pouvez sélectionner un abonnement disponible en vue de le gérer.

    ![X](./media/azure-stack-install-visual-studio/image8.png)

8. Explorez vos ressources, groupes de ressources ou modèles de déploiement existants.

## <a name="next-steps"></a>Étapes suivantes

 - En savoir plus sur la [coexistence](https://msdn.microsoft.com/library/ms246609.aspx) avec d’autres versions de Visual Studio.
 - [Développer des modèles pour Azure Stack](azure-stack-develop-templates.md)