---
author: PatrickFarley
title: Meilleures pratiques d’activités utilisateur
description: Ce guide décrit les pratiques recommandées pour la création et la mise à jour des activités de l’utilisateur.
keywords: activité utilisateur, activités utilisateur, chronologie, cortana reprend là où vous en étiez, cortana reprend là où j’en étais, projet rome
ms.author: pafarley
ms.date: 08/23/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: b11151df981a4b5ce233458581d21e405be9844c
ms.sourcegitcommit: 7efffcc715a4be26f0cf7f7e249653d8c356319b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/30/2018
ms.locfileid: "3112592"
---
# <a name="user-activities-best-practices"></a>Meilleures pratiques d’activités utilisateur

Ce guide décrit les pratiques recommandées pour la création et la mise à jour des activités de l’utilisateur. Pour une vue d’ensemble de la fonctionnalité d’activités de l’utilisateur sur Windows, voir [l’activité utilisateur continuer, même sur différents appareils](https://docs.microsoft.com/windows/uwp/launch-resume/useractivities). Ou, consultez la [section des activités utilisateur](https://docs.microsoft.com/windows/project-rome/user-activities/) du projet «Rome» pour les implémentations des activités sur d’autres plateformes de développement.

## <a name="when-to-create-or-update-user-activities"></a>Quand créer ou mettre à jour des activités de l’utilisateur

Étant donné que chaque application est différente, c’est à chaque développeur pour déterminer le meilleur moyen de mapper des actions au sein de l’application pour les activités de l’utilisateur. Vos activités utilisateur seront exposées dans Cortana et la chronologie, qui sont axées sur la productivité des utilisateurs croissant et l’efficacité en leur permettant d’accéder de nouveau à du contenu qu’ils visités par le passé.

**Recommandations générales**

* **Enregistrer une activité unique pour un groupe d’actions connexes de l’utilisateur.** Cela est particulièrement vrai pour les sélections de musique ou de télévision: une activité unique peut être mise à jour à intervalles réguliers afin de refléter la progression de l’utilisateur. Dans ce cas, vous aurez une activité utilisateur unique avec plusieurs éléments de l’historique représentant les périodes d’engagement sur plusieurs jours ou semaines. Il en va de même pour les activités de document basé sur lequel l’utilisateur effectue une progression progressive au sein de votre application.
* **Stocker les données utilisateur dans le cloud.** Si vous souhaitez prendre en charge les activités multiplateforme, vous devez vous assurer que le contenu requis pour se réengager cette activité est stocké dans un emplacement cloud. Les activités de propre à l’appareil seront affiche sur la chronologie sur l’appareil où l’activité a été créée, mais elles n’apparaissent ne peut-être pas sur d’autres appareils.
* **Ne pas créer des activités pour les actions les utilisateurs n’ont plus besoin de reprendre.** Si votre application est utilisée pour effectuer des opérations simples, à usage unique qui ne sont pas conservées état, probablement est inutile de créer une activité de l’utilisateur.
* **Ne pas créer des activités pour les actions effectuées par les autres utilisateurs.** Si un compte externe envoie un message à l’utilisateur ou @-mentions les au sein de votre application, vous ne devez pas créer une activité pour ce faire. Ce type d’action est mieux servi par des Notifications du centre.
  * Scénarios de collaboration sont une exception: Si plusieurs utilisateurs travaillent sur la même activité simultanément (par exemple, un document Word), il y aura des cas dans lesquels un autre utilisateur a apporté des modifications une fois que votre utilisateur. Dans ce cas, vous voudrez peut-être mettre à jour de l’activité existante afin de refléter les modifications apportées au document. Cela implique de mise à jour les données de contenu de l’activité utilisateur existantes sans avoir à créer un nouvel élément de l’historique.

**Recommandations en matière de types d’applications spécifiques**

Alors que chaque application est différente, la plupart des applications entrent dans l’un des modèles d’interaction suivants.
* **Applications basées sur un document** , créer une activité par document, avec un ou plusieurs éléments de l’historique reflétant les périodes d’utilisation. Il est important de mettre à jour votre activité lorsque des modifications sont apportées au document.
* **Jeux** : créer une activité pour chaque jeu enregistrer ou le monde. Si votre jeu prend en charge uniquement une seule séquence de niveaux, vous pouvez ré-publier la même activité au fil du temps, bien que vous souhaiterez peut-être mettre à jour les données de contenu pour afficher la progression ou les succès plus récentes.
* **Applications utilitaires** , s’il n’a aucun effet au sein de votre application que les utilisateurs doivent laisser et de reprise, vous n’avez pas besoin d’utiliser des activités de l’utilisateur. Un bon exemple est une application simple comme calculatrice.
* **Les applications cœur de métier** , de nombreuses applications existent pour la gestion des tâches simples ou flux de travail. Créer une activité pour chaque flux de travail distinct accédé par le biais de votre application (par exemple, États de dépenses serait chacun une activité distincte, afin que l’utilisateur peut alors cliquer une activité pour voir si un état particulier a été approuvé).
* **Les applications de lecture multimédia** , créer une activité par le regroupement logique de contenu (par exemple, un contenu playlist, le programme ou autonome). La question sous-jacent pour les développeurs d’applications est indique si un chaque élément de contenu (épisode TV, une chanson) considérée comme contenu autonome ou une partie d’une collection. En règle générale, si l’utilisateur choisit pour lire du contenu d’une collection ou un contenu séquentiels, la collection dans son ensemble est l’activité. Si elles optent pour lire un seul élément de contenu, une pièce de contenu est l’activité. Consultez les recommandations en matière de plus spécifiques ci-dessous.
  * **Musique: Album/artiste/Genre** : si l’utilisateur sélectionne un Album, artiste ou Genre et accède à **lire**, cette collection est l’activité; n’écrivez pas une activité distincte pour chaque chanson. Pour les collections courtes comme un seul album ou les collections lues dans un ordre aléatoire, vous devrez pas mettre à jour de l’activité de manière à refléter la position actuelle de l’utilisateur. Pour la lecture séquentielle long tel qu’un album ou d’une playlist, l’enregistrement de votre position dans l’album peut s’avérer pertinent.
  * **Musique: actives playlists** : Applications qui écouter de la musique dans un ordre aléatoire doivent enregistrer une activité unique pour cette sélection. Si l’utilisateur lit la playlist une deuxième fois, vous devez créer des enregistrements d’historique supplémentaires pour la même activité. L’enregistrement de la position actuelle de l’utilisateur de la playlist n’est pas nécessaire, car le classement est aléatoire.
  * **Séries TV** : Si votre application est configurée pour lire le prochain épisode une fois que celui en cours est terminée, vous devez écrire une activité unique pour la série de TV. Lorsque vous lisez les différents épisodes sur plusieurs sessions de l’affichage, vous devez mettre à jour votre activité pour prendre en compte la position actuelle dans la série et nouvel enregistrement d’historique est créé.
  * **Vidéo** : un film est un élément unique de contenu et doit avoir son propre enregistrement d’historique. Si l’utilisateur cesse de regarder la vidéo en cours de traitement, il est souhaitable d’enregistrer leur position. Quand ils le souhaitent reprendre la lecture à l’avenir, l’activité peut reprendre l’animation où il s’est arrêté ou même demander à l’utilisateur s’il souhaite reprendre ou depuis le début.

## <a name="user-activity-design"></a>Conception de l’activité utilisateur

Activités utilisateur consistant de trois composants: une activation URI, les données visual et les métadonnées de contenu.
* L’activation de l’URI est un URI qui peut être transmis à une application ou une expérience afin de reprendre l’application avec un contexte spécifique. En règle générale, ces liens prennent la forme du Gestionnaire de protocole pour un schéma (par exemple, «my-app://page2?action=edit»). Il incombe au développeur de déterminer comment les paramètres URI seront gérées par son application. Pour plus d’informations, consultez [l’activation des URI gérer](https://docs.microsoft.com/windows/uwp/launch-resume/handle-uri-activation) .
* Les données visuelles, consistant en un ensemble de propriétés obligatoires et facultatives (par exemple: titre, la description ou des éléments de carte adaptative), autoriser les utilisateurs à identifier visuellement une activité. Voir ci-dessous pour connaître les instructions sur la création d’éléments visuels de carte adaptative pour votre activité.
* Les métadonnées de contenu sont de données JSON qui peuvent être utilisées pour regrouper et récupérer des activités dans un contexte spécifique. En règle générale, cela prend la forme de http://schema.org données. Voir ci-dessous pour connaître les instructions sur le remplissage de ces données.

### <a name="adaptive-card-design-guidelines"></a>Recommandations en matière de conception de carte adaptatives

Lorsque les activités apparaissent dans la chronologie, ils sont affichés à l’aide de l' [infrastructure de carte adaptative](https://docs.microsoft.com/adaptive-cards/). Si le développeur ne fournit pas une carte adaptative pour chaque activité, chronologie crée automatiquement une carte simple basée sur l’icône/nom d’application, le champ titre requis et le champ de Description facultatif. 

Les développeurs d’applications sont encouragées à fournir des cartes personnalisés à l’aide du schéma JSON de carte adaptative simple. Consultez la [documentation de cartes adaptatives](https://docs.microsoft.com/adaptive-cards/authoring-cards/getting-started) pour obtenir des instructions techniques sur la façon de construire des objets de carte adaptative. Consultez les instructions ci-dessous pour la conception des cartes adaptatives dans les activités de l’utilisateur.
* Utiliser des images
  * Utilisez une image unique pour chaque activité, si possible. Votre nom de l’application et l’icône seront affiche automatiquement en regard de la carte de votre activité; images supplémentaires permet aux utilisateurs de localiser l’activité qu’ils recherchent.
  * Images ne doivent pas inclure de texte que l’utilisateur est censée lire. Ce texte ne sera pas disponible pour les utilisateurs ayant des besoins d’accessibilité et ne peuvent pas être recherché.
  * Si l’image ne contiennent du texte et peut être rognée pour s’adapter à propos d’un rapport de 2:1, vous devez l’utiliser comme image d’arrière-plan. Cela aboutit à une carte d’activité en gras qui ressort dans la chronologie. L’image sera légèrement assombrir pour vous assurer que le texte reste visible sur la carte, et il est recommandé d’utiliser uniquement le nom de l’activité dans ce cas, comme le texte plus petite peut devenir difficile à lire.
  * Si l’image ne peut pas être découpée sur 2:1, vous devez le placer dans la carte d’activité.  
    * Si les proportions est carré ou Portrait, l’image sur le côté droit de la carte sans marge l’ancrage.
    * Si les proportions est définie sur Paysage, ancrez l’image dans le coin supérieur droit de la carte.
* Chaque activité est nécessaire pour fournir un nom de l’activité, qui doit toujours être affichée.
  * Ce nom doit être affiché dans le coin supérieur gauche de la carte à l’aide de l’option gras volumineux. Il est important que le nom est facilement reconnaissable, comme c’est le seul les utilisateurs voient lorsqu’il est indiquée l’activité dans les scénarios de Cortana. Présentant le même nom dans la chronologie facilite la permettant aux utilisateurs de parcourir un grand nombre d’activités.
* Utiliser le même style visuel pour l’ensemble des activités à partir de votre application, afin que les utilisateurs puissent facilement localiser les activités de votre application dans la chronologie.
  * Par exemple, les activités doivent tous utiliser la même couleur d’arrière-plan.
* Utilisez les informations de texte supplémentaire avec parcimonie. 
  * Éviter de remplir la fiche avec du texte et utilisez uniquement des informations supplémentaires qui aide les utilisateurs lors de la recherche de l’activité de droite ou reflète les informations d’état (par exemple, l’état d’avancement dans une tâche spécifique).

### <a name="content-metadata-guidelines"></a>Recommandations en matière de métadonnées de contenu

Activités de l’utilisateur peuvent également contenir des métadonnées de contenu, Windows et Cortana permettent de classer les activités et de générer des inférences. Activités peuvent ensuite être regroupées autour d’un sujet particulier, par exemple, un emplacement (si l’utilisateur recherche vacances), objet (si l’utilisateur recherche quelque chose) ou une action (si l’utilisateur souhaite acheter un produit spécifique entre les sites Web et applications différents). Il est judicieux de représenter les noms et les verbes impliquée dans une activité. 

Dans l’exemple suivant, les métadonnées de contenu JSON, en suivant les normes de [Schema.org](https://schema.org/), représentent le scénario: «John lu oiseaux coléreux avec Steve».

```json
// John played angry birds with Steve.
{
  "@context": "http://schema.org",
  "@type": "PlayAction",
  "agent": {
    "@type": "Person",
    "name": "John"
  },
  "object": {
    "@type": "MobileApplication",
    "name": "Angry Birds."
  },
  "participant": {
    "@type": "Person",
    "name": "Steve"
  }
}
```

## <a name="key-apis"></a>Principales API

* [Espace de noms UserActivities](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities)

## <a name="related-topics"></a>Rubriques associées

* [Activités de l’utilisateur (documentation du projet «Rome»)](https://docs.microsoft.com/windows/project-rome/user-activities/)
* [Cartes adaptatives](https://docs.microsoft.com/adaptive-cards/)
* [Visualiseur de cartes adaptatives, exemples](http://adaptivecards.io/)
* [Gérer l’activation des URI](https://docs.microsoft.com/windows/uwp/launch-resume/handle-uri-activation)
* [Interagir avec vos clients sur toute plateforme à l’aide de MicrosoftGraph, du flux d’activité et des cartes adaptatives](https://channel9.msdn.com/Events/Connect/2017/B111)
* [Microsoft Graph](https://developer.microsoft.com/graph/)
