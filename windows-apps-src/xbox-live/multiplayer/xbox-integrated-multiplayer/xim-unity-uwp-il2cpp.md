---
title: Utiliser le mode XIM (Unity avec IL2CPP)
description: À l’aide de mode multijoueur intégré Xbox avec Unity pour UWP avec le serveur principal script IL2CPP
ms.date: 04/03/2018
ms.topic: article
keywords: Xbox live, xbox, jeux, uwp, windows 10, xbox un, Unity, mode multijoueur intégré Xbox
ms.localizationpriority: medium
ms.openlocfilehash: a600fd253efae1daca34241b105a69514561e01d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57646444"
---
# <a name="use-xim-unity-with-il2cpp"></a>Utiliser le mode XIM (Unity avec IL2CPP)

## <a name="overview"></a>Vue d’ensemble

Prise en charge de Windows Runtime pour IL2CPP dans Unity

Avec la version de Unity 5.6f3 le moteur a inclus une nouvelle fonctionnalité qui permet aux développeurs d’utiliser des composants Windows Runtime (WinRT) directement dans le script en les incluant dans le projet de jeu directement. Jusqu'à ce que les 5.6 développeurs nécessité un plug-in, ou une dll pour prendre en charge des fonctionnalités de plate-forme (y compris le mode multijoueur intégré Xbox) à partir du script de jeu dans UWP. Cette nouvelle couche de projection supprime la nécessité de plug-in et introduit un flux de travail simplifiée et nouvelle prise en charge uniquement avec les jeux que choisissez le serveur principal de script IL2CPP.

