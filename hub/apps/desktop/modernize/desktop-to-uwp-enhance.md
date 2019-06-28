---
Description: Améliorez votre application de bureau pour les utilisateurs de Windows 10 à l’aide de plateforme Windows universelle (UWP) API.
title: Utilisez des API UWP dans les applications de bureau
ms.date: 04/19/2019
ms.topic: article
keywords: windows 10, uwp
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 0545ea525b96d3a9310f3a761fd60a644f21baeb
ms.sourcegitcommit: b8087f8b6cf8367f8adb7d6db4581d9aa47b4861
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/27/2019
ms.locfileid: "67414081"
---
# <a name="call-uwp-apis-in-desktop-apps"></a>Appeler des API UWP dans les applications de bureau

Vous pouvez utiliser la plateforme Windows universelle (UWP) API pour ajouter des expériences modernes à vos applications de bureau qui sont disponibles pour les utilisateurs de Windows 10.

Tout d’abord, configurez votre projet avec les références requises. Ensuite, appelez l’API UWP à partir de votre code pour ajouter des qu'expériences Windows 10 à votre application de bureau. Vous pouvez créer séparément pour les utilisateurs Windows 10 ou distribuer les mêmes fichiers binaires à tous les utilisateurs, quel que soit la version de Windows, ils exécutent.

Certaines API UWP sont pris en charge uniquement dans les applications de bureau qui sont empaquetées dans un [package MSIX](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-root). Pour plus d’informations, consultez [API UWP disponibles](desktop-to-uwp-supported-api.md).

## <a name="set-up-your-project"></a>Configurer votre projet

Vous devrez apporter quelques modifications à votre projet pour utiliser les API UWP.

### <a name="modify-a-net-project-to-use-windows-runtime-apis"></a>Modifier un projet .NET pour utiliser Windows Runtime APIs

Il existe deux options pour les projets .NET :

* Si votre application cible Windows 10 version 1803 ou ultérieure, vous pouvez installer un package NuGet qui fournit toutes les références nécessaires.
* Ou bien, vous pouvez ajouter les références manuellement.

#### <a name="to-use-the-nuget-option"></a>Pour utiliser l’option de NuGet

