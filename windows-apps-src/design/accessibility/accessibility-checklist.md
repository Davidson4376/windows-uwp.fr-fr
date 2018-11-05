---
author: Xansky
Description: Provides a checklist to help you ensure that your Universal Windows Platform (UWP) app is accessible.
ms.assetid: BB8399E2-7013-4F77-AF2C-C1A0E5412856
title: Liste de vérification de l’accessibilité
label: Accessibility checklist
template: detail.hbs
ms.author: mhopkins
ms.date: 02/08/2017
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: bf9c3c4d803c4e5c2bc369449b83cf711bda6f99
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/05/2018
ms.locfileid: "6027417"
---
# <a name="accessibility-checklist"></a>Liste de vérification de l’accessibilité



Fournit une liste de vérification qui vous aide à vous assurer que votre application de plateforme Windows universelle (UWP) est accessible.

Nous fournissons ici une liste de vérification qui vous permet de vous assurer que votre application est accessible.

1.  Définissez le nom accessible (obligatoire) et la description accessible (facultative) du contenu et des éléments d’interface utilisateur interactifs de votre application.

    Le nom accessible est une chaîne de texte courte et descriptive qui est utilisée par les lecteurs d’écran pour présenter un élément d’interface utilisateur. Certains éléments d’interface utilisateur tels que [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/BR209652) et [**TextBox**](https://msdn.microsoft.com/library/windows/apps/BR209683) effectuent la promotion de leur contenu texte comme nom accessible par défaut ; voir [Informations d’accessibilité élémentaires](basic-accessibility-information.md#name_from_inner_text).

    Vous devez définir le nom accessible de manière explicite pour les images ou autres contrôles qui n’effectuent pas la promotion du contenu de texte interne comme nom accessible implicite. Vous devez utiliser des étiquettes pour les éléments de formulaires afin que le texte d’étiquette puisse être utilisé comme cible [**LabeledBy**](https://msdn.microsoft.com/library/windows/apps/Hh759769) dans le modèle Microsoft UI Automation pour la corrélation entre les étiquettes et les entrées. Si vous souhaitez fournir davantage d’instructions dans l’interface utilisateur que celles normalement fournies par le nom accessible, des descriptions accessibles et des info-bulles aident les utilisateurs à mieux comprendre l’interface utilisateur.

    Pour plus d’informations, voir les sections [Nom accessible](basic-accessibility-information.md#accessible_name) et [Description accessible](basic-accessibility-information.md).

2.  Mettez en œuvre l’accessibilité du clavier :

    * Testez l’ordre d’index de tabulation par défaut pour une interface utilisateur. Ajustez l’ordre d’index de tabulation si nécessaire, ce qui peut exiger l’activation ou la désactivation de certains contrôles ou la modification des valeurs par défaut de [**TabIndex**](https://msdn.microsoft.com/library/windows/apps/BR209461) sur certains éléments d’interface utilisateur.
    * Utilisez des contrôles qui prennent en charge la navigation à l’aide des touches de direction pour les éléments composites. Pour les contrôles par défaut, la navigation à l’aide des touches de direction est en général déjà implémentée.
    * Utilisez des contrôles qui prennent en charge l’activation du clavier. Pour les contrôles par défaut, en particulier ceux qui prennent en charge le modèle [**Invoke**](https://msdn.microsoft.com/library/windows/apps/BR242582) UI Automation, l’activation du clavier est généralement disponible ; vérifiez la documentation de ce contrôle.
    * Définissez des touches d’accès rapide ou mettez en œuvre des touches accélérateur pour les parties spécifiques de l’interface utilisateur qui prennent en charge l’interaction.
    * Pour tout contrôle personnalisé que vous utilisez dans votre interface utilisateur, vérifiez que vous avez mis en œuvre ces contrôles avec la prise en charge [**AutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR209185) correcte pour l’activation et que vous avez défini des substitutions pour la gestion des touches selon les besoins pour prendre en charge l’activation, la traversée et les touches d’accès rapide ou accélérateur.

    Pour plus d’informations, voir [Interactions avec le clavier](https://msdn.microsoft.com/library/windows/apps/Mt185607).

3.  Vérifiez visuellement votre interface utilisateur pour vous assurer que le contraste du texte est suffisant, que le rendu des éléments est correct dans les thèmes à contraste élevé et que les couleurs sont utilisées correctement.

    * Utilisez les options d’affichage système qui ajustent la valeur en haute résolution de l’affichage et assurez-vous que l’interface utilisateur de votre application est correctement mise à l’échelle lorsque cette valeur est modifiée. (Certains utilisateurs modifient les valeurs haute résolution en tant qu’option d’accessibilité, par le biais du champ **Options d’ergonomie**.)
    * Utilisez un outil d’analyse des couleurs pour vérifier que le coefficient de contraste de texte visuel est au moins de 4,5 pour 1.
    * Basculez vers un thème à contraste élevé et vérifiez que l’interface utilisateur de votre application est lisible et utilisable.
    * Assurez-vous que votre interface utilisateur n’utilise pas la couleur comme seule façon de transmettre les informations.

    Pour plus d’informations, voir les rubriques [Thèmes à contraste élevé](high-contrast-themes.md) et [Exigences de texte accessible](accessible-text-requirements.md).

4.  Exécutez les outils d’accessibilité, traitez les problèmes signalés et vérifiez l’expérience de lecture d’écran.

    Utilisez des outils tels que [**Inspect**](https://msdn.microsoft.com/library/windows/desktop/Dd318521) pour vérifier l’accès par programme, exécutez des outils de diagnostic tels que [**AccChecker**](https://msdn.microsoft.com/library/windows/desktop/Hh920985) pour identifier les erreurs courantes et vérifiez l’expérience de lecture d’écran avec le Narrateur.

    Pour plus d’informations, voir [Tests d’accessibilité](accessibility-testing.md).

5.  Assurez-vous que vos paramètres de manifeste d’application respectent les recommandations en matière d’accessibilité.

6.  Déclarez votre application comme accessible dans le MicrosoftStore.

    Si vous avez implémenté la prise en charge de l’accessibilité de base, le fait de marquer votre application comme accessible dans le MicrosoftStore peut vous permettre d’atteindre davantage de clients et d’obtenir davantage de bonnes évaluations.

    Pour plus d’informations, voir [Accessibilité dans le Windows Store](accessibility-in-the-store.md).

<span id="related_topics"/>

## <a name="related-topics"></a>Rubriques connexes  
* [Accessibilité](accessibility.md)
* [Concevoir des applications pour l’accessibilité](https://msdn.microsoft.com/library/windows/apps/Hh700407)
* [Pratiques à éviter](practices-to-avoid.md) 
