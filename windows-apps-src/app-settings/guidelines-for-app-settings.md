---
author: mijacobs
Description: "Cet article décrit les meilleures pratiques pour créer et afficher des paramètres d’application."
title: "Recommandations en matière de paramètres de l’application"
ms.assetid: 2D765E90-3FA0-42F5-A5CB-BEDC14C3F60A
label: Guidelines
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: 78ede41d559613e89d3174430f5474559f48c6bf
ms.openlocfilehash: 6302ec1bf332a27986876dbde4da92bbc916f85b

---


# Recommandations en matière de paramètres de l’application

Les paramètres d’application sont les parties de votre application qui sont personnalisables par l’utilisateur. Ces paramètres figurent dans une page qui leur est spécialement dédiée. Par exemple, les paramètres d’une application de lecteur de flux d’actualités peuvent permettre à l’utilisateur de spécifier les sources d’informations à afficher ou le nombre de colonnes à présenter à l’écran, tandis que les paramètres d’une application météo peuvent offrir à l’utilisateur la possibilité de choisir entre les unités Celsius et Fahrenheit comme unité de mesure par défaut. Cet article décrit les meilleures pratiques pour créer et afficher des paramètres d’application.

![exemple de volet paramètres](images/app-settings.png)

## Dois-je inclure une page de paramètres dans mon application?

Voici des exemples d’options d’application qui figurent dans une page de paramètres d’application: 

-   Les options de configuration qui affectent le comportement de l’application et qui sont rarement modifiées, comme le choix de l’unité Celsius ou Fahrenheit comme unité de température par défaut dans une application météo, le changement de paramètres de compte dans une application de messagerie, les paramètres des notifications ou les options d’accessibilité.
-   Les options qui dépendent des préférences de l’utilisateur, comme la musique, les effets sonores ou les thèmes de couleur.
-   Les informations d’application rarement consultées, telles que la déclaration de confidentialité, l’aide, la version de l’application ou les informations de copyright.

Les commandes qui font partie du flux de travail classique de l’application (par exemple, la modification de la taille du pinceau dans une application artistique) ne doivent pas figurer dans une page de paramètres. Pour plus d’informations sur la disposition des commandes, voir [Informations de base sur la conception des commandes](../layout/commanding-basics.md).

## Recommandations générales

-   Faites en sorte que les pages de paramètres restent simples et utilisez des contrôles binaires (activé/désactivé). Un [bouton bascule](../controls-and-patterns/toggles.md) constitue généralement le contrôle le mieux adapté pour un paramètre binaire.
-   Pour permettre aux utilisateurs de choisir un élément dans un ensemble de 5options au maximum ayant un lien entre elles, mais mutuellement exclusives, utilisez des [cases d’option](../controls-and-patterns/radio-button.md).
-   Créez un point d’entrée pour tous les paramètres d’application de votre page de paramètres d’application.
-   Veillez à ce que vos paramètres restent simples. Définissez les valeurs par défaut actives et limitez au minimum le nombre de paramètres.
-   Si un utilisateur modifie un paramètre, l’application doit immédiatement refléter cette modification.
-   N’incluez pas de commandes qui font partie intégrante du flux de travail courant de l’application.

## Point d’entrée


La manière dont les utilisateurs accèdent à votre page de paramètres d’application doit reposer sur la disposition de votre application.

**Volet de navigation**

Pour une disposition de volet de navigation, les paramètres d’application doivent correspondre au dernier élément de la liste de choix de navigation et être épinglés dans la partie inférieure:

![point d’entrée des paramètres d’application pour un volet de navigation](images/appsettings-entrypoint-navpane.png)

**Barre de l’application**

Si vous utilisez une barre d’application ou une barre d’outils, qui font généralement partie intégrante d’une disposition de navigation de type hub ou onglets/sélecteurs de vue, placez le point d’entrée en dernier dans le menu volant «Plus». Si vous avez besoin d’améliorer la détectabilité du point d’entrée des paramètres pour votre application, placez le point d’entrée directement dans la barre d’application plutôt que dans le menu volant «Plus».

