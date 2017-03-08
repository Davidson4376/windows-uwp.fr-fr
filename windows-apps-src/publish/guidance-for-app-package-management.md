---
author: jnHs
Description: "Découvrez comment les packages de votre application sont mis à la disposition de vos clients, et comment gérer des scénarios de package spécifiques."
title: "Aide sur la gestion des packages d’application"
ms.assetid: 55405D0B-5C1E-43C8-91A1-4BFDD336E6AB
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp"
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 54f6d6c786eb0787a441628452d26e46f353b3d8
ms.lasthandoff: 02/07/2017

---

# <a name="guidance-for-app-package-management"></a>Aide sur la gestion des packages d’application


Découvrez comment les packages de votre application sont mis à la disposition de vos clients, et comment gérer des scénarios de package spécifiques.

-   [Versions de système d’exploitation et distribution de package](#os-versions-and-package-distribution)
-   [Ajout de packages pour Windows 10 à une application publiée précédemment](#adding-packages-for-windows-10-to-a-previously-published-app)
-   [Maintien de la compatibilité du package pour Windows Phone 8.1](#maintaining-package-compatibility-for-windows-phone-8-1)
-   [Suppression d'une application du Windows Store](#removing-an-app-from-the-store)
-   [Suppression de packages pour une famille d'appareils précédemment prise en charge](#removing-packages-for-a-previously-supported-device-family)

## <a name="os-versions-and-package-distribution"></a>Versions de système d'exploitation et distribution de package


Les différents systèmes d’exploitation peuvent exécuter différents types de packages. Si plusieurs de vos packages peuvent s’exécuter sur l’appareil d’un client, le Windows Store fournit la meilleure correspondance disponible.

En règle générale, les systèmes d’exploitation plus récents peuvent exécuter des packages ciblant des versions antérieures pour la même famille d’appareils. Toutefois, elles n’obtiennent ces packages que si l’application n’inclut pas de package ciblant la version actuelle du système d’exploitation.

Par exemple, les appareils Windows 10 peuvent exécuter toutes les versions antérieures du système d’exploitation prises en charge (par famille d’appareils). Les appareils de bureau Windows 10 peuvent exécuter des applications conçues pour Windows 8.1 ou Windows 8. Les appareils mobiles Windows 10 peuvent exécuter des applications conçues pour Windows Phone 8.1, Windows Phone 8, voire Windows Phone 7.x.

Les exemples suivants illustrent divers scénarios pour une application incluant des packages ciblant différentes versions de système d’exploitation. Dans certains cas, des contraintes spécifiques de vos packages peuvent ne pas autoriser leur exécution sur toutes les versions de système d’exploitation et tous les types d’appareils répertoriés ici (par exemple, l’architecture doit être appropriée), mais ces exemples devraient vous aider à comprendre quelles versions de système d’exploitation peuvent exécuter vos packages spécifiques.

### <a name="example-app-1"></a>Exemple d’application 1

| Système d’exploitation ciblé du package | Systèmes d’exploitation qui obtiendront ce package |
|-------------------------------------|----------------------------------------------|
| Windows 8.1                         | Appareils de bureau Windows 10, Windows 8.1      |
| Windows Phone 8.1                   | Appareils mobiles Windows 10, Windows Phone 8.1 |
| Windows Phone 8                     | Windows Phone 8                              |
| Windows Phone 7.1                   | Windows Phone 7.x                            |

Dans l’exemple 1, l’application n’a pas encore de packages UWP spécialement conçus pour les appareils Windows 10, mais les clients utilisant Windows 10 peuvent toujours obtenir l’application. Ces clients reçoivent les meilleurs packages disponibles, selon leur type d’appareil.

### <a name="example-app-2"></a>Exemple d’application 2

| Système d’exploitation ciblé du package  | Systèmes d’exploitation qui obtiendront ce package |
|--------------------------------------|----------------------------------------------|
| Windows 10 (famille d’appareils universelle) | Windows 10 (toutes familles d’appareils)             |
| Windows 8.1                          | Windows 8.1                                  |
| Windows Phone 8.1                    | Windows Phone 8.1                            |
| Windows Phone 7.1                    | Windows Phone 7.x, Windows Phone 8           |

Dans l’exemple 2, aucun package ne peut s’exécuter sur Windows 8. Les clients qui exécutent toutes les autres versions du système d'exploitation peuvent télécharger l'application.

### <a name="example-app-3"></a>Exemple d’application 3

| Système d’exploitation ciblé du package | Systèmes d’exploitation qui obtiendront ce package                  |
|-------------------------------------|---------------------------------------------------------------|
| Windows 10 (famille d’appareils de bureau)  | Appareils de bureau Windows 10                                    |
| Windows Phone 8                     | Appareils mobiles Windows 10, Windows Phone 8, Windows Phone 8.1 |

Dans l’exemple 3, puisqu’il n’existe aucun package UWP ciblant la famille d’appareils mobiles, les clients utilisant des appareils mobiles Windows 10 reçoivent le package Windows Phone 8. Si cette application ajoute ultérieurement un package ciblant la famille d’appareils mobiles (ou la famille d’appareils universelle), ces packages seront disponibles pour les clients utilisant des appareils mobiles Windows 10 à la place du package Windows Phone 8.

Notez également que cet exemple d’application n’inclut aucun package pouvant s’exécuter sur Windows 7.x.

### <a name="example-app-4"></a>Exemple d’application 4

| Système d’exploitation ciblé du package  | Systèmes d’exploitation qui obtiendront ce package |
|--------------------------------------|----------------------------------------------|
| Windows 10 (famille d’appareils universelle) | Windows 10 (toutes les familles d’appareils)             |

Dans l’exemple 4, tout appareil exécutant Windows 10 peut obtenir l’application, mais celle-ci ne sera pas disponible pour les clients utilisant une version antérieure du système d’exploitation. Étant donné que le package UWP cible la famille d’appareils universelle, il sera disponible pour les appareils Windows 10 mobiles et de bureau.

## <a name="adding-packages-for-windows-10-to-a-previously-published-app"></a>Ajout de packages pour Windows 10 à une application publiée précédemment


Si vous avez une application dans le Windows Store et que vous voulez mettre à jour votre application pour Windows 10, créez une soumission et ajoutez votre ou vos packages UWP .appxupload pendant l’étape [Packages](upload-app-packages.md). Lorsque votre application passe en certification, les clients qui avaient déjà votre application avant la mise à niveau vers Windows 10 pourront obtenir votre package UWP comme une mise à jour à partir du Windows Store. Le package UWP sera également disponible pour les nouvelles acquisitions effectuées par les clients sur Windows 10.

> **Important**  Une fois qu’un client sur Windows 10 obtient votre package UWP, il lui est impossible de revenir à un package conçu pour une version antérieure du système d’exploitation. Veillez à tester complètement vos packages UWP sur Windows 10 avant de les ajouter à votre soumission.

Vous pouvez mettre à jour tout autre package en même temps ou apporter d’autres modifications à la soumission (par exemple, [créer des descriptions spécifiques de la plateforme](create-platform-specific-descriptions.md) à afficher aux clients utilisant des versions antérieures du système d’exploitation). Si vous le souhaitez, vous pouvez également laisser tout le reste sans y apporter de modification.

> **Remarque**  Le numéro de version de vos packages Windows 10 doit être supérieur à ceux des packages Windows 8, Windows 8.1 et/ou Windows Phone 8.1 que vous publiez (ou avez publiés) pour la même application. Pour plus d’informations sur la numérotation des versions pour Windows 10, voir [Numérotation des versions de packages](package-version-numbering.md).

Une fois la nouvelle soumission certifiée, les packages UWP seront disponibles, ainsi que tous les packages que vous avez mis à disposition pour les clients qui ne sont pas encore sur Windows 10.

Pour plus d’informations sur l’empaquetage d’applications pour UWP pour le Windows Store, consultez [Empaquetage d’applications Windows universelles pour Windows 10](http://go.microsoft.com/fwlink/p/?LinkId=620193 ).

> **Important**  N’oubliez pas que si vous fournissez des packages ciblant la famille d’appareils universelle, chaque client déjà doté de votre application sur un système d’exploitation antérieur (Windows Phone 8, Windows 8.1, etc.) et qui procède à une mise à niveau vers Windows 10 bénéficiera d’une mise à jour vers votre package universel Windows 10.
> 
> Tel est le cas, même si vous avez exclu une famille d’appareils spécifique à l’étape [Tarification et disponibilité](set-app-pricing-and-availability.md#windows-10-device-families) de votre soumission, car la sélection **Familles d’appareils** ne s’applique qu’aux nouvelles acquisitions. Si vous ne voulez pas que chaque client antérieur obtienne votre nouveau package Windows 10, veillez à mettre à jour l’élément [**TargetDeviceFamily**](https://msdn.microsoft.com/library/windows/apps/dn986903) dans votre manifeste appx afin d’inclure uniquement la famille particulière d’appareils que vous voulez prendre en charge.
> 
> Par exemple, supposons que vous souhaitiez que seuls vos clients Windows 8 et Windows 8.1 ayant effectué la mise à niveau vers Windows 10 puissent obtenir votre application pour UWP, et que les clients sur Windows Phone 8.1 et versions antérieures conservent les packages que vous avez mis à disposition précédemment (pour Windows Phone 8 ou Windows Phone 8.1). Pour ce faire, vous devez veiller à mettre à jour [**TargetDeviceFamily**](https://msdn.microsoft.com/library/windows/apps/dn986903) dans votre manifeste appx pour n’inclure que **Windows.Desktop** (pour la famille d’appareils de bureau), au lieu de laisser la valeur **Windows.Universal** (pour la famille d’appareils universelle) que Microsoft Visual Studio inclut dans le manifeste appx par défaut. Ne soumettez pas de packages UWP ciblant la famille d’appareils universelle ou la famille d’appareils mobiles (**Windows.Universal** ou **Windows.Universal**). Ainsi, vos clients mobiles Windows 10 n’obtiendront aucun de vos packages UWP.
> 
> Pour plus d’informations sur les familles d’appareils, voir le [Guide des applications pour la plateforme Windows universelle (UWP)](https://msdn.microsoft.com/library/windows/apps/dn894631).

## <a name="maintaining-package-compatibility-for-windows-phone-81"></a>Maintien de la compatibilité de package pour Windows Phone 8.1


Les types de package doivent respecter certaines conditions lors de la mise à jour des applications précédemment publiées pour Windows Phone 8.1. :

-   Dès qu’une application a un package Windows Phone 8.1 publié, toutes les mises à jour ultérieures doivent également contenir un package Windows Phone 8.1.
-   Une fois qu’une application a un fichier XAP Windows Phone 8.1 publié, les mises à jour ultérieures doivent avoir un fichier XAP Windows Phone 8.1, un fichier .appx Windows Phone 8.1 ou un fichier .appxbundle Windows Phone 8.1.
-   Quand une application a un fichier .appx Windows Phone 8.1 publié, les mises à jour ultérieures doivent avoir un fichier .appx Windows Phone 8.1 ou un fichier .appxbundle Windows Phone 8.1. En d’autres termes, un package XAP Windows Phone 8.1 n’est pas autorisé. Ceci s'applique à un package .appxupload contenant également un package .appx Windows Phone 8.1.
-   Lorsqu’une application a un package .appxbundle Windows Phone 8.1 publié, les mises à jour ultérieures doivent avoir un package .appxbundle Windows Phone 8.1. En d’autres termes, un fichier XAP Windows Phone 8.1 ou un fichier .appx Windows Phone 8.1 n’est pas autorisé. Ceci s'applique à un package .appxupload contenant également un package .appxbundle Windows Phone 8.1.

Le non-respect de ces règles entraîne des erreurs de chargement de package qui vous empêchent de finaliser votre soumission.

## <a name="removing-an-app-from-the-store"></a>Suppression d'une application du Windows Store


Parfois, il est possible que vous souhaitiez arrêter de fournir une application à vos clients, « annuler » sa publication. Pour ce faire, cliquez sur **Rendre votre application indisponible** sur la page Vue d’ensemble de l’application. Quelques heures après que vous avez confirmé vouloir la rendre indisponible, votre application disparaît du Windows Store. Dès lors, aucun nouveau client ne pourra y accéder, quelle que soit la méthode, même au moyen de codes promotionnels.

> **Important**  Les paramètres de [distribution et de visibilité](set-app-pricing-and-availability.md#distribution-and-visibility) sélectionnés dans vos soumissions seront remplacés.

Notez que les clients ayant déjà l’application pourront encore l’utiliser (et même recevoir des mises à jour si vous envoyez de nouveaux packages ultérieurement).

Une application rendue indisponible continue à s'afficher sur votre tableau de bord. Si vous décidez de la remettre à disposition des clients, vous pouvez cliquer sur **Rendre votre application indisponible** sur la page Vue d’ensemble de l’application. L’application est mise à disposition des nouveaux clients (sauf paramétrage contraire dans votre dernière soumission) dans les heures suivant votre confirmation.

> **Remarque**  Si vous souhaitez que votre application reste disponible, mais voulez arrêter de l’offrir aux clients sur une version spécifique de système d’exploitation, vous pouvez créer une autre soumission et supprimer tous les packages associés à la version de système d’exploitation pour laquelle vous souhaitez empêcher toute nouvelle acquisition. Par exemple, si vous possédiez auparavant des packages pour Windows Phone 8, Windows Phone 8.1 et Windows 10 et ne souhaitez pas continuer à offrir l’application aux nouveaux clients sur Windows Phone 8, supprimez vos packages Windows Phone 8 de la soumission. Une fois la mise à jour publiée, aucun nouveau client sous Windows Phone 8 ne pourra acquérir l’application (toutefois, les clients qui la possèdent continuent à en bénéficier). L'application reste disponible pour les nouveaux clients sur Windows Phone 8.1 et Windows 10.

## <a name="removing-packages-for-a-previously-supported-device-family"></a>Suppression de packages pour une famille d'appareils précédemment prise en charge


Vous êtes invité à confirmer la suppression de tous les packages d'une certaine famille d'appareils précédemment prise en charge par votre application avant d'enregistrer vos modifications sur la page **Packages**.

Lorsque vous publiez une soumission qui supprime les packages d'une famille d'appareils précédemment prise en charge par votre application, les nouveaux clients ne pourront pas acquérir l'application sur cette famille d'appareils. Vous pouvez toujours publier une autre mise à jour pour proposer de nouveau des packages pour cette famille d'appareils.

Gardez à l'esprit que même si vous supprimez tous les packages prenant en charge une certaine famille d'appareils, tous les clients existants ayant déjà installé l'application sur ce type d'appareil pourra encore l'utiliser et obtenir les mises à jour que vous proposerez ultérieurement.

 

 





