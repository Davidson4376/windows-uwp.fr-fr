---
title: Authentification et identité des utilisateurs
description: Les applications UWP disposent de plusieurs options pour l’authentification des utilisateurs. Cela peut aller de l’authentification unique (SSO) à l’aide d’un service Broker d’authentification web à l’authentification à 2facteurs hautement sécurisée.
ms.assetid: 53E36DDC-200A-4850-AAF0-07ECA3662BB9
author: PatrickFarley
ms.author: pafarley
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp, sécurité
ms.localizationpriority: medium
ms.openlocfilehash: 6e41ef25f0d4cce3b36187862936136d84988ad0
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/30/2018
ms.locfileid: "5821293"
---
# <a name="authentication-and-user-identity"></a>Authentification et identité des utilisateurs



Les applications de plateforme Windows universelle (UWP) disposent de plusieurs options pour l’authentification des utilisateurs. Cela peut aller de l’authentification unique (SSO) à l’aide d’un [service Broker d’authentification web](web-authentication-broker.md) à l’authentification à 2 facteurs hautement sécurisée.

Pour les connexions d’applications régulières aux services de fournisseur d’identité tiers, tels que Facebook, Twitter, Flickr et ainsi de suite, utilisez le [service Broker d’authentification web](web-authentication-broker.md). Pour plus de commodité, utilisez la fonctionnalité [Stockage sécurisé des informations d’identification](credential-locker.md) pour enregistrer et rendre itinérantes les informations d’ouverture de session de l’utilisateur.

Les entreprises qui utilisent Windows 10 doivent sérieusement envisager d’utiliser [Microsoft Passport et Windows Hello](microsoft-passport.md), ce qui permet l’authentification à 2 facteurs hautement sécurisée. Si l’utilisation de Microsoft Passport se révèle impossible, l’emploi de [cartes à puce](smart-cards.md) et de l’[empreinte digitale biométrique](fingerprint-biometrics.md) peut contribuer à ajouter une couche de sécurité supplémentaire.

<table>
<tr><th>Rubrique</th><th>Description</th></tr>
<tr><td><a href="credential-locker.md">Stockage sécurisé des informations d’identification</a></td><td>Cet article décrit comment des applications peuvent utiliser le stockage sécurisé des informations d’identification pour stocker et récupérer des informations d’identification utilisateur en toute sécurité et les déplacer entre des appareils avec le compte Microsoft de l’utilisateur.</td></tr>

<tr><td><a href="fingerprint-biometrics.md">Empreinte digitale biométrique</a> </td><td>Cet article explique comment ajouter des empreintes digitales biométrique à votre application. L’inclusion d’une demande d’authentification par empreinte digitale (biométrique) lorsque l’utilisateur doit valider une action particulière renforce la sécurité de votre application. Par exemple, vous pouvez exiger une authentification par empreinte digitale avant d’autoriser un achat in-app ou avant l’accès à des ressources restreintes. L’authentification par empreinte digitale est gérée par l’intermédiaire de la classe <a href="https://msdn.microsoft.com/library/windows/apps/dn279134">UserConsentVerifier</a> dans l’espace de noms <a href="https://msdn.microsoft.com/library/windows/apps/hh701356">Windows.Security.Credentials.UI</a>.</td></tr>
<tr><td><a href="microsoft-passport.md">Microsoft Passport et Windows Hello</a></td><td>Cet article décrit la nouvelle technologie Microsoft Passport de Windows 10 et explique comment les développeurs peuvent implémenter cette technologie pour protéger leurs applications et services principaux. Il présente des fonctionnalités spécifiques de ces technologies qui contribuent à atténuer les menaces liées aux informations d’identification classiques et fournit des recommandations sur la conception et le déploiement de ces technologies dans le cadre de votre lancement de Windows 10. </td></tr>
<tr><td><a href="microsoft-passport-login.md">Créer une application de connexion Microsoft Passport</a></td><td>Première partie de la procédure complète sur la création d’une application UWP Windows 10 qui utilise Microsoft Passport comme alternative aux systèmes d’authentification par nom d’utilisateur et mot de passe traditionnels.</td></tr>
<tr><td><a href="microsoft-passport-login-auth-service.md">Créer un service de connexion Microsoft Passport</a></td><td>Deuxième partie de la procédure complète sur l’utilisation de Microsoft Passport comme alternative aux systèmes d’authentification par nom d’utilisateur et mot de passe traditionnels dans des applications UWP Windows 10.</td></tr>
<tr><td><a href="smart-cards.md">Cartes à puce</a></td><td>Cette rubrique explique comment les applications peuvent utiliser des cartes à puce pour connecter des utilisateurs à des services réseau sécurisés, notamment comment accéder aux lecteurs de carte à puce physiques, créer des cartes à puce virtuelles, communiquer avec des cartes à puce, authentifier des utilisateurs, réinitialiser des PIN d’utilisateur, et supprimer ou déconnecter des cartes à puce.</td></tr>
<tr><td><a href="share-certificates.md">Partager des certificats entre applications</a></td><td>Les applications UWP qui nécessitent une authentification sécurisée en plus de l’identifiant et du mot de passe de l’utilisateur peuvent utiliser des certificats à des fins d’authentification. L’authentification par certificat permet d’authentifier un utilisateur avec un niveau de confiance élevé. Dans certains cas, un groupe de services peut authentifier un utilisateur pour plusieurs applications. Cet article montre comment authentifier plusieurs applications à l’aide du même certificat. Vous apprendrez également à écrire du code pour permettre à un utilisateur d’importer un certificat fourni pour accéder à des services web sécurisés.</td></tr>
<tr><td><a href="companion-device-unlock.md">DéverrouillageWindows avec appareils IoT complémentaires</a></td><td>Un dispositif complémentaire est un appareil pouvant agir en conjonction avec votre ordinateur de bureau Windows10 pour améliorer l’expérience d’authentification utilisateur. S’appuyant sur l’infrastructure CompanionDeviceFramework, un dispositif complémentaire peut enrichir considérablement l’expérience MicrosoftPassport, même en l’absence de WindowsHello (par exemple, si l’ordinateur de bureau Windows10 ne dispose pas d’appareil photo pour l’authentification faciale ou d’un lecteur d’empreintes digitales).</td></tr>
<tr><td><a href="web-account-manager.md">Gestionnaire de comptes web</a></td><td>Cet article explique comment afficher la classe AccountsSettingsPane et connecter votre application de plateforme Windows universelle (UWP) à des fournisseurs d’identité externes, tels que Microsoft ou Facebook, à l’aide des nouvelles API du Gestionnaire de comptes web de Windows10. Vous découvrirez comment demander l’autorisation d’un utilisateur pour utiliser son compte Microsoft, obtenir un jeton d’accès et l’utiliser pour effectuer des opérations de base (par exemple, obtenir des données de profil ou télécharger des fichiers dans OneDrive). </td></tr>
<tr><td><a href="web-authentication-broker.md">Service Broker d’authentification web</a></td><td>Cet article explique comment connecter votre application à un fournisseur d’identité en ligne qui utilise des protocoles d’authentification comme OpenID ou OAuth, par exemple, Facebook, Twitter, Flickr, Instagram, etc. La méthode <a href="https://msdn.microsoft.com/library/windows/apps/br212066">AuthenticateAsync</a> envoie une demande au fournisseur d’identité en ligne, puis obtient un jeton d’accès en retour qui décrit les ressources du fournisseur auxquelles l’application a accès.</td></tr>
</table>

