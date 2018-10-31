---
author: jnHs
Description: You can set the precise date and time that your app should become available in the Store, giving you greater flexibility and the ability to customize dates for different markets.
title: Configurer une planification précise de la publication
ms.author: wdg-dev-content
ms.date: 10/31/2018
ms.topic: article
keywords: windows10, uwp, planification, date de publication, dates, lancement
ms.localizationpriority: medium
ms.openlocfilehash: 6474fc334922d49489c377a0c29841fc45889e1d
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/30/2018
ms.locfileid: "5816923"
---
# <a name="configure-precise-release-scheduling"></a>Configurer une planification précise de la publication

La section **Planification** de la page [Tarification et disponibilité](set-app-pricing-and-availability.md) vous permet de définir la date et l’heure précises auxquelles votre application doit devenir disponible dans le Store, ce qui vous offre un surcroît de flexibilité et la possibilité de personnaliser les dates pour différents marchés.

> [!NOTE]
> Bien que cet article fasse référence aux applications, la planification de la publication des soumissions d’extensions applique le même processus.

En outre, vous pouvez choisir de définir une date à laquelle le produit ne sera plus disponible dans le Store. Notez qu'ainsi le produit n'est plus accessible dans le Store via une recherche ou une navigation, mais que les clients disposant d'un lien direct peuvent voir la description du produit dans le Store. Ils peuvent le télécharger uniquement s'ils possèdent déjà le produit ou s’ils disposent d'un [code promotionnel](generate-promotional-codes.md) et utilisent un appareil Windows10.

