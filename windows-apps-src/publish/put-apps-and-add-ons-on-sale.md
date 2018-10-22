---
author: jnHs
Description: You can promote your app or add-on in the Microsoft Store by putting it on sale for a limited time.
title: Commercialiser des applications et des extensions
ms.assetid: 71ABA960-0CDC-4E35-A1C8-1D34B6673817
ms.author: wdg-dev-content
ms.date: 05/08/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: a25980c964e399931a088e281e959df3095ffe9a
ms.sourcegitcommit: c4d3115348c8b54fcc92aae8e18fdabc3deb301d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/22/2018
ms.locfileid: "5402517"
---
# <a name="put-apps-and-add-ons-on-sale"></a>Commercialiser des applications et des extensions

Vous pouvez promouvoir votre application ou votre extension dans le MicrosoftStore en les mettant en vente à prix réduit pendant une période limitée. Vous pouvez choisir de proposer le produit à un niveau de prix inférieur ou avec une remise en pourcentage.

> [!NOTE]
> Prix de vente n’est pas pris en charge pour les extensions d’abonnement.

Lorsque vous utilisez la section **Prix de vente** de la page **Tarification et disponibilité** d’une soumission pour réduire temporairement le prix de votre application ou de votre extension, les clients qui consultent votre description dans le WindowsStore voient apparaître un prix barré indiquant que le prix a baissé (contrairement à une [modification du prix planifiée](set-and-schedule-app-pricing.md#schedule-price-changes), qui peut diminuer ou augmenter le prix sans le signaler en tant que modification dans le WindowsStore). 

Pendant la période de commercialisation de votre produit, les clients peuvent acheter ce dernier au prix inférieur au cours de la période que vous avez sélectionnée. Si vous définissez le prix sur **Gratuit**, les clients peuvent télécharger le produit sans rien payer pendant la période de vente.

> [!IMPORTANT]
> Le prix de vente s’affiche uniquement pour vos clients qui utilisent des appareils Windows10, y compris XboxOne. Les ventes proposées aux possesseurs de l’un de vos autres produits sont uniquement présentées aux clients qui utilisent Windows10, version1607 ou ultérieure.
> 
> Sur les autres systèmes d’exploitation, les clients voient le prix normal de votre application ou de votre extension et ne seront pas en mesure d’acheter ces dernières au prix de vente. Vous pouvez toujours modifier un prix en choisissant un autre niveau de prix dans une nouvelle soumission, mais il ne sera pas affiché en tant que vente à prix réduit limitée dans le temps.


## <a name="scheduling-a-sale"></a>Planification d’une vente à prix réduit

Les ventes à prix réduit sont planifiées dans le cadre de la soumission d’une application ou d’un module complémentaire. Si vous souhaitez planifier une vente à prix réduit pour une application ou un module complémentaire déjà publié(e), vous devez créer une nouvelle soumission, même s’il s’agit de la seule modification que vous voulez apporter.

**Pour planifier une vente**

1. Sur la page **Tarification et disponibilité** d’une soumission d’application ou de module complémentaire en cours, accédez à la section **Prix de vente**.
2. Sélectionnez **Afficher les options**, puis sélectionnez **Nouvelle vente**.
3. La fenêtre contextuelle **Sélection du marché** s’affiche, ce qui vous permet de créer un *Groupe de marchés* qui spécifie le ou les marchés dans lesquels la vente doit être proposée. Vous pouvez cliquer sur **Tout sélectionner** pour proposer la vente à tous les marchés dans lesquels votre application est disponible, ou bien sélectionner un marché particulier ou plusieurs marchés. Vous pouvez éventuellement saisir un nom pour votre groupe de marchés. Lorsque vous avez effectué vos sélections, cliquez sur **Créer**. (Pour modifier les marchés dans le groupe ultérieurement, cliquez sur son nom.)

   > [!NOTE]
   > Les sélections de marché que vous effectuez dans la section Prix de vente n’affectent pas les marchés dans lesquels l’application est proposée. Ces sélections déterminent uniquement si un prix de vente est proposé et dans quels marchés. Si vous définissez un prix de vente pour un marché dans lequel votre application n’est pas disponible, cela n’a pas pour effet de la rendre disponible sur ce marché.
4. Choisissez l’une des options suivantes pour spécifier le type de remise:
   - **Prix**: utilisez cette option pour sélectionner un niveau de prix inférieur, auquel votre application sera proposée. Vous pouvez modifier la devise dans la liste déroulante pour sélectionner le prix dans la devise que vous préférez. (Le prix sera converti au niveau correspondant pour chaque devise. Pour plus d’informations, voir [Tarification](set-app-pricing-and-availability.md).)
   - **Pourcentage**: utilisez cette option pour sélectionner le pourcentage d’une remise qui sera appliquée à votre application. Le même pourcentage de remise est utilisé pour toutes les devises.
5. Sur la ligne **Proposé à**, choisissez l’une des options disponibles suivantes:
   - **Tout le monde**: la vente sera proposée à tous les clients.
   - **Propriétaires de**: la vente sera proposée aux clients qui possèdent déjà l’une de vos applications. Vous pouvez sélectionner des applications publiées dans la liste déroulante qui s’affiche. Pour que cette option soit disponible, vous devez disposer d’une ou de plusieurs applications publiées.

  > [!IMPORTANT]
  > Si vous sélectionnez **Propriétaires de**, la vente sera uniquement visible par les clients qui utilisent Windows10, version1607 ou ultérieure.

   - **Known user group**: la vente sera proposée aux membres du [groupe d’utilisateurs connus](create-known-user-groups.md) que vous sélectionnez. Pour que cette option soit disponible, vous devez avoir déjà créé le groupe d’utilisateurs connus.
   - **Segment**: la vente sera proposée aux personnes présentes dans le segment de clients que vous sélectionnez. Vous pouvez utiliser un [segment que vous avez déjà créé](create-customer-segments.md) ici. Vous pouvez également choisir **Payeur pour la première fois** pour proposer la vente uniquement aux clients qui n'ont jamais rien acheté dans le Windows Store. Nous proposons ici ce segment, car nous avons constaté qu’une fois qu'un client a effectué un premier achat dans le Windows Store, il en fait souvent d'autres. Cela vaut donc la peine d'attirer ce groupe avec un prix de vente.
6. Définissez les dates et heures de début et de fin de la période de vente à prix réduit. Choisissez l'une des options de fuseau horaire:
   - **UTC**: l'heure sélectionnée correspondra à l'heure universelle coordonnée (UTC), afin que la vente soit proposée partout en même temps.
   - **Local**: l'heure sélectionnée sera utilisée dans chaque fuseau horaire associé à un marché. (Notez que pour les marchés qui incluent plusieurs fuseaux horaires, seul un fuseau horaire sur ce marché sera utilisé. Pour les États-Unis, le fuseau horaire Côte Est est utilisé.)
7. Pour planifier une vente supplémentaire, sélectionnez **Nouvelle vente**. Autrement, cliquez sur **Enregistrer** en bas de la **page Tarification et disponibilité**, puis sélectionnez **Envoyer au Windows Store** dans l’aperçu de la soumission.

> [!NOTE]
> Il est possible de sélectionner un niveau de prix supérieur au prix de base de votre application. Toutefois, le prix de vente s’affiche aux clients uniquement s’il est inférieur au prix normal de l’application sur ce marché.
>
> La sélection d’un prix supérieur au prix de base de votre application peut être judicieuse pour votre vente si vous avez déjà défini des prix personnalisés pour certains marchés, qui sont supérieurs au prix de base de votre application, et souhaitez réduire temporairement le prix sur ces marchés (tout en conservant un prix de vente supérieur au prix de base de l’application). Si vos sélections ont pour effet d’augmenter le prix de l’application sur un certain marché, nous ne reflétons pas ce prix (supérieur) aux clients de ce marché, qui continuent à voir l’application proposée à son prix antérieur (inférieur). Nous montrons également aux clients le prix le plus bas disponible si vous planifiez des ventes à prix réduit qui se chevauchent avec des prix différents.

## <a name="changing-or-canceling-a-scheduled-sale"></a>Modification ou annulation d’une vente à prix réduit planifiée

Pour réviser ou annuler une vente à prix réduit que vous avez précédemment planifiée pour une application ou un module complémentaire, vous devez créer une nouvelle soumission et la soumettre au Store.

**Pour modifier une vente planifiée**

1.  Sur la page **Tarification et disponibilité** d’une soumission d’application ou de module complémentaire en cours, accédez à la section **Prix de vente**.
2.  Recherchez la vente que vous souhaitez mettre à jour, puis apportez vos modifications.
3.  Cliquez sur **Enregistrer** en bas de la page **Tarification et disponibilité**, puis cliquez sur **Envoyer au Windows Store** dans l’aperçu de la soumission.

Une fois votre soumission certifiée, les modifications prennent effet.

> [!IMPORTANT]
> Si une vente a déjà démarré, vous ne serez pas en mesure de modifier la date de début. Bien qu'il soit possible de modifier la date de fin d'une vente, nous recommandons de ne pas la modifier à une date antérieure à celle d’origine. Si vous mettez fin à une vente avant la date initialement publiée, cela peut être frustrant pour vos clients potentiels (étant donné qu'ils voient la date de fin planifiée lorsqu'ils consultent la description de votre application dans le Windows Store).

 **Pour annuler une vente qui n’a pas encore démarré**

1.  Sur la page **Tarification et disponibilité** d’une soumission d’application ou de module complémentaire en cours, accédez à la section **Prix de vente**.
2.  Recherchez la vente à annuler, puis cliquez sur **Supprimer**.
3.  Cliquez sur **Enregistrer** en bas de la page **Tarification et disponibilité**, puis cliquez sur **Envoyer au Windows Store** dans l’aperçu de la soumission. Si la vente n’a pas démarré à la fin du processus de certification de la nouvelle soumission, la vente supprimée n’est pas exécutée du tout.




