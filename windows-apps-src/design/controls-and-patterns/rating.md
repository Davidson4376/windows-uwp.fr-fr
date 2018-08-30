---
author: QuinnRadich
description: Permet aux utilisateurs de visualiser et définir des évaluations qui reflètent la satisfaction vis-à-vis du contenu et des services.
title: Contrôle d’évaluation
template: detail.hbs
ms.author: quradic
ms.date: 10/25/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
pm-contact: abarlow
design-contact: kimsea
dev-contact: mitra
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 242ecdaf128e1e01b1bdeac4cce649504b8efc74
ms.sourcegitcommit: 7efffcc715a4be26f0cf7f7e249653d8c356319b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/30/2018
ms.locfileid: "3112080"
---
# <a name="rating-control"></a>Contrôle d’évaluation

Le contrôle d'évaluation permet aux utilisateurs de visualiser et de définir des évaluations qui reflètent le degré de satisfaction vis-à-vis du contenu et des services. Les utilisateurs peuvent interagir avec le contrôle d'évaluation avec des mouvements tactiles, un stylet, une souris, un boîtier de commande ou un clavier. Les instructions de suivi montre comment utiliser les fonctionnalités du contrôle de l’évaluation pour fournir la flexibilité et la personnalisation.

> **API importantes**: [classe RatingControl](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.ratingcontrol)

![Exemple de contrôle d'évaluation](images/rating_rs2_doc_ratings_intro.png)

## <a name="examples"></a>Exemples

<table>
<th align="left">Galerie de contrôles XAML<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si vous disposez de l'application <strong style="font-weight: semi-bold">Galerie de contrôles XAML</strong>, cliquez ici pour <a href="xamlcontrolsgallery:/item/RatingControl">ouvrir l’application et voir l'objet RatingControl en action</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenir l’application Galerie de contrôles XAML (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Obtenir le code source (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

### <a name="editable-rating-with-placeholder-value"></a>Évaluation modifiable avec valeur d’espace réservé

Le mode d’utilisation le plus courant du contrôle d'évaluation est peut-être l’affichage d’une évaluation moyenne tout en autorisant l’utilisateur à saisir sa propre valeur d’évaluation. Dans ce scénario, le contrôle d'évaluation est initialement défini afin de refléter l’évaluation de la satisfaction moyenne de tous les utilisateurs vis-à-vis d’un service ou d’un type de contenu particulier (morceaux de musique, vidéos, livres, etc.). Il reste dans cet état jusqu’à ce qu’un utilisateur interagisse avec le contrôle dans le but d’évaluer individuellement un élément. Cette interaction modifie l’état du contrôle des évaluations afin de refléter l’évaluation de la satisfaction personnelle de l’utilisateur.

#### <a name="initial-average-rating-state"></a>État initial de l’évaluation moyenne
![État initial de l’évaluation moyenne](images/rating_rs2_doc_movie_aggregate.png)

#### <a name="representation-of-user-rating-once-set"></a>Représentation de l’évaluation de l’utilisateur une fois définie

![Représentation de l’évaluation de l’utilisateur une fois définie](images/rating_rs2_doc_movie_user.png)

```XAML
<RatingControl x:Name="MyRating" ValueChanged="RatingChanged"/>
```

```csharp
private void RatingChanged(RatingControl sender, object args)
{
    if (sender.Value == null)
    {
        MyRating.Caption = "(" + SomeWebService.HowManyPreviousRatings() + ")";
    }

    else
    {
        MyRating.Caption = "Your rating";
    }
}
```

### <a name="read-only-rating-mode"></a>Mode d’évaluation en lecture seule

Parfois, vous devez afficher les évaluations d’un contenu secondaire, par exemple celui qui s’affiche dans le contenu recommandé ou lors de l’affichage d’une liste de commentaires et de leurs évaluations correspondantes. Dans ce cas, l’utilisateur ne doit pas être en mesure de modifier l’évaluation, pour que vous puissiez attribuer le mode de lecture seule à ce contrôle.
Le mode de lecture seule est également le mode d’utilisation recommandé du contrôle d'évaluation, lorsqu’il est utilisé dans de très grandes listes virtualisées de contenu, aussi bien pour la conception de l’interface utilisateur que pour des raisons de performances.

![Liste longue en lecture seule](images/rating_rs2_doc_reviews.png)

Pour ce faire, procédez comme suit:

```XAML
<RatingControl IsReadOnly="True"/>
```

## <a name="additional-functionality"></a>Fonctionnalités supplémentaires

Le contrôle d'évaluation offre de nombreuses fonctionnalités supplémentaires. Vous trouverez des détails concernant l’utilisation de ces fonctionnalités dans notre documentation de référence sur MSDN.
Voici une liste non exhaustive des fonctionnalités supplémentaires:
-   Excellentes performances pour les listes longues
-   Dimensionnement compact pour scénarios d’interface utilisateur réduite
-   Remplissage de valeur et évaluation en continu
-   Personnalisation de l’espacement
-   Désactivation des animations de croissance
-   Personnalisation du nombre d’étoiles

## <a name="get-the-sample-code"></a>Obtenir l’exemple de code

- [Exemples de la Galerie de contrôles XAML](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) - Affichez tous les contrôles XAML dans un format interactif.