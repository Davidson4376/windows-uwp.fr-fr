---
author: jnHs
Description: Add-ons (or in-app products) are published through Partner Center.
title: Soumissions d'extensions
ms.assetid: E175AF9E-A1D4-45DF-B353-5E24E573AE67
ms.author: wdg-dev-content
ms.date: 10/31/2018
ms.topic: article
keywords: Windows10, uwp, iap, achat in-app, produit in-app, soumission iap
ms.localizationpriority: medium
ms.openlocfilehash: 28fd2e104de12cc297ce5d28ddd18b0ce550a5d0
ms.sourcegitcommit: 38f06f1714334273d865935d9afb80efffe97a17
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/09/2018
ms.locfileid: "6203525"
---
# <a name="add-on-submissions"></a>Soumissions d'extensions

Les extensions (parfois appelées produits in-app) sont des éléments qui complètent votre application et qui peuvent être achetés par les clients. Un module complémentaire peut être une nouvelle fonctionnalité amusante, un nouveau niveau de jeu ou n’importe quel autre vous pensez sera à maintenir fidéliser les utilisateurs. Les extensions constituent non seulement une excellente façon de gagner de l’argent, mais également un bon moyen de renforcer le niveau d’interactivité et d’implication des clients avec votre application.

Les modules complémentaires sont publiés par le biais de [L’espace partenaires](https://partner.microsoft.com/dashboard)et vous devez disposer d’un [compte de développeur](http://go.microsoft.com/fwlink/p/?LinkId=615100)de actif. Vous devez également [activer les extensions](../monetize/in-app-purchases-and-trials.md) dans le code de votre application.

La première étape dans le processus de soumission d’extension consiste à créer l’extension dans l’espace partenaires en [définissant son type et l’ID de produit](set-your-add-on-product-id.md). Après cela, vous allez créer une soumission afin que votre extension peut être achetée par le biais du Microsoft Store. Vous pouvez soumettre une extension au moment où vous [soumettez votre application](app-submissions.md), ou le soumettre séparément. Enfin, après avoir publié une application dans le Windows Store, vous pouvez proposer des [mises à jour](#updating-an-add-on-after-publication) des extensions sans avoir à soumettre une nouvelle fois l’application.

> [!NOTE]
> Cette section de la documentation décrit comment soumettre des extensions dans l’espace partenaires. Sinon, vous pouvez utiliser [l’API de soumission au Microsoft Store](../monetize/create-and-manage-submissions-using-windows-store-services.md) pour automatiser la soumission des extensions.


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

Une extension est modifiable à tout moment après sa publication. Les modifications de module complémentaire sont soumises et publiées indépendamment de votre application, vous généralement inutile de mettre à jour de l’ensemble de l’application afin d’apporter des modifications à une extension, par exemple, la mise à jour son prix ou sa description.

Pour envoyer des mises à jour, accédez à le de page Ajout dans l’espace partenaires, puis cliquez sur **mise à jour**. Cela crée une nouvelle soumission pour l’extension, en utilisant les informations de votre soumission précédente comme point de départ. Apportez les modifications que vous souhaitez comme, puis cliquez sur **Envoyer au Store**.

Si vous voulez supprimer une extension précédemment proposé, vous pouvez créer une soumission et modifier l’option [Distribution et visibilité](set-add-on-pricing-and-availability.md) en la définissant sur **Hidden in the Store** (Plus disponible à l’achat) avec l'option **Stop acquisition** (arrêt acquisition). Veillez à mettre à jour le code de votre application en fonction des besoins pour supprimer les références à l’extension (en particulier si votre application publiée précédemment prend en charge Windows 8.1 version antérieure; ce paramètre de visibilité ne s’appliquent à ces clients).

> [!IMPORTANT]
> Si votre application publiée précédemment est disponible pour les clients sur Windows 8.x, vous devrez créer et publier une nouvelle soumission d’application pour faire en sorte que les mises à jour de module complémentaire soient visibles par ces clients. De même, si vous ajoutez de nouvelles extensions dans une application ciblant Windows8.x après la publication de cette dernière, vous devrez mettre à jour le code de votre application pour référencer ces extensions, puis soumettre de nouveau l’application. Dans le cas contraire, les nouvelles extensions ne seront pas visibles par les clients utilisant Windows8.x.
