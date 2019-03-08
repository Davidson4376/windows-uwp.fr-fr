---
title: Gérer des scénarios de suppression de périphériques dans Direct3D 11
description: Cette rubrique explique comment recréer la chaîne d’interface d’appareils Direct3D et DXGI quand la carte graphique est supprimée ou réinitialisée.
ms.assetid: 8f905acd-08f3-ff6f-85a5-aaa99acb389a
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, jeux, directx 11, périphérique perdu
ms.localizationpriority: medium
ms.openlocfilehash: c11bbf7657644fbf616590f50d75d93f62ed993e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57646604"
---
# <a name="span-iddevgaminghandlingdevice-lostscenariosspanhandle-device-removed-scenarios-in-direct3d-11"></a><span id="dev_gaming.handling_device-lost_scenarios"></span>Gérer les scénarios d’appareil supprimé dans Direct3D 11



Cette rubrique explique comment recréer la chaîne d’interface d’appareils Direct3D et DXGI quand la carte graphique est supprimée ou réinitialisée.

Dans DirectX 9, les applications peuvent présenter un état de type « [périphérique perdu](https://msdn.microsoft.com/library/windows/desktop/bb174714) » quand un périphérique D3D passe à un état non opérationnel. Par exemple, lorsqu’une application Direct3D 9 en mode plein écran perd le focus, le périphérique Direct3D est « perdu ». Toute tentative de dessin avec un périphérique perdu entraîne un échec sans avertissement. Direct3D 11 utilise des interfaces GDI (Graphics Device Interface) virtuelles, ce qui permet à plusieurs programmes de partager le même périphérique graphique physique et d’éviter les situations où les applications perdent le contrôle du périphérique Direct3D. Cependant, la disponibilité de la carte graphique peut toujours changer. Exemple :

-   Le pilote graphique est mis à jour.
-   Le système passe d’une carte graphique axée sur l’économie d’énergie à une carte graphique axée sur les performances.
-   Le périphérique graphique cesse de répondre et est réinitialisé.
-   Une carte graphique est attachée ou supprimée physiquement.

Dans ces circonstances, DXGI retourne un code d’erreur indiquant que le périphérique Direct3D doit être réinitialisé et que les ressources de périphérique doivent être recréées. Cette procédure pas à pas montre comment les applications et les jeux Direct3D 11 peuvent détecter ces situations et réagir de manière appropriée quand la carte graphique est réinitialisée, supprimée ou modifiée. Exemples de code sont fournis à partir du modèle d’application de 11 DirectX (Windows universel) fourni avec Microsoft Visual Studio 2015.

## <a name="instructions"></a>Instructions

### <a name="spanspanstep-1"></a><span></span>Étape 1 :

Incluez une recherche de l’erreur relative à une suppression d’appareil dans la boucle de rendu. Présentez l’image en appelant [**IDXGISwapChain::Present**](https://msdn.microsoft.com/library/windows/desktop/bb174576) (ou [**Present1**](https://msdn.microsoft.com/library/windows/desktop/hh446797), etc.). Ensuite, vérifiez si elle est retournée [ **DXGI\_erreur\_appareil\_supprimé** ](https://msdn.microsoft.com/library/windows/desktop/bb509553) ou **DXGI\_erreur\_appareil \_Réinitialiser**.

Tout d’abord, le modèle stocke le HRESULT retourné par la chaîne de permutation DXGI :

```cpp
HRESULT hr = m_swapChain->Present(1, 0);
```

Après avoir géré les autres travaux de présentation de l’image, le modèle vérifie s’il existe une erreur relative à la suppression de périphérique. Si nécessaire, il appelle une méthode pour gérer l’état de suppression du périphérique :

```cpp
// If the device was removed either by a disconnection or a driver upgrade, we
// must recreate all device resources.
if (hr == DXGI_ERROR_DEVICE_REMOVED || hr == DXGI_ERROR_DEVICE_RESET)
{
    HandleDeviceLost();
}
else
{
    DX::ThrowIfFailed(hr);
}
```

### <a name="step-2"></a>Étape 2 :

Incluez également une recherche de l’erreur relative à une suppression de périphérique en réponse aux changements de taille de fenêtre. Il s’agit d’un bon emplacement pour rechercher les [ **DXGI\_erreur\_appareil\_supprimé** ](https://msdn.microsoft.com/library/windows/desktop/bb509553) ou **DXGI\_erreur\_appareil\_ RÉINITIALISER** pour plusieurs raisons :

-   Le redimensionnement de la chaîne de permutation nécessite un appel à l’adaptateur DXGI sous-jacent, ce qui peut retourner l’erreur relative à une suppression de périphérique.
-   L’application peut avoir été déplacée vers un moniteur attaché à un autre périphérique graphique.
-   Quand un appareil graphique est supprimé ou réinitialisé, la résolution du Bureau change le plus souvent, ce qui entraîne un changement de taille de fenêtre.

Le modèle vérifie le HRESULT retourné par [**ResizeBuffers**](https://msdn.microsoft.com/library/windows/desktop/bb174577) :

```cpp
// If the swap chain already exists, resize it.
HRESULT hr = m_swapChain->ResizeBuffers(
    2, // Double-buffered swap chain.
    static_cast<UINT>(m_d3dRenderTargetSize.Width),
    static_cast<UINT>(m_d3dRenderTargetSize.Height),
    DXGI_FORMAT_B8G8R8A8_UNORM,
    0
    );

if (hr == DXGI_ERROR_DEVICE_REMOVED || hr == DXGI_ERROR_DEVICE_RESET)
{
    // If the device was removed for any reason, a new device and swap chain will need to be created.
    HandleDeviceLost();

    // Everything is set up now. Do not continue execution of this method. HandleDeviceLost will reenter this method 
    // and correctly set up the new device.
    return;
}
else
{
    DX::ThrowIfFailed(hr);
}
```

### <a name="step-3"></a>Étape 3 :

N’importe quel moment votre application reçoit le [ **DXGI\_erreur\_appareil\_supprimé** ](https://msdn.microsoft.com/library/windows/desktop/bb509553) erreur, il doit réinitialiser le périphérique Direct3D et recréer tout dépendant du périphérique ressources. Libérez les références aux ressources d’appareil graphique créées avec le précédent appareil Direct3D. Ces ressources ne sont plus valides. Toutes les références à la chaîne de permutation doivent être libérées pour en permettre la création d’une autre.

La méthode HandleDeviceLost libère la chaîne d’échange et indique aux composants d’application de libérer les ressources de périphérique :

```cpp
m_swapChain = nullptr;

if (m_deviceNotify != nullptr)
{
    // Notify the renderers that device resources need to be released.
    // This ensures all references to the existing swap chain are released so that a new one can be created.
    m_deviceNotify->OnDeviceLost();
}
```

Elle crée ensuite une chaîne d’échange, puis réinitialise les ressources dépendantes du périphérique contrôlées par la classe de gestion de périphérique :

```cpp
// Create the new device and swap chain.
CreateDeviceResources();
m_d2dContext->SetDpi(m_dpi, m_dpi);
CreateWindowSizeDependentResources();
```

Une fois que le périphérique et la chaîne d’échange ont été rétablis, la méthode indique aux composants d’application de réinitialiser les ressources dépendantes du périphérique :

```cpp
// Create the new device and swap chain.
CreateDeviceResources();
m_d2dContext->SetDpi(m_dpi, m_dpi);
CreateWindowSizeDependentResources();

if (m_deviceNotify != nullptr)
{
    // Notify the renderers that resources can now be created again.
    m_deviceNotify->OnDeviceRestored();
}
```

À la fin de la méthode HandleDeviceLost, le contrôle retourne à la boucle de rendu, qui continue de dessiner l’image suivante.

## <a name="remarks"></a>Notes


### <a name="investigating-the-cause-of-device-removed-errors"></a>Recherche de l’origine des erreurs relatives à une suppression de périphérique

La répétition d’erreurs relatives à une suppression de périphérique DXGI peut indiquer que votre code graphique crée des conditions non valides durant une routine de dessin. Elle peut également indiquer une défaillance matérielle ou un bogue dans le pilote graphique. Pour rechercher l’origine des erreurs relatives à une suppression de périphérique, appelez [**ID3D11Device::GetDeviceRemovedReason**](https://msdn.microsoft.com/library/windows/desktop/ff476526) avant de libérer le périphérique Direct3D. Cette méthode retourne l’un des six codes d’erreur DXGI possibles qui indiquent l’origine de l’erreur relative à une suppression de périphérique :

-   **DXGI\_ERREUR\_APPAREIL\_BLOQUÉS**: Le pilote graphique a cessé de répondre en raison d’une combinaison non valide de commandes graphiques envoyés par l’application. Si vous obtenez cette erreur à plusieurs reprises, cela indique que votre application est probablement à l’origine de la défaillance du périphérique et qu’elle doit être déboguée.
-   **DXGI\_ERREUR\_APPAREIL\_SUPPRIMÉ**: Le périphérique graphique a été supprimé physiquement, désactivé, ou une mise à niveau du pilote s’est produite. Cela arrive parfois et c’est une situation normale. Votre application ou votre jeu doit recréer les ressources de périphérique, comme cela est décrit dans cette rubrique.
-   **DXGI\_ERREUR\_APPAREIL\_RÉINITIALISER**: Le périphérique graphique a échoué en raison d’une commande formée de manière incorrecte. Si vous obtenez cette erreur à plusieurs reprises, cela peut signifier que votre code envoie des commandes de dessin non valides.
-   **DXGI\_ERREUR\_PILOTE\_INTERNE\_ERREUR**: Le pilote graphique a rencontré une erreur et réinitialise l’appareil.
-   **DXGI\_ERREUR\_NON VALIDE\_APPELER**: L’application a fourni les données de paramètre non valide. Si vous obtenez cette erreur, même une seule fois, cela signifie que votre code est à l’origine de l’état de suppression du périphérique et qu’il doit être débogué.
-   **S\_OK**: Retourné lorsqu’un périphérique graphique a été activé, désactivé ou réinitialiser sans invalider le périphérique d’affichage actuel. Par exemple, ce code d’erreur peut être retourné si une application utilise [WARP (Windows Advanced Rasterization Platform)](https://msdn.microsoft.com/library/windows/desktop/gg615082) et si un adaptateur matériel est disponible.

Le code suivant récupère la [ **DXGI\_erreur\_appareil\_supprimé** ](https://msdn.microsoft.com/library/windows/desktop/bb509553) erreur de code et l’imprimer à la console de débogage. Insérez ce code au début de la méthode HandleDeviceLost :

```cpp
    HRESULT reason = m_d3dDevice->GetDeviceRemovedReason();

#if defined(_DEBUG)
    wchar_t outString[100];
    size_t size = 100;
    swprintf_s(outString, size, L"Device removed! DXGI_ERROR code: 0x%X\n", reason);
    OutputDebugStringW(outString);
#endif
```

Pour plus d’informations, consultez [ **GetDeviceRemovedReason** ](https://msdn.microsoft.com/library/windows/desktop/ff476526) et [ **DXGI\_erreur**](https://msdn.microsoft.com/library/windows/desktop/bb509553).

### <a name="testing-device-removed-handling"></a>Gestion de l’appareil de test supprimé

L’invite de commandes de développeur de Visual Studio prend en charge un outil de ligne de commande « dxcap » pour la capture et la lecture d’événement Direct3D en rapport avec les diagnostics des graphiques Visual Studio. Vous pouvez utiliser l’option de ligne de commande «-forcetdr » alors que votre application est en cours d’exécution qui force un événement de détection de délai d’expiration de GPU et de récupération, ce qui a déclenché DXGI\_erreur\_appareil\_supprimées et ce qui vous permet de tester votre erreur code de gestion.

> **Remarque** DXCap et ses DLL de prise en charge sont installés sur system32/syswow64 en tant qu’outils graphiques pour Windows 10, qui ne sont plus distribués via le Kit de développement logiciel (SDK) Windows. Ils sont désormais fournis via la fonctionnalité Outils graphiques à la demande, un composant facultatif de système d’exploitation qui doit être installé afin d’activer et d’utiliser les outils graphiques sous Windows 10. Vous trouverez ici des plus d’informations sur la façon d’installer les outils Graphics pour Windows 10 : <https://msdn.microsoft.com/library/mt125501.aspx#InstallGraphicsTools>
