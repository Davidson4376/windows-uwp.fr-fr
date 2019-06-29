---
Description: Le Microsoft Store applique certaines règles liées à des numéros de version, fonctionnent un peu différemment dans les différentes versions de système d’exploitation.
title: Numérotation des versions de packages
ms.assetid: DD7BAE5F-C2EE-44EE-8796-055D4BCB3152
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: cfab93064cf2fa2edd91798167a2c2fbc368ecb1
ms.sourcegitcommit: 4aef8c01ba9321401d5729a1ec6d46452ee76faf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/29/2019
ms.locfileid: "67468897"
---
# <a name="package-version-numbering"></a>Numérotation des versions de packages

Chaque package que vous fournissez doit avoir un numéro de version (fourni sous la forme d’une valeur dans l’attribut **Version** de l’élément [Package/Identity](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity) dans le manifeste de l’application). Le Microsoft Store applique certaines règles liées à des numéros de version, fonctionnent un peu différemment dans les différentes versions de système d’exploitation.

> [!NOTE]
> Cette rubrique fait référence à « packages », mais sauf mention, les mêmes règles s’appliquent à des numéros de version pour les fichiers.msix/.appx et.msixbundle/.appxbundle.


## <a name="version-numbering-for-windows10-packages"></a>Numérotation des versions pour les packages de Windows 10

> [!IMPORTANT]
> Pour les packages de Windows 10 (UWP), la dernière section (quatrième) du numéro de version est réservée pour une utilisation du Store et doit être laissée sur 0 lorsque vous générez votre package (bien que le Store peut changer la valeur de cette section). Les autres sections doivent être définies sur un entier compris entre 0 et 65535 (à l’exception de la première section, qui ne peut pas être 0).

Lors du choix d’un package UWP à partir de votre soumission publiée, le Microsoft Store utilisera toujours le package de contrôle de version le plus élevé qui s’applique aux appareils de Windows 10 du client. Cela vous offre une plus grande souplesse et vous permet de contrôler les packages fournis aux clients sur des types spécifiques d’appareils. Il est important de noter que vous pouvez soumettre ces packages dans n’importe quel ordre ; vous n’êtes pas obligé de fournir des packages dont le numéro de version est supérieur avec chaque soumission ultérieure.

Vous pouvez fournir plusieurs packages UWP avec le même numéro de version. Toutefois, les packages qui partagent un même numéro de version ne peuvent pas avoir la même architecture, car l’identité complète que le Windows Store utilise pour chaque package doit être unique. Pour plus d’informations, voir [**Identity**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity).

Lorsque vous fournissez plusieurs packages UWP qui utilisent le même numéro de version, l’architecture (dans l’ordre x64, x 86, ARM, neutre) servira à déterminer celle qui est de niveau supérieur (lorsque le Store détermine quel package pour fournir à l’appareil d’un client). Lors du classement des ensembles d’applications qui utilisent la même version, le niveau d’architecture le plus élevé dans l’ensemble est pris en considération : un ensemble d’applications contenant un package x64 aura un classement plus élevé qu’un ensemble contenant uniquement un package x86.

Cela vous offre une grande souplesse pour faire évoluer votre application au fil du temps. Vous pouvez télécharger et envoyer de nouveaux packages qui utilisent des numéros de version inférieure à ajouter la prise en charge pour les appareils Windows 10 que vous ne prenait en charge pas précédemment, vous pouvez ajouter des packages de version ultérieure qui ont des dépendances plus strictes pour tirer parti du matériel ou les fonctionnalités du système d’exploitation ou peut ajouter des packages de version ultérieure qui servent de base mises à jour à tout ou partie de votre client existant.

L’exemple suivant montre comment gérer la numérotation des versions pour livrer les packages voulus à vos clients au gré de soumissions multiples.

### <a name="example-moving-to-a-single-package-over-multiple-submissions"></a>Exemple : Déplacement vers un package unique sur plusieurs soumissions

Windows 10 vous permet d’écrire une seule codebase qui fonctionne partout. Cela facilite considérablement le démarrage d’un nouveau projet interplateforme. Toutefois, pour diverses raisons, il peut sembler inopportun de fusionner plusieurs codes base existants pour créer immédiatement un seul projet.

Vous pouvez utiliser les règles de contrôle de version de package pour déplacer progressivement vos clients à un package unique pour la famille de périphériques universelle, lors de la livraison des mises à jour intermédiaires pour les familles de périphériques spécifiques (y compris celles qui tirent parti des API de Windows 10). L’exemple ci-dessous illustre la façon dont les mêmes règles sont appliquées uniformément sur une série de soumissions pour la même application.

