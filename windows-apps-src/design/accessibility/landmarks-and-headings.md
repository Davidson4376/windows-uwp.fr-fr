---
author: Xansky
Description: Describes the landmarks and headings features of accessibility.
ms.assetid: 019CC63D-D915-4EBD-9442-DE899AB973C9
title: Repères et en-têtes
label: Landmarks and Headings
template: detail.hbs
ms.author: mhopkins
ms.date: 01/24/2018
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 250ed555e6fcf7dc40d31d89a40fa7a96295aacf
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/05/2018
ms.locfileid: "6036047"
---
# <a name="landmarks-and-headings"></a>Repères et en-têtes

Une interface utilisateur est généralement organisée d’une manière visuellement efficace pour permettre à un utilisateur voyant de repérer rapidement ce qui l’intéresse sans avoir à lire *tout* le contenu. Un utilisateur de lecteur d’écran doit disposer de cette même capacité de sélection. Les repères et les en-têtes définissent les sections d’une interface utilisateur qui facilitent la navigation pour les utilisateurs de technologies d’assistance telles que les lecteurs d’écran. Le marquage du contenu en repères et en-têtes permet à l’utilisateur d’un lecteur d’écran de parcourir le contenu de la même façon qu’un utilisateur voyant.

Les concepts de [repèresARIA](https://www.w3.org/WAI/GL/wiki/Using_ARIA_landmarks_to_identify_regions_of_a_page), d’[en-têtesARIA](https://www.w3.org/TR/WCAG20-TECHS/ARIA12.html) et d’[en-têtesHTML](https://www.w3.org/TR/2016/NOTE-WCAG20-TECHS-20161007/H42.html) ont été utilisés dans le contenu Web pendant des années pour permettre aux utilisateurs de lecteur d’écran d’accélérer leur navigation. Les pages Web utilisent des repères et des en-têtes pour rendre leur contenu plus utilisable en permettant à l’utilisateur de technologies d’assistance d’atteindre rapidement les parties les plus volumineuses (repère) et les plus petites (en-tête). Plus précisément, les lecteurs d’écran disposent de commandes permettant aux utilisateurs de passer d’un repère et d’une rubrique à un autre (niveau d’en-tête suivant/précédent ou spécifique). Il est important de prendre en compte les repères et les en-têtes de vos applications pour les mêmes raisons.

Les repères permettent de regrouper le contenu en différentes catégories, telles que la recherche, la navigation, le contenu principal, etc. Une fois le regroupement effectué, l’utilisateur de technologies d’assistance peut rapidement naviguer entre les groupes. Cette navigation rapide permet à l’utilisateur de sauter des quantités potentiellement substantielles de contenu qui, auparavant, auraient dû être parcourues article par article, et donnant lieu à une mauvaise expérience. 

Par exemple, lorsque vous utilisez un volet à onglets, considérez-le comme un repère de navigation. Lorsque vous utilisez une zone d’édition de recherche, considérez-la comme un repère de recherche et définissez votre contenu principal en tant que repère de contenu principal. Que ce soit à l’intérieur ou à l’extérieur d’un repère, pensez à définir des sous-éléments comme en-têtes avec des niveaux de titre logiques. 

Consultez la page **Options d’ergonomie** de l’application Paramètres Windows. 

![Page Options d’ergonomie dans l’application Paramètres Windows](images/EaseOfAccessSettings.png)  

Une zone d’édition de recherche est encapsulée dans un repère recherche. Les éléments de navigation de gauche sont enveloppés dans un repère de navigation et le contenu principal de droite est enveloppé dans un repère de contenu principal. Pour aller plus loin, à l’intérieur du repère de navigation, l’en-tête de groupe principal appelé **Options d’ergonomie** est de niveau1. Les options secondaires **Vision**, **Audition**, et ainsi de suite se trouvent au niveau inférieur. Elles ont un niveau d’en-tête de 2. La définition d’en-têtes se poursuit dans le contenu principal en réglant à nouveau le sujet principal, **Affichage**, comme niveau1 et les sous-groupes comme **Réaliser tout ce qui est plus grand** comme en-tête de niveau2. 

L’application Paramètres serait accessible sans repères et en-têtes, mais elle devient plus utilisable avec eux. Un utilisateur de lecteur d’écran peut rapidement et facilement obtenir le groupe (repère) dont il a besoin, et accéder rapidement au sous-groupe (en-têtes) également. 

Utilisez [AutomationProperties.LandmarkTypeProperty](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.automationproperties.LandmarkTypeProperty) pour configurer l’élément d’interface utilisateur en tant que [type de repère](https://msdn.microsoft.com/library/windows/desktop/mt759299) vous le souhaitez. L’élément d’interface utilisateur de ce repère encapsulerait tous les autres éléments de l’interface utilisateur , ce qui est adapté à ce repère. 

Utilisez [AutomationProperties.LocalizedLandmarkTypeProperty](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.automationproperties.LocalizedLandmarkTypeProperty) pour nommer spécifiquement le repère. Si vous sélectionnez un type de repère prédéfini comme objet principal ou de navigation, ces noms seront utilisés pour le nom du repère. Toutefois, si vous définissez le type de repère sur Personnalisé, vous devez nommer spécifiquement le repère par le biais de cette propriété. Vous pouvez également utiliser cette propriété pour remplacer les noms par défaut des types de repère non personnalisé. 

Utilisez [AutomationProperties.HeadingLevel](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.automationproperties.headinglevelproperty) pour définir l’élément de l’interface utilisateur comme en-tête d’un niveau donné compris entre *Level1* et *Level9*.

