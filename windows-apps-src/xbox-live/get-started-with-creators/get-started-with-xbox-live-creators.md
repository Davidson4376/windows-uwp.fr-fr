---
title: Prise en main du programme Créateurs Xbox Live
description: Fournit des liens pour vous aider à vous lancer avec le programme Créateurs Xbox Live.
ms.assetid: 2a744405-7ee4-42b4-8f36-9916e8c3a530
ms.date: 12/13/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: cce9d34679884d48475b7a7ae0fa8286204a6289
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57615874"
---
# <a name="get-started-with-the-xbox-live-creators-program"></a>Prise en main du programme Créateurs Xbox Live
 
Le programme Créateurs Xbox Live vous permet de publier rapidement et directement vos jeux sur Xbox One et Windows 10 grâce à un processus de certification simplifié n'exigeant aucune approbation du concept. Si votre jeu s'intègre à Xbox Live et respecte nos [stratégies standard du Store](https://msdn.microsoft.com/en-us/library/windows/apps/dn764944.aspx), vous êtes prêt à publier. Cet article décrit les étapes à suivre pour mettre votre au jeu au point avec l'intégration à Xbox Live. 

Les jeux du programme Créateurs Xbox Live doivent être des applications de plateforme Windows universelle (UWP). Pour Xbox One, voir [UWP sur Xbox One](https://msdn.microsoft.com/en-us/windows/uwp/xbox-apps/index) et plus spécifiquement [Ressources système pour les applications UWP et les jeux sur Xbox One](https://msdn.microsoft.com/en-us/windows/uwp/xbox-apps/system-resource-allocation). Les jeux publiées par l'intermédiaire du programme Créateurs Xbox Live ne peuvent pas accéder aux succès ni aux services multijoueur en ligne. Pour obtenir une liste complète des services pris en charge, voir la [Vue d’ensemble du programme pour les développeurs : tableau des fonctionnalités](https://docs.microsoft.com/en-us/windows/uwp/xbox-live/developer-program-overview#feature-table).

## <a name="1-ensure-you-have-a-title-created-in-partner-center"></a>1. Assurez-vous de qu'avoir un titre créé dans l’espace partenaires
Chaque titre de Xbox Live doit être défini dans [partenaires](https://partner.microsoft.com/dashboard) avant que vous serez en mesure de se connecter et effectuer des appels de Service Xbox Live.  [Création d’un nouveau titre Créateurs](create-and-test-a-new-creators-title.md) vous montre comment procéder.

## <a name="2-follow-the-appropriate-guide-to-setup-your-ide-or-game-engine"></a>2. Suivez le guide approprié pour le programme d’installation de votre IDE ou le moteur de jeu
Vous pouvez suivre le « guide de prise en main » approprié pour votre plateforme et votre moteur et apprendre les fondamentaux de Xbox Live au fur et à mesure :

* [Développer un titre de créateurs avec Visual Studio](develop-creators-title-with-visual-studio.md) vous montrera comment lier votre projet Visual Studio avec votre configuration Xbox Live dans Partner Center.
* [Développer un titre Créateurs avec Unity](develop-creators-title-with-unity.md) vous montre comment créer un nouveau jeu Xbox Live fondé sur Unity, gérer la connexion utilisateur unique et multiutilisateur, ajouter des fonctionnalités telles que les statistiques et les classements et générer un projet Visual Studio natif.

Bien que Unity soit le seul moteur de jeu tiers pour lequel nous fournissons de la documentation, les moteurs de jeu [Construct (2 & 3)](https://www.scirra.com/construct2) et [Game Studio Maker](https://www.yoyogames.com/gamemaker) disposent également d'une documentation pour vous aider à intégrer Xbox Live dans votre jeu Construct ou Game Maker Studio, respectivement.

* [Game Maker Studio 2 UWP prend désormais en charge le programme Créateurs Xbox Live](https://www.yoyogames.com/gamemaker/xblc) vous montre comment exporter vos projets Game Maker Studio pour jouer sur Xbox One et PC Windows 10.
* [Utilisation de Xbox Live dans les applications UWP - Construct](https://www.scirra.com/tutorials/9540/using-xbox-live-in-uwp-apps) vous montre comment utiliser Xbox Live dans vos jeux Construct 2 et 3.

Pour les autres moteurs de développement de jeux qui n'ont pas d'intégration à Xbox Live documentée, vous pouvez toujours utiliser les API Xbox Live pour ajouter Xbox Live à votre titre. Pour utiliser l'API Xbox Live de votre projet, vous pouvez soit ajouter des références aux fichiers binaires avec des packages NuGet ou ajouter la source de l’API. L'ajout de packages NuGet rend la compilation plus rapide, alors que l’ajout de la source facilite le débogage.

Pour obtenir de l'aide du sujet de l'utilisation des services Xbox Live avec des moteurs de jeu tiers qui ne sont pas Unity, collaborez avec les équipes concernées pour avoir des réponses à vos questions.

## <a name="3-xbox-live-concepts--testing"></a>3. Concepts de Xbox Live et test
Une fois que vous avez créé un titre, vous devez apprendre les concepts Xbox Live qui vont affecter votre expérience de développement de titres. Il est également important de tester votre jeu sur toutes les plateformes qu'il prendra en charge, pour vous assurer qu’il se comporte comme prévu.

- [Configuration du service Xbox Live pour le programme Creators](xbox-live-service-configuration-creators.md)
- [Environnement de test de Xbox Live](../xbox-live-sandboxes.md)
- [Autoriser des comptes de Xbox Live](authorize-xbox-live-accounts.md)

## <a name="4-enable-xbox-live-sign-in"></a>4. Activer la Xbox Live connexion
Tous les jeux du programme Créateurs Xbox Live doivent intégrer la connexion à Xbox Live et afficher l’identité de l’utilisateur (Gamertag, Gamerpic, etc.). Vous pouvez choisir de connecter l’utilisateur automatiquement ou de lui demander d'appuyer sur un bouton pour activer la connexion. L’identité Gamertag doit être affichée dès la connexion, afin que le joueur puisse vérifier qu'il utilise le bon profil.

- [Xbox Live plate-forme sociale - profil, vos amis, présence](../social-platform/social-platform.md)

## <a name="5-add-optional-xbox-live-features"></a>5. Ajouter des fonctionnalités facultatives de Xbox Live

Le programme Créateurs Xbox Live offre un ensemble de fonctionnalités conçues pour vous aider à promouvoir votre jeu et stimuler l'intérêt des joueurs :

- [Plateforme de données Xbox Live - Statistiques, classements](../data-platform/data-platform.md) aide à stimuler l'intérêt pour votre jeu en permettant aux utilisateurs de jouer en compétition avec leurs amis et de monter dans les classements.
- [Plateforme de stockage Xbox Live - Stockage connectés, Stockage de titres](../storage-platform/storage-platform.md) permet l'itinérance gratuite entre parties enregistrées sur différents appareils .Ainsi, les joueurs peuvent poursuivre leur progression dans le jeu entre la console Xbox One et le PC Windows.
- [Plateforme sociale Xbox Live - Profil, amis, présence](../social-platform/social-platform.md) permet aux joueurs de se connecter avec leurs amis et de parler de leur jeu.

Il est important de noter que le programme Créateurs Xbox Live ne prend pas en charge le mode multijoueur en ligne, les succès ou les scores de joueur.

## <a name="6-release-your-game"></a>6. Votre jeu de mise en production

Si vous utilisez le programme Créateurs Xbox Live, le processus n’est pas différent de celui de la publication de n'importe quelle autre application UWP :

- [Politiques du Microsoft Store](https://msdn.microsoft.com/en-us/library/windows/apps/dn764944.aspx) notamment [Politiques Xbox et en matière de jeu](https://msdn.microsoft.com/en-us/library/windows/apps/dn764944.aspx#pol_10_13)
- [Publier des applications Windows](https://developer.microsoft.com/en-us/store/publish-apps)