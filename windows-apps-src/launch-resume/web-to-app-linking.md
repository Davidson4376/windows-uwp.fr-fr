---
author: TylerMSFT
title: "Liaisons application-site web avec les gestionnaires d’URI"
description: "Rendez votre application plus attractive pour vos utilisateurs à l’aide des gestionnaires d’URI d’application"
keywords: "Liaison Windows ciblées"
translationtype: Human Translation
ms.sourcegitcommit: 9ef86dcd4ae3d922b713d585543f1def48fcb645
ms.openlocfilehash: c9833f29d6080509c849e9d624f2bfcd0b0af04c

---

# Prise en charge de la liaison application-site web avec les gestionnaires d’URI d’application

Apprenez à rendre votre application plus attractive aux yeux des utilisateurs grâce à la prise en charge de la liaison application-site web. La liaison application-site web vous permet d’associer une application à un site web. Lorsque les utilisateurs ouvrent un lien http ou https vers votre site web, ils sont redirigés vers votre application, et non vers leur navigateur. Si votre application n’est pas installée, un lien apparaît pour ouvrir votre site web dans le navigateur. Cette expérience est très fiable puisque seuls les propriétaires de contenus vérifiés peuvent s’inscrire pour créer ce type de lien.

Pour activer la liaison application-site web, vous devez:
- identifier les URI gérés par votre application dans le fichier manifeste;
- disposer d’un fichier JSON comportant le nom de la famille de packages de votre application à la même racine hôte que la déclaration de manifeste de l’application;
- gérer l’activation dans l’application.

## S’inscrire pour bénéficier du traitement des liens http et https dans le manifeste de l’application

Votre application a besoin d’identifier les URI pour les sites web qu’elle gère. Pour ce faire, ajoutez l’inscription d’extension **Windows.appUriHandler** au fichier manifeste de votre application **Package.appxmanifest**.

Par exemple, si l’adresse de votre site web est «msn.com», vous devez saisir le code suivant dans le fichier manifeste de votre application:

```xml
<Applications>
    ...
  <Extensions>
     <uap3:Extension Category="windows.appUriHandler">
      <uap3:AppUriHandler>
        <uap3:Host Name="msn.com" />
      </uap3:AppUriHandler>
    </uap3:Extension>
  </Extensions>
    ...
</Applications>
```

La déclaration ci-dessus inscrit votre application afin qu’elle puisse gérer des liens à partir de l’hôte spécifié. Si votre site web comporte plusieurs adresses (par exemple: m.example.com www.example.com et example.com), ajoutez une autre entrée `<uap3:Host Name=... />` dans `<uap3:AppUriHandler>` pour chaque adresse.

## Associer votre application et votre site web à un fichier JSON

Pour faire en sorte que seule votre application puisse ouvrir du contenu sur votre site web, ajoutez le nom de la famille de packages de votre application dans un fichier JSON situé à la racine du serveur web ou dans le répertoire connu du domaine. Cela signifie que votre site web autorise les applications répertoriées à ouvrir du contenu sur votre site. Vous pouvez trouver le nom de la famille de packages dans la section Packages du concepteur de manifestes de l’application.

>[!Important]
> Le fichier JSON ne doit pas contenir le suffixe de fichier .json.

Créez un fichier JSON (sans l’extension de fichier .json) nommé **windows-app-web-link** et indiquez le nom de la famille de packages de votre application. Par exemple:

``` JSON
[{
  "packageFamilyName": "YourAppsPFN",
  "paths": [ "*" ],
  "excludePaths" : [ "/news/*, /blog/*" ]
 }]
```

Windows utilisera une connexion https vers votre site web et recherchera le fichier JSON correspondant sur votre serveur web.

### Caractères génériques

L’exemple de fichier JSON ci-dessus illustre l’utilisation des caractères génériques. Les caractères génériques vous permettent de prendre en charge une grande diversité de liens, avec moins de lignes de code. La liaison application-site web prend en charge deux types de caractères génériques dans le fichier JSON:

| **Caractère générique** | **Description**               |
|--------------|-------------------------------|
| *****       | Représente une sous-chaîne      |
| **?**        | Représente un caractère unique |

Par exemple, avec la ligne `"excludePaths" : [ "/news/*, /blog/*" ]` donnée dans l’exemple ci-dessus, votre application prendra en charge tous les chemins d’accès qui commencent par l’adresse de votre site web (par exemple, msn.com), **sauf** ceux qui se trouvent sous `/news/` et `/blog/`. **msn.com/weather.html** sera donc pris en charge, mais pas ****msn.com/news/topnews.html****.


### Applications multiples

Si vous avez deux applications que vous voulez lier à votre site web, répertoriez les noms de famille de packages des deux applications dans votre fichier JSON **windows-app-web-link**. Les deux applications peuvent être prises en charge. L’utilisateur pourra alors choisir le lien par défaut si les deux sont installées. S’il souhaite par la suite modifier le lien par défaut, il pourra le faire via le menu **Paramètres &gt; Applications pour les sites web**. Les développeurs peuvent également modifier le fichier JSON à tout moment et voir leur modification le jour même, mais pas plus de huit jours après la mise à jour.

