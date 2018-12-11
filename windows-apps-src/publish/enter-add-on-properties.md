---
Description: When submitting an add-on, the options on the Properties page help determine the behavior of your add-on when offered to customers.
title: Saisir les propriétés d’une extension
ms.assetid: 26D2139F-66FD-479E-940B-7491238ADCAE
ms.date: 10/31/2018
ms.topic: article
keywords: Windows10, uwp, extension, propriétés, période d'abonnement, type de produit, durée de vie du produit, type de contenu, iap, achat in-app, produit in-app
ms.localizationpriority: medium
ms.openlocfilehash: 17025282aec18da01f14431996a3942ffdd90312
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/11/2018
ms.locfileid: "8894351"
---
# <a name="enter-add-on-properties"></a>Saisir les propriétés d’une extension

Quand vous envoyez une extension, les options de la page **Propriétés** vous aident à déterminer le comportement de cette extension lorsqu’elle est proposée aux clients.

## <a name="product-type"></a>Type de produit

Vous sélectionnez le type de produit lors de la [création initiale de l'extension](set-your-add-on-product-id.md). Le type de produit que vous avez sélectionné s’affiche ici, mais vous ne pouvez pas le modifier.

> [!TIP]
> Si vous n’avez pas publié l’extension, vous pouvez supprimer la soumission et recommencer si vous souhaitez choisir un autre type de produit.

Les champs visibles sur cette page varient selon le type de produit de votre extension.


## <a name="product-lifetime"></a>Durée de vie du produit

Si vous avez sélectionné un type de produit **Durable**, le champ **Durée de vie du produit** s’affiche ici. Par défaut, le champ **Durée de vie du produit** d’un extension durable affiche la valeur **Toujours**, ce qui signifie que l’extension n’expire jamais. Si vous préférez, vous pouvez modifier la **durée de vie du produit** , afin que le module complémentaire arrive à expiration après un laps de temps défini (compris entre 1 et 365 jours).


## <a name="quantity"></a>Quantité

Si vous avez sélectionné **Consommable géré par le WindowsStore** comme type de produit, le champ **Quantité** s’affiche ici. Vous devez entrer une valeur comprise entre 1 et 1000000. Cette quantité est accordée au client au moment d’acquérir l'extension. Ensuite, le Windows Store la rééquilibre en fonction de la consommation de l'extension par le client signalée par l’application.


## <a name="subscription-period"></a>Période d’envoi

Si vous avez sélectionné **Abonnement** comme type de produit, le champ **Période d’envoi** s’affiche ici. Choisissez une option pour spécifier la fréquence à laquelle un client sera débité pour son abonnement. L’option par défaut est **mensuelle**, mais vous pouvez également sélectionner **3 mois**, **6 mois**, **chaque année**ou **24 mois**.

> [!IMPORTANT]
> Une fois votre extension publiée, vous ne pouvez plus modifier la valeur du champ **Période d’abonnement**.


## <a name="free-trial"></a>Essai gratuit

Si vous avez sélectionné **Abonnement** comme type de produit, le champ **Essai gratuit** s’affiche également ici. La valeur par défaut est **Aucune version d’évaluation gratuite**. Si vous préférez, vous pouvez laisser les clients utiliser l'extension gratuitement pendant une période de temps définie (soit **1semaine** ou **1mois**). 

> [!IMPORTANT]
> Une fois votre extension publiée, vous ne pouvez plus modifier la valeur du champ **Essai gratuit**.


## <a name="content-type"></a>Type de contenu

Quel que soit le type de votre extension, vous devez également indiquer le type de contenu que vous proposez. Pour la plupart des extensions, le type de contenu doit être **Téléchargement de logiciels électroniques**. Si une autre option de la liste décrit mieux votre extension (par exemple, si vous proposez un téléchargement de musique ou un livre électronique), sélectionnez-la à la place.

Voici les options disponibles pour le type de contenu d’une extension:

-   Téléchargement de logiciels électroniques
-   Livres électroniques
-   Magazine électronique - édition unique
-   Journal électronique - édition unique
-   Téléchargement de musique
-   Musique en continu
-   Services de stockage de données en ligne
-   Logiciel en tant que service
-   Téléchargement vidéo
-   Diffusion de vidéos en continu


## <a name="additional-properties"></a>Propriétés supplémentaires

Ces champs sont facultatifs pour tous les types d’extensions.

<span id="keywords" />

### <a name="keywords"></a>Mots clés

Vous pouvez fournir jusqu’à dix mots clés d’un maximum de 30caractères chacun pour chaque extension envoyée. Votre application peut ensuite rechercher les extensions qui correspondent à ces mots clés. Cette fonctionnalité vous permet de générer dans votre application des écrans qui peuvent charger les extensions, sans avoir à spécifier directement l’ID du produit dans le code de l’application. Vous pouvez ensuite modifier à tout moment les mots clés d’une extension sans modifier le code de votre application ni soumettre de nouveau l’application.

Pour exécuter une requête sur ce champ, utilisez la propriété [StoreProduct.Keywords](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct.Keywords) de [l’espace de noms Windows.Services.Store](https://docs.microsoft.com/uwp/api/Windows.Services.Store). (Si vous utilisez [l’espace de noms Windows.ApplicationModel.Store](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store), utilisez plutôt la propriété [ProductListing.Keywords](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.productlisting.Keywords).)

> [!NOTE]
> Mots clés ne sont pas utilisables dans des packages ciblant package Windows8 et Windows8.1.

<span id="custom-developer-data" />

### <a name="custom-developer-data"></a>Données personnalisées du développeur

Vous pouvez entrer jusqu’à 3000caractères dans le champ **Données personnalisées du développeur** (anciennement libellé **Balise**) afin de fournir un contexte supplémentaire pour votre extension. Le plus souvent, ces données prennent la forme d’une chaîneXML, mais vous pouvez entrer ce que vous voulez dans ce champ. Votre application peut ensuite exécuter une requête sur ce champ afin d’en lire le contenu (bien que l’application ne puisse pas modifier les données et renvoyer les modifications).

Par exemple, supposons que vous proposiez une application de jeu et que vous vendiez une extension permettant au client d’accéder à des niveaux supplémentaires. À l’aide du champ **Données personnalisées du développeur**, l’application peut exécuter une requête afin de connaître les niveaux disponibles lorsqu’un client dispose de cette extension. Vous pouvez modifier la valeur de ce champ à tout moment (en l’occurrence, les niveaux qui sont inclus) sans avoir à modifier le code de votre application ni à soumettre de nouveau l’application, en mettant à jour le contenu du champ **Données personnalisées du développeur** de l’extension, puis en publiant une soumission mise à jour pour l’extension.

Pour exécuter une requête sur ce champ, utilisez la propriété [StoreSku.CustomDeveloperData](https://docs.microsoft.com/uwp/api/windows.services.store.storesku.customdeveloperdata#Windows_Services_Store_StoreSku_CustomDeveloperData) de [l’espace de noms Windows.Services.Store](https://docs.microsoft.com/uwp/api/Windows.Services.Store). (Si vous utilisez [l’espace de noms Windows.ApplicationModel.Store](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store), utilisez plutôt la propriété [ProductListing.Tag](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.productlisting.tag#Windows_ApplicationModel_Store_ProductListing_Tag).)

> [!NOTE]
> Le champ de **données personnalisées du développeur** n’est pas disponible pour une utilisation dans les packages ciblant package Windows8 et Windows8.1.

 

 

 
