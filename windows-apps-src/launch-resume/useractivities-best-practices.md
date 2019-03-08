---
title: Bonnes pratiques des activités utilisateur
description: Ce guide décrit les pratiques recommandées pour la création et la mise à jour des activités de l’utilisateur.
keywords: activité utilisateur, activités utilisateur, chronologie, cortana reprend là où vous en étiez, cortana reprend là où j’en étais, projet rome
ms.date: 08/23/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: e98f5d73cf2d1afb26a823ed417c8980d118485c
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57589904"
---
# <a name="user-activities-best-practices"></a>Bonnes pratiques des activités utilisateur

Ce guide décrit les pratiques recommandées pour la création et la mise à jour des activités de l’utilisateur. Pour une vue d’ensemble de la fonctionnalité d’activités des utilisateurs sur Windows, consultez [continuer les activités des utilisateurs, même sur les appareils](https://docs.microsoft.com/windows/uwp/launch-resume/useractivities). Vous pouvez aussi consulter le [section des activités de l’utilisateur](https://docs.microsoft.com/windows/project-rome/user-activities/) de Project Rome pour les implémentations des activités sur d’autres plateformes de développement.

## <a name="when-to-create-or-update-user-activities"></a>Quand créer ou mettre à jour les activités des utilisateurs

Étant donné que chaque application est différente, c’est à chaque développeur de déterminer la meilleure façon de mapper des actions au sein de l’application à des activités de l’utilisateur. Vos activités utilisateur seront exposées dans Cortana et chronologie, qui se concentrent sur la productivité des utilisateurs croissant et l’efficacité en les aidant à revenir au contenu qu’ils visités par le passé.

**Recommandations générales**

* **Enregistrer une activité unique pour un groupe d’actions de l’utilisateur.** Cela est particulièrement pertinente pour les sélections de musique ou émissions TV : une seule activité peut être mis à jour à intervalles réguliers afin de refléter la progression de l’utilisateur. Dans ce cas, avoir une activité utilisateur unique avec plusieurs éléments d’historique représentant des périodes d’engagement sur plusieurs jours ou semaines. Il en va de même pour les activités basées sur des documents sur lequel l’utilisateur progresse progressive au sein de votre application.
* **Store les données utilisateur dans le cloud.** Si vous souhaitez prendre en charge les activités entre les périphériques, vous devez vous assurer que le contenu requis pour recontacter cette activité est stocké dans un emplacement du cloud. Les activités spécifiques à l’appareil seront affiche sur la chronologie sur l’appareil où l’activité a été créée mais ne peuvent pas apparaître sur d’autres appareils.
* **Ne créez pas d’activités pour les actions que les utilisateurs ne devront pas à reprendre.** Si votre application est utilisée pour effectuer des opérations simples, à usage unique qui ne persistent pas d’état, probablement inutile créer une activité de l’utilisateur.
* **Ne créez pas d’activités pour les actions effectuées par d’autres utilisateurs.** Si un compte externe envoie un message à l’utilisateur ou @-mentions les au sein de votre application, vous ne devez pas créer une activité pour cela. Ce type d’action est préférable d’Action du centre de Notifications.
  * Scénarios de collaboration sont une exception : Si plusieurs utilisateurs travaillent sur la même activité ensemble (par exemple, un document Word), il y aura des cas dans lequel un autre utilisateur a apporté des modifications une fois que votre utilisateur. Dans ce cas, vous souhaiterez mettre à jour de l’activité existante pour refléter les modifications qui ont été apportées au document. Cela implique la mise à jour des données de contenu de l’activité des utilisateurs existantes sans créer un nouvel élément de l’historique.

**Instructions pour les types spécifiques d’applications**

Bien que chaque application soit différente, la plupart des applications entrent dans l’un des modèles d’interaction suivants.
* **Les applications basées sur le document** : créer une activité par document, avec un ou plusieurs éléments d’historique reflétant les périodes d’utilisation. Il est important de mettre à jour de votre activité lorsque des modifications sont apportées au document.
* **Jeux** : créer une activité pour chaque jeu enregistrer ou le monde. Si votre jeu prend en charge uniquement une seule séquence de niveaux, vous pouvez republier la même activité au fil du temps, bien que vous pouvez souhaiter mettre à jour les données de contenu pour afficher la progression ou les primes plus récente.
* **Applications utilitaire** — si aucun élément n’au sein de votre application, les utilisateurs doivent laisser et reprendre, il est inutile d’utiliser les activités de l’utilisateur. Un bon exemple est une application simple comme calculatrice.
* **Les applications Line of business** — nombre d’applications existe pour la gestion des tâches simples ou des flux de travail. Créer une activité pour chaque flux de travail distinct accédé via votre application (par exemple, États de dépenses serait chacun une activité séparée, afin que l’utilisateur peut cliquer ensuite sur une activité pour voir si un rapport particulier a été approuvé).
* **Applications de lecture de médias** : créer une activité par regroupement logique de contenu (par exemple, une sélection de programme, contenu ou autonome). La question sous-jacente pour les développeurs d’applications est si un chaque élément de contenu (épisode TV, chanson) est comptabilisé comme contenu autonome ou une partie d’une collection. En règle générale, si l’utilisateur décide de lire une collection ou un contenu séquentiel, la collection dans son ensemble est l’activité. S’ils optent pour lire une partie du contenu, qui une partie de contenu est l’activité. Consultez des instructions plus spécifiques ci-dessous.
  * **Musique : Album/artiste/Genre** — si l’utilisateur sélectionne un Album, artiste, ou Genre et accès **lire**, cette collection est l’activité ; ne pas écrire une activité séparée pour chaque chanson. Pour les collections courtes comme un seul album ou collections lues dans un ordre aléatoire, vous devrez peut-être pas à jour l’activité afin de refléter la position actuelle de l’utilisateur. Pour lire un long séquentiel tel qu’un album ou une sélection, l’enregistrement de votre position dans l’album peut être judicieuse.
  * **Musique : smart sélections** — les Applications qui la musique dans un ordre aléatoire doivent enregistrer une seule activité pour cette sélection. Si l’utilisateur lit la sélection une deuxième fois, vous devez créer des enregistrements d’historique supplémentaires pour la même activité. L’enregistrement de position actuelle de l’utilisateur dans la liste de lecture n’est pas nécessaire car le classement est aléatoire.
  * **Série TV** : Si votre application est configurée pour lire le prochain épisode après celui en cours est terminée, vous devez écrire une seule activité de la série TV. Lorsque vous lisez les épisodes différents entre plusieurs sessions d’affichage, vous allez mettre à jour votre activité afin de refléter la position actuelle dans la série, et plusieurs enregistrements d’historique seront créés.
  * **Film** — un film est un élément de contenu unique et doit avoir son propre enregistrement de l’historique. Si l’utilisateur arrête de regarder la vidéo en cours de traitement, il est souhaitable d’enregistrer leur position. Quand ils le souhaitent pour la reprendre à l’avenir, l’activité peut reprendre le film où il s’était arrêté, ou même demander à l’utilisateur s’ils le souhaitent commencer au début ou de reprendre.

## <a name="user-activity-design"></a>Conception de l’activité utilisateur

Activités de l’utilisateur se composent de trois composants : un URI de l’activation, données visuelles et métadonnées de contenu.
* L’activation de l’URI est un URI qui peut être passé à une application ou l’expérience afin de reprendre l’application avec un contexte spécifique. En règle générale, ces liens prennent la forme de gestionnaire de protocole pour un schéma (par exemple, « mon-app://page2?action=edit »). Il incombe au développeur de déterminer la gestion des paramètres d’URI par leur application. Consultez [l’activation de gérer une URI](https://docs.microsoft.com/windows/uwp/launch-resume/handle-uri-activation) pour plus d’informations.
* Les données visuelles, consistant en un ensemble de propriétés obligatoires et facultatifs (par exemple : titre, description ou les éléments de carte adaptative), autoriser les utilisateurs à identifier visuellement une activité. Voir ci-dessous pour obtenir des instructions sur la création d’éléments visuels de carte adaptative pour votre activité.
* Les métadonnées de contenu sont des données JSON qui peuvent être utilisées pour grouper et récupérer les activités dans un contexte particulier. En règle générale, cela prend la forme de http://schema.org données. Voir ci-dessous pour obtenir des instructions sur le remplissage de ces données.

### <a name="adaptive-card-design-guidelines"></a>Instructions de conception de carte adaptative

Lorsque les activités s’affichent dans la chronologie, ils sont affichés à l’aide de la [framework de carte adaptative](https://docs.microsoft.com/adaptive-cards/). Si le développeur ne fournit pas d’une carte adaptative pour chaque activité, chronologie crée automatiquement une carte simple basée sur l’icône/nom d’application, le champ de titre obligatoire et le champ de Description facultatif. 

Les développeurs d’applications sont encouragés à proposer des cartes personnalisées en utilisant le schéma JSON de carte adaptative simple. Consultez le [documentation des cartes adaptatives](https://docs.microsoft.com/adaptive-cards/authoring-cards/getting-started) pour obtenir des instructions techniques sur la façon de construire des objets de carte adaptative. Reportez-vous aux instructions ci-dessous pour la conception des cartes adaptatives aux activités des utilisateurs.
* Utiliser des images
  * Utilisez une image unique pour chaque activité, si possible. Votre nom de l’application et une icône s’affiche automatiquement en regard de la carte de votre activité. images supplémentaires aident les utilisateurs à localiser l’activité qu’ils recherchent.
  * Les images ne doivent pas inclure de texte que l’utilisateur est censé lire. Ce texte n’est pas disponible pour les utilisateurs aux besoins d’accessibilité et ne peuvent pas être recherché.
  * Si l’image ne contiennent du texte et peut être rognée sur un rapport de 2:1, vous devez l’utiliser comme image d’arrière-plan. Cela entraîne une carte d’activité en gras qui ressort dans la chronologie. L’image sera légèrement assombrir pour garantir le texte reste visible sur la carte, et il est conseillé d’utiliser uniquement le nom de l’activité dans ce cas, comme texte plus petits peut devenir difficile à lire.
  * Si l’image ne peut pas être rognée à 2:1, vous devez le placer dans la carte de l’activité.  
    * Si les proportions est carré ou Portrait, ancrez l’image sur le côté droit de la carte avec pas de marges.
    * Si les proportions est Paysage, ancrez l’image à l’angle supérieur droit de la carte.
* Chaque activité est nécessaire pour fournir un nom d’activité, qui doit toujours être affichée.
  * Ce nom doit être affiché dans le coin supérieur gauche de la carte à l’aide de l’option de texte en gras de grande taille. Il est important que le nom est facilement reconnaissable, comme c’est la seule partie utilisateurs seront affiche lorsque l’activité est indiquée dans les scénarios de Cortana. Affiche le même nom dans la chronologie facilite aux utilisateurs de parcourir un grand nombre d’activités.
* Utiliser le même style visuel pour toutes les activités à partir de votre application, afin que les utilisateurs peuvent localiser facilement les activités de votre application dans la chronologie.
  * Par exemple, les activités doivent tous utiliser la même couleur d’arrière-plan.
* Utilisez les informations de texte supplémentaire avec parcimonie. 
  * Éviter de remplir la carte avec le texte et utiliser uniquement des informations supplémentaires qui assiste les utilisateurs à trouver l’activité appropriée ou reflète les informations d’état (par exemple, la progression actuelle dans une tâche particulière).

### <a name="content-metadata-guidelines"></a>Instructions de métadonnées de contenu

Activités de l’utilisateur peuvent également contenir des métadonnées de contenu, Windows et Cortana utiliser pour classer les activités et de générer des inférences. Activités peuvent ensuite être regroupées sur des sujets particuliers, comme un emplacement (si l’utilisateur recherche vacances), objet (si l’utilisateur recherche quelque chose) ou une action (si l’utilisateur souhaite acheter un produit particulier entre les sites Web et applications différentes). Il est judicieux pour représenter les noms et les verbes impliquée dans une activité. 

Dans l’exemple suivant, les métadonnées de contenu JSON, en suivant les normes de [Schema.org](https://schema.org/), représente le scénario : « John lu en colère oiseaux avec Steve ».

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

## <a name="related-topics"></a>Rubriques connexes

* [Activités de l’utilisateur (documents de Project Rome)](https://docs.microsoft.com/windows/project-rome/user-activities/)
* [Cartes adaptatives](https://docs.microsoft.com/adaptive-cards/)
* [Visualiseur de cartes adaptatives, exemples](https://adaptivecards.io/)
* [Gérer l’activation d’URI](https://docs.microsoft.com/windows/uwp/launch-resume/handle-uri-activation)
* [Le contact avec vos clients sur n’importe quelle plateforme à l’aide de la Microsoft Graph, flux d’activité et des cartes adaptatives](https://channel9.msdn.com/Events/Connect/2017/B111)
* [Microsoft Graph](https://developer.microsoft.com/graph/)
