---
author: mcleanbyron
description: "Découvrez comment ajouter des publicités natives à votre application UWP."
title: "Publicités natives"
ms.author: mcleans
ms.date: 06/26/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "Windows10, uwp, pub, publicité, contrôle de publicités, publicité native"
ms.openlocfilehash: 47a69e48f04c670a462c34083af1117d2c7908d8
ms.sourcegitcommit: 8c4d50ef819ed1a2f8cac4eebefb5ccdaf3fa898
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/27/2017
---
# <a name="native-ads"></a>Publicités natives

Une publicité native est un format de publicité basée sur un composant où chaque élément de l'annonce publicitaire (par exemple, le titre, l’image, la description et le texte de l’appel à l’action) est fourni à votre application sous forme d'élément individuel. Vous pouvez intégrer ces éléments dans votre application en utilisant vos propres polices, couleurs, animations et autres composants d’interface utilisateur pour composer une expérience utilisateur discrète qui correspond à l’apparence de votre application, tout en profitant des revenus élevés générés par les publicités.

Pour les annonceurs, les publicités natives offrent des emplacements de hautes performances, car l’expérience de publicité est étroitement intégrée à l’application, par conséquent, les utilisateurs ont tendance à interagir davantage avec ces types de publicités.

> [!NOTE]
> Pour servir des publicités natives à la version publique de votre application dans le Windows Store, vous devez créer une unité publicitaire **native** à partir de la page **Monétiser avec des publicités** du tableau de bord du Centre de développement. La possibilité de créer des unités publicitaires **natives** n'est actuellement disponible que pour des développeurs sélectionnés qui participent à un programme pilote, mais nous envisageons de la rendre disponible pour tous les développeurs bientôt. Si vous souhaitez rejoindre notre programme pilote, contactez-nous à l’adresse aiacare@microsoft.com.

> [!NOTE]
> Les publicités natives ne sont actuellement prises en charge que pour les applications UWP basées sur XAML pour Windows10. La prise en charge pour les applications UWP écrites en HTML et JavaScript est prévue pour une future version du SDK MicrosoftAdvertising.

## <a name="prerequisites"></a>Éléments prérequis

