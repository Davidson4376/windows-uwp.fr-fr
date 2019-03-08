---
title: Succès 2017
description: Décrit comment vous pouvez configurer des primes dans partenaires pour fournir des récompenses.
ms.assetid: ''
ms.date: 11/10/2017
ms.topic: article
ms.localizationpriority: medium
keywords: Xbox live, xbox, jeux, uwp, windows 10, xbox une, udc, centre de développement universelle
ms.openlocfilehash: 9ef2365ee560e273108c38570697d599adde4c0b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57655574"
---
# <a name="configure-achievements-2017-in-partner-center"></a>Configurer des primes 2017 dans Partner Center

> [!IMPORTANT]
> Primes sont uniquement applicables à ID@Xbox ou partenaires gérés. Jeux de participer au programme Xbox Live Creators ne sont pas pris en charge.

Vous pouvez utiliser [partenaires](https://partner.microsoft.com/dashboard) pour configurer le [primes 2017](../../achievements-2017/simplified-achievements.md) qui sont associés à votre jeu. Ajoutez une nouvelle avancée en procédant comme suit :

1. Accédez à la section des primes pour votre titre, situé sous **Services** > **Xbox Live** > **primes**.
2. Cliquez sur le **nouvelle prime** bouton et remplissez le formulaire.  Une fois terminé, cliquez sur **enregistrer**.

![Capture d’écran pour créer une prime en nouveau centre de partenaires](../../images/dev-center/achievement-table.png)

## <a name="description"></a>Description
La section de description est où vous pouvez entrer les principes fondamentaux de votre réussite, comme le nom et les descriptions de verrouillage/déverrouillage. Vous pouvez ajouter la prise en charge de la localisation à primes en accédant à la **des chaînes localisées** service section de configuration dans [partenaires](https://partner.microsoft.com/dashboard).

![Capture d’écran des champs description lors de la configuration d’une prime en nouveau centre de partenaires](../../images/dev-center/achievements-2.png)

Le **prime nom** champ est le nom public en vis-à-vis de la prime.

Le **Description verrouillé** champ est la description que les joueurs verront quand ils ont déverrouillé pas la prime. Il a une limite de 100 caractères.

Le **Description déverrouillé** champ est la description de joueurs seront affiche une fois qu’ils ont déverrouillé la réalisation. Il a une limite de 100 caractères.

## <a name="details"></a>Détails
La section de détails est utilisée pour associer des informations importantes telles que l’image, le type de prime, l’avantage lié au score de joueur (le cas échéant) et indique si la prime doit être masquée jusqu'à ce que déverrouillé.

![Capture d’écran de champs de détails lorsque vous configurez une prime en nouveau centre de partenaires](../../images/dev-center/achievements-3.png)

Le **icône d’Image** champ est l’image qui sera affichée à côté de la prime. Il doit être un png de 1920 x 1080.

**Baser les primes** sont disponibles pour vos joueurs quand le jeu initial a été publié. **Non-Base primes** sont disponibles une fois que le jeu initial a publié (par exemple, avec DLC du nouveau contenu).

Le **Gamescore** champ est la quantité de points de score de joueur votre réussite vous attribuons lorsque déverrouillées. Chaque prime pouvez récompenser entre 0-200 points.  

**Public** primes sont visibles par tous les acteurs, quel que soit le si elles ont déverrouillé le succès ou non. **Clé secrète** primes sont masqués jusqu'à ce qu’ils ont été déverrouillés.

**Lien ciblé de réalisation** est un moyen pour vous permettre de récupérer un paramètre à partir de la prime qui vous permet de lier à un endroit de votre jeu sur où la prime, vous pouvez gagner. Le lien profond est retourné dans la réponse de l’API GET. L’URL spécifiée doit contenir `ms-xbl-{titleID}://` préfixe.

> [!TIP]
> Réalisation des liens ciblés nécessitent le n ° titre hexadécimal de votre jeu. Vous pouvez le trouver sur [le programme d’installation de Xbox Live](xbox-live-setup.md) écran dans [partenaires](https://developer.microsoft.com/dashboard).

## <a name="additional-rewards"></a>Récompenses supplémentaires
Dans certains cas, vous souhaiteriez offrir une récompense de dans le jeu ou d’une illustration lorsqu’un joueur déverrouille une prime. Vous pouvez définir les récompenses (le cas échéant) qui sont associés à une prime dans le **récompenses supplémentaires** section. Une prime peut contenir deux récompenses supplémentaires - un de chaque type de récompense. Vous trouverez plus d’informations sur la [prime récompenses](../../achievements-2017/achievement-rewards.md) article.

Pour créer une récompense de nouveau, cliquez sur le **Reward ajouter** situé dans le **récompenses supplémentaires** section et remplissez le formulaire.

![Capture d’écran de l’ajout de récompenses à une prime de partenaires](../../images/dev-center/achievement-reward.png)

### <a name="reward-details"></a>Détails de la récompense
Remplissez les détails de la récompense pour associer une récompense de nouveau. Une fois terminé, cliquez sur **ajouter**.

![Capture d’écran de configuration des détails du prix pour une prime Partner Center](../../images/dev-center/achievements-5.png)

Il existe deux types de récompenses de réalisation peuvent être créés. Ils sont les suivants :

1. Le **Art** type peut être utilisé si vous souhaitez récompenser le joueur des choses telles que la haute résolution concept art, dessins anticipée, créée spécialement les composants des illustrations ou autres images numériques. Composants des illustrations sont affichés dans le tableau de bord Xbox One et peuvent également être affichés dans le Guide des expériences en interrogeant le service de primes.
2. Le **dans le jeu** type peut être utilisé si vous souhaitez récompenser le lecteur avec les récompenses dans le jeu personnalisés sans mettre à jour votre titre. Certains scénarios potentiels sont supplémentaires dans le jeu devise/points ou accès aux mappages, armes ou caractères spéciaux.

Le **surnom** champ est le nom de la récompense joueurs seront affiche. Il a une limite de 57 caractères.

Le **Description** champ est la description de la récompense que joueurs seront affiche et qu’il doivent inclure des instructions pour l’échange. Il a une limite de 90 caractères.

Le **Image** ou **Art** champ est l’image associée à la récompense. Si le type est défini sur l’art, il s’agit de l’illustration qu’ils seront récompensés. Sinon, il représentera la récompense de la partie qui s’affichera. L’image doit être un png de 1920 x 1080.

Le **valeur dans le jeu** champ est visible uniquement si vous sélectionnez le type de récompense de **dans le jeu**. Il est utilisé pour spécifier la valeur qui est passée au code de votre jeu qui peut être utilisé pour déverrouiller la récompense dans le jeu.
