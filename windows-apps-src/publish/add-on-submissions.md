---
author: jnHs
Description: Add-ons are published through the Windows Dev Center dashboard.
title: Soumissions d'extensions
ms.assetid: E175AF9E-A1D4-45DF-B353-5E24E573AE67
ms.author: wdg-dev-content
ms.date: 05/09/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows10, uwp, iap, achat in-app, produit in-app, soumission iap
ms.localizationpriority: medium
ms.openlocfilehash: 37d05722578ed945fbf75040f96360bb569c6d06
ms.sourcegitcommit: 1e5590dd10d606a910da6deb67b6a98f33235959
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2018
ms.locfileid: "3227281"
---
# <a name="add-on-submissions"></a>Soumissions d'extensions

Les extensions (parfois appelées produits in-app) sont des éléments qui complètent votre application et qui peuvent être achetés par les clients. Un module complémentaire peut être une amusante nouvelle fonctionnalité, un nouveau jeu niveau, ou tout ce qui selon vous contribuera à maintenir les utilisateurs impliqués. Les extensions constituent non seulement une excellente façon de gagner de l’argent, mais également un bon moyen de renforcer le niveau d’interactivité et d’implication des clients avec votre application.

Les extensions sont publiées par le biais du tableau de bord du Centre de développement Windows. Vous devez également [activer les extensions](../monetize/in-app-purchases-and-trials.md) dans le code de votre application.

