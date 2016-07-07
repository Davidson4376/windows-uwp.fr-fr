---
title: "Restrictions à l’exportation liées à l’utilisation du chiffrement"
description: "Utilisez ces informations pour déterminer si votre application emploie un type de chiffrement qui pourrait l’empêcher de figurer dans le Windows Store."
ms.assetid: 204C7D1D-6F08-4AEE-A333-434D715E7617
author: awkoren
translationtype: Human Translation
ms.sourcegitcommit: b41fc8994412490e37053d454929d2f7cc73b6ac
ms.openlocfilehash: 37d6131891e93d73021c860df45d1b5fdd7cfa53

---

# Restrictions à l’exportation liées à l’utilisation du chiffrement


\[ Article mis à jour pour les applications UWP sur Windows10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

Utilisez ces informations pour déterminer si votre application emploie un type de chiffrement qui pourrait l’empêcher de figurer dans le Windows Store.

Le « Bureau of Industry and Security » (Bureau de l’industrie et de la sécurité ou BIS) du Département du Commerce des États-Unis réglemente l’exportation des technologies qui utilisent certains types de chiffrements. Toutes les applications répertoriées dans le Windows Store doivent respecter ces lois et réglementations, car les fichiers d’application peuvent être stockés aux États-Unis. Même les applications qui sont chargées par les développeurs d’applications depuis d’autres pays en vue d’une diffusion hors des États-Unis doivent respecter ces réglementations. Lors de la soumission d’une application sur le Windows Store, tous les développeurs d’application doivent s’assurer que leurs applications ne contiennent pas de technologie limitée par ces réglementations.

> **Remarque** Les informations fournies ici sont édifiantes, mais il est de votre responsabilité, en tant que développeur publiant des applications dans le Windows Store, de vous assurer que votre application est conforme à l’ensemble des lois et réglementations en vigueur.

 

Pour obtenir plus d’informations sur le Département du Commerce des États-Unis et le « Bureau of Industry and Security » (Bureau de l’industrie et de la sécurité ou BIS), voir [À propos du Bureau de l’industrie et de la sécurité](http://go.microsoft.com/fwlink/p/?LinkID=245644).

Pour plus d’informations sur la réglementation en vigueur aux États-Unis en matière de contrôle des exportations (« Export Administration Regulations » ou EAR) en ce qui concerne les technologies utilisant le chiffrement, voir [Contrôles EAR des éléments utilisant le chiffrement](http://go.microsoft.com/fwlink/p/?LinkID=245645).

## Utilisations régies

Premièrement, déterminez si votre application utilise un type de chiffrement soumis à la réglementation en vigueur aux États-Unis en matière de contrôle des exportations («Export Administration Regulations» ou EAR). La question inclut les exemples affichés dans la liste à cet endroit, mais rappelez-vous que cette liste ne reprend pas toutes les applications possibles du chiffrement.

> **Important** Tenez compte, non seulement du code que vous avez écrit pour votre application, mais également de l’ensemble des bibliothèques logicielles, des utilitaires et des composants du système d’exploitation que votre application contient ou auxquels elle se réfère.

-   Toute utilisation d’une signature numérique, telle que l’authentification ou la vérification de l’intégrité
-   Chiffrement de données ou de fichiers utilisés par votre application
-   Gestion de clés, gestion de certificats ou toute autre utilisation qui interagit avec une infrastructure à clé publique
-   Utilisation d’un canal de communication sécurisé, tel que NTLM, Kerberos, SSL (Secure Sockets Layer) ou TLS (Transport Layer Security)
-   Chiffrement de mots de passe ou autres formes de sécurité des informations
-   Protection contre la copie ou gestion des droits numériques (DRM)
-   Protection antivirus

Pour obtenir la liste complète et actualisée des applications de chiffrement, voir [Contrôles EAR des éléments utilisant le chiffrement](http://go.microsoft.com/fwlink/p/?LinkID=245645).

## Utilisations non restreintes

Notez que certaines applications de chiffrement ne sont pas restreintes. Les tâches non restreintes sont les suivantes :

-   Chiffrement de mot de passe
-   Protection contre la copie
-   Authentification
-   Gestion des droits numériques (DRM)
-   Utilisation de signatures numériques

Pour obtenir la liste complète et actualisée des applications de chiffrement, voir [Contrôles EAR des éléments utilisant le chiffrement](http://go.microsoft.com/fwlink/p/?LinkID=245645).

Si votre application appelle, prend en charge, contient ou utilise un chiffrement pour une tâche quelconque ne figurant pas dans cette liste, elle a besoin d’un numéro ECCN (Export Commodity Classification Number).

Si vous ne possédez pas de numéro ECCN, consultez l’article [Questions et réponses sur les numéros ECCN](http://go.microsoft.com/fwlink/p/?LinkID=245646).



<!--HONumber=Jun16_HO4-->


