---
ms.assetid: 88132B6F-FB50-4B03-BC21-233988746230
title: Personnaliser l’interface utilisateur d’aperçu avant impression
description: Cette section décrit comment personnaliser les options d’impression et les paramètres dans l’interface utilisateur d’aperçu avant impression.
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp, impression
ms.localizationpriority: medium
ms.openlocfilehash: 68f8f990209a66a8677afbd1913c95bfd2fce187
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66370290"
---
# <a name="customize-the-print-preview-ui"></a>Personnaliser l’interface utilisateur d’aperçu avant impression



**API importantes**

-   [**Windows.Graphics.Printing**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Printing)
-   [**Windows.UI.Xaml.Printing**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Printing)
-   [**PrintManager**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Printing.PrintManager)

Cette section décrit comment personnaliser les options d’impression et les paramètres dans l’interface utilisateur d’aperçu avant impression. Pour plus d’informations sur l’impression, consultez [Imprimer à partir de votre application](print-from-your-app.md).

**Conseil**  la plupart des exemples de cette rubrique sont basées sur l’exemple d’impression. Pour voir le code complet, téléchargez l’[exemple d’impression de plateforme Windows universelle (UWP)](https://go.microsoft.com/fwlink/p/?LinkId=619984) à partir du [référentiel Windows-universal-samples](https://go.microsoft.com/fwlink/p/?LinkId=619979) sur GitHub.

 

## <a name="customize-print-options"></a>Personnaliser les options d’impression

Par défaut, l’interface utilisateur d’aperçu avant impression affiche les options d’impression [**ColorMode**](https://docs.microsoft.com/uwp/api/windows.graphics.printing.standardprinttaskoptions.colormode), [**Copies**](https://docs.microsoft.com/uwp/api/windows.graphics.printing.standardprinttaskoptions.copies) et [**Orientation**](https://docs.microsoft.com/uwp/api/windows.graphics.printing.standardprinttaskoptions.orientation). Outre ces options, Windows fournit plusieurs autres options d’imprimante que vous pouvez ajouter à l’interface utilisateur d’aperçu avant impression.

-   [**Liaison**](https://docs.microsoft.com/uwp/api/windows.graphics.printing.standardprinttaskoptions.binding)
-   [**Collation**](https://docs.microsoft.com/uwp/api/windows.graphics.printing.standardprinttaskoptions.collation)
-   [**Duplex**](https://docs.microsoft.com/uwp/api/windows.graphics.printing.standardprinttaskoptions.duplex)
-   [**HolePunch**](https://docs.microsoft.com/uwp/api/windows.graphics.printing.standardprinttaskoptions.holepunch)
-   [**InputBin**](https://docs.microsoft.com/uwp/api/windows.graphics.printing.standardprinttaskoptions.inputbin)
-   [**MediaSize**](https://docs.microsoft.com/uwp/api/windows.graphics.printing.standardprinttaskoptions.mediasize)
-   [**MediaType**](https://docs.microsoft.com/uwp/api/windows.graphics.printing.standardprinttaskoptions.mediatype)
-   [**NUp**](https://docs.microsoft.com/uwp/api/windows.graphics.printing.standardprinttaskoptions.nup)
-   [**PrintQuality**](https://docs.microsoft.com/uwp/api/windows.graphics.printing.standardprinttaskoptions.printquality)
-   [**Élément de base**](https://docs.microsoft.com/uwp/api/windows.graphics.printing.standardprinttaskoptions.staple)

Ces options sont définies dans la classe [**StandardPrintTaskOptions**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Printing.StandardPrintTaskOptions). Vous pouvez ajouter ou supprimer des options dans la liste d’options affichées dans l’interface utilisateur d’aperçu avant impression. Vous pouvez également modifier l’ordre dans lequel elles apparaissent et définir les paramètres par défaut qui sont proposés à l’utilisateur.

Toutefois, les modifications que vous apportez par le biais de cette méthode affectent uniquement l’interface utilisateur d’aperçu avant impression. L’utilisateur peut toujours accéder à l’ensemble des options prises en charge par l’imprimante en appuyant sur **Autres paramètres** dans l’interface utilisateur d’aperçu avant impression.

**Remarque**  bien que votre application peut spécifier les options d’impression à afficher, uniquement ceux qui sont pris en charge par l’imprimante sélectionnée sont affichées dans l’interface utilisateur d’aperçu avant impression. L’interface utilisateur d’impression n’affiche pas les options que l’imprimante sélectionnée ne prend pas en charge.

 

### <a name="define-the-options-to-display"></a>Définir les options à afficher

Une fois l’écran de l’application chargé, il s’inscrit au contrat d’impression. La définition du gestionnaire d’événements [**PrintTaskRequested**](https://msdn.microsoft.com/library/windows/apps/br206597) fait partie de ce processus d’inscription. Le code nécessaire pour personnaliser les options affichées dans l’interface utilisateur d’aperçu avant impression est ajouté au gestionnaire d’événements **PrintTaskRequested**.

Modifiez le gestionnaire d’événements [**PrintTaskRequested**](https://msdn.microsoft.com/library/windows/apps/br206597) pour inclure les instructions [**printTask.options**](https://docs.microsoft.com/uwp/api/windows.graphics.printing.printtask.options) qui configurent les paramètres d’impression que vous voulez afficher dans l’interface utilisateur d’aperçu avant impression. Pour l’écran de votre application pour lequel vous voulez afficher une liste personnalisée des options d’impression, remplacez le gestionnaire d’événements **PrintTaskRequested** dans la classe d’assistance pour inclure du code qui spécifie les options à afficher lors de l’impression de l’écran.

``` csharp
protected override void PrintTaskRequested(PrintManager sender, PrintTaskRequestedEventArgs e)
{
   PrintTask printTask = null;
   printTask = e.Request.CreatePrintTask("C# Printing SDK Sample", sourceRequestedArgs =>
   {
         IList<string> displayedOptions = printTask.Options.DisplayedOptions;

         // Choose the printer options to be shown.
         // The order in which the options are appended determines the order in which they appear in the UI
         displayedOptions.Clear();
         displayedOptions.Add(Windows.Graphics.Printing.StandardPrintTaskOptions.Copies);
         displayedOptions.Add(Windows.Graphics.Printing.StandardPrintTaskOptions.Orientation);
         displayedOptions.Add(Windows.Graphics.Printing.StandardPrintTaskOptions.MediaSize);
         displayedOptions.Add(Windows.Graphics.Printing.StandardPrintTaskOptions.Collation);
         displayedOptions.Add(Windows.Graphics.Printing.StandardPrintTaskOptions.Duplex);

         // Preset the default value of the printer option
         printTask.Options.MediaSize = PrintMediaSize.NorthAmericaLegal;

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

         sourceRequestedArgs.SetSource(printDocumentSource);
   });
}
```

**Important**  appelant [ **displayedOptions.clear**](https://docs.microsoft.com/uwp/api/windows.graphics.printing.printtaskoptions.displayedoptions)() supprime toutes les options d’impression à partir de l’aperçu avant impression de l’interface utilisateur, y compris le **davantage de paramètres** lien. Veillez à ajouter les options que vous voulez afficher dans l’interface utilisateur d’aperçu avant impression.

### <a name="specify-default-options"></a>Spécifier les options par défaut

Vous pouvez également définir les valeurs par défaut des options dans l’interface utilisateur d’aperçu avant impression. La ligne de code suivante, issue de l’exemple précédent, définit la valeur par défaut de l’option [**MediaSize**](https://docs.microsoft.com/uwp/api/windows.graphics.printing.standardprinttaskoptions.mediasize).

``` csharp
         // Preset the default value of the printer option
         printTask.Options.MediaSize = PrintMediaSize.NorthAmericaLegal;
```         

## <a name="add-new-print-options"></a>Ajouter des options d’impression

Cette section montre comment créer une option d’impression, définir une liste de valeurs que l’option prend en charge, puis ajouter l’option à l’aperçu avant impression. Comme dans la section précédente, ajoutez la nouvelle option d’impression au gestionnaire d’événements [**PrintTaskRequested**](https://msdn.microsoft.com/library/windows/apps/br206597).

Premièrement, obtenez un objet [**PrintTaskOptionDetails**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Printing.OptionDetails.PrintTaskOptionDetails). Celui-ci est utilisé pour ajouter la nouvelle option d’impression à l’interface utilisateur d’aperçu avant impression. Effacez ensuite la liste d’options qui sont affichées dans l’interface utilisateur d’aperçu avant impression et ajoutez les options que vous voulez afficher quand l’utilisateur souhaite imprimer à partir de l’application. Après cela, créez l’option d’impression et initialisez la liste de valeurs de l’option. Enfin, ajoutez la nouvelle option et affectez un gestionnaire pour l’événement **OptionChanged**.

``` csharp
protected override void PrintTaskRequested(PrintManager sender, PrintTaskRequestedEventArgs e)
{
   PrintTask printTask = null;
   printTask = e.Request.CreatePrintTask("C# Printing SDK Sample", sourceRequestedArgs =>
   {
         PrintTaskOptionDetails printDetailedOptions = PrintTaskOptionDetails.GetFromPrintTaskOptions(printTask.Options);
         IList<string> displayedOptions = printDetailedOptions.DisplayedOptions;

         // Choose the printer options to be shown.
         // The order in which the options are appended determines the order in which they appear in the UI
         displayedOptions.Clear();

         displayedOptions.Add(Windows.Graphics.Printing.StandardPrintTaskOptions.Copies);
         displayedOptions.Add(Windows.Graphics.Printing.StandardPrintTaskOptions.Orientation);
         displayedOptions.Add(Windows.Graphics.Printing.StandardPrintTaskOptions.ColorMode);

         // Create a new list option
         PrintCustomItemListOptionDetails pageFormat = printDetailedOptions.CreateItemListOption("PageContent", "Pictures");
         pageFormat.AddItem("PicturesText", "Pictures and text");
         pageFormat.AddItem("PicturesOnly", "Pictures only");
         pageFormat.AddItem("TextOnly", "Text only");

         // Add the custom option to the option list
         displayedOptions.Add("PageContent");

         printDetailedOptions.OptionChanged += printDetailedOptions_OptionChanged;

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

         sourceRequestedArgs.SetSource(printDocumentSource);
   });
}
```

Les options apparaissent dans l’interface utilisateur d’aperçu avant impression dans l’ordre dans lequel elles sont ajoutées, la première option figurant en haut de la fenêtre. Dans cet exemple, l’option personnalisée ajoutée en dernier apparaît en bas de la liste des options. Toutefois, vous pouvez la placer n’importe où dans la liste, car vous n’êtes pas obligé d’ajouter les options d’impression personnalisées en dernier.

Lorsque l’utilisateur modifie l’option sélectionnée dans votre option personnalisée, mettez à jour l’image d’aperçu avant impression. Appelez la méthode [**InvalidatePreview**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.printing.printdocument.invalidatepreview) pour redessiner l’image dans l’interface utilisateur d’aperçu avant impression, comme illustré ci-dessous.

``` csharp
async void printDetailedOptions_OptionChanged(PrintTaskOptionDetails sender, PrintTaskOptionChangedEventArgs args)
{
   // Listen for PageContent changes
   string optionId = args.OptionId as string;
   if (string.IsNullOrEmpty(optionId))
   {
         return;
   }

   if (optionId == "PageContent")
   {
         await scenarioPage.Dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal, () =>
         {
            printDocument.InvalidatePreview();
         });
   }
}
```

## <a name="related-topics"></a>Rubriques connexes

* [Instructions de conception pour l’impression](https://docs.microsoft.com/windows/uwp/devices-sensors/printing-and-scanning)
* [Build 2015 vidéo : Développement d’applications qui s’impriment dans Windows 10](https://channel9.msdn.com/Events/Build/2015/2-94)
* [Exemple d’impression UWP](https://go.microsoft.com/fwlink/p/?LinkId=619984)
