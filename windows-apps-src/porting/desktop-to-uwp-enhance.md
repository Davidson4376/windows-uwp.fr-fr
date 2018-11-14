---
author: normesta
Description: Enhance your desktop application for Windows 10 users by using Universal Windows Platform (UWP) APIs.
Search.Product: eADQiWindows 10XVcnh
title: Améliorer votre application de bureau pour Windows10
ms.author: normesta
ms.date: 10/15/2018
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 5e76d3d517be73417777eb31dfc3994f92186522
ms.sourcegitcommit: 38f06f1714334273d865935d9afb80efffe97a17
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/09/2018
ms.locfileid: "6191392"
---
# <a name="enhance-your-desktop-application-for-windows-10"></a>Améliorer votre application de bureau pour Windows10

Vous pouvez utiliser APIs Windows Runtime pour ajouter des expériences modernes qui se déclenchent pour les utilisateurs de Windows 10.

D'abord, configurez votre projet. Ensuite, ajoutez des expériences Windows10. Vous pouvez créer séparément pour les utilisateurs de Windows10 ou distribuer exactement les mêmes fichiers binaires à tous les utilisateurs, quelle que soit la version de Windows utilisée.

## <a name="first-set-up-your-project"></a>D'abord, configurez votre projet

Vous devrez apporter quelques modifications à votre projet pour utiliser les API UWP.

### <a name="modify-a-net-project-to-use-windows-runtime-apis"></a>Modifier un projet .NET pour utiliser APIs Windows Runtime

Ouvrez la boîte de dialogue **Gestionnaire de références**, choisissez le bouton **Parcourir**, puis sélectionnez **Tous les fichiers**.

![boîte de dialogue ajouter une référence](images/desktop-to-uwp/browse-references.png)

Ensuite, ajoutez une référence à ces fichiers.

|Fichier|Emplacement|
|--|--|
|System.Runtime.WindowsRuntime|C:\Program Files (x86)\Reference Assemblies\Microsoft\Framework\\.NETCore\v4.5|
|System.Runtime.WindowsRuntime.UI.Xaml|C:\Program Files (x86)\Reference Assemblies\Microsoft\Framework\\.NETCore\v4.5|
|System.Runtime.InteropServices.WindowsRuntime|C:\Program Files (x86)\Reference Assemblies\Microsoft\Framework\\.NETCore\v4.5|
|Windows.winmd|C:\Program Files (x86)\Windows Kits\10\UnionMetadata\Facade|
|Windows.Foundation.UniversalApiContract.winmd|C:\Program Files (x86)\Windows Kits\10\References\<*version du sdk*>\Windows.Foundation.UniversalApiContract\<*version*>|
|Windows.Foundation.FoundationContract.winmd|C:\Program Files (x86)\Windows Kits\10\References\<*version du sdk*>\Windows.Foundation.FoundationContract\<*version*>|

Dans la fenêtre **Propriétés**, réglez le champ **Copie locale** de chaque fichier *.winmd* sur **False**.

![copy-local-field](images/desktop-to-uwp/copy-local-field.png)

### <a name="modify-a-c-project-to-use-windows-runtime-apis"></a>Modifier un projet C++ pour utiliser APIs Windows Runtime

