---
author: jnHs
Description: The Microsoft Store enforces certain rules related to version numbers, which work somewhat differently in different OS versions.
title: Numérotation des versions de packages
ms.assetid: DD7BAE5F-C2EE-44EE-8796-055D4BCB3152
ms.author: wdg-dev-content
ms.date: 10/25/2018
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 045e44d3ba243b4c4fbf68ba37e91c06eb022d27
ms.sourcegitcommit: 753e0a7160a88830d9908b446ef0907cc71c64e7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/29/2018
ms.locfileid: "5741683"
---
# <a name="package-version-numbering"></a>Numérotation des versions de packages

Chaque package que vous fournissez doit avoir un numéro de version (fourni sous la forme d’une valeur dans l’attribut **Version** de l’élément [Package/Identity](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity) dans le manifeste de l’application). Le MicrosoftStore applique certaines règles relatives aux numéros de version, qui fonctionnent différemment dans les différentes versions du système d’exploitation.

> [!NOTE]
> Cette rubrique fait référence aux «packages», mais, sauf indication contraire, les mêmes règles s’appliquent aux numéros de version des fichiers.msix/.appx et.msixbundle/.appxbundle.


## <a name="version-numbering-for-windows10-packages"></a>Numérotation des versions pour les packages Windows 10

> [!IMPORTANT]
> Pour les packages Windows 10 (UWP), la dernière (quatrième) partie du numéro de version est réservée au Windows Store et doive conserver la valeur 0 lorsque vous générez votre package (bien que le Windows Store puisse modifier la valeur de cette section). Les autres sections doivent être définies sur un nombre entier compris entre 0 et 65 535 (à l’exception de la première section, qui ne peut pas être 0).

Lors du choix d’un package UWP à partir de votre soumission publiée, le Microsoft Store utilise toujours le package présentant la version la plus élevée applicable à l’appareil du client Windows 10. Cela vous offre une plus grande souplesse et vous permet de contrôler les packages fournis aux clients sur des types spécifiques d’appareils. Il est important de noter que vous pouvez soumettre ces packages dans n’importe quel ordre ; vous n’êtes pas obligé de fournir des packages dont le numéro de version est supérieur avec chaque soumission ultérieure.

Vous pouvez fournir plusieurs packages UWP avec le même numéro de version. Toutefois, les packages qui partagent un même numéro de version ne peuvent pas avoir la même architecture, car l’identité complète que le Windows Store utilise pour chaque package doit être unique. Pour plus d’informations, voir [**Identity**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity).

Lorsque vous fournissez plusieurs packages UWP qui utilisent le même numéro de version, l’architecture (dans l’ordre x64, x 86, ARM, neutre) est utilisée pour déterminer celui qui est de niveau supérieur (lorsque le Windows Store détermine le package à fournir à un appareil d’un client). Lors du classement des ensembles d’applications qui utilisent la même version, le niveau d’architecture le plus élevé dans l’ensemble est pris en considération : un ensemble d’applications contenant un package x64 aura un classement plus élevé qu’un ensemble contenant uniquement un package x86.

Cela vous offre une grande souplesse pour faire évoluer votre application au fil du temps. Vous pouvez charger et soumettre de nouveaux packages qui utilisent des numéros de version inférieurs pour prendre en charge pour les appareils Windows 10 qui vous précédemment ne gérait pas, vous pouvez ajouter des packages de version supérieure comprenant des dépendances plus strictes pour tirer parti de matériel ou les fonctionnalités du système d’exploitation ou vous peut ajouter des packages de version supérieure qui servent de base mises à jour pour tout ou partie de vos clients existants.

L’exemple suivant montre comment gérer la numérotation des versions pour livrer les packages voulus à vos clients au gré de soumissions multiples.

### <a name="example-moving-to-a-single-package-over-multiple-submissions"></a>Exemple : évolution vers un package unique en plusieurs soumissions

Windows 10 vous permet d’écrire un seul code base qui s’exécute partout. Cela facilite considérablement le démarrage d’un nouveau projet interplateforme. Toutefois, pour diverses raisons, il peut sembler inopportun de fusionner plusieurs codes base existants pour créer immédiatement un seul projet.

Vous pouvez utiliser les règles de contrôle de version de package pour amener progressivement vos clients vers un package unique pour la famille d’appareils universelle, tout en envoyant un certain nombre de mises à jour intermédiaires pour des familles (y compris ceux qui tirent parti des API Windows 10). L’exemple suivant illustre la façon dont les mêmes règles sont appliquées de façon cohérente sur une série de soumissions pour la même application.