![point d’entrée des paramètres d’application pour une barre d’application](images/appsettings-entrypoint-tabs.png)

**Hub**

Si vous utilisez une disposition de type hub, le point d’entrée des paramètres d’application doit être placé dans le menu volant « Plus » d’une barre d’application.

**Onglets/sélecteurs de vue**

Dans le cas d’une disposition d’onglets ou de sélecteurs de vue, nous vous déconseillons de placer le point d’entrée des paramètres d’application parmi l’un des premiers éléments de la navigation. Insérez plutôt le point d’entrée des paramètres d’application dans le menu volant « Plus » d’une barre d’application.

**Maître/détails**

Plutôt que d’enfouir profondément le point d’entrée des paramètres d’application dans un volet maître/détails, faites-en le dernier élément épinglé du niveau supérieur du volet maître.

## Disposition


Sur les appareils de bureau et les appareils mobiles, la fenêtre des paramètres d’application doit s’ouvrir en mode plein écran et remplir toute la fenêtre. Si votre menu de paramètres d’application ne comporte pas plus de quatre groupes de niveau supérieur, ces groupes doivent s’afficher en cascade en descendant d’une colonne.

**Bureau**

![disposition d’une page de paramètres d’application sur un appareil de bureau](images/appsettings-layout-navpane-desktop.png)

**Mobile**

![disposition d’une page de paramètres d’application sur un téléphone](images/appsettings-layout-navpane-mobile.png)

## Section «À propos de» et bouton «Envoyer des commentaires»


Si votre application doit comporter une section « À propos de cette application », créez une page de paramètres d’application dédiée à cette section. Si vous voulez inclure un bouton « Envoyer des commentaires », placez-le vers le bas de la page « À propos de cette application ».

Les sections «Conditions d’utilisation» et «Déclaration de confidentialité» doivent être accessibles par le biais de [boutons de lien hypertexte](../controls-and-patterns/hyperlinks.md) avec renvoi à la ligne automatique.

![section «à propos de cette application» avec bouton «envoyer des commentaires»](images/appsettings-about.png)

## Recommandations


### Contenu d’une page de paramètres d’application


Une fois que vous avez répertorié les éléments à inclure dans votre page de paramètres d’application, tenez compte des recommandations suivantes:

-   Regroupez les paramètres similaires ou connexes sous une même étiquette de paramètres.
-   Faites en sorte que le nombre total de paramètres ne dépasse pas quatre ou cinq.
-   Affichez les mêmes paramètres, quel que soit le contexte de l’application. Si certains paramètres ne sont pas pertinents dans un contexte spécifique, désactivez-les dans le menu volant des paramètres d’application.
-   Utilisez des étiquettes descriptives composées d’un seul mot pour les paramètres. Par exemple, nommez l’entrée « Comptes » plutôt que « Paramètres de compte » pour les paramètres liés aux comptes. Si vous ne voulez qu’une seule option pour vos paramètres et que ces derniers ne se prêtent pas à une étiquette descriptive, utilisez « Options » ou « Par défaut ».
-   Si un paramètre est directement lié au web plutôt qu’à un menu volant, informez-en l’utilisateur à l’aide d’un indice visuel, par exemple «Aide (en ligne)» ou «Forums web», doté d’un style de [lien hypertexte](../controls-and-patterns/hyperlinks.md). Envisagez de regrouper plusieurs liens vers le web dans un menu volant constitué d’un seul paramètre. Par exemple, un paramètre « À propos de » peut ouvrir un menu volant proposant des liens vers vos conditions d’utilisation, votre déclaration de confidentialité et le support technique de votre application.
-   Combinez les paramètres peu utilisés dans une même entrée afin que chaque paramètre plus courant puisse disposer de sa propre entrée. Placez le contenu ou les liens qui contiennent uniquement des informations dans un paramètre « À propos de ».
-   Ne dupliquez pas la fonctionnalité du volet « Autorisations ». Windows fournit ce volet par défaut et ne vous permet pas de le modifier.

