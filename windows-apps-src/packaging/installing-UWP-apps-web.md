---
author: laurenhughes
title: "Installer des applicationsUWP à partir d’une page web"
description: "Dans cette section, nous allons examiner les étapes à suivre pour permettre aux utilisateurs d’installer vos applications directement à partir de la page web."
ms.author: lahugh
ms.date: 10/10/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows10, uwp, programme d’installation d’application, appinstaller, charger une version test, ensemble connexe, packages facultatifs"
localizationpriority: medium
ms.openlocfilehash: 00301a61bf4e47659707af883739de57e238c681
ms.sourcegitcommit: 44a24b580feea0f188c7eae36e72e4a4f412802b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/31/2017
---
# <a name="installing-uwp-apps-from-a-web-page"></a>Installer des applicationsUWP à partir d’une page web

En règle générale, une application doit être disponible localement sur un appareil avant de pouvoir être installée avec le Programme d’installation d’application. Pour le scénario web, cela signifie que l’utilisateur doit télécharger le package de l’application à partir du serveur web, après quoi elle peut être installée avec le Programme d’installation d’application. Comme cela est inefficace et gaspille l’espace disque, le Programme d’installation d’application intègre désormais des fonctionnalités intégrées pour simplifier le processus.

Le Programme d’installation d’application permet d’installer une application directement à partir d’un serveur web. Lorsque l’utilisateur clique sur un lien web hébergé d’un package de l’application, le Programme d’installation d’application est automatiquement appelé. L’utilisateur est ensuite dirigé vers l’affichage d’informations sur l’application dans le Programme d’installation d’application. Il est alors à un clic d’interagir directement avec l’application. 

