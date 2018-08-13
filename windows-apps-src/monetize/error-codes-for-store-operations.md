---
author: mcleanbyron
description: Cet article décrit les codes d’erreur courants pour les opérations de banque d’informations pour les applications et les modules complémentaires, y compris les achats dans l’application, les licences et installer une application des mises à jour.
title: Codes d’erreur pour les opérations du Store
ms.author: mcleans
ms.date: 08/24/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, achats dans l’application, IAPs, modules complémentaires, les codes d’erreur
ms.localizationpriority: medium
ms.openlocfilehash: 0931397e24eaba44cdf04092f5f367c43a91dd82
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/13/2018
ms.locfileid: "959377"
---
# <a name="error-codes-for-store-operations"></a>Codes d’erreur pour les opérations du Store

<!-- confirm whether symbolic names are defined for app developers, or do they just handle direct error code values -->

Cet article décrit les codes d’erreur courants que vous pouvez rencontrer lors de développement ou de test des opérations liées à la banque dans votre application.

## <a name="in-app-purchase-error-codes"></a>Codes d’erreur dans l’application achat

Codes d’erreur suivants sont liés aux opérations d’achat dans l’application.

|  Error code  |  Description  |
|--------------|---------------|
| 0x803F6100   | L’achat dans l’application n’a pas pu aboutir car le coin des enfants est actif. Pour terminer l’achat, se connecter au périphérique avec votre compte Microsoft et exécutez à nouveau l’application.               |
| 0x803F6101   | L’application spécifiée est introuvable. L’application peut-être ne plus être disponible dans le magasin, ou vous avez fourni le mauvais ID magasin pour l’application.     |
| 0x803F6102   | L’extension spécifiée est introuvable. Le module complémentaire peut-être ne plus être disponible dans le magasin, ou votre peut avoir fourni le mauvais ID magasin pour le module complémentaire.                                               |
| 0x803F6103   | Le produit spécifié est introuvable. Le produit peut-être ne plus être disponible dans le magasin, ou vous avez fourni l’ID de base incorrecte pour le produit.                                          |
| 0x803F6104   | L’achat dans l’application n’a pas pu aboutir car vous exécutez une version d’évaluation de l’application. Pour effectuer des achats dans l’application, installez la version complète de l’application.               |
| 0x803F6105   | L’achat dans l’application n’a pas pu aboutir car vous n’êtes pas connecté avec votre compte Microsoft.                                              |
| 0x803F6107   | Quelque chose d’inattendu s’est produite lors du traitement de l’opération en cours.                                             |
| 0x803F6108   | L’achat dans l’application n’a pas pu aboutir car la licence d’application manque d’informations. Cette erreur peut se produire lorsque vous côté chargement de votre application. Pour résoudre ce problème, désinstallez l’application et réinstallez-le à partir du magasin pour actualiser la licence d’application.                                          |
| 0x803F6109   | L’exécution du module complémentaire consommables n’a pas pu aboutir car la quantité spécifiée est supérieure à l’équilibre restant.        |
| 0x803F610A   | Le type de fournisseur spécifié pour le compte utilisateur n’est pas pris en charge.                                            |
| 0x803F610B   | L’opération de magasin spécifiée n’est pas pris en charge.                                             |
| 0x803F610C   | L’application n’accepte pas le contrat de tâche d’arrière-plan spécifiée.                                             |
| 0x80040001   | La liste fournie de module complémentaire ID n’est pas valide.                        |
| 0x80040002   | La liste des mots clés fournie n’est pas valide.                   |
| 0 x 80040003   | La cible de l’exécution des commandes n’est pas valide.                       |

## <a name="licensing-error-codes"></a>Gestion des licences des codes d’erreur

Codes d’erreur suivants sont liés à la gestion des licences pour les applications ou les modules complémentaires.

|  Error code  |  Description  |
|--------------|---------------|
| 0x803F700C   | Le périphérique est actuellement hors connexion. Pour utiliser cette application alors que le périphérique est en mode hors connexion, ouvrez vos paramètres de Boutique et faire basculer le paramètre **Autorisations hors connexion** .            |
| 0x803F8001   | Vous n’avez pas un droit pour le produit. Vous pouvez utiliser un autre compte Microsoft que celui qui a été utilisé pour acheter le produit.           |
| 0x803F8002   | Votre abonnement pour le produit a expiré.           |
| 0x803F8003   | Vos droits pour le produit est dans un état non valide qui empêche la création d’une licence.   |
| 0x803F8009<br/>0x803F800A   | La période d’évaluation de l’application a expiré.   |
| 0x803F8190   |  La licence n’autorise pas le produit à utiliser dans le pays actuel ou la région de votre appareil.  |
| 0x803F81F5<br/>0x803F81F6<br/>0x803F81F7<br/>0x803F81F8<br/>0x803F81F9   |  Vous avez atteint le nombre maximal de périphériques qui peut être utilisé avec les applications à partir du magasin et de jeux. Pour utiliser ce jeu ou l’application sur le périphérique actuel, supprimez tout d’abord un autre périphérique à partir de votre compte.  |
| 0x803F9000<br/>0x803F9001    |  La licence est arrivé à expiration ou endommagé. Pour résoudre cette erreur, essayez d’exécuter l' [utilitaire de dépannage pour les applications Windows](https://support.microsoft.com/help/4027498/windows-run-the-troubleshooter-for-windows-apps) pour réinitialiser le cache de la banque.     |
| 0x803F9006    |  L’opération n’a pas pu aboutir car l’utilisateur qui est autorisé à ce produit n’est pas signé avec l’appareil avec leur compte Microsoft.            |
| 0x803F9008<br/>0x803F9009    |  Le périphérique est en mode hors connexion. Le périphérique doit être en ligne pour utiliser ce produit.            |
| 0x803F900A    |  L’abonnement a expiré.            |


## <a name="self-install-update-error-codes"></a>Installer la mise à jour des codes d’erreur

Codes d’erreur suivants sont liés à [l’installation automatique des mises à jour de package](../packaging/self-install-package-updates.md).

|  Error code  |  Description  |
|--------------|---------------|
| 0x803F6200   | Consentement de l’utilisateur est requis pour télécharger la mise à jour de package.               |
| 0x803F6201   | Consentement de l’utilisateur est requis pour télécharger et installer la mise à jour de package.                                                  |
| 0x803F6203   | Consentement de l’utilisateur est requise pour installer la mise à jour de package.                                         |
| 0x803F6204   | Consentement de l’utilisateur est requis pour télécharger la mise à jour de package, car le téléchargement se produit sur une connexion réseau contrôlé.                                             |
| 0x803F6206   | Consentement de l’utilisateur est requis pour télécharger et installer la mise à jour de package, car le téléchargement se produit sur une connexion réseau contrôlé.     |


## <a name="related-topics"></a>Rubriques associées

* [Versions d’évaluation et achats in-app](in-app-purchases-and-trials.md)
* [Obtenir les informations produit des applications et modules complémentaires](get-product-info-for-apps-and-add-ons.md)
* [Obtenir les informations de licence des applications et des modules complémentaires](get-license-info-for-apps-and-add-ons.md)
* [Activer les achats in-app d’applications et de modules complémentaires](enable-in-app-purchases-of-apps-and-add-ons.md)
* [Activer les achats d’extensions consommables](enable-consumable-add-on-purchases.md)
* [Activer les extensions d'abonnement de votre application](enable-subscription-add-ons-for-your-app.md)
* [Implémenter une version d’évaluation de votre application](implement-a-trial-version-of-your-app.md)
