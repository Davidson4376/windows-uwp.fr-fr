---
Description: Découvrez comment les packages de votre application sont mis à la disposition de vos clients, et comment gérer des scénarios de package spécifiques.
title: Aide sur la gestion des packages d’application
ms.assetid: 55405D0B-5C1E-43C8-91A1-4BFDD336E6AB
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: c75eb1a4b28b015b83557f74957a3370f478a26e
ms.sourcegitcommit: fca0132794ec187e90b2ebdad862f22d9f6c0db8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/24/2019
ms.locfileid: "63790780"
---
# <a name="guidance-for-app-package-management"></a>Aide sur la gestion des packages d’application

Découvrez comment les packages de votre application sont mis à la disposition de vos clients, et comment gérer des scénarios de package spécifiques.

-   [Versions de système d’exploitation et la distribution de package](#os-versions-and-package-distribution)
-   [Ajout de packages à une application précédemment publiées pour Windows 10](#adding-packages-for-windows-10-to-a-previously-published-app)
-   [Maintenir la compatibilité du package pour Windows Phone 8.1](#maintaining-package-compatibility-for-windows-phone-81)
-   [Suppression d’une application à partir du Store](#removing-an-app-from-the-store)
-   [Suppression des packages pour une famille de périphériques pris en charge précédemment](#removing-packages-for-a-previously-supported-device-family)


## <a name="os-versions-and-package-distribution"></a>Versions de système d’exploitation et distribution de package

Les différents systèmes d’exploitation peuvent exécuter différents types de packages. Si plusieurs de vos packages peuvent s’exécuter sur l’appareil d’un client, le Microsoft Store fournit la meilleure correspondance disponible.

En règle générale, les systèmes d’exploitation plus récents peuvent exécuter des packages ciblant des versions antérieures pour la même famille d’appareils. Appareils Windows 10 peuvent exécuter toutes les précédentes versions du système d’exploitation pris en charge (par famille de périphériques). Appareils Windows 10 desktop peuvent exécuter des applications qui ont été créées pour Windows 8.1 ou Windows 8 ; Les appareils mobiles Windows 10 peuvent exécuter des applications qui ont été créées pour Windows Phone 8.1, Windows Phone 8 et même Windows Phone 7.x. Toutefois, les clients sur Windows 10 n’obtiendrez ces packages si l’application n’inclut pas les packages UWP ciblant la famille d’appareils applicables.

> [!IMPORTANT]
> À compter du 31 octobre 2018, produits nouvellement créé ne peut pas inclure les packages ciblant 8.x/Windows de Windows Phone 8.x ou une version antérieure. Pour plus d’informations, consultez ce [billet de blog](https://blogs.windows.com/buildingapps/2018/08/20/important-dates-regarding-apps-with-windows-phone-8-x-and-earlier-and-windows-8-8-1-packages-submitted-to-microsoft-store/).


## <a name="removing-an-app-from-the-store"></a>Suppression d'une application du Windows Store

Parfois, il est possible que vous souhaitiez arrêter de fournir une application à vos clients, « annuler » sa publication. Pour ce faire, cliquez sur **Rendre votre application indisponible** sur la page **Vue d’ensemble de l’application**. Quelques heures après que vous avez confirmé vouloir la rendre indisponible, votre application disparaît du Store. Dès lors, aucun nouveau client ne peut plus y accéder (sauf s'il possède un [code promotionnel](generate-promotional-codes.md) et utilise un appareil Windows 10).

> [!IMPORTANT]
> Avec cette option, les paramètres de [visibilité](choose-visibility-options.md#discoverability) sélectionnés dans vos soumissions seront remplacés. 

Cette option a le même effet que si vous avez créé une soumission et choisi l’option **Rendre ce produit disponible mais non détectable dans le Store** avec l’option **Empêcher l'acquisition**. Toutefois, elle ne vous oblige pas à créer une nouvelle soumission.

Notez que les clients ayant déjà l’application pourront encore l’utiliser et la retélécharger (et même recevoir des mises à jour si vous envoyez de nouveaux packages ultérieurement).

Une fois l’application indisponible, vous verrez toujours il dans Partner Center. Si vous décidez de la remettre à disposition des clients, vous pouvez cliquer sur **Rendre votre application indisponible** sur la page Vue d’ensemble de l’application. L’application est mise à disposition des nouveaux clients (sauf paramétrage contraire dans votre dernière soumission) dans les heures suivant votre confirmation.

> [!NOTE]
> Si vous souhaitez que votre application reste disponible, mais voulez arrêter de la proposer aux nouveaux clients sur une version spécifique de système d’exploitation, vous pouvez créer une autre soumission et supprimer tous les packages associés à la version de système d’exploitation pour laquelle vous souhaitez empêcher toute nouvelle acquisition. Par exemple, si vous aviez précédemment des packages pour Windows Phone 8.1 et Windows 10, et vous ne souhaitez pas conserver proposant l’application à de nouveaux clients sur Windows Phone 8.1, supprimez tous vos packages Windows Phone 8.1 à partir de la soumission. Une fois la mise à jour est publiée, aucun nouveau client sur Windows Phone 8.1 ne sera en mesure d’acquérir l’application, bien que les clients qui ont déjà qu’il peuvent continuer à l’utiliser). Toutefois, l’application sera toujours disponible pour les nouveaux clients sur Windows 10.


## <a name="removing-packages-for-a-previously-supported-device-family"></a>Suppression de packages pour une famille d'appareils précédemment prise en charge

Si vous supprimez tous les packages pour un certain [famille de périphériques](https://docs.microsoft.com/uwp/extension-sdks/device-families-overview) que votre application précédemment pris en charge, vous serez invité à confirmer qu’il s’agit de votre intention avant de pouvoir enregistrer vos modifications sur le **Packages** page.

Lorsque vous publiez une soumission qui supprime tous les packages qui peuvent s’exécuter sur une famille de périphériques précédemment pris en charge par votre application, les nouveaux clients ne seront pas pu acquérir l’application sur cette famille de périphériques. Vous pouvez toujours publier une autre mise à jour pour proposer de nouveau des packages pour cette famille d'appareils.

Gardez à l'esprit que même si vous supprimez tous les packages prenant en charge une certaine famille d'appareils, tous les clients existants ayant déjà installé l'application sur ce type d'appareil pourra encore l'utiliser et obtenir les mises à jour que vous proposerez ultérieurement.


<a name="adding-packages-for-windows-10-to-a-previously-published-app"></a>

## <a name="adding-packages-for-windows10-to-a-previously-published-app"></a>Ajout de packages à une application précédemment publiées pour Windows 10

Si vous disposez d’une application dans le Store incluant uniquement les packages pour Windows 8.x et/ou de Windows Phone 8.x et que vous souhaitez mettre à jour votre application pour Windows 10, créez une nouvelle soumission et ajouter votre UWP .msixupload ou .appxupload package (s) pendant la [Packages](upload-app-packages.md) étape. Une fois que votre application passe par le processus de certification, le package UWP sera également disponible pour les nouvelles acquisitions par les clients sur Windows 10.

> [!NOTE]
> Une fois qu’un client sur Windows 10 obtient votre package UWP, vous ne pouvez plus restaurer ce client revenir à un package pour n’importe quelle version du système d’exploitation précédente. 

Notez que le numéro de version de vos packages Windows 10 doit être supérieur à ceux de tous les packages Windows 8, Windows 8.1 ou Windows Phone 8.1 que vous avez utilisé. Pour plus d’informations, voir [Numérotation des versions de packages](package-version-numbering.md).

Pour plus d’informations sur la création de packages d’applications UWP pour le Store, voir [Création de packages d’applications](../packaging/index.md).
