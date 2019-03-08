---
title: Connectez-vous avec le préfabriqué PlayerAuthentication
description: Vue d’ensemble de la préfabriqué de plug-in PlayerAuthentication Unity
ms.date: 05/08/2018
ms.topic: get-started-article
keywords: Xbox live, xbox, jeux, uwp, windows 10, xbox une, unity
ms.openlocfilehash: ea161ff801e2004569d88d53c78ae963e91b4ce6
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57605734"
---
# <a name="easy-sign-in-with-the-playerauthentication-prefab"></a>Connectez-vous facilement avec le préfabriqué PlayerAuthentication

Le préfabriqué PlayerAuthentication le plus simple consiste à ajouter de Xbox Live de l’authentification à votre titre. Ne prend que trois étapes simples pour accéder à partir d’une nouvelle scène à une page de connexion.

1. Le préfabriqué PlayerAuthentication faire glisser la scène
2. Faire glisser un préfabriqué XboxLiveServices la scène
3. Ajouter un EventSystem à la scène (techniquement, la PlayerAuthentication crée un pour vous si un EventSystem n’est pas présent, mais il ajout est une bonne habitude.)

Et voilà. Vous pouvez désormais vous connecter un lecteur dans XboxLive votre titre en cliquant sur le préfabriqué PlayerAuthentication dans votre scène. Test de votre scène dans Unity en cliquant sur le bouton de lecture entraîne votre préfabriqué générer des données fictives, c’est parce que le lecteur Unity ne peut pas se connecter au service Xbox Live. Pour voir une réelle de connexion à, vous devez générer votre projet doit être exécuté localement dans Visual Studio. Si votre titre a été configuré dans Centre partenaires et vous avez autorisé un compte Microsoft/le gamertag se connecter à votre titre, puis vous serez en mesure de vous connecter à un de vos comptes autorisés dans une build de Visual Studio.

Script du préfabriqué de PlayerAuthentication a quelques paramètres que vous pouvez manipuler à partir de son affichage dans l’inspecteur.

![Capture d’écran de PlayerAuthentication inspecteur](../images/unity/playerauthentication_prefab_inspector.JPG)

* Nombre de lecteur - déterminent le lecteur qui est lié à la connexion-dans Panneau de configuration
* Thème : modifie le jeu de couleurs pour le panneau de connexion pour lorsqu’un utilisateur est connecté ou déconnecté. Ce paramètre a une option claire ou sombre.
* Activer contrôleur entrée - permet de cette case à cocher pour les joueurs à utiliser une manette Xbox pour la connexion et de déconnexion à l’aide de la préfabriqué PlayerAuthentication.
* Nombre de la manette de jeu - déterminent le contrôleur qui peut sign-in notre hors à l’aide de la préfabriqué.
* Bouton d’inscription : une liste déroulante qui vous permet de choisir quel bouton sur un contrôleur Xbox se connecte un utilisateur.
* Déconnectez-vous de bouton : une liste déroulante qui vous permet de choisir quel bouton sur un contrôleur Xbox déconnecte un utilisateur.

## <a name="multiplayer-scenario"></a>Scénario multijoueur

En plus de lecteur unique connexion, vous pouvez également utiliser plusieurs PlayerAuthentication prefabs pour implémenter le mode multijoueur local sur les titres de console Xbox One. En ajoutant plusieurs instances de la préfabriqué et en modifiant l’attribut de numéro de lecteur de chacun d’eux, vous pouvez vous connecter plusieurs utilisateurs à votre titre.

> [!WARNING]
> Plusieurs identités de signature n’est pas autorisée sur les PC Windows 10. Pour vous connecter plusieurs utilisateurs, vous devez tester votre jeu sur une Xbox une seule Console.

Création d’une scène permet multijoueur n’est légèrement plus difficile à l’aide de la préfabriqué PlayerAuthentication.

1. Faire glisser une instance de la préfabriqué PlayerAuthentication la scène
2. Vérifier le **activer une entrée de contrôleur** zone dans l’inspecteur du préfabriqué.
3. Assurez-vous que le **Player nombre** et **Joystick nombre** sont définis sur 1.
4. Affecter le **bouton de connexion** dans le menu déroulant.
5. Affecter le **Sign Out bouton** dans le menu déroulant.
6. Faites glisser un *deuxième* instance de la préfabriqué PlayerAuthentication sur la scène.
7. Vérifier le **activer une entrée de contrôleur** zone dans l’inspecteur du préfabriqué.
8. Assurez-vous que le **Player nombre** et **Joystick nombre** sont définies sur 2.
9. Affecter le **bouton de connexion** dans le menu déroulant.
10. Affecter le **Sign Out bouton** dans le menu déroulant.
11. Faire glisser un préfabriqué XboxLiveServices la scène
12. Ajouter un EventSystem à la scène

Vérifiez que les prefabs sont fonctionne correctement en appuyant sur la lecture dans le lecteur Unity et en cliquant sur les prefabs. Elles retournent des données fictives qui sont prévues que le lecteur Unity ne peut pas se connecter à Xbox Live. Avec deux instances de la préfabriqué PlayerAuthentication configuré pour les joueurs et la manette que vous êtes prêt à créer votre jeu dans Visual Studio, il peut être correctement testé sur une Xbox Console. Une fois que votre jeu est créé, ouvrez le fichier solution dans Visual Studio, vous devez activer la prise en charge de plusieurs utilisateur pour votre jeu.
Pour ce faire :

1. Rechercher de l’Explorateur de solutions pour le fichier package.appxmanifest.xml
2. Cliquez avec le bouton droit sur le fichier et choisissez Afficher le Code
3. Sous le `<Properties><\/Properties>` , ajoutez la ligne suivante : ' < uap:SupportedUsers > plusieurs <\/uap:SupportedUsers >.
4. Déployer le jeu sur votre Xbox en démarrant une build de débogage à distance à partir de Visual Studio. Vous pouvez trouver des instructions pour configurer votre titre sur une console Xbox dans le [configurer votre UWP sur un environnement de développement Xbox](../../xbox-apps/development-environment-setup.md) article.

> [!NOTE]
> L’élément de configuration a changé peut vous sembler multijoueurs activé mais il est toujours nécessaire exécuter votre jeu dans les scénarios de lecteur unique.

Une fois que vous avez la PlayerAuthentication prefab fonctionne, vous pouvez en savoir plus sur script signe à l’aide de l’article [connectez-vous avec le Gestionnaire de connexions dans Unity](sign-in-manager.md).