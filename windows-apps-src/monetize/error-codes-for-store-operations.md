---
description: Cet article décrit les codes d’erreur courants pour les opérations du Store portant sur les applications et les extensions, notamment les achats dans l’application, les licences et les mises à jour à installation automatique.
title: Codes d’erreur pour les opérations du Store
ms.date: 08/24/2017
ms.topic: article
keywords: windows 10, uwp, achats dans l’application, IAP, extensions, codes d'erreur
ms.localizationpriority: medium
ms.openlocfilehash: ba505b30076c356a39ae195e1d187cbc49d8a66a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57662874"
---
# <a name="error-codes-for-store-operations"></a>Codes d’erreur pour les opérations du Store

<!-- confirm whether symbolic names are defined for app developers, or do they just handle direct error code values -->

Cet article décrit les codes d’erreur courants que vous pouvez rencontrer lorsque vous développez ou testez des opérations liées au Store dans votre application.

## <a name="in-app-purchase-error-codes"></a>Codes d’erreur des achats in-app

Les codes d’erreur suivants sont liés aux opérations d’achat in-app.

|  Error code  |  Description  |
|--------------|---------------|
| 0x803F6100   | L’achat in-app n’a pas pu aboutir car le Monde des enfants est actif. Pour terminer l’achat, connectez-vous à l’appareil avec votre compte Microsoft et exécutez à nouveau l’application.               |
| 0x803F6101   | L’application spécifiée est introuvable. L’application n'est peut-être plus disponible dans le Store ou vous avez peut-être fourni un ID Store incorrect pour l’application.     |
| 0x803F6102   | L’extension spécifiée est introuvable. L’extension n'est peut-être plus disponible dans le Store ou vous avez peut-être fourni un ID Store incorrect pour l’extension.                                               |
| 0x803F6103   | Le produit spécifié est introuvable. Le produit n'est peut-être plus disponible dans le Store ou vous avez peut-être fourni un ID Store incorrect pour le produit.                                          |
| 0x803F6104   | L’achat in-app n’a pas pu aboutir car vous exécutez une version d’évaluation de l’application. Pour effectuer des achats in-app, installez la version complète de l’application.               |
| 0x803F6105   | L’achat in-app n’a pas pu aboutir car vous n’êtes pas connecté avec votre compte Microsoft.                                              |
| 0x803F6107   | Une erreur inattendue s’est produite lors du traitement de l’opération en cours.                                             |
| 0x803F6108   | L’achat in-app n’a pas pu aboutir car il manque des informations sur la licence de l’application. Cette erreur peut se produire lorsque vous effectuez un chargement indépendant de votre application. Pour résoudre ce problème, désinstallez l’application et réinstallez-la depuis le Store pour actualiser sa licence.                                          |
| 0x803F6109   | L’exécution de l'extension consommable n’a pas pu aboutir car la quantité spécifiée est supérieure au solde restant.        |
| 0x803F610A   | Le type de fournisseur spécifié pour le compte d’utilisateur du Store n’est pas pris en charge.                                            |
| 0x803F610B   | L’opération de Store spécifiée n’est pas prise en charge.                                             |
| 0x803F610C   | L’application ne gère pas le contrat de tâche en arrière-plan spécifié.                                             |
| 0x80040001   | La liste fournie des ID produit de l'extension n’est pas valide.                        |
| 0x80040002   | La liste de mots clés fournie n’est pas valide.                   |
| 0x80040003   | La cible de l’exécution n’est pas valide.                       |

## <a name="licensing-error-codes"></a>Codes d’erreur de licence

Les codes d’erreur suivants sont liés à la gestion des licences des applications ou extensions

|  Error code  |  Description  |
|--------------|---------------|
| 0x803F700C   | L’appareil est actuellement hors ligne. Pour utiliser cette application alors que le périphérique est hors ligne, ouvrez les paramètres du Store et activez/désactivez le paramètre **Autorisations hors ligne**.            |
| 0x803F8001   | Vous n’avez pas de droits sur le produit. Vous pouvez utiliser un compte Microsoft différent de celui qui a été utilisé pour acheter le produit.           |
| 0x803F8002   | Votre abonnement pour le produit a expiré.           |
| 0x803F8003   | Votre droit sur le produit est invalide, ce qui empêche la création d’une licence.   |
| 0x803F8009<br/>0x803F800A   | La période d’évaluation de l’application a expiré.   |
| 0x803F8190   |  La licence ne permet pas d'utiliser le produit dans le pays ou la région où se trouve actuellement votre appareil.  |
| 0x803F81F5<br/>0x803F81F6<br/>0x803F81F7<br/>0x803F81F8<br/>0x803F81F9   |  Vous avez atteint le nombre maximal d’appareils qui peut être utilisé avec des jeux et des applications à partir du Store. Pour utiliser ce jeu ou cette application sur l’appareil actuel, commencez par supprimer un autre appareil de votre compte.  |
| 0x803F9000<br/>0x803F9001    |  La licence a expiré ou est endommagée. Pour résoudre cette erreur, essayez d’exécuter le [résolution des problèmes pour les applications Windows](https://support.microsoft.com/help/4027498/windows-run-the-troubleshooter-for-windows-apps) pour réinitialiser le cache de Store.     |
| 0x803F9006    |  L’opération n’a pas pu aboutir car l’utilisateur autorisé pour ce produit n’est pas connecté à l’appareil avec son compte Microsoft.            |
| 0x803F9008<br/>0x803F9009    |  Votre périphérique est hors ligne. Votre appareil doit être en ligne pour pouvoir utiliser ce produit.            |
| 0x803F900A    |  L’abonnement a expiré.            |


## <a name="self-install-update-error-codes"></a>Codes d’erreur de mise à jour des installations automatiques

Les codes d’erreur suivants sont liés aux [mises à jour des packages à installation automatique](../packaging/self-install-package-updates.md).

|  Error code  |  Description  |
|--------------|---------------|
| 0x803F6200   | L'accord de l’utilisateur est nécessaire pour télécharger la mise à jour du package.               |
| 0x803F6201   | L'accord de l’utilisateur est nécessaire pour télécharger et installer la mise à jour du package.                                                  |
| 0x803F6203   | L'accord de l’utilisateur est nécessaire pour installer la mise à jour du package.                                         |
| 0x803F6204   | L'accord de l’utilisateur est nécessaire pour télécharger la mise à jour du package, car le téléchargement se produira sur une connexion réseau limitée.                                             |
| 0x803F6206   | L'accord de l’utilisateur est nécessaire pour télécharger et installer la mise à jour du package, car le téléchargement se produira sur une connexion réseau limitée.     |


## <a name="related-topics"></a>Rubriques connexes

* [Achats dans l’application et essais](in-app-purchases-and-trials.md)
* [Obtenir des informations sur les produits pour les applications et modules complémentaires](get-product-info-for-apps-and-add-ons.md)
* [Obtenir les informations de licence pour les applications et modules complémentaires](get-license-info-for-apps-and-add-ons.md)
* [Activer les achats dans l’application des applications et des modules complémentaires](enable-in-app-purchases-of-apps-and-add-ons.md)
* [Activer les achats du module complémentaire consommables](enable-consumable-add-on-purchases.md)
* [Activer les modules complémentaires d’abonnement pour votre application](enable-subscription-add-ons-for-your-app.md)
* [Implémenter une version d’évaluation de votre application](implement-a-trial-version-of-your-app.md)
