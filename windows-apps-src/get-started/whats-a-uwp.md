---
author: GrantMeStrength
ms.assetid: C9787269-B54F-4FFA-A884-D4A3BF28F80D
title: "Qu’est-ce qu’une application de plateforme Windows universelle (UWP) ?"
description: "Découvrez les différents types d’application que recouvre l’appellation « applications Windows universelles » : applications du Windows Store, applications du Windows Phone Store et applications Windows Runtime."
ms.author: jken
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp"
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 0507086a7c7ea67f43f61feef6dc5c8de5afe2e5
ms.lasthandoff: 02/07/2017

---

# <a name="whats-a-universal-windows-platform-uwp-app"></a>Qu’est-ce qu’une application de plateforme Windows universelle (UWP) ?

Si vous découvrez la plateforme Windows ou que vous venez de l’univers de .NET, Windows Forms ou Silverlight, vous vous posez peut-être la question suivante : qu’*est-ce* au juste qu’une application UWP ? 

Comme dans une réplique d'un livre célèbre : « Ne paniquez pas ! », car bientôt, tout sera plus clair. 

Une application de plateforme Windows universelle est une expérience Windows qui repose sur la plateforme Windows universelle (UWP), introduite pour la première fois dans Windows 8 en tant que Windows Runtime. Au cœur des applications UWP est l’idée que les utilisateurs souhaitent que leurs *expériences* soient mobiles sur TOUS leurs appareils, et qu’ils puissent utiliser l’appareil le plus adapté ou le plus productif pour la tâche qu’ils ont à effectuer.

Windows 10 facilite le développement d’applications pour UWP avec un seul ensemble d’API, un seul package d’application et un seul Store pour atteindre tous les appareils Windows 10 : PC, tablettes, téléphones, Xbox, HoloLens, Surface Hub, etc. Il est plus facile de prendre en charge plusieurs tailles d’écran et plusieurs modèles d’interaction, qu’il s’agisse d’impulsions tactiles, de la souris et du clavier, d’une manette de jeu ou d’un stylet. De plus, vous n’êtes pas obligé d’utiliser C# et XAML, si vous ne voulez pas. Vous aimez développer dans Unity ou MonoGame ? Vous préférez JavaScript ? Pas de problème, utilisez tout ce que vous voulez.

Résultat : vous pouvez passer votre temps à utiliser les langages de programmation, les infrastructures et les API que vous maîtrisez le mieux, le tout dans un même projet, et aboutir à l’exécution d’un même code sur les divers types d’appareils Windows qui existent aujourd’hui. Une fois que vous avez écrit votre application UWP, vous pouvez ensuite la publier sur le Windows store pour que tout le monde puisse la voir.

![Appareils avec Windows](images/1894834-hig-device-primer-01-500.png)

##<a name="so-what-exactly-is-a-uwp-app"></a>Donc, qu’est-ce *exactement* qu’une application UWP ?


Qu’est-ce qui différencie une application UWP ? Voici quelques-unes des caractéristiques qui rendent différentes les applications UWP sur Windows 10.

-   Vous ciblez des familles d’appareils, pas un système d’exploitation.

    Une famille d’appareils identifie les API, les caractéristiques système et les comportements que vous pouvez attendre sur les différents appareils de cette famille. Elle détermine également l’ensemble des appareils sur lesquels votre application peut être installée à partir du Store.

-   Les applications sont empaquetées et distribuées à l’aide du format d’empaquetage .AppX.

    Toutes les applications UWP sont distribuées en tant que package AppX. Cela fournit un mécanisme d’installation digne de confiance et garantit la possibilité de déployer et de mettre à jour vos applications de façon transparente.

-   Il existe un seul magasin pour tous les appareils.

    Après vous être inscrit en tant que développeur d’applications, vous pouvez soumettre votre application au Store et la rendre disponible sur toutes les familles d’appareils, ou uniquement sur celles que vous choisissez. Vous soumettez et gérez toutes vos applications pour appareils Windows au même endroit.

-   Il existe une surface d’API commune à toutes les familles d’appareils.

    Les API au cœur de la plateforme Windows universelle (UWP) sont les mêmes pour toutes les familles d’appareils Windows. Si votre application utilise uniquement les API principales, elle s’exécute sur tout appareil Windows 10.

-   Des Kits de développement logiciels (SDK) d’extension permettent à votre application de fonctionner sur des appareils spécialisés.

    Les Kits de développement logiciels (SDK) d’extension ajoutent des API spécialisées pour chaque famille d’appareils. Si votre application est destinée à une famille d’appareils particulière, vous pouvez la faire fonctionner à l’aide de ces API. Vous pouvez encore avoir un package d’application qui s’exécute sur tous les appareils en vérifiant la famille d’appareils sur laquelle votre application s’exécute avant d’appeler une API d’extension.

