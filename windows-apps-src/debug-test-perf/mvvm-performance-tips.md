---
ms.assetid: 159681E4-BF9E-4A57-9FEE-EC7ED0BEFFAD
title: Conseils relatifs aux performances du langage de programmation et du modèleMVVM
description: Cette rubrique décrit certaines considérations en matière de performances liées à vos choix de modèles de conception de logiciel et de langage de programmation.
ms.date: 02/08/2017
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 9027362eccfb8130b181bee26a57f13ce1e1af66
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/03/2018
ms.locfileid: "8481467"
---
# <a name="mvvm-and-language-performance-tips"></a>Conseils relatifs aux performances du langage de programmation et du modèle MVVM


Cette rubrique décrit certaines considérations en matière de performances liées à vos choix de modèles de conception de logiciel et de langage de programmation.

## <a name="the-model-view-viewmodel-mvvm-pattern"></a>Modèle MVVM (Model-View-ViewModel)

Le modèle MVVM est courant dans de nombreuses applications XAML. (Le modèle MVVM est très similaire à la description de Fowler du modèle Model-View-Presenter, mais il est adapté au langage XAML.) Le problème du modèle MVVM est qu’il peut, par inadvertance, entraîner un trop grand nombre de couches et d’allocations dans les applications. Les points motivant l’adoption du modèle MVVM sont les suivants.

-   **Séparation des problèmes**. Il est toujours utile de diviser un problème en éléments plus petits, et un modèle tel que MVVM ou MVC permet de diviser une application (voire un contrôle unique) en plus petites parties : l’affichage réel, un modèle logique de l’affichage (modèle d’affichage) et la logique d’application indépendante de l’affichage (modèle). Il s’agit en particulier d’un flux de travail courant pour que les concepteurs possèdent l’affichage à l’aide d’un seul outil, les développeurs possèdent le modèle en utilisant un autre outil, et les intégrateurs de conception possèdent le modèle d’affichage à l’aide de ces deux outils.
-   **Test unitaire**. Vous pouvez procéder au test unitaire du modèle d’affichage (et par conséquent du modèle) indépendamment de l’affichage, sans vous appuyer sur la création des fenêtres, les saisies, etc. En conservant un affichage petit, vous pouvez tester une grande partie de votre application sans avoir à créer de fenêtre.
-   **Agilité des modifications apportées à l’expérience utilisateur**. L’affichage a tendance à montrer les modifications les plus fréquentes et les plus récentes, car l’expérience utilisateur est ajustée en fonction des commentaires des utilisateurs finaux. En maintenant l’affichage distinct, ces modifications peuvent être adaptées plus rapidement et avec moins de perturbation à l’application.

Il existe plusieurs définitions concrètes du modèle MVVM, et des infrastructures tierces qui permettent de l’implémenter. Cependant un strict respect d’une variation du modèle peut entraîner une surcharge bien supérieure à celle qui peut être justifiée dans les applications.

-   La liaison de données XAML (extension de balisage {Binding}) a été conçue en partie pour activer des modèles d’affichage/de modèle. Néanmoins, {Binding} est accompagnée d’une plage de travail non triviale et d’une surcharge de processeur. La création d’une {Binding} entraîne une série d’allocations, et la mise à jour d’une cible de liaison peut provoquer boxing et réflexion. Ces problèmes sont traités avec l’extension de balisage {x:Bind}, qui compile les liaisons au moment de la génération. **Recommandation :** utilisez {x: Bind}.
-   Dans MVVM, il est courant de connecter Button.Click au modèle d’affichage à l’aide d'une ICommand, par exemple les applications auxiliaires DelegateCommand ou RelayCommand courantes. Ces commandes sont des allocations supplémentaires, qui incluent cependant le détecteur d’événements CanExecuteChanged, s’ajoutent à la plage de travail et au temps de démarrage/navigation de la page. **Recommandation :** comme alternative à l’utilisation de l’interface ICommand pratique, pensez à placer des gestionnaires d’événements dans votre code-behind, à les attacher aux événements d’affichage et à appeler une commande dans votre modèle d’affichage lorsque ces événements sont déclenchés. Vous devez également ajouter du code supplémentaire pour désactiver le bouton lorsque la commande n’est pas disponible.
-   Dans le modèle MVVM, il est courant de créer une Page avec toutes les configurations possibles de l’interface utilisateur, puis de réduire les parties de l’arborescence en liant la propriété Visibility aux propriétés de l’ordinateur virtuel. Cela s’ajoute inutilement au temps de démarrage et éventuellement à la plage de travail (car certaines parties de l’arborescence peuvent ne jamais devenir visibles). **Recommandations:** Utilisez la fonctionnalité [x:Load attribute](../xaml-platform/x-load-attribute.md) ou [x:DeferLoadStrategy attribute](../xaml-platform/x-deferloadstrategy-attribute.md) pour différer les parties superflues de l’arborescence hors du démarrage. Vous pouvez également créer des contrôles utilisateur pour les différents modes de la page et utiliser code-behind pour ne conserver que les contrôles nécessaires chargés.

## <a name="ccx-recommendations"></a>Recommandations liées à C++/CX

-   **Utilisez la dernière version**. Des améliorations sont apportées en continu aux performances du compilateur C++/CX. Vérifiez que votre application est générée à l’aide du dernier ensemble d’outils.
-   **Désactivez l’option RTTI (/GR-)**. L’option RTTI est activée par défaut dans le compilateur. À moins que votre environnement de génération ne l’ait désactivée, vous l’utilisez donc probablement. La surcharge de l’option RTTI est importante, et vous devez donc désactiver cette option à moins que votre code n’ait une dépendance approfondie avec elle. L’infrastructure XAML n’impose nullement que votre code utilise l’option RTTI.
-   **Évitez une utilisation intensive des ppltasks**. Les ppltasks sont très pratiques lors de l’appel des API WinRT asynchrones, mais elles sont fournies avec une surcharge de taille de code importante. L’équipe chargée de C++/CX travaille sur une fonctionnalité de langage qui fournit de bien meilleures performances. D’ici là, veillez à équilibrer l’utilisation des ppltasks dans les chemins réactifs de votre code.
-   **Évitez d’utiliser C++/CX dans la « logique métier » de votre application**. C++/CX est conçu pour permettre d’accéder facilement aux API WinRT des applications en C++. Il utilise des wrappers qui génèrent une surcharge. Vous devez éviter d’utiliser C++/CX dans la logique métier/le modèle de votre classe, et le réserver pour une utilisation aux frontières entre votre code et WinRT.