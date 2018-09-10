---
author: jnHs
Description: Learn how your app's packages are made available to your customers, and how to manage specific package scenarios.
title: Aide sur la gestion des packages d’application
ms.assetid: 55405D0B-5C1E-43C8-91A1-4BFDD336E6AB
ms.author: wdg-dev-content
ms.date: 03/28/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 9b0b6315b1177138c3ede7834e2dbc792ee106dd
ms.sourcegitcommit: f5cf806a595969ecbb018c3f7eea86c7a34940f6
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/10/2018
ms.locfileid: "3823083"
---
# <a name="guidance-for-app-package-management"></a>Aide sur la gestion des packages d’application

Découvrez comment les packages de votre application sont mis à la disposition de vos clients, et comment gérer des scénarios de package spécifiques.

-   [Versions de système d’exploitation et distribution de package](#os-versions-and-package-distribution)
-   [Ajout de packages pour Windows 10 à une application publiée précédemment](#adding-packages-for-windows-10-to-a-previously-published-app)
-   [Maintien de la compatibilité du package pour Windows Phone 8.1](#maintaining-package-compatibility-for-windows-phone-81)
-   [Suppression d'une application du Windows Store](#removing-an-app-from-the-store)
-   [Suppression de packages pour une famille d'appareils précédemment prise en charge](#removing-packages-for-a-previously-supported-device-family)


## <a name="os-versions-and-package-distribution"></a>Versions de système d'exploitation et distribution de package

Les différents systèmes d’exploitation peuvent exécuter différents types de packages. Si plusieurs de vos packages peuvent s’exécuter sur l’appareil d’un client, le MicrosoftStore fournit la meilleure correspondance disponible.

En règle générale, les systèmes d’exploitation plus récents peuvent exécuter des packages ciblant des versions antérieures pour la même famille d’appareils. Toutefois, elles n’obtiennent ces packages si l’application n’inclut pas un package ciblant sa version du système d’exploitation en cours.

Par exemple, les appareils Windows 10 peuvent exécuter toutes les versions antérieures du système d’exploitation prises en charge (par famille d’appareils). Les appareils de bureau Windows 10 peuvent exécuter des applications conçues pour Windows 8.1 ou Windows 8. Les appareils mobiles Windows 10 peuvent exécuter des applications conçues pour Windows Phone 8.1, Windows Phone 8, voire Windows Phone 7.x. 

Les exemples suivants illustrent divers scénarios pour une application qui comprend des packages ciblant différentes versions du système d’exploitation (sauf si des contraintes spécifiques de vos packages n'autorisent pas leur exécution sur chaque type de périphérique ou de version du système d’exploitation répertorié ici. Par exemple, le package architecture doit être adaptée à l’appareil). 

### <a name="example-app-1"></a>Exemple d’application1

| Système d’exploitation ciblé du package | Systèmes d’exploitation qui obtiendront ce package |
|-------------------------------------|----------------------------------------------|
| Windows 8.1                         | Appareils de bureau Windows 10, Windows 8.1      |
| Windows Phone 8.1                   | Appareils mobiles Windows 10, Windows Phone 8.1 |
| Windows Phone 8                     | Windows Phone 8                              |
| Windows Phone 7.1                   | Windows Phone 7.x                            |

Dans l’exemple 1, l’application n’a pas encore de packages UWP spécialement conçus pour les appareils Windows 10, mais les clients utilisant Windows 10 peuvent toujours obtenir l’application. Ces clients reçoivent les meilleurs packages disponibles pour leur type d’appareil.

### <a name="example-app-2"></a>Exemple d’application2

| Système d’exploitation ciblé du package  | Systèmes d’exploitation qui obtiendront ce package |
|--------------------------------------|----------------------------------------------|
| Windows 10 (famille d’appareils universelle) | Windows 10 (toutes familles d’appareils)             |
| Windows 8.1                          | Windows 8.1                                  |
| Windows Phone 8.1                    | Windows Phone 8.1                            |
| Windows Phone 7.1                    | Windows Phone 7.x, Windows Phone 8           |

Dans l’exemple2, aucun package ne peut s’exécuter sur Windows8. Les clients qui exécutent une autre version du système d'exploitation peuvent télécharger l'application. Tous les clients sous Windows10 obtiennent le même package.

### <a name="example-app-3"></a>Exemple d’application 3

| Système d’exploitation ciblé du package | Systèmes d’exploitation qui obtiendront ce package                  |
|-------------------------------------|---------------------------------------------------------------|
| Windows 10 (famille d’appareils de bureau)  | Appareils de bureau Windows 10                                    |
| Windows Phone 8                     | Appareils mobiles Windows 10, Windows Phone 8, Windows Phone 8.1 |

Dans l’exemple 3, puisqu’il n’existe aucun package UWP ciblant la famille d’appareils mobiles, les clients utilisant des appareils mobiles Windows 10 reçoivent le package Windows Phone 8. Si cette application ajoute ultérieurement un package ciblant la famille d’appareils mobiles (ou la famille d’appareils universelle), ce package sera disponible pour les clients utilisant des appareils mobiles Windows10 à la place du package Windows Phone8.

Notez également que cet exemple d’application n’inclut aucun package pouvant s’exécuter sur Windows Phone7.x.

### <a name="example-app-4"></a>Exemple d’application 4

| Système d’exploitation ciblé du package  | Systèmes d’exploitation qui obtiendront ce package |
|--------------------------------------|----------------------------------------------|
| Windows 10 (famille d’appareils universelle) | Windows 10 (toutes les familles d’appareils)             |

Dans l’exemple 4, tout appareil exécutant Windows 10 peut obtenir l’application, mais celle-ci ne sera pas disponible pour les clients utilisant une version antérieure du système d’exploitation. Étant donné que le package UWP cible la famille d’appareils universelle, il sera disponible pour n’importe quel appareil Windows 10 (par vos [sélections de disponibilité de la famille d’appareils de périphérique](device-family-availability.md)).


## <a name="removing-an-app-from-the-store"></a>Suppression d'une application du Store

Parfois, il est possible que vous souhaitiez arrêter de fournir une application à vos clients, « annuler » sa publication. Pour ce faire, cliquez sur **Rendre votre application indisponible** sur la page **Vue d’ensemble de l’application**. Quelques heures après que vous avez confirmé vouloir la rendre indisponible, votre application disparaît du Store. Dès lors, aucun nouveau client ne peut plus y accéder (sauf s'il possède un [code promotionnel](generate-promotional-codes.md) et utilise un appareil Windows10).

> [!IMPORTANT]
> Avec cette option, les paramètres de [visibilité](choose-visibility-options.md#discoverability) sélectionnés dans vos soumissions seront remplacés. 

Cette option a le même effet que si vous avez créé une soumission et choisi l’option **Rendre ce produit disponible mais non détectable dans le Store** avec l’option **Empêcher l'acquisition**. Toutefois, elle ne vous oblige pas à créer une nouvelle soumission.

Notez que les clients ayant déjà l’application pourront encore l’utiliser et la retélécharger (et même recevoir des mises à jour si vous envoyez de nouveaux packages ultérieurement).

Une application rendue indisponible continue à s'afficher sur votre tableau de bord. Si vous décidez de la remettre à disposition des clients, vous pouvez cliquer sur **Rendre votre application indisponible** sur la page Vue d’ensemble de l’application. L’application est mise à disposition des nouveaux clients (sauf paramétrage contraire dans votre dernière soumission) dans les heures suivant votre confirmation.

> [!NOTE]
> Si vous souhaitez que votre application reste disponible, mais voulez arrêter de la proposer aux nouveaux clients sur une version spécifique de système d’exploitation, vous pouvez créer une autre soumission et supprimer tous les packages associés à la version de système d’exploitation pour laquelle vous souhaitez empêcher toute nouvelle acquisition. Par exemple, si vous possédiez auparavant des packages pour Windows Phone8.1 et Windows10 et ne souhaitez pas continuer à proposer l’application aux nouveaux clients sur Windows Phone8.1, supprimez tous vos packages Windows Phone8.1 de la soumission. Une fois la mise à jour publiée, aucun nouveau client sous Windows Phone8.1 ne peut acquérir l’application, bien que les clients qui la possèdent continuent d'en bénéficier. L'application reste cependant disponible pour les nouveaux clients sous Windows10.


## <a name="removing-packages-for-a-previously-supported-device-family"></a>Suppression de packages pour une famille d'appareils précédemment prise en charge

Si vous supprimez tous les packages pour une certaine [famille d’appareils](https://docs.microsoft.com/uwp/extension-sdks/device-families-overview) que votre application précédemment pris en charge, vous serez invité à confirmer qu’il s’agit votre intention avant d’enregistrer vos modifications sur la page **Packages** .

Lorsque vous publiez une soumission qui supprime tous les packages qui peuvent s’exécuter sur une famille d’appareils précédemment prise en charge par votre application, les nouveaux clients ne sera pas en mesure d’acquérir l’application sur cette famille. Vous pouvez toujours publier une autre mise à jour pour proposer de nouveau des packages pour cette famille d'appareils.

Gardez à l'esprit que même si vous supprimez tous les packages prenant en charge une certaine famille d'appareils, tous les clients existants ayant déjà installé l'application sur ce type d'appareil pourra encore l'utiliser et obtenir les mises à jour que vous proposerez ultérieurement.


<a name="adding-packages-for-windows-10-to-a-previously-published-app"></a>

## <a name="adding-packages-for-windows-10-to-a-previously-published-app"></a>Ajout de packages pour Windows 10 à une application publiée précédemment

Si vous avez une application dans le Windows Store qui cible Windows 8.x et/ou Windows Phone 8.x, et que vous voulez mettre à jour votre application pour Windows 10, créez une soumission et ajoutez votre ou vos packages UWP .appxupload pendant l’étape [Packages](upload-app-packages.md). Une fois que votre application passe en certification, les clients ayant déjà doté de votre application et sont désormais sur Windows 10 obtiendront votre package UWP comme une mise à jour à partir du Store. Le package UWP sera également disponible pour les nouvelles acquisitions effectuées par les clients sur Windows 10.

> [!NOTE]
> Une fois qu’un client sur Windows10 obtient votre package UWP, vous ne pouvez pas le faire revenir à un package conçu pour une version antérieure du système d’exploitation. 

Notez que le numéro de version de vos packages Windows10 doit être supérieur à celui des packages Windows8, Windows8.1 et/ou WindowsPhone8.1 que vous incluez (ou que vous avez déjà publiés). Pour plus d’informations, voir [Numérotation des versions de packages](package-version-numbering.md).

Pour plus d’informations sur la création de packages d’applicationsUWP pour le Store, voir [Création de packages d’applications](../packaging/index.md).

> [!IMPORTANT]
> N’oubliez pas que si vous fournissez des packages ciblant la famille d’appareils universelle, chaque client déjà doté de votre application sur un système d’exploitation antérieur (WindowsPhone8, Windows8.1, etc.) et qui procède à une mise à niveau vers Windows10 bénéficiera d’une mise à jour vers votre package Windows10.
> 
> Cela se produit même si vous avez exclu une famille d’appareils spécifique à l’étape de [la disponibilité de la famille d’appareils de périphérique](device-family-availability.md) de votre soumission, depuis que section s’applique uniquement aux nouvelles acquisitions. Si vous ne voulez pas que chaque client antérieur obtienne votre package Windows10 universel, veillez à mettre à jour l’élément [**TargetDeviceFamily**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-targetdevicefamily) dans votre manifeste appx afin d’inclure uniquement la famille d’appareils spécifique que vous voulez prendre en charge.
> 
> Par exemple, supposons que vous souhaitez que vos clients Windows 8 et Windows 8.1 qui ont mis à niveau vers un appareil de bureau Windows 10 pour obtenir votre nouvelle application UWP, mais vous souhaitez que les clients Windows Phone qui sont désormais sur les appareils Windows 10 Mobile conservent les packages que vous le feriez précédemment apportées coefficient e (ciblant Windows Phone 8 ou Windows Phone 8.1). Pour ce faire, vous devez mettre à jour le [**TargetDeviceFamily**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-targetdevicefamily) dans votre manifeste appx afin d’inclure uniquement les **Windows.Desktop** (pour la famille d’appareils de bureau), au lieu de laisser la valeur **Windows.Universal** (pour la famille d’appareils universelle) Microsoft Visual Studio inclut dans le manifeste par défaut. Ne soumettez pas de packages UWP ciblant la famille d’appareils universelle ou la famille d’appareils mobiles (**Windows.Universal** or **Windows.Universal**). Ainsi, vos clients Windows10 Mobile n’obtiendront aucun de vos packages UWP.


## <a name="maintaining-package-compatibility-for-windows-phone-81"></a>Maintien de la compatibilité de package pour Windows Phone8.1

Les types de package doivent respecter certaines conditions lors de la mise à jour des applications précédemment publiées pour Windows Phone 8.1. :

-   Dès qu’une application a un package Windows Phone 8.1 publié, toutes les mises à jour ultérieures doivent également contenir un package Windows Phone 8.1.
-   Une fois qu’une application a un fichier XAP Windows Phone 8.1 publié, les mises à jour ultérieures doivent avoir un fichier XAP Windows Phone 8.1, un fichier .appx Windows Phone 8.1 ou un fichier .appxbundle Windows Phone 8.1.
-   Quand une application a un fichier .appx Windows Phone 8.1 publié, les mises à jour ultérieures doivent avoir un fichier .appx Windows Phone 8.1 ou un fichier .appxbundle Windows Phone 8.1. En d’autres termes, un package XAP Windows Phone 8.1 n’est pas autorisé. Ceci s'applique à un package .appxupload contenant également un package .appx Windows Phone 8.1.
-   Lorsqu’une application a un package .appxbundle Windows Phone 8.1 publié, les mises à jour ultérieures doivent avoir un package .appxbundle Windows Phone 8.1. En d’autres termes, un fichier XAP Windows Phone 8.1 ou un fichier .appx Windows Phone 8.1 n’est pas autorisé. Ceci s’applique à un package .appxupload contenant également un package .appxbundle Windows Phone 8.1.

Le non-respect de ces règles peut entraîner des erreurs de chargement de package qui vous empêchent de finaliser votre soumission.