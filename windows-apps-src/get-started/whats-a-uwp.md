---
author: GrantMeStrength
ms.assetid: C9787269-B54F-4FFA-A884-D4A3BF28F80D
title: "Qu’est-ce qu’une application de plateforme Windows universelle (UWP) ?"
description: "Découvrez les différents types d’applications Windows universelles: applications du Windows Store, applications du Windows Phone Store et applications Windows Runtime."
ms.author: jken
ms.date: 03/22/2017
ms.topic: article
pms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.openlocfilehash: 3bbced2db33210952b6c8a45f98e36582330d7d9
ms.sourcegitcommit: 214a1dcb24e0811811bd7a4a07bfe707ecd93b18
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/15/2017
---
# <a name="whats-a-universal-windows-platform-uwp-app"></a>Qu’est-ce qu’une application de plateforme Windows universelle (UWP) ?

La plateforme Windows universelle (UWP) est une plateforme d’application pour Windows10. Vous pouvez développer des applications pour UWP avec un seul ensemble d’API, un seul package d’application et un seul Store pour atteindre tous les appareils Windows 10: PC, tablettes, téléphones, Xbox, HoloLens, Surface Hub, etc. Il est plus facile de prendre en charge plusieurs tailles d’écran et plusieurs modèles d’interaction, qu’il s’agisse d’impulsions tactiles, de la souris et du clavier, d’une manette de jeu ou d’un stylet. Au cœur des applications UWP est l’idée que les utilisateurs souhaitent que leurs *expériences* soient mobiles sur TOUS leurs appareils, et qu’ils puissent utiliser l’appareil le plus adapté ou le plus productif pour la tâche qu’ils ont à effectuer.

Une UWP est également flexible: vous n’êtes pas obligé d’utiliser C# et XAML, si vous ne le souhaitez pas. Aimez-vous développer dans Unity ou MonoGame? Vous préférez JavaScript? Pas de problème, vous pouvez les utiliser tant que vous le voulez. Vous avez une application de bureau C++ que vous souhaitez étendre grâce aux fonctionnalités UWP et commercialiser dans le Windows Store? Cela ne pose aucun problème. 

Résultat: vous pouvez passer votre temps à utiliser les langages de programmation, les infrastructures et les API que vous maîtrisez le mieux, le tout dans un même projet, et aboutir à l’exécution d’un même code sur les divers types d’appareils Windows qui existent aujourd’hui. Une fois que vous avez écrit votre application UWP, vous pouvez ensuite la publier sur le Windows store pour que tout le monde puisse la voir.

![Appareils avec Windows](images/1894834-hig-device-primer-01-500.png)
 
##<a name="so-what-exactly-is-a-uwp-app"></a>Donc, qu’est-ce *exactement* qu’une application UWP?

Qu’est-ce qui différencie une application UWP? Voici quelques-unes des caractéristiques qui rendent différentes les applications UWP sur Windows 10.

-   **Il existe une surface d’API commune à tous les appareils.**

    Les API au cœur de la plateforme Windows universelle (UWP) sont les mêmes pour toutes les catégories d’appareils Windows. Si votre application utilise uniquement les API principales, elle s’exécutera sur n’importe quel appareil Windows10, que vous cibliez un PC de bureau, une Xbox ou un casque à réalité mixte.

-   **Les kits de développement logiciel d’extension permettent à votre application de réaliser des choses bien pratiques sur des appareils spécifiques.**

    Les Kits de développement logiciels (SDK) d’extension ajoutent des API spécialisées pour chaque catégorie d’appareils. Par exemple, si votre application UWP cible HoloLens, vous pouvez ajouter des fonctionnalités HoloLens aux API UWP de base.
    Si vous ciblez des API universelles, votre package d’application peut être lancé sur tous les appareils exécutant Windows10. Toutefois, si vous souhaitez que votre application UWP tire parti d'API de périphériques spécifiques dans le cas où elle est exécutée dans une catégorie particulière d’appareil, vous pouvez vérifier au moment de l’exécution si une API existe avant de l’appeler. 

-   **Les applications sont empaquetées à l’aide du format d’empaquetage .AppX. et distribuées depuis le Windows Store.**

    Toutes les applications UWP sont distribuées en tant que package AppX. Cela fournit un mécanisme d’installation digne de confiance et garantit la possibilité de déployer et de mettre à jour vos applications de façon transparente.

-   **Il existe un seul magasin pour tous les appareils.**

    Après vous être inscrit en tant que développeur d’applications, vous pouvez soumettre votre application au Store et la rendre disponible sur tous les types d’appareils, ou uniquement sur ceux que vous choisissez. Vous soumettez et gérez toutes vos applications pour appareils Windows au même endroit.