| Soumission | Contenu                                                  | Expérience utilisateur                                                                                                                                                                             |
|------------|-----------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 1          | - Version de package: 1.1.10.0 <br> - Famille d’appareils: Windows.Desktop, minVersion 10.0.10240.0 <br> <br> - Version de package: 1.1.0.0 <br> - Famille d’appareils: Windows.Mobile, minVersion 10.0.10240.0     | - Les appareils sous Windows10 Desktop build 10.0.10240.0 et versions supérieures obtiendront le package 1.1.10.0 <br> - Les appareils sous Windows10 Mobile build 10.0.10240.0 et versions supérieures obtiendront le package 1.1.0.0 <br> - Les autres familles d’appareils ne peuvent pas acheter et installer l’application |
| 2          | - Version de package: 1.1.10.0 <br> - Famille d’appareils: Windows.Desktop, minVersion 10.0.10240.0 <br> <br> - Version de package: 1.1.0.0 <br> - Famille d’appareils: Windows.Mobile, minVersion 10.0.10240.0 <br> <br> - Version de package: 1.0.0.0 <br> - Famille d’appareils: Windows.Universal, minVersion 10.0.10240.0    | - Les appareils sous Windows10 Desktop build 10.0.10240.0 et versions supérieures obtiendront le package 1.1.10.0 <br> - Les appareils sous Windows10 Mobile build 10.0.10240.0 et supérieure obtiendront le package 1.1.0.0 <br> - Les autres familles d’appareils (non de bureau, non mobiles) obtiendront le package 1.0.0.0 quand elles seront introduites <br> - Les appareils mobiles et de bureau sur lesquels l’application est déjà installée n’obtiendront aucune mise à jour (car ils ont déjà la meilleure version disponible: les versions 1.1.10.0 et 1.1.0.0 sont supérieures à la version 1.0.0.0) |
| 3          | - Version de package: 1.1.10.0 <br> - Famille d’appareils: Windows.Desktop, minVersion 10.0.10240.0 <br> <br> - Version de package: 1.1.5.0 <br> - Famille d’appareils: Windows.Universal, minVersion 10.0.10250.0 <br> <br> - Version de package: 1.0.0.0 <br> - Famille d’appareils: Windows.Universal, minVersion 10.0.10240.0    | - Les appareils sous Windows10 Desktop build 10.0.10240.0 et versions supérieures obtiendront le package 1.1.10.0 <br> - Les appareils sous Windows10 Mobile build 10.0.10250.0 et versions supérieures obtiendront le package 1.1.5.0 <br> - Les appareils sous Windows10 Mobile build > 10.0.10240.0 et < 10.010250.0 obtiendront le package 1.1.0.0 
| 4          | - Version de package: 2.0.0.0 <br> - Famille d’appareils: Windows.Universal, minVersion 10.0.10240.0   | - Tous les clients sur toutes les familles d’appareils sous Windows10 build v10.0.10240.0 et versions supérieures obtiendront le package 2.0.0.0 | 

> [!NOTE]
>  Dans tous les cas, les appareils clients recevront le package associé au numéro de version le plus élevé possible pour lequel ils sont éligibles. Par exemple, dans la troisième soumission ci-dessus, tous les appareils de bureau recevront le package v1.1.10.0, même si leur système d’exploitation est de la version 10.0.10250.0 ou supérieure, et pourraient donc également accepter le package v1.1.5.0. Étant donné que le package 1.1.10.0 est celui dont le numéro de version est le plus élevé à leur disposition, ils obtiendront ce package.

### <a name="using-version-numbering-to-roll-back-to-a-previously-shipped-package-for-new-acquisitions"></a>Utilisation de la numérotation des versions pour revenir à un package livré précédemment pour de nouvelles acquisitions

Si vous conservez des copies de vos packages, vous avez la possibilité de restaurer le package de votre application dans le Windows Store à un version antérieure du package Windows 10 si vous devez identifier les problèmes avec une version. Il s’agit d’un moyen temporaire de limiter l’interruption pour vos clients pendant que vous prenez le temps de résoudre le problème.

Pour ce faire, créez une nouvelle [soumission](app-submissions.md). Supprimez le package problématique et chargez l’ancien package que vous souhaitez fournir dans le Windows Store. Les clients qui ont déjà reçu le package que vous restaurez auront toujours le package problématique (car votre ancien package aura un numéro de version antérieur). Cela n’empêchera cependant aucun autre utilisateur d’acquérir le package problématique, tout en permettant à l’application de rester disponible dans le Windows Store.

Pour résoudre le problème pour les clients qui ont déjà reçu le package problématique, vous pouvez soumettre un nouveau package Windows 10 qui dispose d’un numéro de version supérieur celui du package incorrect dès que possible. Une fois cette soumission certifiée, tous les clients seront mis à jour vers le nouveau package, car celui-ci aura un numéro de version supérieur.


