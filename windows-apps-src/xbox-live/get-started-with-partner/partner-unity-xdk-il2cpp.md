---
title: Unity pour XDK avec serveur principal IL2CPP
description: Ajouter le support de Xbox Live à Unity pour XDK avec IL2CPP script de back-end pour ID@Xbox et gérés des partenaires
ms.assetid: 790a49ad-eff4-4916-8578-968ca8483211
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, jeux, uwp, windows 10, xbox un, Unity
ms.localizationpriority: medium
ms.openlocfilehash: cfd722ca0d0b080f6395680cd62000cea9b402fa
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57608464"
---
# <a name="add-xbox-live-support-to-unity-for-xdk-with-il2cpp-scripting-backend-for-idxbox-and-managed-partners"></a>Ajouter le support de Xbox Live à Unity pour XDK avec IL2CPP script de back-end pour ID@Xbox et gérés des partenaires

## <a name="overview"></a>Vue d’ensemble

Prise en charge de Windows Runtime pour IL2CPP dans Unity

Avec la version de Unity 5.6f3 le moteur a inclus une nouvelle fonctionnalité qui permet aux développeurs d’utiliser des composants Windows Runtime (WinRT) directement dans le script en les incluant dans le projet de jeu directement. Jusqu'à ce que les 5.6 développeurs nécessité un plug-in, ou une dll pour prendre en charge des fonctionnalités de plate-forme (y compris Xbox Live SDK) à partir du script de jeu. Cette nouvelle couche de projection supprime la nécessité de plug-in et introduit un flux de travail simplifiée et nouvelle prise en charge uniquement avec les jeux que choisissez le serveur principal de script IL2CPP.

Pour plus d’informations sur la prise en main, consultez la documentation Unity : https://docs.unity3d.com/Manual/IL2CPP-WindowsRuntimeSupport.html

## <a name="steps"></a>Étapes

**(1) installer Unity**

Installez Unity 5.6 ou version ultérieure et assurez-vous de qu'avoir l’extension de l’éditeur Xbox One installée.

**(2) installez Visual Studio Tools pour Unity version 3.1 et ci-dessus pour IntelliSense prend en charge lors de l’utilisation de Winmd** pour Visual Studio 2015, cela peut être, consultez https://marketplace.visualstudio.com/items?itemName=SebastienLebreton.VisualStudio2015ToolsforUnity.  Pour Visual Studio 2017, le composant peut être ajouté à l’intérieur du programme d’installation de Visual Studio 2017.

**(3) Ouvrez un projet Unity nouveau ou existant**

**(4) commutateur la plateforme pour Xbox One dans le menu Paramètres de Build Unity**

**(5) activer IL2CPP de script principal dans les paramètres du lecteur Unity et défini la compatibilité d’API sur .NET 4.6**

![](../images/unity/unity-il2cpp-1.png)

**(6) le commutateur du compilateur de Script à Roslyn**

**(7), les bibliothèques système approprié Xbox One sont tous ajoutés automatiquement à votre projet, et aucune des étapes supplémentaires ne sont nécessaires pour inclure les fichiers binaires de plateforme.**

**(8) permet d’ajouter et joindre un nouveau C\# script à un objet de Unity.**

Par exemple, cliquez sur un objet Unity tels que « Main Camera », puis cliquez sur « Ajouter un composant » \| « Nouveau Script » \| C\# Script \| et nommez-le « XboxLiveScript ». N’importe quel objet de jeu fera l’affaire.

**9) Ouvrez le script dans Visual Studio (avec VSTU 3.1 + installé)**

Vous remarquerez deux projets, ouvrez votre script de jeu XboxLiveTest.cs dans le projet « Lecteur » généré par VSTU

![](../images/unity/unity-il2cpp-2.png)

Ceci est un projet spécifique est généré pour XDK et inclut des références pour les fichiers winmd que vous avez placés dans vos éléments multimédias.
Elle définit également la « #if ENABLE_WINMD_SUPPORT » définir pour vous pour IntelliSense et la coloration syntaxique fonctionne correctement.

**10) ajoutez le code de Xbox Live suivant au fichier source XboxLiveTest.cs**

```csharp

using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using System;

public class XboxLiveTest : MonoBehaviour
{
#if ENABLE_WINMD_SUPPORT
    Windows.Xbox.System.User mCurrentUser = null;
    XboxLiveContext mContext = null;
#endif

    // Use this for initialization
    void Start()
    {
#if ENABLE_WINMD_SUPPORT
        mCurrentUser = Windows.Xbox.ApplicationModel.Core.CoreApplicationContext.CurrentUser;
        mContext = new XboxLiveContext(mCurrentUser);
#endif
    }

    // Update is called once per frame
    void Update()
    {
    }

    void OnGUI()
    {
        if(mContext != null) Gui.TextArea(new Rect(10,10,50,200), mContext.XboxUserId);
    }
}

```

**(11) Vérifiez que vous avez fonctionnalité « InternetClient » sélectionnée dans les paramètres de publication trouvés dans les paramètres du lecteur**

![](../images/unity/unity-il2cpp-3.png)

**12) Générez le projet dans Unity.**