L’installation d’application directe est uniquement disponible dans Windows10FallCreatorsUpdate et versions ultérieures. Les versions précédentes de Windows (en revenant à la Mise à jour anniversaire Windows10) seront prises en charge par l’[expérience d’installation web sur les versions antérieures de Windows10](#web-install-experience). Cette expérience n’est pas aussi fluide que l’installation d’application directe, mais elle fournit des améliorations significatives par rapport à la procédure d’installation d’application existante.
  
> [!NOTE]
> Une version du Programme d’installation d’application supérieure à la version 1.0.12271.0 est requise pour prendre en charge les nouvelles fonctionnalités.

### <a name="protocol-activation-scheme"></a>Schéma d’activation de protocole
Dans ce mécanisme, le Programme d’installation d’application inscrit un schéma d’activation de protocole auprès du système d’exploitation. Lorsque l’utilisateur clique sur un lien web, le navigateur vérifie auprès du système d’exploitation les applications qui sont inscrites pour ce lien web. Si le schéma correspond au schéma d’activation de protocole spécifié par le Programme d’installation d’application, alors ce dernier est appelé. Il est important de noter que ce mécanisme est indépendant du navigateur. Cela est utile, par exemple, pour les administrateurs de site qui n’ont pas besoin de prendre en compte les différences entre les navigateurs web lors de l’intégration dans une page web. 

#### <a name="requirements-for-protocol-activation-scheme"></a>Conditions requises pour le schéma d’activation de protocole
   - Les serveurs web qui prennent en charge les requêtes de plages d'octets (HTTP/1.1)
   - Les packages de l’application doivent être hébergés sur des serveurs qui prennent en charge le protocole HTTP/1.1   

#### <a name="how-to-enable-this-on-a-webpage"></a>Comment activer ce mécanisme sur une page web 
Les développeurs d’application qui souhaitent héberger des packages de l’application sur leurs sites Web doivent suivre cette étape:

Préfixer les URI du package de l’application avec le schéma d’activation `'ms-appinstaller:?source='` pour lequel le Programme d’installation d’application est inscrit lors de leur référencement sur votre leur web. Consultez l’exemple pour **MyApp Web Page** pour plus d’informations. 
``` html
<html>
    <body>
        <h1> MyApp Web Page </h1>
        <a href="ms-appinstaller:?source=http://mywebservice.azureedge.net/HubApp.appx"> Install app package </a>
        <a href="ms-appinstaller:?source=http://mywebservice.azureedge.net/HubAppBundle.appxbundle"> Install app bundle  </a>
        <a href="ms-appinstaller:?source=http://mywebservice.azureedge.net/HubAppSet.appinstaller"> Install related set </a>
    </body>
</html>
```

## Expérience d’installation web sur des versions antérieures de Windows10<a name="web-install-experience"></a>

L’appel du Programme d’installation d’application à partir du navigateur est pris en charge sur toutes les versions de Windows10 sur lesquelles le Programme d’installation d’application est disponible (à partir de la Mise à jour anniversaire). Toutefois, la fonctionnalité pour installer directement à partir du web sans avoir besoin de télécharger d'abord le package est uniquement disponible sur Windows10FallCreatorsUpdate.  

Les utilisateurs des versions précédentes de Windows10 (avec le Programme d’installation d’application disponible) peuvent également tirer parti de l’installation web des applicationsUWP via le Programme d’installation d’application, mais bénéficieront d’une expérience utilisateur différente. Lorsque ces utilisateurs cliqueront sur le lien web, le Programme d’installation d’application les invitera à **télécharger** le package au lieu de l'**installer**. Après le téléchargement, le Programme d’installation d’application lancera automatiquement le lancement du package téléchargé. Étant donné que le package de l’application est téléchargé à partir du web, ces fichiers passeront par le biais de MicrosoftSmartScreen pour une vérification de sécurité. Une fois que l’utilisateur donne l’autorisation de continuer, puis clique de nouveau sur **Installer**, l’application est prête à être utilisée!

Bien que ce flux ne soit pas aussi fluide que l’installation directe sur Windows10FallCreatorsUpdate, les utilisateurs peuvent toujours rapidement interagir avec l’application. En outre, avec ce flux, l’utilisateur n’a pas à se soucier du fait que les fichiers du package de l’application occupent inutilement de l’espace sur les lecteurs. Le Programme d’installation d’application gère efficacement l’espace en téléchargeant le package vers son dossier de données d’application et en supprimant les packages lorsqu’ils ne sont plus nécessaires. 

Voici une comparaison rapide de la version Windows10FallCreatorUpdate et de la version précédente du Programme d’installation d’application:

| Programme d’installation d’application, dernière version | Programme d’installation d’application, version précédente |
|------------------------------|----------------------------------|
| Le Programme d’installation d’application affiche des informations sur l’application avant de démarrer le téléchargement | Le navigateur invite l’utilisateur à effectuer le téléchargement  |
| Le Programme d’installation d’application effectue le téléchargement | L’utilisateur doit lancer manuellement le lancement du package de l’application |
| Après le téléchargement du package, le Programme d’installation d’application lance automatiquement le package de l’application | L'utilisateur doit cliquer sur **Installer** et lancer manuellement le package de l’application |
| Le Programme d’installation d’application s’occupe de la suppression des packages téléchargés | L'utilisateur doit supprimer manuellement les fichiers téléchargés |

## <a name="web-install-experience-on-previous-versions-of-windows-10"></a>Expérience d’installation web sur des versions antérieures de Windows10
Sur les versions antérieures à Windows10FallCreatorsUpdate, le Programme d’installation d’application ne peut pas installer directement une application à partir du web. Sur ces versions, le Programme d’installation d’application ne peut installer que les packages de l’application qui sont disponibles localement. Au lieu de cela, le Programme d’installation d’application téléchargera le package et exigera de l’utilisateur qu'il double-clique sur le package téléchargé pour l’installer.


## <a name="microsoft-smartscreen-integration"></a>Intégration de MicrosoftSmartScreen

MicrosoftSmartScreen a toujours fait partie du processus d’installation des applications via le Programme d’installation d’application. SmartScreen garantit que les utilisateurs sont à l'abri de tout mécontentement sur leurs appareils. Avec la dernière mise à jour du Programme d’installation d’application, l’intégration de SmartScreen est plus transparente et fiable, et des avertissements sont générés lorsque des applications inconnues sont installées, afin de protéger les appareils contre tout dommage. 