-   Contrôles adaptatifs et entrée

    Les éléments d’interface utilisateur utilisent des *pixels effectifs* (voir[Conception réactive 101 pour les applications UWP](https://msdn.microsoft.com/library/windows/apps/Dn958435)), de sorte qu’ils s’adaptent automatiquement en fonction du nombre de pixels d’écran disponibles sur l’appareil. Ils fonctionnent bien avec plusieurs types d’entrées, par exemple, le clavier, la souris, les impulsions tactiles, le stylet et les manettes Xbox One. Si vous avez besoin d’adapter davantage votre interface utilisateur à une taille d’écran ou à un appareil spécifiques, de nouveaux panneaux de disposition et outils vous aident à adapter votre interface utilisateur aux appareils sur lesquels votre application s’exécute.

Pour une présentation plus détaillée de l’UWP, voir le [Guide des applications pour la plateforme Windows universelle](universal-application-platform-guide.md).

## <a name="use-a-language-you-already-know"></a>Utilisez un langage que vous connaissez déjà


Vous pouvez créer des applications UWP en utilisant les langages de programmation que vous connaissez, tels que C# ou Visual Basic avec XAML, JavaScript avec HTML, ou C++ avec DirectX et/ou XAML (Extensible Application Markup Language). Vous pouvez même écrire des composants dans un langage et les utiliser dans une application écrite dans un autre langage.

Les applications UWP peuvent utiliser Windows Runtime, API native intégrée au système d’exploitation. Cette API est implémentée en C++, et prise en charge en C#, Visual Basic, C++ et JavaScript d’une façon qui semble naturelle pour chaque langage.

Microsoft Visual Studio 2015 fournit un modèle d’application UWP pour chaque langage, qui vous permet de créer un projet unique pour tous les appareils. Une fois votre travail terminé, vous pouvez produire un package d’application et le soumettre au Windows Store à partir de Visual Studio afin que votre application atteigne des clients sur tout appareil Windows 10.

## <a name="uwp-apps-come-to-life-on-windows"></a>Les applications UWP prennent vie sur Windows


Sur Windows, votre application peut fournir des informations pertinentes en temps réel à vos utilisateurs et les inciter à en redemander. Dans l’économie numérique moderne, votre application doit être attrayante pour que vos utilisateurs s’en souviennent. Windows met à votre disposition de nombreuses ressources permettant de maintenir l’attention des utilisateurs de votre application :

-   Les vignettes dynamiques et l’écran de verrouillage permettent de voir d’un seul coup d’œil des informations fournies au moment opportun et adaptées au contexte.
-   Les notifications Push permettent d’alerter vos utilisateurs en temps réel quand c’est nécessaire.

-   Le centre de maintenance vous permet d’organiser et d’afficher des notifications et contenus sur lesquels les utilisateur doivent agir.

-   L’exécution en arrière-plan et les déclencheurs activent votre application au moment où l’utilisateur en a besoin.

-   Votre application peut utiliser des appareils vocaux et Bluetooth LE pour permettre aux utilisateurs d’interagir avec leur environnement.

Enfin, vous pouvez utiliser les données itinérantes et le Stockage sécurisé des informations d’identification Windows pour offrir une expérience d’itinérance uniforme sur tous les écrans Windows où les utilisateurs exécutent votre application. Les données itinérantes vous permettent de stocker facilement les préférences et paramètres de l’utilisateur dans le cloud, sans avoir à construire votre propre infrastructure de synchronisation. En outre, vous pouvez stocker les informations d’identification de l’utilisateur dans le Stockage sécurisé des informations d’identification, où sécurité et fiabilité occupent une place de choix.

##  <a name="monetize-your-app-your-way"></a>Choisissez comment vous faire rémunérer pour votre application


Sur Windows, vous pouvez choisir comment monétiser votre application — sur des téléphones, des tablettes, des PC et d’autres appareils. Nous vous proposons plusieurs manières de gagner de l’argent avec votre application et les services qu’elle fournit. Vous devez simplement choisir le procédé qui vous convient le mieux :

-   L’option la plus simple est de faire payer le téléchargement. Il vous suffit de fixer votre prix.
-   Les périodes d’essai permettent à l’utilisateur d’essayer votre application avant de l’acheter. Il peut ainsi la découvrir et l’adopter plus facilement que si vous aviez choisi l’option plus classique du « freemium ».
-   L’achat dans l’application est la solution offrant la plus grande souplesse pour faire fructifier votre application.

## <a name="lets-get-started"></a>Commençons


Pour une présentation plus détaillée d’UWP, lisez le [Guide des applications de plateforme Windows universelle](universal-application-platform-guide.md). Ensuite, consultez l’article [Se préparer](get-set-up.md) pour télécharger les outils dont vous avez besoin pour commencer à créer des applications.

## <a name="related-topics"></a>Rubriques connexes


* [Guide des applications de plateforme Windows universelle](universal-application-platform-guide.md)
* [Préparation](get-set-up.md)

## <a name="more-advanced-topics"></a>Rubriques plus avancées

* [.NET Native - What it means for Universal Windows Platform (UWP) developers [Les implications de .NET Native pour les développeurs d’applications de plateforme Windows universelle (UWP)]](https://blogs.windows.com/buildingapps/2015/08/20/net-native-what-it-means-for-universal-windows-platform-uwp-developers/#TYsD3tJuBJpK3Hc7.97)
* [Universal Windows apps in .NET (Applications Windows universelles dans .NET)](https://blogs.msdn.microsoft.com/dotnet/2015/07/30/universal-windows-apps-in-net)
* [.NET pour les applications UWP](https://msdn.microsoft.com/en-us/library/mt185501.aspx)

