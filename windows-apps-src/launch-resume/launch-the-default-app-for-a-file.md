---
title: Lancer l’application par défaut d’un fichier
description: Découvrez comment lancer l’application par défaut d’un fichier.
ms.assetid: BB45FCAF-DF93-4C99-A8B5-59B799C7BD98
ms.date: 07/05/2018
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: f8e59ae5fb20ce8e1a900f7c1415a699715215e0
ms.sourcegitcommit: 231065c899d0de285584d41e6335251e0c2c4048
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/10/2018
ms.locfileid: "8827934"
---
# <a name="launch-the-default-app-for-a-file"></a>Lancer l’application par défaut pour un fichier

**API importantes**

-   [**Windows.System.Launcher.LaunchFileAsync**](https://msdn.microsoft.com/library/windows/apps/hh701461)

Découvrez comment lancer l’application par défaut pour un fichier. De nombreuses applications doivent utiliser des fichiers qu’elles ne peuvent pas gérer elles-mêmes. Par exemple, les applications de courrier électronique reçoivent une grande variété de types de fichiers, et ont besoin d’un moyen de lancer ces fichiers dans leurs gestionnaires par défaut. Ces étapes montrent comment utiliser l’API [**Windows.System.Launcher**](https://msdn.microsoft.com/library/windows/apps/br241801) pour lancer le gestionnaire par défaut d’un fichier que votre application ne peut pas gérer elle-même.

## <a name="get-the-file-object"></a>Obtenir l’objet fichier

Tout d’abord, obtenez un objet [**Windows.Storage.StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) pour le fichier.

Si le fichier est inclus dans le package de votre application, vous pouvez utiliser la propriété [**Package.InstalledLocation**](https://msdn.microsoft.com/library/windows/apps/br224681) pour obtenir un objet [**Windows.Storage.StorageFolder**](https://msdn.microsoft.com/library/windows/apps/br227230) et la méthode [**Windows.Storage.StorageFolder.GetFileAsync**](https://msdn.microsoft.com/library/windows/apps/br227272) pour obtenir l’objet [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171).

Si le fichier se trouve dans un dossier connu, vous pouvez utiliser les propriétés de la classe [**Windows.Storage.KnownFolders**](https://msdn.microsoft.com/library/windows/apps/br227151) pour obtenir un élément [**StorageFolder**](https://msdn.microsoft.com/library/windows/apps/br227230) et la méthode [**GetFileAsync**](https://msdn.microsoft.com/library/windows/apps/br227272) pour obtenir l’objet [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171).

## <a name="launch-the-file"></a>Lancer le fichier

Windows fournit plusieurs options différentes pour le lancement du gestionnaire par défaut d’un fichier. Ces options sont décrites dans ce tableau et dans les sections qui suivent.

| Option | Méthode | Description |
|--------|--------|-------------|
| Lancement par défaut | [**LaunchFileAsync(IStorageFile)**](https://msdn.microsoft.com/library/windows/apps/hh701471) | Lancer le fichier spécifié avec le gestionnaire par défaut. |
| Ouvrir avec lancement | [**LaunchFileAsync(IStorageFile, LauncherOptions)**](https://msdn.microsoft.com/library/windows/apps/hh701465) | Lancer le fichier spécifié en laissant l’utilisateur choisir le gestionnaire dans la boîte de dialogue Ouvrir avec. |
| Lancer avec une application de secours recommandée | [**LaunchFileAsync(IStorageFile, LauncherOptions)**](https://msdn.microsoft.com/library/windows/apps/hh701465) | Lancer le fichier spécifié avec le gestionnaire par défaut. Si aucun gestionnaire n’est installé sur le système, recommandez une application du Windows Store à l’utilisateur. |
| Lancer avec un affichage restant souhaité | [**LaunchFileAsync(IStorageFile, LauncherOptions)**](https://msdn.microsoft.com/library/windows/apps/hh701465) (Windows uniquement) | Lancer le fichier spécifié avec le gestionnaire par défaut. Spécifiez une préférence à conserver à l’écran après le lancement et demandez une taille de fenêtre spécifique. [**LauncherOptions.DesiredRemainingView**](https://msdn.microsoft.com/library/windows/apps/dn298314) n’est pas pris en charge sur la famille d’appareils mobiles. |

### <a name="default-launch"></a>Lancement par défaut

Appelez la méthode [**Windows.System.Launcher.LaunchFileAsync(IStorageFile)**](https://msdn.microsoft.com/library/windows/apps/hh701471) pour lancer l’application par défaut. Cet exemple utilise la méthode [**Windows.Storage.StorageFolder.GetFileAsync**](https://msdn.microsoft.com/library/windows/apps/br227272) pour lancer un fichier d’image, test.png, inclus dans le package de l’application.

```csharp
async void DefaultLaunch()
{
   // Path to the file in the app package to launch
   string imageFile = @"images\test.png";
   
   var file = await Windows.ApplicationModel.Package.Current.InstalledLocation.GetFileAsync(imageFile);
   
   if (file != null)
   {
      // Launch the retrieved file
      var success = await Windows.System.Launcher.LaunchFileAsync(file);

      if (success)
      {
         // File launched
      }
      else
      {
         // File launch failed
      }
   }
   else
   {
      // Could not find file
   }
}
```

```vb
async Sub DefaultLaunch()
   ' Path to the file in the app package to launch
   Dim imageFile = "images\test.png"
   Dim file = await Windows.ApplicationModel.Package.Current.InstalledLocation.GetFileAsync(imageFile)
   
   If file IsNot Nothing Then
      ' Launch the retrieved file
      Dim success = await Windows.System.Launcher.LaunchFileAsync(file)

      If success Then
         ' File launched
      Else
         ' File launch failed
      End If
   Else
      ' Could not find file
   End If
End Sub
```

```cppwinrt
Windows::Foundation::IAsyncAction MainPage::DefaultLaunch()
{
    auto installFolder{ Windows::ApplicationModel::Package::Current().InstalledLocation() };

    Windows::Storage::StorageFile file{ co_await installFolder.GetFileAsync(L"images\\test.png") };

    if (file)
    {
        // Launch the retrieved file
        bool success = co_await Windows::System::Launcher::LaunchFileAsync(file);
        if (success)
        {
            // File launched
        }
        else
        {
            // File launch failed
        }
    }
    else
    {
        // Could not find file
    }
}
```

```cpp
void MainPage::DefaultLaunch()
{
   auto installFolder = Windows::ApplicationModel::Package::Current->InstalledLocation;

   concurrency::task<Windows::Storage::StorageFile^getFileOperation(installFolder->GetFileAsync("images\\test.png"));
   getFileOperation.then([](Windows::Storage::StorageFile^ file)
   {
      if (file != nullptr)
      {
         // Launch the retrieved file
         concurrency::task<bool> launchFileOperation(Windows::System::Launcher::LaunchFileAsync(file));
         launchFileOperation.then([](bool success)
         {
            if (success)
            {
               // File launched
            }
            else
            {
               // File launch failed
            }
         });
      }
      else
      {
         // Could not find file
      }
   });
}
```

### <a name="open-with-launch"></a>Ouvrir avec lancement

Appelez la méthode [**Windows.System.Launcher.LaunchFileAsync(IStorageFile, LauncherOptions)**](https://msdn.microsoft.com/library/windows/apps/hh701465) avec [**LauncherOptions.DisplayApplicationPicker**](https://msdn.microsoft.com/library/windows/apps/hh701438) défini sur **true** pour lancer l’application que l’utilisateur sélectionne dans la boîte de dialogue **Ouvrir avec**.

Nous vous recommandons d’utiliser la boîte de dialogue **Ouvrir avec** lorsque l’utilisateur souhaite sélectionner une application qui n’est pas l’application par défaut pour un fichier particulier. Par exemple, si votre application permet à l’utilisateur de lancer un fichier image, le gestionnaire par défaut sera probablement une visionneuse. Dans certains cas, l’utilisateur peut vouloir modifier l’image au lieu de la visualiser. Utilisez l’option **Ouvrir avec** au moyen d’une autre commande dans **AppBar** ou dans un menu contextuel afin de permettre à l’utilisateur d’afficher la boîte de dialogue **Ouvrir avec** et de sélectionner l’éditeur dans ces types de scénarios.

![boîte de dialogue Ouvrir avec pour le lancement d’un fichier .png. la boîte de dialogue contient une case à cocher qui indique si le choix de l’utilisateur doit être utilisé pour tous les fichiers .png ou simplement ce fichier .png. La boîte de dialogue contient quatre options d’application pour le lancement du fichier et un lien vers d’autres options.](images/checkboxopenwithdialog.png)

```csharp
async void DefaultLaunch()
{
   // Path to the file in the app package to launch
      string imageFile = @"images\test.png";
      
   var file = await Windows.ApplicationModel.Package.Current.InstalledLocation.GetFileAsync(imageFile);

   if (file != null)
   {
      // Set the option to show the picker
      var options = new Windows.System.LauncherOptions();
      options.DisplayApplicationPicker = true;

      // Launch the retrieved file
      bool success = await Windows.System.Launcher.LaunchFileAsync(file, options);
      if (success)
      {
         // File launched
      }
      else
      {
         // File launch failed
      }
   }
   else
   {
      // Could not find file
   }
}
```

```vb
async Sub DefaultLaunch()

   ' Path to the file in the app package to launch
   Dim imageFile = "images\test.png"

   Dim file = await Windows.ApplicationModel.Package.Current.InstalledLocation.GetFileAsync(imageFile)

   If file IsNot Nothing Then
      ' Set the option to show the picker
      Dim options = Windows.System.LauncherOptions()
      options.DisplayApplicationPicker = True

      ' Launch the retrieved file
      Dim success = await Windows.System.Launcher.LaunchFileAsync(file)

      If success Then
         ' File launched
      Else
         ' File launch failed
      End If
   Else
      ' Could not find file
   End If
End Sub
```

```cppwinrt
Windows::Foundation::IAsyncAction MainPage::DefaultLaunch()
{
    auto installFolder{ Windows::ApplicationModel::Package::Current().InstalledLocation() };

    Windows::Storage::StorageFile file{ co_await installFolder.GetFileAsync(L"images\\test.png") };

    if (file)
    {
        // Set the option to show the picker
        Windows::System::LauncherOptions launchOptions;
        launchOptions.DisplayApplicationPicker(true);

        // Launch the retrieved file
        bool success = co_await Windows::System::Launcher::LaunchFileAsync(file, launchOptions);
        if (success)
        {
            // File launched
        }
        else
        {
            // File launch failed
        }
    }
    else
    {
        // Could not find file
    }
}
```

```cpp
void MainPage::DefaultLaunch()
{
   auto installFolder = Windows::ApplicationModel::Package::Current->InstalledLocation;

   concurrency::task<Windows::Storage::StorageFile^> getFileOperation(installFolder->GetFileAsync("images\\test.png"));
   getFileOperation.then([](Windows::Storage::StorageFile^ file)
   {
      if (file != nullptr)
      {
         // Set the option to show the picker
         auto launchOptions = ref new Windows::System::LauncherOptions();
         launchOptions->DisplayApplicationPicker = true;

         // Launch the retrieved file
         concurrency::task<bool> launchFileOperation(Windows::System::Launcher::LaunchFileAsync(file, launchOptions));
         launchFileOperation.then([](bool success)
         {
            if (success)
            {
               // File launched
            }
            else
            {
               // File launch failed
            }
         });
      }
      else
      {
         // Could not find file
      }
   });
}
```

**Lancer avec une application de secours recommandée**

Dans certains cas, l’utilisateur ne dispose pas forcément d’une application installée pour gérer le fichier que vous lancez. Par défaut, Windows gèrera ces cas en fournissant à l’utilisateur un lien pour rechercher une application appropriée dans le Windows Store. Si vous voulez recommander à l’utilisateur l’acquisition d’une application spécifique dans ce scénario, vous pouvez passer cette recommandation avec le fichier que vous lancez. Pour ce faire, appelez la méthode [**Windows.System.Launcher.launchFileAsync(IStorageFile, LauncherOptions)**](https://msdn.microsoft.com/library/windows/apps/hh701465) avec [**LauncherOptions.PreferredApplicationPackageFamilyName**](https://msdn.microsoft.com/library/windows/apps/hh965482) ayant pour valeur le nom de famille du package de l’application que vous voulez recommander dans le Windows Store. Affectez ensuite à [**LauncherOptions.PreferredApplicationDisplayName**](https://msdn.microsoft.com/library/windows/apps/hh965481) le nom de cette application. Windows utilisera cette information pour remplacer l’option générale permettant de rechercher une application dans le Windows Store par une option spécifique permettant d’acquérir l’application recommandée dans le Windows Store.

> [!NOTE]
> Vous devez définir ces deux options pour recommander une application. La définition de l’une sans l’autre conduit à un échec.

![boîte de dialogue Ouvrir avec pour le lancement d’un fichier .contoso. étant donné qu’aucun gestionnaire n’est installé sur l’ordinateur pour les fichiers .contoso, la boîte de dialogue contient une option avec l’icône du Windows Store et du texte qui indique à l’utilisateur le gestionnaire approprié sur le Windows Store. La boîte de dialogue contient également un lien vers d’autres options.](images/howdoyouwanttoopen.png)

```csharp
async void DefaultLaunch()
{
   // Path to the file in the app package to launch
   string imageFile = @"images\test.contoso";

   // Get the image file from the package's image directory
   var file = await Windows.ApplicationModel.Package.Current.InstalledLocation.GetFileAsync(imageFile);

   if (file != null)
   {
      // Set the recommended app
      var options = new Windows.System.LauncherOptions();
      options.PreferredApplicationPackageFamilyName = "Contoso.FileApp_8wknc82po1e";
      options.PreferredApplicationDisplayName = "Contoso File App";

      // Launch the retrieved file pass in the recommended app
      // in case the user has no apps installed to handle the file
      bool success = await Windows.System.Launcher.LaunchFileAsync(file, options);
      if (success)
      {
         // File launched
      }
      else
      {
         // File launch failed
      }
   }
   else
   {
      // Could not find file
   }
}
```

```vb
async Sub DefaultLaunch()

   ' Path to the file in the app package to launch
   Dim imageFile = "images\test.contoso"

   ' Get the image file from the package's image directory
   Dim file = await Windows.ApplicationModel.Package.Current.InstalledLocation.GetFileAsync(imageFile)

   If file IsNot Nothing Then
      ' Set the recommended app
      Dim options = Windows.System.LauncherOptions()
      options.PreferredApplicationPackageFamilyName = "Contoso.FileApp_8wknc82po1e";
      options.PreferredApplicationDisplayName = "Contoso File App";

      ' Launch the retrieved file pass in the recommended app
      ' in case the user has no apps installed to handle the file
      Dim success = await Windows.System.Launcher.LaunchFileAsync(file)

      If success Then
         ' File launched
      Else
         ' File launch failed
      End If
   Else
      ' Could not find file
   End If
End Sub
```

```cppwinrt
Windows::Foundation::IAsyncAction MainPage::DefaultLaunch()
{
    auto installFolder{ Windows::ApplicationModel::Package::Current().InstalledLocation() };

    Windows::Storage::StorageFile file{ co_await installFolder.GetFileAsync(L"images\\test.png") };

    if (file)
    {
        // Set the recommended app
        Windows::System::LauncherOptions launchOptions;
        launchOptions.PreferredApplicationPackageFamilyName(L"Contoso.FileApp_8wknc82po1e");
        launchOptions.PreferredApplicationDisplayName(L"Contoso File App");

        // Launch the retrieved file, and pass in the recommended app
        // in case the user has no apps installed to handle the file.
        bool success = co_await Windows::System::Launcher::LaunchFileAsync(file, launchOptions);
        if (success)
        {
            // File launched
        }
        else
        {
            // File launch failed
        }
    }
    else
    {
        // Could not find file
    }
}
```

```cpp
void MainPage::DefaultLaunch()
{
   auto installFolder = Windows::ApplicationModel::Package::Current->InstalledLocation;

   concurrency::task<Windows::Storage::StorageFile^> getFileOperation(installFolder->GetFileAsync("images\\test.contoso"));
   getFileOperation.then([](Windows::Storage::StorageFile^ file)
   {
      if (file != nullptr)
      {
         // Set the recommended app
         auto launchOptions = ref new Windows::System::LauncherOptions();
         launchOptions->PreferredApplicationPackageFamilyName = "Contoso.FileApp_8wknc82po1e";
         launchOptions->PreferredApplicationDisplayName = "Contoso File App";
         
         // Launch the retrieved file pass, and in the recommended app
         // in case the user has no apps installed to handle the file.
         concurrency::task<bool> launchFileOperation(Windows::System::Launcher::LaunchFileAsync(file, launchOptions));
         launchFileOperation.then([](bool success)
         {
            if (success)
            {
               // File launched
            }
            else
            {
               // File launch failed
            }
         });
      }
      else
      {
         // Could not find file
      }
   });
}
```

### <a name="launch-with-a-desired-remaining-view-windows-only"></a>Lancer avec un affichage restant souhaité (Windows uniquement)

Les applications sources qui appellent la méthode [**LaunchFileAsync**](https://msdn.microsoft.com/library/windows/apps/hh701461) peuvent demander à rester à l’écran après le lancement d’un fichier. Par défaut, Windows essaie de partager tout l’espace disponible de manière équitable entre l’application source et l’application cible qui gère le fichier. Les applications sources peuvent utiliser la propriété [**DesiredRemainingView**](https://msdn.microsoft.com/library/windows/apps/dn298314) pour indiquer au système d’exploitation qu’elles préfèrent que leur fenêtre d’application occupe une plus grande ou plus petite partie de l’espace disponible. La propriété **DesiredRemainingView** peut également servir à indiquer que l’application source n’a pas besoin de rester à l’écran après le lancement du fichier et qu’elle peut être complètement remplacée par l’application cible. Cette propriété spécifie uniquement la taille de fenêtre par défaut de l’application appelante. Elle ne spécifie pas le comportement d’autres applications qui peuvent se trouver en même temps sur l’écran.

> [!NOTE]
> Windows prend en compte plusieurs facteurs différents pour déterminer taille de finale de la fenêtre de l’application source, par exemple, la préférence de l’application source, le nombre d’applications à l’écran, l’orientation de l’écran et ainsi de suite. La définition de la propriété [**DesiredRemainingView**](https://msdn.microsoft.com/library/windows/apps/dn298314) ne garantit pas un comportement de fenêtrage spécifique pour l’application source.

**Famille d’appareils mobiles:** [**LauncherOptions.DesiredRemainingView**](https://msdn.microsoft.com/library/windows/apps/dn298314) n’est pas pris en charge sur la famille d’appareils mobiles.

```csharp
async void DefaultLaunch()
{
   // Path to the file in the app package to launch
   string imageFile = @"images\test.png";
   
   var file = await Windows.ApplicationModel.Package.Current.InstalledLocation.GetFileAsync(imageFile);

   if (file != null)
   {
      // Set the desired remaining view
      var options = new Windows.System.LauncherOptions();
      options.DesiredRemainingView = Windows.UI.ViewManagement.ViewSizePreference.UseLess;

      // Launch the retrieved file
      bool success = await Windows.System.Launcher.LaunchFileAsync(file, options);
      if (success)
      {
         // File launched
      }
      else
      {
         // File launch failed
      }
   }
   else
   {
      // Could not find file
   }
}
```

```cppwinrt
Windows::Foundation::IAsyncAction MainPage::DefaultLaunch()
{
    auto installFolder{ Windows::ApplicationModel::Package::Current().InstalledLocation() };

    Windows::Storage::StorageFile file{ co_await installFolder.GetFileAsync(L"images\\test.png") };

    if (file)
    {
        // Set the desired remaining view.
        Windows::System::LauncherOptions launchOptions;
        launchOptions.DesiredRemainingView(Windows::UI::ViewManagement::ViewSizePreference::UseLess);

        // Launch the retrieved file.
        bool success = co_await Windows::System::Launcher::LaunchFileAsync(file, launchOptions);
        if (success)
        {
            // File launched
        }
        else
        {
            // File launch failed
        }
    }
    else
    {
        // Could not find file
    }
}
```

```cpp
void MainPage::DefaultLaunch()
{
   auto installFolder = Windows::ApplicationModel::Package::Current->InstalledLocation;

   concurrency::task<Windows::Storage::StorageFile^> getFileOperation(installFolder->GetFileAsync("images\\test.png"));
   getFileOperation.then([](Windows::Storage::StorageFile^ file)
   {
      if (file != nullptr)
      {
         // Set the desired remaining view.
         auto launchOptions = ref new Windows::System::LauncherOptions();
         launchOptions->DesiredRemainingView = Windows::UI::ViewManagement::ViewSizePreference::UseLess;

         // Launch the retrieved file.
         concurrency::task<bool> launchFileOperation(Windows::System::Launcher::LaunchFileAsync(file, launchOptions));
         launchFileOperation.then([](bool success)
         {
            if (success)
            {
               // File launched
            }
            else
            {
               // File launch failed
            }
         });
      }
      else
      {
         // Could not find file
      }
   });
}
```

## <a name="remarks"></a>Remarques

Votre application ne peut pas sélectionner l’application qui est lancée. L’utilisateur détermine l’application à lancer. L’utilisateur peut sélectionner une application de la plateforme Windows universelle (UWP) ou une application de bureau Windows.

Lors du lancement d’un fichier, votre application doit être l’application au premier plan, c’est-à-dire qu’elle doit être visible pour l’utilisateur. Cette condition permet de garantir que l’utilisateur conserve le contrôle. Pour pouvoir la respecter, veillez à relier directement tous les lancements de fichiers à l’interface utilisateur de votre application. Dans la majorité des cas, l’utilisateur doit toujours exercer une action pour initier un lancement de fichier.

Vous ne pouvez pas lancer des types de fichiers qui contiennent du code ou un script s’ils sont exécutés automatiquement par le système d’exploitation, tels que les fichiers .exe, .msi et .js. Cette restriction protège les utilisateurs de fichiers potentiellement malveillants susceptibles de modifier le système d’exploitation. Vous pouvez utiliser cette méthode pour lancer des types de fichiers qui contiennent un script s’ils sont exécutés automatiquement par une application qui isole le script, tels que les fichiers .docx. Les applications comme Microsoft Word empêchent le script de modifier le système d’exploitation dans des fichiers .docx.

Si vous essayez de lancer un type de fichier restreint, le lancement se soldera par un échec et votre rappel d’erreur sera appelé. Si votre application gère de nombreux types de fichiers divers et que vous vous attendez à rencontrer cette erreur, nous vous recommandons de proposer un mécanisme de secours à vos utilisateurs. Par exemple, vous pouvez leur offrir comme option d’enregistrer le fichier sur le Bureau pour l’ouvrir depuis cet emplacement.

## <a name="related-topics"></a>Rubriquesassociées

### <a name="tasks"></a>Tâches

* [Lancer l’application par défaut pour un URI](launch-default-app.md)
* [Gérer l’activation des fichiers](handle-file-activation.md)

### <a name="guidelines"></a>Recommandations

* [Recommandations en matière de types de fichiers et d’URI](https://msdn.microsoft.com/library/windows/apps/hh700321)

### <a name="reference"></a>Référence

* [**Windows.Storage.StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171)
* [**Windows.System.Launcher.LaunchFileAsync**](https://msdn.microsoft.com/library/windows/apps/hh701461)
