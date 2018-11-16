---
author: drewbatgit
ms.assetid: ''
description: Montre comment gérer la diffusion de jeu pour une application UWP.
title: Gérer la diffusion de jeu
ms.author: drewbat
ms.date: 09/27/2017
ms.topic: article
keywords: windows10, jeu, diffusion
ms.localizationpriority: medium
ms.openlocfilehash: ae70c29927925abcf948435ed768871ba2427fd9
ms.sourcegitcommit: e38b334edb82bf2b1474ba686990f4299b8f59c7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/14/2018
ms.locfileid: "6842931"
---
# <a name="manage-game-broadcasting"></a>Gérer la diffusion de jeu
Cet article montre comment gérer la diffusion de jeu pour une application UWP. Les utilisateurs doivent lancer la diffusion de jeu à l’aide de l’interface utilisateur système qui est intégrée à Windows. Toutefois, à partir de Windows10, version1709, les applications peuvent lancer l’interface utilisateur de diffusion système et recevoir des notifications lorsque la diffusion démarre et s’arrête.

## <a name="add-the-windows-desktop-extensions-for-the-uwp-to-your-app"></a>Ajouter les extensions de bureau Windows pour UWP à votre application
Les API utilisées pour la gestion de diffusion d’application, disponibles dans l’espace de noms **[Windows.Media.AppBroadcasting](https://docs.microsoft.com/uwp/api/windows.media.appbroadcasting)**, ne sont pas incluses dans le contrat API universelle. Pour accéder à ces API, vous devez ajouter une référence aux extensions de bureau Windows pour UWP à votre application en procédant comme suit.

1. Dans VisualStudio, dans l’**Explorateur de solutions**, développez votre projet UWP, cliquez avec le bouton droit sur **Références** et sélectionnez **Ajouter une référence...**. 
2. Développez le nœud **Windows universel** et sélectionnez **Extensions**.
3. Dans la liste d’extensions, cochez la case en regard de l’entrée **Extensions de bureau Windows pour UWP** qui correspond à la version cible de votre projet. Pour les fonctionnalités de diffusion d’application, il doit s’agir au minimum de la version1709.
4. Cliquez sur **OK**.

## <a name="launch-the-system-ui-to-allow-the-user-to-initiate-broadcasting"></a>Lancer l’interface utilisateur système pour permettre à l’utilisateur de commencer la diffusion
Il existe plusieurs raisons pour lesquelles votre application peut ne pas être en mesure d’effectuer la diffusion actuellement. Ceci est notamment le cas si l’appareil utilisé ne satisfait pas la configuration matérielle requise pour la diffusion ou si une autre application est en cours de diffusion. Avant de lancer l’interface utilisateur système, vous pouvez vérifier si votre application peut effectuer la diffusion actuellement. Tout d’abord, vérifiez si les API de diffusion sont disponibles sur l’appareil actuel. Les API ne sont pas disponibles sur les appareils exécutant une version du système d’exploitation antérieure à Windows10, version1709. Plutôt que de rechercher une version spécifique du système d’exploitation, utilisez la méthode **[ApiInformation.IsApiContractPresent](https://docs.microsoft.com/uwp/api/windows.foundation.metadata.apiinformation.isapicontractpresent)** pour rechercher *Windows.Media.AppBroadcasting.AppBroadcastingContract* version1.0. Si ce contrat est présent, les API de diffusion sont disponibles sur l’appareil.

Ensuite, obtenez une instance de la classe **[AppBroadcastingUI](https://docs.microsoft.com/uwp/api/windows.media.appbroadcasting.appbroadcastingui)** en appelant la méthode de fabrique **[GetDefault](https://docs.microsoft.com/uwp/api/windows.media.appbroadcasting.appbroadcastingui.GetDefault)** sur le PC, sur lequel un seul utilisateur est connecté à la fois. Sur XBox, où plusieurs utilisateurs peuvent être connectés simultanément, appelez **[GetForUser](https://docs.microsoft.com/uwp/api/windows.media.appbroadcasting.appbroadcastingui.getforuser)** à la place. Ensuite, appelez **[GetStatus](https://docs.microsoft.com/uwp/api/windows.media.appbroadcasting.appbroadcastingui.GetStatus)** pour obtenir l’état de diffusion de votre application.

La propriété **[CanStartBroadcast](https://docs.microsoft.com/uwp/api/windows.media.appbroadcasting.appbroadcastingstatus.CanStartBroadcast)** de la classe **AppBroadcastingStatus** vous indique si l’application peut démarrer la diffusion actuellement. Sinon, vous pouvez vérifier la propriété **[Details](https://docs.microsoft.com/uwp/api/windows.media.appbroadcasting.appbroadcastingstatus.Details)** pour déterminer pour quelle raison la diffusion n’est pas disponible. En fonction de la raison, vous voudrez peut-être afficher l’état à l’utilisateur ou afficher des instructions pour l’activation de la diffusion.

[!code-cpp[CanStartBroadcast](./code/AppBroadcast/cpp/AppBroadcastExampleApp/App.cpp#SnippetCanStartBroadcast)]

Demandez l’affichage par le système de l’interface utilisateur de diffusion d’application en appelant **[ShowBroadcastUI](https://docs.microsoft.com/uwp/api/windows.media.appbroadcasting.appbroadcastingui.ShowBroadcastUI)**.

> [!NOTE] 
> La méthode **ShowBroadcastUI** représente une requête qui peut échouer, selon l’état actuel du système. Votre application ne doit pas supposer que la diffusion a commencé après l’appel de cette méthode. Utilisez l’événement **[IsCurrentAppBroadcastingChanged](https://docs.microsoft.com/uwp/api/windows.media.appbroadcasting.appbroadcastingmonitor.IsCurrentAppBroadcastingChanged)** pour être averti lorsque la diffusion démarre ou s’arrête.

[!code-cpp[LaunchBroadcastUI](./code/AppBroadcast/cpp/AppBroadcastExampleApp/App.cpp#SnippetLaunchBroadcastUI)]

## <a name="receive-notifications-when-broadcasting-starts-and-stops"></a>Recevoir des notifications lors du démarrage et de l’arrêt de la diffusion
Inscrivez-vous pour recevoir des notifications lorsque l’utilisateur utilise l’interface utilisateur système pour démarrer ou arrêter la diffusion de votre application en initialisant une instance de la classe **[AppBroadcastingMonitor](https://docs.microsoft.com/uwp/api/windows.media.appbroadcasting.appbroadcastingmonitor)** et en inscrivant un gestionnaire pour l’événement **[IsCurrentAppBroadcastingChanged](https://docs.microsoft.com/uwp/api/windows.media.appbroadcasting.appbroadcastingmonitor.IsCurrentAppBroadcastingChanged)**. Comme indiqué dans la section précédente, assurez-vous d’utiliser **[ApiInformation.IsApiContractPresent](https://docs.microsoft.com/uwp/api/windows.foundation.metadata.apiinformation.isapicontractpresent)** à un moment donné pour vérifier que les API de diffusion sont présentes sur l’appareil avant de tenter de les utiliser. 

[!code-cpp[AppBroadcastingRegisterChangedHandler](./code/AppBroadcast/cpp/AppBroadcastExampleApp/App.cpp#SnippetAppBroadcastingRegisterChangedHandler)]

Dans le gestionnaire de l’événement **IsCurrentAppBroadcastingChanged**, vous pouvez éventuellement mettre à jour l’interface utilisateur de votre application afin de refléter l’état actuel de diffusion.

[!code-cpp[AppBroadcastingChangedHandler](./code/AppBroadcast/cpp/AppBroadcastExampleApp/App.cpp#SnippetAppBroadcastingChangedHandler)]

## <a name="related-topics"></a>Rubriques associées

* [Jeux](index.md)

 

 