La première étape de la procédure de soumission d’une extension consiste à créer cette extension dans le tableau de bord en [définissant son type de produit et son ID produit](set-your-add-on-product-id.md). Après cela, vous allez créer une soumission afin que votre extension peut être achetée par le biais du Microsoft Store. Vous pouvez soumettre une extension au moment où vous [soumettez votre application](app-submissions.md), ou le soumettre séparément. Enfin, après avoir publié une application dans le Windows Store, vous pouvez proposer des [mises à jour](#updating-an-add-on-after-publication) des extensions sans avoir à soumettre une nouvelle fois l’application.

> [!NOTE]
> Cette section de la documentation explique comment soumettre des extensions dans le tableau de bord du Centre de développement. Sinon, vous pouvez utiliser [l’API de soumission au Microsoft Store](../monetize/create-and-manage-submissions-using-windows-store-services.md) pour automatiser la soumission des extensions.


## <a name="checklist-for-submitting-an-add-on"></a>Liste de vérification relative à la soumission d’une extension

Voici la liste des informations que vous fournissez quand vous créez la soumission d’une extension. Les éléments que vous devez obligatoirement spécifier sont signalés ci-dessous. Quelques-uns sont facultatifs ou présentent déjà des valeurs par défaut que vous pouvez modifier selon vos besoins.


### <a name="create-a-new-add-on-page"></a>Créer une nouvelle page d'extension

| Nom du champ                    | Remarques                            |
|-------------------------------|----------------------------------|
| [**Type de produit**](set-your-add-on-product-id.md#product-type)      | Requis |  
| [**ID produit**](set-your-add-on-product-id.md#product-id)          | Obligatoire |        


### <a name="properties-page"></a>Page Propriétés

| Nom du champ                    | Remarques                              |   
|-------------------------------|------------------------------------|
| [**Durée de vie du produit**](enter-add-on-properties.md#product-lifetime)  | Obligatoire si le type de produit est **Durable**. Non applicable à d’autres types de produit. |
| [**Quantité**](enter-add-on-properties.md#quantity)  | Obligatoire si le type de produit est **Consommable géré par le Windows Store**. Non applicable à d’autres types de produit. |
| [**Période d’envoi**](enter-add-on-properties.md#subscription-period)          | Obligatoire si le type de produit est **Abonnement**. Non applicable à d’autres types de produit.       |  
| [**Essai gratuit**](enter-add-on-properties.md#free-trial)          | Obligatoire si le type de produit est **Abonnement**. Non applicable à d’autres types de produit.       |
| [**Type de contenu**](enter-add-on-properties.md#content-type)          | Obligatoire    |               
| [**Mots clés**](enter-add-on-properties.md#keywords)                  | Facultatif (jusqu’à 10mots clés d’un maximum de 30caractères chacun) |
| [**Données personnalisées du développeur**](enter-add-on-properties.md#custom-developer-data)   | Facultatif (3000 caractères maximum)            |


### <a name="pricing-and-availability-page"></a>Page Tarification et disponibilité

| Nom du champ                    | Remarques                                       |
|-------------------------------|---------------------------------------------|
| [**Marchés**](set-add-on-pricing-and-availability.md#markets)  | Par défaut: tous les marchés possibles |
| [**Visibilité**](set-add-on-pricing-and-availability.md#visibility)   | Par défaut: disponible à l’achat. Affichable dans la description de votre application |
| [**Planification**](set-add-on-pricing-and-availability.md#schedule)    | Par défaut: sortie dès que possible
| [**Tarification**](set-add-on-pricing-and-availability.md#pricing)                | Requis                                    |
| [**Prix de vente**](put-apps-and-add-ons-on-sale.md)               | Facultatif                    |


### <a name="store-listings"></a>Descriptions dans le Windows Store

Une description est requise dans le Windows Store. Nous vous recommandons de fournir une description dans le Windows Store pour chaque [langue](create-add-on-store-listings.md#store-listing-languages) prise en charge par votre application.

| Nom du champ                    | Remarques                                       |
|-------------------------------|---------------------------------------------|
| [**Titre**](create-add-on-store-listings.md#title)                    | Obligatoire (100 caractères maximum)           |
| [**Description**](create-add-on-store-listings.md#description)       | Facultatif (200caractères maximum)            |
| [**Icône**](create-add-on-store-listings.md#icon)                    | Facultatif (fichier .png, 300 x 300 pixels)            |


Quand vous avez terminé d'entrer ces informations, cliquez sur **Soumettre au Windows Store**. Dans la plupart des cas, le processus de certification prend environ une heure. Au terme de ce processus, votre extension est publiée sur le WindowsStore et devient aussitôt disponible à l’achat par les clients.

> [!NOTE]
> Les extensions doivent également être implémentées dans le code de votre application. Pour plus d’informations, consultez l’article [Achats dans l’application et versions d’évaluation](../monetize/in-app-purchases-and-trials.md).


## <a name="updating-an-add-on-after-publication"></a>Mise à jour d’une extension après sa publication

Une extension est modifiable à tout moment après sa publication. Les modifications de module complémentaire sont soumises et publiées indépendamment de votre application, vous généralement inutile de mettre à jour de l’ensemble de votre application afin d’apporter des modifications à une extension, par exemple, la mise à jour son prix ou sa description.

> [!IMPORTANT]
> Si votre application est accessible aux clients utilisant Windows8.x, vous devrez créer et publier une nouvelle soumission d’application pour faire en sorte que les mises à jour d’extension soient visibles par ces clients. De même, si vous ajoutez de nouvelles extensions dans une application ciblant Windows8.x après la publication de cette dernière, vous devrez mettre à jour le code de votre application pour référencer ces extensions, puis soumettre de nouveau l’application. Dans le cas contraire, les nouvelles extensions ne seront pas visibles par les clients utilisant Windows8.x.

Pour soumettre des mises à jour, accédez à la page de l'extension dans votre tableau de bord, puis cliquez sur **Mettre à jour**. Cela crée une nouvelle soumission pour l’extension, en utilisant les informations de votre soumission précédente comme point de départ. Apportez les modifications que vous souhaitez comme, puis cliquez sur **Envoyer au Store**.

Si vous voulez supprimer une extension précédemment proposé, vous pouvez créer une soumission et modifier l’option [Distribution et visibilité](set-add-on-pricing-and-availability.md) en la définissant sur **Hidden in the Store** (Plus disponible à l’achat) avec l'option **Stop acquisition** (arrêt acquisition). Veillez à mettre à jour le code de votre application selon les besoins pour supprimer les références à l'extension (en particulier si votre application prend en charge Windows8.1 ou version antérieure; ce paramètre de visibilité ne s’appliquera pas à ces clients).
