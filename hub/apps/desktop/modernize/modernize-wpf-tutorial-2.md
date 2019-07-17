---
description: Ce didacticiel montre comment ajouter des interfaces utilisateur UWP XAML, créer des packages MSIX et incorporer d’autres composants modernes dans votre application WPF.
title: Ajouter un contrôle InkCanvas UWP à l’aide d'îles XAML
ms.topic: article
ms.date: 06/27/2019
ms.author: mcleans
author: mcleanbyron
keywords: Windows 10, uwp, WinForms, wpf, xaml (îles)
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: 35b6886389640c7960c4120772c169161779ab68
ms.sourcegitcommit: 734aa941dc675157c07bdeba5059cb76a5626b39
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/15/2019
ms.locfileid: "68141838"
---
# <a name="part-2-add-a-uwp-inkcanvas-control-using-xaml-islands"></a>Partie 2 : Ajouter un contrôle InkCanvas UWP à l’aide d'îles XAML

Il s’agit de la deuxième partie d’un didacticiel qui montre comment moderniser une application de bureau WPF exemple nommée dépenses de Contoso. Pour une vue d’ensemble du didacticiel, configuration requise et des instructions pour télécharger l’exemple d’application, consultez [didacticiel : Moderniser une application WPF](modernize-wpf-tutorial.md). Cet article suppose que vous avez déjà effectué [partie 1](modernize-wpf-tutorial-1.md).

Dans le scénario fictif de ce didacticiel, l’équipe de développement Contoso souhaite ajouter la prise en charge pour les signatures numériques à l’application de dépenses de Contoso. La plateforme Windows universelle **InkCanvas** contrôle est une excellente solution pour ce scénario, car il prend en charge l’encre numérique et intégrant l’intelligence artificielle de fonctionnalités telles que la possibilité de reconnaître les formes et du texte. Pour ce faire, vous allez utiliser le [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) encapsulé contrôle UWP disponible dans le Kit de ressources de communauté de Windows. Ce contrôle encapsule l’interface et des fonctionnalités de la plateforme Windows universelle **InkCanvas** contrôle pour une utilisation dans une application WPF. Pour plus d’informations sur les contrôles UWP encapsulés, consultez [hôte UWP XAML des contrôles dans les applications de bureau (îles XAML)](xaml-islands.md).

## <a name="configure-the-project-to-use-xaml-islands"></a>Configurer le projet pour utiliser XAML (îles)

Avant de pouvoir ajouter un **InkCanvas** contrôle à l’application de dépenses de Contoso, vous devez d’abord configurer le projet pour prendre en charge des îles de XAML UWP.