| Soumission | Contenu                                                  | Expérience utilisateur                                                                                                                                                                             |
|------------|-----------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 1          | -Version de package : 1.1.10.0 <br> -Famille d’appareils : Windows.Desktop, minVersion 10.0.10240.0 <br> <br> -Version de package : 1.1.0.0 <br> -Famille d’appareils : Windows.Mobile, minVersion 10.0.10240.0     | - Les appareils sous Windows 10 Desktop build 10.0.10240.0 et versions supérieures obtiendront le package 1.1.10.0 <br> - Les appareils sous Windows 10 Mobile build 10.0.10240.0 et versions supérieures obtiendront le package 1.1.0.0 <br> - Les autres familles d’appareils ne peuvent pas acheter et installer l’application |
| 2          | -Version de package : 1.1.10.0 <br> -Famille d’appareils : Windows.Desktop, minVersion 10.0.10240.0 <br> <br> -Version de package : 1.1.0.0 <br> -Famille d’appareils : Windows.Mobile, minVersion 10.0.10240.0 <br> <br> -Version de package : 1.0.0.0 <br> -Famille d’appareils : Windows.Universal, minVersion 10.0.10240.0    | - Les appareils sous Windows 10 Desktop build 10.0.10240.0 et versions supérieures obtiendront le package 1.1.10.0 <br> - Les appareils sous Windows 10 Mobile build 10.0.10240.0 et versions supérieures obtiendront le package 1.1.0.0 <br> - Les autres familles d’appareils (non de bureau, non mobiles) obtiendront le package 1.0.0.0 quand elles seront introduites <br> - Les appareils mobiles et de bureau sur lesquels l’application est déjà installée n’obtiendront aucune mise à jour (car ils ont déjà la meilleure version disponible : les versions 1.1.10.0 et 1.1.0.0 sont supérieures à la version 1.0.0.0) |
| 3          | -Version de package : 1.1.10.0 <br> -Famille d’appareils : Windows.Desktop, minVersion 10.0.10240.0 <br> <br> -Version de package : 1.1.5.0 <br> -Famille d’appareils : Windows.Universal, minVersion 10.0.10250.0 <br> <br> -Version de package : 1.0.0.0 <br> -Famille d’appareils : Windows.Universal, minVersion 10.0.10240.0    | - Les appareils sous Windows 10 Desktop build 10.0.10240.0 et versions supérieures obtiendront le package 1.1.10.0 <br> - Les appareils sous Windows 10 Mobile build 10.0.10250.0 et versions supérieures obtiendront le package 1.1.5.0 <br> - Les appareils sous Windows 10 Mobile build > 10.0.10240.0 et < 10.010250.0 obtiendront le package 1.1.0.0 
| 4          | -Version de package : 2.0.0.0 <br> -Famille d’appareils : Windows.Universal, minVersion 10.0.10240.0   | - Tous les clients sur toutes les familles d’appareils sous Windows 10 build v10.0.10240.0 et versions supérieures obtiendront le package 2.0.0.0 | 

> [!NOTE]
>  Dans tous les cas, les périphériques client reçoit le package qui a le numéro de version possible le plus élevé qui elles sont éligibles pour. Par exemple, dans la troisième soumission ci-dessus, tous les appareils de bureau recevront le package v1.1.10.0, même si leur système d’exploitation est de la version 10.0.10250.0 ou supérieure, et pourraient donc également accepter le package v1.1.5.0. Étant donné que le package 1.1.10.0 est celui dont le numéro de version est le plus élevé à leur disposition, ils obtiendront ce package.

### <a name="using-version-numbering-to-roll-back-to-a-previously-shipped-package-for-new-acquisitions"></a>Utilisation de la numérotation des versions pour revenir à un package livré précédemment pour de nouvelles acquisitions

Si vous conservez des copies de vos packages, vous aurez la possibilité de restaurer votre package de l’application dans le Store à un package de Windows 10 antérieur si vous devez détecter les problèmes liés à une mise en production. Il s’agit d’une façon temporaire afin de limiter les perturbations pour vos clients pendant que vous prenez le temps de résoudre le problème.

Pour ce faire, créez un nouveau [soumission](app-submissions.md). Supprimez le package problématique et chargez l’ancien package que vous souhaitez fournir dans le Windows Store. Les clients qui ont déjà reçu le package que vous restaurez auront toujours le package problématique (car votre ancien package aura un numéro de version antérieur). Cela n’empêchera cependant aucun autre utilisateur d’acquérir le package problématique, tout en permettant à l’application de rester disponible dans le Windows Store.

Pour résoudre ce problème pour les clients qui ont déjà reçu le package problématique, vous pouvez soumettre un nouveau package de Windows 10 qui possède un numéro de version supérieur à celle du package incorrect dès que possible. Une fois cette soumission certifiée, tous les clients seront mis à jour vers le nouveau package, car celui-ci aura un numéro de version supérieur.


## <a name="version-numbering-for-windows81-and-earlier-and-windows-phone-81-packages"></a>Version de numérotation pour Windows 8.1 (et versions antérieures) et les packages de Windows Phone 8.1

