---
author: jnHs
Description: "La première étape de création d’une application dans votre tableau de bord du Centre de développement Windows consiste à réserver un nom d’application. Découvrez comment réserver des noms d’application et bénéficiez de suggestions pour choisir un nom exceptionnel pour votre application."
title: "Créer votre application en réservant un nom"
keywords: 
ms.assetid: 6DC58A9A-DF47-4652-8D13-0AC9289F5950
translationtype: Human Translation
ms.sourcegitcommit: 3b65bbaf2498dde7484c055ff86ed09e89bf3405
ms.openlocfilehash: 1be7229086e1f2f932e0945334098d89a9978b70

---

# Créer votre application en réservant un nom


La première étape de création d’une application dans votre tableau de bord du Centre de développement Windows consiste à réserver un nom d’application. Découvrez comment réserver des noms d’application et bénéficiez de suggestions pour [choisir un nom exceptionnel pour votre application](#choosing-your-app-s-name). Chaque nom réservé doit être unique à travers le Windows Store.

> **Remarque** Si vous venez de créer une application Windows Phone et que vous n’avez pas encore réservé de nom pour celle-ci, vous pouvez toujours gérer et soumettre cette application. Toutefois, pour charger des packages .appx pour celle-ci, ou pour [voir les détails relatifs à l’identité de l’application](view-app-identity-details.md) propres à la création de packages .appx, vous devez réserver un nom unique en procédant de la manière décrite ci-dessous. Cela empêche également quiconque de réserver ce nom.

Au moment de [charger les packages de votre application](upload-app-packages.md), vérifiez que la valeur [**Package/Properties/DisplayName**](https://msdn.microsoft.com/library/windows/apps/dn423240) correspond au nom que vous avez réservé pour votre application dans le **Tableau de bord**. Si vous utilisez Microsoft Visual Studio pour créer le package de votre application, cet attribut est rempli automatiquement.

## Création de votre application en réservant un nouveau nom

Pour créer une application dans le tableau de bord, vous devez tout d'abord réserver un nom, même si vous n'avez pas encore commencé à la générer. Nous vous recommandons de le faire aussi vite que possible, pour que personne d'autre ne puisse utiliser ce nom.

1.  Depuis la **page de présentation du tableau de bord** ou la page **Toutes les applications**, cliquez sur **Créer une application**.
2.  Dans la zone de texte, entrez le nom à utiliser, puis cliquez sur le lien **Vérifier la disponibilité**. Si ce nom est disponible, une coche verte apparaît. (Si le nom que vous avez entré est déjà réservé ou en cours d'utilisation par un autre développeur, vous obtenez un message indiquant que ce nom n'est pas disponible.)
3.  Cliquez sur **Réserver le nom d’application**.

Ce nom est maintenant réservé pour vous et vous pourrez [soumettre](app-submissions.md) l’application lorsque vous serez prêt.

> **Remarque** Étant donné que les noms peuvent être réservés pendant un an, il se peut que vous ne puissiez pas réserver un nom, même si vous ne voyez aucune application portant ce nom dans le Windows Store. Cela est généralement dû au fait qu’un autre développeur a réservé ce nom, mais n’a pas encore soumis son application. Si vous ne parvenez pas à réserver un nom sur lequel vous détenez un quelconque droit, ou si vous voyez une autre application dans le Windows Store qui utilise ce nom, [contactez Microsoft](http://go.microsoft.com/fwlink/p/?LinkId=233777).

Une fois le nom réservé, vous disposez d'un an pour soumettre l'application. Si vous ne soumettez pas l'application dans ce délai, la réservation expire et un autre développeur peut alors utiliser ce nom pour une autre application. Vous obtenez parfois un message d'erreur si vous tentez de soumettre une application dont vous avez laissé le nom expirer.

## Choix du nom de votre application

Il est important de bien choisir le nom de votre application. Choisissez un nom qui suscitera l'intérêt de vos clients et qui les incitera à en savoir davantage. Vous trouverez ci-dessous quelques conseils pour choisir un nom d'application attractif.

-   **Choisissez un nom court.** L'espace disponible pour afficher le nom de l'application étant souvent limité, nous vous suggérons d'utiliser le nom le plus court possible. Le nom de votre application peut comporter jusqu’à 256caractères, mais les clients ne le verront pas toujours en entier s’il est trop long.

    > **Remarque** Le nombre de caractères réellement affichés à différents emplacements peut varier en fonction de la longueur allouée et des types de caractères utilisés dans le nom de votre application. Par exemple, dans la police Segoe UI utilisée par Windows, environ 30caractères «I» occupent le même espace que 10caractères «W». En raison de cette variation, veillez à tester votre application et à vérifier l'affichage de son nom sur les vignettes (si vous choisissez de superposer le nom de l'application), dans les résultats de recherche et dans l'application elle-même, avant de la soumettre. Pensez également à toutes les langues dans lesquelles vous proposez votre application. N’oubliez pas que les caractères d’Asie de l’Est sont généralement plus larges que les caractères latins, ce qui signifie que moins de caractères seront affichés.

-   **Évitez d’ajouter des informations de différenciation à la fin du nom.** Si vous ajoutez des informations qui différencient plusieurs applications à la fin d’un nom, les clients risquent de passer à côté, surtout si le nom est long. En effet, toutes les applications risquent alors de présenter le même nom. Si vous ne pouvez pas faire autrement, utilisez des logos et images d’application différents pour pouvoir distinguer plus facilement une application d’une autre.
-   **Faites preuve d’originalité.** Assurez-vous que le nom de votre application se distingue pour éviter toute confusion avec une application existante.
-   **N’utilisez pas de noms de marque qui sont la propriété de tiers** Assurez-vous de disposer des droits d’utilisation du nom que vous réservez. Si quelqu’un d’autre a déposé le nom en tant que marque, il peut dénoncer une violation de propriété et vous ne pourrez pas continuer à utiliser ce nom. Si cela se produit après la publication de votre application, celle-ci sera retirée du Windows Store. Vous devez alors modifier le nom de votre application, ainsi que toutes les instances du nom figurant dans celle-ci et son contenu, avant de [soumettre l’application](app-submissions.md) à nouveau pour certification.

## Gérer des noms d’application supplémentaires

Vous pouvez gérer les noms de vos applications sur la page **Gérer les noms d’application** dans la section **Gestion des applications** pour chaque application figurant dans le tableau de bord du Centre de développement Windows.

Dans certains cas, vous pouvez réserver plusieurs noms à utiliser pour une même application, par exemple, lorsque vous voulez proposer celle-ci dans plusieurs langues et utiliser des noms différents. Si vous voulez modifier complètement le nom d’une application, vous devez réserver un nom supplémentaire.

Dans cette page, vous pouvez également supprimer des noms que vous avez réservés mais vous ne voulez plus utiliser.

Pour plus d’informations, voir [Gestion des noms d’application](manage-app-names.md).

 

 







<!--HONumber=Aug16_HO3-->


