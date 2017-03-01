---
author: seksenov
title: "Apps web hébergées – Convertir votre application Chrome en app UWP"
description: "Convertissez votre application Chrome ou votre extension Chrome en application de plateforme Windows universelle (UWP) pour le Windows Store."
kw: Package Chrome Extension for Windows Store tutorial, Port Chrome Extension to Windows 10, How to convert Chrome App to Windows, How to add Chrome Extension to Windows Store, hwa-cli, Hosted Web Apps Command Line Interface CLI Tool, Install Chrome Extension on Windows 10 Device, convert .crx to .AppX
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: extensions Chrome pour Windows, applications Chrome pour Windows, hwa-cli, convertir .crx en .AppX
ms.assetid: 04f37333-48ba-441b-875e-246fbc3e1a4d
translationtype: Human Translation
ms.sourcegitcommit: 5645eee3dc2ef67b5263b08800b0f96eb8a0a7da
ms.openlocfilehash: 84d8875cc7b1c8540f54fec78cd675bd96919fd2
ms.lasthandoff: 02/08/2017

---

# <a name="convert-your-existing-chrome-app-to-a-universal-windows-platform-app"></a>Convertir votre application Chrome existante en application de plateforme Windows universelle

Nous avons simplifié la conversion de votre application hébergée Chrome existante en application exécutée sur la plateforme Windows universelle (UWP). Deux moyens s’offrent à vous pour convertir votre application Chrome :