- Pour plus d’informations sur la prise en main à l’aide de WinRT et Unity, consultez le [documentation Unity](https://docs.unity3d.com/Manual/IL2CPP-WindowsRuntimeSupport.html).
- Pour plus d’informations sur la façon d’ajouter à Unity à l’aide de IL2CPP Xbox Live, consultez le [documentation Xbox Live](https://docs.microsoft.com/windows/uwp/xbox-live/get-started-with-partner/partner-add-xbox-live-to-unity-uwp) sur le sujet.

## <a name="using-the-xim-unity-asset-package"></a>L’utilisation du package de ressource XIM Unity

### <a name="1-install-unity"></a>1. Installer Unity

Installer Unity 5.6 ou version ultérieure et assurez-vous d’avoir le **back-end de script Windows Store Il2CPP** sélectionnée pendant l’installation

### <a name="2-install-visual-studio-tools-for-unity-version-31-and-above-for-intellisense-support-when-using-winmds"></a>2. Installer Visual Studio Tools pour Unity version 3.1 et ci-dessus pour IntelliSense prend en charge lors de l’utilisation de Winmd

Pour Visual Studio 2015, vous pouvez le trouver [dans Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=SebastienLebreton.VisualStudio2015ToolsforUnity). Pour Visual Studio 2017, le composant peut être ajouté à l’intérieur du programme d’installation de Visual Studio 2017.

### <a name="3-open-a-new-or-existing-unity-project"></a>3. Ouvrez un projet Unity nouveau ou existant

### <a name="4-switch-the-platform-to-universal-windows-platform-in-the-unity-build-settings-menu"></a>4. Commutateur de la plateforme pour la plateforme Windows universelle dans le menu Paramètres de Build Unity

![Paramètre sélectionné de la build du menu de paramètres de build Unity avec la plateforme Windows universelle](../../images/xboxintegratedmultiplayer/xim-unity-build.png)

### <a name="5-enable-il2cpp-scripting-backend-in-the-unity-player-settings-and-set-api-compatibility-to-net-46"></a>5. Activer IL2CPP de script principal dans les paramètres du lecteur Unity et la valeur de compatibilité de l’API .NET 4.6

![La section de Configuration du menu Paramètres du lecteur Unity avec le paramètre « Compatibilité des Api » défini sur « .NET 4.6 »](../../images/unity/unity-il2cpp-1.png)

### <a name="6-import-the-latest-version-of-the-xbox-integrated-multiplayer-winrt-unity-asset-package"></a>6. Importer la dernière version du package Xbox intégré multijoueurs WinRT Unity asset

Vous pouvez le trouver à https://github.com/Microsoft/xbox-integrated-multiplayer-unity-plugin/releases

### <a name="7-you-can-now-use-xim-in-your-scripts"></a>7. Vous pouvez maintenant utiliser XIM dans vos scripts

Pour plus d’informations sur l’utilisation de XIM avec C#, consultez [XIM utiliser (C#)](using-xim-cs.md).

L’extrait de code suivant montre comment les XIM peut-être être intégrée à votre code :

```cs

using System;
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

#if ENABLE_WINMD_SUPPORT
using Microsoft.Xbox.Services.XboxIntegratedMultiplayer;
#endif

public class XimScript
{
    public void Start()
    {
#if ENABLE_WINMD_SUPPORT
        XboxIntegratedMultiplayer.TitleId = XboxLiveAppConfiguration.SingletonInstance.TitleId;
        XboxIntegratedMultiplayer.ServiceConfigurationId = XboxLiveAppConfiguration.SingletonInstance.ServiceConfigurationId;
#endif
    }

    public void Update()
    {
#if ENABLE_WINMD_SUPPORT
        using (var stateChanges = XboxIntegratedMultiplayer.GetStateChanges())
        {
            foreach (var stateChange in stateChanges)
            {
                switch (stateChange.Type)
                {
                    case XimStateChangeType.PlayerJoined:
                        HandlePlayerJoined((XimPlayerJoinedStateChange)stateChange);
                        break;

                    case XimStateChangeType.PlayerLeft:
                        HandlePlayerLeft((XimPlayerLeftStateChange)stateChange);
                        break;
                    [...]
                }
            }
        }
#endif
    }
}
```

Pour plus d’informations sur la `ENABLE_WINMD_SUPPORT` #define, directive, consultez la documentation Unity sur [prise en charge de Windows Runtime](https://docs.unity3d.com/Manual/IL2CPP-WindowsRuntimeSupport.html)

### <a name="8-required-capability-content"></a>8. Contenu de la fonctionnalité requise

Une application à l’aide de XIM par nature nécessite la connexion à et accepte les connexions à partir des ressources réseau à la fois sur Internet et le réseau local. Elle requiert également un accès aux appareils de microphone pour prendre en charge de la conversation vocale. Par conséquent, l’application doit déclarer les fonctions « InternetClientServer » et « PrivateNetworkClientServer » et la fonctionnalité de l’appareil « Microphone » dans les paramètres de publication trouvé dans les paramètres du lecteur.

![Menu de fonctionnalités d’Unity avec la « InternetClientServer », « PrivateNetworkClientServer » et « Microphone » sélectionnée](../../images/xboxintegratedmultiplayer/xim-unity-capability.png)

### <a name="9-build-the-project-in-unity"></a>9. Générez le projet dans Unity.

1. Accédez au fichier \| paramètres de Build, cliquez sur **plateforme Windows universelle** et assurez-vous que vous cliquez sur **plateforme de commutation**

2. Cliquez sur « Ajouter Open coulisses » pour ajouter la scène actuelle à la build

3. Dans la zone de liste déroulante de kit de développement logiciel, choisissez « 10 universelle »

4. Dans la zone de liste déroulante type de build UWP, choisir « D3D », mais « XAML » fonctionne également si vous préférez.

5. Cliquez sur « Build » pour Unity générer le projet UWP de Visual Studio qui encapsule votre jeu Unity dans une application UWP.

    Lorsque vous êtes invité pour un emplacement, créez un dossier pour éviter toute confusion, car un grand nombre de nouveaux fichiers sera créé. Il est recommandé de vous appelez le dossier « Build », puis sélectionnez ce dossier.

6. Ajouter le manifeste du réseau de XIM à votre projet

    Ajoutez le fichier networkmanifest.xml :

    ![Propriétés de networkmanifest.xml de Visual Studio](../../images/xboxintegratedmultiplayer/xim-unity-networkmanifest.png)

    Consultez [Configuration de projet XIM](xim-manifest.md) pour plus d’informations sur le manifeste de réseau et son contenu.

7. Compiler et exécuter l’application UWP à partir de Visual Studio

Cela lance l’application comme une application UWP normale et permettre les appels de Xbox Live à fonctionner comme ils ont besoin d’un conteneur d’application UWP à la fonction.

### <a name="10-rebuild-if-you-make-changes-to-anything-in-unity"></a>10. Régénérer si vous apportez des modifications à n’importe quel élément Unity

Si vous modifiez quelque chose dans Unity, vous devez reconstruire le projet UWP
