---
Description: Décrit les étapes requises pour garantir que votre application UWP est utilisable lorsqu’un thème à contraste élevé est actif.
title: Thèmes à contraste élevé
ms.assetid: FD7CA6F6-A8F1-47D8-AA6C-3F2EC3168C45
label: High-contrast themes
template: detail.hbs
---

Thèmes à contraste élevé
=============================================================================

\[ Mise à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]


Décrit les étapes requises pour garantir que votre application UWP est utilisable lorsqu’un thème à contraste élevé est actif.

Une application UWP prend en charge les thèmes à contraste élevé par défaut. Si un utilisateur a décidé que le système doit utiliser un thème à contraste élevé parmi les outils d’accessibilité ou les paramètres système, l’infrastructure utilise automatiquement des couleurs et paramètres de style qui produisent une disposition à contraste élevé et un rendu pour les contrôles et composants dans l’interface utilisateur.

Cette prise en charge par défaut est basée sur l’utilisation des thèmes et des modèles par défaut. Ces thèmes et modèles font référence aux couleurs système comme définitions de ressources, et les sources des ressources changent automatiquement quand le système utilise un mode de contraste élevé. Toutefois, si vous utilisez des modèles, thèmes et styles personnalisés pour votre contrôle, prenez soin de ne pas désactiver la prise en charge intégrée du contraste élevé. Si vous utilisez l’un des concepteurs XAML pour Microsoft Visual Studio pour les styles, le concepteur génère un thème à contraste élevé distinct en plus du thème principal chaque fois que vous définissez un modèle qui diffère sensiblement du modèle par défaut. Les dictionnaires de thèmes distincts s’inscrivent dans la collection [**ThemeDictionaries**](https://msdn.microsoft.com/library/windows/apps/BR208807), une propriété dédiée d’un élément [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/BR208794).

Pour plus d’informations sur les thèmes et modèles de contrôles, voir [Démarrage rapide : modèles de contrôles](https://msdn.microsoft.com/library/windows/apps/xaml/Hh465374). Il est souvent très instructif d’examiner les thèmes et dictionnaires de ressources XAML pour des contrôles spécifiques, et de voir la façon dont les thèmes sont créés et dont ils font référence à des ressources qui sont similaires mais différentes pour chaque paramètre de contraste élevé possible.

<span id="Detecting_when_a_high-contrast_theme_is_enabled"> </span> <span id="detecting_when_a_high-contrast_theme_is_enabled"> </span> <span id="DETECTING_WHEN_A_HIGH-CONTRAST_THEME_IS_ENABLED"> </span>Détection de l’activation d’un thème à contraste élevé
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Une application UWP peut utiliser des membres de la classe [**AccessibilitySettings**](https://msdn.microsoft.com/library/windows/apps/BR242237) pour détecter les paramètres actuels pour les thèmes à contraste élevé. La propriété [**HighContrast**](https://msdn.microsoft.com/library/windows/apps/BR242237_highcontrast) détermine si un thème à contraste élevé est actuellement sélectionné. Si **HighContrast** a la valeur **true**, l’étape suivante consiste à vérifier la valeur de la propriété [**HighContrastScheme**](https://msdn.microsoft.com/library/windows/apps/BR242237_highcontrastscheme) afin d’obtenir le nom du thème à contraste élevé utilisé. « Contraste blanc élevé » et « Contraste noir élevé » sont les valeurs normales de **HighContrastScheme** auxquelles votre code doit répondre. Comme les clés [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/BR208794) définies en XAML ne peuvent pas contenir d’espaces, les clés de ces thèmes dans un dictionnaire de ressources sont généralement « HighContrastWhite » et « HighContrastBlack », respectivement. Vous devez également disposer d’une logique de remplacement pour un thème à contraste élevé par défaut au cas où la valeur serait une autre chaîne. L’[exemple de contraste élevé XAML](http://go.microsoft.com/fwlink/p/?linkid=254993) illustre cette logique.

**Remarque** Prenez soin d’appeler le constructeur [**AccessibilitySettings**](https://msdn.microsoft.com/library/windows/apps/BR242237) à partir d’une étendue dans laquelle l’application est initialisée et affiche déjà du contenu.

 

Les applications peuvent utiliser des valeurs de ressource à contraste élevé pendant qu’elles sont en cours d’exécution. Cela ne fonctionne que si les ressources sont demandées à l’aide de l’[extension de balisage {ThemeResource}](https://msdn.microsoft.com/library/windows/apps/Mt185591) dans le code XAML du style ou du modèle. Comme les thèmes par défaut (generic.xaml) utilisent tous cette technique d’extension de balisage {ThemeResource}, vous obtenez ce comportement si vous utilisez des thèmes de contrôle par défaut. Les contrôles ou styles de contrôle personnalisés prennent cela en charge si vous avez également utilisé cette technique d’extension de balisage {ThemeResource} dans vos modèles et styles personnalisés.

<span id="related_topics"> </span>Rubriques connexes
-----------------------------------------------

* [Accessibilité](accessibility.md)
* [Exemple de paramètres et de contraste d’interface utilisateur](http://go.microsoft.com/fwlink/p/?linkid=231539)
* [Exemple d’accessibilité XAML](http://go.microsoft.com/fwlink/p/?linkid=238570)
* [Exemple de contraste élevé XAML](http://go.microsoft.com/fwlink/p/?linkid=254993)
* [**AccessibilitySettings**](https://msdn.microsoft.com/library/windows/apps/BR242237)
 

 





<!--HONumber=Mar16_HO3-->


