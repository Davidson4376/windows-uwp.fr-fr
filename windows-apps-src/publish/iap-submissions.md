---
Description: Les produits in-app sont publiés par le biais du tableau de bord du Centre de développement Windows.
title: Soumissions des produits in-app
ms.assetid: E175AF9E-A1D4-45DF-B353-5E24E573AE67
---

# Soumissions des produits in-app


Un produit intégré à l'application est un élément supplémentaire pour votre application qui peut être acheté par les clients. Il peut s'agir d'un composant additionnel permettant d'ajouter une nouvelle fonctionnalité amusante, un nouveau niveau de jeu ou tout ce qui selon vous contribuera à maintenir les utilisateurs impliqués. Les produits intégrés à l'application constituent non seulement une excellente façon de gagner de l'argent, mais également un bon moyen de renforcer le niveau d'interactivité et d'implication des clients avec votre application.

Les produits in-app sont publiés par le biais du tableau de bord du Centre de développement Windows. Vous devez également [activer les produits in-app](https://msdn.microsoft.com/library/windows/apps/mt219684) dans le code de votre application.

La première étape de la procédure de soumission d'un produit in-app consiste à créer ce produit dans le tableau de bord en [définissant son ID produit](set-your-iap-product-id.md). Vous pouvez ensuite créer une soumission afin que votre produit in-app puisse être acheté par le biais du Windows Store. Vous pouvez soumettre un produit in-app au moment où vous [soumettez votre application](app-submissions.md), ou le soumettre séparément. Enfin, après avoir publié une application dans le Windows Store, vous pouvez proposer des [mises à jour](#updating-an-iap-after-submission) des produits in-app sans avoir à soumettre une nouvelle fois l’application.

## Liste de contrôle relative à la soumission d’un produit in-app


Voici les informations que vous pouvez fournir quand vous créez la soumission d’un produit in-app. Les éléments que vous devez obligatoirement spécifier sont signalés ci-dessous. Quelques-uns sont facultatifs ou présentent déjà des valeurs par défaut que vous pouvez modifier selon vos besoins.

### Page Propriétés
| Nom du champ                    | Remarques                                       | Informations supplémentaires                                                             |
|-------------------------------|---------------------------------------------|---------------------------------------------------------------------------|
| **Type de produit**              | Obligatoire. Si le produit est de type **Durable**, vous devez renseigner le champ **Durée de vie du produit**. | [Entrer les propriétés d'un produit intégré à l'application](enter-iap-properties.md)         |
| **Type de contenu**              | Obligatoire                                    | [Entrer les propriétés d’un produit in-app](enter-iap-properties.md)                           | 
| **Mots clés**                  | Facultatif (jusqu’à 10 mots clés d’un maximum de 30 caractères chacun) | [Entrer les propriétés d’un produit in-app](enter-iap-properties.md)                 |
| **Balise**                       | Facultatif (3 000 caractères maximum)             | [Entrer les propriétés d’un produit in-app](enter-iap-properties.md)                           |

### Page Tarification et disponibilité 
| Nom du champ                    | Remarques                                       | Informations supplémentaires                                                             |
|-------------------------------|---------------------------------------------|---------------------------------------------------------------------------|
| **Prix de base**                | Obligatoire                                    | [Définir la tarification et la disponibilité d'un produit intégré à l'application](set-iap-pricing-and-availability.md)   |
| **Marchés et prix personnalisés** | Valeur par défaut : produit disponible dans tous les marchés possibles | [Définir la tarification et la disponibilité d’un produit in-app](set-iap-pricing-and-availability.md)   |
| **Prix de vente**              | Facultatif                                    | [Définir la tarification et la disponibilité d’un produit in-app](set-iap-pricing-and-availability.md)   |
| **Distribution et visibilité** | Valeur par défaut : les clients peuvent trouver le produit in-app en parcourant le Windows Store ou en effectuant une recherche dans ce dernier | [Définir la tarification et la disponibilité d’un produit in-app](set-iap-pricing-and-availability.md) |
| **Date de publication**              | Valeur par défaut : publication dès que le produit in-app a obtenu la certification | [Définir la tarification et la disponibilité d’un produit in-app](set-iap-pricing-and-availability.md)   |

### Page Descriptions
Une description requise. Nous vous recommandons de fournir une description pour chaque langue prise en charge par votre application.

| Nom du champ                    | Remarques                                       | Informations supplémentaires       |
|-------------------------------|---------------------------------------------|---------------------|
| **Titre**                     | Obligatoire (100 caractères maximum)              | [Créer des descriptions de produit intégré à l'application](create-iap-descriptions.md)                     |
| **Description**               | Facultatif (200 caractères maximum)              | [Créer des descriptions de produit in-app](create-iap-descriptions.md)                     |
| **Icône**                      | Facultatif (fichier .png, 300 x 300 pixels)             | [Créer des descriptions de produit in-app](create-iap-descriptions.md)                     |

Quand vous avez terminé d’entrer ces informations, cliquez sur **Envoyer au Store**. Dans la plupart des cas, le processus de certification prend environ une heure. Au terme de ce processus, votre produit in-app est publié sur le Windows Store et devient aussitôt disponible à l’achat par les clients.

**Remarque** Les produits in-app doivent également être implémentés dans le code de votre application. Pour plus d’informations, voir [Activer les achats de produits in-app](https://msdn.microsoft.com/library/windows/apps/mt219684).


## Mise à jour d'un produit in-app après sa publication


Un produit intégré à l’application est modifiable à tout moment après sa publication. Les modifications apportées à un produit in-app sont soumises et publiées indépendamment de votre application. Par conséquent, vous n’avez généralement pas besoin de mettre à jour la totalité de l’application pour modifier un produit in-app, par exemple afin de mettre à jour son prix ou sa description.

> **Important** Si votre application est accessible aux clients utilisant Windows 8.x, vous devrez créer et publier une nouvelle soumission d’application pour faire en sorte que les mises à jour de produit in-app soient visibles par ces clients. De même, si vous ajoutez de nouveaux produits in-app dans une application ciblant Windows 8.x après la publication de cette dernière, vous devrez mettre à jour le code de votre application pour référencer ces produits, puis soumettre de nouveau l’application. Dans le cas contraire, les nouveaux produits intégrés à l’application ne seront pas visibles par les clients utilisant Windows 8.x.

Pour soumettre des mises à jour, accédez à la page du produit intégré à l’application dans votre tableau de bord, puis cliquez sur **Mettre à jour**. Cette opération crée une autre soumission pour le produit in-app, initialement renseignée avec les informations de votre soumission précédente. Modifiez les informations souhaitées, puis cliquez sur **Envoyer au Store**.

Si vous voulez supprimer un produit in-app précédemment proposé, vous pouvez créer une soumission et modifier l’option [Distribution et visibilité](set-iap-pricing-and-availability.md) en la définissant sur **Plus disponible à l’achat. Non affiché dans la description de votre application**. Veillez à mettre à jour le code de votre application comme nécessaire pour supprimer les références à ce produit in-app.



<!--HONumber=Mar16_HO1-->