Par défaut (sauf si vous avez sélectionné l'une des options **Rendre votre application disponible, mais non détectable dans le Store** dans la section [Visibilité](choose-visibility-options.md#discoverability)), votre application sera mise à disposition des clients dès qu'elle aura obtenu la certification et terminé le processus de publication. Pour choisir d’autres dates, sélectionnez **Afficher les options** pour développer cette section.

Notez que vous ne serez pas en mesure de configurer des dates dans la section **Planification** si vous avez sélectionné l'une des options **Rendre votre application disponible, mais non détectable dans le Store** dans la section [Visibilité](choose-visibility-options.md#discoverability). En effet, puisque votre application n'est pas publiée, il n’existe aucune date de publication à configurer.

> [!IMPORTANT]
> Les dates que vous spécifiez dans la section Planification s’appliquent uniquement aux clients sous Windows10.
>
>Si votre application publiée précédemment prend en charge les versions antérieures du système d’exploitation, n’importe quelle date **empêcher l’acquisition** que vous sélectionnez ne s’applique pas à ces clients; ils seront toujours en mesure d’acquérir l’application (sauf si vous soumettez une mise à jour avec une nouvelle sélection dans la section [visibilité](choose-visibility-options.md#discoverability) , ou si vous sélectionnez **rendre votre application indisponible** à partir de la page de **Présentation de l’application** ).


## <a name="base-schedule"></a>Planification de base

Vos sélections pour la Planification de base s’appliquent à tous les marchés dans lesquels votre application est disponible, sauf si vous ajoutez ensuite des dates pour des marchés spécifiques (ou des groupes de marché) en sélectionnant [Personnaliser pour des marchés spécifiques](#customize-the-schedule-for-specific-markets).

Vous disposez de deux options: **Publication** et **Empêcher l'acquisition**. 

## <a name="release"></a>Publication

Dans le menu déroulant **Publication**, vous pouvez définir le moment où vous souhaitez que votre application soit disponible dans le Store. Cela signifie que l’application est détectable dans le Store via une recherche ou une navigation et que les clients peuvent afficher sa description dans le Store et acquérir l’application.

>[!NOTE]
> Une fois votre application publiée et disponible dans le Store, vous ne serez plus en mesure de sélectionner une date de **Publication** (dans la mesure où l’application est déjà publiée).

Voici les options que vous pouvez configurer pour la planification de **Publication** d'un produit:
- **dès que possible**: le produit sera publié dès qu’il sera certifié et publié. Il s’agit de l’option par défaut.
- **à**: le produit sera publié à la date et à l’heure de votre choix. Vous disposez de deux options supplémentaires:
   - **UTC**: l'heure sélectionnée correspondra à l'heure universelle coordonnée (UTC), afin que l’application soit publiée partout en même temps.
   - **Local**: l'heure sélectionnée sera utilisée dans chaque fuseau horaire associé à un marché. (Notez que pour les marchés qui incluent plusieurs fuseaux horaires, seul un fuseau horaire sur ce marché sera utilisé. Pour les États-Unis, le fuseau horaire Côte Est est utilisé.)
- **non planifiée**: l’application ne sera pas disponible dans le Store. Si vous choisissez cette option, vous pouvez rendre l’application disponible dans le Store ultérieurement en créant une nouvelle soumission et en choisissant l’une des autres options.


## <a name="stop-acquisition"></a>Empêcher l'acquisition

Dans la liste déroulante **Empêcher l'acquisition**, vous pouvez définir une date et une heure auxquelles vous souhaitez empêcher les nouveaux clients d'acquérir votre application à partir du Store ou de la détecter dans ses descriptions. Cela peut être utile si vous voulez contrôler précisément le moment où une application ne sera plus proposée aux nouveaux clients, par exemple, lorsque vous coordonnez la disponibilité de plusieurs de vos applications.

Par défaut, l'option **Empêcher l'acquisition** est configurée sur jamais. Pour modifier ce paramètre, sélectionnez **à** dans le menu déroulant et spécifiez une date et une heure, comme décrit ci-dessus. À la date et à l'heure que vous sélectionnez, les clients ne seront plus en mesure d’acquérir l’application.

Il est important de comprendre que cette option a le même effet que la sélection de **rendre votre application détectable mais ne sont pas disponibles** dans la section [visibilité](choose-visibility-options.md#discoverability) et en choisissant **empêcher l’acquisition: tout client ayant un lien direct peut voir Store du produit Description, mais ils peuvent le télécharger que s’il possède déjà le produit, ou ont un code promotionnel et utilisent un appareil Windows 10.** Pour arrêter de proposer une application aux nouveaux clients, cliquez sur **Rendre votre application indisponible**, sur la page Vue d’ensemble de l’application. Pour en savoir plus, consultez l’article [Suppression d’une application du Store](guidance-for-app-package-management.md#removing-an-app-from-the-store).

> [!TIP]
> Si vous sélectionnez une date pour **Empêcher l'acquisition** et décidez ultérieurement que vous souhaitez la rendre disponible à nouveau, vous pouvez créer une nouvelle soumission et reconfigurer **Empêcher l'acquisition** sur **Jamais**. L’application sera à nouveau disponible après la publication de votre soumission mise à jour.

## <a name="customize-the-schedule-for-specific-markets"></a>Personnaliser la planification pour des marchés spécifiques 

Par défaut, les options que vous sélectionnez ci-dessus s'appliquent à tous les marchés dans lesquels votre application est proposée. Pour personnaliser le prix pour des marchés spécifiques, cliquez sur **Personnaliser pour des marchés spécifiques**. La fenêtre contextuelle **Sélection du marché** s’affiche, répertoriant tous les marchés dans lesquels vous avez choisi de rendre votre application disponible. Si vous avez exclu des marchés dans la section [Marchés](define-pricing-and-market-selection.md), ces marchés ne s'afficheront pas. 

Pour ajouter une planification pour un marché, sélectionnez-le et cliquez sur **Enregistrer**. Vous disposerez des options **Publication** et **Empêcher l'acquisition** déjà décrites ci-dessus, mais les sélections que vous ferez s’appliqueront uniquement à ce marché.

Pour ajouter une planification à appliquer à plusieurs marchés, vous devez créer un *Groupe de marchés*. Pour ce faire, sélectionnez les marchés que vous souhaitez inclure, puis entrez un nom pour le groupe. (Ce nom sert uniquement de référence et est invisible pour les clients). Par exemple, si vous souhaitez créer un groupe de marchés pour l’Amérique du Nord, vous pouvez sélectionner **Canada**, **Mexique** et **États-Unis**et l'appeler **Amérique du Nord** ou lui donner un autre nom de votre choix. Lorsque vous avez fini de créer votre groupe de marchés, cliquez sur **Enregistrer**. Vous disposerez des options **Publication** et **Empêcher l'acquisition** déjà décrites ci-dessus, mais les sélections que vous ferez s’appliqueront uniquement à ce groupe de marchés.

Pour ajouter une planification personnalisée pour un marché ou un groupe de marchés supplémentaire, cliquez simplement à nouveau sur **Personnaliser pour des marchés spécifiques** et répétez ces étapes. Pour modifier les marchés inclus dans un groupe de marchés, sélectionnez son nom. Pour supprimer la planification personnalisée pour un groupe de marchés (ou un marché individuel), cliquez sur **Supprimer**.

> [!NOTE]
> Un marché ne peut appartenir qu’à un seul des groupes de marchés utilisés dans la section **Planification**. 