1. Dans Visual Studio 2019, cliquez sur le **ContosoExpenses.Core** projet **l’Explorateur de solutions** et choisissez **gérer les Packages NuGet**.

    ![Gérer le menu de Packages NuGet dans Visual Studio](images/wpf-modernize-tutorial//ManageNuGetPackages.png)

2. Dans le **Gestionnaire de Package NuGet** fenêtre, cliquez sur **Parcourir**. Sélectionnez le **inclure la version préliminaire** option, recherchez le `Microsoft.Toolkit.Wpf.UI.Controls` empaqueter et installer la dernière version du package indiqué dans les résultats.

    > [!NOTE]
    > Ce package contient toute l’infrastructure nécessaire pour l’hébergement des îles de XAML UWP dans une application WPF, y compris le **InkCanvas** encapsulé de contrôle UWP. Un package similaire nommé `Microsoft.Toolkit.Forms.UI.Controls` est disponible pour les applications Windows Forms.

3. Avec le bouton droit **ContosoExpenses.Core** projet **l’Explorateur de solutions** et choisissez **Ajouter -> nouvel élément**.

4. Sélectionnez **fichier manifeste d’Application**, nommez-le **App.manifest**, puis cliquez sur **ajouter**.

5. Dans le fichier manifeste ouvert, recherchez le **compatibilité** section et identifiez les éléments suivants commentés **supportedOS** élément pour Windows 10.

    ```xml
    <!-- Windows 10 -->
    <!--<supportedOS Id="{8e0f7a12-bfb3-4fe8-b9a5-48fd50a15a9a}" />-->
    ```

6. Sous cet élément, ajoutez le code suivant **maxversiontested** élément.

    ```xml
    <maxversiontested Id="10.0.18362.0"/>
    ```

7. Supprimez les commentaires de la **supportedOS** élément pour Windows 10. Cette section doit maintenant ressembler à ceci.

    ```xml
    <!-- Windows 10 -->
    <supportedOS Id="{8e0f7a12-bfb3-4fe8-b9a5-48fd50a15a9a}" />
    <maxversiontested Id="10.0.18362.0"/>
    ```

    > [!NOTE]
    > Le **maxversiontested** élément spécifie que l’application nécessite Windows 10, version 1903 (build 18362) ou version ultérieure. Il s’agit de la première version de Windows 10 qui prend en charge des îles de XAML. Sans cette entrée dans le manifeste d’application, l’application lève une exception au moment de l’exécution. Après l’ajout de cet élément de l’avertissement de génération suivant peut apparaître dans votre projet : `manifest authoring warning 81010002: Unrecognized Element "maxversiontested" in namespace "urn:schemas-microsoft-com:compatibility.v1"`. Cet avertissement n’indique pas que rien ne va pas dans votre projet, et il peut être ignoré.

8. Dans le fichier manifeste, recherchez les éléments suivants commentés **application** section.

    ```xml
    <!--
    <application xmlns="urn:schemas-microsoft-com:asm.v3">
      <windowsSettings>
        <dpiAware xmlns="http://schemas.microsoft.com/SMI/2005/WindowsSettings">true</dpiAware>
      </windowsSettings>
    </application>
    -->
    ```

9. Supprimer cette section et remplacez-le par le code XML suivant. Cela permet de configurer l’application pour être des PPP prenant en charge et une meilleure handle différents facteurs d’échelle pris en charge par Windows 10.

    ```xml
    <application xmlns="urn:schemas-microsoft-com:asm.v3">
      <windowsSettings>
          <dpiAware xmlns="http://schemas.microsoft.com/SMI/2005/WindowsSettings">true/PM</dpiAware>
          <dpiAwareness xmlns="http://schemas.microsoft.com/SMI/2016/WindowsSettings">PerMonitorV2, PerMonitor</dpiAwareness>
      </windowsSettings>
    </application>
    ```

10. Enregistrez et fermez le `app.manifest` fichier.

12. Dans **l’Explorateur de solutions**, avec le bouton droit le **ContosoExpenses.Core** de projet et choisissez **propriétés**.

13. Dans le **ressources** section de la **Application** onglet, vérifiez que le **manifeste** liste déroulante est définie sur **App.manifest**.

    ![Manifeste de l’application .NET core](images/wpf-modernize-tutorial/NetCoreAppManifest.png)

16. Enregistrer les modifications apportées aux propriétés du projet.

## <a name="add-an-inkcanvas-control-to-the-app"></a>Ajouter un contrôle InkCanvas à l’application

Maintenant que vous avez configuré votre projet pour utiliser des îlots de XAML UWP, vous êtes maintenant prêt à ajouter un [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) encapsulé contrôle UWP à l’application.

1. Dans **l’Explorateur de solutions**, développez le **vues** dossier de la **ContosoExpenses.Core** de projet et double-cliquez sur le **ExpenseDetail.xaml**fichier.

2. Dans le **fenêtre** élément vers le haut du fichier XAML, ajoutez l’attribut suivant. Cela fait référence à l’espace de noms XAML pour le [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) encapsulé de contrôle UWP.

    ```xml
    xmlns:toolkit="clr-namespace:Microsoft.Toolkit.Wpf.UI.Controls;assembly=Microsoft.Toolkit.Wpf.UI.Controls"
    ```

    Après l’ajout de cet attribut, le **fenêtre** élément doit maintenant ressembler à ceci.

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

4. Dans le **ExpenseDetail.xaml** de fichier, recherchez la fermeture `</Grid>` balise qui précède immédiatement la `<!-- Chart -->` commentaire. Ajoutez le XAML suivant juste avant la fermeture `</Grid>` balise. Ce XAML ajoute un **InkCanvas** contrôle (précédé la **toolkit** mot clé que vous avez défini précédemment comme un espace de noms) et une simple **TextBlock** qui agit comme un en-tête pour le contrôle.

    ```xml
    <TextBlock Text="Signature:" FontSize="16" FontWeight="Bold" Grid.Row="5" />

    <toolkit:InkCanvas x:Name="Signature" Grid.Row="6" />
    ```

5. Enregistrer le **ExpenseDetail.xaml** fichier.

6. Appuyez sur F5 pour exécuter l’application dans le débogueur.

7. Choisissez un employé dans la liste, puis choisissez une des dépenses disponibles. Notez que la page de détail des frais contient un espace pour le **InkCanvas** contrôle.

    ![Stylo canevas uniquement](images/wpf-modernize-tutorial/InkCanvasPenOnly.png)

    Si vous avez un appareil qui prend en charge un stylo numérique, comme une Surface, et que vous exécutez ce laboratoire sur un ordinateur physique, allez et tenter d’utiliser. Vous verrez l’encre numérique s’affichant à l’écran. Toutefois, si vous n’avez pas un périphérique stylet et que vous essayez de vous connecter avec votre souris, rien ne se produira. Cela se produit, car le **InkCanvas** contrôle est activé uniquement pour les stylets numériques par défaut. Toutefois, nous pouvons modifier ce comportement.

8. Fermez l’application et double-cliquez sur le **ExpenseDetail.xaml.cs** de fichiers sous le **vues** dossier de la **ContosoExpenses.Core** projet.

9. Ajoutez la déclaration d’espace de noms suivante en haut de la classe :

    ```csharp
    using Microsoft.Toolkit.Win32.UI.Controls.Interop.WinRT;
    ```

10. Recherchez le `ExpenseDetail()` constructeur.

11. Ajoutez la ligne suivante de code directement après le `InitializeComponent()` (méthode) et enregistrez le fichier de code.

    ```csharp
    Signature.InkPresenter.InputDeviceTypes = CoreInputDeviceTypes.Mouse | CoreInputDeviceTypes.Pen;
    ```

    Vous pouvez utiliser la **InkPresenter** objet pour personnaliser la valeur par défaut pour l’écriture manuscrite expérience. Ce code utilise le **InputDeviceTypes** propriété pour activer la souris, ainsi que d’entrée de stylet.

12. Appuyez sur F5 pour régénérer et exécuter l’application dans le débogueur. Choisissez un employé dans la liste, puis choisissez une des dépenses disponibles.

13. Tentez maintenant dessinez un élément dans l’espace de la signature avec la souris. Cette fois, vous verrez l’encre apparaît sur l’écran.

    ![Signature](images/wpf-modernize-tutorial/Signature.png)

## <a name="next-steps"></a>Étapes suivantes

À ce stade du didacticiel, vous avez ajouté une UWP **InkCanvas** contrôle à l’application de dépenses de Contoso. Vous êtes maintenant prêt pour [partie 3 : Ajouter un contrôle UWP CalendarView à l’aide de XAML îles](modernize-wpf-tutorial-3.md).
