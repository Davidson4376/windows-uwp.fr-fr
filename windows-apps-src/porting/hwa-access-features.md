---
author: seksenov
title: "Apps web hébergées- Accès aux fonctionnalitésUWP et aux API Runtime"
description: "Accédez aux fonctionnalités natives de la plateforme Windows universelle (UWP) et aux API Windows10 Runtime, notamment aux commandes vocales de Cortana, aux vignettes dynamiques, aux règles ACUR pour la sécurité, à OpenID et à OAuth, le tout à partir du code JavaScript distant."
kw: Hosted Web Apps, Accessing Windows 10 features from remote JavaScript, Building a Win10 Web Application, Windows JavaScript Apps, Microsoft Web Apps, HTML5 app for PC, ACUR URI Rules for Windows App, Call Live Tiles with web app, Use Cortana with web app, Access Cortana from website, msapplication-cortanavcd
translationtype: Human Translation
ms.sourcegitcommit: 3de603aec1dd4d4e716acbbb3daa52a306dfa403
ms.openlocfilehash: a7f7dccb9c7461e482bd43c8f370a2a7244eb735

---

# Accès aux fonctionnalités de la plateforme Windows universelle (UWP)

Votre application web hébergée peut avoir un accès complet à la plateforme Windows universelle (UWP), notamment l’activation des fonctionnalités natives sur les appareils Windows, [la mise à profit de la sécurité Windows](#keep-your-app-secure-setting-application-content-uri-rules-acurs) et [l’appel des API Windows Runtime](#call-windows-runtime-apis) directement à partir du script hébergé sur un serveur, tirant parti de [l’intégration de Cortana](#integrate-cortana-voice-commands) et utilisant un [fournisseur d’authentification en ligne](#web-authentication-broker). Les [applications hybrides](#create-hybrid-apps-packaged-web-apps-vs-hosted-web-apps) sont également prises en charge pour que vous puissiez inclure du code local pour être appelé à partir du script hébergé et gérer la navigation entre les pages locales et distantes dans l’application.

## Préserver la sécurité de votre application – Définir les règles ACUR

Par le biais des règles ACUR, également appelées liste des URL autorisées, vous pouvez accorder à des URL distantes un accès direct aux API Windows universelles depuis du code distant HTML, CSS et JavaScript. Au niveau du système d’exploitation Windows, des limites de stratégie appropriées ont été définies pour autoriser le code hébergé sur votre serveur web à appeler directement les API de plateforme. Vous définissez ces limites dans le manifeste du package de l’application lorsque vous placez l’ensemble des URL qui constituent votre application web hébergée dans les règles ACUR. Vos règles doivent inclure la page d’accueil de votre application et toutes les autres pages que vous souhaitez inclure en tant que pages de l’application. Vous pouvez éventuellement exclure les URL trop spécifiques.

Il existe plusieurs façons de spécifier une correspondance URL dans vos règles :

- Nom d’hôte exact
- Nom d’hôte sur la base duquel inclure ou exclure tout URI contenant un sous-domaine de ce nom d’hôte.
- URI exact
- URI exact qui peut contenir une propriété de requête
- Chemin d’accès partiel et caractère générique permettant d’indiquer une extension de fichier particulière pour une règle d’inclusion.
- Chemins d’accès relatifs pour les règles d’exclusion

Si l’utilisateur navigue vers une URL qui n’est pas incluse dans vos règles, Windows ouvre l’URL cible dans un navigateur.

Voici quelques exemples de règles ACUR.

```HTML
<Application
Id="App"
StartPage="http://contoso.com/home">
<uap:ApplicationContentUriRules>
    <uap:Rule Type="include" Match="https://contoso.com/" WindowsRuntimeAccess="all" />
    <uap:Rule Type="include" Match="https://*.contoso.com/" WindowsRuntimeAccess="all" />
    <uap:Rule Type="exclude" Match="https://contoso.com/excludethispage.aspx" />
</uap:ApplicationContentUriRules>
```

## Appeler des API Windows Runtime

Si une URL est définie dans les limites de l’application (règles ACUR), elle peut appeler des API Windows Runtime en JavaScript à l’aide de l’attribut «WindowsRuntimeAccess». L’espace de noms Windows sera injecté et présent dans le moteur de script lorsqu’une URL avec un accès approprié sera chargée dans l’hôte de l’application. Ainsi, les API Windows universelles sont disponibles pour des appels directs des scripts de l’application. En tant que développeur, vous devez simplement intégrer une fonctionnalité de détection de l’API Windows que vous souhaitez appeler et, le cas échéant, mettre en évidence les fonctionnalités de la plateforme.

Pour ce faire, vous devez spécifier l’attribut `(WindowsRuntimeAccess="<<level>>")` dans les règles ACUR avec l’une de valeurs suivantes:

- **all**: le code JavaScript distant dispose d’un accès à toutes les API UWP et à tous les composants empaquetés locaux.
- **allowForWeb**: le code JavaScript distant dispose d’un accès personnalisé au code du package uniquement. Accès local aux composants C++/C# personnalisés
- **none**: par défaut. L’URL spécifiée n’a pas d’accès à la plateforme.

Voici un exemple de type de règle:

```HTML
<uap:ApplicationContentUriRules>
    <uap:Rule Type="include" Match="http://contoso.com/" WindowsRuntimeAccess="all"  />
</uap:ApplicationContentUriRules>
```

Cela permet au script qui s’exécute sur http://contoso.com/ d’accéder aux espaces de noms Windows Runtime et aux composants packagés personnalisés du package. Voir l’exemple [Windows.UI.Notifications.js](https://gist.github.com/Gr8Gatsby/3d471150e5b317eb1813#file-windows-ui-notifications-js) sur GitHub pour les notifications de toast.

Voici un exemple qui montre comment mettre en œuvre une vignette dynamique et la mettre à jour à partir de code JavaScript distant:

```Javascript
function updateTile(message, imgUrl, imgAlt) {
    // Namespace: Windows.UI.Notifications

    if (typeof Windows !== 'undefined'&&
            typeof Windows.UI !== 'undefined' &&
            typeof Windows.UI.Notifications !== 'undefined') {  
        var notifications = Windows.UI.Notifications,
        tile = notifications.TileTemplateType.tileSquare150x150PeekImageAndText01,
        tileContent = notifications.TileUpdateManager.getTemplateContent(tile),
        tileText = tileContent.getElementsByTagName('text'),
        tileImage = tileContent.getElementsByTagName('image');  
        tileText[0].appendChild(tileContent.createTextNode(message || 'Demo Message'));
        tileImage[0].setAttribute('src', imgUrl || 'https://unsplash.it/150/150/?random');
        tileImage[0].setAttribute('alt', imgAlt || 'Random demo image');    
        var tileNotification = new notifications.TileNotification(tileContent);
        var currentTime = new Date();
        tileNotification.expirationTime = new Date(currentTime.getTime() + 600 * 1000);
        notifications.TileUpdateManager.createTileUpdaterForApplication().update(tileNotification);
    } else {
        //Alternative behavior

    }
}
```

Ce code génère une vignette qui ressemble à ce qui suit:

![Windows10 appelant une vignette dynamique](images/hwa-to-uwp/hwa_livetile.png)

Appelez les API Windows Runtime avec l’environnement ou la technique que vous connaissez le mieux en conservant vos ressources sur une fonctionnalité serveur détectant les fonctionnalités de Windows avant de les appeler. Si les fonctionnalités de plateforme ne sont pas disponibles, et si l’application web s’exécute dans un autre hôte, vous pouvez fournir à l’utilisateur une expérience par défaut standard qui fonctionne dans le navigateur.

## Intégrer des commandes vocales Cortana

Vous pouvez tirer parti de l’intégration de Cortana en spécifiant un fichier de définition des commandes voix (VCD) sur votre page html. Le fichier VCD est un fichier xml qui mappe des commandes à des expressions spécifiques. Parexemple, un utilisateur peut appuyer sur le bouton Démarrer et dire «Contoso Books, afficher les meilleures ventes» pour à la fois lancer une application appelée Contoso Books et accéder à une page des meilleures ventes dans l’application.

Lorsque vous ajoutez une balise d’élément `<meta>` qui répertorie l’emplacement de votre fichier VCD, Windows télécharge automatiquement et enregistre le fichier de définition des commandes voix.

Voici un exemple d’utilisation de la balise sur une page html dans une application web hébergée:

```HTML
<meta name="msapplication-cortanavcd" content="http:// contoso.com/vcd.xml"/>
```

Pour plus d’informations sur l’intégration de Cortana et les fichiers VCD, voir Interactions avec Cortana et Voice Command Defintion (VCD) elements and attributes v1.2.

## Créer des applications hybrides – Comparaison entre applications web empaquetées et applications web hébergées

Plusieurs options s’offrent à vous pour créer votre application UWP. L’application peut être conçue pour être téléchargée à partir du Windows Store et entièrement hébergée sur le client local; ce type d’application est souvent appelé **Application web empaquetée**. Cela vous permet d’exécuter votre application en mode hors connexion sur n’importe quelle plateforme compatible. L’application peut également être une application web entièrement hébergée qui s’exécute sur un serveur web distant ; ce type d’application est généralement appelé **Application web hébergée**. Mais il existe une troisième option: l’application peut être hébergée partiellement sur le client local et partiellement sur un serveur web distant. Ce type d’application est appelé **Application hybride** et il utilise généralement le composant **WebView** pour donner au contenu distant l’apparence de contenu local. Les applications hybrides peuvent inclure votre code HTML5, CSS et JavaScript exécuté sous forme de package à l’intérieur du client d’application local et conserver la possibilité d’interagir avec du contenu distant.

## Service Broker d’authentification web

Vous pouvez utiliser le service Broker d’authentification web pour gérer le flux de connexion de vos utilisateurs, si vous disposez d’un fournisseur d’identité en ligne qui utilise les protocoles d’authentification et d’autorisation Internet comme OpenID ou OAuth. Spécifiez les URI de début et de fin dans une balise `<meta>` sur une page html dans votre application. Windows détecte ces URI et les transmet au service Broker d’authentification web pour réaliser le flux de connexion. L’URI de début se compose de l’adresse à laquelle vous envoyez la demande d’authentification à votre fournisseur en ligne, complétée par d’autres informations requises, comme un ID d’application ou un URI de redirection où l’utilisateur est envoyé après avoir effectué l’authentification, ainsi que le type de réponse attendu. Vous pouvez déterminer auprès de votre fournisseur les paramètres requis. Voici un exemple d’utilisation de la balise `<meta>`:

```HTML
<meta name="ms-webauth-uris" content="https://<providerstartpoint>?client_id=<clientid>&response_type=token, https://<appendpoint>"/>
```

Pour plus d’informations, voir [Considérations relatives au service Broker d’authentification web pour les fournisseurs en ligne](https://msdn.microsoft.com/library/windows/apps/dn448956.aspx).

## Déclarations des fonctionnalités d’application

Si votre application a besoin d’un accès par programme à des ressources utilisateur telles que des images ou de la musique, ou à des appareils tels qu’une caméra ou un microphone, vous devez déclarer la fonctionnalité appropriée. Il existe trois catégories de déclaration de fonctionnalités d’application: 

- Les [fonctionnalités à usage général](https://msdn.microsoft.com/library/windows/apps/Mt270968.aspx#General-use_capabilities) qui s’appliquent aux scénarios d’application les plus courants. 
- Les [fonctionnalités d’appareil](https://msdn.microsoft.com/library/windows/apps/Mt270968.aspx#Device_capabilities) qui permettent à votre application d’accéder à des périphériques et à des dispositifs internes. 
- Les [fonctionnalités à usage spécial](https://msdn.microsoft.com/library/windows/apps/Mt270968.aspx#Special_and_restricted_capabilities) qui nécessitent un compte d’entreprise spécial pour les soumettre au Windows Store et les utiliser. 

Pour plus d’informations sur les comptes d’entreprise, voir [Types de compte, emplacements et frais](https://msdn.microsoft.com/library/windows/apps/jj863494.aspx).

> [!NOTE]
> Il est important de savoir que lorsque des clients acquièrent votre application dans le Windows Store, toutes les fonctionnalités déclarées par l’application leur sont notifiées. Par conséquent, n’utilisez pas de fonctionnalités dont votre application n’a pas besoin.

Vous pouvez demander l’accès en déclarant les fonctionnalités dans le [manifeste de package](https://msdn.microsoft.com/library/windows/apps/br211474.aspx) de votre application. Vous pouvez déclarer les fonctionnalités générales à l’aide du [concepteur de manifeste](https://msdn.microsoft.com/library/windows/apps/xaml/hh454036(v=vs.140).aspx#Configure) dans Microsoft Visual Studio ou vous pouvez les ajouter manuellement, voir [Comment spécifier des fonctionnalités dans un manifeste de package](https://msdn.microsoft.com/library/windows/apps/br211477.aspx).

Certaines fonctionnalités permettent aux applications d’accéder à des ressources sensibles. Ces ressources sont considérées comme «sensibles» parce qu’elles ont accès aux données personnelles de l’utilisateur ou ont un coût pour celui-ci. Les paramètres de confidentialité, gérés par l’application Paramètres, permettent à l’utilisateur de contrôler de façon dynamique l’accès aux ressources sensibles. Il est donc important que votre application ne présume pas de la disponibilité d’une ressource sensible. Pour plus d’informations sur l’accès aux ressources sensibles, voir [Recommandations en matière d’applications prenant en charge la confidentialité](https://msdn.microsoft.com/library/windows/apps/hh768223.aspx).

## manifoldjs et manifeste de l’application

Un moyen simple de transformer votre site web en application UWP consiste à utiliser un **manifeste de l’application** et **manifoldjs**. Le manifeste de l’application est un fichier xml qui contient les métadonnées relatives à l’application. Il spécifie des éléments tels que le nom de l’application, les liens vers les ressources, le mode d’affichage, les URL et d’autres données qui décrivent comment l’application doit être déployée et exécutée. manifoldjs facilite considérablement ce processus, même sur des systèmes qui ne prennent pas en charge les applications web. Pour plus d’informations sur le fonctionnement de cet outil, accédez au site [manifoldjs.com](http://www.manifoldjs.com/). Vous pouvez également consulter une démonstration de manifoldjs dans le cadre de cette [présentation des applications web Windows10](http://channel9.msdn.com/Events/WebPlatformSummit/2015/Hosted-web-apps-and-web-platform-innovations?wt.mc_id=relatedsession).

## Rubriques connexes
- [API Windows Runtime: exemples de code JavaScript](http://rjs.azurewebsites.net/)
- [Codepen: sandbox à utiliser pour appeler les API Windows Runtime](http://codepen.io/seksenov/pen/wBbVyb/)
- [Interactions avec Cortana](https://msdn.microsoft.com/library/windows/apps/dn974231.aspx)
- [Éléments et attributs d’un fichierVCDv1.2](https://msdn.microsoft.com/library/windows/apps/dn954977.aspx)
- [Considérations relatives au service Broker d’authentification web pour les fournisseurs en ligne](https://msdn.microsoft.com/library/windows/apps/dn448956.aspx)
- [Déclarations des fonctionnalités d’application](https://msdn.microsoft.com/ibrary/windows/apps/hh464936.aspx)


<!--HONumber=Jul16_HO2-->


