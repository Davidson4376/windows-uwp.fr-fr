---
author: PatrickFarley
title: Détecter des appareils distants
description: Apprenez à détecter des appareils distants depuis votre application à l'aide du projet «Rome».
ms.assetid: 5b4231c0-5060-49e2-a577-b747e20cf633
ms.author: pafarley
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: les appareils Windows 10, uwp, connectés, systèmes distants, rome, projet rome
ms.localizationpriority: medium
ms.openlocfilehash: 02d04074ece0033da8c3454a95bc35af201903f3
ms.sourcegitcommit: 933e71a31989f8063b020746fdd16e9da94a44c4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/11/2018
ms.locfileid: "4532723"
---
# <a name="discover-remote-devices"></a>Détecter des appareils distants
Votre application peut utiliser une connexion réseau sans fil, Bluetooth et cloud pour détecter les appareils Windows signés avec le même compte Microsoft que l’appareil détecteur. Aucun logiciel particulier n’est nécessaire sur les appareils distants pour qu’ils soient détectables.

> [!NOTE]
> Ce guide suppose que vous avez déjà accès à la fonctionnalité Systèmes distants, conformément à la procédure décrite dans [Lancer une application sur un appareil distant](launch-a-remote-app.md).

## <a name="filter-the-set-of-discoverable-devices"></a>Filtrer l’ensemble des appareils détectables
Vous pouvez restreindre l’ensemble des appareils détectables en utilisant un [**RemoteSystemWatcher**](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems.RemoteSystemWatcher) avec des filtres. Les filtres peuvent identifier le type de détection (proximale, réseau local ou connexion cloud), le type d’appareil (bureau, appareil mobile, Xbox, Hub et Holographique) et l’état de disponibilité (capacité d’un appareil à utiliser les fonctionnalités de système distant).

Les objets de filtrage doivent être construits avant ou pendant l’initialisation de l’objet **RemoteSystemWatcher**, car ils sont transmis en tant que paramètre à leur constructeur. Le code suivant crée un filtre de chaque type disponible, puis les ajoute à la liste.

> [!NOTE]
> Le code de ces exemples nécessite que votre fichier contienne une instruction `using Windows.System.RemoteSystems`.

