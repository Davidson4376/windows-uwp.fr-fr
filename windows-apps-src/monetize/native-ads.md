---
author: Xansky
description: Découvrez comment ajouter des publicités natives à votre application UWP.
title: Publicités natives
ms.author: mhopkins
ms.date: 05/11/2018
ms.topic: article
keywords: Windows10, uwp, pub, publicité, contrôle de publicités, publicité native
ms.localizationpriority: medium
ms.openlocfilehash: 36b96add3aa785ad20ddd1c42cd46e498d0264a6
ms.sourcegitcommit: 93c0a60cf531c7d9fe7b00e7cf78df86906f9d6e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/21/2018
ms.locfileid: "7558343"
---
# <a name="native-ads"></a>Publicités natives

Une publicité native est un format de publicité basée sur un composant où chaque élément de l'annonce publicitaire (par exemple, le titre, l’image, la description et le texte de l’appel à l’action) est fourni à votre application sous forme d'élément individuel. Vous pouvez intégrer ces éléments dans votre application en utilisant vos propres polices, couleurs, animations et autres composants d’interface utilisateur pour composer une expérience utilisateur discrète qui correspond à l’apparence de votre application, tout en profitant des revenus élevés générés par les publicités.

Pour les annonceurs, les publicités natives offrent des emplacements de hautes performances, car l’expérience de publicité est étroitement intégrée à l’application, par conséquent, les utilisateurs ont tendance à interagir davantage avec ces types de publicités.

> [!NOTE]
> Les publicités natives ne sont actuellement prises en charge que pour les applications UWP basées sur XAML pour Windows10. La prise en charge pour les applications UWP écrites en HTML et JavaScript est prévue pour une future version du SDK MicrosoftAdvertising.

## <a name="prerequisites"></a>Éléments prérequis

