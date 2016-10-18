---
author: jnHs
Description: "La page Propriétés de l’application du processus de soumission d’application vous permet de définir la catégorie de votre application, ainsi que les préférences matérielles ou d’autres déclarations."
title: "Entrer les propriétés d’une application"
ms.assetid: CDE4AF96-95A0-4635-9D07-A27B810CAE26
translationtype: Human Translation
ms.sourcegitcommit: 345350cea16b850e49f6f958e304654aba1299bb
ms.openlocfilehash: d2eb9a62dcaadca599136f4505f85e3ba94c0189

---

# Entrer les propriétés d’une application

La page **Propriétés de l'application** du [processus de soumission d'application](app-submissions.md) vous permet de définir la catégorie de votre application, ainsi que les préférences matérielles ou d'autres déclarations. Cet article décrit les options disponibles sur cette page et les éléments à prendre en compte pour la spécification de ces informations.

> **Remarque** L’évaluation de l’âge constitue désormais une page distincte du processus de soumission. Pour plus d’informations, voir la section [Classification par âge](age-ratings.md).

## Catégorie et sous-catégorie

Dans cette section, vous indiquez la catégorie (et la sous-catégorie s'il y a lieu) dans laquelle le Windows Store doit classer votre application. Vous devez définir une catégorie pour être en mesure de soumettre votre application.

Pour plus d’informations, voir l’article [Tableau des catégories et sous-catégories](category-and-subcategory-table.md).

## Déclarations de produit

Les cases à cocher de cette section vous permettent d’indiquer les déclarations qui s’appliquent à votre application. Votre sélection affecte le mode d'affichage de votre application, le public auquel elle est proposée ou la façon dont les clients peuvent l'utiliser.

Pour plus d’informations, voir l’article [Déclarations d’application](app-declarations.md).

## Configuration système

Cette section vous permet d’indiquer si certaines fonctionnalités matérielles sont requises ou recommandées pour exécuter votre application et interagir avec cette dernière. Lorsque vous souhaitez spécifier **Matériel minimum** et/ou **Matériel recommandé** pour un composant matériel, activez la case à cocher (ou indiquez l’option appropriée).

Si vous effectuez des sélections pour **Matériel recommandé**, ces composants sont affichés dans la description du produit sur le Windows Store en tant que matériel recommandé pour les clients utilisant Windows10, version1607 ou ultérieure. Les clients utilisant des versions antérieures du système d’exploitation ne voient pas ces informations.

Si vous effectuez des sélections pour **Matériel minimum**, ces composants sont affichés dans la description du produit sur le Windows Store en tant que matériel minimum requis pour les clients utilisant Windows10, version1607 ou ultérieure. Les clients utilisant des versions antérieures du système d’exploitation ne voient pas ces informations. Le Windows Store peut également afficher un avertissement pour les clients qui consultent la description de votre application sur un appareil ne disposant pas du matériel requis. Les clients peuvent quand même télécharger votre application sur des appareils non équipés du matériel nécessaire, mais ils ne sont pas en mesure d’évaluer ou de commenter votre application sur ces appareils. 

Le comportement des clients varie selon les configurations requises spécifiques et la version de Windows du client:

- **Pour les clients utilisant Windows10, version1607 ou ultérieure:**
     - La configuration minimale requise et recommandée est affichée dans la description du Windows Store.
     - Le Windows Store vérifie la configuration minimale requise et affiche un avertissement si l’appareil du client ne satisfait pas ces exigences.
- **Pour les clients utilisant des versions antérieures de Windows10:**
     - Pour la plupart des clients, la configuration matérielle minimale requise et recommandée s’affiche dans la description au sein du Windows Store (toutefois les clients utilisant une version antérieure du client Windows Store voient seulement la configuration matérielle minimale requise).
     - Le Windows Store tente de vérifier les composants que vous spécifiez dans **Matériel minimum**, à l’exception de **Mémoire**, **DirectX**, **Mémoire vidéo**, **Carte graphique** et **Processeur**. Aucun de ces composants n’est vérifié, et les clients ne voient aucun avertissement sur les appareils qui ne satisfont pas ces exigences. 
- **Pour les clients utilisant Windows8.x et des versions antérieures ou Windows Phone8.x et des versions antérieures:**
     - Si vous activez la case à cocher **Matériel minimum** pour **Écran tactile**, cette configuration requise s’affiche dans la description de votre application sur le Windows Store, et les clients dont les appareils n’ont pas d’écran tactile voient un avertissement s’ils tentent de télécharger l’application. Aucune autre configuration requise n’est vérifiée ou affichée dans votre description au sein du Windows Store.

Nous vous recommandons également d’ajouter des vérifications d’exécution pour le matériel spécifié dans votre application, car il peut arriver que le Windows Store ne puisse pas détecter que l’appareil d’un client ne dispose pas des fonctionnalités sélectionnées. Dans ce cas, le client peut toujours télécharger votre application, même si un avertissement s’affiche.

> **Conseil** Si vous souhaitez empêcher complètement le téléchargement de votre application UWP sur un appareil qui ne satisfait pas la configuration minimale requise pour la mémoire ou le niveau DirectX, vous pouvez indiquer cette configuration minimale requise dans un fichier XML StoreManifest. Pour plus d’informations, voir [Schéma StoreManifest (Windows 10)](https://msdn.microsoft.com/library/windows/apps/mt617335).





<!--HONumber=Aug16_HO5-->


