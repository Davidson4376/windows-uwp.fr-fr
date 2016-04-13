---
title: Mode Fractionné
ms.assetid: E9E4537F-1160-4183-9A83-26602FCFDC9A
description: Un contrôle de mode Fractionné inclut un volet, qui peut être visible ou masqué, ainsi qu’une zone de contenu.
label: Mode Fractionné
template: detail.hbs
---

# Recommandations en matière de contrôles de mode Fractionné


\[ Mise à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]


**API importantes**

-   [**Classe SplitView (XAML)**](https://msdn.microsoft.com/library/windows/apps/dn864360)
-   [**Objet SplitView (HTML)**](https://msdn.microsoft.com/library/windows/apps/dn919970)

Un contrôle de mode Fractionné inclut un volet, qui peut être visible ou masqué, ainsi qu’une zone de contenu. La zone de contenu est toujours visible. Le volet peut être visible ou masqué ; il est susceptible de s’afficher depuis le côté gauche ou droit de la fenêtre. Il inclut trois modes :

-   **Superposition**

    Le volet est masqué jusqu’à ce qu’il soit ouvert. Lorsqu’il est ouvert, il recouvre la zone de contenu.

-   **Inclus**

    Le volet est toujours visible et ne recouvre pas la zone de contenu. Les zones de volet et de contenu divisent l’espace disponible à l’écran.

-   **Compact**

    Le volet est toujours visible à l’état compact dans ce mode, sa largeur étant juste suffisante pour afficher les icônes (soit une largeur de 48 epx, en général). La zone de volet et la zone de contenu divisent l’espace disponible à l’écran. Le volet en mode compact standard ne recouvre pas la zone de contenu, mais il peut se transformer en volet plus large pour afficher davantage de contenu, lequel recouvrira la zone de contenu.

## <span id="Is_this_the_right_control_"> </span> <span id="is_this_the_right_control_"> </span> <span id="IS_THIS_THE_RIGHT_CONTROL_"> </span>Est-ce le contrôle approprié ?


Le contrôle de mode Fractionné peut être utilisé pour créer un [modèle de volet de navigation](nav-pane.md). Pour créer ce modèle, ajoutez un bouton Développer/réduire (le bouton « hamburger ») et un affichage Liste au contrôle de mode Fractionné.

## <span id="Examples"> </span> <span id="examples"> </span> <span id="EXAMPLES"> </span>Exemples


Par défaut, le contrôle de mode Fractionné correspond à un conteneur de base. Lorsqu’un bouton et un affichage Liste sont ajoutés, ce contrôle peut être utilisé en tant que menu de navigation. Voici quelques exemples du mode Fractionné utilisé en tant que menu de navigation (mode développé et mode compact).

![Exemple de menu de mode Fractionné en modes de superposition et compact](images/controls-splitview-menu01.png)
## <span id="Recommendations"> </span> <span id="recommendations"> </span> <span id="RECOMMENDATIONS"> </span>Recommandations


-   Lorsque vous utilisez le mode Fractionné pour un menu de navigation, nous recommandons de placer dans le volet les contrôles de navigation qui permettent d’accéder aux autres zones de l’application. L’utilisation du volet de navigation offre une expérience utilisateur cohérente. En outre, cette implémentation de menu peut contribuer à familiariser les utilisateurs avec toutes les parties d’une application, offrir un accès rapide à la page d’accueil de l’application et encourager les utilisateurs à explorer les autres zones de l’application.

\[Cet article contient des informations propres aux applications de plateforme Windows universelle (UWP) et à Windows 10. Pour obtenir de l’aide concernant Windows 8.1, téléchargez le [document PDF de recommandations pour Windows 8.1](https://go.microsoft.com/fwlink/p/?linkid=258743) (en anglais).\]

## <span id="related_topics"> </span>Rubriques connexes


* [Modèle de volet de navigation](nav-pane.md)
* [Affichage Liste](lists.md)
 

 






<!--HONumber=Mar16_HO1-->