* Installez le [SDK MicrosoftAdvertising](http://aka.ms/ads-sdk-uwp) (version10.0.4 ou ultérieure) avec Visual Studio2015 ou une version ultérieure de Visual Studio. Pour des instructions d’installation, voir [cet article](install-the-microsoft-advertising-libraries.md). Vous pouvez installer le SDK sur votre ordinateur de développement via le programme d’installation MSI, ou l'installer pour l'utiliser dans un projet spécifique via le package NuGet.

## <a name="integrate-a-native-ad-into-your-app"></a>Intégrer une publicité native dans votre application

Suivez ces instructions pour intégrer une publicité native dans votre application et vérifiez que son implémentation affiche une publicité de test.

1. Dans Visual Studio, ouvrez votre projet ou créez-en un.

2. Si votre projet cible **Toute UC**, mettez-le à jour pour utiliser une sortie de génération propre à l’architecture (par exemple, **x86**). Si votre projet cible **Toute UC**, vous ne pourrez pas ajouter une référence au SDK Microsoft Advertising dans les étapes suivantes. Pour plus d’informations, voir [Erreurs de référence provoquées par le ciblage de Toute UC dans votre projet](known-issues-for-the-advertising-libraries.md#reference_errors).

3.  Dans la fenêtre **Explorateur de solutions**, cliquez avec le bouton droit sur **Références**, puis sélectionnez **Ajouter une référence...**

4.  Dans **Gestionnaire de références**, développez **Windows universel**, cliquez sur **Extensions**, puis cochez la case en regard de **SDK Microsoft Advertising pour XAML** (version10.0).

5.  Dans **Gestionnaire de références**, cliquez sur OK.

6. Dans le fichier de code approprié de votre application (par exemple, dans MainPage.xaml.cs ou un fichier de code d’une autre page), ajoutez les références d’espace de noms suivantes.

    [!code-cs[NativeAd](./code/AdvertisingSamples/NativeAdSamples/cs/MainPage.xaml.cs#Namespaces)]

7.  À un emplacement approprié de votre application (par exemple, dans ```MainPage``` ou dans une autre page), déclarez un objet [NativeAdsManager](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.nativeadsmanager.aspx) et plusieurs champs de chaîne représentant l’ID d'application et l’ID d’unité publicitaire de votre publicité native. L’exemple de code suivant affecte les champs `myAppId` et `myAdUnitId` aux [valeurs de test](test-mode-values.md) des publicités natives.

    > [!NOTE]
    > Chaque **NativeAdsManager** est associé à une *unité publicitaire* qui est utilisée par nos services pour servir des publicités au contrôle de publicités natives, et chaque unité publicitaire se compose d’un *ID d'unité publicitaire* et d'un *ID d'application*. Dans ces étapes, vous attribuez à votre contrôle des valeurs de test ID d’unité publicitaire et ID d'application. Ces valeurs de test ne peuvent être utilisées que dans une version de test de votre application. Avant de publier votre application sur le Windows Store, vous devez [remplacer ces valeurs de test par des valeurs dynamiques](#release) du Centre de développement Windows.

    [!code-cs[NativeAd](./code/AdvertisingSamples/NativeAdSamples/cs/MainPage.xaml.cs#Variables)]

8.  Dans le code exécuté au démarrage (par exemple, dans le constructeur de la page), instanciez l’objet **NativeAdsManager** et connectez les gestionnaires d’événements correspondant aux événements de l’objet [AdReady](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.nativeadsmanager.adready.aspx) et [ErrorOccurred](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.nativeadsmanager.erroroccurred.aspx).

    [!code-cs[NativeAd](./code/AdvertisingSamples/NativeAdSamples/cs/MainPage.xaml.cs#ConfigureNativeAd)]

9.  Lorsque vous êtes prêt à afficher une publicité native, appelez la méthode [RequestAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.nativeadsmanager.requestad.aspx) pour extraire une publicité.

    [!code-cs[NativeAd](./code/AdvertisingSamples/NativeAdSamples/cs/MainPage.xaml.cs#RequestAd)]

10.  Quand une publicité native est prête pour votre application, votre gestionnaire d’événements **AdReady** est appelé et un objet [NativeAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.nativead.aspx) qui représente la publicité native est transmis au paramètre *e*. Utilisez les propriétés **NativeAd** pour récupérer chaque élément de la publicité native et afficher ces éléments sur votre page. Veillez à appeler également la méthode [RegisterAdContainer](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.nativead.registeradcontainer.aspx) pour inscrire l’élément d’interface utilisateur qui agit comme un conteneur pour la publicité native. C'est indispensable pour suivre correctement les expositions publicitaires et les clics.
  > [!NOTE]
  > Certains éléments de la publicité native sont obligatoires et doivent toujours être affichés dans votre application. Pour plus d’informations, voir les [exigences et directives](#requirements-and-guidelines).

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

11.  Définir un gestionnaire d’événements pour l'événement **ErrorOccurred** afin de gérer les erreurs liées à la publicité native. L’exemple suivant écrit les informations d’erreur dans la fenêtre **Sortie** de Visual Studio au cours du test.

    [!code-cs[NativeAd](./code/AdvertisingSamples/NativeAdSamples/cs/MainPage.xaml.cs#ErrorOccurred)]

12.  Compilez et exécutez l’application pour la voir avec une publicité de test.

<span id="release" />
## <a name="release-your-app-with-live-ads"></a>Publier votre application avec des publicités dynamiques

Après avoir vérifié que votre implémentation de publicité native affiche correctement une publicité de test, suivez ces instructions pour configurer votre application pour qu'elle affiche des publicités réelles, puis soumettez votre application mise à jour dans le Windows Store.

1.  Assurez-vous que votre implémentation de publicité native suit les [exigences et directives](#requirements-and-guidelines) concernant les publicités natives.

2.  Dans le tableau de bord du Centre de développement, accédez à la page [Monétiser avec des publicités](../publish/monetize-with-ads.md) de votre application, puis [créez une unité publicitaire](../monetize/set-up-ad-units-in-your-app.md). Comme type d’unité publicitaire, spécifiez **Native**. Prenez note de l’ID d’unité publicitaire et de l’ID d’application.
    > [!IMPORTANT]
    > Vous pouvez utiliser chaque unité publicitaire dans une seule application. Si vous utilisez une unité publicitaire dans plusieurs applications, les publicités ne seront pas servies à cette unité publicitaire.

3. Vous pouvez éventuellement activer la médiation publicitaire pour la publicité native en configurant les paramètres de la section [Médiation publicitaire](../publish/monetize-with-ads.md#mediation) sur la page [Monétiser avec des publicités](../publish/monetize-with-ads.md). La médiation publicitaire vous permet d'optimiser vos revenus publicitaires et vos capacités de promotion d'application en affichant des publicités issues de plusieurs réseaux publicitaires.

4.  Dans votre code, remplacez les valeurs de test de l’unité publicitaire (c'est-à-dire les paramètres *applicationId* et *adUnitId* du constructeur [NativeAdsManager](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.nativeadsmanager.nativeadsmanager.aspx)) par les valeurs dynamiques que vous avez générées dans le Centre de développement.

5.  [Soumettez votre application](../publish/app-submissions.md) au Windows Store à l’aide du tableau de bord du Centre de développement.

6.  Passez en revue vos [rapports de performances des publicités](../publish/advertising-performance-report.md) dans le tableau de bord du Centre de développement.

<span id="requirements-and-guidelines" />
## <a name="requirements-and-guidelines"></a>Exigences et directives

Les publicités natives vous donnent beaucoup de contrôle sur la façon de présenter le contenu publicitaire à vos utilisateurs. Respectez ces exigences et ces directives pour vous assurer que le message de l’annonceur atteigne vos utilisateurs tout en évitant que les publicités natives ne perturbent l'expérience de vos utilisateurs.

### <a name="register-the-container-for-your-native-ad"></a>Inscrire le conteneur de votre publicité native

Dans votre code, vous devez appeler la méthode [RegisterAdContainer](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.nativead.registeradcontainer.aspx) de l'objet **NativeAd** pour enregistrer l’élément d’interface utilisateur agissant comme conteneur de la publicité native, ainsi que tous les contrôles spécifiques éventuels que vous souhaitez inscrire comme cibles interactives pour la publicité. C'est indispensable pour suivre correctement les expositions publicitaires et les clics.

Il existe deux surcharges pour la méthode **RegisterAdContainer** que vous pouvez utiliser:

* Si vous voulez que tout le conteneur de l'ensemble des éléments individuels de publicité native soit interactif, appelez la méthode [RegisterAdContainer(FrameworkElement)](https://msdn.microsoft.com/library/windows/apps/mt809188.aspx) et transmettez-lui le contrôle du conteneur. Par exemple, si vous affichez tous les éléments de publicité native dans des contrôles distincts tous hébergés dans un **StackPanel** et que vous voulez que tout le **StackPanel** soit interactif, transmettez le **StackPanel** à cette méthode.

* Si vous souhaitez que seuls certains éléments de publicité native soit interactifs, appelez la méthode [RegisterAdContainer(FrameworkElement, IVector(FrameworkElement))](https://msdn.microsoft.com/library/windows/apps/mt809189.aspx). Seuls les contrôles que vous transmettez au deuxième paramètre seront interactifs.

### <a name="required-native-ad-elements"></a>Éléments de publicité native obligatoires

Au minimum, vous devez toujours montrer les éléments de publicité native suivants à l’utilisateur dans votre conception de publicité native. Si vous ne parvenez pas à inclure ces éléments, vous obtiendrez peut-être de mauvaises performances et de faibles revenus de votre unité publicitaire.

1. Affichez toujours le titre de la publicité native (disponible dans la propriété [Title](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.nativead.title.aspx) de l'objet **NativeAd**). Fournissez suffisamment d’espace pour afficher au moins 25caractères. Si le titre est plus long, remplacez le texte supplémentaire par des points de suspension.
2. Affichez toujours au moins l'un des éléments suivants pour différencier la publicité native du reste de votre application et indiquer clairement que le contenu est fourni par un annonceur:
  * L’icône identifiable *publicité* (disponible dans la propriété [AdIcon](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.nativead.adicon.aspx) de l'objet **NativeAd**). Cette icône est fournie par Microsoft.
  * Le texte *parrainé par* (disponible dans la propriété [SponsoredBy](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.nativead.sponsoredby.aspx) de l'objet **NativeAd**). Ce texte est fourni par l'annonceur.
  * À la place du texte *parrainé par*, vous pouvez choisir d’afficher un autre texte qui permet de différencier la publicité native du reste de votre application, par exemple «Contenu sponsorisé», «Contenu promotionnel», «Contenu recommandé», etc.

### <a name="user-experience"></a>Expérience utilisateur

Votre publicité native doit être délimitée clairement du reste de votre application et être entourée d’espace pour éviter les clics accidentels. Utilisez des bordures, des arrière-plans différents ou une autre interface utilisateur pour séparer le contenu de la publicité du reste de votre application. N’oubliez pas que les clics accidentels ne sont pas utiles pour votre chiffre d’affaires publicitaire ni pour l'expérience de votre utilisateur final à long terme.

### <a name="description"></a>Description

Si vous choisissez d’afficher la description de la publicité (disponible dans la propriété [Description](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.nativead.description.aspx) de l'objet **NativeAd**), fournissez suffisamment d’espace pour afficher au moins 75caractères. Nous vous recommandons d’utiliser une animation pour afficher l’intégralité du contenu de description de la publicité.

### <a name="call-to-action"></a>Appel à l’action

Le texte *appel à l’action* (disponible dans la propriété [CallToAction](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.nativead.calltoaction.aspx) de l'objet **NativeAd**) est un composant essentiel de la publicité. Si vous choisissez d’afficher ce texte, suivez ces instructions:

* Montrez toujours le texte *appel à l’action* à l’utilisateur sur un contrôle interactif tel qu’un bouton ou un lien hypertexte.
* Affichez toujours le texte *appel à l’action* dans son intégralité.
* Vérifiez que le texte *appel à l’action* est séparé du reste du texte promotionnel de l’annonceur.

### <a name="learn-and-optimize"></a>En savoir plus et optimiser

Nous vous recommandons de créer et d'utiliser des unités publicitaires différentes pour chaque emplacement différent de la publicité native dans votre application. Cela vous permet d’obtenir des données de création de rapports distinctes pour chaque emplacement de la publicité native, et vous pouvez utiliser ces données pour apporter des modifications qui optimisent les performances de chaque emplacement de publicité native.

## <a name="related-topics"></a>Rubriques associées

* [Monétiser avec des publicités](../publish/monetize-with-ads.md)
* [Configurer des unités publicitaires pour votre application](../monetize/set-up-ad-units-in-your-app.md)
