---
Description: La première étape de création d’une application dans le centre partenaires est réservation d’un nom de l’application. Découvrez comment réserver des noms d’application et bénéficiez de suggestions pour choisir un nom exceptionnel pour votre application.
title: Créer votre application en réservant un nom
keywords: windows 10, uwp, réservation de noms, nom de l’application, noms d’application, noms, nom du produit, attribution de noms, nom réservé, titre, noms, titres
ms.assetid: 6DC58A9A-DF47-4652-8D13-0AC9289F5950
ms.date: 10/31/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: e1cb52bdb579f2ce4cda28440c0b820329173a3e
ms.sourcegitcommit: fca0132794ec187e90b2ebdad862f22d9f6c0db8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/24/2019
ms.locfileid: "63807373"
---
# <a name="create-your-app-by-reserving-a-name"></a>Créer votre application en réservant un nom

La première étape de création d’une nouvelle application dans [partenaires](https://partner.microsoft.com/dashboard) réserve un nom d’application. Chaque nom réservé (parfois appelé *titre* de votre application) doit être unique partout dans le Microsoft Store.

Vous pouvez réserver un nom pour votre application même si vous n'avez pas encore commencé à la générer. Nous vous recommandons de faire dès que possible, afin que personne d’autre ne puisse utiliser le nom. Notez que vous devrez soumettre l’application dans les trois mois afin de conserver ce nom réservé pour votre usage.

Lorsque vous [chargez les packages de votre application](upload-app-packages.md), la valeur [**Package/Properties/DisplayName**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-displayname) doit correspondre au nom que vous avez réservé pour votre application. Si vous utilisez Microsoft Visual Studio pour créer le package de votre application, cet attribut est rempli automatiquement.

> [!IMPORTANT]
> Vous pouvez réserver des noms supplémentaires pour une application, et vous pouvez choisir d’utiliser une de celles de la version publiée de votre application au lieu de celle que vous réservez lorsque vous créez tout d’abord votre application dans le centre de partenaires. Toutefois, n’oubliez pas que le premier nom que vous entrez ici servira dans la partie de votre application [détails de l’identité](view-app-identity-details.md), telles que la **nom de famille de packages (NFP)**. Ces valeurs peuvent être visibles à certains utilisateurs et ne peut pas être modifié, par conséquent, assurez-vous que le nom que vous réservez est approprié pour cette utilisation.


## <a name="create-your-app-by-reserving-a-new-name"></a>Création de votre application en réservant un nouveau nom

Réservation d’un nom est la première étape de création d’une application dans le centre de partenaires. 

1.  Sur la page **Vue d’ensemble**, cliquez sur **Créer une application**.
2.  Dans la zone de texte, entrez le nom à utiliser, puis sélectionnez **Vérifier la disponibilité**. Si ce nom est disponible, une coche verte apparaît. (Si le nom que vous avez entré est déjà réservé ou en cours d'utilisation par un autre développeur, vous obtenez un message indiquant que ce nom n'est pas disponible.)
3.  Cliquez sur **Reserve product name**.

Ce nom est maintenant réservé pour vous et vous pourrez [soumettre](app-submissions.md) l’application lorsque vous serez prêt. 

> [!NOTE]
> Il est possible que vous ne puissiez pas réserver un nom, même si vous ne voyez aucune application portant ce nom dans le Microsoft Store. Cela est généralement dû au fait qu’un autre développeur a réservé ce nom, mais n’a pas encore soumis son application. Si vous ne parvenez pas à réserver un nom sur lequel vous détenez un quelconque droit, ou si vous voyez une autre application dans le Microsoft Store qui utilise ce nom, [contactez Microsoft](https://go.microsoft.com/fwlink/p/?LinkId=233777).

Une fois le nom réservé, vous disposez de trois mois pour soumettre cette application. Si vous ne soumettez pas l’application dans ce délai, la réservation du nom expire et un autre développeur peut alors être en mesure d’utiliser ce nom pour une autre application. Vous obtenez parfois un message d'erreur si vous tentez de soumettre une application dont vous avez laissé le nom expirer.


## <a name="choosing-your-apps-name"></a>Choix du nom de votre application

Il est important de bien choisir le nom de votre application. Choisissez un nom qui suscitera l'intérêt de vos clients et qui les incitera à en savoir davantage. Vous trouverez ci-dessous quelques conseils pour choisir un nom d'application attractif.

-   **Gardez-le court.** L'espace disponible pour afficher le nom de l'application étant souvent limité, nous vous suggérons d'utiliser le nom le plus court possible. Le nom de votre application peut comporter jusqu’à 256 caractères, mais les clients ne le verront pas toujours en entier s’il est trop long.
    > [!NOTE]
    > Le nombre de caractères réellement affichés à différents emplacements peut varier en fonction de la longueur allouée et des types de caractères utilisés dans le nom de votre application. Par exemple, dans la police Segoe UI utilisée par Windows, environ 30 caractères « I » occupent le même espace que 10 caractères « W ». En raison de cette variation, veillez à tester votre application et vérifier la façon dont son nom apparaît sur ses mosaïques (si vous choisissez de superposer le nom de l’application) dans les résultats de la recherche et au sein de l’application elle-même. Pensez également à toutes les langues dans lesquelles vous proposez votre application. N’oubliez pas que les caractères d’Asie de l’Est sont généralement plus larges que les caractères latins, ce qui signifie que moins de caractères seront affichés.
-   **Être d’origine.** Assurez-vous que le nom de votre application se distingue pour éviter toute confusion avec une application existante.
-   **N’utilisez pas les noms marques par d’autres.** Assurez-vous de disposer des droits d’utilisation du nom que vous réservez. Si quelqu’un d’autre a déposé le nom en tant que marque, il peut dénoncer une violation de propriété et vous ne pourrez pas continuer à utiliser ce nom. Si cela se produit après la publication de votre application, celle-ci sera retirée du Windows Store. Vous devez alors modifier le nom de votre application, ainsi que toutes les instances du nom figurant dans celle-ci et son contenu, avant de [soumettre l’application](app-submissions.md) à nouveau pour certification.
-   **Évitez d’ajouter des informations de différenciation à la fin du nom.** Si vous ajoutez des informations qui différencient plusieurs applications à la fin d’un nom, les clients risquent de passer à côté, surtout si le nom est long. En effet, toutes les applications risquent alors de présenter le même nom. Si c’est inévitable, vous devez utiliser différents logos et images d’application pour le rendre plus facile de différencier une application à partir d’un autre.
-   **N’incluez pas emojis dans votre nom.** Vous ne serez pas en mesure de réserver un nom qui inclut des emojis ou d’autres caractères non pris en charge.


## <a name="manage-additional-app-names"></a>Gérer des noms d’application supplémentaires

Vous pouvez ajouter et gérer les noms supplémentaires sur le **gérer les noms d’application** page dans le **gestion des applications** section pour chacun de vos applications dans le centre de partenaires.

Dans certains cas, vous pouvez réserver plusieurs noms à utiliser pour une même application, par exemple lorsque vous voulez proposer cette dernière dans plusieurs langues et utiliser des noms différents pour chaque langue. Si vous voulez modifier complètement le nom d’une application, vous devez réserver un nom supplémentaire.

Dans cette page, vous pouvez également supprimer des noms que vous avez réservés mais vous ne voulez plus utiliser.

Pour plus d’informations, voir [Gestion des noms d’application](manage-app-names.md).

 

 




