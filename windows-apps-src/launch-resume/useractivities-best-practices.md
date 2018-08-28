---
author: PatrickFarley
title: Méthodes conseillées activités utilisateur
description: Ce guide décrit les pratiques recommandées pour la création et la mise à jour des activités de l’utilisateur.
keywords: activité utilisateur, activités utilisateur, chronologie, cortana reprend là où vous en étiez, cortana reprend là où j’en étais, projet rome
ms.author: pafarley
ms.date: 08/23/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: b11151df981a4b5ce233458581d21e405be9844c
ms.sourcegitcommit: 9a17266f208ec415fc718e5254d5b4c08835150c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/28/2018
ms.locfileid: "2891775"
---
# <a name="user-activities-best-practices"></a>Méthodes conseillées activités utilisateur

Ce guide décrit les pratiques recommandées pour la création et la mise à jour des activités de l’utilisateur. Pour une vue d’ensemble de la fonctionnalité d’activités des utilisateurs sur Windows, voir [Continuer l’activité des utilisateurs, même sur des périphériques](https://docs.microsoft.com/windows/uwp/launch-resume/useractivities). Ou, consultez la [section activités utilisateur](https://docs.microsoft.com/windows/project-rome/user-activities/) Rome Project pour les implémentations des activités sur d’autres plateformes de développement.

## <a name="when-to-create-or-update-user-activities"></a>Quand créer ou mettre à jour des activités de l’utilisateur

Étant donné que chaque application est différente, c’est à chaque développeur de déterminer la meilleure façon pour mapper les actions dans l’application pour les activités de l’utilisateur. Vos activités d’utilisateur seront exposées dans Cortana et chronologie, qui portent principalement sur la productivité des utilisateurs croissant et l’efficacité en leur permettant de revenir au contenu qu’ils visitées les.

**Recommandations générales**

* **Enregistrer une activité unique pour un groupe d’actions de l’utilisateur associés.** C’est particulièrement vrai pour les sélections de musique ou télévision: une activité unique peut être mis à jour à intervalles réguliers pour refléter la progression de l’utilisateur. Dans ce cas, vous aurez une activité utilisateur unique avec plusieurs éléments d’historique qui représente les périodes d’engagement sur plusieurs jours ou semaines. Les mêmes s’applique aux activités de document basé sur lequel l’utilisateur progresse progressive au sein de votre application.
* **Stocker les données utilisateur dans le nuage.** Si vous souhaitez prendre en charge les activités cross-périphérique, vous devrez Assurez-vous que le contenu requis pour recontacter cette activité est stocké dans un emplacement dans le nuage. Activités spécifiques aux périphériques seront affiche sur la chronologie sur l’appareil où l’activité a été créée, mais n’apparaissent pas sur les autres périphériques.
* **Ne créez pas d’activités pour les actions que les utilisateurs devront pas reprendre.** Si votre application est utilisée pour effectuer des opérations simples, ce qui ne sont pas conservées état, probablement inutile créer une activité de l’utilisateur.
* **Ne créez pas d’activités pour les actions effectuées par d’autres utilisateurs.** Si un compte externe envoie un message à l’utilisateur ou @-mentions les au sein de votre application, vous ne devez pas créer une activité pour ce. Ce type d’action est préférable d’Action centre de Notifications.
  * Scénarios de collaboration sont une exception: Si plusieurs utilisateurs travaillent sur l’activité même ensemble (par exemple un document Word), il y aura des cas dans lesquels un autre utilisateur a modifié une fois que votre utilisateur. Dans ce cas, vous souhaiterez peut-être mettre à jour l’activité existante afin de répercuter les modifications qui ont été apportées au document. Cela entraîne la mise à jour les données de contenu de l’activité des utilisateurs existantes sans créer un nouvel élément d’historique.

**Recommandations pour des types spécifiques d’applications**

Alors que chaque application est différente, la plupart des applications tombent dans l’un des modèles d’interaction suivants.
* **Applications basées sur des documents** , créer une activité par document, avec un ou plusieurs éléments d’historique qui reflète des périodes d’utilisation. Il est important de mettre à jour votre activité lorsque des modifications sont apportées au document.
* **Jeux** : créer une activité pour chaque jeu enregistrer ou le monde. Si votre jeu prend en charge uniquement une seule séquence des niveaux, vous pouvez publier renouveler l’activité même au fil du temps, bien que vous pouvez mettre à jour les données de contenu pour afficher la progression ou les résultats le plus récent.
* **Utilitaire applications** — s’il n’a rien au sein de votre application que les utilisateurs doivent laisser et reprendre, il est inutile d’utiliser des activités de l’utilisateur. Un bon exemple est une application simple comme calculatrice.
* **Applications métier de** — existent de nombreuses applications pour la gestion des tâches simples ou flux de travail. Créer une activité pour chaque flux de travail distinct accédé par le biais de votre application (par exemple, États de dépenses serait chacun une activité distincte, afin que l’utilisateur pourrait puis cliquez sur une activité pour voir si un état particulier a été approuvé).
* **Applications de lecture multimédia** : créer une activité par un regroupement logique de contenu (par exemple, un contenu sélection, programme ou autonome). La question sous-jacent pour les développeurs d’applications est ou non une chaque élément de contenu (épisode TV, chanson) est considérée comme contenu autonome ou une partie d’une collection. En règle générale, si l’utilisateur choisit pour lire une collection ou un contenu séquentiel, la collection dans sa globalité est l’activité. Si leur abonnement pour lire une partie du contenu, qui une partie de contenu est l’activité. Voir les instructions plus spécifiques ci-dessous.
  * **Musique: Album/artistes/Genre** — si l’utilisateur sélectionne un Album, l’auteur ou Genre et accède à **lire**, cette collection est l’activité; n’écrivez pas une activité distincte pour chaque chanson. Pour les collections de courtes comme un seul album ou des collections de la lecture dans un ordre aléatoire, vous devrez pas mettre à jour de l’activité pour refléter la position actuelle de l’utilisateur. Pour une lecture séquentielle long comme un album ou une sélection, l’enregistrement de votre position dans l’album peut être utile.
  * **Musique: actives sélections** : Applications qui la musique dans un ordre aléatoire doivent enregistrer une activité unique pour cette sélection. Si l’utilisateur est lu à la sélection une deuxième fois, vous devez créer des enregistrements d’historique supplémentaires pour la même activité. Position actuelle de l’utilisateur dans la sélection d’enregistrement n’est pas nécessaire, car le classement est aléatoire.
  * **Série TV** — si votre application est configurée pour lire le prochain épisode après celle en cours est terminée, vous devez écrire une activité unique pour la série TV. Lorsque vous lisez les épisodes différents entre plusieurs sessions de l’affichage, vous allez mettre à jour votre activité pour refléter la position actuelle dans la série et plusieurs enregistrements d’historique seront créés.
  * **Vidéo** : un film est un élément de contenu unique et doit avoir son propre enregistrement de l’historique. Si l’utilisateur s’arrête, regardez la vidéo en cours de traitement, il est souhaitable d’enregistrer leur position. Lorsqu’ils souhaitent relancer à l’avenir, l’activité peut reprendre la vidéo où ils arrêtés ou même demander à l’utilisateur s’il souhaite reprendre ou depuis le début.

## <a name="user-activity-design"></a>Conception de l’activité utilisateur

Activités de l’utilisateur se composent de trois composants: un URI de l’activation, les données visual et les métadonnées de contenu.
* L’activation de l’URI est un URI qui peut être passé à une application ou d’expérience pour reprendre l’application d’un contexte spécifique. En règle générale, ces liens prennent la forme de gestionnaire de protocole pour un schéma (par exemple, «mon-app://page2?action=edit»). Il incombe au développeur de déterminer comment les paramètres URI seront gérés par leur application. Pour plus d’informations, voir [activation de gérer les URI](https://docs.microsoft.com/windows/uwp/launch-resume/handle-uri-activation) .
* Les données visual, constitué d’un ensemble de propriétés obligatoires et facultatifs (par exemple: titre, description ou éléments de la carte Adaptive), autoriser les utilisateurs à identifier une activité. Pour connaître les instructions sur la création d’effets visuels carte Adaptive pour votre activité, voir ci-dessous.
* Les métadonnées de contenu donnée JSON qui peut être utilisé pour regrouper et récupérer des activités dans un contexte spécifique. En règle générale, il prend la forme de http://schema.org données. Voir ci-dessous pour connaître les instructions sur le remplissage de ces données.

### <a name="adaptive-card-design-guidelines"></a>Instructions de conception de carte Adaptive

Lorsque les activités apparaissent dans la chronologie, ils sont affichés à l’aide de la [carte Adaptive framework](https://docs.microsoft.com/adaptive-cards/). Si le développeur ne fournit pas une carte Adaptive pour chaque activité, chronologie crée automatiquement une carte simple basée sur l’icône/nom d’application, le champ titre requis et le champ Description facultatif. 

Les développeurs d’applications sont invités à fournir des cartes personnalisées à l’aide du schéma Adaptive carte JSON simple. Consultez la [documentation Adaptive cartes](https://docs.microsoft.com/adaptive-cards/authoring-cards/getting-started) pour obtenir des instructions techniques sur la construction d’objets Adaptive carte. Consultez les instructions ci-dessous pour concevoir des cartes Adaptive dans les activités de l’utilisateur.
* Utiliser des images
  * Utiliser une image unique pour chaque activité, si possible. Votre nom de l’application et une icône s’affiche automatiquement en regard de la carte de votre activité; images supplémentaires permettra aux utilisateurs de localiser l’activité qu’ils cherchent.
  * Images ne doivent pas inclure de texte que l’utilisateur doit lire. Ce texte n’est pas disponible pour les utilisateurs avec des besoins en matière d’accessibilité et ne peuvent pas être recherché.
  * Si l’image ne contiennent du texte et peut être découpée à sur un rapport de 2:1, vous devez l’utiliser comme image d’arrière-plan. Cela entraîne une carte gras activité qui ressort dans la chronologie. L’image sera être légèrement obscurcie pour garantir le texte reste visible sur la carte, et il est recommandé d’utiliser uniquement le nom de l’activité dans ce cas, comme le plus petit texte peut devenir difficile à lire.
  * Si l’image ne peut pas être découpée à 2:1, vous devez la placer dans la carte de l’activité.  
    * Si le rapport hauteur / largeur est carrée ou Portrait, l’image sur le côté droit de la carte avec aucun des marges d’ancrage.
    * Si le rapport hauteur / largeur est Paysage, l’image sur le coin supérieur droit de la carte d’ancrage.
* Chaque activité est nécessaire pour fournir un nom de l’activité, qui doit toujours être affiché.
  * Ce nom doit être affiché dans le coin supérieur gauche de la carte à l’aide de l’option volumineux contenant du texte en gras. Il est important que le nom est facilement reconnaissable, comme c’est le seul composant s’affiche lorsque l’activité est indiquée dans les scénarios de Cortana. Affichant le même nom dans la chronologie facilite aux utilisateurs de parcourir un grand nombre d’activités.
* Utiliser le même style visual pour toutes les activités à partir de votre application, afin que les utilisateurs peuvent rechercher facilement des activités de votre application dans la chronologie.
  * Par exemple, activités doivent toutes utilisent la même couleur d’arrière-plan.
* Utilisez les informations de texte supplémentaire avec modération. 
  * Éviter de remplir la fiche avec du texte et utilisez uniquement des informations supplémentaires qui aide les utilisateurs à trouver l’activité droite ou reflètent les informations d’état (par exemple, l’état d’avancement dans une tâche spécifique).

### <a name="content-metadata-guidelines"></a>Instructions de métadonnées de contenu

Activités de l’utilisateur peuvent également contenir les métadonnées de contenu, Windows et Cortana utilisent pour classer les activités et générer des inférences. Activités peuvent être regroupées autour d’un sujet particulier, par exemple un emplacement (si l’utilisateur recherche vacances), objet (si l’utilisateur est recherche) ou une action (si l’utilisateur souhaite acheter un produit particulier entre les sites Web et d’applications différents). Il est préférable de représenter les noms et les verbes impliquée dans une activité. 

Dans l’exemple suivant, les métadonnées de contenu JSON, en suivant les normes de [Schema.org](https://schema.org/), représentent le scénario: «John joue en colère oiseaux avec Steve.»

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

* [Activités de l’utilisateur (documents de projet Rome)](https://docs.microsoft.com/windows/project-rome/user-activities/)
* [Cartes adaptatives](https://docs.microsoft.com/adaptive-cards/)
* [Visualiseur de cartes adaptatives, exemples](http://adaptivecards.io/)
* [Gérer l’activation des URI](https://docs.microsoft.com/windows/uwp/launch-resume/handle-uri-activation)
* [Interagir avec vos clients sur toute plateforme à l’aide de MicrosoftGraph, du flux d’activité et des cartes adaptatives](https://channel9.msdn.com/Events/Connect/2017/B111)
* [Microsoft Graph](https://developer.microsoft.com/graph/)
