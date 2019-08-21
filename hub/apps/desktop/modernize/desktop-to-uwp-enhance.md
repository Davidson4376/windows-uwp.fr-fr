---
Description: Améliorez votre application bureautique pour les utilisateurs de Windows 10 à l’aide d’API plateforme Windows universelle (UWP).
title: Utiliser les API UWP dans les applications de bureau
ms.date: 08/20/2019
ms.topic: article
keywords: windows 10, uwp
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 44ea01bbc2200c1b028ed41e7c6a2845c7a1568b
ms.sourcegitcommit: 6bb794c6e309ba543de6583d96627fbf1c177bef
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/20/2019
ms.locfileid: "69643364"
---
# <a name="call-uwp-apis-in-desktop-apps"></a>Appeler les API UWP dans les applications de bureau

Vous pouvez utiliser des API plateforme Windows universelle (UWP) pour ajouter des expériences modernes à vos applications de bureau qui s’illuminent pour les utilisateurs de Windows 10.

Tout d’abord, configurez votre projet avec les références requises. Appelez ensuite les API UWP à partir de votre code pour ajouter des expériences Windows 10 à votre application de bureau. Vous pouvez créer séparément pour les utilisateurs Windows 10 ou distribuer les mêmes fichiers binaires à tous les utilisateurs, quelle que soit la version de Windows qu’ils exécutent.

Certaines API UWP sont prises en charge uniquement dans les applications de bureau qui sont empaquetées dans un [package MSIX](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-root). Pour plus d’informations, consultez [API UWP disponibles](desktop-to-uwp-supported-api.md).

## <a name="set-up-your-project"></a>Configurer votre projet

Vous devrez apporter quelques modifications à votre projet pour utiliser les API UWP.

### <a name="modify-a-net-project-to-use-windows-runtime-apis"></a>Modifier un projet .NET pour utiliser des API Windows Runtime

Il existe deux options pour les projets .NET:

* Si votre application cible Windows 10 version 1803 ou ultérieure, vous pouvez installer un package NuGet qui fournit toutes les références nécessaires.
* Vous pouvez également ajouter les références manuellement.

#### <a name="to-use-the-nuget-option"></a>Pour utiliser l’option NuGet

