---
author: DelfCo
Description: "Développez votre application pour prendre en charge les dispositions et les polices de plusieurs langues, notamment le sens du flux de droite à gauche (DàG)."
title: "Ajuster la disposition et les polices, et prendre en charge le sens du flux DàG"
ms.assetid: F2522B07-017D-40F1-B3C8-C4D0DFD03AC3
label: Adjust layout and fonts, and support RTL
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: a3924fef520d7ba70873d6838f8e194e5fc96c62
ms.openlocfilehash: 05208607b148a48e8a680691de90161feeea7700

---

# <a name="adjust-layout-and-fonts-and-support-rtl"></a>Ajuster la disposition et les polices, et prendre en charge le sens du flux DàG
<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

Développez votre application pour prendre en charge les dispositions et les polices de plusieurs langues, notamment le sens du flux de droite à gauche (DàG).

## <a name="layout-guidelines"></a>Recommandations en matière de disposition


Certaines langues, comme l’allemand et le finnois, nécessitent plus d’espace que l’anglais pour leur texte. Les polices de certaines langues, comme le japonais, exigent elles une hauteur supérieure. Et pour certaines langues, telles que l’arabe et l’hébreu, la disposition du texte et de l’application doit être dans le sens de lecture de droite à gauche.

Utilisez des mécanismes de disposition flexibles à la place d’un positionnement absolu, de largeurs fixes ou de hauteurs fixes. Le cas échéant, il est possible d’ajuster des éléments d’interface utilisateur selon la langue.

Spécifiez l’**Uid** d’un élément :

```XML
<TextBlock x:Uid="Block1">
```

Assurez-vous que le fichier ResW de votre application possède une ressource pour Block1.Width, que vous pouvez définir pour chaque langue cible de localisation.

Pour les applications du Windows Store en C++, C# ou Visual Basic, utilisez la propriété [**FlowDirection**](https://msdn.microsoft.com/library/windows/apps/br208716), avec un remplissage et des marges symétriques pour permettre la localisation pour d’autres sens de disposition.

