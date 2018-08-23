---
author: jnHs
Description: Select the base price for an app and schedule price changes. You can also customize these options for specific markets.
title: Définir et planifier le prix de l’application
ms.author: wdg-dev-content
ms.date: 05/08/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp, tarification, prix de l’application, vente d’applications, modification de prix, prix personnalisé, prix, tarif, coût, remplacer le prix de base, prix au format libre, format libre
ms.localizationpriority: medium
ms.openlocfilehash: ca37d0b360679a878cff3aeabd96f82016c36fde
ms.sourcegitcommit: 9c79fdab9039ff592edf7984732d300a14e81d92
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/23/2018
ms.locfileid: "2819439"
---
# <a name="set-and-schedule-app-pricing"></a>Définir et planifier le prix de l’application

La section **Tarification** de la page [Tarification et disponibilité](set-app-pricing-and-availability.md) vous permet de sélectionner le prix de base d’une application. Vous pouvez également [planifier des changements de prix](#schedule-price-changes) pour indiquer la date et l’heure auxquelles le prix de votre application doit changer. En outre, vous avez la possibilité de [remplacer le prix de base pour des marchés spécifiques](#override-base-price-for-specific-markets), en sélectionnant un nouveau niveau de prix ou en saisissant un prix au format libre dans la devise locale du marché.

> [!NOTE]
> Bien que cet article fasse référence aux applications, la sélection du prix des soumissions d’extensions applique le même processus. Notez que pour [les modules complémentaires d’abonnement](../monetize/enable-subscription-add-ons-for-your-app.md), le prix de base que vous sélectionnez ne peut jamais être augmenté (que ce soit par modification du prix de base ou en planifiant une modification de prix), même si elle peut être réduit.

## <a name="base-price"></a>Prix de base

Une fois que vous avez sélectionné le **Prix de base** de votre application, ce prix est utilisé dans tous les marchés où votre application est vendue, sauf si vous choisissez de remplacer le prix de base dans un ou plusieurs marchés.

Vous pouvez définir le **Prix de Base** sur **Gratuit** ou choisir un niveau de prix disponible, qui définit le prix dans tous les pays où vous souhaitez distribuer votre application. Les niveaux de prix commencent à 0,99USD, avec des niveaux supplémentaires disponibles par incréments croissants (1,09USD, 1,19USD, etc.). Les incréments augmentent généralement à mesure que le prix devient plus élevé. 

> [!NOTE]
> Ces niveaux de prix s’appliquent également aux modules complémentaires. 

Chaque niveau de prix a une valeur correspondante dans chacune des devises du Windows Store, qui en compte plus de60. Ces valeurs vous aident à vendre vos applications à un prix comparable dans le monde entier. Vous pouvez sélectionner votre prix de base dans n’importe quelle devise et nous utiliserons automatiquement la valeur correspondante pour différents marchés. Notez que nous pouvons parfois ajuster la valeur correspondante dans un certain marché pour prendre en compte les modifications des taux de conversion de devises.

Dans la section **Tarification**, cliquez sur **Afficher la table de conversion** pour voir les prix correspondants dans toutes les devises. Cela permet également d'afficher un numéro d’identification associé à chaque niveau de prix. Vous en aurez besoin si vous utilisez l'[API de soumission au Microsoft Store](../monetize/manage-app-submissions.md#price-tiers) pour entrer des prix. Vous pouvez cliquer sur **Télécharger** pour télécharger une copie de la table des niveaux de prix sous forme de fichier .csv.

N'oubliez pas que le niveau de prix que vous sélectionnez peut inclure la taxe de vente ou la taxe sur la valeur ajoutée que vos clients doivent payer. Pour plus d’informations sur les implications fiscales de votre application dans les marchés sélectionnés, voir l’article [Informations fiscales pour les applications payantes](tax-details-for-paid-apps.md). Nous vous conseillons également de consulter les [considérations de prix pour des marchés spécifiques](define-pricing-and-market-selection.md#price-considerations-for-specific-markets).

> [!NOTE]
> Si vous choisissez l’option **Empêcher l'acquisition** sous **Rendre ce produit disponible mais non détectable dans le Store** dans la section [Visibilité](choose-visibility-options.md#discoverability), vous ne pourrez pas définir la tarification pour votre soumission (comme personne ne sera en mesure d’acquérir l’application, sauf si quelqu’un dispose d’un code promotionnel pour obtenir l’application gratuitement).

## <a name="schedule-price-changes"></a>Planifier des changements de prix

Vous pouvez également planifier un ou plusieurs changements de prix si vous souhaitez que le prix de base de votre application change à une date et à une heure spécifiques. 

> [!IMPORTANT]
> Les modifications de prix s'affichent uniquement pour les clients utilisant des appareils Windows10 (y compris Xbox). Si votre application prend en charge les versions antérieures du système d’exploitation, les changements de prix ne s'appliquent pas. 
>
> - Pour les clients sur Windows8, l’application sera toujours proposée à son **Prix de Base** (et non à un prix spécifique au marché), même si vous planifiez des changements de prix supplémentaires. 
> - Pour les clients sur Windows8.1 ou WindowsPhone8.1 et versions antérieures, l’application sera toujours proposée au premier niveau de prix pour le marché du client, même si vous planifiez des changements de prix supplémentaires sur ce marché.
> 
> Tenez-en compte lors de la planification des changements de prix. Par exemple, si vous publiez initialement votre application à un niveau de prix inférieur, puis planifiez une date à laquelle le prix doit augmenter, vos clients qui utilisent des versions antérieures du système d’exploitation achèteront l’application au prix inférieur (d’origine).

Cliquez sur **Planifier un changement de prix** pour afficher les options de changement de prix. Choisissez le niveau de prix que vous souhaitez utiliser (ou entrez un prix au format libre pour les remplacements du prix de base pour un marché unique), puis sélectionnez la date, l’heure et le fuseau horaire.

Vous pouvez cliquer sur **Planifier un nouveau changement de prix** pour planifier autant de changements consécutifs que vous le souhaitez.

> [!NOTE]
> Les changements de prix planifiés fonctionnent différemment du [Prix de vente](put-apps-and-add-ons-on-sale.md). Lorsque vous mettez une application en vente, le prix barré s’affiche dans le Store et les clients peuvent acheter l’application au prix de vente pendant la période que vous avez sélectionnée. Une fois la période de vente terminée, le prix de vente ne s’applique plus et l’application devient disponible à son prix de base (ou à un autre prix que vous avez spécifié pour ce marché, le cas échéant).
>
> Avec un changement de prix planifié, vous pouvez ajuster le prix pour qu'il soit supérieur ou inférieur. La modification aura lieu à la date que vous spécifiez, mais elle ne s’affichera pas sous forme de vente dans le Store et aucun formatage spécial ne lui sera appliqué; l’application aura simplement un nouveau prix. 


## <a name="override-base-price-for-specific-markets"></a>Remplacer le prix de base pour des marchés spécifiques

Par défaut, les options que vous sélectionnez ci-dessus s'appliquent à tous les marchés dans lesquels votre application est proposée. Vous pouvez éventuellement modifier le prix pour un ou plusieurs marchés, en choisissant un autre niveau de prix ou en saisissant un prix au format libre dans la devise locale du marché.

> [!IMPORTANT]
> Les clients sur Windows8 voient toujours l’application à son **Prix de Base**, même si vous sélectionnez un prix différent pour leur marché.

Pour modifier le prix pour des marchés spécifiques, cliquez sur **Select markets for base price override**. La fenêtre contextuelle **Sélection du marché** s’affiche, répertoriant tous les marchés dans lesquels vous avez choisi de rendre votre application disponible. (Si vous avez exclu des marchés dans la section **Marchés**, ces marchés ne seront pas disponibles). 

Vous pouvez remplacer le prix de base pour un marché à la fois, ou simultanément pour un groupe de marchés. Une fois cela fait, vous pouvez remplacer le prix de base pour un marché supplémentaire (ou un groupe de marchés supplémentaires) en sélectionnant **Select markets for base price override** de nouveau et en répétant le processus décrit ci-dessous. Pour supprimer le prix de remplacement que vous avez spécifié pour un marché (ou un groupe de marchés), cliquez sur **Supprimer**.


### <a name="override-the-base-price-for-a-single-market"></a>Remplacer le prix de base pour un marché unique

Pour modifier le prix pour un seul marché, sélectionnez-le et cliquez sur **Créer**. Vous verrez les mêmes options **Prix de Base** et **Planifier un changement de prix** que décrit ci-dessus, mais les sélections que vous effectuez seront spécifiques à ce marché. Étant donné que vous remplacez le prix de base pour un seul marché, les niveaux de prix seront affichés dans la devise locale de ce marché. Vous pouvez cliquer sur **Afficher la table de conversion** pour voir les prix correspondants dans toutes les devises. 

Remplacer le prix de base pour un marché unique vous permet également d’entrer le prix au format libre de votre choix dans la devise locale du marché. Vous pouvez entrer n’importe quel prix (dans une plage minimale et maximale), même s’il ne correspond pas à des niveaux de prix standard. Ce prix sera utilisé uniquement pour les clients sur Windows10 (y compris Xbox) sur le marché sélectionné. 

> [!IMPORTANT]
> Si vous entrez un prix au format libre, ce prix ne sera pas ajusté (même si le taux de conversion change), sauf si vous soumettez une mise à jour avec un nouveau prix. 

### <a name="override-the-base-price-for-a-market-group"></a>Remplacer le prix de base pour un groupe de marchés

Pour remplacer le prix de base pour plusieurs marchés, vous allez créer un *Groupe de marchés*. Pour ce faire, sélectionnez les marchés que vous souhaitez inclure, puis entrez éventuellement un nom pour le groupe. (Ce nom sert uniquement de référence et est invisible pour les clients). Lorsque vous avez terminé, cliquez sur **Créer**. Vous verrez les mêmes options **Prix de Base** et **Planifier un changement de prix** que décrit ci-dessus, mais les sélections que vous effectuez seront propres à ce groupe de marchés. Notez que les prix au format libre ne peuvent pas être utilisés avec les groupes de marchés. Vous devrez sélectionner un niveau de prix disponible.

Pour modifier les marchés inclus dans un groupe de marchés, cliquez sur le nom du groupe de marchés, ajoutez ou supprimez les marchés que vous souhaitez, puis cliquez sur **OK** pour enregistrer vos modifications. 

> [!NOTE]
> Notez qu’un marché ne peut appartenir qu’à un seul des groupes de marchés utilisés dans la section **Tarification**.





