---
title: Activer les applications pour les sites web à l’aide de gestionnaires d’URI d’application
description: Rendez votre application plus attractive en prenant en charge la fonctionnalité Applications pour les sites web.
keywords: Liaisons Windows ciblées
ms.date: 08/25/2017
ms.topic: article
ms.assetid: 260cf387-88be-4a3d-93bc-7e4560f90abc
ms.localizationpriority: medium
ms.openlocfilehash: 66284538c97aee1a11c27beaa483dcfe109b6615
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57641074"
---
# <a name="enable-apps-for-websites-using-app-uri-handlers"></a>Activer les applications pour les sites web à l’aide de gestionnaires d’URI d’application

Applications pour les sites web associe votre application à un site web afin que, lorsqu’un utilisateur ouvre un lien vers votre site web, l'application soit lancée plutôt que d’ouvrir le navigateur. Si votre application n’est pas installée, votre site web s'ouvre dans le navigateur comme d'habitude. Cette expérience est très fiable puisque seuls les propriétaires de contenus vérifiés peuvent s’inscrire pour créer ce type de lien. Les utilisateurs peuvent vérifier toutes les liaisons application-site web enregistrées en accédant à Paramètres > Applications > Applications pour les sites web.

Pour activer la liaison application-site web, vous devez :
- identifier les URI gérés par votre application dans le fichier manifeste ;
- disposer d’un fichier JSON qui définit l’association entre votre application et votre site Web, comportant le nom de la famille de packages de votre application à la même racine hôte que la déclaration de manifeste de l’application ;
- gérer l’activation dans l’application.

> [!Note]
> À compter de la mise à jour Windows 10 Creators, pris en charge des liens cliqués dans Microsoft Edge lancera l’application correspondante. S'il a été cliqué dans d’autres navigateurs sur les liens pris en charge (par exemple, Internet Explorer, etc.), vous resterez dans le navigateur.

## <a name="register-to-handle-http-and-https-links-in-the-app-manifest"></a>S’inscrire pour bénéficier du traitement des liens http et https dans le manifeste de l’application

Votre application a besoin d’identifier les URI pour les sites web qu’elle gère. Pour ce faire, ajoutez l’inscription d’extension **Windows.appUriHandler** au fichier manifeste de votre application **Package.appxmanifest**.

Par exemple, si l’adresse de votre site web est « msn.com », vous devez saisir le code suivant dans le fichier manifeste de votre application :

```xml
<Applications>
  <Application ... >
      ...
      <Extensions>
         <uap3:Extension Category="windows.appUriHandler">
          <uap3:AppUriHandler>
            <uap3:Host Name="msn.com" />
          </uap3:AppUriHandler>
        </uap3:Extension>
      </Extensions>
  </Application>
</Applications>
```

La déclaration ci-dessus inscrit votre application afin qu’elle puisse gérer des liens à partir de l’hôte spécifié. Si votre site web comporte plusieurs adresses (par exemple : m.example.com www.example.com et example.com), ajoutez une autre entrée `<uap3:Host Name=... />` dans `<uap3:AppUriHandler>` pour chaque adresse.

## <a name="associate-your-app-and-website-with-a-json-file"></a>Associer votre application et votre site web à un fichier JSON

Pour faire en sorte que seule votre application puisse ouvrir du contenu sur votre site web, ajoutez le nom de la famille de packages de votre application dans un fichier JSON situé à la racine du serveur web ou dans le répertoire connu du domaine. Cela signifie que votre site web autorise les applications répertoriées à ouvrir du contenu sur votre site. Vous pouvez trouver le nom de la famille de packages dans la section Packages du concepteur de manifestes de l’application.

>[!Important]
> Le fichier JSON ne doit pas contenir le suffixe de fichier .json.

Créez un fichier JSON (sans l’extension de fichier .json) nommé **windows-app-web-link** et indiquez le nom de la famille de packages de votre application. Exemple :

``` JSON
[{
  "packageFamilyName": "Your app's package family name, e.g MyApp_9jmtgj1pbbz6e",
  "paths": [ "*" ],
  "excludePaths" : [ "/news/*", "/blog/*" ]
 }]
```

Windows utilisera une connexion https vers votre site web et recherchera le fichier JSON correspondant sur votre serveur web.

### <a name="wildcards"></a>Caractères génériques