1. Assurez-vous que [références de package](https://docs.microsoft.com/nuget/consume-packages/package-references-in-project-files) sont activés :

    1. Dans Visual Studio, cliquez sur **Outils -> Gestionnaire de Package NuGet -> Paramètres du Gestionnaire de Package**.
    2. Assurez-vous que **PackageReference** est sélectionné pour **format de gestion de package par défaut**.

2. Avec votre projet ouvert dans Visual Studio, cliquez sur votre projet dans **l’Explorateur de solutions** et choisissez **gérer les Packages NuGet**.

3. Dans le **Gestionnaire de Package NuGet** fenêtre, sélectionnez le **Parcourir** onglet et recherchez `Microsoft.Windows.SDK.Contracts`.

4. Après le `Microsoft.Windows.SDK.Contracts` package est trouvé, dans le volet droit de la **Gestionnaire de Package NuGet** fenêtre Sélectionnez les **Version** du package que vous souhaitez installer selon la version de Windows 10 à cibler :

    * **10.0.18362.xxxx-Preview**: Choisissez cette option pour Windows 10, version 1903.
    * **10.0.17763.xxxx-Preview**: Choisissez cette option pour Windows 10, version 1809.
    * **10.0.17134.xxxx-Preview**: Choisissez cette option pour Windows 10, version 1803.

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
    |windows.winmd|C:\Program Files (x86)\Windows Kits\10\UnionMetadata\\<*sdk version*>\Facade|
    |Windows.Foundation.UniversalApiContract.winmd|C:\Program fichiers (x86) \Windows Kits\10\References\\<*version_sdk*> \Windows.Foundation.UniversalApiContract\<*version*>|
    |Windows.Foundation.FoundationContract.winmd|C:\Program fichiers (x86) \Windows Kits\10\References\\<*version_sdk*> \Windows.Foundation.FoundationContract\<*version*>|

3. Dans la fenêtre **Propriétés**, réglez le champ **Copie locale** de chaque fichier *.winmd* sur **False**.

    ![copy-local-field](images/desktop-to-uwp/copy-local-field.png)

### <a name="modify-a-c-project-to-use-windows-runtime-apis"></a>Modifier un projet C++ pour utiliser Windows Runtime APIs

Utilisez [C++ / c++ / WinRT](https://docs.microsoft.com/windows/uwp/cpp-and-winrt-apis/) pour consommer le Windows Runtime APIs. C++/WinRT est une projection de langage C++17 moderne entièrement standard pour les API Windows Runtime (WinRT), implémentée en tant que bibliothèque basée sur un fichier d'en-tête et conçue pour vous fournir un accès de première classe à l’API Windows moderne.

Pour configurer votre projet pour C / c++ / WinRT, consultez [modifier un projet d’application de bureau de Windows pour ajouter C + c++ / WinRT support](https://docs.microsoft.com/windows/uwp/cpp-and-winrt-apis/get-started#modify-a-windows-desktop-application-project-to-add-cwinrt-support).

## <a name="add-windows-10-experiences"></a>Ajouter des expériences Windows 10

Vous êtes maintenant prêt à ajouter des expériences modernes qui se déclenchent lorsque les utilisateurs exécutent votre application sur Windows 10. Utilisez ce flux de conception.

:white_check_mark: **Tout d’abord, décider quelles expériences que vous souhaitez ajouter**

Vous avez le choix parmi une grande variété. Par exemple, vous pouvez simplifier votre flux de commandes d’achat à l’aide de [monétisation API](/windows/uwp/monetize), ou [attirer l’attention à votre application](/windows/uwp/design/shell/tiles-and-notifications/adaptive-interactive-toasts) lorsque vous avez quelque chose d’intéressant à partager, comme une nouvelle image qui un autre utilisateur a publié.

![Toast](images/desktop-to-uwp/toast.png)

Même si les utilisateurs ignorent ou ferment votre message, ils peuvent le revoir dans le centre de notifications et cliquer sur le message pour ouvrir votre application. Cela augmente l’engagement avec votre application et a l’avantage supplémentaire de faire apparaître profondément intégré avec le système d’exploitation de votre application. Nous allons vous montrer le code de cette expérience un peu plus loin dans cet article.

Visitez le [documentation UWP](/windows/uwp/get-started/) pour plus d’informations.

:white_check_mark: **Décider d’améliorer ou étendre**

Allez on entend souvent que nous utilisons les termes *améliorer* et *étendre*, de sorte que nous allons prendre un moment pour expliquer chacune de ces conditions d’utilisation moyenne.

Nous utilisons le terme *améliorer* pour décrire le Windows Runtime APIs que vous pouvez appeler directement à partir de votre application de bureau (si vous avez choisi empaqueter votre application dans un package MSIX). Lorsque vous avez choisi une expérience de Windows 10, identifier les API dont vous avez besoin pour créer et de voir si cette API s’affiche dans [cette liste](desktop-to-uwp-supported-api.md). Il s’agit d’une liste d’API que vous pouvez appeler directement à partir de votre application de bureau. Si votre API n’apparaît pas dans cette liste, c'est parce que la fonctionnalité associée à cette API ne peut s’exécuter qu'au sein d'un processus UWP. Il arrive souvent que ceux-ci incluent des API qui restituent UWP XAML comme un contrôle de carte UWP ou une invite de sécurité Windows Hello.

> [!NOTE]
> Bien que les API qui restituent UWP XAML en général, ne peut pas être appelées directement à partir de votre bureau, vous pourrez peut-être utiliser d’autres approches. Si vous souhaitez héberger les contrôles UWP XAML ou autres expériences visuelles personnalisées, vous pouvez utiliser [XAML îles](xaml-islands.md) (à partir de Windows 10, version 1903) et le [couche visuelle](visual-layer-in-desktop-apps.md) (à partir de Windows 10, version 1803). Ces fonctionnalités peuvent être utilisées dans les applications de bureau empaquetées ou décompressées.

Si vous avez choisi d’empaqueter votre application de bureau dans un package MSIX, une autre option consiste à *étendre* l’application en ajoutant un projet UWP à votre solution. Le projet de bureau est toujours le point d’entrée de votre application, mais le projet UWP vous donne accès à toutes les API qui n’apparaissent pas dans [cette liste](desktop-to-uwp-supported-api.md). L’application de bureau peut communiquer avec le processus UWP en utilisant un un service d’application et nous permet de bénéficier des conseils sur la façon de configurer cela. Si vous souhaitez ajouter une expérience qui nécessite un projet UWP, consultez [étendre avec des composants UWP](desktop-to-uwp-extend.md).

:white_check_mark: **Contrats d’API de référence**

Si vous pouvez appeler l’API directement à partir de votre application de bureau, ouvrez un navigateur et la recherche de la rubrique de référence pour cette API.
Sous le résumé de l’API, vous trouverez une table qui décrit le contrat API pour cette API. Voici un exemple de cette table :

![Table de contrat API](images/desktop-to-uwp/contract-table.png)

Si vous avez une application de bureau .NET, ajoutez une référence à ce contrat API et définissez la propriété **Copie locale** de ce fichier sur la valeur **False**. Si vous avez un projet C++, ajoutez à vos **Autres répertoires Include** un chemin d’accès au dossier qui contient ce contrat.

:white_check_mark: **Appeler les API pour ajouter votre expérience**

Voici le code qui vous permettrait d'afficher la fenêtre de notification que nous avons examinée plus haut. Ces API apparaître dans ce [liste](desktop-to-uwp-supported-api.md) afin de pouvoir ajouter ce code à votre application de bureau et exécutez-le dès maintenant.

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

Vous pouvez moderniser votre application pour Windows 10 sans avoir à créer une branche et de gérer des bases de code distinctes.

Si vous souhaitez créer des fichiers binaires distincts pour les utilisateurs de Windows 10, utilisez la compilation conditionnelle. Si vous préférez créer un ensemble de fichiers binaires que vous déployez sur tous les utilisateurs de Windows, utilisez des vérifications à l’exécution.

Jetons un coup d’œil à chaque option.

### <a name="conditional-compilation"></a>Compilation conditionnelle

Vous pouvez conserver une seule base de code et compiler un ensemble de fichiers binaires uniquement pour les utilisateurs de Windows 10.

D'abord, ajoutez une nouvelle configuration de build à votre projet.

![Configuration de build](images/desktop-to-uwp/build-config.png)

Pour cette configuration de build, créez une constante pour identifier le code qui appelle Windows APIs Runtime.  

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

Vous pouvez compiler un ensemble de fichiers binaires pour l’ensemble de vos utilisateurs Windows, quelle que soit la version de Windows exécutée. Votre application appelle Windows APIs Runtime uniquement si l’utilisateur s’exécute votre application sous la forme d’une application empaquetée sur Windows 10.

Pour ajouter des vérifications à l’exécution à votre code, le plus simple consiste à installer ce package Nuget : [Programmes d’assistance de pont du bureau](https://www.nuget.org/packages/DesktopBridge.Helpers/) , puis utilisez le ``IsRunningAsUWP()`` méthode porte désactiver tout le code qui appelle Windows APIs Runtime. consultez le blog pour plus d’informations : [Pont du bureau - identifier le contexte de l’application](https://blogs.msdn.microsoft.com/appconsult/2016/11/03/desktop-bridge-identify-the-applications-context/).

## <a name="related-samples"></a>Exemples connexes

* [Exemple Hello World](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/HelloWorldSample)
* [Vignette secondaire](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/SecondaryTileSample)
* [Exemple d’API de Store](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/StoreSample)
* [Application WinForms qui implémente un UpdateTask UWP](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/WinFormsUpdateTaskSample)
* [Pont d’application de bureau vers des exemples UWP](https://github.com/Microsoft/DesktopBridgeToUWP-Samples)

## <a name="support-and-feedback"></a>Support et commentaires

**Trouvez des réponses à vos questions**

Des questions ? Contactez-nous sur Stack Overflow. Notre équipe contrôle ces [balises](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge). Vous pouvez également nous poser vos questions [ici](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D).

**Donner votre avis ou faire des suggestions de fonctionnalité**

Consultez [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial).