[!code-cs[Main](./code/DiscoverDevices/MainPage.xaml.cs#SnippetMakeFilterList)]

> [!NOTE]
> La valeur du filtre «proximal» ne garantit pas le degré de proximité physique. Pour les scénarios qui requièrent une proximité physique fiable, utilisez la valeur [**RemoteSystemDiscoveryType.SpatiallyProximal**](https://docs.microsoft.com/uwp/api/windows.system.remotesystems.remotesystemdiscoverytype) dans votre filtre. Actuellement, ce filtre autorise uniquement les périphériques découverts par Bluetooth. À mesure que de nouveaux mécanismes et protocoles de détection garantissant une proximité physique seront pris en charge, ils seront également inclus ici.  
Il existe également une propriété dans la classe [**RemoteSystem**](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems.RemoteSystem) qui indique si un appareil détecté existe en réalité au sein d’une proximité physique: [**RemoteSystem.IsAvailableBySpatialProximity**](https://docs.microsoft.com/uwp/api/Windows.System.RemoteSystems.RemoteSystem.IsAvailableByProximity).

> [!NOTE]
> Si vous souhaitez découvrir les appareils sur un réseau local (déterminé par votre sélection de filtre du type de découverte), votre réseau doit utiliser un profil «privé» ou «domaine». Votre appareil ne découvrira pas les autres appareils qui se trouvent sur un réseau «public».

Lorsqu’une liste d’objets [**IRemoteSystemFilter**](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems.IRemoteSystemFilter) est créée, il est possible de la transmettre au constructeur d’un **RemoteSystemWatcher**.

[!code-cs[Main](./code/DiscoverDevices/MainPage.xaml.cs#SnippetCreateWatcher)]

Lorsque la méthode [**Start**](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems.RemoteSystemWatcher.Start) de cet observateur est appelée, elle ne déclenche l’événement [**RemoteSystemAdded**](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems.RemoteSystemWatcher.RemoteSystemAdded) que si un appareil correspondant à l’ensemble des critères suivants est détecté:
* L’appareil est détectable par une connexion proximale.
* Il s’agit d’un ordinateur de bureau ou d’un téléphone.
* Il est considéré comme disponible.

Ensuite, la procédure de traitement des événements, de récupération des objets [**RemoteSystem**](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems.RemoteSystem) et de connexion aux appareils distants est exactement le même que dans [Lancer une application sur un appareil distant](launch-a-remote-app.md). En résumé, les objets **RemoteSystem** sont stockés comme des propriétés d’objets [**RemoteSystemAddedEventArgs**](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems.RemoteSystemAddedEventArgs), qui sont transmis avec chaque événement **RemoteSystemAdded**.

## <a name="discover-devices-by-address-input"></a>Détecter des appareils par entrée d’adresse
Certains appareils ne peuvent pas être associés à un utilisateur ou détectés par un balayage, mais ils restent accessibles si l’application détectrice utilise une adresse directe. La classe [**HostName**](https://msdn.microsoft.com/library/windows/apps/windows.networking.hostname.aspx) permet de représenter l’adresse d’un appareil distant. Cette adresse est souvent stockée sous la forme d’une adresseIP, mais d’autres formats sont autorisés (pour plus d’informations, consultez la section [**Constructeur HostName**](https://msdn.microsoft.com/library/windows/apps/br207118.aspx)).

Un objet **RemoteSystem** est récupéré si un objet **HostName** valide est fourni. Si l’adresse n’est pas valide, une référence d’objet `null` est renvoyée.

[!code-cs[Main](./code/DiscoverDevices/MainPage.xaml.cs#SnippetFindByHostName)]

## <a name="querying-a-capability-on-a-remote-system"></a>Interroger une fonctionnalité sur un système distant

Bien que distincte du filtrage de détection, l’interrogation des fonctionnalités de périphérique peut être une composante essentielle du processus de détection. À l’aide de la méthode [**RemoteSystem.GetCapabilitySupportedAsync**](https://docs.microsoft.com/uwp/api/windows.system.remotesystems.remotesystem.GetCapabilitySupportedAsync), vous pouvez interroger les systèmes distants découverts pour prendre en charge certaines fonctionnalités telles que la connexion aux sessions à distance et le partage d’entités spatiales (holographiques). Voir la classe [**KnownRemoteSystemCapabilities**](https://docs.microsoft.com/uwp/api/windows.system.remotesystems.knownremotesystemcapabilities) pour consulter la liste des fonctionnalités pouvant être interrogées.

```csharp
// Check to see if the given remote system can accept LaunchUri requests
bool isRemoteSystemLaunchUriCapable = remoteSystem.GetCapabilitySupportedAsync(KnownRemoteSystemCapabilities.LaunchUri);
```

## <a name="cross-user-discovery"></a>Détection entre utilisateurs

Les développeurs peuvent spécifier la détection de _tous_ les appareils à proximité de l’appareil du client, et non des seuls appareils enregistrés pour ce même utilisateur. Cela est implémenté par le biais d’un filtre **IRemoteSystemFilter** spécial, [**RemoteSystemAuthorizationKindFilter**](https://docs.microsoft.com/uwp/api/windows.system.remotesystems.remotesystemauthorizationkindfilter). Son implémentation est similaire aux autres types de filtres:

```csharp
// Construct a user type filter that includes anonymous devices
RemoteSystemAuthorizationKindFilter authorizationKindFilter = new RemoteSystemAuthorizationKindFilter(RemoteSystemAuthorizationKind.Anonymous);
// then add this filter to the RemoteSystemWatcher
```

* Une valeur **Anonymous** de [**RemoteSystemAuthorizationKind**](https://docs.microsoft.com/uwp/api/windows.system.remotesystems.remotesystemauthorizationkind) autorise la détection de tous les appareils proximaux, y compris ceux des utilisateurs non approuvés.
* La valeur **SameUser** filtre la découverte aux seuls appareils enregistrés pour le même utilisateur que l’appareil du client. Il s’agit du comportement par défaut.

### <a name="checking-the-cross-user-sharing-settings"></a>Vérification des paramètres de partage entre utilisateurs

Outre le filtre ci-dessus spécifié dans votre application de détection, l’appareil du client lui-même doit également être configuré pour autoriser les expériences partagées à partir d’appareils connectés avec d’autres utilisateurs. Il s’agit d’un paramètre système qu’il est possible d’interroger avec une méthode statique dans la classe **RemoteSystem**:

```csharp
if (!RemoteSystem.IsAuthorizationKindEnabled(RemoteSystemAuthorizationKind.Anonymous)) {
    // The system is not authorized to connect to cross-user devices. 
    // Inform the user that they can discover more devices if they
    // update the setting to "Anonymous".
}
```

Pour modifier ce paramètre, l’utilisateur doit ouvrir l’application **paramètres**. Dans le menu **Système** > **Expériences partagées** > **Partager avec d’autres appareils**, une liste déroulante permet à l’utilisateur d’indiquer les appareils avec lesquels leur système peut partager des contenus.

![Page de paramètres Expériences partagées](images/shared-experiences-settings.png)

## <a name="related-topics"></a>Rubriques connexes
* [Applications et appareils connectés (projet «Rome»)](connected-apps-and-devices.md)
* [Lancer une application distante](launch-a-remote-app.md)
* [Référence sur l’API Systèmes distants](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems)
* [Exemple de systèmes distants](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/RemoteSystems)
