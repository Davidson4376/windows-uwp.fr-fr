---
author: jnHs
Description: "Quand vous envoyez un module complémentaire, les options de la page Propriétés vous aident à déterminer le comportement de ce module lorsqu’il est proposé aux clients."
title: "Définir les propriétés d’une extension"
ms.assetid: 26D2139F-66FD-479E-940B-7491238ADCAE
ms.author: wdg-dev-content
ms.date: 06/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.openlocfilehash: 253e008d3622094dcfe765531d71e5f37b7777b0
ms.sourcegitcommit: de6bc8acec2cd5ebc36bb21b2ce1a9980c3e78b2
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2017
---
# <a name="enter-add-on-properties"></a>Définir les propriétés d’une extension


Quand vous envoyez un module complémentaire, les options de la page **Propriétés** vous aident à déterminer le comportement de ce module lorsqu’il est proposé aux clients.

## <a name="product-type"></a>Type de produit

Vous sélectionnez le type de produit lors de la [création initiale du module complémentaire](set-your-add-on-product-id.md). Le type de produit que vous avez sélectionné s’affiche ici, mais vous ne pouvez pas le modifier.

> [!TIP]
> Si vous n’avez pas publié l’extension, vous pouvez supprimer la soumission et recommencer si vous souhaitez choisir un autre type de produit.

Les champs visibles sur cette page varient selon le type de produit de votre extension.

## <a name="product-lifetime"></a>Durée de vie du produit


Si vous avez sélectionné un type de produit **Durable**, le champ **Durée de vie du produit** s’affiche ici. Par défaut, le champ **Durée de vie du produit** d’un extension durable affiche la valeur **Toujours**, ce qui signifie que l’extension n’expire jamais. Selon votre gré, vous pouvez définir le champ **Durée de vie du produit** pour que le module complémentaire arrive à expiration au bout d’un laps de temps défini (compris entre1 et 365jours).

## <a name="quantity"></a>Quantité


Si vous avez sélectionné **Consommable géré par le WindowsStore** comme type de produit, le champ **Quantité** s’affiche ici. Vous devez entrer une valeur comprise entre 1 et 1000000. Cette quantité est accordée au client au moment d’acquérir le module complémentaire. Ensuite, le Windows Store la rééquilibre en fonction de la consommation du module complémentaire par le client signalée par l’application.


## <a name="subscription-period"></a>Période d’envoi

Si vous avez sélectionné **Abonnement** comme type de produit, le champ **Période d’envoi** s’affiche ici. Vous devez choisir l’une des options disponibles (**Mensuel**, **3mois**, **6mois**, **Annuel** ou **24mois**) pour indiquer la fréquence à laquelle un client sera facturé pour l’abonnement. Notez qu’une fois votre extension publiée, vous ne pouvez plus modifier la valeur du champ **Période d’envoi**.

> [!NOTE]
> Actuellement, la possibilité de créer des extensions d’abonnement n’est disponible que pour un ensemble de comptes de développeur qui participent à un programme d’adoption anticipée. À l’avenir, nous rendrons les extensions d’abonnement disponibles pour tous les comptes de développeur, mais nous mettons cette documentation préliminaire à disposition dès à présent pour donner aux développeurs un aperçu de cette fonctionnalité. Pour plus d’informations, consultez l’article [Activer les extensions d’abonnement de votre application](../monetize/enable-subscription-add-ons-for-your-app.md).


## <a name="free-trial"></a>Essai gratuit

Pour les extensions d’abonnement, le champ **Essai gratuit** s’affiche également ici. Vous devez choisir si vous souhaitez autoriser les clients à utiliser l’extension gratuitement pendant une période définie (**1semaine** ou **1mois**), ou si vous préférez définir l’option **Pas d’essai gratuit**. Notez qu’une fois votre extension publiée, vous ne pouvez plus modifier la valeur du champ **Essai gratuit**.


## <a name="content-type"></a>Type de contenu

Quel que soit le type de votre extension, vous devez également indiquer le type de contenu que vous proposez. Pour la plupart des extensions, le type de contenu doit être **Téléchargement de logiciels électroniques**. Si une autre option de la liste décrit mieux votre extension (par exemple, si vous proposez un téléchargement de musique ou un livre électronique), sélectionnez-la à la place.

Voici les options disponibles pour le type de contenu d’un module complémentaire:

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

Vous pouvez fournir jusqu’à dix mots clés d’un maximum de 30caractères chacun pour chaque module complémentaire envoyé. Votre application peut ensuite rechercher les modules complémentaires qui correspondent à ces mots clés. Cette fonctionnalité vous permet de générer dans votre application des écrans qui peuvent charger les modules complémentaires, sans avoir à spécifier directement l’ID du produit dans le code de l’application. Vous pouvez ensuite modifier à tout moment les mots clés d’une extension sans modifier le code de votre application ni soumettre de nouveau l’application.

Pour exécuter une requête sur ce champ, utilisez la propriété [StoreProduct.Keywords](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct#Windows_Services_Store_StoreProduct_Keywords) de [l’espace de noms Windows.Services.Store](https://msdn.microsoft.com/en-us/library/windows/apps/windows.services.store.aspx). (Si vous utilisez [l’espace de noms Windows.ApplicationModel.Store](https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.store.aspx), utilisez plutôt la propriété [ProductListing.Keywords](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.productlisting#Windows_ApplicationModel_Store_ProductListing_Keywords).)

> [!NOTE]
> Les mots clés ne sont pas utilisables dans les packages ciblant Windows8 et Windows8.1.

<span id="custom-developer-data" />
### <a name="custom-developer-data"></a>Données personnalisées du développeur

Vous pouvez entrer jusqu’à 3000caractères dans le champ **Données personnalisées du développeur** (anciennement libellé **Balise**) afin de fournir un contexte supplémentaire pour votre extension. Le plus souvent, ces données prennent la forme d’une chaîneXML, mais vous pouvez entrer ce que vous voulez dans ce champ. Votre application peut ensuite exécuter une requête sur ce champ afin d’en lire le contenu (bien que l’application ne puisse pas modifier les données et renvoyer les modifications).

Par exemple, supposons que vous proposiez une application de jeu et que vous vendiez une extension permettant au client d’accéder à des niveaux supplémentaires. À l’aide du champ **Données personnalisées du développeur**, l’application peut exécuter une requête afin de connaître les niveaux disponibles lorsqu’un client dispose de cette extension. Vous pouvez modifier la valeur de ce champ à tout moment (en l’occurrence, les niveaux qui sont inclus) sans avoir à modifier le code de votre application ni à soumettre de nouveau l’application, en mettant à jour le contenu du champ **Données personnalisées du développeur** de l’extension, puis en publiant une soumission mise à jour pour l’extension.

Pour exécuter une requête sur ce champ, utilisez la propriété [StoreSku.CustomDeveloperData](https://msdn.microsoft.com/en-us/library/windows/apps/windows.services.store.storesku.customdeveloperdata.aspx) de [l’espace de noms Windows.Services.Store](https://msdn.microsoft.com/en-us/library/windows/apps/windows.services.store.aspx). (Si vous utilisez [l’espace de noms Windows.ApplicationModel.Store](https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.store.aspx), utilisez plutôt la propriété [ProductListing.Tag](https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.store.productlisting.tag.aspx).)

> [!NOTE]
> Le champ **Données personnalisées du développeur** n’est pas utilisable dans les packages ciblant Windows8 et Windows8.1.

 

 

 
