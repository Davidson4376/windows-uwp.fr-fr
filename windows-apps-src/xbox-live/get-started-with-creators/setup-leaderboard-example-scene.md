---
title: Utilisez la scène de l’exemple du classement dans Unity
description: Décrit les étapes pour configurer correctement de la scène de classement Unity
ms.date: 04/24/2018
ms.topic: get-started-article
keywords: Xbox live, xbox, jeux, uwp, windows 10, xbox une, unity, classements
ms.openlocfilehash: d931a0fdcbdec5dd9deb53876a19e1b475afcb93
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57589744"
---
# <a name="the-leaderboard-example-scene-in-unity"></a>La scène d’exemple de classement dans Unity

[Le plug-in Unity](https://github.com/Microsoft/xbox-live-unity-plugin) héberge un nombre de séquences d’exemple pour illustrer les services Xbox Live disponibles pour les développeurs Unity. Une telle scène est la scène d’exemple de classement. Dans la capture d’écran ci-dessous, vous verrez que la scène d’exemple de classement affiche simplement un panneau de connexion pour l’utilisateur de Xbox Live et un panneau pour contenir le classement. Si vous appuyez sur play à ce stade sans ajouter à cette scène, vous trouverez le panneau d’authentification est remplie avec des données utilisateur factices, mais le classement ne charge aucune information. Afin d’obtenir cette scène d’exemple pour charger un classement réel, vous devrez effectuer quelques ajouts.

![Capture d’écran de la scène de classement](../images/unity/leaderboard-scene-1804.JPG)

## <a name="prerequisites"></a>Conditions préalables

Classements dans Xbox Live sont basées sur les statistiques dans le service stats Xbox Live. Avant que vous pouvez remplir un classement avec des données, que vous devez avoir certaines statistiques associées à vos comptes de test. Si vous n’avez pas déjà ajouté les statistiques pour votre titre vous pouvez apprendre à le faire en lisant [ajouter des statistiques et des tableaux de résultats à votre projet Unity](add-stats-and-leaderboards-in-unity.md). Après avoir effectué les actions dans la section des statistiques de cet article revenir ici pour afficher un état dans la scène de classement d’exemple.

## <a name="the-leaderboard-inspector"></a>L’inspecteur de classement

Le préfabriqué classement a un nombre de paramètres qui peuvent être modifiés dans la section inspecteur composant du classement de script, tels que l’interface utilisateur *thème*, le joueur associé avec le classement, les paramètres du contrôleur xbox, et autres paramètres de classement. Vous pouvez voir que les paramètres de classement sont divisées en plusieurs sections différentes dans l’inspecteur ci-dessous.

![Capture d’écran de classement inspecteur](../images/unity/leaderboard_script_inspector.JPG)

### <a name="theme-and-display-settings"></a>Paramètres de thème et affichez

Cette section contient un paramètre appelé *thème*. Il s’agit d’une simple liste déroulante qui vous permet d’utiliser un thème sombre ou clair pour votre classement prefab. Cela modifiera l’arrière-plan, la police et couleurs d’une image de la préfabriqué. L’effet est facilement visible lors de la lecture de la scène dans le lecteur unity.

![Thème clair](../images/unity/leaderboard_light_theme.JPG) ![Thème sombre](../images/unity/leaderboard_dark_theme.JPG)

### <a name="stat-configuration"></a>Configuration Stat

Cette section vous permet de déterminer quel type de données est récupérée lors de la partie du classement est rempli avec des lignes.

- **Nombre de joueur** -ce paramètre détermine quel lecteur est associé avec le classement.
- **Stat** -la statistique utilisée pour remplir les données de classement. Cela est nécessaire pour le classement préfabriqué charger des données.
- **Type de classement** -ce menu déroulant applique un filtre aux données de classement chargées. Si *Global* fait partie du classement sera non filtré et affiche chaque lecteur avec une valeur pour la statistique sélectionnée. Si *amis* fait partie du classement sera filtré pour uniquement afficher des lecteurs sur le classement qui se trouvent également dans votre liste d’amis. Si *favori* est sélectionné le classement sera filtré pour uniquement afficher des lecteurs sur le classement qui se trouvent également dans votre liste de favoris.
- **Nombre d’entrées** -un curseur avec une plage de 1 à 100 qui détermine le nombre de lignes du classement s’affichera à la fois. Le nombre défini ici détermine le nombre de lignes de classement affichés par page.

### <a name="controller-configuration"></a>Configuration du contrôleur

Le préfabriqué classement permet aux développeurs de facilement configurer Xbox, utilisez contrôleur. La section de configuration de la préfabriqué contrôleur vous permet d’activer et sélectionnez les boutons qui contrôlent le préfabriqué de classement.

- **Activer l’entrée de contrôleur** -une bascule de bouton de case d’option simple. Si cette option est activée vous pouvez utiliser une manette Xbox pour interagir avec le préfabriqué. Requis pour la prise en charge du contrôleur.
- **Nombre de joystick** -désigne le contrôleur peut interagir avec ce classement prefab.
- **Bouton de contrôleur de Page suivant** -menu déroulant de quels contrôles de bouton qui charge la page suivante de classement des lignes.
- **Bouton de contrôleur de Page précédente** -menu déroulant de quels contrôles de bouton qui charge la page précédente de classement des lignes.
- **Bouton de contrôleur de vue suivant** -Active ou désactive le type de vue entre **tous les** et **le plus proche ME**.
- **Bouton de contrôleur de vue précédente** -Active ou désactive le type de vue entre **tous les** et **le plus proche ME**.
- **Axe d’entrée défilement vertical** -chaîne qui désigne les axes de contrôleur est associé avec le défilement.
- **Faites défiler vers le multiplicateur de vitesse** -détermine la vitesse de défilement de contrôleur.

> [!NOTE]
> Les boutons utilisés pour la modification de la page ou la vue du classement modification s’appliquera également l’image associée au bouton utilisé pour chaque fonction du classement. Il est inutile de modifier la section Références de l’interface utilisateur manuellement pour correspondre à l’image au bouton.

### <a name="ui-references"></a>Références d’interface utilisateur

Cette section contrôle les images et la composition générale de la préfabriqué de classement. Cette section n’a pas besoin être modifiée pour l’utilisation réussie de la préfabriqué de classement. Toutefois, vous devrez effectuer des ajustements dans cette section pour personnaliser l’apparence de la préfabriqué selon vos besoins.

## <a name="populating-the-unity-player-leaderboard-with-fake-data"></a>Remplissage le classement de lecteur Unity (avec des données fictives)

Afin de renseigner le classement de lecteur Unity avec des données, vous devrez ajouter une statistique pour le préfabriqué de classement. Afficher le classement préfabriqué dans l’inspecteur révèle que peut prendre un objet de type `Stat Base` comme paramètre public dans son script. Vous pouvez faire glisser de la `State Base` tapez prefabs `IntegerStat`, `DoubleStat`, ou `StringStat` à partir du dossier prefabs le plug-in de Xbox Live Unity et placez-le à cet endroit dans le classement prefab.

![Faites glisser et déposez préfabriqué Stat](../images/unity/stat-to-leaderbaord-drag.gif)

Lire maintenant la scène dans Unity et vous constaterez que le classement est rempli avec les données factices comme ci-dessous.

![Capture d’écran de classement des données fictives remplies](../images/unity/leaderboard-fake-data-1804.JPG)

## <a name="populating-a-visual-studio-built-project-with-real-data"></a>Remplissage d’un projet avec des données réelles de Visual Studio

Afin de renseigner un classement avec des données réelles pour votre titre, vous devrez créer votre jeu pour exécuter localement sur votre ordinateur. Étant donné que l’éditeur Unity n’a pas accès à la Xbox Live, vous devez une build locale. En plus de générer votre projet doit être exécuté localement, vous devrez configurer le stat dans votre classement pour une statistique qui est initialisé et comporte des valeurs pour votre titre. Vous devez modifier l’ID et le nom d’affichage de l’objet de statistiques dans le classement préfabriqué afin d’associer un stat à votre classement. L’ID doit correspondre à celle d’un état configuré dans [partenaires](https://partner.microsoft.com/dashboard). Une fois que vous avez fait, générez votre projet comme décrit dans la [section de la Xbox Live dans l’article Unity build](configure-xbox-live-in-unity.md#build-and-test-the-project). L’exécution de ce projet en tant qu’une build ciblant l’ordinateur Local doit vous permettre de x64 connectez-vous avec une véritable identité et remplissez le classement avec des données réelles.