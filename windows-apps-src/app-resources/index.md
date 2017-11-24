---
author: stevewhims
Description: "Cette section vous montre comment créer, mettre en package et utiliser les ressources de chaîne, d’image et de fichier de votre application."
title: "Ressources d’application et système de gestion des ressources"
label: Intro
template: detail.hbs
ms.author: stwhi
ms.date: 10/20/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp, ressources, image, MRT, qualificateur
localizationpriority: medium
ms.openlocfilehash: 38a131704bacbffdf89636aa70b405aa30861d27
ms.sourcegitcommit: d0c93d734639bd31f264424ae5b6fead903a951d
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/03/2017
---
# <a name="app-resources-and-the-resource-management-system"></a>Ressources d’application et système de gestion des ressources
<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

Cette section vous montre comment créer, mettre en package et utiliser les ressources de chaîne, d’image et de fichier de votre application. Par exemple, vous pouvez créer un package contenant un fichier avec votre jeu simple contenant une définition des niveaux de jeu, puis charger le fichier au moment de l’exécution. Nous vous montrons également comment, en conservant vos ressources séparées de la logique de l’application, il est plus facile de localiser et personnaliser votre application pour différents paramètres régionaux, écrans d’appareils, paramètres d’accessibilité et autres contextes d’utilisateur et d’ordinateur. Les ressources telles que les chaînes et les images doivent généralement exister dans plusieurs variantes de langue, échelle et contraste. Pour ce type de ressources, vous pouvez vous appuyer sur le [système de gestion des ressources](resource-management-system.md).

Il existe deux types de ressource d’application.
- Une ressource de fichier est une ressource stockée sous la forme d’un fichier sur disque. Une ressource de fichier peut contenir une image bitmap, des données XAML, XML, HTML ou tout autre type de données.
- Une ressource incorporée est une ressource qui est incorporée dans un fichier de ressources contenant. L’exemple le plus courant est une ressource de chaîne incorporée dans un fichier de ressources (.resw ou.resjson).

Pour plus d’informations sur la proposition de valeur de la localisation de votre application, voir [Internationalisation et localisation](../globalizing/globalizing-portal.md).

| Article | Description |
|---------|-------------|
| [Système de gestion des ressources](resource-management-system.md) | Lors de la génération, le système de gestion des ressources crée un index de toutes les variantes de ressources qui sont incluses dans le package avec votre application. Lors de l’exécution, le système détecte les paramètres en vigueur pour l’utilisateur et l’ordinateur et charge les ressources les plus appropriées pour ces paramètres. |
| [Comment le système de gestion des ressources met en correspondance et sélectionne les ressources](how-rms-matches-and-chooses-resources.md) | Lorsqu’une ressource est requise, plusieurs candidats sont susceptibles de correspondre, dans une certaine mesure, au contexte de ressource actuel. Le système de gestion des ressources analyse tous les candidats et identifie le meilleur à renvoyer. Cette rubrique décrit ce processus en détail et donne des exemples. |
| [Comment le système de gestion des ressources met en correspondance les balises de langue](how-rms-matches-lang-tags.md) | La rubrique précédente ([Comment le système de gestion des ressources met en correspondance et sélectionne les ressources](how-rms-matches-and-chooses-resources.md)) aborde la correspondance entre les qualificateurs de manière générale. Cette rubrique se concentre plus particulièrement sur la correspondance entre les balises de langue. |
| [Personnaliser vos ressources pour la langue, l’échelle, le contraste élevé et d’autres qualificateurs](tailor-resources-lang-scale-contrast.md) | Cette rubrique explique le concept général des qualificateurs de ressource, leur utilisation et le rôle de chaque nom de qualificateur. |
| [Localiser les chaînes dans l’interface utilisateur et le manifeste du package d’application](localize-strings-ui-manifest.md) | Si vous souhaitez que votre application prenne en charge différentes langues d’affichage et si des opérateurs de chaîne figurent dans votre code, votre balisage XAML ou dans le manifeste du package d’application, déplacez ces chaînes dans un fichier de ressources (.resw). Vous pouvez ensuite effectuer une copie traduite de ce fichier de ressources pour chaque langue prise en charge par votre application. |
| [Charger des images et des ressources adaptées à l’échelle, au thème, au contraste élevé et à d’autres contextes](images-tailored-for-scale-theme-contrast.md) | Votre application peut charger des fichiers de ressources d’image contenant des images adaptées au facteur d’échelle de l’affichage, au thème, au contraste élevé et à d’autres contextes d’exécution. |
| [Prise en charge des vignettes et notifications toast pour la langue, l’échelle et le contraste élevé](tile-toast-language-scale-contrast.md) | Vos vignettes et toasts peuvent charger des chaînes et des images adaptées à la langue, au facteur d’échelle de l’affichage, au contraste élevé et à d’autres contextes d’exécution. |
| [Schémas d’URI](uri-schemes.md) | Vous pouvez utiliser plusieurs schémas d’URI (Uniform Resource Identifier) pour faire référence à des fichiers provenant de votre package d’application, des dossiers de données de votre application ou du cloud. Vous pouvez également utiliser un schéma d’URI pour faire référence à des chaînes chargées à partir des fichiers de ressources (.resw) de votre application. |
| [Compiler des ressources manuellement avec MakePri.exe](compile-resources-manually-with-makepri.md) | MakePri.exe est un outil de ligne de commande que vous pouvez utiliser pour créer et vider des fichiers PRI. Il est intégré en tant qu’élément de MSBuild dans MicrosoftVisualStudio, mais il peut aussi être utilisé pour la création de packages manuellement ou à l’aide d’un système de génération personnalisé. |
| [Options de ligne de commande de MakePri.exe](makepri-exe-command-options.md) | MakePri.exe inclut le jeu de commandes `createconfig`, `dump`, `new`, `resourcepack` et `versioned`. Cette rubrique détaille les options de ligne de commande utilisées avec ces commandes. |
| [Fichier de configuration de MakePri.exe](makepri-exe-configuration.md) | Cette rubrique décrit le schéma du fichier de configurationXML de MakePri.exe. |
| [Indexeurs spécifiques au format de MakePri.exe](makepri-exe-format-specific-indexers.md) | Cette rubrique décrit les indexeurs spécifiques au format qu’utilise l’outil MakePri.exe pour générer son index de ressources. |
| [Utiliser le système de gestion des ressources Windows10 dans une application ou un jeu hérité](using-mrt-for-converted-desktop-apps-and-games.md) | En incluant votre application ou jeu.NET ouWin32 dans un packageAppX, vous pouvez exploiter le système de gestion des ressources pour charger des ressources d’application adaptées au contexte d’exécution. Cette rubrique détaillée décrit ces techniques. |

Consultez également la documentation initialement créée pour Windows8.x, qui s’applique toujours aux applications de plateforme Windows universelle (UWP) et à Windows10.

-   [Ressources et localisation des applications](https://msdn.microsoft.com/library/windows/apps/xaml/hh710212.aspx)
