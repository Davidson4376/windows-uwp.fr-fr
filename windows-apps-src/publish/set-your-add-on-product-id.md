---
author: jnHs
Description: "Lorsque vous créez un module complémentaire dans le tableau de bord du Centre de développement Windows, affectez-lui un type et un ID de produit."
title: "Définir le type et l’ID de produit d’un module complémentaire"
ms.assetid: 59497B0F-82F0-4CEE-B628-040EF9ED8D3D
translationtype: Human Translation
ms.sourcegitcommit: e59324aca65cf8baacb085da22a20d952fdb8c9a
ms.openlocfilehash: 2a469506c8b440e1aa8555ac57b88f2026ae4d8e

---

# Définir le type et l’ID de produit d’un module complémentaire

Un module complémentaire doit être associé à une application que vous avez déjà créée dans le tableau de bord (même si vous ne l’avez pas encore soumise). Recherchez le bouton **Créer un module complémentaire** sur la page **Vue d’ensemble** ou **Modules complémentaires** de votre application.

Une fois que vous avez cliqué sur ce bouton, la page **Créer un module complémentaire** s’affiche. Ici, vous devez spécifier un type de produit et lui attribuer un ID de produit.

## Type de produit

Vous devez commencer par indiquer le type de module complémentaire que vous proposez. Cette sélection détermine la façon dont le client pourra utiliser votre module complémentaire.

> **Remarque** Vous ne pourrez pas modifier le type de produit après avoir enregistré cette page pour créer le module complémentaire. Si vous avez choisi un type de produit incorrect, vous pouvez toujours supprimer votre soumission de module complémentaire en cours et recommencer en créant un autre module complémentaire.

Si le produit peut être acheté, utilisé (consommé), puis racheté, vous devez sélectionner un type de produit **consommable**. Les modules complémentaires consommables sont souvent utilisés pour la monnaie d’un jeu par exemple (or, pièces, etc.), qui peuvent être achetés en montants prédéfinis puis dépensés par le client. Pour plus d’informations sur l’ajout de modules complémentaires consommables dans votre application, voir [Activer les achats de modules complémentaires consommables](../monetize/enable-consumable-add-on-purchases.md).

Vous pouvez sélectionner deux types de modules complémentaires consommables:

- **Consommable géré par le développeur**: pris en charge sur toutes les versions de système d’exploitation. Le solde et l’acquisition doivent être gérés dans votre application. 
- **Consommable géré par le Windows store:** Microsoft assure le suivi du solde sur tous les appareils du client fonctionnant sous Windows10, version1607 ou ultérieure; non pris en charge sur les versions antérieures du système d’exploitation. Pour utiliser cette option, le produit parent doit être compilé à l’aide du Kit de développement logiciel Windows10 version14393 ou ultérieure. Notez également que vous ne pouvez pas soumettre au Windows Store un module complémentaire consommable géré par le WindowsStore tant que le produit parent n’a pas été publié (vous pouvez cependant créer la soumission dans votre tableau de bord et commencer à travailler dessus à tout moment). Vous devez renseigner la quantité concernant votre module complémentaire consommable géré par le WindowsStore dans la page **Propriétés**.

Si votre produit ne peut être acheté qu’une seule fois, vous devez sélectionner **Durable**. Les modules complémentaires durables servent généralement à déverrouiller des fonctionnalités supplémentaires d’une application. Ils ne sont pas consommés, mais vous pouvez définir le champ **Durée de vie du produit** pour qu’ils arrivent à expiration au bout d’un laps de temps défini (compris entre 1 et 365jours). Par défaut, le champ **Durée de vie du produit** d’un module complémentaire durable affiche **Toujours**, ce qui signifie que ce module n’expire jamais. Vous pouvez changer la durée à l’étape [Propriétés du module complémentaire](enter-add-on-properties.md) du processus de soumission du module complémentaire.

## ID de produit

Entrez un ID de produit unique pour votre module complémentaire. Cet ID produit correspond à l’identificateur que vous devrez référencer dans le [code de votre application pour appeler le module complémentaire](https://msdn.microsoft.com/library/windows/apps/mt219684).

Voici quelques éléments à prendre en considération lors du choix d’un ID produit:

-   Les clients ne verront pas cet ID produit. (Par la suite, vous pourrez entrer un [titre et une description](create-add-on-descriptions.md) qui seront visibles par les clients.)
-   Vous ne pouvez plus modifier ni supprimer l’ID produit d’un module complémentaire après la publication de ce dernier.
-   Un ID produit ne peut pas comporter plus de 100caractères.
-   Un ID produit ne peut pas inclure les caractères suivants: **&lt; &gt; \* % &amp; : \\ ? + ,**
-   Pour offrir votre module complémentaire sur tous les appareils, utilisez uniquement des caractères alphanumériques, des points et/ou des traits de soulignement. Si vous utilisez d’autres types de caractère, le module complémentaire ne sera pas disponible à l’achat pour les clients utilisant WindowsPhone8.1 ou une version antérieure.
-   Un ID produit ne doit pas nécessairement être unique dans le Windows Store, mais doit l’être dans votre compte de développeur.
 







<!--HONumber=Aug16_HO5-->


