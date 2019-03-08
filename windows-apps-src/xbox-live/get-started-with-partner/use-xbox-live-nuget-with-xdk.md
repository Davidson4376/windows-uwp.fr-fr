---
title: Utiliser le package de Xbox Live NuGet avec le XDK
description: Découvrez comment utiliser le package NuGet de l’API Xbox Live pour développer des titres XDK.
ms.assetid: 2c5ae514-393d-48bb-afd8-a897d35f7938
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, jeux, uwp, windows 10, xbox un, NuGet
ms.localizationpriority: medium
ms.openlocfilehash: c955ca42c09075e5125683588c335cfa47443f00
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57655034"
---
# <a name="use-the-xbox-live-api-nuget-package-to-develop-xdk-titles"></a>Utiliser le package NuGet de l’API Xbox Live pour développer des titres XDK

### <a name="1--ensure-you-have-the-latest-nuget-package-manager-installed"></a>1.  Assurez-vous de qu'avoir le dernier gestionnaire de Package NuGet installé
1.  Vérifier votre version actuelle :
    - Sur la barre de menus, sélectionnez Outils -> Extensions et mises à jour.
    - Sous l’onglet installé, recherchez `NuGet Package Manager`
![](../images/nuget/nuget_uwp_install_1.png)
2.  Pour mettre à jour votre version actuelle :
    - Sur la barre de menus, sélectionnez Outils -> Extensions et mises à jour.
    - Sous les mises à jour -> onglet de la galerie Visual Studio, sélectionnez `Update`
![](../images/nuget/nuget_uwp_install_2.png)

### <a name="2--add-reference-to-the-project"></a>2.  Ajouter une référence au projet
1.  Ajouter une référence au projet
    1.  Cliquez avec le bouton droit sur votre solution de projet et sélectionnez « Gérer les Packages NuGet »
<br/>
![](../images/nuget/nuget_xbox_install_4.png)
1.  Recherchez `Xbox Live` et sélectionnez le package approprié, puis cliquez sur `Install`.
  - L’API de Services Xbox est fourni dans les versions pour UWP et XDK et pour C++ et WinRT.  
  - Choisissez entre `Microsoft.Xbox.Live.SDK.*.UWP` et `Microsoft.Xbox.Live.SDK.*.XboxOneXDK`.  `XboxOneXDK` concerne ID@Xbox et géré les développeurs qui utilisent le XDK une Xbox.  `UWP` concerne les jeux, UWP qui peuvent s’exécuter sur des PC, Xbox One ou Windows Phone.  Vous trouverez plus d’informations sur l’exécution UWP sur Xbox One à [https://docs.microsoft.com/en-us/windows/uwp/xbox-apps/getting-started](https://docs.microsoft.com/en-us/windows/uwp/xbox-apps/getting-started)
  - Choisissez entre `Microsoft.Xbox.Live.SDK.Cpp.*` et `Microsoft.Xbox.Live.SDK.WinRT.*`. `Cpp` concerne les moteurs de jeux C++ à l’aide de l’API Live Xbox.  `WinRT` pour les moteurs de jeux écrits avec C++, C#, ou Javascript à l’aide de l’API Live Xbox.  Lorsque vous utilisez WinRT avec un moteur de C++, vous utiliseriez C + c++ / CX qui utilise des rôles (^).  `Cpp` est l’API recommandée à utiliser pour les moteurs de jeux de C++.    
![](../images/nuget/nuget_xbox_install_5.png)
![](../images/nuget/nuget_uwp_install_7.png)
1. Après avoir accepté le type de service de licence, patientez jusqu'à ce que le package a été ajouté avec succès.  Vous devez voir ce journal dans la fenêtre de sortie du Gestionnaire de Package :

```
========== Finished ==========
```

### <a name="3--optionally-include-header"></a>3.  Si vous le souhaitez inclure en-tête
* Pour `Microsoft.Xbox.Live.SDK.Cpp.*` projets `#include <xsapi\services.h>` dans la source de votre projet.
* Pour `Microsoft.Xbox.Live.SDK.WinRT.*` en fonction des projets, inutile d’inclure des en-têtes.   