L’exemple de fichier JSON ci-dessus illustre l’utilisation des caractères génériques. Les caractères génériques vous permettent de prendre en charge une grande diversité de liens, avec moins de lignes de code. La liaison application-site web prend en charge deux types de caractères génériques dans le fichier JSON :

| **Wildcard** | **Description**               |
|--------------|-------------------------------|
| **\***       | Représente une sous-chaîne      |
| **?**        | Représente un caractère unique |

Par exemple, avec la ligne `"excludePaths" : [ "/news/*", "/blog/*" ]` donnée dans l’exemple ci-dessus, votre application prendra en charge tous les chemins d’accès qui commencent par l’adresse de votre site web (par exemple, msn.com), **sauf** ceux qui se trouvent sous `/news/` et `/blog/`. **msn.com/weather.html** sera donc pris en charge, mais pas ****msn.com/news/topnews.html****.

### <a name="multiple-apps"></a>Applications multiples

Si vous avez deux applications que vous voulez lier à votre site web, répertoriez les noms de famille de packages des deux applications dans votre fichier JSON **windows-app-web-link**. Les deux applications peuvent être prises en charge. L’utilisateur pourra alors choisir le lien par défaut si les deux sont installées. S’il souhaite par la suite modifier le lien par défaut, il pourra le faire via le menu **Paramètres &gt; Applications pour les sites web**. Les développeurs peuvent également modifier le fichier JSON à tout moment et voir leur modification le jour même, mais pas plus de huit jours après la mise à jour.

``` JSON
[{
  "packageFamilyName": "Your apps's package family name, e.g MyApp_9jmtgj1pbbz6e",
  "paths": [ "*" ],
  "excludePaths" : [ "/news/*", "/blog/*" ]
 },
 {
  "packageFamilyName": "Your second app's package family name, e.g. MyApp2_8jmtgj2pbbz6e",
  "paths": [ "/example/*", "/links/*" ]
 }]
```

Pour offrir à vos utilisateurs la meilleure expérience possible, utilisez les chemins d’accès des exclusions pour vous assurer que le contenu uniquement en ligne est exclu des chemins d’accès pris en charge dans votre fichier JSON.

Les chemins d’accès des exclusions sont vérifiés en premier lieu et, s’il existe une correspondance, la page correspondante s’ouvre avec le navigateur au lieu de l’application désignée. Dans l’exemple ci-dessus, ' /news/\*» inclut toutes les pages sous ce chemin d’accès lors de la ' / news\*» (aucune barre oblique ne pistes « Actualités ») inclut des chemins d’accès sous ' news\*' comme ' newslocal /', ' newsinternational /', et ainsi de suite.

## <a name="handle-links-on-activation-to-link-to-content"></a>Gérer les liens à l’activation pour créer un lien vers le contenu

Accédez au fichier **App.xaml.cs** dans la solution Visual Studio de votre application puis, dans **OnActivated()**, ajoutez la gestion des contenus liés. Dans l’exemple suivant, la page ouverte ouvert dans l’application dépend du chemin d’accès de l’URI :

``` CS
protected override void OnActivated(IActivatedEventArgs e)
{
    Frame rootFrame = Window.Current.Content as Frame;
    if (rootFrame == null)
    {
        ...
    }

    // Check ActivationKind, Parse URI, and Navigate user to content
    Type deepLinkPageType = typeof(MainPage);
    if (e.Kind == ActivationKind.Protocol)
    {
        var protocolArgs = (ProtocolActivatedEventArgs)e;        
        switch (protocolArgs.Uri.AbsolutePath)
        {
            case "/":
                break;
            case "/index.html":
                break;
            case "/sports.html":
                deepLinkPageType = typeof(SportsPage);
                break;
            case "/technology.html":
                deepLinkPageType = typeof(TechnologyPage);
                break;
            case "/business.html":
                deepLinkPageType = typeof(BusinessPage);
                break;
            case "/science.html":
                deepLinkPageType = typeof(SciencePage);
                break;
        }
    }

    if (rootFrame.Content == null)
    {
        // Default navigation
        rootFrame.Navigate(deepLinkPageType, e);
    }

    // Ensure the current window is active
    Window.Current.Activate();
}
```

**Important** Veillez à remplacer la dernière logique `if (rootFrame.Content == null)` par `rootFrame.Navigate(deepLinkPageType, e);`, comme indiqué dans l’exemple ci-dessus.

