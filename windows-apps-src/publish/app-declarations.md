---
author: jnHs
Description: Product declarations help make sure your app is displayed appropriately in the Microsoft Store and offered to the right set of customers.
title: Déclarations de produit
ms.assetid: 3AF618F3-2B47-4A57-B7E8-1DF979D4A82C
ms.author: wdg-dev-content
ms.date: 12/05/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 959e056d5edf5e1fe7a1c51a2f855c9e11512cb0
ms.sourcegitcommit: c6d6f8b54253e79354f8db14e5cf3b113a3e5014
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/24/2018
ms.locfileid: "2839656"
---
# <a name="product-declarations"></a>Déclarations de produit

La section des **déclarations de produit** de la page [Propriétés](enter-app-properties.md) du [processus de soumission](app-submissions.md) permet d’assurer votre application est affichée correctement et proposée aux ensembles de clients et vous aide à les comprennent comment ils peuvent utiliser votre application.

Les sections suivantes décrivent certaines des déclarations et ce que vous devez prendre en compte lors de la détermination si chaque déclaration s’applique à votre application. Notez que deux de ces déclarations sont activées par défaut (tel que décrit ci-dessous). En fonction de la catégorie de votre produit, vous pouvez également voir des déclarations supplémentaires. Veillez à consulter toutes les déclarations et vous assurer qu’ils reflètent précisément votre présentation.

## <a name="this-app-allows-users-to-make-purchases-but-does-not-use-the-microsoft-store-commerce-system"></a>Cette application permet aux utilisateurs d’effectuer des achats, mais n’utilise pas le système de commerce Microsoft Store.

Pour presque chaque envoi, vous devez laisser cette case décochée, depuis des applications qui offrent la possibilité d’acheter les éléments qui sont ou peuvent être consommées ou utilisées au sein de votre application doivent utiliser l’achat de dans l’application Microsoft Store API pour créer et soumettre les modules complémentaires. Par le [Contrat de développeur d’application](https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement), les applications qui ont été créées et envoyées avant le 29 juin 2015, peuvent continuer à offrir des fonctionnalités des achats dans l’application sans utiliser le moteur de commerce de Microsoft, tant que la fonctionnalité de fournisseur est conforme à la [ Stratégies de banque de Microsoft](https://docs.microsoft.com/legal/windows/agreements/store-policies#108-financial-transactions). Si cela s’applique à votre application, vous devez activer cette case. Sinon, laissez-la désactivée.

## <a name="this-app-has-been-tested-to-meet-accessibility-guidelines"></a>Cette application a fait l'objet de tests pour voir si elle est conforme aux recommandations d'accessibilité.

En cochant cette case, vous rendez votre application détectable par les clients qui recherchent tout particulièrement des applications accessibles dans le Windows Store.

Cochez uniquement cette case si vous avez :

-   défini toutes les informations d'accessibilité relatives aux éléments de l'interface utilisateur, comme les noms accessibles ;
-   implémenté la navigation et l'utilisation à partir du clavier en tenant compte de l'ordre des onglets, de l'activation du clavier, de la navigation à l'aide des touches de direction et des raccourcis ;
-   vérifié la présence d'une expérience visuelle accessible respectant notamment un coefficient de contraste de texte de 4.5:1 et n'utilisant pas simplement la couleur pour transmettre des informations à l'utilisateur ;
-   utilisé des outils de test d'accessibilité, comme Inspect ou AccChecker, afin de vérifier votre application et de résoudre les erreurs de priorité élevée détectées par ces outils ;
-   vérifié les scénarios clés de votre application de bout en bout à l'aide de fonctionnalités et d'outils tels que le Narrateur, la Loupe, le clavier sur écran, le contraste élevé et la haute résolution.

Lorsque vous déclarez votre application comme étant accessible, vous certifiez qu'elle est accessible à tous les utilisateurs, y compris ceux souffrant de handicaps. Cela signifie, par exemple, que vous avez testé l'application avec le mode de contraste élevé et avec un lecteur d'écran. Vous avez également vérifié que l’interface utilisateur fonctionnait correctement avec un clavier, la Loupe et d’autres outils d’accessibilité.

Pour plus d’informations, voir [accessibilité](../design/accessibility/accessibility.md), le [test d’accessibilité](../design/accessibility/accessibility-testing.md)et [accessibilité dans le magasin](../design/accessibility/accessibility-in-the-store.md).

> [!IMPORTANT]
> Ne liste votre application en tant qu’accessibles, sauf si vous avez spécifiquement conçu et testé à cet effet. Si votre application est déclarée comme étant accessible, mais qu’elle ne prend pas réellement en charge l’accessibilité, vous allez probablement recevoir des commentaires négatifs de la part de la communauté.

## <a name="customers-can-install-this-app-to-alternate-drives-or-removable-storage"></a>Les clients peuvent installer cette application sur d'autres disques ou dispositifs de stockage amovible.

Cette case est activée par défaut, pour permettre aux clients d’installer votre application vers un stockage amovible ou externe comme un lecteur externe du lecteur multimédia comme une carte mémoire SD ou sur un volume non-système. (Pour Windows Phone 8.1, il a été indiqué précédemment via StoreManifest.xml.)

Si vous souhaitez empêcher que votre application en cours d’installation pour les lecteurs de substitution ou de stockage amovible et autoriser uniquement l’installation sur le disque dur interne sur leur appareil, désactivez cette case à cocher.

Notez qu’il n’existe aucune option pour restreindre l’installation afin qu’une application peut *uniquement* être installé sur un support amovible.


## <a name="windows-can-include-this-apps-data-in-automatic-backups-to-onedrive"></a>Windows peut intégrer les données de cette application dans les sauvegardes automatiques sur OneDrive.

Cette case est cochée par défaut pour permettre l'insertion des données de votre application quand un client choisit de paramétrer Windows pour des sauvegardes automatiques sur OneDrive. (Pour Windows Phone 8.1, il a été indiqué précédemment via StoreManifest.xml.)

Si vous voulez empêcher l’insertion des données de votre application dans les sauvegardes automatiques, décochez cette case.


## <a name="this-app-sends-kinect-data-to-external-services"></a>Cette application envoie des données de Kinect aux services externes. 

Si votre application utilise des données Kinect et l’envoie à un service externe, vous devez vérifier cette zone.



 

 

 




