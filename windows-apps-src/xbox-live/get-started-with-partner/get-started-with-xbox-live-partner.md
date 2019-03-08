---
title: Prise en main Xbox Live en tant que partenaire
description: Fournit des liens pour aider un partenaire géré ou ID@Xbox membre prise en main développement Xbox Live.
ms.assetid: 69ab75d1-c0e7-4bad-bf8f-5df5cfce13d5
ms.date: 06/07/2017
ms.topic: article
keywords: Xbox live, xbox, jeux, uwp, windows 10, xbox une, partenaire, ID@Xbox
ms.localizationpriority: medium
ms.openlocfilehash: 1a219ee6f4e1dc80ead893c4e230d70e8caa2400
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57659824"
---
# <a name="get-started-with-xbox-live-as-a-managed-partner-or-an-idxbox-developer"></a>Prise en main Xbox Live comme étant un partenaire géré ou un ID@Xbox développeur

Cette section décrit la prise en main Xbox Live sous la forme d’un partenaire géré ou un ID@Xbox développeur. Partenaires et développeurs de code peuvent accéder aux fonctionnalités plage complète de Xbox Live, y compris les primes, le mode multijoueur et bien plus encore.

Gérés partenaires et ID@Xbox les développeurs peuvent développer des titres de Xbox Live pour la plateforme universelle Windows (UWP) ou de la plateforme du Kit de développement Xbox (XDK).

En plus du contenu disponible ici, il existe également une documentation supplémentaire qui est disponible pour les partenaires avec un compte autorisé de partenaires. Vous pouvez accéder à ce contenu ici : [Contenu de partenaire Xbox Live](https://developer.microsoft.com/en-us/games/xbox/docs/xboxlive/xbox-live-partners/partner-content).

## <a name="why-should-you-use-xbox-live"></a>Pourquoi devez-vous utiliser Xbox Live ?

Xbox Live propose un large éventail de fonctionnalités conçues pour aider à promouvoir votre jeu et de conserver les joueurs engagés :

- Xbox Live de plate-forme sociale permet aux joueurs vous connecter avec vos amis et parler de votre jeu.
- Primes Xbox Live permet de concevoir votre jeu populaires en donnant votre promotion jeu gratuite pour le graphique social Xbox Live lorsque les joueurs déverrouillez primes.
- Classements de Xbox Live permet de favoriser l’engagement de votre jeu en permettant aux joueurs rivaliser pour battre leurs amis et déplacer vers le haut les rangs.
- Multijoueurs Xbox Live permet aux joueurs de jouer avec leurs amis ou une opération get mis en correspondance avec d’autres joueurs à Combattez ou dans votre jeu.
- Xbox Live connectés à des offres de stockage libre enregistrée itinérance entre les appareils afin que les joueurs peuvent continuer facilement jeu progression entre Xbox One et les PC Windows.

## <a name="1-choose-a-platform"></a>1. Choisissez une plateforme
Décider d’apporter un Kit de développement Xbox (XDK), Universal Windows Platform (UWP) ou jeu de cross-play :

- XDK en fonction des jeux de s’exécuter uniquement sur la console Xbox One
- Jeux UWP peuvent cibler n’importe quelle plateforme Windows tels que les PC Windows, Windows Phone ou Xbox One
  - Pour Xbox One, consultez [UWP sur Xbox One](https://msdn.microsoft.com/en-us/windows/uwp/xbox-apps/index) et spécifiquement [ressources système pour les applications UWP et des jeux sur Xbox One](https://msdn.microsoft.com/en-us/windows/uwp/xbox-apps/system-resource-allocation)
- Jeux de cross-play sont généralement les jeux qui ciblent une Xbox et PC Windows à l’aide de chemins d’accès à la fois le XDK et UWP.

## <a name="2-ensure-that-you-have-a-title-created-in-partner-center-or-xdp"></a>2. Assurez-vous d’avoir un titre créé dans l’espace partenaires ou XDP
Chaque titre de Xbox Live doit être défini dans le centre de partenaires ou le portail de développement Xbox (XDP) avant que vous serez en mesure de se connecter et effectuer des appels de Service Xbox Live.  [Création d’un nouveau titre](create-a-new-title.md) vous montrer comment effectuer cette opération.

## <a name="3-follow-the-appropriate-guide-to-setup-your-ide-or-game-engine"></a>3. Suivez le guide approprié pour le programme d’installation de votre IDE ou le moteur de jeu
Vous pouvez suivre le guide de mise en route approprié pour votre plateforme et le moteur et découvrez les principes fondamentaux de Xbox Live fur.

* [Mise en route à l’aide de Visual Studio pour les jeux UWP](get-started-with-visual-studio-and-uwp.md) vous montrera comment lier votre projet Visual Studio avec votre configuration Xbox Live dans Partner Center.
* [Mise en route à l’aide d’Unity pour les jeux UWP](partner-add-xbox-live-to-unity-uwp.md) sera vous indiquent comment créer une nouvelle Xbox Live activé titre Unity, ajoutez des fonctionnalités telles que des tableaux de résultats à votre titre et générez un projet Visual Studio natif.
* [Mise en route à l’aide de Visual Studio pour XDK des jeux fonctionnant sous](xdk-developers.md) vous montrera comment obtenir votre configuration de projet Visual Studio si vous apportez un Xbox One titre à l’aide du XDK.
* [Rendre prise en main obtention un jeu de cross-play](get-started-with-cross-play-games.md) explique comment rendre un produit qui est un jeu en fonction de XDK pour Xbox One et un basés sur UWP jeux pour PC Windows 10.

## <a name="4-xbox-live-concepts"></a>4. Concepts de Xbox Live
Une fois que vous avez un titre créé, vous devez en savoir plus sur les concepts de Xbox Live qui affecteront votre expérience de développement de titres :

- [Les bacs à sable Xbox Live](../xbox-live-sandboxes.md)
- [Comptes de test de Xbox Live](../xbox-live-test-accounts.md)
- [Introduction aux API Live Xbox](../introduction-to-xbox-live-apis.md)

## <a name="5-add-xbox-live-features"></a>5. Ajouter des fonctionnalités Xbox Live

Ajouter des fonctionnalités Xbox Live à votre jeu :

- [Xbox Live plate-forme sociale - profil, vos amis, présence](../social-platform/social-platform.md)
- [Xbox Live des tableaux de résultats de la plateforme - statistiques, de données, primes](../data-platform/data-platform.md)
- [Xbox Live plateforme multijoueur - invitation, Matchmaking, tournois](../multiplayer/multiplayer-intro.md)
- [Xbox Live plateforme - stockage connecté, titre de stockage](../storage-platform/storage-platform.md)
- [Effectuer une recherche contextuelle](../contextual-search/introduction-to-contextual-search.md)

## <a name="6-release-your-title"></a>6. Votre titre de la version

ID@Xbox et des partenaires gérés doivent passer par un processus de certification avant de libérer leurs titres.  Il s’agit, car les titres ont accès à des fonctionnalités supplémentaires telles que le mode multijoueur en ligne, des primes et présence riche.  Une fois que vous êtes prêt à publier votre titre, contactez votre représentant Microsoft pour plus d’informations sur le processus de soumission et la mise en production.
