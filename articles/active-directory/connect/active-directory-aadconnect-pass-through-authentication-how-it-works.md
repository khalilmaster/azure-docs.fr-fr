---
title: 'Azure AD Connect : Fonctionnement de l’authentification directe | Microsoft Docs'
description: Cet article décrit le fonctionnement de l’authentification directe Azure Active Directory
services: active-directory
keywords: Authentification directe Azure AD Connect, installation d’Active Directory, composants requis pour Azure AD, SSO, Authentification unique
documentationcenter: ''
author: billmath
manager: mtillman
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/24/2018
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: 4a98b971367c9f83826e85bdc24bbcfe48483f57
ms.sourcegitcommit: a06c4177068aafc8387ddcd54e3071099faf659d
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/09/2018
ms.locfileid: "37916263"
---
# <a name="azure-active-directory-pass-through-authentication-technical-deep-dive"></a>Authentification directe Azure Active Directory : immersion technique
Cet article propose une vue d’ensemble du fonctionnement de l’authentification directe Azure Active directory (Azure AD). Pour en savoir plus sur la sécurité et accéder à d’autres détails techniques, consultez l’article [Présentation approfondie des fonctions de sécurité](active-directory-aadconnect-pass-through-authentication-security-deep-dive.md).

## <a name="how-does-azure-active-directory-pass-through-authentication-work"></a>Comment l’authentification directe Azure Active Directory fonctionne-t-elle ?

Quand un utilisateur tente de se connecter à une application sécurisée par Azure AD et que l’authentification directe est activée sur le client, les étapes suivantes se produisent :

1. L'utilisateur tente d’accéder à une application, par exemple, [Outlook Web App](https://outlook.office365.com/owa/).
2. Si l’utilisateur n’est pas déjà connecté, il est redirigé vers la page **Connexion de l'utilisateur** Azure AD.
3. L’utilisateur entre son nom d’utilisateur dans la page de connexion Azure AD, puis sélectionne le bouton **Suivant**.
4. L’utilisateur entre son mot de passe dans la page de connexion Azure AD, puis sélectionne le bouton **Se connecter**.
5. Lors de la réception de la demande de connexion, Azure AD place le nom d’utilisateur et le mot de passe (chiffré à l’aide de la clé publique des agents d’authentification) dans une file d’attente.
6. Un agent d’authentification local récupère le nom d’utilisateur et le mot de passe chiffré à partir de la file d’attente. Notez que l’agent n’interroge pas fréquemment les requêtes à partir de la file d’attente, mais qu’il les récupère via une connexion permanente établie au préalable.
7. L’agent déchiffre le mot de passe à l’aide de sa clé privée.
8. L’agent valide le nom d’utilisateur et le mot de passe par rapport à votre annuaire Active Directory à l’aide des API Windows standard, un mécanisme similaire à celui utilisé par les services de fédération Active Directory (AD FS). Le nom d’utilisateur peut être soit le nom d’utilisateur sur site par défaut, généralement `userPrincipalName`, soit un autre attribut (appelé `Alternate ID`) configuré dans Azure AD Connect.
9. Le contrôleur de domaine Active Directory sur site évalue la demande et retourne la réponse appropriée (succès, échec, mot de passe expiré ou utilisateur verrouillé) à l’agent.
10. L’agent d’authentification retourne à son tour cette réponse à Azure AD.
11. Azure AD évalue la réponse et répond à l’utilisateur comme il convient. Par exemple, Azure AD connecte immédiatement l'utilisateur ou demande une authentification multifacteur Azure.
12. Si l’utilisateur parvient à se connecter, il peut accéder à l’application.

Le schéma suivant illustre tous les composants et les étapes impliquées dans ce processus :

![Authentification directe](./media/active-directory-aadconnect-pass-through-authentication/pta2.png)

## <a name="next-steps"></a>Étapes suivantes
- [Limitations actuelles](active-directory-aadconnect-pass-through-authentication-current-limitations.md) : découvrez les scénarios pris en charge et ceux qui ne le sont pas.
- [Démarrage rapide](active-directory-aadconnect-pass-through-authentication-quick-start.md) : soyez opérationnel sur l’authentification directe Azure AD.
- [Verrouillage intelligent](../authentication/howto-password-smart-lockout.md) : configurez la fonctionnalité Verrouillage intelligent sur votre locataire pour protéger les comptes d’utilisateur.
- [Forum aux questions](active-directory-aadconnect-pass-through-authentication-faq.md) : trouvez des réponses aux questions fréquemment posées.
- [Résoudre les problèmes](active-directory-aadconnect-troubleshoot-pass-through-authentication.md) : découvrez comment résoudre les problèmes courants liés à la fonctionnalité d’authentification directe.
- [Présentation approfondie de sécurité](active-directory-aadconnect-pass-through-authentication-security-deep-dive.md) : obtenez des informations techniques approfondies sur l’authentification directe.
- [Authentification unique transparente Azure AD](active-directory-aadconnect-sso.md) : explorez en détail cette fonctionnalité complémentaire.
- [UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) : utilisez le Forum Azure Active Directory pour consigner de nouvelles demandes de fonctionnalités.

