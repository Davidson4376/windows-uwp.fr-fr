---
author: jnHs
Description: Cette opération constitue la première étape de la soumission d’un produit in-app dans le tableau de bord du Centre de développement Windows.
title: Définir l’ID produit d’un produit in-app
ms.assetid: 59497B0F-82F0-4CEE-B628-040EF9ED8D3D
---

# Définir l’ID produit d’un produit in-app


Cette opération constitue la première étape de la soumission d&#39;un produit intégré à l&#39;application dans le tableau de bord du Centre de développement Windows. Cet ID produit correspond à l’identificateur que vous devrez référencer dans le [code de votre application pour appeler le produit in-app](https://msdn.microsoft.com/library/windows/apps/mt219684).

Un produit in-app doit être associé à une application que vous avez déjà créée dans le tableau de bord (même si vous ne l’avez pas encore soumise). Recherchez le bouton **Créer un produit in-app** sur la page **Vue d’ensemble** ou **Produits in-app** de votre application.

Une fois que vous avez cliqué sur ce bouton, la page **Créer un produit in-app** s’affiche. Entrez sur cette page l'ID produit unique que vous utiliserez pour faire référence à l'offre dans votre code.

Voici quelques éléments à prendre en considération lors du choix d'un ID produit :

-   Les clients ne verront pas cet ID produit. (Par la suite, vous pourrez entrer un [titre et une description](create-iap-descriptions.md) qui seront visibles par les clients.)
-   Vous ne pouvez plus modifier ni supprimer l'ID produit d'un produit intégré à l'application après la publication de ce dernier.
-   Un ID produit ne peut pas comporter plus de 100 caractères.
-   Un ID produit ne peut pas inclure les caractères suivants : **&lt;&gt;\* % &amp; : \\ ? + ,**
-   Pour offrir votre produit in-app sur tous les appareils, utilisez uniquement des caractères alphanumériques, des points et/ou des traits de soulignement. Si vous utilisez d'autres types de caractère, le produit intégré à l'application ne sera pas disponible à l'achat pour les clients utilisant Windows Phone 8.1 ou une version antérieure.
-   Un ID produit ne doit pas nécessairement être unique dans le Windows Store, mais doit l'être dans votre compte de développeur.

 

 






<!--HONumber=May16_HO2-->


