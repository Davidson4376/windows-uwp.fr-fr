---
author: jnHs
Description: "Quand vous envoyez un module complémentaire, les options de la page Propriétés vous aident à déterminer le comportement de ce module lorsqu’il est proposé aux clients."
title: "Définir les propriétés d’un module complémentaire"
ms.assetid: 26D2139F-66FD-479E-940B-7491238ADCAE
translationtype: Human Translation
ms.sourcegitcommit: e3bc74fab0ae75c35581e33323100376ad755e9c
ms.openlocfilehash: 1c030f7d79de37e20620cf56a30b1c1570e9b90b

---

# Définir les propriétés d’un module complémentaire


Quand vous envoyez un module complémentaire, les options de la page **Propriétés** vous aident à déterminer le comportement de ce module lorsqu’il est proposé aux clients.

## Type de produit

Vous sélectionnez le type de produit lors de la [création initiale du module complémentaire](set-your-add-on-product-id.md). Le type de produit que vous avez sélectionné s’affiche ici, mais vous ne pouvez pas le modifier.

> **Remarque** Si vous n’avez pas publié le module complémentaire. Pour choisir un type de produit différent, vous pouvez supprimer la soumission et recommencer. 

Selon le type de produit que vous avez sélectionné, vous pouvez voir les champs suivants:

### Durée de vie du produit
Si vous avez sélectionné un type de produit **Durable**, la **Durée de vie du produit** s’affiche ici. Par défaut, le champ **Durée de vie du produit** d’un module complémentaire durable affiche **Toujours**, ce qui signifie que ce module n’expire jamais. Selon votre gré, vous pouvez définir le champ **Durée de vie du produit** pour que le module complémentaire arrive à expiration au bout d’un laps de temps défini (compris entre1 et 365jours). 

### Quantité
Si vous avez sélectionné **Consommable géré par le Windows Store** comme type de produit, ce champ affiche la **Quantité**. Vous devez entrer un numéro compris entre 1 et 1000000. Cette quantité est accordée au client au moment d’acquérir le module complémentaire. Ensuite, le Windows Store la rééquilibre en fonction de la consommation du module complémentaire par le client signalée par l’application.

## Type de contenu

Quel que soit le type de votre module complémentaire, vous devez également indiquer le type de contenu que vous proposez. Pour la plupart des modules complémentaires, le type de contenu doit être **Téléchargement de logiciels électroniques**. Si une autre option dans la liste semble mieux décrire vos modules complémentaires (par exemple, si vous proposez un téléchargement de musique ou un livre électronique), choisissez-la à la place. 

Voici les options disponibles pour le type de contenu d’un module complémentaire:

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

## Mots clés

Vous pouvez fournir jusqu’à dix mots clés d’un maximum de 30caractères chacun pour chaque module complémentaire envoyé. Votre application peut ensuite rechercher les modules complémentaires qui correspondent à ces mots clés. Cette fonctionnalité vous permet de générer dans votre application des écrans qui peuvent charger les modules complémentaires, sans avoir à spécifier directement l’ID du produit dans le code de l’application. Vous pouvez ensuite modifier à tout moment les mots clés d’un module complémentaire sans toucher le code de votre application ni soumettre l’application à nouveau.

> **Remarque** Les mots clés ne sont pas pris en charge dans les packages ciblant Windows8 et Windows8.1.

## Données personnalisées du développeur

Vous pouvez entrer jusqu’à 3000caractères dans le champ **Données personnalisées du développeur** afin de fournir du contexte supplémentaire pour votre produit in-app.

> **Remarque** Ce champ s’appelait auparavant **Balise**.

Par exemple, supposons que vous proposiez une application de jeu et que vous vendiez un sac de pièces d’or comme module complémentaire. Le champ **Données personnalisées du développeur** permet à l’application de rechercher ce sac d’or. Vous pouvez modifier la valeur de ce champ à tout moment (en l’occurrence, le nombre de pièces figurant dans le sac) en mettant à jour le contenu du champ **Données personnalisées du développeur** du module complémentaire, sans avoir à modifier le code de votre application ni à soumettre l’application à nouveau.

> **Remarque** Le champ **Données personnalisées du développeur** n’est pas pris en charge dans les packages ciblant Windows8 et Windows8.1.

 

 

 







<!--HONumber=Aug16_HO5-->