Utilisez [C++ / WinRT](https://docs.microsoft.com/windows/uwp/cpp-and-winrt-apis/) pour consommer APIs Windows Runtime. C++/WinRT est une projection de langage C++17 moderne entièrement standard pour les API Windows Runtime (WinRT), implémentée en tant que bibliothèque basée sur un fichier d'en-tête et conçue pour vous fournir un accès de première classe à l’API Windows moderne.

Pour configurer votre projet pour C++ / WinRT, voir [Modifier un projet d’application de bureau Windows pour ajouter C++ / WinRT support](https://docs.microsoft.com/windows/uwp/cpp-and-winrt-apis/get-started#modify-a-windows-desktop-application-project-to-add-cwinrt-support).

## <a name="add-windows-10-experiences"></a>Ajouter des expériences Windows10

Vous êtes maintenant prêt à ajouter des expériences modernes qui se déclenchent lorsque les utilisateurs exécutent votre application sur Windows10. Utilisez ce flux de conception.

:white_check_mark: **D’abord, choisissez les expériences que vous voulez ajouter**

Vous avez le choix parmi une grande variété. Par exemple, vous pouvez simplifier votre flux de bons de commande à l’aide des API de monétisation ou attirer l’attention vers votre application lorsque vous avez quelque chose d’intéressant à partager, par exemple, une nouvelle image un autre utilisateur a validé.

![Toast](images/desktop-to-uwp/toast.png)

Même si les utilisateurs ignorent ou ferment votre message, ils peuvent le revoir dans le centre de notifications et cliquer sur le message pour ouvrir votre application. Cela avec votre application et présente l’avantage supplémentaire de faire apparaître profondément intégrée au système d’exploitation de votre application. Nous vous présenterons le code de cette expérience un peu plus tard.

Visitez notre [centre de développement](https://developer.microsoft.com/windows) pour trouver de l'inspiration.

:white_check_mark: **Décidez entre améliorer ou étendre**

Vous nous entendrez souvent utiliser les termes «améliorer» et «étendre» donc nous allons prendre quelques instants pour expliquer ce que signifie chacun de ces termes exactement.

Nous utilisons le terme «améliorer» pour décrire APIs Windows Runtime que vous pouvez appeler directement à partir de votre application de bureau. Une fois que vous avez choisi une expérience Windows10, identifiez les API dont vous avez besoin pour la créer, puis vérifiez si elles figurent dans cette [liste](desktop-to-uwp-supported-api.md). Il s’agit d’une liste des API que vous pouvez appeler directement à partir de votre application de bureau. Si votre API n’apparaît pas dans cette liste, c'est parce que la fonctionnalité associée à cette API ne peut s’exécuter qu'au sein d'un processus UWP. Souvent, il s'agit d'API qui présentent des interfaces utilisateur modernes, comme un contrôle de carte UWP ou une confirmation de sécurité Windows Hello.

Cela dit, si vous souhaitez inclure ces expériences dans votre application, il suffit d'«étendre» l’application en ajoutant un projet UWP à votre solution. Le projet de bureau est toujours le point d’entrée de votre application, mais le projet UWP vous donne accès à toutes les API qui n’apparaissent pas dans cette [liste](desktop-to-uwp-supported-api.md). L’application de bureau peut communiquer avec le processus UWP en utilisant un service d’application et nous offrons de nombreux conseils sur la façon de configurer cette fonctionnalité. Si vous souhaitez ajouter une expérience qui requiert un projet UWP, voir [Étendre avec UWP](desktop-to-uwp-extend.md).

:white_check_mark: **Référencer des contrats API**

Si vous pouvez appeler l’API directement à partir de votre application de bureau, ouvrez un navigateur et recherchez la rubrique de référence de cette API.
Sous le résumé de l’API, vous trouverez une table qui décrit le contrat API pour cette API. Voici un exemple de cette table:

![Table de contrat API](images/desktop-to-uwp/contract-table.png)

Si vous avez une application de bureau .NET, ajoutez une référence à ce contrat API et définissez la propriété **Copie locale** de ce fichier sur la valeur **False**. Si vous avez un projet C++, ajoutez à vos **Autres répertoires Include** un chemin d’accès au dossier qui contient ce contrat.

:white_check_mark: **Appeler les API pour ajouter votre expérience**

Voici le code qui vous permettrait d'afficher la fenêtre de notification que nous avons examinée plus haut. Ces API s’affichent dans cette [liste](desktop-to-uwp-supported-api.md) afin que vous puissiez ajouter ce code à votre application de bureau et l’exécuter immédiatement.

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

## <a name="support-windows-xp-windows-vista-and-windows-78-install-bases"></a>Prise en charge des bases d'installation WindowsXP, WindowsVista et Windows7/8

Vous pouvez moderniser votre application pour Windows 10 sans avoir à créer une nouvelle branche et gérer des bases de code distinct.

Si vous souhaitez créer des fichiers binaires distincts pour les utilisateurs de Windows10, utilisez la compilation conditionnelle. Si vous préférez créer un ensemble de fichiers binaires que vous déployez sur tous les utilisateurs de Windows, utilisez des vérifications à l’exécution.

Jetons un coup d’œil à chaque option.

### <a name="conditional-compilation"></a>Compilation conditionnelle

Vous pouvez conserver une seule base de code et compiler un ensemble de fichiers binaires uniquement pour les utilisateurs de Windows10.

D'abord, ajoutez une nouvelle configuration de build à votre projet.

![Configuration de build](images/desktop-to-uwp/build-config.png)

Pour cette configuration de build, créez une constante pour identifier le code qui appelle APIs Windows Runtime.  

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

Vous pouvez compiler un ensemble de fichiers binaires pour l’ensemble de vos utilisateurs Windows, quelle que soit la version de Windows exécutée. Votre application appelle APIs Windows Runtime uniquement si l’utilisateur s’exécute votre application comme une application empaquetée sur Windows 10.

Le moyen le plus simple pour ajouter des vérifications à l’exécution de votre code consiste à installer ce package Nuget: [Desktop Bridge Helpers](https://www.nuget.org/packages/DesktopBridge.Helpers/) , puis utiliser le ``IsRunningAsUWP()`` méthode pour désactiver tout le code qui appelle APIs Windows Runtime. consultez ce billet de blog pour plus d’informations: [Pont du bureau: identifier le contexte de l’application](https://blogs.msdn.microsoft.com/appconsult/2016/11/03/desktop-bridge-identify-the-applications-context/).

## <a name="related-video"></a>Vidéo associée

<iframe src="https://mva.microsoft.com/en-US/training-courses-embed/developers-guide-to-the-desktop-bridge-17373/Demo-Use-UWP-APIs-in-Your-Code-3d78c6WhD_9506218965" width="636" height="480" allowFullScreen frameBorder="0"></iframe>

## <a name="related-samples"></a>Exemples connexes

* [Exemple Hello World](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/HelloWorldSample)
* [Vignette secondaire](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/SecondaryTileSample)
* [Exemple d'API Store](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/StoreSample)
* [Application WinForms qui implémente un UpdateTask UWP](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/WinFormsUpdateTaskSample)
* [Exemples de passerelle d’applications de bureau vers UWP](https://github.com/Microsoft/DesktopBridgeToUWP-Samples)


## <a name="support-and-feedback"></a>Support et commentaires

**Trouvez des réponses à vos questions**

Des questions? Contactez-nous sur Stack Overflow. Notre équipe contrôle ces [balises](http://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge). Vous pouvez également nous poser vos questions [ici](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D).

**Transmettre des commentaires ou suggérer des fonctionnalités**

Consultez [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial).