-   **Les applications prennent en charge les contrôles et les entrées adaptatives.**

    Les éléments d’interface utilisateur utilisent des *pixels effectifs* (voir[Conception réactive 101 pour les applications UWP](https://msdn.microsoft.com/library/windows/apps/Dn958435)), de sorte à pouvoir répondre avec une couche adaptée au nombre de pixels d’écran disponibles sur l’appareil. Ils fonctionnent bien avec plusieurs types d’entrées, par exemple, le clavier, la souris, les impulsions tactiles, le stylet et les manettes Xbox One. Si vous avez besoin d’adapter davantage votre interface utilisateur à une taille d’écran ou à un appareil spécifiques, de nouveaux panneaux de disposition et outils vous aident à adapter votre interface utilisateur aux appareils sur lesquels votre application s’exécute.



## <a name="use-a-language-you-already-know"></a>Utilisez un langage que vous connaissez déjà


Les applications UWP peuvent utiliser Windows Runtime, API native intégrée au système d’exploitation. Cette API est implémentée en C++ et prise en charge dans C#, Visual Basic, C++ et JavaScript. Certaines options d'écriture d'applications UWP incluent:
-   Interface utilisateur XAML et un arrière-plan C#, VB ou C++
-   Interface utilisateur DirectX et arrière-plan C++
-   JavaScript et HTML

Microsoft Visual Studio 2017 fournit un modèle d’application UWP pour chaque langage, qui vous permet de créer un projet unique pour tous les appareils. Une fois votre travail terminé, vous pouvez produire un package d’application et le soumettre au Windows Store à partir de Visual Studio afin que votre application atteigne des clients sur tout appareil Windows 10.

## <a name="uwp-apps-come-to-life-on-windows"></a>Les applications UWP prennent vie sur Windows


Sur Windows, votre application peut fournir des informations pertinentes en temps réel à vos utilisateurs et les inciter à en redemander. Dans l’économie numérique moderne, votre application doit être attrayante pour que vos utilisateurs s’en souviennent. Windows met à votre disposition de nombreuses ressources permettant de maintenir l’attention des utilisateurs de votre application :

-   Les vignettes dynamiques et l’écran de verrouillage permettent de voir d’un seul coup d’œil des informations fournies au moment opportun et adaptées au contexte.

-   Les notifications Push permettent d’alerter vos utilisateurs en temps réel quand c’est nécessaire.

-   Le centre de maintenance vous permet d’organiser et d’afficher des notifications et contenus sur lesquels les utilisateur doivent agir.

-   L’exécution en arrière-plan et les déclencheurs activent votre application au moment où l’utilisateur en a besoin.

-   Votre application peut utiliser des appareils vocaux et Bluetooth LE pour permettre aux utilisateurs d’interagir avec leur environnement.

-   Prise en charge de l’encre numérique riche et Dial innovant.

-   Cortana ajoute de la personnalité à votre logiciel.

-   XAML fournit les outils nécessaires pour créer des interfaces utilisateur fluides et animées.

Enfin, vous pouvez utiliser les données itinérantes et le Stockage sécurisé des informations d’identification Windows pour offrir une expérience d’itinérance uniforme sur tous les écrans Windows où les utilisateurs exécutent votre application. Les données itinérantes vous permettent de stocker facilement les préférences et paramètres de l’utilisateur dans le cloud, sans avoir à construire votre propre infrastructure de synchronisation. En outre, vous pouvez stocker les informations d’identification de l’utilisateur dans le Stockage sécurisé des informations d’identification, où sécurité et fiabilité occupent une place de choix.

##  <a name="monetize-your-app"></a>Monétiser votre application


Sur Windows, vous pouvez choisir comment monétiser votre application— sur des téléphones, des tablettes, des PC et d’autres appareils. Nous vous proposons plusieurs manières de gagner de l’argent avec votre application et les services qu’elle fournit. Vous devez simplement choisir le procédé qui vous convient le mieux :

-   L’option la plus simple est de faire payer le téléchargement. Il vous suffit de fixer votre prix.
-   Les périodes d’essai permettent à l’utilisateur d’essayer votre application avant de l’acheter. Il peut ainsi la découvrir et l’adopter plus facilement que si vous aviez choisi l’option plus classique du «freemium».
-   Utiliser des prix de vente pour les applications et les extensions.
-   Des achats et des publicités dans les applications sont également disponibles.

## <a name="lets-get-started"></a>Commençons


Pour une présentation plus détaillée d’UWP, lisez le [Guide des applications de plateforme Windows universelle](universal-application-platform-guide.md). Ensuite, consultez [Se préparer](get-set-up.md) pour télécharger les outils dont vous avez besoin pour commencer à créer des applications, puis à écrire [votre première application](your-first-app.md)!


## <a name="more-advanced-topics"></a>Rubriques plus avancées

* [.NET Native - What it means for Universal Windows Platform (UWP) developers [Les implications de .NET Native pour les développeurs d’applications de plateforme Windows universelle (UWP)]](https://blogs.windows.com/buildingapps/2015/08/20/net-native-what-it-means-for-universal-windows-platform-uwp-developers/#TYsD3tJuBJpK3Hc7.97)
* [Universal Windows apps in .NET (Applications Windows universelles dans .NET)](https://blogs.msdn.microsoft.com/dotnet/2015/07/30/universal-windows-apps-in-net)
* [.NET pour les applications UWP](https://msdn.microsoft.com/library/mt185501.aspx)
