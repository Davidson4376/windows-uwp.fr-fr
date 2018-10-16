---
author: Xansky
ms.assetid: 141900dd-f1d3-4432-ac8b-b98eaa0b0da2
description: Découvrez les solutions aux problèmes de développement courants liés aux bibliothèques de publicités Microsoft dans les applications XAML.
title: Guide de résolution des problèmes pour XAML et C#
ms.author: mhopkins
ms.date: 08/23/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp, publicités, publicité, AdControl, résolution des problèmes, XAML, c#
ms.localizationpriority: medium
ms.openlocfilehash: 4f3ebd2877dd786161c76d89e17a2667c07e65b3
ms.sourcegitcommit: 106aec1e59ba41aae2ac00f909b81bf7121a6ef1
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/15/2018
ms.locfileid: "4620536"
---
# <a name="xaml-and-c-troubleshooting-guide"></a>Guide de résolution des problèmes pour XAML et C#

Cette rubrique contient les solutions aux problèmes de développement courants liés aux bibliothèques de publicités Microsoft dans les applications XAML.

* [XAML](#xaml)
  * [AdControl invisible](#xaml-notappearing)
  * [Une boîte noire clignote et disparaît](#xaml-blackboxblinksdisappears)
  * [Non-actualisation des publicités](#xaml-adsnotrefreshing)

* [C#](#csharp)
  * [AdControl invisible](#csharp-adcontrolnotappearing)
  * [Une boîte noire clignote et disparaît](#csharp-blackboxblinksdisappears)
  * [Non-actualisation des publicités](#csharp-adsnotrefreshing)

<span id="xaml"/>

## <a name="xaml"></a>XAML

<span id="xaml-notappearing"/>

### <a name="adcontrol-not-appearing"></a>AdControl invisible

1.  Assurez-vous que la fonctionnalité **Internet (client)** est sélectionnée dans le fichier Package.appxmanifest.

2.  Vérifiez l’ID de l’application et l’ID d’unité publicitaire. Ces ID doivent correspondre à l’ID de l’application et à l’ID d’unité publicitaire que vous avez obtenus dans le Centre de développement Windows. Pour plus d’informations, voir [Configurer des unités publicitaires dans votre application](set-up-ad-units-in-your-app.md#live-ad-units).

    > [!div class="tabbedCodeSnippets"]
    ``` xml
    <UI:AdControl AdUnitId="{AdUnitID}" ApplicationId="{ApplicationID}"
                  Width="728" Height="90" />
    ```

3.  Vérifiez les propriétés **Height** et **Width**. Elles doivent être définies sur l’une des [tailles de bannières publicitaires prises en charge](supported-ad-sizes-for-banner-ads.md).

    > [!div class="tabbedCodeSnippets"]
    ``` xml
    <UI:AdControl AdUnitId="{AdUnitID}"
                  ApplicationId="{ApplicationID}"
                  Width="728" Height="90" />
    ```

4.  Vérifiez la position des éléments. Le contrôle [AdControl](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol) doit se situer à l’intérieur de la zone d’affichage.

5.  Vérifiez la propriété **Visibility**. La propriété facultative **Visibility** ne doit pas être définie sur collapsed ou hidden. Cette propriété peut être incluse (comme illustré ci-dessous) ou définie dans une feuille de style externe.

    > [!div class="tabbedCodeSnippets"]
    ``` xml
    <UI:AdControl AdUnitId="{AdUnitID}"
                  ApplicationId="{ApplicationID}"
                  Visibility="Visible"
                  Width="728" Height="90" />
    ```

6.  Vérifiez le parent du **AdControl**. Si l’élément **AdControl** réside dans un élément parent, ce dernier doit être actif et visible.

    > [!div class="tabbedCodeSnippets"]
    ``` xml
    <StackPanel>
        <UI:AdControl AdUnitId="{AdUnitID}"
                      ApplicationId="{ApplicationID}"
                      Width="728" Height="90" />
    </StackPanel>
    ```

7.  Vérifiez que le **AdControl** n’est pas masqué dans la fenêtre d’affichage. Le **AdControl** doit être visible afin que les publicités s’affichent correctement.

8.  Les valeurs dynamiques des paramètres **ApplicationId** et **AdUnitId** ne doivent pas être testées dans l’émulateur. Pour vous assurer du bon fonctionnement du contrôle **AdControl**, utilisez les [valeurs de test](set-up-ad-units-in-your-app.md#test-ad-units) pour **ApplicationId** et **AdUnitId**.

<span id="xaml-blackboxblinksdisappears"/>

### <a name="black-box-blinks-and-disappears"></a>Une boîte noire clignote et disparaît

1.  Vérifiez toutes les étapes indiquées dans la section précédente [AdControl invisible](#xaml-notappearing).

2.  Gérez l’événement **ErrorOccurred**, puis utilisez le message transmis au gestionnaire d’événements pour déterminer si une erreur s’est produite et identifier le type d’erreur levée. Voir [Gestion des erreurs dans la procédure pas à pas pour XAML/C#](error-handling-in-xamlc-walkthrough.md) pour plus d’informations.

    Cet exemple illustre un gestionnaire d’événements **ErrorOccurred**. Le premier extrait est le balisage d’interface utilisateur XAML.

    > [!div class="tabbedCodeSnippets"]
    ``` xml
    <UI:AdControl AdUnitId="{AdUnitID}"
                  ApplicationId="{ApplicationID}"
                  Width="728" Height="90"
                  ErrorOccurred="adControl_ErrorOccurred" />
    <TextBlock x:Name="TextBlock1" TextWrapping="Wrap" Width="500" Height="250" />
    ```

    Cet exemple présente le code C# correspondant.

    > [!div class="tabbedCodeSnippets"]
    ``` cs
    private void adControl_ErrorOccurred(object sender,               
        Microsoft.Advertising.WinRT.UI.AdErrorEventArgs e)
    {
        TextBlock1.Text = e.ErrorMessage;
    }
    ```

    L’erreur la plus courante provoquant une boîte noire est la suivante: «Aucune publicité disponible». Cette erreur signifie qu’aucune publicité n’est disponible pour être retourné à partir de la demande.

3.  Le contrôle [AdControl](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol) se comporte normalement.

    Par défaut, le **AdControl** est réduit s’il ne peut pas afficher de publicité. Si d’autres éléments sont des enfants du même parent, ils peuvent être déplacés pour combler le vide du contrôle **AdControl** réduit, et développés à la prochaine demande.

<span id="xaml-adsnotrefreshing"/>

### <a name="ads-not-refreshing"></a>Non-actualisation des publicités

1.  Vérifiez la propriété [IsAutoRefreshEnabled](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol.isautorefreshenabled). Par défaut, cette propriété facultative est définie sur **True**. Si elle est définie sur **False**, la méthode [Refresh](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol.refresh) doit être utilisée pour récupérer une autre publicité.

    > [!div class="tabbedCodeSnippets"]
    ``` xml
    <UI:AdControl AdUnitId="{AdUnitID}"
                  ApplicationId="{ApplicationID}"
                  Width="728" Height="90"
                  IsAutoRefreshEnabled="True" />
    ```

2.  Vérifiez les appels à la méthode **Refresh**. Si vous utilisez l’actualisation automatique, la méthode **Refresh** ne permet pas de récupérer une autre publicité. Si vous utilisez l’actualisation manuelle, la méthode **Refresh** doit être appelée uniquement après un minimum de 30à 60secondes en fonction de la connexion de données actuelle de l’appareil.

    Les extraits de code suivants illustrent comment utiliser la méthode **Refresh**. Le premier extrait est le balisage d’interface utilisateur XAML.

    > [!div class="tabbedCodeSnippets"]
    ``` xml
    <UI:AdControl x:Name="adControl1"
                  AdUnitId="{AdUnit_ID}"
                  ApplicationId="{ApplicationID}"
                  Width="728" Height="90"
                  IsAutoRefreshEnabled="False" />
    ```

    Cet extrait de code présente un exemple de code C# derrière le balisage d’interface utilisateur.

    > [!div class="tabbedCodeSnippets"]
    ``` cs
    public RefreshAds()
    {
        var timer = new DispatcherTimer() { Interval = TimeSpan.FromSeconds(60) };
        timer.Tick += (s, e) => adControl1.Refresh();
        timer.Start();
    }
    ```

3.  Le contrôle **AdControl** se comporte normalement. Parfois, une même publicité s’affiche plusieurs fois dans une ligne, ce qui donne l’impression que les publicités ne sont pas actualisées.

<span id="csharp"/>

## <a name="c"></a>C\# #

<span id="csharp-adcontrolnotappearing"/>

### <a name="adcontrol-not-appearing"></a>AdControl invisible

1.  Assurez-vous que la fonctionnalité **Internet (client)** est sélectionnée dans le fichier Package.appxmanifest.

2.  Vérifiez que le contrôle **AdControl** est instancié. Si le contrôle **AdControl** n’est pas instancié, il ne sera pas disponible.

    > [!div class="tabbedCodeSnippets"]
    [!code-cs[AdControl](./code/AdvertisingSamples/AdControlSamples/cs/MiscellaneousSnippets.cs#Snippet1)]

3.  Vérifiez l’ID de l’application et l’ID d’unité publicitaire. Ces ID doivent correspondre à l’ID de l’application et à l’ID d’unité publicitaire que vous avez obtenus dans le Centre de développement Windows. Pour plus d’informations, voir [Configurer des unités publicitaires dans votre application](set-up-ad-units-in-your-app.md#live-ad-units).

    > [!div class="tabbedCodeSnippets"]
    ``` cs
    adControl = new AdControl();
    adControl.ApplicationId = "{ApplicationID}";adControl.AdUnitId = "{AdUnitID}";
    adControl.Height = 90;
    adControl.Width = 728;
    ```

4.  Vérifiez les paramètres **Height** et **Width**. Ils doivent être définis sur l’une des [tailles de bannières publicitaires prises en charge](supported-ad-sizes-for-banner-ads.md).

    > [!div class="tabbedCodeSnippets"]
    ``` cs
    adControl = new AdControl();
    adControl.ApplicationId = "{ApplicationID}";
    adControl.AdUnitId = "{AdUnitID}";
    adControl.Height = 90;adControl.Width = 728;
    ```

5.  Assurez-vous que le contrôle **AdControl** est ajouté à un élément parent. Pour qu’il s’affiche, le contrôle **AdControl** doit être ajouté en tant qu’enfant d’un contrôle parent (par exemple, **StackPanel** ou **Grid**).

    > [!div class="tabbedCodeSnippets"]
    ``` cs
    ContentPanel.Children.Add(adControl);
    ```

6.  Vérifiez le paramètre **Margin**. Le contrôle **AdControl** doit se situer à l’intérieur de la zone d’affichage.

7.  Vérifiez la propriété **Visibility**. La propriété facultative **Visibility** doit être définie sur **Visible**.

    > [!div class="tabbedCodeSnippets"]
    ``` cs
    adControl = new AdControl();
    adControl.ApplicationId = "{ApplicationID}";
    adControl.AdUnitId = "{AdUnitID}";
    adControl.Height = 90;
    adControl.Width = 728;
    adControl.Visibility = System.Windows.Visibility.Visible;
    ```

8.  Vérifiez le parent du **AdControl**. Le parent doit être actif et visible.

9. Les valeurs dynamiques des paramètres **ApplicationId** et **AdUnitId** ne doivent pas être testées dans l’émulateur. Pour vous assurer du bon fonctionnement du contrôle **AdControl**, utilisez les [valeurs de test](set-up-ad-units-in-your-app.md#test-ad-units) pour **ApplicationId** et **AdUnitId**.

<span id="csharp-blackboxblinksdisappears"/>

### <a name="black-box-blinks-and-disappears"></a>Une boîte noire clignote et disparaît

1.  Vérifiez toutes les étapes indiquées dans la section [AdControl invisible](#csharp-adcontrolnotappearing) ci-dessus.

2.  Gérez l’événement **ErrorOccurred**, puis utilisez le message transmis au gestionnaire d’événements pour déterminer si une erreur s’est produite et identifier le type d’erreur levée. Voir [Gestion des erreurs dans la procédure pas à pas pour XAML/C#](error-handling-in-xamlc-walkthrough.md) pour plus d’informations.

    Les exemples suivants présentent le code de base requis pour implémenter un appel d’erreur. Ce code XAML définit un **TextBlock** utilisé pour afficher le message d’erreur.

    > [!div class="tabbedCodeSnippets"]
    ``` xml
    <TextBlock x:Name="TextBlock1" TextWrapping="Wrap" Width="500" Height="250" />
    ```

    Ce code C# récupère le message d’erreur et l’affiche dans le **TextBlock**.

    > [!div class="tabbedCodeSnippets"]
    [!code-cs[AdControl](./code/AdvertisingSamples/AdControlSamples/cs/MiscellaneousSnippets.cs#Snippet2)]

    L’erreur la plus courante provoquant une boîte noire est la suivante: «Aucune publicité disponible». Cette erreur signifie qu’aucune publicité n’est disponible pour être retourné à partir de la demande.

3.  Le contrôle **AdControl** se comporte normalement. Parfois, une même publicité s’affiche plusieurs fois dans une ligne, ce qui donne l’impression que les publicités ne sont pas actualisées.

<span id="csharp-adsnotrefreshing"/>

### <a name="ads-not-refreshing"></a>Non-actualisation des publicités

1.  Vérifiez si la propriété [IsAutoRefreshEnabled](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol.isautorefreshenabled.aspx) de votre **AdControl** est définie sur false. Par défaut, cette propriété facultative est définie sur **true**. Si elle est définie sur **false**, la méthode **Refresh** doit être utilisée pour récupérer une autre publicité.

2.  Vérifiez les appels à la méthode [Refresh](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol.refresh.aspx). Si vous utilisez l’actualisation automatique (**IsAutoRefreshEnabled** est définie sur **true**), la méthode **Refresh** ne permet pas de récupérer une autre publicité. Si vous utilisez l’actualisation manuelle (**IsAutoRefreshEnabled** est définie sur **false**), la méthode **Refresh** doit être appelée uniquement après un minimum de 30à 60secondes en fonction de la connexion de données actuelle de l’appareil.

    L’exemple suivant montre comment appeler la méthode **Refresh**.

    > [!div class="tabbedCodeSnippets"]
    [!code-cs[AdControl](./code/AdvertisingSamples/AdControlSamples/cs/MiscellaneousSnippets.cs#Snippet3)]

3.  Le contrôle **AdControl** se comporte normalement. Parfois, une même publicité s’affiche plusieurs fois dans une ligne, ce qui donne l’impression que les publicités ne sont pas actualisées.

 

 
