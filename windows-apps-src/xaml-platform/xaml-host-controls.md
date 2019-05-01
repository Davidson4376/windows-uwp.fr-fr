---
description: Ce guide vous aide à créer des interfaces utilisateur UWP Fluent directement dans vos applications WPF et Windows Forms
title: Contrôles UWP dans des applications de bureau
ms.date: 04/19/2019
ms.topic: article
keywords: Windows 10, uwp, WinForms, wpf, xaml (îles)
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: df2bb23f3934b7629f2fec408f8bde713e5f69d9
ms.sourcegitcommit: fca0132794ec187e90b2ebdad862f22d9f6c0db8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/24/2019
ms.locfileid: "63812721"
---
# <a name="uwp-controls-in-desktop-applications-xaml-islands"></a>Contrôles UWP dans les applications de bureau (îles XAML)

À compter de Windows 10, version 1903, vous pouvez héberger des contrôles UWP dans les applications de bureau non UWP à l’aide d’une fonctionnalité appelée *XAML îles*. Cette fonctionnalité vous permet d’améliorer l’apparence et les fonctionnalités de vos applications de bureau existantes avec les dernières fonctionnalités de l’interface utilisateur de Windows 10 qui sont uniquement disponibles via les contrôles UWP. Cela signifie que vous pouvez utiliser les fonctionnalités UWP comme [Windows Ink](../design/input/pen-and-stylus-interactions.md) et les contrôles qui prennent en charge la [Fluent Design System](../design/fluent-design-system/index.md) dans votre existante, Windows Forms, applications WPF et Win32 C++.

Nous fournissons plusieurs façons d’utiliser des îlots de XAML dans votre WPF, Windows Forms, et C++ applications Win32, en fonction de la technologie ou d’un framework que vous utilisez.

> [!NOTE]
> Si vous avez des commentaires concernant les îles de XAML, créez un nouveau problème dans le [Microsoft.Toolkit.Win32 référentiel](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/issues) et y laisser vos commentaires. Si vous préférez envoyer vos commentaires en privé, vous pouvez l’envoyer à XamlIslandsFeedback@microsoft.com. Vos idées et les scénarios sont extrêmement importantes pour nous.

## <a name="how-do-xaml-islands-work"></a>Comment fonctionnent les îles de XAML ?

À compter de Windows 10, version 1903, nous fournissons deux façons d’utiliser des îlots de XAML dans votre WPF, Windows Forms, et C++ applications Win32 :

* Le SDK Windows fournit plusieurs classes Windows Runtime et les interfaces COM que votre application peut utiliser pour héberger n’importe quel contrôle UWP qui dérive de [ **Windows.UI.Xaml.UIElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement). Collectivement, ces classes et les interfaces sont appelés les *XAML UWP API d’hébergement*, et ils vous permettent d’héberger des contrôles UWP dans n’importe quel élément d’interface utilisateur dans votre application qui a un handle de fenêtre associée (HWND). Pour plus d’informations sur cette API, consultez [à l’aide de l’API d’hébergement de XAML](using-the-xaml-hosting-api.md).

