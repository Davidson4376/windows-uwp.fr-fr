---
author: jnHs
Description: "Les modules complémentaires sont publiés par le biais du tableau de bord du Centre de développement Windows."
title: "Soumissions de modules complémentaires"
ms.assetid: E175AF9E-A1D4-45DF-B353-5E24E573AE67
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.openlocfilehash: 7de8b6db65bb9ec58f16643dc8b477c274513c9a
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="add-on-submissions"></a>Soumissions de modules complémentaires

Les modules complémentaires (parfois appelés produits in-app) sont des éléments qui complètent votre application et qui peuvent être achetés par les clients. Il peut s’agir d’une nouvelle fonctionnalité amusante, d’un nouveau niveau de jeu ou de tout ce qui selon vous contribuera à maintenir les utilisateurs impliqués. Les modules complémentaires constituent non seulement une excellente façon de gagner de l’argent, mais également un bon moyen de renforcer le niveau d’interactivité et d’implication des clients avec votre application.

Les modules complémentaires sont publiés par le biais du tableau de bord du Centre de développement Windows. Vous devez également [activer les modules complémentaires](../monetize/in-app-purchases-and-trials.md) dans le code de votre application.

La première étape de la procédure de soumission d’un module complémentaire consiste à créer ce module dans le tableau de bord en [définissant son type de produit et son ID produit](set-your-add-on-product-id.md). Vous pouvez ensuite créer une soumission afin que votre module complémentaire puisse être acheté par le biais du Windows Store. Vous pouvez soumettre un module complémentaire au moment où vous [soumettez votre application](app-submissions.md), ou le soumettre séparément. Enfin, après avoir publié une application dans le Windows Store, vous pouvez proposer des [mises à jour](#updating-an-add-on-after-publication) des modules complémentaires sans avoir à soumettre une nouvelle fois l’application.

> **Remarque**&nbsp;&nbsp;Cette section de la documentation explique comment soumettre des extensions dans le tableau de bord du Centre de développement. Sinon, vous pouvez utiliser [l’API de soumission du Windows Store](../monetize/create-and-manage-submissions-using-windows-store-services.md) pour automatiser la soumission des modules complémentaires.

## <a name="checklist-for-submitting-an-add-on"></a>Liste de vérification relative à la soumission d’un module complémentaire

Voici la liste des informations que vous fournissez quand vous créez la soumission d’un module complémentaire. Les éléments que vous devez obligatoirement spécifier sont signalés ci-dessous. Quelques-uns sont facultatifs ou présentent déjà des valeurs par défaut que vous pouvez modifier selon vos besoins.

### <a name="create-a-new-add-on-page"></a>Créer une nouvelle page de module complémentaire
| Nom du champ                    | Remarques                            |
|-------------------------------|----------------------------------|
| [**Type de produit**](set-your-add-on-product-id.md#product-type)      | Obligatoire. Si le produit est de type **Durable**, vous devez renseigner le champ **Durée de vie du produit**. |  
| [**ID de produit**](set-your-add-on-product-id.md#product-id)          | Obligatoire |        

<span/>

### <a name="properties-page"></a>Page Propriétés
| Nom du champ                    | Remarques                              |   
|-------------------------------|------------------------------------|
| [**Durée de vie du produit**](enter-add-on-properties.md#product-lifetime)  | Obligatoire si le type de produit est **Durable**. Non applicable à d’autres types de produit. |
| [**Quantité**](enter-add-on-properties.md#quantity)  | Obligatoire si le type de produit est **Consommable géré par le Windows Store**. Non applicable à d’autres types de produit.
| [**Type de contenu**](enter-add-on-properties.md#content-type)          | Obligatoire       |               
| [**Mots clés**](enter-add-on-properties.md#keywords)                  | Facultatif (jusqu’à 10mots clés d’un maximum de 30caractères chacun) |
| [**Données personnalisées du développeur**](enter-add-on-properties.md#custom-developer-data)                               | Facultatif (3000 caractères maximum)             |

<span/>

### <a name="pricing-and-availability-page"></a>Page Tarification et disponibilité
| Nom du champ                    | Remarques                                       |
|-------------------------------|---------------------------------------------|
| [**Prix de base**](set-add-on-pricing-and-availability.md#base-price)                | Obligatoire                                    |
| [**Marchés et prix personnalisés**](set-add-on-pricing-and-availability.md#markets-and-custom-prices)  | Valeur par défaut : produit disponible dans tous les marchés possibles |
| [**Prix de vente**](put-apps-and-add-ons-on-sale.md)               | Facultatif                             |
| [**Distribution et visibilité**](set-add-on-pricing-and-availability.md#distribution-and-visibility)   | Valeur par défaut: les clients peuvent trouver le module complémentaire à l’application en parcourant le WindowsStore ou en effectuant une recherche dans ce dernier |
| [**Date de publication**](set-add-on-pricing-and-availability.md#publish-date)                | Valeur par défaut: publication dès que le module complémentaire a obtenu la certification |

<span/>

### <a name="store-listings"></a>Descriptions dans le Windows Store
Une description est requise dans le Windows Store. Nous vous recommandons de fournir une description dans le Windows Store pour chaque [langue](create-add-on-store-listings.md#languages) prise en charge par votre application.

| Nom du champ                    | Remarques                                       |
|-------------------------------|---------------------------------------------|
| [**Titre**](create-add-on-store-listings.md#title)                    | Obligatoire (100 caractères maximum)              |
| [**Description**](create-add-on-store-listings.md#description)       | Facultatif (200caractères maximum)              |
| [**Icône**](create-add-on-store-listings.md#icon)                    | Facultatif (fichier .png, 300 x 300 pixels)             |

<span/>

Quand vous avez terminé d'entrer ces informations, cliquez sur **Soumettre au Windows Store**. Dans la plupart des cas, le processus de certification prend environ une heure. Au terme de ce processus, votre module complémentaire est publié sur le WindowsStore et devient aussitôt disponible à l’achat par les clients.

>**Remarque**&nbsp;&nbsp;Les modules complémentaires doivent également être implémentés dans le code de votre application. Pour plus d’informations, voir [Versions d’évaluation et achats in-app](../monetize/in-app-purchases-and-trials.md).


## <a name="updating-an-add-on-after-publication"></a>Mise à jour d’un module complémentaire après sa publication

Un module complémentaire est modifiable à tout moment après sa publication. Les modifications apportées à un module complémentaire sont soumises et publiées indépendamment de votre application. Par conséquent, vous n’avez généralement pas besoin de mettre à jour la totalité de l’application pour modifier un module complémentaire, par exemple afin de mettre à jour son prix ou sa description.

> **Important**&nbsp;&nbsp;Si votre application est accessible aux clients utilisant Windows8.x, vous devrez créer et publier une nouvelle soumission d’application pour faire en sorte que les mises à jour de module complémentaire soient visibles par ces clients. De même, si vous ajoutez de nouveaux modules complémentaires dans une application ciblant Windows8.x après la publication de cette dernière, vous devrez mettre à jour le code de votre application pour référencer ces modules, puis soumettre de nouveau l’application. Dans le cas contraire, les nouveaux modules complémentaires ne seront pas visibles par les clients utilisant Windows8.x.

Pour soumettre des mises à jour, accédez à la page du module complémentaire dans votre tableau de bord, puis cliquez sur **Mettre à jour**. Cette opération crée une autre soumission pour le module complémentaire, initialement renseignée avec les informations de votre soumission précédente. Modifiez les informations souhaitées, puis cliquez sur **Envoyer au Store**.

Si vous voulez supprimer un module complémentaire précédemment proposé, vous pouvez créer une soumission et modifier l’option [Distribution et visibilité](set-add-on-pricing-and-availability.md) en la définissant sur **Plus disponible à l’achat. Non affiché dans la description de votre application**. Veillez à mettre à jour le code de votre application comme nécessaire pour supprimer les références à ce module complémentaire.
