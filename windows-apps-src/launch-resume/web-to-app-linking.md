---
author: TylerMSFT
title: Permettent aux applications pour les sites Web à l’aide de gestionnaires d’URI d’application
description: Attractive avec votre application prenant en charge les applications pour la fonctionnalité de sites Web.
keywords: Liaisons Windows ciblées
ms.author: twhitney
ms.date: 08/25/2017
ms.topic: article
ms.assetid: 260cf387-88be-4a3d-93bc-7e4560f90abc
ms.localizationpriority: medium
ms.openlocfilehash: 7f6438b8d1d7b8a8ce47ed4e5baddcb59285e660
ms.sourcegitcommit: cbe7cf620622a5e4df7414f9e38dfecec1cfca99
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/20/2018
ms.locfileid: "7441183"
---
# <a name="enable-apps-for-websites-using-app-uri-handlers"></a>Permettent aux applications pour les sites Web à l’aide de gestionnaires d’URI d’application

Applications pour les sites Web associe votre application à un site Web afin que lorsqu’un utilisateur ouvre un lien vers votre site Web, votre application est lancée au lieu d’ouvrir le navigateur. Si votre application n’est pas installée, votre site Web s’ouvre comme d’habitude dans le navigateur. Cette expérience est très fiable puisque seuls les propriétaires de contenus vérifiés peuvent s’inscrire pour créer ce type de lien. Les utilisateurs seront en mesure de vérifier toutes leurs liens web applications inscrits en accédant à Paramètres > applications > applications pour les sites Web.

Pour activer le web-la liaison application-vous devez:
- identifier les URI gérés par votre application dans le fichier manifeste;
- Un fichier JSON qui définit l’association entre votre application et votre site Web. déclaration de manifeste avec le nom de famille de Package d’application à la même racine hôte que l’application.
- gérer l’activation dans l’application.

> [!Note]
> À partir de la mise à jour Windows 10 Creators, pris en charge des liens cliqués dans Microsoft Edge lance l’application correspondante. Prise en charge des liens a cliqué dans d’autres navigateurs (par exemple, Internet Explorer, etc.), vous gardera dans l’expérience de navigation.

## <a name="register-to-handle-http-and-https-links-in-the-app-manifest"></a>S’inscrire pour bénéficier du traitement des liens http et https dans le manifeste de l’application

Votre application a besoin d’identifier les URI pour les sites web qu’elle gère. Pour ce faire, ajoutez l’inscription d’extension **Windows.appUriHandler** au fichier manifeste de votre application **Package.appxmanifest**.

Par exemple, si l’adresse de votre site web est «msn.com», vous devez saisir le code suivant dans le fichier manifeste de votre application:

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

La déclaration ci-dessus inscrit votre application afin qu’elle puisse gérer des liens à partir de l’hôte spécifié. Si votre site web comporte plusieurs adresses (par exemple: m.example.com www.example.com et example.com), ajoutez une autre entrée `<uap3:Host Name=... />` dans `<uap3:AppUriHandler>` pour chaque adresse.

## <a name="associate-your-app-and-website-with-a-json-file"></a>Associer votre application et votre site web à un fichier JSON

Pour faire en sorte que seule votre application puisse ouvrir du contenu sur votre site web, ajoutez le nom de la famille de packages de votre application dans un fichier JSON situé à la racine du serveur web ou dans le répertoire connu du domaine. Cela signifie que votre site web autorise les applications répertoriées à ouvrir du contenu sur votre site. Vous pouvez trouver le nom de la famille de packages dans la section Packages du concepteur de manifestes de l’application.

>[!Important]
> Le fichier JSON ne doit pas contenir le suffixe de fichier .json.

Créez un fichier JSON (sans l’extension de fichier .json) nommé **windows-app-web-link** et indiquez le nom de la famille de packages de votre application. Par exemple:

``` JSON
[{
  "packageFamilyName": "Your app's package family name, e.g MyApp_9jmtgj1pbbz6e",
  "paths": [ "*" ],
  "excludePaths" : [ "/news/*", "/blog/*" ]
 }]
```

Windows utilisera une connexion https vers votre site web et recherchera le fichier JSON correspondant sur votre serveur web.

### <a name="wildcards"></a>Caractères génériques

L’exemple de fichier JSON ci-dessus illustre l’utilisation des caractères génériques. Les caractères génériques vous permettent de prendre en charge une grande diversité de liens, avec moins de lignes de code. La liaison application-site web prend en charge deux types de caractères génériques dans le fichier JSON:

| **Caractère générique** | **Description**               |
|--------------|-------------------------------|
| **\***       | Représente une sous-chaîne      |
| **?**        | Représente un caractère unique |

Par exemple, pour `"excludePaths" : [ "/news/*", "/blog/*" ]` dans l’exemple ci-dessus, votre application prendra en charge tous les chemins d’accès qui démarrent avec l’adresse de votre site Web (par exemple, msn.com), **sauf** ceux sous `/news/` et `/blog/`. **msn.com/weather.html** sera donc pris en charge, mais pas ****msn.com/news/topnews.html****.

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

