---
Description: Développez votre application pour prendre en charge les dispositions et les polices de plusieurs langues, notamment le sens du flux de droite à gauche (DàG).
title: Ajuster la disposition et les polices, et prendre en charge le sens du flux DàG
ms.assetid: F2522B07-017D-40F1-B3C8-C4D0DFD03AC3
label: Adjust layout and fonts, and support RTL
template: detail.hbs
---

# Ajuster la disposition et les polices, et prendre en charge le sens du flux DàG


\[ Mise à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


Développez votre application pour prendre en charge les dispositions et les polices de plusieurs langues, notamment le sens du flux de droite à gauche (DàG).

## <span id="Layout_guidelines"> </span> <span id="layout_guidelines"> </span> <span id="LAYOUT_GUIDELINES"> </span>Recommandations en matière de disposition


Certaines langues, comme l’allemand et le finnois, nécessitent plus d’espace que l’anglais pour leur texte. Les polices de certaines langues, comme le japonais, exigent elles une hauteur supérieure. Et pour certaines langues, telles que l’arabe et l’hébreu, la disposition du texte et de l’application doit être dans le sens de lecture de droite à gauche.

Utilisez des mécanismes de disposition flexibles à la place d’un positionnement absolu, de largeurs fixes ou de hauteurs fixes. Le cas échéant, il est possible d’ajuster des éléments d’interface utilisateur selon la langue.

### <span id="XAML"> </span> <span id="xaml"> </span>XAML

Spécifiez l’**Uid** d’un élément :

```XAML
<TextBlock x:Uid="Block1">
```

Assurez-vous que le fichier ResW de votre application possède une ressource pour Block1.Width, que vous pouvez définir pour chaque langue cible de localisation.

Pour les applications du Windows Store en C++, C# ou Visual Basic, utilisez la propriété [**FlowDirection**](https://msdn.microsoft.com/library/windows/apps/br208716), avec un remplissage et des marges symétriques pour permettre la localisation pour d’autres sens de disposition.

Les contrôles de disposition XAML tels que [**Grid**](https://msdn.microsoft.com/library/windows/apps/br242704) sont mis à l’échelle et pivotent automatiquement avec la propriété [**FlowDirection**](https://msdn.microsoft.com/library/windows/apps/br208716). Exposez votre propre propriété **FlowDirection** dans votre application en tant que ressource pour les traducteurs.

Spécifiez un **Uid** pour la page principale de votre application :

```XAML
<Page x:Uid="MainPage">
```

Assurez-vous que le fichier **ResW** de votre application possède une ressource pour MainPage.FlowDirection, que vous pouvez définir pour chaque langue cible de localisation.

### <span id="HTML"> </span> <span id="html"> </span>HTML

Pour les applications du Windows Store en JavaScript, utilisez les mécanismes de mise en page des [feuilles de style en cascade (CSS)](https://msdn.microsoft.com/library/ms531209), tels que [-ms-grid](https://msdn.microsoft.com/en-us/library/windows/apps/hh465453.aspx#g_section) et [–ms-box](https://msdn.microsoft.com/en-us/library/windows/apps/hh465453.aspx#f_section). Utilisez un remplissage et des marges symétriques pour permettre la localisation quel que soit le sens de la disposition.

Votre application peut également utiliser le sélecteur de pseudo-classe [**:-ms-lang()**](https://msdn.microsoft.com/library/cc848867) pour ajuster des propriétés CSS comme la largeur sur des éléments particuliers selon la langue de l’interface. Pour cela, l’hôte de l’application définit l’attribut **lang** de l’élément racine sur la langue de l’application.

**CSS**
```CSS
.item:-ms-lang(de, fi) { width: 350px; }
```

Le sens de mise en page du corps des applications du Windows Store en JavaScript qui utilisent les feuilles de style ui-light.css ou ui-dark.css est défini automatiquement en fonction de la langue de l’interface. La feuille de style CSS suivante se trouve dans ui-light et ui-dark.css, et vous n’avez pas besoin de l’écrire vous-même.

**CSS**
```CSS
body:-ms-lang(ar,he…) { direction: rtl;}
```

Cela signifie que la plupart des dispositions d’application sont définies correctement lorsque le système utilise une langue qui s’écrit et se lit de droite à gauche.

Comme les contrôles [WinJS.UI](https://msdn.microsoft.com/library/windows/apps/br229782), votre application peut utiliser le sélecteur de pseudo-classe [**:-ms-lang()**](https://msdn.microsoft.com/library/cc848867) pour ajuster les propriétés CSS physiques, telles que **margin** et **padding**. Vous n’avez pas besoin d’ajuster les propriétés CSS logiques qui utilisent des mots-clés tels que **after** et **before**.

N’utilisez pas l’attribut ni la propriété **align** en HTML. À la place, utilisez la propriété **direction** pour contrôler l’alignement de composants particuliers.

Utilisez la propriété [**writing-mode**](https://msdn.microsoft.com/library/ms531187) pour prendre en charge les dispositions de texte verticales dans la feuille de style CSS.

## <span id="Mirroring_images"> </span> <span id="mirroring_images"> </span> <span id="MIRRORING_IMAGES"> </span>Mise en miroir des images


### <span id="XAML"> </span> <span id="xaml"> </span>XAML

Si votre application possède des images qui doivent être mises en miroir (c’est-à-dire qu’une même image peut être renversée) pour une disposition de droite à gauche, vous pouvez appliquer la propriété [**FlowDirection**](https://msdn.microsoft.com/library/windows/apps/br208716) :

```XAML
<!-- en-US\localized.xaml -->
<Image ... FlowDirection="LeftToRight" />

<!-- ar-SA\localized.xaml -->
<Image ... FlowDirection="RightToLeft" />
```

### <span id="HTML"> </span> <span id="html"> </span>HTML

Si votre application comporte des images qui doivent être mises en miroir (c’est-à-dire qu’une même image peut être renversée) pour la disposition de droite à gauche, vous pouvez utiliser des transformations CSS pour mettre en miroir vos images au moment du rendu en ajoutant une classe .mirrorable à vos éléments et en ajoutant la classe CSS suivante :

```CSS
.mirrorable { transform: scaleX(-1); }
```

**Pour XAML et HTML :** si votre application requiert une autre image à faire pivoter correctement, vous pouvez utiliser le système de gestion des ressources avec le [qualificateur layoutdir](https://msdn.microsoft.com/library/windows/apps/xaml/hh965324). Le système choisit une image nommée file.layoutdir-rtl.png lorsque la [langue de l’application](manage-language-and-region.md) est définie sur une langue qui s’écrit et se lit de droite à gauche. Cette approche peut s’avérer nécessaire lorsqu’une certaine partie de l’image est pivotée, alors qu’une autre partie ne l’est pas.

## <span id="Fonts"> </span> <span id="fonts"> </span> <span id="FONTS"> </span>Polices


**Pour XAML et HTML :** utilisez les API de mappage de polices [**LanguageFont**](https://msdn.microsoft.com/library/windows/apps/br206864) pour l’accès par programme à la gamme de polices, à la taille, au poids et au style recommandés pour une langue particulière. L’objet **LanguageFont** assure l’accès aux informations de police appropriées pour diverses catégories de contenu, notamment les en-têtes d’interface utilisateur, les notifications, le texte de corps et les polices de corps de document modifiables par l’utilisateur.

### <span id="HTML"> </span> <span id="html"> </span>HTML

Les applications du Windows Store en JavaScript qui utilisent les feuilles de style ui-light.css ou ui-dark.css ont leur police définie automatiquement sur la police la plus appropriée en fonction de la langue de l’application. L’hôte d’application définit l’attribut **lang** de l’élément racine sur la langue de l’application.

Les applications qui affichent plusieurs langues sur une même page doivent définir l’attribut **lang** pour chaque section dans une langue différente. Le sélecteur de pseudo-classe [**:-ms-lang()**](https://msdn.microsoft.com/library/cc848867) sélectionne la police appropriée pour chaque section de la page.

 

 





<!--HONumber=Mar16_HO4-->