> [!IMPORTANT]
> À compter du 31 octobre 2018, produits nouvellement créé ne peut pas inclure les packages ciblant 8.x/Windows de Windows Phone 8.x ou une version antérieure. Pour plus d’informations, consultez ce [billet de blog](https://blogs.windows.com/windowsdeveloper/2018/08/20/important-dates-regarding-apps-with-windows-phone-8-x-and-earlier-and-windows-8-8-1-packages-submitted-to-microsoft-store).

Pour les packages .appx qui ciblent Windows Phone 8.1, le numéro de version du package dans une nouvelle soumission doit toujours être supérieur à celui du package inclus dans la dernière soumission (ou toute soumission précédente).

Pour les packages .appx qui ciblent Windows 8 et Windows 8.1, la même règle s’applique par architecture : le numéro de version du package dans une nouvelle soumission doit toujours être supérieur à celle du package dernière publié sur le Store pour la même architecture.

En outre, le numéro de version de Windows 8.1 packages doit toujours être supérieur aux numéros de version d’une de vos packages de Windows 8 pour la même application. En d’autres termes, le numéro de version de n’importe quel package de Windows 8 que vous envoyez doit être inférieur au numéro de version d’un package Windows 8.1 que vous avez envoyé pour la même application.

> [!NOTE]
> Si votre application a également des packages Windows 10, le numéro de version des packages Windows 10 doive être supérieur à ceux d’un de vos packages Windows 8, Windows 8.1 ou Windows Phone 8.1. Pour plus d’informations, consultez [Ajout de packages pour Windows 10 vers une application publiée précédemment](guidance-for-app-package-management.md#adding-packages-for-windows-10-to-a-previously-published-app).

Voici quelques exemples de ce qui se passe dans les scénarios de mise à jour numéro version différente pour les packages ciblant Windows 8 et Windows 8.1.

| Version de votre application dans le Store  | Version transférée | Une fois la nouvelle version présente dans le Store, elle sera installée dans une nouvelle acquisition. | Une fois la nouvelle version présente dans le Store, elle sera mise à jour si un client possède déjà l’application. |
|---------------------------------------------|-----------------------------|--------------------------------------------------------------------------------------------|----------|
| Rien                                     | x86, v1.0.0.0               | x86, v1.0.0.0 sur les ordinateurs x86 et x64                                                | Aucune. |
| x86, v1.0.0.0                               | x64, v1.0.0.0               | v1.0.0.0 pour l’architecture de l’ordinateur du client                                                   | Aucune. Les numéros de version sont les mêmes. |
| x86, v1.0.0.0 <br> x64, v1.0.0.0            | x64, v1.0.0.1               | v1.0.0.0 pour les clients possédant un ordinateur x86 <br> v1.0.0.1 pour les clients possédant un ordinateur x64                 | Rien pour les clients exécutant l’application sur un ordinateur x86. <br> Mise à jour de v1.0.0.0 vers v1.0.0.1 pour les clients exécutant l’application sur un ordinateur x64. <br> **Remarque**  si la version de l’application s’exécute sur un x64 de x86 ordinateur, l’application ne sont pas mis à jour à le x64 version, sauf si le client désinstalle et réinstalle. |
| Rien                                     | neutre, v1.0.0.1           | neutre, v1.0.0.1 sur tous les ordinateurs                                                         | Aucune. |
| neutre, v1.0.0.1                           | x86, v1.0.0.0 <br> x64, v1.0.0.0 <br> ARM, v1.0.0.0 | v1.0.0.0 pour l’architecture de l’ordinateur du client.          | Aucune. Les clients qui possèdent la version neutre v1.0.0.1 de l’application continuent de l’utiliser. |
| neutre, v1.0.0.1 <br> x86, v1.0.0.0 <br> x64, v1.0.0.0 <br> ARM, v1.0.0.0 | x86, v1.0.0.1 <br> x64, v1.0.0.1 <br> ARM, v1.0.0.1 | v1.0.0.1 pour l’architecture de l’ordinateur du client. | Rien pour les clients qui exécutent la version neutre v1.0.0.1 de l’application. <br> Mise à jour de v1.0.0.0 vers v1.0.0.1 pour les clients exécutant la version v1.0.0.0 de l’application générée pour l’architecture de leur ordinateur. |
| x86, v1.0.0.1 <br> x64, v1.0.0.1 <br> ARM, v1.0.0.1 | x86, v1.0.0.2 <br> x64, v1.0.0.2 <br> ARM, v1.0.0.2 | v1.0.0.2 pour l’architecture de l’ordinateur du client.  | Mise à jour de v1.0.0.1 vers v1.0.0.2 pour les clients exécutant la version v1.0.0.1 de l’application générée pour l’architecture de leur ordinateur. |

> [!NOTE]
> À la différence des packages .appx, les numéros de version dans les packages .xap ne sont pas pris en compte lors de la détermination du package à fournir à un client donné. Pour mettre à jour un client d’un package .xap vers une version plus récente, veillez à supprimer l’ancien .xap dans la nouvelle soumission.
