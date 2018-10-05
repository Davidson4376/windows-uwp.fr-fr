---
author: jnHs
Description: The Pricing and availability page of the app submission process lets you determine how much your app will cost, whether you'll offer a free trial, and how, when, and where it will be available to customers.
title: Définir la tarification et la disponibilité d’une application
ms.assetid: 37BE7C25-AA74-43CD-8969-CBA3BD481575
ms.author: wdg-dev-content
ms.date: 05/11/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp, prix, disponible, détectable, version d’évaluation gratuite, versions d’évaluation, applications, date de publication
ms.localizationpriority: medium
ms.openlocfilehash: 20c52687b375f9bf33dd491eeb37d4142acace99
ms.sourcegitcommit: 63cef0a7805f1594984da4d4ff2f76894f12d942
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/05/2018
ms.locfileid: "4383140"
---
# <a name="set-app-pricing-and-availability"></a>Définir la tarification et la disponibilité d’une application


La page **Tarification et disponibilité** du [processus de soumission d’application](app-submissions.md) vous permet de déterminer le prix de votre application, la mise à disposition ou non d’une version d’évaluation gratuite, ainsi que le mode, la date et l’emplacement d’accessibilité de votre application auprès des clients. Cet article décrit les options disponibles sur cette page et les éléments à prendre en compte pour la spécification de ces informations.


## <a name="markets"></a>Marchés

Le Microsoft Store touche des clients dans plus de 200pays et régions dans le monde. Par défaut, nous allons proposer votre application sur tous les marchés possibles. Si vous préférez, vous pouvez choisir les marchés spécifiques sur lesquels vous souhaitez proposer votre application. 

Pour plus d’informations, voir l’article [Définir la sélection du marché](define-pricing-and-market-selection.md).


## <a name="visibility"></a>Visibilité

La section **Visibilité** vous permet de définir des restrictions relatives aux modes de découverte et d’acquisition de votre application, y compris si les personnes peuvent trouver votre application dans le Store ou voir sa description dans le Store.

Pour plus d’informations, voir [Choisir les options de visibilité](choose-visibility-options.md).


## <a name="schedule"></a>Planification

Par défaut (sauf si vous avez sélectionné l’une des options **Rendre votre application disponible mais non détectable dans le Store** dans la section [Visibilité](choose-visibility-options.md#discoverability)), votre application sera disponible pour les clients dès qu’elle aura obtenu la certification et terminé le processus de publication. Pour choisir d’autres dates, sélectionnez **Afficher les options** pour développer cette section. 

Pour plus d’informations, voir [Configurer le calendrier de publication exact](configure-precise-release-scheduling.md).


## <a name="pricing"></a>Tarification

Vous devez sélectionner un prix de base pour votre application (sauf si vous avez sélectionné l’option **Empêcher l’acquisition** sous **Rendre votre application disponible mais non détectable dans le Store** dans la section [Visibilité](choose-visibility-options.md#discoverability)), en choisissant soit **Gratuit**, soit l’un des niveaux de prix disponibles. Vous pouvez également programmer des modifications de tarifs pour indiquer la date et l'heure à laquelle le prix de votre application doit changer. Vous pouvez aussi personnaliser ces changements pour des marchés spécifiques. 

Pour plus d’informations, voir [Fixer et planifier le prix des applications](set-and-schedule-app-pricing.md).


## <a name="free-trial"></a>Essai gratuit

De nombreux développeurs offrent aux utilisateurs la possibilité d’essayer gratuitement leur application à l’aide de la fonctionnalité d’essai fournie par le Store. Par défaut, **Aucune version d’essai gratuit** est sélectionné. Il n’y aura alors aucune version d’essai de votre application. Si vous souhaitez en proposer une, vous pouvez sélectionner une valeur dans la liste déroulante **Version d’essai gratuite**.

Vous pouvez choisir parmi deux types de version d’essai et vous avez la possibilité de configurer la date et l’heure à laquelle cette version d’essai doit démarrer et arrêter d'être proposée.

### <a name="time-limited"></a>Durée limitée

Choisissez **Durée limitée** pour permettre aux clients d’essayer votre application gratuitement pendant un certain nombre de jours: **1 jour**, **7jours**, **15jours** ou **30jours**. Vous pouvez limiter les fonctionnalités en ajoutant du code pour [exclure ou limiter des fonctionnalités de la version d’essai](../monetize/in-app-purchases-and-trials.md)ou vous pouvez donner accès à toutes les fonctionnalités pendant cette durée. 
> [!NOTE]
> Les versions d’essai limitées dans le temps ne sont pas proposées aux clients sous Windows10 build10.0.10586 ou versions antérieures, ni aux clients sous WindowsPhone8.1 et versions antérieures.

### <a name="unlimited"></a>Illimité

Choisissez **Illimité** pour permettre aux clients d’accéder à votre application gratuitement pendant une période indéfinie. Si vous voulez les inciter à acheter la version complète ultérieurement, veillez à ajouter du code pour [exclure ou limiter des fonctionnalités de la version d’évaluation](../monetize/in-app-purchases-and-trials.md).

### <a name="start-and-end-dates"></a>Dates de début et de fin

Par défaut, votre version d’essai sera disponible dès que votre application aura été publiée et elle sera proposée indéfiniment. Si vous le souhaitez, vous pouvez spécifier la date et l’heure à laquelle votre version d’évaluation doit commencer à être proposée et le moment où cette proposition doit s’arrêter. 

>[!NOTE]
> Ces dates ne s’appliquent qu’aux clients sous Windows10 (y compris Xbox). Si votre application est disponible pour les clients utilisant des versions antérieures du système d’exploitation, la version d'essai sera proposée à ces clients aussi longtemps que votre produit sera disponible. 

Pour définir les dates auxquelles votre version d’essai doit être proposée aux clients sous Windows10, changez les paramètres **Commence le** et/ou **Se termine le** sur la liste déroulante en **au**, puis choisissez la date et l’heure. Si vous procédez ainsi, vous pouvez choisir **UTC**, afin que l’heure que vous sélectionnez s'exprime en temps universel coordonné (UTC), ou **Local**, afin que ces dates soient utilisées dans chaque fuseau horaire associé à un marché. (Notez que pour les marchés qui incluent plusieurs fuseaux horaires, un seul fuseau horaire de ces marchés sera utilisé. Pour les États-Unis, le fuseau horaire est est utilisé.) Vous pouvez sélectionner **Personnaliser pour des marchés spécifiques** si vous souhaitez définir des dates différentes pour un ou plusieurs marchés.


## <a name="sale-pricing"></a>Prix de vente

Si vous souhaitez proposer votre application à un prix réduit pendant une durée limitée, vous pouvez créer et planifier une vente.

Pour plus d’informations, voir [Vendre des applications et des extensions à prix réduit](put-apps-and-add-ons-on-sale.md).


## <a name="organizational-licensing"></a>Gestion des licences organisationnelles

Par défaut, votre application peut être proposée aux entreprises avec une formule d’achat en volume. Vous pouvez indiquer si et comment votre application peut être proposée dans cette section.

Pour plus d’informations, voir [Options de gestion des licences organisationnelles](organizational-licensing.md).


## <a name="publish-date"></a>Date de publication

Auparavant, la section **Date de publication** apparaissait sur cette page. Cette fonctionnalité se trouve à présent sur la page [Options de soumission](manage-submission-options.md), dans la section **Options de mise en attente de publication**. (Pour contrôler la date à laquelle votre application doit être publiée dans le Store, nous vous recommandons d’utiliser la section [Planification](configure-precise-release-scheduling.md) de la page **Tarification et disponibilité**.)


