---
author: jnHs
Description: "Lorsque vous soumettez une extension, les options de la page Propriétés vous aident à déterminer le comportement de cette extension lorsqu’elle est proposée aux clients."
title: "Définir les propriétés d’une extension"
ms.assetid: 26D2139F-66FD-479E-940B-7491238ADCAE
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp"
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 186088f249c2e6fe116c970bd1969fcb59863ba6
ms.lasthandoff: 02/07/2017

---

# <a name="enter-add-on-properties"></a>Définir les propriétés d’une extension


Lorsque vous soumettez une extension, les options de la page **Propriétés** vous aident à déterminer le comportement de cette extension lorsqu’elle est proposée aux clients.

## <a name="product-type"></a>Type de produit

Vous sélectionnez le type de produit lors de la [création initiale du module complémentaire](set-your-add-on-product-id.md). Le type de produit que vous avez sélectionné s’affiche ici, mais vous ne pouvez pas le modifier.

> **Remarque**  Si vous n’avez pas publié l’extension, Pour choisir un type de produit différent, vous pouvez supprimer la soumission et recommencer. 

Selon le type de produit que vous avez sélectionné, vous pouvez voir les champs suivants :

### <a name="product-lifetime"></a>Durée de vie du produit
Si vous avez sélectionné un type de produit **Durable**, la **Durée de vie du produit** s’affiche ici. Par défaut, le champ **Durée de vie du produit** d’un module complémentaire durable affiche **Toujours**, ce qui signifie que ce module n’expire jamais. Selon votre gré, vous pouvez définir le champ **Durée de vie du produit** pour que le module complémentaire arrive à expiration au bout d’un laps de temps défini (compris entre 1 et 365 jours). 

### <a name="quantity"></a>Quantité
Si vous avez sélectionné **Consommable géré par le Windows Store** comme type de produit, ce champ affiche la **Quantité**. Vous devez entrer un numéro compris entre 1 et 1000000. Cette quantité est accordée au client au moment d’acquérir le module complémentaire. Ensuite, le Windows Store la rééquilibre en fonction de la consommation du module complémentaire par le client signalée par l’application.

## <a name="content-type"></a>Type de contenu

Quel que soit le type de votre module complémentaire, vous devez également indiquer le type de contenu que vous proposez. Pour la plupart des modules complémentaires, le type de contenu doit être **Téléchargement de logiciels électroniques**. Si une autre option dans la liste semble mieux décrire vos modules complémentaires (par exemple, si vous proposez un téléchargement de musique ou un livre électronique), choisissez-la à la place. 

Voici les options disponibles pour le type de contenu d’un module complémentaire :

-   Téléchargement de logiciels électroniques
-   Livres électroniques
-   Magazine électronique - édition unique
-   Journal électronique - édition unique
-   Téléchargement de musique
-   Musique en continu
-   Services de stockage de données en ligne
-   Téléchargement de vidéos
-   Téléchargement vidéo
-   Logiciel en tant que service

## <a name="keywords"></a>Mots clés

Vous pouvez fournir jusqu’à dix mots clés d’un maximum de 30 caractères chacun pour chaque module complémentaire envoyé. Votre application peut ensuite rechercher les modules complémentaires qui correspondent à ces mots clés. Cette fonctionnalité vous permet de générer dans votre application des écrans qui peuvent charger les modules complémentaires, sans avoir à spécifier directement l’ID du produit dans le code de l’application. Vous pouvez ensuite modifier à tout moment les mots clés d’un module complémentaire sans toucher le code de votre application ni soumettre l’application à nouveau.

> **Remarque**  Les mots clés ne sont pas pris en charge dans les packages ciblant Windows 8 et Windows 8.1.

## <a name="custom-developer-data"></a>Données personnalisées du développeur

Vous pouvez entrer jusqu’à 3 000 caractères dans le champ **Données personnalisées du développeur** (anciennement libellé **Balise**) afin de fournir un contexte supplémentaire pour votre extension. Le plus souvent, ces données prennent la forme d’une chaîne XML, mais vous pouvez entrer ce que vous voulez dans ce champ.

Pour exécuter une requête sur ce champ, utilisez la propriété [StoreSku.CustomDeveloperData](https://msdn.microsoft.com/en-us/library/windows/apps/windows.services.store.storesku.customdeveloperdata.aspx) dans l’[espace de noms Windows.Services.Store](https://msdn.microsoft.com/en-us/library/windows/apps/windows.services.store.aspx). (Si vous utilisez l’[espace de noms Windows.ApplicationModel.Store](https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.store.aspx), utilisez plutôt la propriété [ProductListing.Tag](https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.store.productlisting.tag.aspx).)

Par exemple, supposons que vous proposiez une application de jeu et que vous vendiez un sac de pièces d’or en tant qu’extension. Le champ **Données personnalisées du développeur** permet à l’application de rechercher ce sac d’or. Vous pouvez modifier la valeur de ce champ à tout moment (en l’occurrence, le nombre de pièces figurant dans le sac) en mettant à jour le contenu du champ **Données personnalisées du développeur** du module complémentaire, sans avoir à modifier le code de votre application ni à soumettre l’application à nouveau.

> **Remarque**  Le champ **Données personnalisées du développeur** n’est pas pris en charge dans les packages ciblant Windows 8 et Windows 8.1.

 

 

 





