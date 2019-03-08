---
title: Ajout de Xbox Live API binaire à un projet UWP
description: Apprenez à utiliser NuGet pour ajouter le package binaire Xbox Live API à votre projet UWP.
ms.assetid: 1e77ce9f-8a0e-402c-9f46-e37f9cda90ed
ms.date: 11/28/2017
ms.topic: article
keywords: Xbox live, xbox, jeux, uwp, windows 10, xbox une, nuget
ms.localizationpriority: medium
ms.openlocfilehash: 20b4d4ae27282ccf71964d31da4d1f577c280de8
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57593944"
---
# <a name="add-xbox-live-apis-binary-package-to-your-uwp-project"></a>Ajouter le package binaire Xbox Live API à votre projet UWP

## <a name="requirements"></a>Configuration requise

2. **[Windows 10](https://microsoft.com/windows)**.
3. **[Visual Studio](https://www.visualstudio.com/)**. Les applications UWP peuvent être générées avec Visual Studio 2015 Update 3 ou version ultérieure. Nous vous recommandons d’utiliser la dernière version de Visual Studio pour les mises à jour de sécurité et de développement.
4. **[SDK Windows 10](https://developer.microsoft.com/windows/downloads/windows-10-sdk) v10.0.10586.0** ou version ultérieure.

## <a name="add-the-binary-package-via-nuget"></a>Ajoutez le package binaire via NuGet

Pour utiliser l’API de Xbox Live à partir de votre projet, vous pouvez soit ajouter des références aux fichiers binaires à l’aide de packages NuGet ou ajout de la source de l’API. L'ajout de packages NuGet rend la compilation plus rapide, alors que l’ajout de la source facilite le débogage. Cet article vous guide lors de l’utilisation de packages NuGet. Si vous souhaitez utiliser la source, puis consultez [compilation le Xbox Live API Source dans votre projet UWP](add-xbox-live-apis-source-to-a-uwp-project.md).

L’API de Services Xbox est fourni dans les versions pour UWP et XDK et pour C++ et WinRT et avait leur espace de noms sont structurées sous la forme **Microsoft.Xbox.Live.SDK.*. UWP** et **Microsoft.Xbox.Live.SDK.*. XboxOneXDK**.

1. **UWP** est destiné aux développeurs qui créent un jeu UWP, ce qui peut s’exécuter sur des PC, Xbox One ou Windows Phone.
2. **XboxOneXDK** concerne ID@Xbox et géré les développeurs qui utilisent le XDK une Xbox.
3. Le SDK de C++ peut être utilisé pour les moteurs de jeux de C++, alors que le SDK WinRT est pour les moteurs de jeux écrits avec C++, C#, ou JavaScript.
4. Lorsque vous utilisez WinRT avec un moteur de C++, vous devez utiliser C++ / c++ / CX qui utilise des rôles (^). C++ est l’API recommandée à utiliser pour les moteurs de jeux de C++.  

> [!TIP]
> Vous trouverez plus d’informations sur l’exécution UWP sur Xbox One à [mise en route du développement d’applications UWP sur Xbox One](https://docs.microsoft.com/windows/uwp/xbox-apps/getting-started).

Vous pouvez ajouter le package NuGet de kit de développement logiciel Xbox Live par :

1. Dans Visual Studio, accédez à **outils** > **Gestionnaire de Package NuGet** > **gérer les Packages NuGet pour la Solution...** .
2. Dans le Gestionnaire de package NuGet, cliquez sur **Parcourir** et entrez **Xbox.Live.SDK** dans la zone de recherche.
3. Sélectionnez la version de Xbox Live SDK que vous souhaitez utiliser dans la liste sur la gauche.
3. Sur le côté droit de la fenêtre, cochez la case en regard de votre projet et cliquez sur **installer**.

> [!NOTE]
> Les développeurs de programme Xbox Live Creators doivent utiliser une des versions du SDK Xbox Live, UWP XDK n’est pas prise en charge.

![Ajouter XBL via NuGet](../images/getting_started/vs-add-nuget-xbl.gif)

> [!IMPORTANT]
> Pour `Microsoft.Xbox.Live.SDK.Cpp.*` projets, assurez-vous d’inclure l’en-tête `#include <xsapi\services.h>` dans la source de votre projet.
