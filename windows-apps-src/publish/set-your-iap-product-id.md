---
author: jnHs
Description: "Lorsque vous créez un produit in-app dans le tableau de bord du Centre de développement Windows, spécifiez un type de produit et affectez-lui un ID de produit."
title: "Définir le type de produit in-app et son ID de produit"
ms.assetid: 59497B0F-82F0-4CEE-B628-040EF9ED8D3D
translationtype: Human Translation
ms.sourcegitcommit: ae4727974af632a275c102a6328734597cee3e9b
ms.openlocfilehash: 9faee009cd907cd8ccdeded019e23713cbc058f0

---

# Définir le type de produit in-app et son ID de produit

Un produit in-app doit être associé à une application que vous avez déjà créée dans le tableau de bord (même si vous ne l’avez pas encore soumise). Recherchez le bouton **Créer un produit in-app** sur la page **Vue d’ensemble** ou **Produits in-app** de votre application.

Une fois que vous avez cliqué sur ce bouton, la page **Créer un produit in-app** s’affiche. Ici, vous devez spécifier un type de produit et lui attribuer un ID de produit.

## Type de produit

Vous devez commencer par indiquer le type de produit in-app que vous proposez. Cette sélection détermine la façon dont le client pourra utiliser le produit in-app.

> **Remarque** Vous ne pourrez pas modifier le type de produit après avoir enregistré cette page pour créer le produit in-app. Si vous avez choisi un type de produit incorrect, vous pouvez toujours supprimer votre soumission de produit in-app en cours et recommencer en créant un autre produit in-app.

Sélectionnez le type de produit approprié pour votre produit in-app:

- Consommable: produit pouvant être acheté, utilisé (consommé), puis acheté de nouveau. Les produits in-app consommables sont souvent utilisés pour des éléments tels que la monnaie d’un jeu (or, pièces, etc.), qui peuvent être achetés en montants prédéfinis puis dépensés par le client.
- Durable: produit acheté et détenu par l’acheteur pendant une durée définie. Les produits in-app durables servent généralement à déverrouiller des fonctionnalités supplémentaires d’une application. Ils ne sont pas consommés, mais vous pouvez définir le champ **Durée de vie du produit** pour qu’ils arrivent à expiration au bout d’un laps de temps défini (compris entre 1 et 365 jours). Par défaut, le champ **Durée de vie du produit** d’un produit in-app durable affiche **Toujours**, ce qui signifie que ce produit n’expire jamais. Vous pouvez changer la durée à l’étape [Propriétés du produit in-app](enter-iap-properties.md) du processus de soumission du produit in-app.

## ID de produit

Entrez un ID de produit unique pour votre produit in-app. Cet ID produit correspond à l’identificateur que vous devrez référencer dans le [code de votre application pour appeler le produit in-app](https://msdn.microsoft.com/library/windows/apps/mt219684).

Voici quelques éléments à prendre en considération lors du choix d'un ID produit :

-   Les clients ne verront pas cet ID produit. (Par la suite, vous pourrez entrer un [titre et une description](create-iap-descriptions.md) qui seront visibles par les clients.)
-   Vous ne pouvez plus modifier ni supprimer l'ID produit d'un produit intégré à l'application après la publication de ce dernier.
-   Un ID produit ne peut pas comporter plus de 100caractères.
-   Un ID produit ne peut pas inclure les caractères suivants : **&lt;&gt;\* % &amp; : \\ ? + ,**
-   Pour offrir votre produit in-app sur tous les appareils, utilisez uniquement des caractères alphanumériques, des points et/ou des traits de soulignement. Si vous utilisez d'autres types de caractère, le produit intégré à l'application ne sera pas disponible à l'achat pour les clients utilisant Windows Phone 8.1 ou une version antérieure.
-   Un ID produit ne doit pas nécessairement être unique dans le Windows Store, mais doit l'être dans votre compte de développeur.
 







<!--HONumber=Jun16_HO5-->