Les contrôles de disposition XAML tels que [**Grid**](https://msdn.microsoft.com/library/windows/apps/br242704) sont mis à l’échelle et pivotent automatiquement avec la propriété [**FlowDirection**](https://msdn.microsoft.com/library/windows/apps/br208716). Exposez votre propre propriété **FlowDirection** dans votre application en tant que ressource pour les outils de localisation.

Spécifiez un **Uid** pour la page principale de votre application :

```XML
<Page x:Uid="MainPage">
```

Assurez-vous que le fichier **ResW** de votre application possède une ressource pour MainPage.FlowDirection, que vous pouvez définir pour chaque langue cible de localisation.


## <a name="mirroring-images"></a>Mise en miroir des images

Si votre application possède des images qui doivent être mises en miroir (c’est-à-dire qu’une même image peut être renversée) pour une disposition de droite à gauche, vous pouvez appliquer la propriété [**FlowDirection**](https://msdn.microsoft.com/library/windows/apps/br208716) :

```XML
<!-- en-US\localized.xaml -->
<Image ... FlowDirection="LeftToRight" />

<!-- ar-SA\localized.xaml -->
<Image ... FlowDirection="RightToLeft" />
```


si votre application nécessite une autre image à faire pivoter correctement, vous pouvez utiliser le système de gestion des ressources avec le [qualificateur layoutdir](https://msdn.microsoft.com/library/windows/apps/xaml/hh965324). Le système choisit une image nommée file.layoutdir-rtl.png lorsque la [langue de l’application](manage-language-and-region.md) est définie sur une langue qui s’écrit et se lit de droite à gauche. Cette approche peut s’avérer nécessaire lorsqu’une certaine partie de l’image est pivotée, alors qu’une autre partie ne l’est pas.

## <a name="fonts"></a>Polices

Utilisez les API de mappage de polices [**LanguageFont**](https://msdn.microsoft.com/library/windows/apps/br206864) pour permettre l’accès par programmation à la famille de polices et aux attributs de police (taille, épaisseur et style) recommandés pour une langue. L’objet **LanguageFont** assure l’accès aux informations de police appropriées pour diverses catégories de contenu, notamment les en-têtes d’interface utilisateur, les notifications, le texte de corps et les polices de corps de document modifiables par l’utilisateur.

## <a name="best-practices-for-handling-right-to-left-rtl-languages"></a>Bonnes pratiques en matière de gestion de langues se lisant de droite à gauche (DàG)

Quand votre application est traduite dans des langues se lisant de droite à gauche (DàG), utilisez les API pour définir l’orientation de texte par défaut pour l’élément RootFrame. De cette façon, tous les contrôles contenus dans l’élément RootFrame répondront correctement à l’orientation de texte par défaut.  Quand plusieurs langues sont prises en charge, utilisez l’élément LayoutDirection de la langue préférée pour définir la propriété FlowDirection. La plupart des contrôles inclus dans Windows utilisent déjà FlowDirection. Si vous implémentez des contrôles personnalisés, ils doivent utiliser FlowDirection pour apporter les modifications de disposition appropriées pour les langues DàG et GàD.

C#
```csharp    
// For bidirectional languages, determine flow direction for RootFrame and all derived UI.

    string resourceFlowDirection = ResourceContext.GetForCurrentView().QualifierValues["LayoutDirection"];
    if (resourceFlowDirection == "LTR")
    {
       RootFrame.FlowDirection = FlowDirection.LeftToRight;
    }
    else
    {
       RootFrame.FlowDirection = FlowDirection.RightToLeft;
    }
```

C++ :
```
    // Get preferred app language
    m_language = Windows::Globalization::ApplicationLanguages::Languages->GetAt(0);
     
    // Set flow direction accordingly
    m_flowDirection = ResourceManager::Current->DefaultContext->QualifierValues->Lookup("LayoutDirection") != "LTR" ? 
       FlowDirection::RightToLeft : FlowDirection::LeftToRight;
```


### <a name="rtl-faq"></a>FAQ sur les langues DàG 

<dl>
  <dt> <p><b>Q :</b> La propriété <b>FlowDirection</b> est-elle automatiquement définie en fonction de la langue sélectionnée ? Par exemple, si la langue sélectionnée est l’anglais, le texte s’affiche-t-il de gauche à droite ? Inversement, si l’arabe est sélectionné, s’affiche-t-il de droite à gauche ?</p></dt>

  <dd><p><b>R : </b> <b>FlowDirection</b> ne prend pas en compte la langue. Vous devez définir <b>FlowDirection</b> en fonction de la langue active. Consultez l’exemple de code ci-dessus.</p></dd> 

  <dt> <p><b>Q :</b> Je ne suis pas expert en traduction. Les ressources contiennent-elles déjà le sens du déroulement ? Est-il possible de déterminer le sens du déroulement à partir de la langue active ?</p></dt>

  <dd> <p><b>R :</b> Si vous mettez en application les bonnes pratiques actuelles, les ressources ne contiennent pas directement le sens du déroulement. Vous devez déterminer le sens du déroulement de la langue active. Voici deux façons d’y remédier : </p>
   <p>La méthode à privilégier consiste à utiliser l’élément LayoutDirection pour la langue préférée de façon à définir la propriété FlowDirection de l’élément RootFrame. Tous les contrôles de l’élément RootFrame héritent de la propriété FlowDirection de l’élément RootFrame.</p>
   <p>L’autre méthode consiste à définir la propriété FlowDirection dans le fichier resw pour les langues DàG que vous traduisez. Par exemple, vous pouvez avoir un fichier resw arabe et un fichier resw hébreu. Dans ces fichiers, vous pouvez utiliser x:UID pour définir la propriété FlowDirection. Cette méthode est cependant plus exposée aux erreurs que la méthode par programmation.</p></dd>
</dl>


## <a name="related-topics"></a>Rubriques connexes
[FlowDirection](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.frameworkelement.flowdirection.aspx)



<!--HONumber=Dec16_HO2-->


