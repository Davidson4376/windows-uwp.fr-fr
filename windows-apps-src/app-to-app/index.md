---
ms.assetid: E0728EB0-DFC3-4203-A367-8997B16E2328
description: Cette section explique comment partager des données entre des applications UWP, notamment comment utiliser le contrat de partage, copier et coller, et glisser-déplacer.
title: Communication entre les applications
author: awkoren
---

# Communication entre les applications

\[ Article mis à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Cette section explique comment partager des données entre des applications de plateforme Windows universelle (UWP), notamment comment utiliser le contrat de partage, copier et coller, et glisser-déplacer.

Le contrat de partage permet aux utilisateurs d’échanger rapidement des données entre des applications. Par exemple, un utilisateur peut partager une page web avec ses amis à l’aide d’une application de réseau social ou enregistrer un lien dans une application de prise de notes pour s’y référer plus tard. Utilisez un contrat de partage si votre application reçoit du contenu dans les scénarios qu’un utilisateur peut traiter rapidement tout en étant dans une autre application.

Une application peut prendre en charge la fonctionnalité de partage de deux manières. En premier lieu, il peut s’agir d’une application source qui fournit le contenu que l’utilisateur veut partager. En second lieu, l’application peut être une application cible que l’utilisateur sélectionne en tant que destination du contenu partagé. Notez qu’une application peut à la fois être une application source ou une application cible. Si vous voulez que votre application partage du contenu en tant qu’application source, vous devez déterminer quels formats de données votre application peut fournir.

Outre le contrat de partage, les applications peuvent également intégrer des techniques classiques de transfert de données, comme glisser-déplacer ou copier-coller. Outre la communication entre les applications UWP, ces méthodes prennent également en charge le partage vers et depuis des applications de bureau.

## Dans cette section

| Rubrique | Description |
|-------|-------------|
| [Partager des données](share-data.md) | Cet article explique comment prendre en charge le contrat de partage dans une application UWP. Le contrat de partage constitue un moyen simple pour partager rapidement des données, telles que du texte, des liens, des photos et vidéos, entre les applications. Par exemple, un utilisateur peut partager une page web avec ses amis à l’aide d’une application de réseau social ou enregistrer un lien dans une application de prise de notes pour s’y référer plus tard. |
| [Recevoir des données](receive-data.md) | Cet article explique comment recevoir dans votre application UWP du contenu partagé à partir d’une autre application à l’aide du contrat de partage. Ce contrat de partage permet à votre application d’être présentée en tant qu’option quand l’utilisateur appelle l’option Partager. |
| [Copier et coller](copy-and-paste.md) | Cet article explique comment prendre en charge le copier-coller dans les applications UWP en utilisant le Presse-papiers. Le copier-coller est la méthode classique utiliser pour échanger des données entre les applications, ou dans une application, et presque chaque application peut prendre en charge les opérations du Presse-papiers dans une certaine mesure. |
| [Glisser-déplacer](drag-and-drop.md) | Cet article explique comment ajouter le glisser-déplacer dans votre application UWP. Glisser-déplacer est une méthode naturelle et classique d’interaction avec le contenu comme les images et les fichiers. Une fois implémenté, le glisser-déplacer fonctionne parfaitement dans toutes les directions, notamment d’application à application, d’application à bureau et de bureau à application. |
| [Utiliser EDP pour protéger les données d’entreprise transférées entre les applications](use-edp-to-protect-enterprise-data-transferred-between-apps.md) | Cette rubrique présente des exemples de tâches de codage nécessaires pour réaliser certains des scénarios de protection des données d’entreprise (EDP) relatifs au transfert de fichiers les plus courants. |


<!--HONumber=May16_HO2-->


