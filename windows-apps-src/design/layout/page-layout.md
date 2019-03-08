---
title: Mise en page pour les applications UWP
description: Lorsque vous concevez votre application, la première chose à prendre en considération est la structure de disposition. Cet article traite de la structure commune de dispositions de page de base, y compris les éléments d’interface utilisateur, vous aurez besoin, et où ils vont sur une page. Dans les applications UWP, chaque page possède généralement les éléments de contenu, de commande et de navigation.
ms.date: 03/19/2018
ms.topic: article
keywords: windows 10, uwp
localizationpriority: medium
ms.openlocfilehash: edda9948e70b1757ddb46fae45a579bb2fdb8de1
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57633714"
---
# <a name="page-layout"></a>Mise en page

Dans les applications UWP, chaque [ **Page** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Page) a généralement des éléments de contenu, de commande et de navigation. 

Votre application peut avoir plusieurs pages : lorsqu’un utilisateur lance une application UWP, le code d’application crée un [ **Frame** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Frame) à placer à l’intérieur de l’application [ **fenêtre** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.window). L’image peut ensuite [accédez](../basics/navigate-between-two-pages.md) entre l’application [ **Page** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Page) instances. 

La plupart des pages suivez une structure commune de la mise en page, et cet article couvre le UI éléments que vous avez besoin, et où ils vont sur une page. 

![structure de page](images/page-components.svg)

## <a name="navigation"></a>Navigation
Disposition de votre application commence par le modèle de navigation que vous choisissez, qui définit comment vos utilisateurs naviguent entre les pages dans votre application. Pour cet article, nous allons aborder deux modèles de navigation courantes : gauche nav et le volet de navigation supérieur. Pour obtenir des conseils sur le choix d’autres options de navigation, consultez [principes fondamentaux de conception de Navigation pour les applications UWP](../basics/navigation-basics.md).

![modèles de navigation supérieure et gauche](images/top-left-nav.svg)

### <a name="left-nav"></a>Navigation de gauche
Gauche, ou le [volet de navigation](../controls-and-patterns/navigationview.md) de modèle, est généralement réservé pour la navigation au niveau de l’application et existe au niveau le plus élevé au sein de l’application, ce qui signifie qu’il doit toujours être visible et accessible. Nous vous recommandons de navigation de gauche lorsqu’il y a plus de cinq éléments de navigation, ou plus de cinq pages de votre application. Le modèle de volet de navigation contient généralement :
- Éléments de navigation
- Point d’entrée dans les paramètres de l’application
- Point d’entrée dans les paramètres de compte

Le [NavigationView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview) contrôle implémente le modèle de navigation de gauche pour UWP.

Lorsqu’un élément de navigation est sélectionné, le Frame doit accéder à la Page de l’élément sélectionné.

![volet de navigation développé](images/navview-expanded.svg)

Le bouton de menu permet aux utilisateurs de développer et réduire le volet de navigation. Lorsque la taille d’écran est supérieure à 640 px, en cliquant sur le bouton de menu réduit le volet de navigation dans une barre.

![volet de navigation compact](images/navview-compact.svg)

Lorsque la taille d’écran est inférieure à 640 px, le volet de navigation est entièrement réduits.

![volet de navigation minimale](images/navview-minimal.svg)

### <a name="top-nav"></a>Navigation supérieure

Navigation supérieure peut également agir en tant que la navigation de niveau supérieur. Bien que la navigation de gauche est réductible, navigation supérieure est toujours visible. Le [NavigationView](../controls-and-patterns/navigationview.md) contrôle implémente le modèle de navigation et des onglets principaux pour UWP.

![navigation en haut](images/pivot-large.svg)

## <a name="command-bar"></a>Barre de commandes

Ensuite, vous souhaiterez peut-être fournir aux utilisateurs un accès facile aux tâches les plus courantes de votre application. Un [barre de commandes](../controls-and-patterns/app-bars.md) permet d’accéder aux commandes de niveau de l’application ou au niveau des pages, et il peut être utilisé avec n’importe quel modèle de navigation.

![emplacement en haut de la barre de commande ](images/app-bar-desktop.svg)

La barre de commandes peut être placée en haut ou bas de la page, selon ce qui convient le mieux à votre application.

![emplacement en bas de la barre de commande](images/app-bar-mobile.svg)

## <a name="content"></a>Contenu

Enfin, le contenu varie largement à partir d’une application vers, donc vous pouvez présenter le contenu de différentes manières. Ici, nous décrivons quelques modèles courants de page que vous pouvez utiliser dans votre application. De nombreuses applications utilisent tout ou partie de ces modèles de page pour afficher différents types de contenu. De même, n’hésitez pas à les combiner et associer ces modèles afin d’optimiser votre application.

## <a name="landing"></a>Accueil

![page d’accueil](images/hero-screen.svg)

Les pages d’accueil, également connues sous le nom d’écrans bannière, apparaissent souvent au niveau supérieur d’une expérience applicative. La grande surface sert à exposer les applications, et plus particulièrement à mettre en avant le contenu que les utilisateurs peuvent vouloir parcourir et utiliser.

## <a name="collections"></a>Collections

![galerie](images/gridview.svg)

Les collections permettent aux utilisateurs de parcourir les groupes de contenu ou de données. L’[affichage Grille](../controls-and-patterns/item-templates-gridview.md) est idéal pour les photos ou les contenus centrés sur les médias ; le [mode Liste](../controls-and-patterns/item-templates-listview.md) est, quant à lui, plus particulièrement destiné aux données ou aux contenus riches en texte.

## <a name="masterdetail"></a>Maître/Détail

![maître et détails](images/master-detail.svg)

Le modèle [maître/détails](../controls-and-patterns/master-details.md) se compose du mode Liste (maître) et d’une vue de contenu (détail). Ces deux volets sont figés et offrent un défilement vertical. Lorsqu’un élément dans la vue de liste est sélectionné, l’affichage du contenu est à jour en conséquence. 

## <a name="forms"></a>Formulaires
![formulaire](images/form.svg)

Un [formulaire](../controls-and-patterns/forms.md) est un groupe de commandes qui collectent et envoient les données des utilisateurs. La plupart des applications, voire toutes, utilisent un formulaire pour les pages de paramètres, les portails de connexion, les hubs de commentaires, la création de compte, etc. 

## <a name="sample-apps"></a>Exemples d’applications
Pour voir comment ces modèles peuvent être implémentés, consultez notre [exemples d’applications UWP](https://developer.microsoft.com/en-us/windows/samples):
- [Lecteur vidéo de BuildCast](https://github.com/Microsoft/BuildCast)
- [Planificateur de déjeuners](https://github.com/Microsoft/Windows-appsample-lunch-scheduler)
- [Livre de coloriage](https://github.com/Microsoft/Windows-appsample-coloringbook)
- [Base de données des commandes de clients](https://github.com/Microsoft/Windows-appsample-customers-orders-database)