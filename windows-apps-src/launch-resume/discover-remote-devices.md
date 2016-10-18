---
author: PatrickFarley
title: "Détecter des appareils distants"
description: "Découvrez comment détecter des appareils distants à partir de votre application à l’aide du projet «Rome»."
translationtype: Human Translation
ms.sourcegitcommit: ff8e16d0e376d502157ae42b9cdae11875008554
ms.openlocfilehash: cb1f9cf6915378203919fdf63bcebc935af74a30

---

# Détecter des appareils distants
Votre application peut utiliser une connexion réseau sans fil, Bluetooth et cloud pour détecter les appareils Windows authentifiés avec le même compte Microsoft que l’appareil détecteur. Les appareils communs qui peuvent accepter des connexions anonymes, comme SurfaceHub et XboxOne, sont également détectables. Aucun logiciel particulier n’est nécessaire sur les appareils distants pour qu’ils soient détectables.

> [!NOTE]
> Ce guide suppose que vous avez déjà accès à la fonctionnalité Systèmes distants, conformément à la procédure décrite dans [Lancer une application sur un appareil distant](launch-a-remote-app.md).

## Filtrer l’ensemble des appareils détectables
Vous pouvez restreindre l’ensemble des appareils détectables en utilisant un [**RemoteSystemWatcher**](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems.RemoteSystemWatcher) avec des filtres. Les filtres peuvent identifier le type de détection (réseau local ou connexion cloud), le type d’appareil (bureau, appareil mobile, Xbox, Hub et Holographique) et l’état de disponibilité (capacité d’un appareil à utiliser les fonctionnalités de système distant).

Les objets de filtrage doivent être construits avant l’initialisation de l’objet **RemoteSystemWatcher**, car ils sont transmis en tant que paramètre à leur constructeur. Le code suivant crée un filtre de chaque type disponible, puis les ajoute à la liste.

> [!NOTE]
> Le code de ces exemples suppose que votre fichier contient une instruction `using Windows.System.RemoteSystems`.

[!code-cs[Main](./code/DiscoverDevices/MainPage.xaml.cs#SnippetMakeFilterList)]

Une fois créée, la liste des objets [**IRemoteSystemFilter**](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems.IRemoteSystemFilter) peut être transmise au constructeur d’un **RemoteSystemWatcher**.

[!code-cs[Main](./code/DiscoverDevices/MainPage.xaml.cs#SnippetCreateWatcher)]

Lorsque la méthode [**Start**](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems.RemoteSystemWatcher.Start) de cet observateur est appelée, elle ne déclenche l’événement [**RemoteSystemAdded**](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems.RemoteSystemWatcher.RemoteSystemAdded) que si un appareil correspondant à l’ensemble des critères suivants est détecté:
* L’appareil est détectable par une connexion proximale.
* Il s’agit d’un ordinateur de bureau ou d’un téléphone.
* Il est considéré comme disponible.

Ensuite, la procédure de traitement des événements, de récupération des objets [**RemoteSystem**](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems.RemoteSystem) et de connexion aux appareils distants est exactement le même que dans [Lancer une application sur un appareil distant](launch-a-remote-app.md). En résumé, les objets **RemoteSystem** sont stockés comme des propriétés d’objets [**RemoteSystemAddedEventArgs**](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems.RemoteSystemAddedEventArgs), qui sont des paramètres de chaque événement **RemoteSystemAdded**.

## Détecter des appareils par entrée d’adresse
Certains appareils ne peuvent pas être associés à un utilisateur ou détectés par un balayage, mais ils restent accessibles si l’application détectrice utilise une adresse directe. La classe [**HostName**](https://msdn.microsoft.com/library/windows/apps/windows.networking.hostname.aspx) permet de représenter l’adresse d’un appareil distant. Cette adresse est souvent stockée sous la forme d’une adresseIP, mais d’autres formats sont autorisés (pour plus d’informations, consultez la section [**Constructeur HostName**](https://msdn.microsoft.com/library/windows/apps/br207118.aspx)).

Un objet **RemoteSystem** est récupéré si un objet **HostName** valide est fourni. Si l’adresse n’est pas valide, une référence d’objet `null` est renvoyée.

[!code-cs[Main](./code/DiscoverDevices/MainPage.xaml.cs#SnippetFindByHostName)]

## Rubriques connexes
[Applications et appareils connectés (projet «Rome»)](connected-apps-and-devices.md)  
[Lancer une application sur un appareil distant](launch-a-remote-app.md)  
[Référence sur l’API Systèmes distants](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems)  
L’[exemple Systèmes distants](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/RemoteSystems ) montre comment détecter un système distant, lancer une application sur un système distant et utiliser des services d’application pour échanger des messages avec des applications qui s’exécutent sur deuxsystèmes.



<!--HONumber=Aug16_HO5-->


