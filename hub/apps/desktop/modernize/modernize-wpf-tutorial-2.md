---
description: Ce didacticiel montre comment ajouter des interfaces utilisateur en XAML UWP, créer des packages MSIX et intégrer d’autres composants modernes à votre application WPF.
title: Ajouter un contrôle InkCanvas UWP à l’aide d'îles XAML
ms.topic: article
ms.date: 08/15/2019
ms.author: mcleans
author: mcleanbyron
keywords: Windows 10, UWP, Windows Forms, WPF, îlots XAML
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: fb7bb6d4e5af8992571f9740c1321e271b2e1672
ms.sourcegitcommit: 6bb794c6e309ba543de6583d96627fbf1c177bef
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/20/2019
ms.locfileid: "69643423"
---
# <a name="part-2-add-a-uwp-inkcanvas-control-using-xaml-islands"></a>Partie 2 : Ajouter un contrôle InkCanvas UWP à l’aide d'îles XAML

Il s’agit de la deuxième partie d’un didacticiel qui montre comment moderniser un exemple d’application de bureau WPF nommée Contoso depenses. Pour obtenir une vue d’ensemble du didacticiel, des conditions préalables et des instructions pour télécharger l’exemple [d’application, consultez le didacticiel : Moderniser une application](modernize-wpf-tutorial.md)WPF. Cet article suppose que vous avez déjà effectué la [partie 1](modernize-wpf-tutorial-1.md).

Dans le scénario fictif de ce didacticiel, l’équipe de développement de contoso souhaite ajouter la prise en charge des signatures numériques à l’application Contoso depenses. Le contrôle de l' **INKCANVAS** UWP est une option intéressante pour ce scénario, car il prend en charge les fonctionnalités de l’encre numérique et de l’intelligence artificielle, telles que la possibilité de reconnaître du texte et des formes. Pour ce faire, vous allez utiliser le contrôle [UWP encapsulé qui](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) est disponible dans le kit de tâches de la communauté Windows. Ce contrôle encapsule l’interface et les fonctionnalités du contrôle de l' **INKCANVAS** UWP pour une utilisation dans une application WPF. Pour plus d’informations sur les contrôles UWP encapsulés, consultez [héberger des contrôles XAML UWP dans les applications de bureau (îlots XAML)](xaml-islands.md).

## <a name="configure-the-project-to-use-xaml-islands"></a>Configurer le projet pour utiliser des îlots XAML

Avant de pouvoir ajouter un contrôle **InkCanvas** à l’application Contoso depenses, vous devez d’abord configurer le projet pour qu’il prenne en charge les îles de la série UWP.