1. Vérifiez que les [références de package](https://docs.microsoft.com/nuget/consume-packages/package-references-in-project-files) sont activées:

    1. Dans Visual Studio, cliquez sur **Outils-> gestionnaire de package NuGet-> paramètres du gestionnaire de package**.
    2. Assurez-vous que **PackageReference** est sélectionné pour le **format de gestion des packages par défaut**.

2. Une fois votre projet ouvert dans Visual Studio, cliquez avec le bouton droit sur votre projet dans **Explorateur de solutions** , puis choisissez **gérer les packages NuGet**.

3. Dans la fenêtre **Gestionnaire de package NuGet** , assurez-vous que l’option **inclure la version préliminaire** est sélectionnée. Ensuite, sélectionnez l’onglet **Parcourir** et recherchez `Microsoft.Windows.SDK.Contracts`.

4. Une fois `Microsoft.Windows.SDK.Contracts` le package trouvé, dans le volet droit de la fenêtre **Gestionnaire de package NuGet** , sélectionnez la **version** du package que vous souhaitez installer en fonction de la version de Windows 10 que vous souhaitez cibler:

    * **10.0.18362. xxxx-** préversion: Choisissez cette version pour Windows 10, version 1903.
    * **10.0.17763. xxxx-** préversion: Choisissez cette version pour Windows 10, version 1809.
    * **10.0.17134. xxxx-** préversion: Choisissez cette version pour Windows 10, version 1803.

5. Cliquez sur **Installer**.

#### <a name="to-add-the-required-references-manually"></a>Pour ajouter manuellement les références requises

1. Ouvrez la boîte de dialogue **Gestionnaire de références**, choisissez le bouton **Parcourir**, puis sélectionnez **Tous les fichiers**.

    ![boîte de dialogue ajouter une référence](images/desktop-to-uwp/browse-references.png)

2. Ajoutez une référence à ces fichiers.

    |Fichier|Location|
    |--|--|
    |System.Runtime.WindowsRuntime|C:\Windows\Microsoft.NET\Framework\v4.0.30319|
    |System.Runtime.WindowsRuntime.UI.Xaml|C:\Windows\Microsoft.NET\Framework\v4.0.30319|
    |System.Runtime.InteropServices.WindowsRuntime|C:\Windows\Microsoft.NET\Framework\v4.0.30319|
    |Windows. winmd|C:\Program Files (x86) \Windows Kits\10\UnionMetadata\\<*SDK version*> \Facade|
    |Windows.Foundation.UniversalApiContract.winmd|C:\Program Files (x86) \Windows Kits\10\References\\<*SDK version*> \Windows.Foundation.UniversalApiContract\<>|
    |Windows.Foundation.FoundationContract.winmd|C:\Program Files (x86) \Windows Kits\10\References\\<*SDK version*> \Windows.Foundation.FoundationContract\<>|

3. Dans la fenêtre **Propriétés**, réglez le champ **Copie locale** de chaque fichier *.winmd* sur **False**.

    ![copy-local-field](images/desktop-to-uwp/copy-local-field.png)

### <a name="modify-a-c-win32-project-to-use-windows-runtime-apis"></a>Modifier un C++ projet Win32 pour utiliser des API Windows Runtime

Utilisez [ C++/WinRT](https://docs.microsoft.com/windows/uwp/cpp-and-winrt-apis/) pour utiliser des API Windows Runtime. C++/WinRT est une projection de langage C++17 moderne entièrement standard pour les API Windows Runtime (WinRT), implémentée en tant que bibliothèque basée sur un fichier d'en-tête et conçue pour vous fournir un accès de première classe à l’API Windows moderne.

Pour configurer votre projet pour C++/WinRT:

* Pour les nouveaux projets, vous pouvez installer l' [ C++extension Visual Studio/WinRT (VSIX)](https://marketplace.visualstudio.com/items?itemName=CppWinRTTeam.cppwinrt101804264) et utiliser l’un C++des modèles de projet/WinRT inclus dans cette extension.
* Pour les projets existants, vous pouvez installer le package NuGet [Microsoft. Windows. CppWinRT](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/) dans le projet.

Pour plus d’informations sur ces options, consultez [cet article](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package).

## <a name="add-windows-10-experiences"></a>Ajouter des expériences Windows 10

Vous êtes maintenant prêt à ajouter des expériences modernes qui se déclenchent lorsque les utilisateurs exécutent votre application sur Windows 10. Utilisez ce flux de conception.

:white_check_mark: **Tout d’abord, déterminez les expériences que vous souhaitez ajouter**

Vous avez le choix parmi une grande variété. Par exemple, vous pouvez simplifier votre bon de commande à l’aide des [API](/windows/uwp/monetize)de monétisation ou [attirer l’attention sur votre application](/windows/uwp/design/shell/tiles-and-notifications/adaptive-interactive-toasts) lorsque vous avez des éléments intéressants à partager, par exemple une nouvelle image publiée par un autre utilisateur.

![Toast](images/desktop-to-uwp/toast.png)

Même si les utilisateurs ignorent ou ferment votre message, ils peuvent le revoir dans le centre de notifications et cliquer sur le message pour ouvrir votre application. Cela augmente l’engagement avec votre application et a pour avantage de rendre votre application plus profondément intégrée au système d’exploitation. Nous vous présenterons le code de cette expérience un peu plus loin dans cet article.

Pour plus d’informations, consultez la [documentation UWP](/windows/uwp/get-started/) .

:white_check_mark: **Décider s’il faut améliorer ou étendre**

Nous entendons souvent utiliser les termes *améliorer* et *étendre*. nous allons donc prendre un moment pour expliquer exactement ce que signifient chacun de ces termes.

Nous utilisons le terme « *améliorer* » pour décrire Windows Runtime API que vous pouvez appeler directement à partir de votre application de bureau (que vous ayez ou non choisi d’empaqueter votre application dans un package MSIX). Lorsque vous avez choisi une expérience Windows 10, identifiez les API dont vous avez besoin pour la créer, puis vérifiez si cette API s’affiche dans [cette liste](desktop-to-uwp-supported-api.md). Il s’agit d’une liste d’API que vous pouvez appeler directement à partir de votre application de bureau. Si votre API n’apparaît pas dans cette liste, c'est parce que la fonctionnalité associée à cette API ne peut s’exécuter qu'au sein d'un processus UWP. Souvent, il s’agit d’API qui affichent le XAML UWP, par exemple un contrôle de carte UWP ou une invite de sécurité Windows Hello.

> [!NOTE]
> Bien que les API qui affichent le code XAML UWP ne puissent généralement pas être appelées directement à partir de votre bureau, vous pourrez peut-être utiliser d’autres approches. Si vous souhaitez héberger des contrôles XAML UWP ou d’autres expériences visuelles personnalisées, vous pouvez utiliser des [îlots XAML](xaml-islands.md) (à partir de Windows 10, version 1903) et la [couche visuelle](visual-layer-in-desktop-apps.md) (à partir de windows 10, version 1803). Ces fonctionnalités peuvent être utilisées dans les applications de bureau empaquetées ou non.

Si vous avez choisi d’empaqueter votre application de bureau dans un package MSIX, une autre option consiste à *étendre* l’application en ajoutant un projet UWP à votre solution. Le projet de bureau est toujours le point d’entrée de votre application, mais le projet UWP vous donne accès à toutes les API qui n’apparaissent pas dans [cette liste](desktop-to-uwp-supported-api.md). L’application de bureau peut communiquer avec le processus UWP à l’aide d’un app service, et nous disposons d’un grand nombre d’instructions sur la façon de le configurer. Si vous souhaitez ajouter une expérience nécessitant un projet UWP, consultez [étendre avec les composants UWP](desktop-to-uwp-extend.md).

:white_check_mark: **Contrats d’API de référence**

Si vous pouvez appeler l’API directement à partir de votre application de bureau, ouvrez un navigateur et recherchez la rubrique de référence pour cette API.
Sous le résumé de l’API, vous trouverez une table qui décrit le contrat API pour cette API. Voici un exemple de cette table :

![Table de contrat API](images/desktop-to-uwp/contract-table.png)

Si vous avez une application de bureau .NET, ajoutez une référence à ce contrat API et définissez la propriété **Copie locale** de ce fichier sur la valeur **False**. Si vous avez un projet C++, ajoutez à vos **Autres répertoires Include** un chemin d’accès au dossier qui contient ce contrat.

:white_check_mark: **Appeler les API pour ajouter votre expérience**

Voici le code qui vous permettrait d'afficher la fenêtre de notification que nous avons examinée plus haut. Ces API apparaissent dans cette [liste](desktop-to-uwp-supported-api.md) , ce qui vous permet d’ajouter ce code à votre application de bureau et de l’exécuter immédiatement.

```csharp
using Windows.Foundation;
using Windows.System;
using Windows.UI.Notifications;
using Windows.Data.Xml.Dom;
...

private void ShowToast()
{
    string title = "featured picture of the day";
    string content = "beautiful scenery";
    string image = "https://picsum.photos/360/180?image=104";
    string logo = "https://picsum.photos/64?image=883";

    string xmlString =
    $@"<toast><visual>
       <binding template='ToastGeneric'>
       <text>{title}</text>
       <text>{content}</text>
       <image src='{image}'/>
       <image src='{logo}' placement='appLogoOverride' hint-crop='circle'/>
       </binding>
      </visual></toast>";

    XmlDocument toastXml = new XmlDocument();
    toastXml.LoadXml(xmlString);

    ToastNotification toast = new ToastNotification(toastXml);

    ToastNotificationManager.CreateToastNotifier().Show(toast);
}
```

```C++
using namespace Windows::Foundation;
using namespace Windows::System;
using namespace Windows::UI::Notifications;
using namespace Windows::Data::Xml::Dom;

void UWP::ShowToast()
{
    Platform::String ^title = "featured picture of the day";
    Platform::String ^content = "beautiful scenery";
    Platform::String ^image = "https://picsum.photos/360/180?image=104";
    Platform::String ^logo = "https://picsum.photos/64?image=883";

    Platform::String ^xmlString =
        L"<toast><visual><binding template='ToastGeneric'>" +
        L"<text>" + title + "</text>" +
        L"<text>"+ content + "</text>" +
        L"<image src='" + image + "'/>" +
        L"<image src='" + logo + "'" +
        L" placement='appLogoOverride' hint-crop='circle'/>" +
        L"</binding></visual></toast>";

    XmlDocument ^toastXml = ref new XmlDocument();

    toastXml->LoadXml(xmlString);

    ToastNotificationManager::CreateToastNotifier()->Show(ref new ToastNotification(toastXml));
}
```

Pour en savoir plus sur les notifications, voir [Notifications toast adaptatives et interactives](https://docs.microsoft.com/windows/uwp/design/shell/tiles-and-notifications/adaptive-interactive-toasts).

## <a name="support-windows-xp-windows-vista-and-windows-78-install-bases"></a>Prise en charge des bases d'installation Windows XP, Windows Vista et Windows 7/8

Vous pouvez moderniser votre application pour Windows 10 sans avoir à créer une nouvelle branche et à conserver des bases de code distinctes.

Si vous souhaitez créer des fichiers binaires distincts pour les utilisateurs de Windows 10, utilisez la compilation conditionnelle. Si vous préférez créer un ensemble de fichiers binaires que vous déployez sur tous les utilisateurs de Windows, utilisez des vérifications à l’exécution.

Jetons un coup d’œil à chaque option.

### <a name="conditional-compilation"></a>Compilation conditionnelle

Vous pouvez conserver une seule base de code et compiler un ensemble de fichiers binaires uniquement pour les utilisateurs de Windows 10.

D'abord, ajoutez une nouvelle configuration de build à votre projet.

![Configuration de build](images/desktop-to-uwp/build-config.png)

Pour cette configuration de build, créez une constante permettant d’identifier le code qui appelle des API Windows Runtime.  

Pour les projets .NET, la constante s'appelle **Constante de compilation conditionnelle**.

![préprocesseur](images/desktop-to-uwp/compilation-constants.png)

Pour les projets C++, la constante s'appelle **Définition du préprocesseur**.

![préprocesseur](images/desktop-to-uwp/pre-processor.png)

Ajoutez cette constante avant un bloc de code UWP.

```csharp

[System.Diagnostics.Conditional("_UWP")]
private void ShowToast()
{
 ...
}

```

```C++

#if _UWP
void UWP::ShowToast()
{
 ...
}
#endif

```

Le compilateur génère ce code uniquement si cette constante est définie dans votre configuration de build active.

### <a name="runtime-checks"></a>Vérifications à l’exécution

Vous pouvez compiler un ensemble de fichiers binaires pour l’ensemble de vos utilisateurs Windows, quelle que soit la version de Windows exécutée. Votre application appelle Windows Runtime API uniquement si l’utilisateur exécute votre application en tant qu’application empaquetée sur Windows 10.

Le moyen le plus simple d’ajouter des vérifications à l’exécution à votre code consiste à installer ce package NuGet: Les [applications d’assistance de Desktop Bridge](https://www.nuget.org/packages/DesktopBridge.Helpers/) , ``IsRunningAsUWP()`` puis utilisent la méthode pour emporter tout le code qui appelle des API Windows Runtime. Pour plus d’informations, consultez ce billet de blog: [Desktop Bridge-identifie le contexte de l’application](https://blogs.msdn.microsoft.com/appconsult/2016/11/03/desktop-bridge-identify-the-applications-context/).

## <a name="related-samples"></a>Exemples connexes

* [Exemple de Hello World](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/HelloWorldSample)
* [Vignette secondaire](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/SecondaryTileSample)
* [Exemple d’API Store](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/StoreSample)
* [Application WinForms qui implémente un UpdateTask UWP](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/WinFormsUpdateTaskSample)
* [Exemples de pont d’application de bureau vers UWP](https://github.com/Microsoft/DesktopBridgeToUWP-Samples)

## <a name="support-and-feedback"></a>Support et commentaires

**Trouvez les réponses à vos questions**

Des questions ? Contactez-nous sur Stack Overflow. Notre équipe contrôle ces [balises](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge). Vous pouvez également nous poser vos questions [ici](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D).

**Envoyer des commentaires ou apporter des suggestions de fonctionnalités**

Consultez [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial).
