---
author: mcleanbyron
ms.assetid: 141900dd-f1d3-4432-ac8b-b98eaa0b0da2
description: "Découvrez les solutions aux problèmes de développement courants liés aux bibliothèques de publicités Microsoft dans les applications XAML."
title: "Guide de résolution des problèmes pour XAML et C#"
translationtype: Human Translation
ms.sourcegitcommit: cf695b5c20378f7bbadafb5b98cdd3327bcb0be6
ms.openlocfilehash: ef9ad8f8056b17793d7ad8230e410e014edf2c95

---

# Guide de résolution des problèmes pour XAML et C#

\[ Mise à jour pour les applications UWP sur Windows10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

Cette rubrique contient les solutions aux problèmes de développement courants liés aux bibliothèques de publicités Microsoft dans les applications XAML.

-   [XAML](#xaml)

    -   [AdControl invisible](#xaml-notappearing)

    -   [Une boîte noire clignote et disparaît](#xaml-blackboxblinksdisappears)

    -   [Non-actualisation des publicités](#xaml-adsnotrefreshing)

-   [C#](#csharp)

    -   [AdControl invisible](#csharp-adcontrolnotappearing)

    -   [Une boîte noire clignote et disparaît](#csharp-blackboxblinksdisappears)

    -   [Non-actualisation des publicités](#csharp-adsnotrefreshing)

<span id="xaml"/>
## XAML

<span id="xaml-notappearing"/>
### AdControl invisible

1.  Assurez-vous que la fonctionnalité **Internet (client)** est sélectionnée dans le fichier Package.appxmanifest.

2.  Vérifiez l’ID de l’application et l’ID d’unité publicitaire. Ces ID doivent correspondre à l’ID de l’application et à l’ID d’unité publicitaire que vous avez obtenus dans le Centre de développement Windows. Pour plus d’informations, voir [Configurer des unités publicitaires dans votre application](set-up-ad-units-in-your-app.md).

    ``` syntax
    <UI:AdControl AdUnitId="{AdUnitID}" ApplicationId="{ApplicationID}"
                  Width="728" Height="90" />
    ```

3.  Vérifiez les propriétés **Height** et **Width**. Elles doivent être définies sur l’une des [tailles de bannières publicitaires prises en charge](supported-ad-sizes-for-banner-ads.md).

    ``` syntax
    <UI:AdControl AdUnitId="{AdUnitID}"
                  ApplicationId="{ApplicationID}"
                  Width="728" Height="90" />
    ```

4.  Vérifiez la position des éléments. Le contrôle [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) doit se situer à l’intérieur de la zone d’affichage.

5.  Vérifiez la propriété **Visibility**. La propriété facultative **Visibility** ne doit pas être définie sur collapsed ou hidden. Cette propriété peut être incluse (comme illustré ci-dessous) ou définie dans une feuille de style externe.

    ``` syntax
    <UI:AdControl AdUnitId="{AdUnitID}"
                  ApplicationId="{ApplicationID}"
                  Visibility="Visible"
                  Width="728" Height="90" />
    ```

6.  Vérifiez la propriété **IsEnabled**. La propriété facultative `IsEnabled` doit être définie sur `True`.

    ``` syntax
    <UI:AdControl AdUnitId="{AdUnitID}"
                  ApplicationId="{ApplicationID}"
                  IsEnabled="True"
                  Width="728" Height="90" />
    ```

7.  Vérifiez le parent du **AdControl**. Si l’élément **AdControl** réside dans un élément parent, ce dernier doit être actif et visible.

    ``` syntax
    <StackPanel>
        <UI:AdControl AdUnitId="{AdUnitID}"
                      ApplicationId="{ApplicationID}"
                      Width="728" Height="90" />
    </StackPanel>
    ```

8.  Vérifiez que le **AdControl** n’est pas masqué dans la fenêtre d’affichage. Le **AdControl** doit être visible afin que les publicités s’affichent correctement.

9.  Les valeurs dynamiques des paramètres **ApplicationId** et **AdUnitId** ne doivent pas être testées dans l’émulateur. Pour vous assurer du bon fonctionnement du contrôle **AdControl**, utilisez les ID de test pour les valeurs **ApplicationId** et **AdUnitId** contenues dans [Valeurs du mode test](test-mode-values.md).

<span id="xaml-blackboxblinksdisappears"/>
### Une boîte noire clignote et disparaît

1.  Vérifiez toutes les étapes indiquées dans la section précédente [AdControl invisible](#xaml-notappearing).

2.  Gérez l’événement **ErrorOccurred**, puis utilisez le message transmis au gestionnaire d’événements pour déterminer si une erreur s’est produite et identifier le type d’erreur levée. Voir [Gestion des erreurs dans la procédure pas à pas pour XAML/C#](error-handling-in-xamlc-walkthrough.md) pour plus d’informations.

    Cet exemple illustre un gestionnaire d’événements **ErrorOccurred**. Le premier extrait est le balisage d’interface utilisateur XAML.

    ``` syntax
    <UI:AdControl AdUnitId="{AdUnitID}"
                  ApplicationId="{ApplicationID}"
                  Width="728" Height="90"
                  ErrorOccurred="adControl_ErrorOccurred" />

    <TextBlock x:Name="TextBlock1" TextWrapping="Wrap" Width="500" Height="250" />
    ```

    Cet exemple présente le code correspondant.

    ``` syntax
    private void adControl_ErrorOccurred(object sender,               
        Microsoft.Advertising.WinRT.UI.AdErrorEventArgs e)
    {
        TextBlock1.Text = e.Error.Message;
    }
    ```

    L’erreur la plus courante provoquant une boîte noire est la suivante: «Aucune publicité disponible». Cette erreur signifie qu’aucune publicité n’est disponible pour être retourné à partir de la demande.

3.  Le contrôle [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) se comporte normalement.

    Par défaut, le **AdControl** est réduit s’il ne peut pas afficher de publicité. Si d’autres éléments sont des enfants du même parent, ils peuvent être déplacés pour combler le vide du contrôle **AdControl** réduit, et développés à la prochaine demande.

<span id="xaml-adsnotrefreshing"/>
### Non-actualisation des publicités

1.  Vérifiez la propriété [IsAutoRefreshEnabled](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.isautorefreshenabled.aspx). Par défaut, cette propriété facultative est définie sur **True**. Si elle est définie sur **False**, la méthode [Refresh](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.refresh.aspx) doit être utilisée pour récupérer une autre publicité.

    ``` syntax
    <UI:AdControl AdUnitId="{AdUnitID}"
                  ApplicationId="{ApplicationID}"
                  Width="728" Height="90"
                  IsAutoRefreshEnabled="True" />
    ```

2.  Vérifiez les appels à la méthode **Refresh**. Si vous utilisez l’actualisation automatique, la méthode **Refresh** ne permet pas de récupérer une autre publicité. Si vous utilisez l’actualisation manuelle, la méthode **Refresh** doit être appelée uniquement après un minimum de 30à 60secondes en fonction de la connexion de données actuelle de l’appareil.

    Les extraits de code suivants illustrent comment utiliser la méthode **Refresh**. Le premier extrait est le balisage d’interface utilisateur XAML.

    ``` syntax
    <UI:AdControl x:Name="adControl1"
                  AdUnitId="{AdUnit_ID}"
                  ApplicationId="{ApplicationID}"
                  Width="728" Height="90"
                  IsAutoRefreshEnabled="False" />
    ```

    Cet extrait de code présente un exemple de code C# derrière le balisage d’interface utilisateur.

    ``` syntax
    public Ads()
    {
        var timer = new DispatcherTimer() { Interval = TimeSpan.FromSeconds(60) };
        timer.Tick += (s, e) => adControl1.Refresh();
        timer.Start();
    }
    ```

3.  Le contrôle **AdControl** se comporte normalement. Parfois, une même publicité s’affiche plusieurs fois dans une ligne, ce qui donne l’impression que les publicités ne sont pas actualisées.

<span id="csharp"/>
## C\# #

<span id="csharp-adcontrolnotappearing"/>
### AdControl invisible

1.  Assurez-vous que la fonctionnalité **Internet (client)** est sélectionnée dans le fichier Package.appxmanifest.

2.  Vérifiez que le contrôle **AdControl** est instancié. Si le contrôle **AdControl** n’est pas instancié, il ne sera pas disponible.

    ``` syntax
    using Microsoft.Advertising.WinRT.UI;

    namespace App1
    {
        public sealed partial class MainPage : Page
        {
            AdControl adControl;

            public MainPage()
            {
                this.InitializeComponent();

                adControl = new AdControl()
                {
                    ApplicationId = "{ApplicationID}",
                    AdUnitId = "{AdUnitID}",
                    Height = 90,
                    Width = 728
                };
            }
        }
    }
    ```

3.  Vérifiez l’ID de l’application et l’ID d’unité publicitaire. Ces ID doivent correspondre à l’ID de l’application et à l’ID d’unité publicitaire que vous avez obtenus dans le Centre de développement Windows. Pour plus d’informations, voir [Configurer des unités publicitaires dans votre application](set-up-ad-units-in-your-app.md).

    ``` syntax
    adControl = new AdControl();
    adControl.ApplicationId = "{ApplicationID}";adControl.AdUnitId = "{AdUnitID}";
    adControl.Height = 90;
    adControl.Width = 728;
    ```

4.  Vérifiez les paramètres **Height** et **Width**. Ils doivent être définis sur l’une des [tailles de bannières publicitaires prises en charge](supported-ad-sizes-for-banner-ads.md).

    ``` syntax
    adControl = new AdControl();
    adControl.ApplicationId = "{ApplicationID}";
    adControl.AdUnitId = "{AdUnitID}";
    adControl.Height = 90;adControl.Width = 728;
    ```

5.  Assurez-vous que le contrôle **AdControl** est ajouté à un élément parent. Pour qu’il s’affiche, le contrôle **AdControl** doit être ajouté en tant qu’enfant d’un contrôle parent (par exemple, **StackPanel** ou **Grid**).

    ``` syntax
    ContentPanel.Children.Add(adControl);
    ```

6.  Vérifiez le paramètre **Margin**. Le contrôle **AdControl** doit se situer à l’intérieur de la zone d’affichage.

7.  Vérifiez la propriété **Visibility**. La propriété facultative **Visibility** doit être définie sur **Visible**.

    ``` syntax
    adControl = new AdControl();
    adControl.ApplicationId = "{ApplicationID}";
    adControl.AdUnitId = "{AdUnitID}";
    adControl.Height = 90;
    adControl.Width = 728;
    adControl.Visibility = System.Windows.Visibility.Visible;
    ```

8.  Vérifiez la propriété **IsEnabled**. La propriété facultative **IsEnabled** doit être définie sur **True**.

    ``` syntax
    adControl = new AdControl();
    adControl.ApplicationId = "{ApplicationID}";
    adControl.AdUnitId = "{AdUnitID}";
    adControl.Height = 90;
    adControl.Width = 728;
    adControl.IsEnabled = True;
    ```

9.  Vérifiez le parent du **AdControl**. Le parent doit être actif et visible.

10. Les valeurs dynamiques des paramètres **ApplicationId** et **AdUnitId** ne doivent pas être testées dans l’émulateur. Pour vous assurer du bon fonctionnement du contrôle **AdControl**, utilisez les ID de test pour les valeurs **ApplicationId** et **AdUnitId** contenues dans [Valeurs du mode test](test-mode-values.md).

<span id="csharp-blackboxblinksdisappears"/>
### Une boîte noire clignote et disparaît

1.  Vérifiez toutes les étapes indiquées dans la section [AdControl invisible](#csharp-adcontrolnotappearing) ci-dessus.

2.  Gérez l’événement **ErrorOccurred**, puis utilisez le message transmis au gestionnaire d’événements pour déterminer si une erreur s’est produite et identifier le type d’erreur levée. Voir [Gestion des erreurs dans la procédure pas à pas pour XAML/C#](error-handling-in-xamlc-walkthrough.md) pour plus d’informations.

    Les exemples suivants présentent le code de base requis pour implémenter un appel d’erreur. Ce code XAML définit un **TextBlock** utilisé pour afficher le message d’erreur.

    ``` syntax
    <TextBlock x:Name="TextBlock1" TextWrapping="Wrap" Width="500" Height="250" />
    ```

    Ce code C# récupère le message d’erreur et l’affiche dans le **TextBlock**.

    ``` syntax
    using Microsoft.Advertising.WinRT.UI;

    namespace App1
    {
        public partial class MainPage : Page
        {
            AdControl adControl;

            public MainPage()
            {
                this.InitializeComponent();

                adControl = new AdControl();
                adControl.ApplicationId = "{ApplicationID}";
                adControl.AdUnitId = "{AdUnitID}";
                adControl.Height = 90;
                adControl.Width = 728;
                adControl.ErrorOccurred += (s,e) =>
                {
                    TextBlock1.Text = e.Error.Message;
                };
            }
        }
    }
    ```

    L’erreur la plus courante provoquant une boîte noire est la suivante: «Aucune publicité disponible». Cette erreur signifie qu’aucune publicité n’est disponible pour être retourné à partir de la demande.

3.  
            Le contrôle **AdControl** se comporte normalement. Parfois, une même publicité s’affiche plusieurs fois dans une ligne, ce qui donne l’impression que les publicités ne sont pas actualisées.

<span id="csharp-adsnotrefreshing"/>
### Non-actualisation des publicités

1.  Vérifiez la propriété **IsAutoRefreshEnabled**. Par défaut, cette propriété facultative est définie sur **True**. Si elle est définie sur **False**, la méthode **Refresh** doit être utilisée pour récupérer une autre publicité.

    L’exemple suivant montre comment utiliser la propriété **isAutoRefreshEnabled**.

    ``` syntax
    adControl = new AdControl();
    adControl.ApplicationId = "{ApplicationID}";
    adControl.AdUnitId = "{AdUnitID}";
    adControl.Height = 90;
    adControl.Width = 728;
    adControl.IsAutoRefreshEnabled = true;
    ```

2.  Vérifiez les appels à la méthode **Refresh**. Si vous utilisez l’actualisation automatique, la méthode **Refresh** ne permet pas de récupérer une autre publicité. Si vous utilisez l’actualisation manuelle, la méthode **Refresh** doit être appelée uniquement après un minimum de 30à 60secondes en fonction de la connexion de données actuelle de l’appareil.

    L’exemple suivant montre comment appeler la méthode **Refresh**.

    ``` syntax
    public MainPage()
    {
        InitializeComponent();

        adControl = new AdControl();
        adControl.ApplicationId = "{ApplicationID}";
        adControl.AdUnitId = "{AdUnitID}";
        adControl.Height = 90;
        adControl.Width = 728;
        adControl.IsAutoRefreshEnabled = false;

        ContentPanel.Children.Add(adControl);

        var timer = new DispatcherTimer() { Interval = TimeSpan.FromSeconds(60) };
        timer.Tick += (s, e) => adControl.Refresh();
        timer.Start();
    }
    ```

3.  Le contrôle **AdControl** se comporte normalement. Parfois, une même publicité s’affiche plusieurs fois dans une ligne, ce qui donne l’impression que les publicités ne sont pas actualisées.

 

 



<!--HONumber=Jun16_HO4-->