1. Dans Visual Studio 2019, cliquez avec le bouton droit sur le projet **ContosoExpenses. Core** dans **Explorateur de solutions** , puis choisissez **gérer les packages NuGet**.

    ![Menu gérer les packages NuGet dans Visual Studio](images/wpf-modernize-tutorial//ManageNuGetPackages.png)

2. Dans la fenêtre **Gestionnaire de package NuGet** , cliquez sur **Parcourir**. Sélectionnez l’option **inclure la version préliminaire** , recherchez le `Microsoft.Toolkit.Wpf.UI.Controls` package et installez la dernière version préliminaire du package affichée dans les résultats. Veillez à installer la version 6.0.0-preview7 ou une version ultérieure.

    > [!NOTE]
    > Ce package contient toute l’infrastructure nécessaire pour l’hébergement des îlots XAML UWP dans une application WPF, y compris le contrôle UWP encapsulé dans **InkCanvas** . Un package similaire nommé `Microsoft.Toolkit.Forms.UI.Controls` est disponible pour les applications Windows Forms.

3. Cliquez avec le bouton droit sur le projet **ContosoExpenses. Core** dans **Explorateur de solutions** et choisissez **Ajouter-> nouvel élément**.

4. Sélectionnez **fichier manifeste**de l’application, nommez-le **app. manifest**, puis cliquez sur **Ajouter**. Pour plus d’informations sur les manifestes d’application, consultez [cet article](https://docs.microsoft.com/windows/desktop/SbsCs/application-manifests).

5. Dans le fichier manifeste, supprimez les marques `<supportedOS>` de commentaire de l’élément suivant pour Windows 10.

    ```xml
    <!-- Windows 10 -->
    <supportedOS Id="{8e0f7a12-bfb3-4fe8-b9a5-48fd50a15a9a}" />
    ```

6. Dans le fichier manifeste, localisez l’élément `<application>` commenté suivant.

    ```xml
    <!--
    <application xmlns="urn:schemas-microsoft-com:asm.v3">
      <windowsSettings>
        <dpiAware xmlns="http://schemas.microsoft.com/SMI/2005/WindowsSettings">true</dpiAware>
      </windowsSettings>
    </application>
    -->
    ```

7. Supprimez cette section et remplacez-la par le code XML suivant. Cela permet de configurer l’application pour qu’elle prenne en charge DPI et de mieux gérer les différents facteurs de mise à l’échelle pris en charge par Windows 10.

    ```xml
    <application xmlns="urn:schemas-microsoft-com:asm.v3">
      <windowsSettings>
          <dpiAware xmlns="http://schemas.microsoft.com/SMI/2005/WindowsSettings">true/PM</dpiAware>
          <dpiAwareness xmlns="http://schemas.microsoft.com/SMI/2016/WindowsSettings">PerMonitorV2, PerMonitor</dpiAwareness>
      </windowsSettings>
    </application>
    ```

8. Enregistrez et fermez le `app.manifest` fichier.

9. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le projet **ContosoExpenses. Core** , puis choisissez **Propriétés**.

10. Dans la section **ressources** de l’onglet **application** , assurez-vous que la liste déroulante **manifeste** est définie sur **app. manifest**.

    ![Manifeste de l’application .NET Core](images/wpf-modernize-tutorial/NetCoreAppManifest.png)

11. Enregistrez les modifications apportées aux propriétés du projet.

## <a name="add-an-inkcanvas-control-to-the-app"></a>Ajouter un contrôle InkCanvas à l’application

Maintenant que vous avez configuré votre projet pour qu’il utilise des îlots XAML UWP, vous êtes maintenant prêt à [Ajouter un contrôle](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) UWP encapsulé dans l’application.

1. Dans **Explorateur de solutions**, développez le dossier **views** du projet **ContosoExpenses. Core** , puis double-cliquez sur le fichier **ExpenseDetail. Xaml** .

2. Dans l’élément de **fenêtre** près du haut du fichier XAML, ajoutez l’attribut suivant. Cela fait référence à l’espace de noms XAML pour le contrôle UWP encapsulé dans [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) .

    ```xml
    xmlns:toolkit="clr-namespace:Microsoft.Toolkit.Wpf.UI.Controls;assembly=Microsoft.Toolkit.Wpf.UI.Controls"
    ```

    Après avoir ajouté cet attribut, l’élément de **fenêtre** doit maintenant ressembler à ceci.

    ```xml
    <Window x:Class="ContosoExpenses.Views.ExpenseDetail"
            xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
            xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
            xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
            xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
            xmlns:toolkit="clr-namespace:Microsoft.Toolkit.Wpf.UI.Controls;assembly=Microsoft.Toolkit.Wpf.UI.Controls"
            xmlns:converters="clr-namespace:ContosoExpenses.Converters"
            DataContext="{Binding Source={StaticResource ViewModelLocator}, Path=ExpensesDetailViewModel}"
            xmlns:local="clr-namespace:ContosoExpenses"
            mc:Ignorable="d"
            Title="Expense Detail" Height="500" Width="800"
            Background="{StaticResource HorizontalBackground}">
    ```

4. Dans le fichier **ExpenseDetail. Xaml** , recherchez la balise de fermeture `</Grid>` qui précède immédiatement le `<!-- Chart -->` commentaire. Ajoutez le code XAML suivant juste avant la `</Grid>` balise de fermeture. Ce code XAML ajoute un contrôle **InkCanvas** (préfixé par le mot clé **Toolkit** que vous avez défini précédemment en tant qu’espace de noms) et un **TextBlock** simple qui agit comme un en-tête pour le contrôle.

    ```xml
    <TextBlock Text="Signature:" FontSize="16" FontWeight="Bold" Grid.Row="5" />

    <toolkit:InkCanvas x:Name="Signature" Grid.Row="6" />
    ```

5. Enregistrez le fichier **ExpenseDetail. Xaml** .

6. Appuyez sur F5 pour exécuter l’application dans le débogueur.

7. Choisissez un employé dans la liste, puis choisissez l’une des dépenses disponibles. Notez que la page de détails des dépenses contient de l’espace pour le contrôle **InkCanvas** .

    ![Stylet de canevas manuscrit uniquement](images/wpf-modernize-tutorial/InkCanvasPenOnly.png)

    Si vous avez un appareil qui prend en charge un stylet numérique, comme une surface, et que vous exécutez ce laboratoire sur un ordinateur physique, passez à l’utilisation et essayez de l’utiliser. L’encre numérique s’affiche à l’écran. Toutefois, si vous n’avez pas de périphérique avec stylet et que vous essayez de vous connecter avec la souris, rien ne se produit. Cela est dû au fait que le contrôle **InkCanvas** est activé uniquement pour les stylets numériques par défaut. Toutefois, nous pouvons modifier ce comportement.

8. Fermez l’application et double-cliquez sur le fichier **ExpenseDetail.Xaml.cs** dans le dossier **views** du projet **ContosoExpenses. Core** .

9. Ajoutez la déclaration d’espace de noms suivante en haut de la classe :

    ```csharp
    using Microsoft.Toolkit.Win32.UI.Controls.Interop.WinRT;
    ```

10. Recherchez le `ExpenseDetail()` constructeur.

11. Ajoutez la ligne de code suivante après la `InitializeComponent()` méthode et enregistrez le fichier de code.

    ```csharp
    Signature.InkPresenter.InputDeviceTypes = CoreInputDeviceTypes.Mouse | CoreInputDeviceTypes.Pen;
    ```

    Vous pouvez utiliser l’objet **InkPresenter** pour personnaliser l’expérience d’encrage par défaut. Ce code utilise la propriété **InputDeviceTypes** pour activer l’entrée de la souris et du stylet.

12. Appuyez de nouveau sur F5 pour régénérer et exécuter l’application dans le débogueur. Choisissez un employé dans la liste, puis choisissez l’une des dépenses disponibles.

13. Essayez maintenant de dessiner un texte dans l’espace de la signature avec la souris. Cette fois-ci, vous verrez que l’encre apparaît à l’écran.

    ![Signature](images/wpf-modernize-tutorial/Signature.png)

## <a name="next-steps"></a>Étapes suivantes

À ce stade du didacticiel, vous avez ajouté avec succès un contrôle de l' **INKCANVAS** UWP à l’application Contoso depenses. Vous êtes maintenant prêt pour [la partie 3 : Ajoutez un contrôle UWP CalendarView à l’aide](modernize-wpf-tutorial-3.md)des îlots XAML.