## <a name="test-it-out-local-validation-tool"></a>Le tester : Outil de validation locale

Vous pouvez tester la configuration de votre application et de votre site web en exécutant l’outil de vérification de l’inscription de l’hôte de l’application disponible dans :

%windir%\\system32\\**AppHostRegistrationVerifier.exe**

Testez la configuration de votre application et de votre site web en exécutant cet outil avec les paramètres suivants :

**AppHostRegistrationVerifier.exe** *filepath packagefamilyname de nom d’hôte*

-   Nom d’hôte : Votre site Web (par exemple, microsoft.com)
-   Nom de famille de packages (NFP) : NFP de votre application
-   Chemin d’accès du fichier : Le fichier JSON pour la validation locale (par exemple, C:\\SomeFolder\\windows-application-web-link)

Si l’outil ne retourne rien, la validation fonctionnera sur ce fichier lors du téléchargement. S’il existe un code d’erreur, il ne fonctionnera pas.

Vous pouvez activer la clé de Registre suivante afin qu'elle force la correspondance du chemin d’accès pour les applications chargées dans le cadre de la validation locale :

`HKCU\Software\Classes\LocalSettings\Software\Microsoft\Windows\CurrentVersion\
AppModel\SystemAppData\YourApp\AppUriHandlers`

KeyName : `ForceValidation` Valeur : `1`

## <a name="test-it-web-validation"></a>Testez-le : Validation de Web

Fermez votre application pour vérifier que l’application est activée lorsque vous cliquez sur un lien. Copiez ensuite l’adresse de l’un des chemins d’accès pris en charge dans votre site web. Par exemple, si l’adresse de votre site Web est « msn.com » et qu’un des chemins d’accès des prise en charge est « chemin1 », vous utiliseriez `http://msn.com/path1`

Vérifiez que votre application est fermée. Appuyez sur la **touche Windows + R** pour ouvrir la boîte de dialogue **Exécuter**, puis collez le lien dans la fenêtre. Votre application doit alors démarrer à la place du navigateur web.

Vous pouvez aussi tester votre application en la démarrant à partir d’une autre application à l’aide de l’API [LaunchUriAsync](https://msdn.microsoft.com/library/windows/apps/hh701480.aspx). Vous pouvez utiliser cette API pour tester également votre application sur des téléphones.

Si vous souhaitez suivre la logique d’activation du protocole, définissez un point d’arrêt dans le gestionnaire d’événements **OnActivated**.

## <a name="appurihandlers-tips"></a>Conseils concernant AppUriHandlers :

- Veillez à spécifier uniquement les liens que votre application peut gérer.
- Répertoriez tous les hôtes que vous allez prendre en charge.  Notez que www.example.com et example.com sont des hôtes différents.
- Les utilisateurs peuvent utiliser le menu Paramètres pour choisir l’application qu’ils préfèrent pour la gestion des sites web.
- Votre fichier JSON doit être téléchargé sur un serveur https.
- Si vous devez modifier les chemins d’accès que vous souhaitez prendre en charge, vous pouvez republier votre fichier JSON sans avoir à republier votre application. Les utilisateurs verront ces modifications dans un délai de 1 à 8 jours.
- Toutes les applications chargées de manière indépendante avec AppUriHandlers auront des liens validés pour l’hôte au moment de l’installation. Il est inutile de charger un fichier JSON pour tester la fonctionnalité.
- Cette fonctionnalité est toujours disponible si votre application est une application UWP lancée avec [LaunchUriAsync](https://msdn.microsoft.com/library/windows/apps/hh701480.aspx) ou une application de bureau Windows lancée avec [ShellExecuteEx](https://msdn.microsoft.com/library/windows/desktop/bb762154(v=vs.85).aspx). Si l’URL correspond à un gestionnaire d’URI d’application enregistré, l’application sera lancée en lieu et place du navigateur.

## <a name="see-also"></a>Voir également

[Exemple de projet Application-site web](https://github.com/project-rome/AppUriHandlers/tree/master/NarwhalFacts)
[Inscription de windows.protocol](https://msdn.microsoft.com/library/windows/apps/br211458.aspx)
[Gérer l’activation des URI](https://msdn.microsoft.com/windows/uwp/launch-resume/handle-uri-activation)
[L'exemple de lancement d'association](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/AssociationLaunching) explique comment utiliser l'API LaunchUriAsync().