``` JSON
[{
  "packageFamilyName": "YourAppsPFN",
  "paths": [ "*" ],
  "excludedPaths" : [ "/news/*, /blog/*" ]
 },
 {
  "packageFamilyName": "Your2ndAppsPFN",
  "paths": [ "/example/*, /links/*" ]
 }]
```

Pour offrir à vos utilisateurs la meilleure expérience possible, utilisez les chemins d’accès exclus pour vous assurer que le contenu uniquement en ligne est exclu des chemins d’accès pris en charge dans votre fichier JSON.

Les chemins d’accès exclus sont vérifiés en premier lieu et, s’il existe une correspondance, la page correspondante s’ouvre avec le navigateur au lieu de l’application désignée. Dans l’exemple ci-dessus, «/news/\*» inclut toutes les pages qui se trouvent sous ce chemin d’accès, tandis que «/news\*» (sans barre oblique après «news») inclut les chemins d’accès sous «news\*», comme «newslocal/», «newsinternational/», etc.

## Gérer les liens à l’activation pour créer un lien vers le contenu

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

## Test: outil de validation locale

Vous pouvez tester la configuration de votre application et de votre site web en exécutant l’outil de vérification de l’inscription de l’hôte de l’application disponible dans:

%windir%\\system32\\**AppHostRegistrationVerifier.exe**

Testez la configuration de votre application et de votre site web en exécutant cet outil avec les paramètres suivants:

**AppHostRegistrationVerifier.exe** *hostname packagefamilyname filepath*

-   Hostname: votre site web (par exemple, microsoft.com)
-   Package Family Name: le nom de la famille de packages (PFN) de votre application
-   File path: le fichier JSON utilisé pour la validation locale (par exemple, C:\\SomeFolder\\windows-app-web-link)

## Test: validation sur le web

Fermez votre application pour vérifier que l’application est activée lorsque vous cliquez sur un lien. Copiez ensuite l’adresse de l’un des chemins d’accès pris en charge dans votre site web. Par exemple, si l’adresse de votre site web est «msn.com» et si l’un des chemins pris en charge est «path1», vous devez utiliser: `http://msn.com/path1`

Vérifiez que votre application est fermée. Appuyez sur la **touche Windows + R** pour ouvrir la boîte de dialogue **Exécuter**, puis collez le lien dans la fenêtre. Votre application doit alors démarrer à la place du navigateur web.

Vous pouvez aussi tester votre application en la démarrant à partir d’une autre application à l’aide de l’API [LaunchUriAsync](https://msdn.microsoft.com/en-us/library/windows/apps/hh701480.aspx). Vous pouvez utiliser cette API pour tester également votre application sur des téléphones.

Si vous souhaitez suivre la logique d’activation du protocole, définissez un point d’arrêt dans le gestionnaire d’événements **OnActivated**.

**Remarque:** si vous cliquez sur un lien dans le navigateur MicrosoftEdge, il ne lancera pas votre application, mais vous redirigera vers votre site web.

## Conseils concernant AppUriHandlers:

- Veillez à spécifier uniquement les liens que votre application peut gérer.

- Répertoriez tous les hôtes que vous allez prendre en charge.  Notez que www.example.com et example.com sont des hôtes différents.

- Les utilisateurs peuvent utiliser le menu Paramètres pour choisir l’application qu’ils préfèrent pour la gestion des sites web.

- Votre fichier JSON doit être téléchargé sur un serveur https.

- Si vous devez modifier les chemins d’accès que vous souhaitez prendre en charge, vous pouvez republier votre fichier JSON sans avoir à republier votre application. Les utilisateurs verront ces modifications dans un délai de 1 à 8jours.

- Toutes les applications chargées de manière indépendante avec AppUriHandlers auront des liens validés pour l’hôte au moment de l’installation. Il est inutile de charger un fichier JSON pour tester la fonctionnalité.

- Cette fonctionnalité fonctionne à chaque fois que votre application est une application UWP lancée avec [LaunchUriAsync](https://msdn.microsoft.com/en-us/library/windows/apps/hh701480.aspx) ou une application de bureau Windows lancée avec [ShellExecuteEx](https://msdn.microsoft.com/en-us/library/windows/desktop/bb762154(v=vs.85).aspx). Si l’URL correspond à un gestionnaire d’URI d’application enregistré, l’application sera lancée en lieu et place du navigateur.

## Voir également

[Inscription de windows.protocol](https://msdn.microsoft.com/en-us/library/windows/apps/br211458.aspx)

[Gérer l’activation des URI](https://msdn.microsoft.com/en-us/windows/uwp/launch-resume/handle-uri-activation)

[L’exemple de lancement d’association](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/AssociationLaunching) explique comment utiliser l’API LaunchUriAsync().



<!--HONumber=Aug16_HO4-->


