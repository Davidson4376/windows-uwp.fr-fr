---
author: jnHs
Description: "Les produits in-app sont publiés par le biais du tableau de bord du Centre de développement Windows."
title: Soumissions des produits in-app
ms.assetid: E175AF9E-A1D4-45DF-B353-5E24E573AE67
ms.sourcegitcommit: 97f4aee47cab9064ac053e7a6e16441d6960d41f
ms.openlocfilehash: 4a1764dfb8f94409aba973a28ba2999854179196

---

# Soumissions des produits in-app


Un produit intégré à l&#39;application est un élément supplémentaire pour votre application qui peut être acheté par les clients. Il peut s'agir d'un composant additionnel permettant d'ajouter une nouvelle fonctionnalité amusante, un nouveau niveau de jeu ou tout ce qui selon vous contribuera à maintenir les utilisateurs impliqués. Les produits intégrés à l'application constituent non seulement une excellente façon de gagner de l'argent, mais également un bon moyen de renforcer le niveau d'interactivité et d'implication des clients avec votre application.

Les produits in-app sont publiés par le biais du tableau de bord du Centre de développement Windows. Vous devez également [activer les produits in-app](../monetize/enable-in-app-product-purchases.md) dans le code de votre application.

La première étape de la procédure de soumission d’un produit in-app consiste à créer ce produit dans le tableau de bord en [définissant son type de produit et son ID produit](set-your-iap-product-id.md). Vous pouvez ensuite créer une soumission afin que votre produit in-app puisse être acheté par le biais du Windows Store. Vous pouvez soumettre un produit in-app au moment où vous [soumettez votre application](app-submissions.md), ou le soumettre séparément. Enfin, après avoir publié une application dans le Windows Store, vous pouvez proposer des [mises à jour](#updating-an-iap-after-submission) des produits in-app sans avoir à soumettre une nouvelle fois l’application.

## Liste de contrôle relative à la soumission d’un produit in-app

Voici la liste des informations que vous fournissez quand vous créez la soumission d’un produit in-app. Les éléments que vous devez obligatoirement spécifier sont signalés ci-dessous. Quelques-uns sont facultatifs ou présentent déjà des valeurs par défaut que vous pouvez modifier selon vos besoins.

### Créer une page d’un produit in-app
| Nom du champ                    | Remarques                            | 
|-------------------------------|----------------------------------|
| [**Type de produit**](set-your-iap-product-id.md#product-type)      | Obligatoire. Si le produit est de type **Durable**, vous devez renseigner le champ **Durée de vie du produit**. |  
| [**ID de produit**](set-your-iap-product-id.md#product-id)          | Obligatoire |        

### Page Propriétés
| Nom du champ                    | Remarques                              |   
|-------------------------------|------------------------------------|
| [**Durée de vie du produit**](enter-iap-properties.md#product-lifetime)  | Obligatoire si le type de produit est **Durable**. Non applicable si le type de produit est **Consommable**. | 
| [**Type de contenu**](enter-iap-properties.md#content-type)          | Obligatoire       |               
| [**Mots clés**](enter-iap-properties.md#keywords)                  | Facultatif (jusqu'à 10mots clés d'un maximum de 30caractères chacun) | 
| [**Étiquette**](enter-iap-properties.md#tag)                               | Facultatif (3000 caractères maximum)             | 

### Page Tarification et disponibilité 
| Nom du champ                    | Remarques                                       | 
|-------------------------------|---------------------------------------------|
| [**Prix de base**](set-iap-pricing-and-availability.md#base-price)                | Obligatoire                                    | 
| [**Marchés et prix personnalisés**](set-iap-pricing-and-availability.md#markets-and-custom-prices)  | Valeur par défaut : produit disponible dans tous les marchés possibles | 
| [**Prix de vente**](put-apps-and-iaps-on-sale.md)               | Facultatif                             |
| [**Distribution et visibilité**](set-iap-pricing-and-availability.md#distribution-and-visibility)   | Valeur par défaut: les clients peuvent trouver le produit intégré à l'application en parcourant le WindowsStore ou en effectuant une recherche dans ce dernier | 
| [**Date de publication**](set-iap-pricing-and-availability.md#publish-date)                | Valeur par défaut: publication dès que le produit in-app a obtenu la certification |

### Page Descriptions
Une description requise. Nous vous recommandons de fournir une description pour chaque [langue](create-iap-descriptions.md#languages) prise en charge par votre application.

| Nom du champ                    | Remarques                                       | 
|-------------------------------|---------------------------------------------|
| [**Titre**](create-iap-descriptions.md#title)                    | Obligatoire (100 caractères maximum)              |
| [**Description**](create-iap-descriptions.md#description)       | Facultatif (200caractères maximum)              |
| [**Icône**](create-iap-descriptions.md#icon)                    | Facultatif (fichier .png, 300 x 300 pixels)             | 

Quand vous avez terminé d'entrer ces informations, cliquez sur **Soumettre au Windows Store**. Dans la plupart des cas, le processus de certification prend environ une heure. Au terme de ce processus, votre produit in-app est publié sur le WindowsStore et devient aussitôt disponible à l’achat par les clients.

**Remarque** Les produits in-app doivent également être implémentés dans le code de votre application. Pour plus d’informations, voir [Activer les achats de produits in-app](../monetize/enable-in-app-product-purchases.md).


## Mise à jour d'un produit in-app après sa publication

Un produit intégré à l’application est modifiable à tout moment après sa publication. Les modifications apportées à un produit in-app sont soumises et publiées indépendamment de votre application. Par conséquent, vous n’avez généralement pas besoin de mettre à jour la totalité de l’application pour modifier un produit in-app, par exemple afin de mettre à jour son prix ou sa description.

> **Important** Si votre application est accessible aux clients utilisant Windows 8.x, vous devrez créer et publier une nouvelle soumission d’application pour faire en sorte que les mises à jour de produit in-app soient visibles par ces clients. De même, si vous ajoutez de nouveaux produits in-app dans une application ciblant Windows 8.x après la publication de cette dernière, vous devrez mettre à jour le code de votre application pour référencer ces produits, puis soumettre de nouveau l’application. Dans le cas contraire, les nouveaux produits intégrés à l’application ne seront pas visibles par les clients utilisant Windows 8.x.

Pour soumettre des mises à jour, accédez à la page du produit intégré à l’application dans votre tableau de bord, puis cliquez sur **Mettre à jour**. Cette opération crée une autre soumission pour le produit in-app, initialement renseignée avec les informations de votre soumission précédente. Modifiez les informations souhaitées, puis cliquez sur **Envoyer au Store**.

Si vous voulez supprimer un produit in-app précédemment proposé, vous pouvez créer une soumission et modifier l’option [Distribution et visibilité](set-iap-pricing-and-availability.md) en la définissant sur **Plus disponible à l’achat. Non affiché dans la description de votre application**. Veillez à mettre à jour le code de votre application comme nécessaire pour supprimer les références à ce produit in-app.




<!--HONumber=Jun16_HO5-->


