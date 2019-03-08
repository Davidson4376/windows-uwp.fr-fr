---
title: Guide étape par étape pour les partenaires de Xbox Live
description: Fournit des indications d’étapes pour intégrer la Xbox Live pour les partenaires gérés.
ms.assetid: f0c9db8f-f492-4955-8622-e1736f0a4509
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 9a2d0b9e7d97adfb02281e7bae34ed51afd44f7f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57660844"
---
# <a name="step-by-step-guide-to-integrate-xbox-live-for-managed-partners-and-idxbox-members"></a>Guide étape par étape pour intégrer la Xbox Live pour les partenaires gérés et ID@Xbox membres

Cette section vous aideront à être rapidement opérationnel avec Xbox Live :

## <a name="1-choose-a-platform"></a>1. Choisissez une plateforme
Décider d’apporter un Kit de développement Xbox (XDK), Universal Windows Platform (UWP) ou jeu de cross-play.

- XDK en fonction des jeux de s’exécuter uniquement sur la console Xbox One
- Jeux UWP peuvent cibler n’importe quelle plateforme Windows tels que les PC Windows, Windows Phone ou Xbox One
  - Pour Xbox One, consultez [UWP sur Xbox One](https://msdn.microsoft.com/en-us/windows/uwp/xbox-apps/index) et spécifiquement [ressources système pour les applications UWP et des jeux sur Xbox One](https://msdn.microsoft.com/en-us/windows/uwp/xbox-apps/system-resource-allocation)
- Jeux de cross-play sont généralement les jeux qui ciblent une Xbox et PC Windows à l’aide de chemins d’accès à la fois le XDK et UWP.

## <a name="2-ensure-you-have-a-title-created-in-partner-center-or-xdp"></a>2. Assurez-vous de qu'avoir un titre créé dans l’espace partenaires ou XDP
Chaque titre de Xbox Live doit être défini dans le centre de partenaires ou le portail de développement Xbox (XDP) avant que vous serez en mesure de se connecter et effectuer des appels de Service Xbox Live.  [Création d’un nouveau titre](create-a-new-title.md) vous montrer comment effectuer cette opération.

## <a name="3-follow-the-appropriate-guide-to-setup-your-ide-or-game-engine"></a>3. Suivez le guide approprié pour le programme d’installation de votre IDE ou le moteur de jeu
Vous pouvez suivre le guide de mise en route approprié pour votre plateforme et le moteur et découvrez les principes fondamentaux de Xbox Live fur.

* [Mise en route à l’aide de Visual Studio pour les jeux UWP](get-started-with-visual-studio-and-uwp.md) vous montrera comment lier votre projet Visual Studio avec votre configuration Xbox Live dans Partner Center.

* [Mise en route à l’aide d’Unity pour les jeux UWP](partner-add-xbox-live-to-unity-uwp.md) sera vous indiquent comment créer une nouvelle Xbox Live activé titre Unity, ajoutez des fonctionnalités telles que des tableaux de résultats à votre titre et générez un projet Visual Studio natif.

* [Mise en route à l’aide de Visual Studio pour XDK des jeux fonctionnant sous](xdk-developers.md) vous montrera comment obtenir votre configuration de projet Visual Studio si vous apportez un Xbox One titre à l’aide du XDK.

* [Rendre prise en main obtention un jeu de cross-play](get-started-with-cross-play-games.md) explique comment rendre un produit qui est un jeu en fonction de XDK pour Xbox One et un basés sur UWP jeux pour PC Windows 10.

## <a name="4-xbox-live-concepts"></a>4. Concepts de Xbox Live
Une fois que vous avez créé un titre, vous devez apprendre les concepts Xbox Live qui vont affecter votre expérience de développement de titres.

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

ID@Xbox titres et les titres publiés par le fabricant du jeu s’agit-il d’un Microsoft Partner doit passer par un processus de certification avant la mise en production.  Il s’agit, car ces titres ont accès à des fonctionnalités supplémentaires telles que le mode multijoueur, les primes et présence enrichie.  Une fois que vous êtes prêt à publier votre titre, contactez votre représentant Microsoft pour plus d’informations sur le processus de soumission et la mise en production.