- Option 1 : [ManifoldJS](http://manifoldjs.com/) accepte désormais les manifestes Chrome comme type d’entrée. 

- Option 2 : nous avons développé un [outil d’interface de ligne de commande](https://github.com/MicrosoftEdge/hwa-cli) qui génère un package `.appx` à partir de vos fichiers `.zip` ou `.crx` existants.

## <a name="convert-your-existing-chrome-app-using-the-command-line-interface"></a>Convertir votre application Chrome existante à l’aide de l’interface de ligne de commande

1. Installez [NodeJS](https://nodejs.org/en/) et son gestionnaire de packages [NPM](https://www.npmjs.com/). 


2. Ouvrir une fenêtre d’invite de commandes dans le répertoire de votre choix


3. Installez l’interface de ligne de commande des applications web hébergées en entrant la commande suivante dans votre ligne de commande : `npm i -g hwa-cli`

4. Convertissez votre package Chrome (`.crx` et `.zip` sont les formats de package pris en charge), en entrant la commande suivante dans votre ligne de commande : `hwa convert path/to/chrome/app.crx`, ou `hwa convert path/to/chrome/app.zip`

    **Remplacez `path/to/chrome/app` par les informations du chemin menant à votre application Chrome.*
    
5. L’élément `.appx` généré apparaîtra dans le même dossier que votre package Chrome. Vous êtes désormais prêt à charger votre application dans le Windows Store. 

## <a name="uploading-your-app-to-the-windows-store"></a>Chargement de votre application dans le Windows Store

Pour charger votre application, consultez le tableau de bord dans le [Centre de développement Windows](https://developer.microsoft.com/windows). Cliquez sur « [Créer une application](https://developer.microsoft.com/dashboard/Application/New) » et réservez le nom de votre application.
![Réservation d’un nom dans le tableau de bord du Centre de développement Windows](images/hwa-to-uwp/reserve_a_name.png)


Chargez votre package `AppX` en accédant à la page « Packages » dans la section Soumissions.

Renseignez les invites du Windows Store.

    During the conversion process, you will be prompted for an Identity Name, Publisher Identity, and Publisher Display Name. To retrieve these values, visit the Dashboard in the [Windows Dev Center](https://developer.microsoft.com/windows).
    - Click on "[Create a new app](https://developer.microsoft.com/dashboard/Application/New)" and reserve your app name.
![Réservation d’un nom dans le tableau de bord du Centre de développement Windows](images/hwa-to-uwp/reserve_a_name.png)
    - Ensuite, cliquez sur « Identité de l’application » dans le menu de gauche sous la section « Gestion des applications ».
    ![Identité de l’application dans le tableau de bord du Centre de développement Windows](images/hwa-to-uwp/app_identity.png)
    - La page devrait afficher trois valeurs que vous êtes invité à renseigner : 
        1. Nom de l’identité : `Package/Identity/Name`
        2. Identité de l’éditeur : `Package/Identity/Publisher`
        3. Nom complet de l’éditeur : `Package/Properties/PublisherDisplayName`


## <a name="guide-for-migrating-your-hosted-web-app"></a>Guide pour la migration de votre application web hébergée

Après avoir créé le package de votre application web pour le Windows Store, personnalisez-le pour qu’il fonctionne de manière optimale sur tous les appareils Windows, notamment les PC, tablettes, téléphones, HoloLens, Surface Hub, Xbox et Raspberry Pi.

### <a name="application-content-uri-rules"></a>Règles URI de contenu de l’application

Les [règles URI de contenu de l’application (ACUR)](./hwa-access-features.md) ou les URI de contenu définissent la portée de votre application web hébergée par le biais d’une liste d’URL autorisées dans le manifeste du package de votre application. Afin de contrôler les communications vers et depuis du contenu distant, vous devez définir les URL incluses dans et/ou exclues de cette liste. Si un utilisateur clique sur une URL qui n’est pas explicitement incluse, Windows ouvre le chemin d’accès cible dans le navigateur par défaut. Avec les règles ACUR, vous êtes en mesure d’accorder un accès URL aux [API Windows universelles](https://msdn.microsoft.com/library/windows/apps/br211377.aspx).

Vos règles doivent, au minimum, inclure la page de démarrage de votre application. L’outil de conversion créera automatiquement un ensemble de règles ACUR pour vous, en fonction de votre page de démarrage et de son domaine. Toutefois, en cas de redirection par programme, que ce soit sur le serveur ou sur le client, ces destinations devront être ajoutées à la liste autorisée.

*Remarque : les règles ACUR s’appliquent uniquement à la navigation entre les pages. Les images, bibliothèques JavaScript et autres ressources similaires ne sont pas affectées par ces restrictions.*

De nombreuses applications utilisent des sites tiers pour leurs flux de connexion, par exemple, Facebook et Google. L’outil de conversion créera automatiquement un ensemble de règles ACUR pour vous, en fonction des sites les plus populaires. Si votre méthode d’authentification n’est pas incluse dans cette liste, et s’il s’agit d’un flux de redirection, vous devrez ajouter son ou ses chemins d’accès sous forme de règles ACUR. Vous pouvez également envisager d’utiliser un [service Broker d’authentification web](./hwa-access-features.md).

### <a name="flash"></a>Flash

La technologie Flash n’est pas autorisée dans les applications Windows 10. Vous devrez vous assurer que son absence n’affecte pas l’expérience d’utilisation de votre application.

Pour les publicités, assurez-vous que votre fournisseur de publicités intègre une option HTML5. Vous pouvez consulter [Bing Ads](https://bingads.microsoft.com/) et [Publicités dans les applications](http://adsinapps.microsoft.com/).

Les vidéos YouTube devraient encore fonctionner, car elles utilisent désormais [par défaut HTML5 `<video>`,](http://youtube-eng.blogspot.com/2015/01/youtube-now-defaults-to-html5_27.html) pourvu que vous utilisiez la [`<iframe>` méthode intégrée](https://developers.google.com/youtube/iframe_api_reference). Si votre application utilise toujours l’API Flash, vous devrez basculer vers le style d’intégration mentionné précédemment.

### <a name="image-assets"></a>Ressources d’image

Le Chrome Web Store [exige déjà une image d’icône d’application au format 128 x 128](https://developer.chrome.com/webstore/images) dans le package de votre application. Pour les applications Windows 10, vous devez, au minimum, fournir des images d’icônes d’application au format 44 x 44, 50 x 50, 150 x 150 et 600 x 350. L’outil de conversion créera automatiquement ces images pour vous, sur la base de l’image au format 128 x 128. Pour une expérience plus riche et plus fluide, nous vous recommandons vivement de créer vos propres fichiers image. Voici des [recommandations en matière de ressources de vignette et d’icône](https://msdn.microsoft.com/library/windows/apps/mt412102.aspx).

### <a name="capabilities"></a>Fonctionnalités

Les fonctionnalités de l’application doivent être [déclarées](https://msdn.microsoft.com/windows/uwp/packaging/app-capability-declarations) dans le manifeste de votre package afin d’accéder à certaines API et ressources. L’outil de conversion permet d’activer automatiquement pour vous trois fonctionnalités populaires sur votre appareil : la localisation, le microphone et la webcam. Avec l’ancien outil, le système demandait systématiquement l’autorisation de l’utilisateur avant d’accorder l’accès.

*Remarque : les utilisateurs sont informés de toutes les fonctionnalités que déclare une application. Nous vous recommandons de supprimer les fonctionnalités dont votre application n’a pas besoin.*

### <a name="file-downloads"></a>Téléchargements de fichiers

Actuellement, les téléchargements classiques de fichiers, comme vous le voyez dans le navigateur, ne sont pas pris en charge.

### <a name="chrome-platform-apis"></a>API de la plateforme Chrome

Chrome fournit des applications avec des [API à usage spécifique](https://developer.chrome.com/apps/api_index) qui peuvent être exécutées en tant que script en arrière-plan. Elles ne sont pas prises en charge. Vous pouvez trouver des fonctionnalités équivalentes et bien plus encore, avec les [API Windows Runtime](https://msdn.microsoft.com/library/windows/apps/br211377.aspx).

## <a name="related-topics"></a>Rubriques connexes

- [Améliorer votre application web en accédant aux fonctionnalités de plateforme Windows universelle (UWP)](./hwa-access-features.md)
- [Guide des applications de plateforme Windows universelle (UWP)](http://go.microsoft.com/fwlink/p/?LinkID=397871)
- [Télécharger des ressources de conception pour les applications du Windows Store](https://msdn.microsoft.com/library/windows/apps/xaml/bg125377.aspx)

