---
author: Xansky
description: Cet article décrit les codes d’erreur courants pour les opérations du Windows Store pour les applications et modules complémentaires, y compris les achats dans l’application, les licences et installer des mises à jour.
title: Codes d’erreur pour les opérations du Store
ms.author: mhopkins
ms.date: 08/24/2017
ms.topic: article
keywords: Windows 10, uwp, achats dans l’application, FAI, extensions, les codes d’erreur
ms.localizationpriority: medium
ms.openlocfilehash: 1a4eff890da48bd60405cadee2d7ecb92bb1b2fa
ms.sourcegitcommit: ed0304b8a214c03b8aab74b8ef12c9f82b8e3c5f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/19/2018
ms.locfileid: "7296041"
---
# <a name="error-codes-for-store-operations"></a>Codes d’erreur pour les opérations du Store

<!-- confirm whether symbolic names are defined for app developers, or do they just handle direct error code values -->

Cet article décrit les codes d’erreur courants que vous pouvez rencontrer lorsque vous développez ou de tester les opérations associées au Windows Store dans votre application.

## <a name="in-app-purchase-error-codes"></a>Codes d’erreur achat in-app

Codes d’erreur suivants sont liés aux opérations d’achat dans l’application.

|  Error code  |  Description  |
|--------------|---------------|
| 0x803F6100   | L’achat dans l’application n’a pas pu aboutir, car le des enfants monde sont actif. Pour terminer l’achat, se connecter à l’appareil avec votre compte Microsoft et réexécutez l’application.               |
| 0x803F6101   | L’application spécifiée est introuvable. L’application n’est plus peut-être être disponible dans le Windows Store, ou que vous avez fourni le mauvais ID Windows Store pour l’application.     |
| 0x803F6102   | L’extension spécifiée est introuvable. L’extension n’est plus peut-être être disponible dans le Windows Store ou votre peut avoir fourni l’ID Windows Store incorrect pour l’extension.                                               |
| 0x803F6103   | Le produit spécifié est introuvable. Le produit n’est plus peut-être être disponible dans le Windows Store, ou que vous avez fourni le mauvais ID Store du produit.                                          |
| 0x803F6104   | L’achat dans l’application n’a pas pu aboutir, car vous exécutez une version d’évaluation de l’application. Pour effectuer des achats in-app, installez la version complète de l’application.               |
| 0x803F6105   | L’achat dans l’application n’a pas pu aboutir, car vous n’êtes pas connecté avec votre compte Microsoft.                                              |
| 0x803F6107   | Un événement inattendu s’est produit lors du traitement de l’opération en cours.                                             |
| 0x803F6108   | L’achat dans l’application n’a pas pu aboutir, car les informations est manquantes dans la licence de l’application. Cette erreur peut se produire lorsque vous chargement indépendant de votre application. Pour résoudre ce problème, désinstallez l’application, et réinstaller à partir du Windows Store pour actualiser la licence de l’application.                                          |
| 0x803F6109   | L’acquisition de module complémentaire consommable n’a pas pu aboutir, car la quantité spécifiée est plus que le solde restant.        |
| 0x803F610A   | Le type de fournisseur spécifié pour le compte d’utilisateur du Windows Store n’est pas pris en charge.                                            |
| 0x803F610B   | L’opération de magasin spécifiée n’est pas pris en charge.                                             |
| 0x803F610C   | L’application ne gère pas le contrat de tâche en arrière-plan spécifiée.                                             |
| 0x80040001   | La liste fournie de module complémentaire ID n’est pas valide.                        |
| 0x80040002   | La liste de mots clés fournie n’est pas valide.                   |
| 0 x 80040003   | La cible de l’acquisition n’est pas valide.                       |

## <a name="licensing-error-codes"></a>Codes d’erreur de gestion de licences

Codes d’erreur suivants sont liés à la gestion des licences des applications ou des modules complémentaires.

|  Error code  |  Description  |
|--------------|---------------|
| 0x803F700C   | L’appareil est actuellement en mode hors connexion. Pour utiliser cette application pendant que l’appareil est en mode hors connexion, ouvrir les paramètres de Windows Store et définissez le paramètre **Autorisations hors connexion** .            |
| 0x803F8001   | Vous ne disposez pas des droits pour le produit. Vous pouvez utiliser un autre compte Microsoft que celui qui a été utilisé pour acheter le produit.           |
| 0x803F8002   | Votre droit pour le produit a expiré.           |
| 0x803F8003   | Votre droit pour le produit est dans un état non valide qui empêche la création d’une licence.   |
| 0x803F8009<br/>0x803F800A   | La période d’évaluation de l’application a expiré.   |
| 0x803F8190   |  La licence n’autorise pas le produit être utilisé dans les pays en cours ou une région de votre appareil.  |
| 0x803F81F5<br/>0x803F81F6<br/>0x803F81F7<br/>0x803F81F8<br/>0x803F81F9   |  Vous avez atteint le nombre maximal de périphériques qui peuvent être utilisées avec les jeux et applications à partir du Store. Pour utiliser ce jeu ou une application sur l’appareil actuel, commencez par supprimer un autre appareil à partir de votre compte.  |
| 0x803F9000<br/>0x803F9001    |  La licence est arrivé à expiration ou endommagé. Pour vous aider à résoudre ce problème, essayez d’exécuter la [résolution des problèmes pour les applications Windows](https://support.microsoft.com/help/4027498/windows-run-the-troubleshooter-for-windows-apps) pour réinitialiser le cache de Windows Store.     |
| 0x803F9006    |  L’opération a échoué car l’utilisateur qui est autorisé à ce produit n’est pas connecté à l’appareil avec leur compte Microsoft.            |
| 0x803F9008<br/>0x803F9009    |  Votre appareil est en mode hors connexion. Votre appareil doit être en ligne pour utiliser ce produit.            |
| 0x803F900A    |  L’abonnement a expiré.            |


## <a name="self-install-update-error-codes"></a>Installation automatique des codes d’erreur de mise à jour

Codes d’erreur suivants sont liés à [l’installation automatique des mises à jour de package](../packaging/self-install-package-updates.md).

|  Error code  |  Description  |
|--------------|---------------|
| 0x803F6200   | Consentement de l’utilisateur est nécessaire pour télécharger la mise à jour de package.               |
| 0x803F6201   | Consentement de l’utilisateur est nécessaire pour télécharger et installer la mise à jour de package.                                                  |
| 0x803F6203   | Consentement de l’utilisateur est nécessaire pour installer la mise à jour de package.                                         |
| 0x803F6204   | Consentement de l’utilisateur est nécessaire pour télécharger la mise à jour de package, car le téléchargement se produit sur une connexion réseau limitée.                                             |
| 0x803F6206   | Consentement de l’utilisateur est nécessaire pour télécharger et installer la mise à jour de package, car le téléchargement se produit sur une connexion réseau limitée.     |


## <a name="related-topics"></a>Rubriques associées

* [Versions d’évaluation et achats in-app](in-app-purchases-and-trials.md)
* [Obtenir les informations produit des applications et modules complémentaires](get-product-info-for-apps-and-add-ons.md)
* [Obtenir les informations de licence des applications et des modules complémentaires](get-license-info-for-apps-and-add-ons.md)
* [Activer les achats in-app d’applications et de modules complémentaires](enable-in-app-purchases-of-apps-and-add-ons.md)
* [Activer les achats d’extensions consommables](enable-consumable-add-on-purchases.md)
* [Activer les extensions d'abonnement de votre application](enable-subscription-add-ons-for-your-app.md)
* [Implémenter une version d’évaluation de votre application](implement-a-trial-version-of-your-app.md)
