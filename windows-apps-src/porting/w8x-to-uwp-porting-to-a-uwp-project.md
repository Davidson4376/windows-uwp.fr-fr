---
author: stevewhims
description: Lorsque vous commencez le processus de portage, vous avez le choix entre deux options.
title: Portage d’un projet Windows Runtime8.x vers un projet UWP
ms.assetid: 2dee149f-d81e-45e0-99a4-209a178d415a
ms.author: stwhi
ms.date: 02/08/2017
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: a4e0ff78f2872e572c370411a1aad38ccbd7fb6a
ms.sourcegitcommit: 753e0a7160a88830d9908b446ef0907cc71c64e7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/29/2018
ms.locfileid: "5748697"
---
# <a name="porting-a-windows-runtime-8x-project-to-a-uwp-project"></a>Portage d’un projet Windows Runtime8.x vers un projet UWP



Lorsque vous commencez le processus de portage, vous avez le choix entre deux options. La première consiste à modifier une copie de vos fichiers de projet existants, y compris le manifeste de package d’application (pour cette option, voir les informations sur la mise à jour de vos fichiers de projet dans [Migrer des applications vers la plateforme Windows universelle](https://msdn.microsoft.com/library/mt148501.aspx)). L’autre option consiste à créer un nouveau projet Windows 10 dans Visual Studio et copier vos fichiers. La première section de cette rubrique décrit la seconde option, mais le reste de la rubrique comporte des informations supplémentaires applicables aux deux options. Vous pouvez également choisir de conserver votre nouveau projet Windows 10 dans la même solution que vos projets existants et de partager des fichiers de code source à l’aide d’un projet partagé. Ou vous pouvez conserver le nouveau projet dans une solution propre à celui-ci et partager les fichiers de code source à l’aide de la fonctionnalité de fichiers liés offerte par Visual Studio.

## <a name="create-the-project-and-copy-files-to-it"></a>Création du projet et copie de fichiers dans ce dernier

Cette procédure est axée sur la possibilité de créer un nouveau projet Windows 10 dans Visual Studio et copier vos fichiers. Certains détails précis concernant le nombre de projets créés et les fichiers copiés dépendent de facteurs et de décisions décrits dans [Si vous disposez d’une application 8.1 universelle](w8x-to-uwp-root.md) et dans les sections qui suivent. Cette procédure envisage le cas le plus simple.

1.  Lancez Microsoft Visual Studio2015 et créez un nouveau projet Application vide (Windows universel). Pour plus d’informations, voir [vous lancer rapidement dans votre application de 8.x Windows Runtime à l’aide de modèles (C# C++, Visual Basic)](https://msdn.microsoft.com/library/windows/apps/hh768232). Votre nouveau projet crée un package d’application (fichier APPX) exécutable sur toutes les familles d’appareils.
2.  Dans votre projet d’application 8.1 universelle, identifiez tous les fichiers de code source et les fichiers de ressources visuelles que vous souhaitez réutiliser. Au moyen de l’Explorateur de fichiers, copiez les modèles de données, les modèles d’affichage, les ressources visuelles, les dictionnaires de ressources, la structure des dossiers et toute information que vous souhaitez réutiliser dans votre nouveau projet. Copiez ou créez des sous-dossiers sur le disque, si nécessaire.
3.  Copiez également les vues (par exemple, les fichiers MainPage.xaml et MainPage.xaml.cs) dans le nouveau projet. Là encore, créez autant de sous-dossiers que nécessaire, puis supprimez les affichages existants du projet. Toutefois, avant de remplacer ou de supprimer un affichage généré par Visual Studio, créez-en une copie, car vous pourrez avoir besoin de vous y référer ultérieurement. La première phase du portage d’une application 8.1 universelle se focalise sur l’obtention d’une application qui s’affiche et fonctionne correctement sur une famille d’appareils spécifique. Par la suite, vous ferez en sorte que les vues s’adaptent bien à tous les facteurs de forme et aurez la possibilité d’ajouter du code adaptatif pour tirer le meilleur parti d’une famille d’appareils spécifique.
4.  Dans l’**Explorateur de solutions**, assurez-vous que l’option **Afficher tous les fichiers** est activée. Sélectionnez les fichiers que vous avez copiés, cliquez dessus avec le bouton droit de la souris et sélectionnez **Inclure dans le projet**. Les dossiers conteneurs sont automatiquement inclus. Vous pouvez ensuite désactiver l’option **Afficher tous les fichiers**, si vous le souhaitez. Vous pouvez également opter pour un flux de travail alternatif, qui repose sur l’utilisation de la commande **Ajouter un élément existant** après la création des sous-dossiers requis dans l’**Explorateur de solutions** de Visual Studio. Pour les ressources visuelles, vérifiez que l’option **Action de génération** est définie sur **Contenu** et que l’option **Copier dans le répertoire de sortie** est définie sur **Ne pas copier**.
5.  À ce stade, il est possible que vous rencontriez des erreurs de génération. Toutefois, si vous savez ce que vous devez modifier, vous pouvez utiliser la commande **Rechercher et remplacer** de Visual Studio pour apporter des modifications en bloc à votre code source. Puis, dans l’éditeur de code impératif, utilisez les commandes **Résoudre** et **Organiser les instructions Using** du menu contextuel pour effectuer des modifications plus ciblées.

## <a name="maximizing-markup-and-code-reuse"></a>Valorisation de la réutilisation du code et du balisage

Vous constaterez qu’une légère refactorisation et/ou l’ajout de code adaptatif (expliqué ci-dessous) vous permettront d’optimiser la réutilisation du code et du balisage fonctionnant sur toutes les familles d’appareils. Voici quelques informations supplémentaires.

-   Les fichiers communs à toutes les familles d’appareils ne requièrent aucune considération particulière. Ces fichiers seront utilisés par l’application sur toutes les familles d’appareils sur lesquelles elle est exécutée. Cela inclut les fichiers de balisage XAML, les fichiers de code source impératif et les fichiers de ressources.
-   Il est possible de faire en sorte que votre application détecte la famille d’appareils sur laquelle elle est exécutée et navigue vers une vue spécialement conçue pour cette famille d’appareils. Pour plus d’informations, voir [Détection de la plateforme d’exécution de votre application](w8x-to-uwp-input-and-sensors.md).
-   Une technique similaire qui peut se révéler utile s’il n’existe aucune autre solution consiste à donner à un fichier de balisage ou à un fichier **ResourceDictionary** (ou au dossier contenant le fichier) un nom spécifique, de manière qu’il soit chargé automatiquement à l’exécution uniquement lorsque votre application est exécutée sur une famille d’appareils particulière. Cette technique est illustrée dans l’étude de cas [Bookstore1](w8x-to-uwp-case-study-bookstore1.md).
-   Vous devez être en mesure de supprimer une grande partie des directives de compilation conditionnelle dans le code source de votre application 8.1 universelle si vous avez uniquement besoin prendre en charge de Windows 10. Voir [Compilation conditionnelle et code adaptatif](#conditional-compilation-and-adaptive-code) dans cette rubrique.
-   Pour utiliser des fonctionnalités qui ne sont pas disponibles sur toutes les familles d’appareils (imprimantes, scanneurs, bouton de l’appareil photo, etc.), vous pouvez écrire du code adaptatif. Voir le troisième exemple de la section [Compilation conditionnelle et code adaptatif](#conditional-compilation-and-adaptive-code) dans cette rubrique.
-   Si vous souhaitez prendre en charge Windows 8.1, Windows Phone 8.1 et Windows 10, vous pouvez conserver trois projets dans la même solution et partager du code avec un projet partagé. Vous pouvez également partager des fichiers de code source entre des projets. Pour ce faire, procédez comme suit : dans Visual Studio, cliquez avec le bouton droit sur le projet dans l’**Explorateur de solutions**, sélectionnez **Ajouter un élément existant**, sélectionnez les fichiers à partager, puis cliquez sur **Ajouter en tant que lien**. Stockez vos fichiers de code source dans un dossier commun sur le système de fichiers sur lequel les projets liés peuvent les voir. N’oubliez pas de les ajouter dans le contrôle de code source.
-   Pour effectuer une réutilisation au niveau binaire plutôt qu’au niveau du code source, consultez l’article [Création de composants Windows Runtime en C# et Visual Basic](http://msdn.microsoft.com/library/windows/apps/xaml/br230301.aspx). Il existe également des bibliothèques de classes portables, qui prend en charge le sous-ensemble d’API .NET disponibles dans le .NET Framework pour Windows 8.1, Windows Phone 8.1 et les applications Windows 10 (.NET Core), ainsi que .NET Framework. Les assemblies des bibliothèques de classes portables sont des fichiers binaires compatibles avec toutes ces plateformes. Utilisez VisualStudio pour créer un projet qui cible une bibliothèque de classes portable. Voir [Développement interplateforme avec la bibliothèque de classes portable](http://msdn.microsoft.com/library/gg597391.aspx).

## <a name="extension-sdks"></a>Kits de développement logiciel (SDK) d’extension

La plupart des API WindowsRuntime déjà appelées par votre application8.1 universelle sont implémentées dans l’ensemble d’API désigné sous le terme de famille d’appareils universels. Toutefois, certaines d’entre elles sont implémentées dans des SDK d’extension, et Visual Studio ne reconnaît que les API implémentées par la famille d’appareils cible de votre application ou par les SDK d’extension que vous avez référencés.

Si vous obtenez des erreurs de compilation à propos d’espaces de noms, de types ou de membres introuvables, cela en est probablement la cause. Ouvrez la rubrique concernant l’API dans la documentation de référence sur les API et accédez à la section Configuration requise pour connaître la famille d’appareils d’implémentation. Si celle-ci ne correspond pas à votre famille d’appareils cible, vous avez besoin d’ajouter une référence au SDK d’extension pour cette famille d’appareils afin que l’API soit disponible pour votre projet.

Cliquez sur **Projet** &gt; **Ajouter une référence** &gt; **Windows universel** &gt; **Extensions**, puis sélectionnez le SDK d’extension approprié. Par exemple, si les API que vous voulez appeler sont uniquement disponibles dans la famille d’appareils mobiles et qu’elles ont été introduites dans la version10.0.x.y, cochez **Extensions Windows Mobile pour UWP**.

La référence suivante sera ajoutée à votre fichier de projet:

```XML
<ItemGroup>
    <SDKReference Include="WindowsMobile, Version=10.0.x.y">
        <Name>Windows Mobile Extensions for the UWP</Name>
    </SDKReference>
</ItemGroup>
```

Le nom et le numéro de version correspondent à ceux des dossiers dans l’emplacement d’installation de votre SDK. Par exemple, les informations ci-dessus correspondent à ce nom de dossier :

`\Program Files (x86)\Windows Kits\10\Extension SDKs\WindowsMobile\10.0.x.y`

À moins que votre application cible la famille d’appareils qui implémente l’API, vous devez utiliser la classe [**ApiInformation**](https://msdn.microsoft.com/library/windows/apps/dn949001) pour vérifier la présence de l’API avant de l’appeler (on parle alors de « code adaptatif »). Cette condition sera ensuite évaluée chaque fois que votre application est exécutée, mais elle renverra la valeur « true » uniquement sur les appareils où l’API est présente et peut donc être appelée. Utilisez uniquement des SDK d’extension et du code adaptatif après avoir vérifié si une API universelle existe. Quelques exemples sont fournis dans la section ci-dessous.

Voir également [Manifeste du package de l’application](#app-package-manifest).

## <a name="conditional-compilation-and-adaptive-code"></a>Compilation conditionnelle et code adaptatif

Si vous utilisez la compilation conditionnelle (avec des directives de préprocesseur c#) afin que vos fichiers de code fonctionnent sur Windows 8.1 et Windows Phone 8.1, vous pouvez maintenant consulter cette compilation conditionnelle la lumière du travail de convergence effectué dans Windows 10. Cette convergence signifie que, dans votre application Windows 10, certaines conditions peuvent être complètement supprimées. D’autres sont remplacées par des vérifications à l’exécution, comme illustré dans les exemples ci-dessous.

**Remarque**  si vous souhaitez prendre en charge de Windows 8.1, Windows Phone 8.1 et Windows 10 dans un seul fichier de code, vous pouvez le faire trop. Si vous regardez dans votre projet Windows 10 pages de propriétés, vous verrez que le projet définit WINDOWS\_UAP en tant que symbole de compilation conditionnelle. Par conséquent, vous pouvez l’utiliser en association avec WINDOWS\_APP et WINDOWS\_PHONE\_APP. Ces exemples illustrent le cas le plus simple de suppression de la compilation conditionnelle d’une application 8.1 universelle et en remplaçant le code équivalent pour une application Windows 10.

Le premier exemple illustre le modèle d’utilisation pour l’API **PickSingleFileAsync** (qui s’applique uniquement à Windows 8.1) et l’API **PickSingleFileAndContinue** (qui concerne uniquement Windows Phone 8.1).

```csharp
#if WINDOWS_APP
    // Use Windows.Storage.Pickers.FileOpenPicker.PickSingleFileAsync
#else
    // Use Windows.Storage.Pickers.FileOpenPicker.PickSingleFileAndContinue
#endif // WINDOWS_APP
```

Windows 10 converge sur les API, [**PickSingleFileAsync**](https://msdn.microsoft.com/library/windows/apps/jj635275) votre code à ceci:

```csharp
    // Use Windows.Storage.Pickers.FileOpenPicker.PickSingleFileAsync
```

Dans cet exemple, nous gérons le bouton matériel Précédent, mais uniquement sur WindowsPhone.

```csharp
#if WINDOWS_PHONE_APP
        Windows.Phone.UI.Input.HardwareButtons.BackPressed += this.HardwareButtons_BackPressed;
#endif // WINDOWS_PHONE_APP

...

#if WINDOWS_PHONE_APP
    void HardwareButtons_BackPressed(object sender, Windows.Phone.UI.Input.BackPressedEventArgs e)
    {
        // Handle the event.
    }
#endif // WINDOWS_PHONE_APP
```

Dans Windows 10, l’événement de bouton précédent est un concept universel. Les boutons Précédent implémentés de manière matérielle ou logicielle déclenchent tous l’événement [**BackRequested**](https://msdn.microsoft.com/library/windows/apps/dn893596), qui est donc l’élément à gérer.

```csharp
    Windows.UI.Core.SystemNavigationManager.GetForCurrentView().BackRequested +=
        this.ViewModelLocator_BackRequested;

...

private void ViewModelLocator_BackRequested(object sender, Windows.UI.Core.BackRequestedEventArgs e)
{
    // Handle the event.
}
```

Ce dernier exemple est semblable au précédent. Ici, nous gérons le bouton matériel d’appareil photo, mais une fois encore, uniquement dans le code compilé dans le package d’application WindowsPhone.

```csharp
#if WINDOWS_PHONE_APP
    Windows.Phone.UI.Input.HardwareButtons.CameraPressed += this.HardwareButtons_CameraPressed;
#endif // WINDOWS_PHONE_APP

...

#if WINDOWS_PHONE_APP
void HardwareButtons_CameraPressed(object sender, Windows.Phone.UI.Input.CameraEventArgs e)
{
    // Handle the event.
}
#endif // WINDOWS_PHONE_APP
```

Dans Windows 10, le bouton matériel d’appareil photo est un concept propre à la famille d’appareils mobiles. Comme un même package d’application s’exécutera sur tous les appareils, nous transformons notre condition de compilation en condition d’exécution en utilisant ce que l’on appelle du code adaptatif. Pour ce faire, nous utilisons la classe [**ApiInformation**](https://msdn.microsoft.com/library/windows/apps/dn949001) afin d’envoyer une requête au moment de l’exécution pour vérifier si la classe [**HardwareButtons**](https://msdn.microsoft.com/library/windows/apps/jj207557) est présente. **HardwareButtons** étant défini dans le SDK d’extension mobile, nous devons ajouter une référence à ce SDK dans notre projet pour permettre la compilation de ce code. Notez cependant que le gestionnaire sera uniquement exécuté sur les appareils qui implémentent les types définis dans le SDK d’extension mobile, c’est-à-dire appartenant à la famille d’appareils mobiles. Ce code est donc moralement équivalent au code 8.1 universel en ce sens qu’il prend soin de n’utiliser que des fonctionnalités présentes, bien que la méthode utilisée pour y parvenir soit différente.

```csharp
    // Note: Cache the value instead of querying it more than once.
    bool isHardwareButtonsAPIPresent = Windows.Foundation.Metadata.ApiInformation.IsTypePresent
        ("Windows.Phone.UI.Input.HardwareButtons");

    if (isHardwareButtonsAPIPresent)
    {
        Windows.Phone.UI.Input.HardwareButtons.CameraPressed +=
            this.HardwareButtons_CameraPressed;
    }

    ...

private void HardwareButtons_CameraPressed(object sender, Windows.Phone.UI.Input.CameraEventArgs e)
{
    // Handle the event.
}
```

Voir également [Détection de la plateforme d’exécution de votre application](w8x-to-uwp-input-and-sensors.md).

## <a name="app-package-manifest"></a>Manifeste du package de l’application

La rubrique de [ce qui a changé dans Windows 10](https://msdn.microsoft.com/library/windows/apps/dn705793) répertorie les modifications apportées à la référence de schéma de manifeste de package pour Windows 10, y compris les éléments qui ont été ajoutés, supprimés et modifiés. Pour consulter des informations de référence sur l’ensemble des éléments, attributs et types du schéma, voir [Hiérarchie d’éléments](https://msdn.microsoft.com/library/windows/apps/dn934819). Si vous portez une application du WindowsPhoneStore, ou si votre application est une mise à jour vers une application à partir du Windows Phone Store, vérifiez que l'élément **pm:PhoneIdentity** correspond au contenu du manifeste de votre application précédente (utilisez les mêmes GUID que ceux qui ont été affectés à l’application par le Windows Store). Cela permet de garantir que les utilisateurs de votre application qui effectuent la mise à niveau vers Windows10 reçoivent votre nouvelle application comme une mise à jour, non comme un doublon. Pour plus d'informations, voir la rubrique de référence [**pm:PhoneIdentity****](https://msdn.microsoft.com/library/windows/apps/dn934763).

Les paramètres de votre projet (y compris les références aux SDK d’extension) déterminent la surface d’exposition d’API que votre application peut appeler. Mais le manifeste de votre package d’application est ce qui détermine l’ensemble réel d’appareils sur lesquels vos clients peuvent installer votre application à partir du Windows Store. Pour plus d’informations, voir la section d’exemples de [**TargetDeviceFamily**](https://msdn.microsoft.com/library/windows/apps/dn986903).

Vous pouvez modifier le manifeste de package d’application pour définir diverses déclarations, fonctionnalités et autres paramètres requis par certaines fonctionnalités. Vous pouvez utiliser l’éditeur de manifeste de package d’application proposé par Visual Studio pour effectuer vos modifications. Si l’**Explorateur de solutions** ne s’affiche pas, sélectionnez-le dans le menu **Affichage**. Double-cliquez sur **Package.appxmanifest**. Cette opération affiche la fenêtre de l’éditeur de manifeste. Sélectionnez l’onglet approprié pour vos modifications, puis enregistrez-les.

Rubrique suivante : [Résolution des problèmes](w8x-to-uwp-troubleshooting.md).

## <a name="related-topics"></a>Rubriques connexes

* [Développer des applications pour la plateforme Windows universelle](http://msdn.microsoft.com/library/dn975273.aspx)
* [Vb et Windows Runtime 8.x application à l’aide de modèles (c#, C++, Visual Basic)](https://msdn.microsoft.com/library/windows/apps/hh768232)
* [Création de composants Windows Runtime](https://msdn.microsoft.com/library/windows/apps/xaml/hh441572.aspx)
* [Développement interplateforme avec la bibliothèque de classes portable](http://msdn.microsoft.com/library/gg597391.aspx)