* Le [Kit de ressources de communauté Windows](https://docs.microsoft.com/windows/uwpcommunitytoolkit/) fournit également des contrôles supplémentaires îlot de XAML pour WPF et Windows Forms. Ces contrôles utilisent le XAML UWP API d’hébergement en interne et implémentent le comportement que vous devrez sinon gérer vous-même si vous avez utilisé le XAML UWP API directement, y compris les modifications de navigation et la disposition de clavier d’hébergement. Pour les applications WPF et Windows Forms, nous recommandons que vous utilisez ces contrôles au lieu du XAML UWP API d’hébergement directement, car ils vous déchargent de rangements plusieurs détails de l’implémentation de l’utilisation de l’API. Notez qu’à compter de Windows 10, version 1903, ces contrôles sont [disponible en version préliminaire développeur](#feature-roadmap).

> [!NOTE]
> Les applications de bureau Win32 C++ doivent utiliser le XAML UWP API d’hébergement pour héberger des contrôles UWP. Les contrôles de l’île de XAML dans le Kit de ressources de communauté de Windows ne sont pas disponibles pour C++ des applications de bureau Win32.

Il existe deux types de contrôles XAML (île) fournis par le Kit de ressources de communauté de Windows pour les applications WPF et Windows Forms : *encapsulé les contrôles* et *contrôles hôtes*.

### <a name="wrapped-controls"></a>Contrôles inclus dans un wrapper

Les applications WPF et Windows Forms peuvent utiliser une sélection de contrôles UWP encapsulées dans la [Kit de ressources de communauté Windows](https://docs.microsoft.com/windows/uwpcommunitytoolkit/). Ils sont appelés *encapsulé les contrôles* , car elles encapsulent l’interface et les fonctionnalités d’un contrôle UWP spécifique. Vous pouvez ajouter ces contrôles directement à l’aire de conception de votre projet WPF ou Windows Forms, puis de les utiliser comme tout autre contrôle WPF ou Windows Forms dans votre concepteur.

Les contrôles UWP encapsulées suivants pour l’implémentation XAML îles sont actuellement disponibles pour les applications WPF et Windows Forms.

| Commande | Minimale prise en charge du système d’exploitation | Description |
|-----------------|-------------------------------|-------------|
| [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas)<br>[InkToolbar](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inktoolbar) | Windows 10, version 1903 | Fournir un barres d’outils de l’aire de conception et connexes pour l’interaction utilisateur Windows encrage dans votre application de bureau Windows Forms ou WPF. |
| [MediaPlayerElement](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/mediaplayerelement) | Windows 10, version 1903 | Incorpore une vue qui transmet en continu et restitue le contenu multimédia par exemple une vidéo dans votre application de bureau Windows Forms ou WPF. |
| [MapControl](https://docs.microsoft.com/en-us/windows/communitytoolkit/controls/wpf-winforms/mapcontrol) | Windows 10, version 1903 | Vous permet d’afficher une carte symbolique ou photoréaliste dans votre application de bureau Windows Forms ou WPF. |

Outre les contrôles inclus dans un wrapper pour XAML (îles), le Kit de ressources de communauté Windows fournit également les contrôles suivants pour l’hébergement de contenu web.

| Commande | Minimale prise en charge du système d’exploitation | Description |
|-----------------|-------------------------------|-------------|
| [WebView](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/webview) | Windows 10, version 1803 | Utilise le moteur de rendu Microsoft Edge pour afficher le contenu web. |
| [WebViewCompatible](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/webviewcompatible) | Windows 7 | Fournit une version de **WebView** qui est compatible avec plusieurs versions de système d’exploitation. Ce contrôle utilise le moteur de rendu Microsoft Edge pour afficher le contenu web sur Windows 10 version 1803 et versions ultérieure et le moteur de rendu d’Internet Explorer pour afficher le contenu web sur les versions antérieures de Windows 10, Windows 8.x et Windows 7. |

### <a name="host-controls"></a>Contrôles hôtes

Pour les scénarios, au-delà de ceux couverts par les contrôles inclus dans un wrapper disponibles, WPF et Windows Forms applications peuvent également utiliser le [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) dans contrôler le [Kit de ressources de communauté Windows](https://docs.microsoft.com/windows/uwpcommunitytoolkit/). Ce contrôle peut héberger n’importe quel contrôle UWP qui dérive de [ **Windows.UI.Xaml.UIElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement), y compris de n’importe quel contrôle UWP fournis par le Kit de développement Windows, ainsi que les contrôles utilisateur personnalisés. Ce contrôle prend en charge le SDK Windows 10 Insider Preview build 17709 et versions ultérieures.

### <a name="architecture-overview"></a>Présentation de l'architecture

Voici un aperçu de la façon dont ces contrôles sont organisés du point de vue architectural.

![Architecture de contrôle de l'hôte](images/host-controls.png)

Les API qui s’affichent en bas de ce diagramme sont livrés avec le SDK Windows. Les contrôles inclus dans un wrapper et les contrôles hôtes sont disponibles via les packages Nuget dans le Kit de ressources de communauté de Windows.

## <a name="feature-roadmap"></a>Feuille de route de fonctionnalité

Depuis la version de Windows 10, version 1903, les contrôles inclus dans un wrapper et les contrôles hôtes dans le Kit de ressources de communauté de Windows sont toujours en version préliminaire de développeur jusqu'à ce que la version 1.0 des contrôles est disponible.

* Version 1.0 des contrôles pour le .NET Framework 4.6.2 et ultérieurement planifiés pour être publié dans le [version 6.0 du Kit de ressources](https://github.com/windows-toolkit/WindowsCommunityToolkit/milestones).
* Version 1.0 des contrôles pour .NET Core 3 sont prévus pour une version ultérieure du Kit de ressources.
* Si vous souhaitez essayer les dernières versions préliminaires des versions de ces contrôles version 1.0 pour .NET Framework et .NET Core 3, consultez le **6.0.0-preview3** de packages NuGet dans le [UWP Community Toolkit](https://dotnet.myget.org/gallery/uwpcommunitytoolkit) galerie.

## <a name="requirements"></a>Configuration requise

Îles de XAML nécessite Windows 10, version 1903 et versions ultérieure. Pour utiliser des îlots de XAML dans votre application, vous devez tout d’abord configurer votre projet.

### <a name="step-1-modify-your-project-to-use-windows-runtime-apis"></a>Étape 1 : Modifier votre projet pour utiliser Windows Runtime APIs

Pour obtenir des instructions, consultez [cet article](../porting/desktop-to-uwp-enhance.md#first-set-up-your-project).

### <a name="step-2-enable-xaml-island-support-in-your-project"></a>Étape 2 : Prise en charge de l’île de XAML activer dans votre projet

Apportez l’une des modifications suivantes à votre projet pour activer la prise en charge de l’île de XAML. Pour plus d’informations, consultez [ce billet de blog](https://techcommunity.microsoft.com/t5/Windows-Dev-AppConsult/Using-XAML-Islands-on-Windows-10-19H1-fixing-the-quot/ba-p/376330#M117).

#### <a name="option-1-package-your-application-in-an-msix-package"></a>Option 1 : Empaqueter votre application dans un package MSIX  

Installer Windows 10, version 1903 SDK (ou une version ultérieure). Ensuite, empaqueter votre application dans un package MSIX en ajoutant un [projet d’empaquetage Windows Application](https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-packaging-dot-net) à votre solution et ajouter une référence à votre projet WPF ou Windows Forms.

#### <a name="option-2-set-the-maxversiontested-value-in-your-assembly-manifest"></a>Option 2 : Définissez la valeur de maxVersionTested dans votre manifeste d’assembly

Si vous ne souhaitez empaqueter votre application dans un package MSIX, vous pouvez ajouter un [manifeste d’assembly de côte à côte](https://docs.microsoft.com/windows/desktop/SbsCs/application-manifests) à votre projet et ajoutez le **maxVersionTested** valeur dans le manifeste de pour spécifier que votre application est compatible avec Windows 10, version 1903 ou version ultérieure.

1. Si vous ne disposez pas d’un assembly de manifeste dans votre projet, ajoutez un nouveau fichier XML à votre projet et nommez-le **App.manifest**. Pour une application WPF ou Windows Forms, assurez-vous que vous affectez également la **manifeste** propriété **. app.manifest** dans le **Application** page de votre [projet propriétés](https://docs.microsoft.com/en-us/visualstudio/ide/reference/application-page-project-designer-csharp?view=vs-2019#resources).
2. Dans votre manifeste d’assembly, incluez le **compatibilité** élément et les éléments enfants indiqués dans l’exemple suivant. Remplacez le **Id** attribut de la **maxVersionTested** élément avec le numéro de version de Windows 10 que vous ciblez (il doit s’agir de Windows 10, version 1903 ou une version ultérieure). 

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <assembly xmlns="urn:schemas-microsoft-com:asm.v1" manifestVersion="1.0">
        <compatibility xmlns="urn:schemas-microsoft-com:compatibility.v1">
            <application>
                <!-- Windows 10 -->
                <maxversiontested Id="10.0.18362.0"/>
                <supportedOS Id="{8e0f7a12-bfb3-4fe8-b9a5-48fd50a15a9a}" />
            </application>
        </compatibility>
    </assembly>
    ```

## <a name="additional-resources"></a>Ressources supplémentaires

Pour plus d’informations générales et des didacticiels sur l’utilisation de XAML (îles), consultez les articles et ressources suivants :

* [Îles de XAML Lab](https://github.com/Microsoft/Windows-AppConsult-XAMLIslandsLab/tree/microsoftlearn). Ce laboratoire complet fournit des instructions détaillées pour l’utilisation de contrôles inclus dans un wrapper et des contrôles hôtes dans le Kit de ressources de communauté de Windows pour ajouter des contrôles UWP à une application WPF line-of-business. Ce laboratoire inclut le [le code complet de l’application WPF](https://github.com/Microsoft/Windows-AppConsult-XAMLIslandsLab/tree/microsoftlearn/Lab) ainsi que [des instructions détaillées](https://github.com/Microsoft/Windows-AppConsult-XAMLIslandsLab/blob/microsoftlearn/Manual/README.md) pour chaque étape du processus.
