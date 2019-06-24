---
Description: Distribuer une application de bureau empaquetée (pont du bureau)
title: Publier votre application de bureau empaquetée dans le Microsoft Store ou le chargement de version test sur un ou plusieurs appareils.
ms.date: 05/18/2018
ms.topic: article
keywords: windows 10, uwp
ms.assetid: edff3787-cecb-4054-9a2d-1fbefa79efc4
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.openlocfilehash: 2d10836da46cce4d862f7f727890b0c9c107df5a
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/21/2019
ms.locfileid: "67317063"
---
# <a name="distribute-your-packaged-desktop-app"></a>Distribuer votre application de bureau empaquetée

Si vous décidez de [empaqueter votre application de bureau dans un package MSIX](/windows/msix/desktop/desktop-to-uwp-root), vous pouvez publier votre application empaquetée dans le Microsoft Store ou le chargement de version test sur un ou plusieurs appareils.

> [!NOTE]
> Avez-vous un plan pour comment vous pouvez transférer les utilisateurs à votre application empaquetée ? Avant de distribuer votre application, consultez la section [Migration des utilisateurs vers votre application empaquetée](#transition-users) de ce guide où vous trouverez quelques idées.

## <a name="distribute-your-application-by-publishing-it-to-the-microsoft-store"></a>Distribuer votre application en les publiant sur le Microsoft Store

Le [Microsoft Store](https://www.microsoft.com/store/apps) est une méthode pratique pour rendre votre application accessible aux clients.

Publier votre application sur le Microsoft Store pour atteindre l’audience plus large. En outre, les clients d’organisation peuvent acquérir votre application pour distribuer en interne à leur organisation via le [Microsoft Store pour entreprises](https://businessstore.microsoft.com/store).

Si vous envisagez de publier dans le Microsoft Store, vous êtes invité à répondre à quelques questions supplémentaires dans le cadre du processus de soumission. Ce, parce que votre manifeste du package déclare une fonctionnalité restreinte nommée **runFullTrust** et que nous avons besoin d'approuver l’utilisation de cette fonctionnalité par votre application. Vous trouverez plus d’informations sur cette exigence ici : [Des capacités restreintes](/windows/uwp/packaging/app-capability-declarations#restricted-capabilities).

Vous n’êtes pas obligé de vous connecter à votre application avant son envoi vers le Store.

>[!IMPORTANT]
> Si vous envisagez de publier votre application sur le Microsoft Store, assurez-vous que votre application fonctionne correctement sur les appareils qui exécutent Windows 10 S. Il s’agit d’une exigence de Store. Consultez [Tester votre application pour Windows 10 S](/windows/msix/desktop/desktop-to-uwp-test-windows-s).

<a id="side-load" />

## <a name="distribute-your-application-without-placing-it-onto-the-microsoft-store"></a>Distribuer votre application sans le placer sur le Microsoft Store

Si vous devez plutôt distribuer votre application sans utiliser le Store, vous pouvez distribuer manuellement les applications à un ou plusieurs appareils.

Ceci peut être utile si vous souhaitez contrôler davantage l’expérience de distribution ou si vous ne voulez pas vous impliquer dans le processus de certification du Microsoft Store.

Pour distribuer votre application avec d’autres appareils sans le placer dans le Store, vous devez obtenir un certificat, signer votre application à l’aide de ce certificat, puis charger une version test à votre application sur ces appareils.

Vous pouvez [créer un certificat](/windows/uwp/packaging/create-certificate-package-signing) ou en obtenir un auprès d’un fournisseur populaire, tel que [Verisign](https://www.verisign.com/).

Si vous prévoyez de distribuer votre application sur les appareils qui exécutent Windows 10 S, votre application doit être signée par le Microsoft Store, vous devez donc passer par le processus de soumission Store avant de pouvoir distribuer votre application sur ces appareils.

Si vous créez un certificat, vous devez l’installer dans le magasin de certificats **Racine approuvée** ou **Personnes autorisées** de chaque appareil exécutant votre application. Si vous obtenez un certificat auprès d’un fournisseur populaire, vous n’aurez rien à installer sur les autres systèmes, hormis votre application.  

> [!IMPORTANT]
> Assurez-vous que le nom de l’éditeur mentionné sur votre certificat correspond à celui de l’éditeur de votre application.

Pour vous connecter à votre application à l’aide d’un certificat, consultez [signer un package d’application à l’aide de SignTool](/windows/uwp/packaging/sign-app-package-using-signtool).

Chargement de version test, votre application sur d’autres appareils, consultez [LOB de chargement de version test des applications dans Windows 10](/windows/application-management/sideload-apps-in-windows-10).

<a id="transition-users" />

## <a name="transition-users-to-your-packaged-app"></a>Migration des utilisateurs vers votre application empaquetée

Avant de distribuer votre application, envisagez d’ajouter quelques extensions à votre manifeste du package pour aider les utilisateurs à prendre l’habitude d’utiliser votre application empaquetée. Voici certaines choses que vous pouvez faire.

* Pointez les vignettes existantes de l’écran de démarrage et les boutons de barre des tâches vers votre application empaquetée.
* Associer votre application empaquetée avec un ensemble de types de fichiers.
* Rendre votre application empaquetée ouvrir certains types de fichiers par défaut.

Pour obtenir la liste complète des extensions et des conseils pour leur utilisation, voir [Migration des utilisateurs vers votre application](desktop-to-uwp-extensions.md#transition-users-to-your-app).

En outre, envisagez d’ajouter de code à votre application empaquetée qui permet d’effectuer ces tâches :

* Migre les données utilisateur associées à votre application de bureau pour les emplacements de dossier approprié de votre application empaquetée.
* permettre aux utilisateurs de désinstaller la version de bureau de votre application.

Examinons chacune de ces tâches. Nous allons commencer par la migration des données utilisateur.

### <a name="migrate-user-data"></a>Migrer les données utilisateur

Si vous allez ajouter du code qui migre les données utilisateur, il est préférable d’exécuter ce code uniquement lors du premier démarrage de l’application. Avant de migrer les données des utilisateurs, affichez une boîte de dialogue expliquant à l’utilisateur ce qui est en train de se produire, les raisons pour lesquelles cette action est recommandée, et ce qui va advenir des données existantes.

Voici un exemple montrant comment vous pourriez procéder dans une application empaquetée .NET.

```csharp
private void MigrateUserData()
{
    String sourceDir = Environment.GetFolderPath
        (Environment.SpecialFolder.ApplicationData) + "\\AppName";

    if (sourceDir != null)
    {
        DialogResult migrateResult = MessageBox.Show
            ("Would you like to migrate your data from the previous version of this app?",
             "Data Migration", MessageBoxButtons.YesNo);

        if (migrateResult.Equals(DialogResult.Yes))
        {
            String destinationDir =
                Windows.Storage.ApplicationData.Current.LocalFolder.Path + "\\AppName";

            Process process = new Process();
            process.StartInfo.FileName = "robocopy.exe";
            process.StartInfo.Arguments = "%LOCALAPPDATA%\\AppName " + destinationDir + " /move";
            process.StartInfo.CreateNoWindow = true;
            process.Start();
            process.WaitForExit();

            if (process.ExitCode > 1)
            {
                //Migration was unsuccessful -- you can choose to block/retry/other action
            }
        }
    }
}
```

### <a name="uninstall-the-desktop-version-of-your-app"></a>Désinstallez la version bureau de votre application

Il est préférable de ne pas désinstaller l’application de bureau des utilisateurs sans autorisation préalable leur autorisation. Affichez une boîte de dialogue demandant l’autorisation de l’utilisateur. Les utilisateurs peuvent décider de conserver la version bureau de votre application. Si cela se produit, vous devrez décider si vous souhaitez bloquer l’utilisation de l’application de bureau ou prennent en charge l’utilisation côte à côte des deux applications.

Voici un exemple montrant comment vous pourriez procéder dans une application empaquetée .NET.

Pour voir le contexte complet de cet extrait de code, consultez le fichier **MainWindow.cs** de cet exemple [Visionneuse d’images WPF avec transition/migration/désinstallation](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/DesktopAppTransition).

```csharp
private void RemoveDesktopApp()
{              
    //Typically, you can find your uninstall string at this location.
    String uninstallString = (String)Microsoft.Win32.Registry.GetValue
        (@"HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Microsoft\Windows\CurrentVersion" +
         @"\Uninstall\{7AD02FB8-B85E-44BC-8998-F4803BA5A0E3}\", "UninstallString", null);

    //Detect if the previous version of the Desktop application is installed.
    if (uninstallString != null)
    {
        DialogResult uninstallResult = MessageBox.Show
            ("To have the best experience, consider uninstalling the "
              + " previous version of this app. Would you like to do that now?",
              "Uninstall the previous version", MessageBoxButtons.YesNo);

        if (uninstallResult.Equals(DialogResult.Yes))
        {
                    string[] uninstallArgs = uninstallString.Split(' ');

            Process process = new Process();
            process.StartInfo.FileName = uninstallArgs[0];
            process.StartInfo.Arguments = uninstallArgs[1];
            process.StartInfo.CreateNoWindow = true;

            process.Start();
            process.WaitForExit();

            if (process.ExitCode != 0)
            {
                //Uninstallation was unsuccessful - You can choose to block the application here.
            }
        }
    }

}
```

## <a name="next-steps"></a>Étapes suivantes

**Trouvez des réponses à vos questions**

Des questions ? Contactez-nous sur Stack Overflow. Notre équipe contrôle ces [balises](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge). Vous pouvez également nous poser vos questions [ici](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D).

Si vous rencontrez des problèmes lors de la publication de votre application sur le Store, ce [billet de blog](https://blogs.msdn.microsoft.com/appconsult/2017/09/25/preparing-a-desktop-bridge-application-for-the-store-submission/) offre certaines astuces utiles.

**Donner votre avis ou faire des suggestions de fonctionnalité**

Consultez [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial).
