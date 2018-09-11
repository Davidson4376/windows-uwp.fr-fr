---
title: Restrictions à l’exportation liées à l’utilisation du chiffrement
description: Utilisez ces informations pour déterminer si votre application emploie un type de chiffrement qui pourrait l’empêcher de figurer dans le Microsoft Store.
ms.assetid: 204C7D1D-6F08-4AEE-A333-434D715E7617
author: msatranjr
ms.author: misatran
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, sécurité
ms.localizationpriority: medium
ms.openlocfilehash: 842d26a2bb257dd182813832c5e6480237a9f220
ms.sourcegitcommit: 72710baeee8c898b5ab77ceb66d884eaa9db4cb8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/11/2018
ms.locfileid: "3844993"
---
# <a name="export-restrictions-on-cryptography"></a>Restrictions à l’exportation liées à l’utilisation du chiffrement



Utilisez ces informations pour déterminer si votre application emploie un type de chiffrement qui pourrait l’empêcher de figurer dans le Microsoft Store.

Le « Bureau of Industry and Security » (Bureau de l’industrie et de la sécurité ou BIS) du Département du Commerce des États-Unis réglemente l’exportation des technologies qui utilisent certains types de chiffrements. Toutes les applications répertoriées dans le Microsoft Store doivent respecter ces lois et réglementations, car les fichiers d’application peuvent être stockés aux États-Unis. Même les applications qui sont chargées par les développeurs d’applications depuis d’autres pays en vue d’une diffusion hors des États-Unis doivent respecter ces réglementations. Lors de la soumission d’une application sur le Microsoft Store, tous les développeurs d’application doivent s’assurer que leurs applications ne contiennent pas de technologie limitée par ces réglementations.

> **Remarque**  Les informations fournies ici sont édifiantes, mais il est de votre responsabilité, en tant que développeur publiant des applications dans le MicrosoftStore, de vous assurer que votre application est conforme à l’ensemble des lois et réglementations en vigueur.

 

Pour obtenir plus d’informations sur le Département du Commerce des États-Unis et le « Bureau of Industry and Security » (Bureau de l’industrie et de la sécurité ou BIS), voir [À propos du Bureau de l’industrie et de la sécurité](http://go.microsoft.com/fwlink/p/?LinkID=245644).

Pour plus d’informations sur la réglementation en vigueur aux États-Unis en matière de contrôle des exportations (« Export Administration Regulations » ou EAR) en ce qui concerne les technologies utilisant le chiffrement, voir [Contrôles EAR des éléments utilisant le chiffrement](http://go.microsoft.com/fwlink/p/?LinkID=245645).

## <a name="governed-uses"></a>Utilisations régies

Premièrement, déterminez si votre application utilise un type de chiffrement soumis à la réglementation en vigueur aux États-Unis en matière de contrôle des exportations («Export Administration Regulations» ou EAR). La question inclut les exemples affichés dans la liste à cet endroit, mais rappelez-vous que cette liste ne reprend pas toutes les applications possibles du chiffrement.

> **Important**  Tenez compte non seulement du code que vous avez écrit pour votre application, mais également de l’ensemble des bibliothèques logicielles, des utilitaires et des composants du système d’exploitation que votre application contient ou auxquels elle se réfère.

-   Toute utilisation d’une signature numérique, telle que l’authentification ou la vérification de l’intégrité
-   Chiffrement de données ou de fichiers utilisés par votre application
-   Gestion de clés, gestion de certificats ou toute autre utilisation qui interagit avec une infrastructure à clé publique
-   Utilisation d’un canal de communication sécurisé, tel que NTLM, Kerberos, SSL (Secure Sockets Layer) ou TLS (Transport Layer Security)
-   Chiffrement de mots de passe ou autres formes de sécurité des informations
-   Protection contre la copie ou gestion des droits numériques (DRM)
-   Protection antivirus

Pour obtenir la liste complète et actualisée des applications de chiffrement, voir [Contrôles EAR des éléments utilisant le chiffrement](http://go.microsoft.com/fwlink/p/?LinkID=245645).

## <a name="non-restricted-uses"></a>Utilisations non restreintes

Notez que certaines applications de chiffrement ne sont pas restreintes. Les tâches non restreintes sont les suivantes :

-   Chiffrement de mot de passe
-   Protection contre la copie
-   Authentification
-   Gestion des droits numériques (DRM)
-   Utilisation de signatures numériques

Pour obtenir la liste complète et actualisée des applications de chiffrement, voir [Contrôles EAR des éléments utilisant le chiffrement](http://go.microsoft.com/fwlink/p/?LinkID=245645).

Si votre application appelle, prend en charge, contient ou utilise un chiffrement pour une tâche quelconque ne figurant pas dans cette liste, elle a besoin d’un numéro ECCN (Export Commodity Classification Number).

Si vous ne possédez pas de numéro ECCN, consultez l’article [Questions et réponses sur les numéros ECCN](http://go.microsoft.com/fwlink/p/?LinkID=245646).
