---
author: PatrickFarley
ms.assetid: 9A0F1852-A76B-4F43-ACFC-2CC56AAD1C03
title: Imprimer à partir de votre application
description: Découvrez comment imprimer des documents à partir d’une application Windows universelle. Cette rubrique montre également comment imprimer des pages spécifiques.
ms.author: pafarley
ms.date: 01/29/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, l’impression
ms.localizationpriority: medium
ms.openlocfilehash: cff96c0b8daf9f3ef32815437b510a5b94641527
ms.sourcegitcommit: d10fb9eb5f75f2d10e1c543a177402b50fe4019e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/12/2018
ms.locfileid: "4567739"
---
# <a name="print-from-your-app"></a>Imprimer à partir de votre application



**API importantes**

-   [**Windows.Graphics.Printing**](https://msdn.microsoft.com/library/windows/apps/BR226489)
-   [**Windows.UI.Xaml.Printing**](https://msdn.microsoft.com/library/windows/apps/BR243325)
-   [**PrintDocument**](https://msdn.microsoft.com/library/windows/apps/BR243314)

Découvrez comment imprimer des documents à partir d’une application Windows universelle. Cette rubrique montre également comment imprimer des pages spécifiques. Pour plus d’informations sur les modifications avancées de l’interface utilisateur d’aperçu avant impression, consultez [Personnaliser l’interface utilisateur d’aperçu avant impression](customize-the-print-preview-ui.md).

> [!TIP]
> La plupart des exemples de cette rubrique sont basés sur l’exemple d’impression. Pour voir le code complet, téléchargez l’[exemple d’impression de plateforme Windows universelle (UWP)](http://go.microsoft.com/fwlink/p/?LinkId=619984) à partir du [référentiel Windows-universal-samples](http://go.microsoft.com/fwlink/p/?LinkId=619979) sur GitHub.

## <a name="register-for-printing"></a>S’inscrire pour l’impression

Pour ajouter la fonctionnalité d’impression à votre application, vous devez commencer par vous inscrire au contrat d’impression. Vous devez répéter cette opération sur chaque écran à partir duquel vous souhaitez que votre utilisateur soit en mesure d’imprimer. Seul l’écran qui est affiché à l’utilisateur peut être inscrit à des fins d’impression. Si un écran de votre application est inscrit à des fins d’impression, son inscription doit être annulée au moment de sa fermeture. S’il est remplacé par un autre écran, l’écran suivant doit s’inscrire à un nouveau contrat d’impression lorsqu’il s’ouvre.

> [!TIP]
> Si vous souhaitez prendre en charge l’impression à partir de plusieurs pages dans votre application, vous pouvez inclure ce code d’impression dans une classe d’assistance commune afin que les pages de votre application le réutilisent. Pour obtenir un exemple de procédure à suivre, voir la classe `PrintHelper` dans l’[exemple d’impression UWP](http://go.microsoft.com/fwlink/p/?LinkId=619984).

Pour commencer, déclarez les classes [**PrintManager**](https://msdn.microsoft.com/library/windows/apps/BR226426) et [**PrintDocument**](https://msdn.microsoft.com/library/windows/apps/BR243314). Le type **PrintManager** se trouve dans l’espace de noms [**Windows.Graphics.Printing**](https://msdn.microsoft.com/library/windows/apps/BR226489) avec des types prenant en charge d’autres fonctionnalités d’impression Windows. Le type **PrintDocument** se trouve dans l’espace de noms [**Windows.UI.Xaml.Printing**](https://msdn.microsoft.com/library/windows/apps/BR243325) avec d’autres types autorisant la préparation du contenu XAML à des fins d’impression. Vous pouvez faciliter l’écriture de votre code d’impression en ajoutant les instructions **using** ou **Imports** suivantes dans votre page.

```csharp
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
```

La classe [**PrintDocument**](https://msdn.microsoft.com/library/windows/apps/BR243314) permet de gérer une grande partie de l’interaction entre l’application et la classe [**PrintManager**](https://msdn.microsoft.com/library/windows/apps/BR226426), mais elle expose quelques rappels qui lui sont propres. Lors de l’inscription, créez des instances des classes **PrintManager** et **PrintDocument**, et inscrivez des gestionnaires pour leurs événements d’impression.

Dans l’[exemple d’impression UWP](http://go.microsoft.com/fwlink/p/?LinkId=619984), l’inscription est effectuée à l’aide de la méthode `RegisterForPrinting`.

```csharp
public virtual void RegisterForPrinting()
{
   printDocument = new PrintDocument();
   printDocumentSource = printDocument.DocumentSource;
   printDocument.Paginate += CreatePrintPreviewPages;
   printDocument.GetPreviewPage += GetPrintPreviewPage;
   printDocument.AddPages += AddPrintPages;

   PrintManager printMan = PrintManager.GetForCurrentView();
   printMan.PrintTaskRequested += PrintTaskRequested;
}
```

Quand l’utilisateur accède à une page qui prend en charge l'impression, il initie l'inscription à l'intérieur de la méthode `OnNavigatedTo`.

```csharp
protected override void OnNavigatedTo(NavigationEventArgs e)
{
   // Initialize common helper class and register for printing
   printHelper = new PrintHelper(this);
   printHelper.RegisterForPrinting();

   // Initialize print content for this scenario
   printHelper.PreparePrintContent(new PageToPrint());

   // Tell the user how to print
   MainPage.Current.NotifyUser("Print contract registered with customization, use the Print button to print.", NotifyType.StatusMessage);
}
```

Quand l’utilisateur quitte la page, déconnectez les gestionnaires d’événements d’impression. Si vous disposez d’une application de plusieurs pages et ne déconnectez pas l’impression, une exception est levée quand l’utilisateur quitte la page, puis y revient.

```csharp
protected override void OnNavigatedFrom(NavigationEventArgs e)
{
   if (printHelper != null)
   {
         printHelper.UnregisterForPrinting();
   }
}
```
## <a name="create-a-print-button"></a>Créer un bouton d’impression

Ajoutez un bouton d’impression à l’emplacement de votre choix dans l’écran de votre application. Assurez-vous qu’il n’interfère pas avec le contenu que vous voulez imprimer.

```xml
<Button x:Name="InvokePrintingButton" Content="Print" Click="OnPrintButtonClick"/>
```

Ensuite, ajoutez un gestionnaire d’événements au code de votre application pour gérer l’événement Click. Utilisez la méthode [**ShowPrintUIAsync**](https://msdn.microsoft.com/library/windows/apps/windows.graphics.printing.printmanager.showprintuiasync) pour démarrer l’impression à partir de votre application. **ShowPrintUIAsync** est une méthode asynchrone qui affiche la fenêtre d’impression appropriée. Nous vous recommandons d’appeler la méthode [**IsSupported**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Printing.PrintManager.IsSupported) en premier pour vérifier que l’application est exécutée sur un appareil qui prend en charge l’impression (et pour gérer la situation dans le cas contraire). Si l’impression ne peut pas être effectuée à ce moment pour toute autre raison, **ShowPrintUIAsync** lève une exception. Nous vous recommandons d’intercepter ces exceptions et d’informer l’utilisateur quand l’impression ne peut pas être effectuée.

```csharp
async private void OnPrintButtonClick(object sender, RoutedEventArgs e)
{
    if (Windows.Graphics.Printing.PrintManager.IsSupported())
    {
        try
        {
            // Show print UI
            await Windows.Graphics.Printing.PrintManager.ShowPrintUIAsync();

        }
        catch
        {
            // Printing cannot proceed at this time
            ContentDialog noPrintingDialog = new ContentDialog()
            {
                Title = "Printing error",
                Content = "\nSorry, printing can' t proceed at this time.", PrimaryButtonText = "OK"
            };
            await noPrintingDialog.ShowAsync();
        }
    }
    else
    {
        // Printing is not supported on this device
        ContentDialog noPrintingDialog = new ContentDialog()
        {
            Title = "Printing not supported",
            Content = "\nSorry, printing is not supported on this device.",PrimaryButtonText = "OK"
        };
        await noPrintingDialog.ShowAsync();
    }
}
```

Dans cet exemple, une fenêtre d’impression s’affiche dans le gestionnaire d’événements pour un clic sur un bouton. Si la méthode lève une exception (parce que l’impression ne peut pas être effectuée à ce moment précis), un contrôle [**ContentDialog**](https://msdn.microsoft.com/library/windows/apps/Dn633972) informe l’utilisateur de la situation.

## <a name="format-your-apps-content"></a>Mettre en forme le contenu de votre application

Lors de l’appel de **ShowPrintUIAsync**, l’événement [**PrintTaskRequested**](https://msdn.microsoft.com/library/windows/apps/br206597) est déclenché. Le gestionnaire d’événements **PrintTaskRequested** présenté dans cette étape crée une classe [**PrintTask**](https://msdn.microsoft.com/library/windows/apps/BR226436) en appelant la méthode [**PrintTaskRequest.CreatePrintTask**](https://msdn.microsoft.com/library/windows/apps/windows.graphics.printing.printtaskrequest.createprinttask.aspx) et transmet le titre de la page d’impression et le nom d’un délégué [**PrintTaskSourceRequestedHandler**](https://msdn.microsoft.com/library/windows/apps/windows.graphics.printing.printtask.source). Notez que dans cet exemple, le délégué **PrintTaskSourceRequestedHandler** est défini inline. Le délégué **PrintTaskSourceRequestedHandler**, qui fournit le contenu mis en forme pour l’impression, est décrit plus tard.

Dans cet exemple, un gestionnaire d’achèvement est également défini pour déceler les erreurs. La gestion des événements d’achèvement est recommandée ; de cette façon, votre application peut informer l’utilisateur en cas d’erreur et fournir des solutions possibles. De même, votre application peut utiliser l’événement d’achèvement pour indiquer à l’utilisateur les prochaines étapes à suivre une fois le travail d’impression terminé.

```csharp
protected virtual void PrintTaskRequested(PrintManager sender, PrintTaskRequestedEventArgs e)
{
   PrintTask printTask = null;
   printTask = e.Request.CreatePrintTask("C# Printing SDK Sample", sourceRequested =>
   {
         // Print Task event handler is invoked when the print job is completed.
         printTask.Completed += async (s, args) =>
         {
            // Notify the user when the print operation fails.
            if (args.Completion == PrintTaskCompletion.Failed)
            {
               await scenarioPage.Dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal, () =>
               {
                     MainPage.Current.NotifyUser("Failed to print.", NotifyType.ErrorMessage);
               });
            }
         };

         sourceRequested.SetSource(printDocumentSource);
   });
}
```

Une fois la tâche d’impression créée, la classe [**PrintManager**](https://msdn.microsoft.com/library/windows/apps/BR226426) demande une collection de pages d’impression à afficher dans l’interface utilisateur d’aperçu avant impression en déclenchant l’événement [**Paginate**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.printing.printdocument.paginate). Cela correspond à la méthode **Paginate** de l’interface **IPrintPreviewPageCollection**. Le gestionnaire d’événements que vous avez créé lors de l’inscription est appelé à ce moment.

> [!IMPORTANT]
> Si l’utilisateur modifie les paramètres d’impression, le gestionnaire d’événements de pagination est appelé à nouveau pour vous permettre de redisposer le contenu. Pour une expérience utilisateur optimale, nous vous recommandons de vérifier les paramètres avant de redisposer le contenu et d’éviter de réinitialiser le contenu paginé si ce n’est pas nécessaire.

Dans le gestionnaire d’événements [**Paginate**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.printing.printdocument.paginate) (la méthode `CreatePrintPreviewPages` dans l’[exemple d’impression UWP](http://go.microsoft.com/fwlink/p/?LinkId=619984)), créez les pages à afficher dans l’interface utilisateur d’aperçu avant impression et à envoyer à l’imprimante. Le code dont vous vous servez pour préparer le contenu de votre application à imprimer est propre à votre application et au contenu que vous imprimez. Reportez-vous au code source de l’[exemple d’impression UWP](http://go.microsoft.com/fwlink/p/?LinkId=619984) pour voir comment il met en forme le contenu à des fins d’impression.

```csharp
protected virtual void CreatePrintPreviewPages(object sender, PaginateEventArgs e)
{
   // Clear the cache of preview pages
   printPreviewPages.Clear();

   // Clear the print canvas of preview pages
   PrintCanvas.Children.Clear();

   // This variable keeps track of the last RichTextBlockOverflow element that was added to a page which will be printed
   RichTextBlockOverflow lastRTBOOnPage;

   // Get the PrintTaskOptions
   PrintTaskOptions printingOptions = ((PrintTaskOptions)e.PrintTaskOptions);

   // Get the page description to deterimine how big the page is
   PrintPageDescription pageDescription = printingOptions.GetPageDescription(0);

   // We know there is at least one page to be printed. passing null as the first parameter to
   // AddOnePrintPreviewPage tells the function to add the first page.
   lastRTBOOnPage = AddOnePrintPreviewPage(null, pageDescription);

   // We know there are more pages to be added as long as the last RichTextBoxOverflow added to a print preview
   // page has extra content
   while (lastRTBOOnPage.HasOverflowContent && lastRTBOOnPage.Visibility == Windows.UI.Xaml.Visibility.Visible)
   {
         lastRTBOOnPage = AddOnePrintPreviewPage(lastRTBOOnPage, pageDescription);
   }

   if (PreviewPagesCreated != null)
   {
         PreviewPagesCreated.Invoke(printPreviewPages, null);
   }

   PrintDocument printDoc = (PrintDocument)sender;

   // Report the number of preview pages created
   printDoc.SetPreviewPageCount(printPreviewPages.Count, PreviewPageCountType.Intermediate);
}
```

Quand une page particulière doit être affichée dans la fenêtre d’aperçu avant impression, la classe [**PrintManager**](https://msdn.microsoft.com/library/windows/apps/BR226426) déclenche l’événement [**GetPreviewPage**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.printing.printdocument.getpreviewpage). Cela correspond à la méthode **MakePage** de l’interface **IPrintPreviewPageCollection**. Le gestionnaire d’événements que vous avez créé lors de l’inscription est appelé à ce moment.

Dans le gestionnaire d’événements [**GetPreviewPage**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.printing.printdocument.getpreviewpage) (la méthode `GetPrintPreviewPage` dans l’[exemple d’impression UWP](http://go.microsoft.com/fwlink/p/?LinkId=619984)), définissez la page appropriée sur le document à imprimer.

```csharp
protected virtual void GetPrintPreviewPage(object sender, GetPreviewPageEventArgs e)
{
   PrintDocument printDoc = (PrintDocument)sender;
   printDoc.SetPreviewPage(e.PageNumber, printPreviewPages[e.PageNumber - 1]);
}
```

Enfin, quand l’utilisateur clique sur le bouton d’impression, la classe [**PrintManager**](https://msdn.microsoft.com/library/windows/apps/BR226426) demande la collection finale de pages à envoyer à l’imprimante en appelant la méthode **MakeDocument** de l’interface **IDocumentPageSource**. En XAML, cela déclenche l’événement [**AddPages**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.printing.printdocument.addpages). Le gestionnaire d’événements que vous avez créé lors de l’inscription est appelé à ce moment.

Dans le gestionnaire d’événements [**AddPages**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.printing.printdocument.addpages) (la méthode `AddPrintPages` dans l’[exemple d’impression UWP](http://go.microsoft.com/fwlink/p/?LinkId=619984)), ajoutez des pages de la collection de pages à l’objet [**PrintDocument**](https://msdn.microsoft.com/library/windows/apps/BR243314) à envoyer à l’imprimante. Si un utilisateur spécifie des pages ou une plage de pages en particulier à imprimer, vous utilisez ces informations à cet endroit pour ajouter uniquement les pages qui seront effectivement envoyées à l’imprimante.

```csharp
protected virtual void AddPrintPages(object sender, AddPagesEventArgs e)
{
   // Loop over all of the preview pages and add each one to  add each page to be printied
   for (int i = 0; i < printPreviewPages.Count; i++)
   {
         // We should have all pages ready at this point...
         printDocument.AddPage(printPreviewPages[i]);
   }

   PrintDocument printDoc = (PrintDocument)sender;

   // Indicate that all of the print pages have been provided
   printDoc.AddPagesComplete();
}
```

## <a name="prepare-print-options"></a>Préparer les options d’impression

Préparez ensuite les options d’impression. À titre d’exemple, cette section décrit comment définir l’option d’étendue de pages pour permettre l’impression de pages spécifiques. Pour d’autres d’options avancées, consultez[Personnaliser l’interface utilisateur d’aperçu avant impression](customize-the-print-preview-ui.md).

Cette étape crée une nouvelle option d’impression, définit une liste de valeurs prises en charge par l’option, puis ajoute l’option à l’interface utilisateur d’aperçu avant impression. L’option d’étendue de pages comprend les paramètres suivants :

| Nom de l’option          | Action |
|----------------------|--------|
| **Print all (Imprimer tout)**        | Imprime toutes les pages du document.|
| **Print Selection (Imprimer la sélection)**  | Imprime uniquement le contenu sélectionné par l’utilisateur.|
| **Print Range (Étendue)**      | Affiche un contrôle d’édition dans lequel l’utilisateur peut entrer les pages à imprimer.|

Premièrement, modifiez le gestionnaire d’événements [**PrintTaskRequested**](https://msdn.microsoft.com/library/windows/apps/br206597) pour ajouter le code afin d’obtenir un objet [**PrintTaskOptionDetails**](https://msdn.microsoft.com/library/windows/apps/Hh701256).

```csharp
PrintTaskOptionDetails printDetailedOptions = PrintTaskOptionDetails.GetFromPrintTaskOptions(printTask.Options);
```

Effacez la liste des options affichées dans l’interface utilisateur d’aperçu avant impression, puis ajoutez les options que vous voulez afficher quand l’utilisateur souhaite imprimer à partir de l’application.

> [!NOTE]
> Les options apparaissent dans l’interface utilisateur d’aperçu avant impression dans l’ordre dans lequel elles sont ajoutées, la première option figurant en haut de la fenêtre.

```csharp
IList<string> displayedOptions = printDetailedOptions.DisplayedOptions;

displayedOptions.Clear();
displayedOptions.Add(Windows.Graphics.Printing.StandardPrintTaskOptions.Copies);
displayedOptions.Add(Windows.Graphics.Printing.StandardPrintTaskOptions.Orientation);
displayedOptions.Add(Windows.Graphics.Printing.StandardPrintTaskOptions.ColorMode);
```

Créez l’option d’impression et initialisez la liste de valeurs de l’option.

```csharp
// Create a new list option
PrintCustomItemListOptionDetails pageFormat = printDetailedOptions.CreateItemListOption("PageRange", "Page Range");
pageFormat.AddItem("PrintAll", "Print all");
pageFormat.AddItem("PrintSelection", "Print Selection");
pageFormat.AddItem("PrintRange", "Print Range");
```

Ajoutez votre option d’impression personnalisée, puis attribuez le gestionnaire d’événements. L’option personnalisée est ajoutée en dernière position, en bas de la liste des options. Vous pouvez cependant la placer n’importe où dans la liste, de sorte que vous n’avez pas à vous soucier d’ajouter les options d’impression personnalisées en dernier.

```csharp
// Add the custom option to the option list
displayedOptions.Add("PageRange");

// Create new edit option
PrintCustomTextOptionDetails pageRangeEdit = printDetailedOptions.CreateTextOption("PageRangeEdit", "Range");

// Register the handler for the option change event
printDetailedOptions.OptionChanged += printDetailedOptions_OptionChanged;
```

La méthode [**CreateTextOption**](https://msdn.microsoft.com/library/windows/apps/windows.graphics.printing.optiondetails.printtaskoptiondetails.createtextoption) crée la zone de texte **Range** (Étendue). Il s’agit de l’emplacement où l’utilisateur spécifie les pages qu’il souhaite imprimer quand il sélectionne l’option **Print Range** (Étendue).

## <a name="handle-print-option-changes"></a>Gérer les modifications apportées aux options d’impression

Le gestionnaire d’événements **OptionChanged** a un rôle double. Premièrement, il affiche et masque le champ d’édition de texte pour l’étendue de pages en fonction de l’option d’étendue de pages sélectionnée par l’utilisateur. Deuxièmement, il teste le texte entré dans la zone de texte d’étendue de pages pour vérifier qu’il représente une étendue de pages valide pour le document.

Cet exemple montre comment l’[exemple d’impression UWP](http://go.microsoft.com/fwlink/p/?LinkId=619984) gère les événements de modification.

```csharp
async void printDetailedOptions_OptionChanged(PrintTaskOptionDetails sender, PrintTaskOptionChangedEventArgs args)
{
   if (args.OptionId == null)
   {
         return;
   }

   string optionId = args.OptionId.ToString();

   // Handle change in Page Range Option
   if (optionId == "PageRange")
   {
         IPrintOptionDetails pageRange = sender.Options[optionId];
         string pageRangeValue = pageRange.Value.ToString();

         selectionMode = false;

         switch (pageRangeValue)
         {
            case "PrintRange":
               // Add PageRangeEdit custom option to the option list
               sender.DisplayedOptions.Add("PageRangeEdit");
               pageRangeEditVisible = true;
               await scenarioPage.Dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal, () =>
               {
                     ShowContent(null);
               });
               break;
            case "PrintSelection":
               await scenarioPage.Dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal, () =>
               {
                     Scenario4PageRange page = (Scenario4PageRange)scenarioPage;
                     PageToPrint pageContent = (PageToPrint)page.PrintFrame.Content;
                     ShowContent(pageContent.TextContentBlock.SelectedText);
               });
               RemovePageRangeEdit(sender);
               break;
            default:
               await scenarioPage.Dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal, () =>
               {
                     ShowContent(null);
               });
               RemovePageRangeEdit(sender);
               break;
         }

         Refresh();
   }
   else if (optionId == "PageRangeEdit")
   {
         IPrintOptionDetails pageRange = sender.Options[optionId];
         // Expected range format (p1,p2...)*, (p3-p9)* ...
         if (!Regex.IsMatch(pageRange.Value.ToString(), @"^\s*\d+\s*(\-\s*\d+\s*)?(\,\s*\d+\s*(\-\s*\d+\s*)?)*$"))
         {
            pageRange.ErrorText = "Invalid Page Range (eg: 1-3, 5)";
         }
         else
         {
            pageRange.ErrorText = string.Empty;
            try
            {
               GetPagesInRange(pageRange.Value.ToString());
               Refresh();
            }
            catch (InvalidPageException ipex)
            {
               pageRange.ErrorText = ipex.Message;
            }
         }
   }
}
```

> [!TIP]
> Consultez la méthode `GetPagesInRange` dans l'[exemple d’impression UWP](http://go.microsoft.com/fwlink/p/?LinkId=619984) pour obtenir des détails sur la façon d’analyser l’étendue de pages que l’utilisateur entre dans la zone de texte Range (Étendue).

## <a name="preview-selected-pages"></a>Afficher un aperçu des pages sélectionnées

La façon dont vous mettez en forme le contenu de votre application à des fins d’impression dépend de la nature de votre application et de son contenu. L’[exemple d’impression UWP](http://go.microsoft.com/fwlink/p/?LinkId=619984) utilise la classe d’assistance d’impression pour mettre en forme son contenu à des fins d’impression.

Si vous imprimez un sous-ensemble de pages, plusieurs options s’offrent à vous pour afficher le contenu dans l’aperçu avant impression. Quelle que soit la méthode que vous choisissez pour afficher l’étendue de pages dans l’aperçu avant impression, la sortie imprimée doit contenir uniquement les pages sélectionnées.

-   Affichez toutes les pages dans l’aperçu avant impression, qu’une étendue de pages soit spécifiée ou non, sans informer l’utilisateur des pages qui seront réellement imprimées.
-   Affichez uniquement les pages sélectionnées par l’utilisateur par le biais de l’étendue de pages dans l’aperçu avant impression en mettant à jour l’écran chaque fois que l’utilisateur modifie l’étendue de pages.
-   Affichez toutes les pages dans l’aperçu avant impression, mais en grisant les pages qui ne figurent pas dans l’étendue sélectionnée par l’utilisateur.

## <a name="related-topics"></a>Rubriques connexes

* [Instructions de conception concernant l’impression](https://msdn.microsoft.com/library/windows/apps/Hh868178)
* [//Vidéo de la build 2015 : développement d’applications qui impriment dans Windows 10](https://channel9.msdn.com/Events/Build/2015/2-94)
* [Exemple d’impression UWP](http://go.microsoft.com/fwlink/p/?LinkId=619984)