* Installer le [SDK MicrosoftAdvertising](http://aka.ms/ads-sdk-uwp) avec VisualStudio2015 ou une version ultérieure de Visual Studio. Pour des instructions d’installation, voir [cet article](install-the-microsoft-advertising-libraries.md).

## <a name="integrate-a-native-ad-into-your-app"></a>Intégrer une publicité native dans votre application

Suivez ces instructions pour intégrer une publicité native dans votre application et vérifiez que son implémentation affiche une publicité de test.

1. Dans Visual Studio, ouvrez votre projet ou créez-en un.
    > [!NOTE]
    > Si vous utilisez un projet existant, ouvrez le fichier Package.appxmanifest dans votre projet et assurez-vous que la fonctionnalité **Internet (Client)** est sélectionnée. Votre application a besoin de cette fonctionnalité pour recevoir des publicités tests et des publicités dynamiques.

2. Si votre projet cible **Toute UC**, mettez-le à jour pour utiliser une sortie de génération propre à l’architecture (par exemple, **x86**). Si votre projet cible **Toute UC**, vous ne pourrez pas ajouter une référence au SDK Microsoft Advertising dans les étapes suivantes. Pour plus d’informations, voir [Erreurs de référence provoquées par le ciblage de Toute UC dans votre projet](known-issues-for-the-advertising-libraries.md#reference_errors).

3. Ajoutez une référence au SDK Microsoft Advertising de votre projet:

    1. Dans la fenêtre **Explorateur de solutions**, cliquez avec le bouton droit sur **Références**, puis sélectionnez **Ajouter une référence...**
    2.  Dans **Gestionnaire de références**, développez **Windows universel**, cliquez sur **Extensions**, puis cochez la case en regard de **SDK Microsoft Advertising pour XAML** (version10.0).
    3.  Dans **Gestionnaire de références**, cliquez sur OK.

4. Dans le fichier de code approprié de votre application (par exemple, dans MainPage.xaml.cs ou un fichier de code d’une autre page), ajoutez les références d’espace de noms suivantes.

    [!code-cs[NativeAd](./code/AdvertisingSamples/NativeAdSamples/cs/MainPage.xaml.cs#Namespaces)]

5.  À un emplacement approprié de votre application (par exemple, dans ```MainPage``` ou dans une autre page), déclarez un objet [NativeAdsManagerV2](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.nativeadsmanagerv2) et plusieurs champs de chaîne représentant l’ID d'application et l’ID d’unité publicitaire de votre publicité native. L’exemple de code suivant affecte les champs `myAppId` et `myAdUnitId` aux [valeurs de test](set-up-ad-units-in-your-app.md#test-ad-units) des publicités natives.
    > [!NOTE]
    > Chaque **NativeAdsManagerV2** est associé à une *unité publicitaire* qui est utilisée par nos services pour servir des publicités au contrôle de publicités natives, et chaque unité publicitaire se compose d’un *ID d'unité publicitaire* et d'un *ID d'application*. Dans ces étapes, vous attribuez à votre contrôle des valeurs de test ID d’unité publicitaire et ID d'application. Ces valeurs de test ne peuvent être utilisées que dans une version de test de votre application. Avant de publier votre application au Windows Store, vous devez [remplacer ces valeurs par des valeurs dynamiques de test](#release) à partir de l’espace partenaires.

    [!code-cs[NativeAd](./code/AdvertisingSamples/NativeAdSamples/cs/MainPage.xaml.cs#Variables)]

6.  Dans le code exécuté au démarrage (par exemple, dans le constructeur de la page), instanciez l’objet **NativeAdsManagerV2** et connectez les gestionnaires d’événements correspondant aux événements de l’objet **AdReady** et **ErrorOccurred**.

    [!code-cs[NativeAd](./code/AdvertisingSamples/NativeAdSamples/cs/MainPage.xaml.cs#ConfigureNativeAd)]

7.  Lorsque vous êtes prêt à afficher une publicité native, appelez la méthode **RequestAd** pour extraire une publicité.

    [!code-cs[NativeAd](./code/AdvertisingSamples/NativeAdSamples/cs/MainPage.xaml.cs#RequestAd)]

8.  Quand une publicité native est prête pour votre application, votre gestionnaire d’événements [AdReady](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.nativeadsmanagerv2.adready) est appelé et un objet [NativeAdV2](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.nativeadv2) qui représente la publicité native est transmis au paramètre *e*. Utilisez les propriétés **NativeAdV2** pour récupérer chaque élément de la publicité native et afficher ces éléments sur votre page. Veillez à appeler également la méthode **RegisterAdContainer** pour inscrire l’élément d’interface utilisateur qui agit comme un conteneur pour la publicité native. C'est indispensable pour suivre correctement les expositions publicitaires et les clics.
    > [!NOTE]
    > Certains éléments de la publicité native sont obligatoires et doivent toujours être affichés dans votre application. Pour plus d’informations, voir nos [recommandations en matière de publicité native](ui-and-user-experience-guidelines.md#guidelines-for-native-ads).

    Par exemple, supposons que votre application contienne un ```MainPage``` (ou une autre page) avec l'objet **StackPanel** suivant. Ce **StackPanel** contient une série de contrôles qui affichent les différents éléments d’une publicité native, notamment le titre, la description, les images, le texte *parrainé par* et un bouton qui affiche le texte *appel à l’action*.

    ``` xml
    <StackPanel x:Name="NativeAdContainer" Background="#555555" Width="Auto" Height="Auto"
                Orientation="Vertical">
        <Image x:Name="AdIconImage" HorizontalAlignment="Left" VerticalAlignment="Center"
               Margin="20,20,20,20"/>
        <TextBlock x:Name="TitleTextBlock" HorizontalAlignment="Left" VerticalAlignment="Center"
               Text="The ad title will go here" FontSize="24" Foreground="White" Margin="20,0,0,10"/>
        <TextBlock x:Name="DescriptionTextBlock" HorizontalAlignment="Left" VerticalAlignment="Center"
                   Foreground="White" TextWrapping="Wrap" Text="The ad description will go here"
                   Margin="20,0,0,0" Visibility="Collapsed"/>
        <Image x:Name="MainImageImage" HorizontalAlignment="Left"
               VerticalAlignment="Center" Margin="20,20,20,20" Visibility="Collapsed"/>
        <Button x:Name="CallToActionButton" Background="Gray" Foreground="White"
                HorizontalAlignment="Left" VerticalAlignment="Center" Width="Auto" Height="Auto"
                Content="The call to action text will go here" Margin="20,20,20,20"
                Visibility="Collapsed"/>
        <StackPanel x:Name="SponsoredByStackPanel" Orientation="Horizontal" Margin="20,20,20,20">
            <TextBlock x:Name="SponsoredByTextBlock" Text="The ad sponsored by text will go here"
                       FontSize="24" Foreground="White" Margin="20,0,0,0" HorizontalAlignment="Left"
                       VerticalAlignment="Center" Visibility="Collapsed"/>
            <Image x:Name="IconImageImage" Margin="40,20,20,20" HorizontalAlignment="Left"
                   VerticalAlignment="Center" Visibility="Collapsed"/>
        </StackPanel>
    </StackPanel>
    ```

    L’exemple de code suivant montre un gestionnaire d’événements **AdReady** qui affiche chaque élément de la publicité native dans les contrôles du **StackPanel**, puis appelle la méthode **RegisterAdContainer** pour inscrire le **StackPanel**. Ce code part du principe qu’il est exécuté à partir du fichier code-behind de la page qui contient le **StackPanel**.

    [!code-cs[NativeAd](./code/AdvertisingSamples/NativeAdSamples/cs/MainPage.xaml.cs#AdReady)]

9.  Définir un gestionnaire d’événements pour l'événement **ErrorOccurred** afin de gérer les erreurs liées à la publicité native. L’exemple suivant écrit les informations d’erreur dans la fenêtre **Sortie** de Visual Studio au cours du test.

    [!code-cs[NativeAd](./code/AdvertisingSamples/NativeAdSamples/cs/MainPage.xaml.cs#ErrorOccurred)]

10.  Compilez et exécutez l’app pour la voir avec une publicité de test.

<span id="release" />

## <a name="release-your-app-with-live-ads"></a>Publier votre application avec des publicités dynamiques

Après avoir vérifié que votre implémentation de publicité native affiche correctement une publicité de test, suivez ces instructions pour configurer votre application pour qu'elle affiche des publicités réelles, puis soumettez votre application mise à jour dans le Windows Store.

1.  Assurez-vous que votre implémentation de publicité native suit nos [directives concernant les publicités natives](ui-and-user-experience-guidelines.md#guidelines-for-native-ads).

2.  Dans l’espace partenaires, accédez à la page de [publicités dans l’application](../publish/in-app-ads.md) et [créer une unité publicitaire](set-up-ad-units-in-your-app.md#live-ad-units). Comme type d’unité publicitaire, spécifiez **Native**. Prenez note de l’ID d’unité publicitaire et de l’ID d’application.
    > [!NOTE]
    > Les valeurs d'ID d’application pour les unités publicitaires de test et les unités publicitaires dynamiques UWP ont des formats différents. Les valeurs d’ID d'application tests sont des GUID. Lorsque vous créez une unité de publicité dynamique UWP dans l’espace partenaires, la valeur de ID d’application pour l’unité publicitaire correspond toujours à l’ID Windows Store pour votre application (une valeur d’ID du Windows Store de l’exemple ressemble à 9NBLGGH4R315).

3. Vous pouvez éventuellement activer la médiation publicitaire pour la publicité native en configurant les paramètres de la section [Paramètres de médiation](../publish/in-app-ads.md#mediation) sur la page [Publicités in-app](../publish/in-app-ads.md). La médiation publicitaire vous permet d'optimiser vos revenus publicitaires et vos capacités de promotion d'app en affichant des publicités issues de plusieurs réseaux publicitaires.

4.  Dans votre code, remplacez les valeurs d’unité publicitaire test (en d’autres termes, les paramètres *applicationId* et *adUnitId* du constructeur [NativeAdsManagerV2](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.nativeadsmanagerv2.-ctor) ) par les valeurs dynamiques que vous avez générées dans l’espace partenaires.

5.  [Soumettre votre application](../publish/app-submissions.md) au Store à l’aide de l’espace partenaires.

6.  Passez en revue vos [rapports de performances des publicités](../publish/advertising-performance-report.md) dans l’espace partenaires.

## <a name="manage-ad-units-for-multiple-native-ads-in-your-app"></a>Gérer des unités publicitaires pour plusieurs publicités natives dans votre application

Vous pouvez utiliser plusieurs placements de publicité native dans une seule application. Dans ce scénario, nous vous recommandons d’attribuer une unité publicitaire différente à chaque placement de publicité native. L’utilisation de différentes unités publicitaires pour chaque publicité native vous permet de [configurer les paramètres de médiation](../publish/in-app-ads.md#mediation) séparément et d'obtenir des [données de rapport](../publish/advertising-performance-report.md) discrètes pour chaque contrôle. Cela permet également à nos services de mieux optimiser les publicités que nous servons à votre application.

> [!IMPORTANT]
> Vous pouvez utiliser chaque unité publicitaire dans une seule application. Si vous utilisez une unité publicitaire dans plusieurs applications, les publicités ne seront pas servies à cette unité publicitaire.

## <a name="related-topics"></a>Rubriquesassociées

* [Recommandations concernant les publicités natives](ui-and-user-experience-guidelines.md#guidelines-for-native-ads)
* [Publicités dans l’app](../publish/in-app-ads.md)
* [Configurer des unités publicitaires pour votre app](set-up-ad-units-in-your-app.md)