## <a name="version-numbering-for-windows81-and-earlier-and-windows-phone-81-packages"></a>Numérotation des versions Windows8.1 (ou version antérieure) et des packages Windows Phone 8.1

Pour les packages .appx qui ciblent Windows Phone 8.1, le numéro de version du package dans une nouvelle soumission doit toujours être supérieur à celui du package inclus dans la dernière soumission (ou toute soumission précédente).

Pour les packages .appx qui ciblent package Windows8 et Windows8.1, la même règle s’applique par architecture: le numéro de version du package dans une nouvelle soumission doit toujours être supérieur à celui du package publié en dernier dans le Windows Store pour la même architecture.

En outre, le numéro de version des packages Windows8.1 doit toujours être supérieur aux numéros de version d’un de vos packages de package Windows8 pour la même application. En d’autres termes, le numéro de version d’un package de package Windows8 que vous soumettez doit être inférieur au numéro de version d’un package de Windows8.1 que vous avez soumis pour la même application.

> [!NOTE]
> Si votre application possède également des packages Windows 10, le numéro de version des packages Windows 10 doit être supérieur à ceux aucun de vos packages package Windows8, Windows8.1 et/ou Windows Phone 8.1. Pour plus d’informations, voir [Ajout des packages pour Windows 10 vers une application publiée précédemment](guidance-for-app-package-management.md#adding-packages-for-windows-10-to-a-previously-published-app).

Voici quelques exemples de ce qui se passe dans les scénarios de mise à jour de numéro de version différent pour les packages ciblant package Windows8 et Windows8.1.

| Version de votre application dans le Store  | Version transférée | Une fois la nouvelle version présente dans le Store, elle sera installée dans une nouvelle acquisition. | Une fois la nouvelle version présente dans le Store, elle sera mise à jour si un client possède déjà l’application. |
|---------------------------------------------|-----------------------------|--------------------------------------------------------------------------------------------|----------|
| Rien                                     | x86, v1.0.0.0               | x86, v1.0.0.0 sur les ordinateurs x86 et x64                                                | Rien |
| x86, v1.0.0.0                               | x64, v1.0.0.0               | v1.0.0.0 pour l’architecture de l’ordinateur du client                                                   | Rien Les numéros de version sont les mêmes. |
| x86, v1.0.0.0 <br> x64, v1.0.0.0            | x64, v1.0.0.1               | v1.0.0.0 pour les clients possédant un ordinateur x86 <br> v1.0.0.1 pour les clients possédant un ordinateur x64                 | Rien pour les clients exécutant l’application sur un ordinateur x86. <br> Mise à jour de v1.0.0.0 vers v1.0.0.1 pour les clients exécutant l’application sur un ordinateur x64. <br> **Remarque**si la version de l’application s’exécute sur un x64 de x86 ordinateur, l’application ne sont pas mis à jour à le x64 version, sauf si le client effectue une désinstallation puis une réinstallation. |
| Rien                                     | neutre, v1.0.0.1           | neutre, v1.0.0.1 sur tous les ordinateurs                                                         | Rien |
| neutre, v1.0.0.1                           | x86, v1.0.0.0 <br> x64, v1.0.0.0 <br> ARM, v1.0.0.0 | v1.0.0.0 pour l’architecture de l’ordinateur du client.          | Rien Les clients qui possèdent la version neutre v1.0.0.1 de l’application continuent de l’utiliser. |
| neutre, v1.0.0.1 <br> x86, v1.0.0.0 <br> x64, v1.0.0.0 <br> ARM, v1.0.0.0 | x86, v1.0.0.1 <br> x64, v1.0.0.1 <br> ARM, v1.0.0.1 | v1.0.0.1 pour l’architecture de l’ordinateur du client. | Rien pour les clients qui exécutent la version neutre v1.0.0.1 de l’application. <br> Mise à jour de v1.0.0.0 vers v1.0.0.1 pour les clients exécutant la version v1.0.0.0 de l’application générée pour l’architecture de leur ordinateur. |
| x86, v1.0.0.1 <br> x64, v1.0.0.1 <br> ARM, v1.0.0.1 | x86, v1.0.0.2 <br> x64, v1.0.0.2 <br> ARM, v1.0.0.2 | v1.0.0.2 pour l’architecture de l’ordinateur du client.  | Mise à jour de v1.0.0.1 vers v1.0.0.2 pour les clients exécutant la version v1.0.0.1 de l’application générée pour l’architecture de leur ordinateur. |

> [!NOTE]
> À la différence des packages.appx, les numéros de version dans les packages.xap ne sont pas pris en compte lors de la détermination du package à fournir à un client donné. Pour mettre à jour un client d’un package .xap vers une version plus récente, veillez à supprimer l’ancien .xap dans la nouvelle soumission.
