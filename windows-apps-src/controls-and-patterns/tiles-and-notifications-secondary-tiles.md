---
author: anbare
Description: "Les vignettes secondaires permettent aux utilisateurs d’épingler du contenu spécifique et des liens ciblés à partir de votre application sur le menu Démarrer pour faciliter l’accès au contenu depuis votre application."
title: Vignettes secondaires
label: Secondary tiles
template: detail.hbs
ms.author: wdg-dev-content
ms.date: 05/25/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows10, uwp, vignettes secondaires
ms.openlocfilehash: b5f81f27c1e042f45882f026ff6d20e0e5f5456b
ms.sourcegitcommit: 6396a69aab081f5c7a9a59739c83538616d3b1c7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/30/2017
---
# <a name="secondary-tiles"></a>Vignettes secondaires
<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

Les vignettes secondaires permettent aux utilisateurs d’épingler du contenu spécifique et des liens ciblés à partir de votre application sur le menu Démarrer pour faciliter l’accès au contenu depuis votre application.

![Capture d’écran de vignettes secondaires](images/secondarytiles.png)

Par exemple, les utilisateurs peuvent épingler la météo de plusieurs lieux spécifiques sur leur menu Démarrer, ce qui leur permet d’accéder (1) d’un simple coup d’œil aux informations météorologiques actuelles grâce aux vignettes dynamiques et (2) rapidement à la météo de la ville qui les intéressent. Les utilisateurs peuvent également épingler des actions spécifiques, des articles d’actualité et autres éléments qui sont importants pour eux.

En ajoutant des vignettes secondaires à votre application, vous permettez à l’utilisateur de se réengager rapidement et efficacement dans votre application, en l’incitant à revenir plus souvent grâce à la simplicité d’accès qu’offrent les vignettes secondaires.

**Seuls les utilisateurs peuvent épingler une vignette secondaire; les applications ne peuvent pas épingler de vignettes secondaires par programme sans l’approbation de l’utilisateur **. L’utilisateur doit explicitement cliquer sur un bouton «Épingler» au sein de votre application, moment auquel vous utilisez ensuite l’API à demander pour créer une vignette secondaire. Ensuite, le système affiche une boîte de dialogue demandant à l’utilisateur de confirmer s’il souhaite que la vignette soit épinglée.

## <a name="quick-links"></a>Liens rapides

| Article | Description |
| --- | --- |
| [Instructions relatives aux vignettes secondaires](tiles-and-notifications-secondary-tiles-guidance.md) | Découvrez quand et où vous devez utiliser les vignettes secondaires. |
| [Épingler les vignettes secondaires](tiles-and-notifications-secondary-tiles-pinning.md) | Découvrez comment épingler une vignette secondaire. |
| [Épingler à partir de l’application de bureau](tiles-and-notifications-secondary-tiles-desktop-pinning.md) | Les applications de bureau Windows peuvent épingler des vignettes secondaires grâce au Pont du bureau! |


## <a name="secondary-tiles-in-relation-to-primary-tiles"></a>Vignettes secondaires associées aux vignettes principales

Les vignettes secondaires sont associées à une application parente unique. Elles sont épinglées au menu Démarrer pour offrir à un utilisateur un moyen cohérent et efficace de lancer directement une zone fréquemment utilisée de l’application parente. Il peut s’agir d’une sous-section générale de l’application parente qui contient du contenu fréquemment mis à jour ou un lien ciblé vers une zone spécifique de l’application.

Voici quelques exemples de scénarios de vignettes secondaires:

* Un bulletin météorologique actualisé selon la ville dans l’application Météo
* Un récapitulatif des événements à venir dans une application de calendrier
* L’état et les mises à jour d’un contact important dans une application de réseaux sociaux
* Des flux spécifiques dans un lecteur RSS
* Une playlist
* Un blog

Tout contenu soumis à des modifications fréquentes qu’un utilisateur souhaite suivre peut faire l’objet d’une vignette secondaire. Une fois la vignette secondaire épinglée, les utilisateurs peuvent recevoir des mises à jour rapides par le biais de la vignette et l’utiliser pour accéder directement à l’application parente.

Les vignettes secondaires sont semblables aux vignettes principales à de nombreux égards:

* Elles utilisent les notifications par vignette pour afficher du contenu enrichi.
* Elles doivent inclure un logo de 150x150pixels pour le contenu de vignette par défaut.
* Elles peuvent inclure éventuellement les tailles d’autres logos pour s’adapter aux tailles de vignettes plus importantes.
* Elles peuvent afficher des notifications et des badges.
* Elles peuvent être réorganisées dans le menu Démarrer.
* Elles sont automatiquement supprimées lorsque l’application est désinstallée.
* Le texte relatif à l’état détaillé de leur verrouillage ou de leur badge peut s’afficher au verrouillage.

Toutefois, les vignettes secondaires diffèrent des vignettes principales à plusieurs égards:

* Les utilisateurs peuvent supprimer leurs vignettes secondaires à tout moment sans supprimer l’application parente.
* Les vignettes secondaires peuvent être créées en cours d’exécution. Les vignettes d’application peuvent être créées uniquement lors de l’installation.
* Un menu volant invite l’utilisateur à confirmer l’ajout d’une vignette secondaire.
* Elles ne peuvent pas être sélectionnées par programmation pour l’écran de verrouillage par le biais d’une demande faite à l’utilisateur. L’utilisateur doit ajouter manuellement le la vignette secondaire par le biais de la page Personnaliser dans les paramètres du PC.

Pour envoyer des notifications, des méthodes spécifiques sont fournies pour les outils de mises à jour par vignette et badge et les canaux de notification Push utilisés avec les vignettes secondaires. Celles-ci mettent en parallèle les versions utilisées avec les vignettes secondaires. Par exemple, CreateBadgeUpdaterForApplication par rapport à CreateBadgeUpdaterForSecondaryTile.


## <a name="guidance-on-secondary-tiles"></a>Instructions relatives aux vignettes secondaires
Pour découvrir quand et où vous devez utiliser les vignettes secondaires et d’autres instructions relatives à leur utilisation, consultez [Instructions relatives aux vignettes secondaires](tiles-and-notifications-secondary-tiles-guidance.md)


## <a name="pinning-secondary-tiles"></a>Épingler des vignettes secondaires
Pour savoir comment épingler des vignettes secondaires, consultez [Épingler des vignettes secondaires](tiles-and-notifications-secondary-tiles-pinning.md).


## <a name="desktop-applications-and-secondary-tiles"></a>Applications de bureau et vignettes secondaires
Pour savoir comment utiliser des vignettes secondaires à partir de votre application de bureau via le Pont du bureau, consultez [Épingler des vignettes secondaires à partir d’une application de bureau](tiles-and-notifications-secondary-tiles-desktop-pinning.md).