Les chemins d’accès des exclusions sont vérifiés en premier lieu et, s’il existe une correspondance, la page correspondante s’ouvre avec le navigateur au lieu de l’application désignée. Dans l’exemple ci-dessus, «/news/\*» inclut toutes les pages qui se trouvent sous ce chemin d’accès, tandis que «/news\*» (sans barre oblique après «news») inclut les chemins d’accès sous «news\*», comme «newslocal/», «newsinternational/», etc.

## <a name="handle-links-on-activation-to-link-to-content"></a>Gérer les liens à l’activation pour créer un lien vers le contenu

Accédez au fichier **App.xaml.cs** dans la solution VisualStudio de votre application puis, dans **OnActivated()**, ajoutez la gestion des contenus liés. Dans l’exemple suivant, la page ouverte ouvert dans l’application dépend du chemin d’accès de l’URI:

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

## <a name="test-it-out-local-validation-tool"></a>Test: outil de validation locale

Vous pouvez tester la configuration de votre application et de votre site web en exécutant l’outil de vérification de l’inscription de l’hôte de l’application disponible dans:

%windir%\\system32\\**AppHostRegistrationVerifier.exe**

Testez la configuration de votre application et de votre site web en exécutant cet outil avec les paramètres suivants:

**AppHostRegistrationVerifier.exe** *hostname packagefamilyname filepath*

-   Hostname: votre site web (par exemple, microsoft.com)
-   Package Family Name: le nom de la famille de packages (PFN) de votre application
-   File path: le fichier JSON utilisé pour la validation locale (par exemple, C:\\SomeFolder\\windows-app-web-link)

Si l’outil ne renvoie rien, validation fonctionnent sur ce fichier lors du téléchargement. S’il existe un code d’erreur, il fonctionnera pas.

Vous pouvez activer la clé de Registre suivante forcer le chemin d’accès de la mise en correspondance pour les applications chargées dans le cadre de la validation locale:

`HKCU\Software\Classes\LocalSettings\Software\Microsoft\Windows\CurrentVersion\
AppModel\SystemAppData\YourApp\AppUriHandlers`

Nom: `ForceValidation` valeur: `1`

## <a name="test-it-web-validation"></a>Test: validation sur le web

Fermez votre application pour vérifier que l’application est activée lorsque vous cliquez sur un lien. Copiez ensuite l’adresse de l’un des chemins d’accès pris en charge dans votre site web. Par exemple, si l’adresse de votre site web est «msn.com» et si l’un des chemins pris en charge est «path1», vous devez utiliser: `http://msn.com/path1`

Vérifiez que votre application est fermée. Appuyez sur la **touche Windows + R** pour ouvrir la boîte de dialogue **Exécuter**, puis collez le lien dans la fenêtre. Votre application doit alors démarrer à la place du navigateur web.

Vous pouvez aussi tester votre application en la démarrant à partir d’une autre application à l’aide de l’API [LaunchUriAsync](https://msdn.microsoft.com/library/windows/apps/hh701480.aspx). Vous pouvez utiliser cette API pour tester également votre application sur des téléphones.

Si vous souhaitez suivre la logique d’activation du protocole, définissez un point d’arrêt dans le gestionnaire d’événements **OnActivated**.

## <a name="appurihandlers-tips"></a>Conseils concernant AppUriHandlers:

- Veillez à spécifier uniquement les liens que votre application peut gérer.
- Répertoriez tous les hôtes que vous allez prendre en charge.  Notez que www.example.com et example.com sont des hôtes différents.
- Les utilisateurs peuvent utiliser le menu Paramètres pour choisir l’application qu’ils préfèrent pour la gestion des sites web.
- Votre fichier JSON doit être téléchargé sur un serveur https.
- Si vous devez modifier les chemins d’accès que vous souhaitez prendre en charge, vous pouvez republier votre fichier JSON sans avoir à republier votre application. Les utilisateurs verront ces modifications dans un délai de 1 à 8jours.
- Toutes les applications chargées de manière indépendante avec AppUriHandlers auront des liens validés pour l’hôte au moment de l’installation. Il est inutile de charger un fichier JSON pour tester la fonctionnalité.
- Cette fonctionnalité est toujours disponible si votre application est une application UWP lancée avec [LaunchUriAsync](https://msdn.microsoft.com/library/windows/apps/hh701480.aspx) ou une application de bureau Windows lancée avec [ShellExecuteEx](https://msdn.microsoft.com/library/windows/desktop/bb762154(v=vs.85).aspx). Si l’URL correspond à un gestionnaire d’URI d’application enregistré, l’application sera lancée en lieu et place du navigateur.

## <a name="see-also"></a>Voir également

[Web application-exemple de projet](https://github.com/project-rome/AppUriHandlers/tree/master/NarwhalFacts)
[inscription de windows.protocol](https://msdn.microsoft.com/library/windows/apps/br211458.aspx)
[Gérer l’Activation des URI](https://msdn.microsoft.com/windows/uwp/launch-resume/handle-uri-activation)
[Lancement d’Association](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/AssociationLaunching) illustre comment utiliser l’API LaunchUriAsync().
