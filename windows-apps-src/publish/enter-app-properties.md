---
author: jnHs
Description: La page Propriétés de l’application du processus de soumission d’application vous permet de définir la catégorie de votre application, ainsi que les préférences matérielles ou d’autres déclarations.
title: Entrer les propriétés d’une application
ms.assetid: CDE4AF96-95A0-4635-9D07-A27B810CAE26
---

# Entrer les propriétés d’une application

La page **Propriétés de l'application** du [processus de soumission d'application](app-submissions.md) vous permet de définir la catégorie de votre application, ainsi que les préférences matérielles ou d'autres déclarations. Cet article décrit les options disponibles sur cette page et les éléments à prendre en compte pour la spécification de ces informations.

> **Remarque** L’évaluation de l’âge constitue désormais une page distincte du processus de soumission. Pour plus d’informations, voir la section [Classification par âge](age-ratings.md).

## Catégorie et sous-catégorie

Dans cette section, vous indiquez la catégorie (et la sous-catégorie s'il y a lieu) dans laquelle le Windows Store doit classer votre application. Vous devez définir une catégorie pour être en mesure de soumettre votre application.

Pour plus d'informations, voir l'article [Tableau des catégories et sous-catégories](category-and-subcategory-table.md).

## Préférences matérielles


Cette section vous permet d'indiquer si l'exécution de votre application et l'interaction avec cette dernière nécessitent certaines fonctionnalités matérielles.

Dans ce cas, le Windows Store tente de détecter si l'appareil d'un client prend en charge la fonctionnalité matérielle sélectionnée. Si nous détectons que tel n'est pas le cas, un avertissement s'affiche. Il informe le client qu'il tente de télécharger une application qui a déclaré une préférence pour ce matériel. Les clients utilisant des appareils Windows 10 verront également la ou les fonction(s) sélectionnée(s) s'afficher dans la section **Configuration matérielle requise** de la description de l'application dans le Windows Store.

Les clients peuvent quand même télécharger votre application sur des appareils non équipés du matériel nécessaire, mais ils ne sont pas en mesure d’évaluer ou de commenter votre application sur ces appareils.

> **Important** À l’exception de l’**écran tactile**, ces avertissements s’affichent uniquement pour les clients sur appareils Windows 10 qui n’ont pas les fonctionnalités sélectionnées.

En plus d’effectuer une sélection ici, nous vous recommandons d’ajouter des vérifications d’exécution pour le matériel spécifié dans votre application, car il peut arriver que le Windows Store ne puisse pas détecter que l’appareil d’un client ne dispose pas de la fonctionnalité sélectionnée. Dans ce cas, le client peut toujours télécharger votre application, même si un avertissement s’affiche.

> **Conseil** Si vous souhaitez empêcher le téléchargement de votre application UWP sur un appareil qui ne répond pas à la configuration minimale requise pour la mémoire ou le niveau DirectX, vous pouvez indiquer cette configuration minimale requise dans un fichier XML StoreManifest. Pour plus d’informations, voir [Schéma StoreManifest (Windows 10)](https://msdn.microsoft.com/library/windows/apps/mt617335).

## Déclarations d’application


Les cases à cocher de cette section vous permettent d'indiquer les déclarations qui s'appliquent à votre application. Votre sélection affecte le mode d'affichage de votre application, le public auquel elle est proposée ou la façon dont les clients peuvent l'utiliser.

Pour plus d'informations, voir l'article [Déclarations d'application](app-declarations.md).


<!--HONumber=May16_HO2-->


