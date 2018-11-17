---
author: stevewhims
Description: This section shows you how to author, package, and consume your app's string, image, and file resources.
title: Ressources d’application et système de gestion des ressources
label: Intro
template: detail.hbs
ms.author: stwhi
ms.date: 10/20/2017
ms.topic: article
keywords: windows10, uwp, ressources, image, MRT, qualificateur
ms.localizationpriority: medium
ms.openlocfilehash: 199f9def3150373fed2b3c7d8e711c1eeda6e721
ms.sourcegitcommit: 3257416aebb5a7b1515e107866806f8bd57845a8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/16/2018
ms.locfileid: "7148541"
---
# <a name="app-resources-and-the-resource-management-system"></a>Ressources d’application et système de gestion des ressources


Cette section vous montre comment créer, mettre en package et utiliser les ressources de chaîne, d’image et de fichier de votre application. Par exemple, vous pouvez créer un package contenant un fichier avec votre jeu simple contenant une définition des niveaux de jeu, puis charger le fichier au moment de l’exécution. Nous vous montrons également comment, en conservant vos ressources séparées de la logique de l’application, il est plus facile de localiser et personnaliser votre application pour différents paramètres régionaux, écrans d’appareils, paramètres d’accessibilité et autres contextes d’utilisateur et d’ordinateur. Les ressources telles que les chaînes et les images doivent généralement exister dans plusieurs variantes de langue, échelle et contraste. Pour ce type de ressources, vous pouvez vous appuyer sur le [système de gestion des ressources](resource-management-system.md).

Il existe deux types de ressource d’application.
- Une ressource de fichier est une ressource stockée sous la forme d’un fichier sur disque. Une ressource de fichier peut contenir une image bitmap, des données XAML, XML, HTML ou tout autre type de données.
- Une ressource incorporée est une ressource qui est incorporée dans un fichier de ressources contenant. L’exemple le plus courant est une ressource de chaîne incorporée dans un fichier de ressources (.resw ou.resjson).

Pour plus d’informations sur la proposition de valeur de la localisation de votre application, voir [Internationalisation et localisation](../design/globalizing/globalizing-portal.md).

| Article | Description |
|---------|-------------|
| [Système de gestion des ressources](resource-management-system.md) | Lors de la génération, le système de gestion des ressources crée un index de toutes les variantes de ressources qui sont incluses dans le package avec votre application. Lors de l’exécution, le système détecte les paramètres en vigueur pour l’utilisateur et l’ordinateur et charge les ressources les plus appropriées pour ces paramètres. |
| [Comment le système de gestion des ressources met en correspondance et sélectionne les ressources](how-rms-matches-and-chooses-resources.md) | Lorsqu’une ressource est requise, plusieurs candidats sont susceptibles de correspondre, dans une certaine mesure, au contexte de ressource actuel. Le système de gestion des ressources analyse tous les candidats et identifie le meilleur à renvoyer. Cette rubrique décrit ce processus en détail et donne des exemples. |
| [Comment le système de gestion des ressources met en correspondance les balises de langue](how-rms-matches-lang-tags.md) | La rubrique précédente ([Comment le système de gestion des ressources met en correspondance et sélectionne les ressources](how-rms-matches-and-chooses-resources.md)) aborde la correspondance entre les qualificateurs de manière générale. Cette rubrique se concentre plus particulièrement sur la correspondance entre les balises de langue. |
| [Personnaliser vos ressources pour la langue, l’échelle, le contraste élevé et d’autres qualificateurs](tailor-resources-lang-scale-contrast.md) | Cette rubrique explique le concept général des qualificateurs de ressource, leur utilisation et le rôle de chaque nom de qualificateur. |
| [Localiser les chaînes dans l’interface utilisateur et le manifeste du package d’application](localize-strings-ui-manifest.md) | Si vous souhaitez que votre application prenne en charge différentes langues d’affichage et si des opérateurs de chaîne figurent dans votre code, votre balisage XAML ou dans le manifeste du package d’application, déplacez ces chaînes dans un fichier de ressources (.resw). Vous pouvez ensuite effectuer une copie traduite de ce fichier de ressources pour chaque langue prise en charge par votre application. |
| [Charger des images et des ressources adaptées à l’échelle, au thème, au contraste élevé et à d’autres contextes](images-tailored-for-scale-theme-contrast.md) | Votre application peut charger des fichiers de ressources d’image contenant des images adaptées pour le facteur d’échelle de l’affichage, le thème, le contraste élevé et d’autres contextes d’exécution. |
| [Schémas d’URI](uri-schemes.md) | Vous pouvez utiliser plusieurs schémas d’URI (Uniform Resource Identifier) pour faire référence à des fichiers qui proviennent de votre package d’application, des dossiers de données de votre application ou du cloud. Vous pouvez également utiliser un schéma d’URI pour faire référence à des chaînes chargées à partir des fichiers de ressources (.resw) de votre application. |
| [Préciser les ressources par défaut que votre application utilise](specify-default-resources-installed.md) | Si votre application n’a pas les ressources qui correspondent aux paramètres particuliers d’un périphérique client, les ressources de l’application par défaut sont utilisées. Cette rubrique explique comment spécifier ce que sont ces ressources par défaut. |
| [Générer des ressources dans votre package d’application, plutôt que dans un pack de ressources](build-resources-into-app-package.md) | Certains types d’applications (dictionnaires multilingues, outils de traduction, etc.) doivent remplacer le comportement par défaut d’un ensemble d’applications et créer des ressources dans le package d’application plutôt que dans des packages de ressources distincts. Cette rubrique explique la procédure à suivre. |
| [API d’indexation de ressources de package (IRP) et systèmes de génération personnalisés](pri-apis-custom-build-systems.md) | Grâce aux [API d’indexation de ressource de package (IRP)](https://msdn.microsoft.com/library/windows/desktop/mt845690), vous pouvez développer un système de génération personnalisé pour les ressources de votre application UWP. Le système de génération pourra créer, versionner, et vider (en tant que XML) les fichiers d’index de ressource de package (IRP) au niveau de complexité dont votre application UWP a besoin. |
| [Compiler des ressources manuellement avec MakePri.exe](compile-resources-manually-with-makepri.md) | MakePri.exe est un outil de ligne de commande que vous pouvez utiliser pour créer et vider des fichiers PRI. Il est intégré en tant qu’élément de MSBuild dans MicrosoftVisualStudio, mais il peut aussi être utilisé pour la création de packages manuellement ou à l’aide d’un système de génération personnalisé. |
| [Utiliser le système de gestion des ressources Windows10 dans une application ou un jeu hérité](using-mrt-for-converted-desktop-apps-and-games.md) | En incluant votre application ou jeu.NET ouWin32 dans un packageAppX, vous pouvez exploiter le système de gestion des ressources pour charger des ressources d’application adaptées au contexte d’exécution. Cette rubrique détaillée décrit ces techniques. |

Voir aussi [Prise en charge des notifications de vignettes et toasts pour la langue, la mise à l’échelle et le contraste élevé](../design/shell/tiles-and-notifications/tile-toast-language-scale-contrast.md).
