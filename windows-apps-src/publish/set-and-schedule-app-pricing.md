---
author: jnHs
Description: "Sélectionnez le prix de base pour une application et planifiez les modifications du prix. Vous pouvez également personnaliser ces options pour des marchés spécifiques."
title: "Définir et planifier le prix de l’application"
ms.author: wdg-dev-content
ms.date: 08/03/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.openlocfilehash: 65d2fa64e4d442da42b8c546693c61eda817aa18
ms.sourcegitcommit: 968187e803a866b60cda0528718a3d31f07dc54c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/03/2017
---
# <a name="set-and-schedule-app-pricing"></a>Définir et planifier le prix de l’application

La section **Tarification** de la page [Tarification et disponibilité](set-app-pricing-and-availability.md) vous permet de sélectionner le prix de base d’une application. Vous pouvez également [planifier des changements de prix](#schedule-price-changes) pour indiquer la date et l’heure auxquelles le prix de votre application doit changer. En outre, vous avez la possibilité de [personnaliser ces modifications pour des marchés spécifiques](#customize-pricing-for-specific-markets). 

> [!NOTE]
> Bien que cet article fasse référence aux applications, la sélection du prix des soumissions d’extensions applique le même processus.

## <a name="base-price"></a>Prix de base

Une fois que vous avez sélectionné le **Prix de base** de votre application, ce prix est utilisé dans tous les marchés où votre application est vendue, sauf si vous spécifiez un prix personnalisé pour un ou plusieurs marchés spécifiques.

Vous pouvez définir le **Prix de Base** sur **Gratuit** ou choisir un niveau de prix disponible, qui définit le prix de vente dans tous les pays où vous souhaitez distribuer votre application. Les niveaux de prix commencent à 0,99USD, avec des niveaux supplémentaires disponibles par incréments croissants (1,09USD, 1,19USD, etc.). Les incréments augmentent généralement à mesure que le prix devient plus élevé. 

> [!NOTE]
> Ces niveaux de prix s’appliquent également aux modules complémentaires. 

Chaque niveau de prix a une valeur correspondante dans chacune des devises du Windows Store, qui en compte plus de60. Ces valeurs vous aident à vendre vos applications à un prix comparable dans le monde entier. Vous pouvez sélectionner votre prix de base dans n’importe quelle devise et nous utiliserons automatiquement la valeur correspondante pour différents marchés.

Dans la section **Tarification**, cliquez sur **Afficher la table** pour voir les prix correspondants dans toutes les devises. Cela permet également d'afficher un numéro d’identification associé à chaque niveau de prix. Vous en aurez besoin si vous utilisez l'[API de soumission au Windows Store](../monetize/manage-app-submissions.md#price-tiers) pour entrer des prix. Vous pouvez cliquer sur **Télécharger** pour télécharger une copie de la table des niveaux de prix sous forme de fichier .csv.

N'oubliez pas que le niveau de prix que vous sélectionnez peut inclure la taxe de vente ou la taxe sur la valeur ajoutée que vos clients doivent payer. Pour plus d’informations sur les implications fiscales de votre application dans les marchés sélectionnés, voir l’article [Informations fiscales pour les applications payantes](tax-details-for-paid-apps.md). Nous vous conseillons également de consulter les [considérations de prix pour des marchés spécifiques](define-pricing-and-market-selection.md#price-considerations-for-specific-markets).

## <a name="schedule-price-changes"></a>Planifier des changements de prix

Vous pouvez également planifier un ou plusieurs changements de prix si vous souhaitez que le prix de base de votre application change à une date et à une heure spécifiques. 

> [!IMPORTANT]
> Les modifications de prix s'affichent uniquement pour les clients sur Windows10. Si votre application prend en charge les versions antérieures du système d’exploitation, les changements de prix ne s'appliquent pas. 
>
> - Pour les clients sur Windows8, l’application sera toujours proposée à son **Prix de Base** (et non à un prix spécifique au marché), même si vous planifiez des changements de prix supplémentaires. 
> - Pour les clients sur Windows8.1 ou WindowsPhone8.1 et versions antérieures, l’application sera toujours proposée au prix initial pour le marché du client, même si vous planifiez des changements de prix supplémentaires sur ce marché.
> 
> Tenez-en compte lors de la planification des changements de prix. Par exemple, si vous publiez initialement votre application à un prix inférieur, puis planifiez une date à laquelle le prix doit augmenter, vos clients qui utilisent des versions antérieures du système d’exploitation achèteront l’application au prix inférieur (d’origine).

Cliquez sur **Planifier un changement de prix** pour afficher les options de changement de prix. Choisissez le niveau de prix que vous souhaitez utiliser, puis sélectionnez la date, l'heure et le fuseau horaire.

Vous pouvez cliquer sur **Planifier un nouveau changement de prix** pour planifier autant de changements consécutifs que vous le souhaitez.

> [!NOTE]
> Les changements de prix planifiés fonctionnent différemment du [Prix de vente](put-apps-and-add-ons-on-sale.md). Lorsque vous mettez une application en vente, les clients consultant votre description dans le Windows Store voient que le prix a été réduit, et peuvent acheter l’application au prix inférieur pendant la période que vous avez sélectionnée. Une fois la période de vente finie, l'application retourne au prix de base.
>
> Avec un changement de prix planifié, vous pouvez ajuster le prix pour qu'il soit supérieur ou inférieur. La modification aura lieu à la date que vous spécifiez, mais elle ne s’affiche pas sous forme de vente dans le Windows Store; l’application aura simplement un nouveau prix de base. 

## <a name="customize-pricing-for-specific-markets"></a>Personnaliser la tarification pour des marchés spécifiques

Par défaut, les options que vous sélectionnez ci-dessus s'appliquent à tous les marchés dans lesquels votre application est proposée. Pour personnaliser le prix pour des marchés spécifiques, sélectionnez **Personnaliser pour des marchés spécifiques**. La fenêtre contextuelle **Sélection du marché** s’affiche, répertoriant tous les marchés dans lesquels vous avez choisi de rendre votre application disponible. Si vous avez exclu des marchés dans la section Marchés, ces marchés ne s'afficheront pas ici. 

> [!IMPORTANT]
> Les clients sur Windows8 voient toujours l’application à son **Prix de Base**, même si vous sélectionnez un prix différent pour leur marché.

Pour ajouter un prix personnalisé pour un marché, sélectionnez-le et cliquez sur **Enregistrer**. Vous verrez les mêmes options **Prix de Base** et **Planifier un changement de prix** que décrit ci-dessus, mais les sélections que vous effectuez seront spécifiques à ce marché.

Pour ajouter des prix personnalisés pour plusieurs marchés, vous allez créer un *Groupe de marchés*. Pour ce faire, sélectionnez les marchés que vous souhaitez inclure, puis entrez un nom pour le groupe. (Ce nom sert uniquement de référence et est invisible pour les clients). Lorsque vous avez terminé, cliquez sur **Enregistrer**. Vous verrez les mêmes options **Prix de Base** et **Planifier un changement de prix** que décrit ci-dessus, mais les sélections que vous effectuez seront propres à ce groupe de marchés.

> [!NOTE]
> Notez qu’un marché ne peut appartenir qu’à un seul des groupes de marchés utilisés dans la section Tarification.

Pour ajouter un prix personnalisé pour un marché ou un groupe de marchés supplémentaire, sélectionnez de nouveau **Personnaliser pour des marchés spécifiques** et répétez ces étapes. Pour modifier les marchés inclus dans un groupe de marchés, sélectionnez son nom. Pour supprimer le prix pour un groupe de marchés (ou un marché individuel), cliquez sur **Supprimer**.



