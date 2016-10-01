---
author: jwmsft
description: "Un namescope XAML stocke les relations entre les noms définis en XAML des objets et leurs instances équivalentes. Ce concept est similaire à celui dont la signification plus large correspond au terme namescope dans d’autres langages et technologies de programmation."
title: Namescopes XAML
ms.assetid: EB060CBD-A589-475E-B83D-B24068B54C21
translationtype: Human Translation
ms.sourcegitcommit: 98b9bca2528c041d2fdfc6a0adead321737932b4
ms.openlocfilehash: 34ef0bf246abe49a5e19adef66bddda7004a3441

---

# Namescopes XAML

\[ Article mis à jour pour les applications UWP sur Windows10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

Un *namescope XAML* stocke les relations entre les noms définis en XAML des objets et leurs instances équivalentes. Ce concept est similaire à celui dont la signification plus large correspond au terme *namescope* dans d’autres langages et technologies de programmation.

## Mode de définition des namescopes XAML

Les noms dans les namescopes XAML permettent au code utilisateur de référencer les objets qui ont été initialement déclarés en XAML. Le résultat interne de l’analyse du code XAML est que l’exécution crée un ensemble d’objets qui conservent une partie ou la totalité des relations que ces objets entretenaient dans les déclarations XAML. Ces relations sont gérées en tant que propriétés d’objets spécifiques des objets créés ou sont exposées aux méthodes utilitaires dans les API du modèle de programmation.

L’utilisation la plus fréquente d’un nom dans un namescope XAML est en tant que référence directe à une instance d’objet, laquelle est activée par une passe du compilateur du balisage en tant qu’action de génération de projet, associée à une méthode **InitializeComponent** générée dans les modèles de classe partielle.

Vous pouvez également utiliser la méthode utilitaire [**FindName**](https://msdn.microsoft.com/library/windows/apps/br208715) vous-même au moment de l’exécution pour renvoyer une référence à des objets qui ont été définis avec un nom dans le balisage XAML.

### Plus d’informations sur les actions de génération et le langage XAML

Ce qui se passe techniquement, c’est que le code XAML lui-même subit une passe du compilateur de balisage en même temps que la classe partielle qu’il définit pour le code-behind et lui-même sont compilés ensemble. Chaque élément objet avec un attribut **Name** ou [x:Name](x-name-attribute.md) défini dans le balisage génère un champ interne portant un nom qui correspond au nom XAML. Ce champ est initialement vide. Ensuite, la classe génère une méthode **InitializeComponent** qui est appelée uniquement à l’issue du chargement de tout le code XAML. Au sein de la logique **InitializeComponent**, chaque champ interne est alors renseigné à l’aide de la valeur de retour [**FindName**](https://msdn.microsoft.com/library/windows/apps/br208715) de la chaîne de nom équivalente. Vous pouvez observer cette infrastructure à titre personnel en examinant les fichiers «.g» (générés) créés pour chaque page XAML dans le sous-dossier /obj d’un projet d’application Windows Runtime à l’issue de la compilation. Vous pouvez également considérer les champs et la méthode **InitializeComponent** comme des membres de vos assemblys obtenus en y réfléchissant ou en examinant le contenu linguistique de leur interface.

**Remarque** Plus précisément, pour les applications utilisant les extensions de composant Visual C++ (C++/CX), aucun champ de sauvegarde d’une référence **x:Name** n’est créé pour l’élément racine d’un fichier XAML. Si vous devez référencer l’objet racine à partir d’un fichier code-behind C++/CX, utilisez d’autres API ou une traversée d’arborescence. Par exemple, vous pouvez appeler [**FindName**](https://msdn.microsoft.com/library/windows/apps/br208715) pour un élément enfant nommé connu avant d’appeler [**Parent**](https://msdn.microsoft.com/library/windows/apps/br208739).

## Création d’objets au moment de l’exécution avec XamlReader.Load

Le code XAML peut également servir d’entrée de chaîne pour la méthode [**XamlReader.Load**](https://msdn.microsoft.com/library/windows/apps/br228048), qui agit de façon analogue à l’opération d’analyse initiale de la source XAML. **XamlReader.Load**crée une arborescence déconnectée d’objets au moment de l’exécution. Cette arborescence déconnectée peut ensuite être attachée à un certain point de l’arborescence d’objets principale. Vous devez connecter de manière explicite votre arborescence d’objets créée, soit en l’ajoutant à une collection de propriétés de contenu telle que **Children**, soit en définissant une autre propriété qui prend une valeur d’objet (par exemple en chargeant une nouvelle classe [**ImageBrush**](https://msdn.microsoft.com/library/windows/apps/br210101) pour une valeur de propriété [**Fill**](https://msdn.microsoft.com/library/windows/apps/br243378)).

### Implications du namescope XAML de XamlReader.Load

Le namescope XAML préliminaire défini par la nouvelle arborescence d’objets créée par [**XamlReader.Load**](https://msdn.microsoft.com/library/windows/apps/br228048) évalue tous les noms définis dans le code XAML fourni quant à leur unicité. S’il existe des noms dans le code XAML fourni qui ne sont pas uniques en interne à ce stade, **XamlReader.Load** lève une exception. L’arborescence d’objets déconnectée n’essaie pas de fusionner son namescope XAML avec le namescope XAML de l’application principale, si ou lorsqu’elle est connectée à l’arborescence d’objets de l’application principale. Après avoir connecté les arborescences, votre application possède une arborescence d’objets unifiée, mais celle-ci comporte des namescopes XAML discrets. Les divisions ont lieu aux points de connexion entre les objets, où vous définissez une propriété en tant que valeur renvoyée à partir d’un appel **XamlReader.Load**.

Ce qui est compliqué dans le fait d’avoir des namescopes XAML discrets et déconnectés, c’est que les appels de la méthode [**FindName**](https://msdn.microsoft.com/library/windows/apps/br208715) et les références d’objets managés directes ne fonctionnent plus par rapport à un namescope XAML unifié. Au contraire, l’objet particulier sur lequel la méthode **FindName** est appelée implique l’étendue, en sachant que l’étendue correspond au namescope XAML dans lequel se trouve l’objet appelant. Dans le cas d’une référence d’objet managé directe, l’étendue est impliquée par la classe dans laquelle le code existe. En général, le code-behind d’interaction de l’exécution d’une « page » de contenu d’application existe dans la classe partielle qui stocke la « page » racine ; par conséquent, le namescope XAML est le namescope XAML racine.

Si vous appelez [**FindName**](https://msdn.microsoft.com/library/windows/apps/br208715) pour obtenir un objet nommé dans le namescope XAML racine, la méthode de trouve pas les objets provenant d’un namescope XAML discret créé par [**XamlReader.Load**](https://msdn.microsoft.com/library/windows/apps/br228048). Inversement, si vous appelez **FindName** à partir d’un objet obtenu depuis l’extérieur du namescope XAML discret, la méthode ne trouve pas les objets nommés dans le namescope XAML racine.

Ce problème de namescope XAML discret affecte uniquement la recherche d’objets par nom dans des namescopes XAML dans le cadre de l’utilisation de l’appel [**FindName**](https://msdn.microsoft.com/library/windows/apps/br208715).

Pour obtenir les références à des objets définis dans un autre namescope XAML, vous pouvez utiliser plusieurs techniques :

-   Parcourez toute l’arborescence par étapes discrètes avec la propriété [**Parent**](https://msdn.microsoft.com/library/windows/apps/br208739) et/ou les propriétés de collection connues pour exister dans la structure de votre arborescence d’objets (telle que la collection renvoyée par [**Panel.Children**](https://msdn.microsoft.com/library/windows/apps/br227514)).
-   Si vous appelez depuis un namescope XAML discret alors que vous souhaitez le namescope XAML racine, il est toujours facile d’obtenir une référence à la fenêtre principale actuellement affichée. Vous pouvez obtenir la racine visuelle (l’élément XAML racine, également connu sous le nom de source de contenu) depuis la fenêtre d’application actuelle dans une seule ligne de code à l’aide de l’appel `Window.Current.Content`. Vous pouvez alors effectuer un transtypage vers [**FrameworkElement**](https://msdn.microsoft.com/library/windows/apps/br208706) et appeler [**FindName**](https://msdn.microsoft.com/library/windows/apps/br208715) depuis cette étendue.
-   Si vous appelez depuis le namescope XAML racine et souhaitez obtenir un objet au sein d’un namescope XAML discret, la meilleure chose à faire consiste à le prévoir dans votre code en conservant une référence à l’objet qui a été renvoyé par [**XamlReader.Load**](https://msdn.microsoft.com/library/windows/apps/br228048), puis ajouté à l’arborescence d’objets principale. Cet objet est maintenant un objet valide pour appeler [**FindName**](https://msdn.microsoft.com/library/windows/apps/br208715) au sein du namescope XAML discret. Vous pouvez garder cet objet disponible en tant que variable globale ou bien la passer à l’aide de paramètres de méthode.
-   Vous pouvez totalement éviter les considérations liées aux noms et aux namescopes XAML en examinant l’arborescence visuelle. L’API [**VisualTreeHelper**](https://msdn.microsoft.com/library/windows/apps/br243038) vous permet de balayer l’arborescence visuelle en termes d’objets parents et de collections enfants, purement selon la position et de l’index.

## Namescopes XAML dans les modèles

Les modèles en langage XAML offrent la possibilité de réutiliser et réappliquer du contenu de manière directe, mais les modèles peuvent également inclure des éléments avec des noms définis au niveau du modèle. Ce même modèle peut servir plusieurs fois dans une page. C’est pourquoi les modèles définissent leurs propres namescopes XAML, indépendamment de la page qui les contient dans laquelle le style ou modèle est appliqué. Examinez cet exemple :

```xml
<Page
  xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation" 
  xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"  >
  <Page.Resources>
    <ControlTemplate x:Key="MyTemplate">
      ....
      <TextBlock x:Name="MyTextBlock" />
    </ControlTemplate>
  </Page.Resources>
  <StackPanel>
    <SomeControl Template="{StaticResource MyTemplate}" />
    <SomeControl Template="{StaticResource MyTemplate}" />
  </StackPanel>
</Page>
```

Ici, le même modèle est appliqué à deux contrôles différents. Si les modèles n’avaient pas de namescopes XAML discrets, le nom «MyTextBlock» utilisé dans le modèle entraînerait un conflit de noms. Chaque instanciation du modèle possède son propre namescope XAML, si bien que dans cet exemple, le namescope XAML de chaque modèle instancié contiendrait exactement un seul nom. Toutefois, le namescope XAML racine ne contient pas le nom de l’un ou l’autre des modèles.

En raison des namescopes XAML distincts, la recherche d’éléments nommés au sein d’un modèle à partir de l’étendue de la page dans laquelle le modèle est appliqué requiert une autre technique. Au lieu d’appeler [**FindName**](https://msdn.microsoft.com/library/windows/apps/br208715) sur un objet dans l’arborescence d’objets, vous obtenez d’abord l’objet auquel le modèle est appliqué, puis vous appelez [**GetTemplateChild**](https://msdn.microsoft.com/library/windows/apps/br209416). Si vous êtes l’auteur d’un contrôle et que vous générez une convention selon laquelle un élément nommé particulier dans un modèle appliqué est la cible d’un comportement défini par le contrôle lui-même, vous pouvez utiliser la méthode **GetTemplateChild** de votre code d’implémentation du contrôle. La méthode **GetTemplateChild** est protégée, donc seul l’auteur du contrôle y a accès. En outre, il existe des conventions que les auteurs de contrôles doivent suivre afin de nommer des composants et des composants de modèles pour signaler ces derniers comme des valeurs d’attribut appliquées à la classe des contrôles. Cette technique rend détectables les noms des composants importants par les utilisateurs des contrôles, susceptibles de vouloir appliquer un autre modèle, ce qui impliquerait de remplacer les composants nommés afin de conserver les fonctionnalités des contrôles.

## Rubriques connexes

* [Vue d’ensemble du langage XAML](xaml-overview.md)
* [Attribut x:Name](x-name-attribute.md)
* [Démarrage rapide: modèles de contrôles](https://msdn.microsoft.com/library/windows/apps/xaml/hh465374)
* [**XamlReader.Load**](https://msdn.microsoft.com/library/windows/apps/br228048)
* [**FindName**](https://msdn.microsoft.com/library/windows/apps/br208715)
 




<!--HONumber=Aug16_HO3-->


