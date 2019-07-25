---
description: Cette rubrique vous guide dans le processus d’ajout d’une prise en charge simple de WinUI dans un projet C++/WinRT.
title: Exemple de bibliothèque d’IU Windows C++/WinRT simple
ms.date: 07/12/2019
ms.topic: article
keywords: windows 10, uwp, standard, c++, cpp, winrt, Windows UI Library, WinUI
ms.localizationpriority: medium
ms.openlocfilehash: 082e7ca0684495e1f67c2fa79b448866f68a059c
ms.sourcegitcommit: cba3ba9b9a9f96037cfd0e07d05bd4502753c809
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/14/2019
ms.locfileid: "67870344"
---
# <a name="a-simple-cwinrt-windows-ui-library-example"></a>Exemple de bibliothèque d’IU Windows C++/WinRT simple

Cette rubrique vous guide dans le processus d’ajout d’une prise en charge simple de la bibliothèque d’IU Windows (WinUI) dans un projet C++/WinRT.

> [!NOTE]
> Le kit d’outils de la bibliothèque d’IU Windows (WinUI) est disponible sous forme de packages NuGet que vous pouvez ajouter à un projet existant ou nouveau à l’aide de Visual Studio, comme nous le verrons dans cette rubrique. Pour plus d’informations sur l’arrière-plan, la configuration et la prise en charge, consultez [Prise en main de la bibliothèque d’IU Windows](/uwp/toolkits/winui/getting-started).

## <a name="create-a-blank-app-hellowinuicppwinrt"></a>Créer une application vide (HelloWinUICppWinRT)

Dans Visual Studio, créez un projet à l’aide du modèle de projet **Blank App (C++/WinRT)** , puis nommez-le *HelloWinUICppWinRT*.

## <a name="install-the-microsoftuixaml-nuget-package"></a>Installer le package NuGet Microsoft.UI.Xaml

Cliquez sur **Projet** \> **Gérer des packages NuGet...** \> **Parcourir**, saisissez ou collez **Microsoft.UI.Xaml** dans la zone de recherche, sélectionnez l’élément dans les résultats de la recherche, puis cliquez sur **Installer** pour installer le package correspondant à votre projet (une invite Contrat de licence s’affichera également). Veillez à installer uniquement le package **Microsoft.UI.Xaml** et non **Microsoft.UI.Xaml.Core.Direct**.

## <a name="declare-winui-application-resources"></a>Déclarer des ressources d’application WinUI

Ouvrez `App.xaml` et collez la marque suivante entre les balises **Application** d’ouverture et de fermeture existantes.

```xaml
<Application.Resources>
    <XamlControlsResources xmlns="using:Microsoft.UI.Xaml.Controls"/>
</Application.Resources>
```

## <a name="add-a-winui-control-to-mainpage"></a>Ajouter un contrôle WinUI à MainPage

Ensuite, ouvrez `MainPage.xaml`. Dans la balise **Application** d’ouverture existante, il existe des déclarations d’espace de noms XML. Ajoutez la déclaration d’espace de noms XML `xmlns:muxc="using:Microsoft.UI.Xaml.Controls"`. Collez ensuite la marque suivante entre les balises **Page** d’ouverture et de fermeture existantes en remplaçant l’élément **StackPanel** existant.

```xaml
<muxc:NavigationView PaneTitle="Welcome">
    <TextBlock Text="Hello, World!" VerticalAlignment="Center" HorizontalAlignment="Center" Style="{StaticResource TitleTextBlockStyle}"/>
</muxc:NavigationView>
```

## <a name="edit-mainpageh-and-cpp-as-necessary"></a>Modifiez MainPage.h et .cpp selon vos besoins

Dans `MainPage.h`, modifiez vos fichiers inclus afin qu’ils ressemblent à ce qui suit.

```cppwinrt
#include "MainPage.g.h"
#include "winrt/Microsoft.UI.Xaml.Controls.h"
#include "winrt/Microsoft.UI.Xaml.XamlTypeInfo.h"
```

Enfin, dans `MainPage.cpp`, supprimez le code à l’intérieur de votre implémentation de **MainPage::ClickHandler**, car le balisage XAML ne contient plus *myButton*.

Vous pouvez à présent lancer le processus de génération et exécuter le projet.

![Capture d’écran de l’exemple de bibliothèque d’IU Windows C++/WinRT simple](images/winui.png)

## <a name="related-topics"></a>Rubriques connexes
* [Prise en main de la bibliothèque d’IU Windows](/uwp/toolkits/winui/getting-started)