###  Ajouter le contenu des paramètres aux menus volants des paramètres


-   Présentez le contenu de haut en bas dans une seule colonne que l’utilisateur pourra faire défiler, si nécessaire. Limitez le défilement à un maximum de deux fois la hauteur de l’écran.
-   Utilisez les contrôles suivants pour les paramètres d’application :

    -   [Boutons bascule](../controls-and-patterns/toggles.md): pour permettre aux utilisateurs de définir des valeurs Activé ou Désactivé.
    -   [Cases d’option](../controls-and-patterns/radio-button.md): pour permettre à l’utilisateur de choisir un élément dans un ensemble de 5 options au maximum ayant un lien entre elles, mais mutuellement exclusives.
    -   [Zone de texte](../controls-and-patterns/text-block.md): pour permettre aux utilisateurs d’entrer du texte. Utilisez le type de zone de texte qui correspond au type de texte que vous obtenez de l’utilisateur, tel qu’un message électronique ou un mot de passe.
    -   [Liens hypertexte](../controls-and-patterns/hyperlinks.md): pour diriger l’utilisateur vers une autre page de l’application ou un site Web externe. Quand un utilisateur clique sur un lien hypertexte, le menu volant des paramètres se ferme.
    -   [Boutons](../controls-and-patterns/buttons.md): pour permettre aux utilisateurs de lancer une action immédiate sans quitter le menu volant des paramètres actuel.
-   Ajoutez un message descriptif si l’un des contrôles est désactivé. Placez ce message au-dessus du contrôle désactivé.
-   Une fois l’animation du menu volant Paramètres et de l’en-tête terminée, animez le contenu et les contrôles dans un bloc unique. Animez le contenu à l’aide des animations [**enterPage**](https://msdn.microsoft.com/library/windows/apps/br212672) ou [**EntranceThemeTransition**](https://msdn.microsoft.com/library/windows/apps/br210288) avec un décalage gauche de 100px.
-   Utilisez des en-têtes de section, des paragraphes et des étiquettes pour organiser et clarifier le contenu, si nécessaire.
-   Si vous avez besoin de répéter des paramètres, utilisez un niveau supplémentaire d’interface utilisateur ou un modèle développer/réduire, mais évitez les hiérarchies de plus de deux niveaux. Par exemple, une application météo qui fournit des paramètres par ville peut répertorier les villes et permettre à l’utilisateur d’appuyer sur la ville pour ouvrir un nouveau menu volant ou développer les options de paramètres.
-   Si le chargement des contrôles ou du contenu Web prend du temps, utilisez le contrôle de progression indéterminée pour indiquer aux utilisateurs que les informations sont en cours de chargement. Pour plus d’informations, consultez [Recommandations en matière de contrôles de progression](../controls-and-patterns/progress-controls.md).
-   N’utilisez pas de boutons pour la navigation ou pour valider des modifications. Utilisez des liens hypertexte pour accéder à d’autres pages et, au lieu d’utiliser un bouton pour valider les modifications, enregistrez automatiquement les modifications apportées aux paramètres d’application quand un utilisateur ferme le menu volant des paramètres.

\[Cet article contient des informations propres aux applications de plateforme Windows universelle (UWP) et à Windows10. Pour obtenir de l’aide concernant Windows 8.1, téléchargez le [document PDF de recommandations pour Windows 8.1](https://go.microsoft.com/fwlink/p/?linkid=258743) (en anglais).\]

## Articles connexes

* [Informations de base sur la conception des commandes](../layout/commanding-basics.md)
* [Recommandations en matière de contrôles de progression](../controls-and-patterns/progress-controls.md)
* [Stocker et récupérer des données d’application](store-and-retrieve-app-data.md)
* [**EntranceThemeTransition**](https://msdn.microsoft.com/library/windows/apps/br210288)




<!--HONumber=Aug16_HO3-->


