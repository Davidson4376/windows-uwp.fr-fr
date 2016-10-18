---
author: QuinnRadich
Description: "Ces recommandations décrivent comment concevoir un contenu d’aide efficace pour votre application."
title: "Recommandations en matière d’aide de l’application"
label: Guidelines for app help
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: 0aa2db498ab7ec25839da259dd0026b0a7cd2b13
ms.openlocfilehash: 0ca099b0acfcba16d62c6158c7a829adaf475891

---

# Recommandations en matière d’aide de l’application

\[ Mise à jour pour des applications UniversalWindowsPlatform (UWP) sur Windows10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

Du fait de la complexité de certaines applications, la fourniture d’une aide efficace sur ces dernières peut améliorer considérablement l’expérience des utilisateurs. L’inclusion d’informations d’aide n’est pas nécessaire pour toutes les applications, et le type d’aide à fournir peut varier sensiblement selon les applications et selon l’usage.

Si vous décidez de proposer une aide, consultez les directives décrites ici. Gardez à l’esprit que des informations d’aide inutiles peuvent se révéler plus perturbantes qu’une aide inexistante.

## <span id="intuitive_design"></span><span id="INTUITIVE_DESIGN"></span>Conception intuitive

Quelle que soit l’utilité de votre contenu d’aide, elle ne garantit pas à elle seule une expérience de qualité dans votre application. Si un utilisateur ne parvient pas à découvrir et exploiter immédiatement les fonctions essentielles de votre application, il n’utilisera pas cette dernière. Aucun contenu d’aide ne pourra changer cette première impression.

Une conception intuitive et conviviale constitue donc la première étape de la création d’une aide efficace. Elle permet non seulement de maintenir l’intérêt de l’utilisateur suffisamment longtemps pour inciter ce dernier à utiliser des fonctionnalités plus élaborées, mais elle lui offre également une solide connaissance de base des fonctions clés de l’application sur laquelle l’utilisateur pourra s’appuyer.

## <span id="general_instructions"></span><span id="GENERAL_INSTRUCTIONS"></span>Instructions d’ordre général

Un utilisateur ne recherchera des informations d’aide que s’il a rencontré un problème. Votre contenu d’aide doit donc lui fournir une réponse rapide et efficace à ce problème. Si les informations d’aide ne sont pas immédiatement utiles ou se révèlent trop complexes, les utilisateurs seront plus susceptibles de les ignorer.

Quel que soit le type de votre contenu d’aide, il doit présenter les caractéristiques de base suivantes:

-   **Intelligibilité:** une aide incompréhensible se révèle plus perturbante qu’une aide inexistante.

-   **Simplicité:** les utilisateurs qui cherchent des informations d’aide veulent obtenir des réponses claires et directes.

-   **Pertinence:** les utilisateurs ne veulent pas avoir besoin de rechercher leur problème spécifique. Ils souhaitent d’abord obtenir l’aide la plus pertinente (appelée «aide contextuelle») ou une interface facile à parcourir.

-   **Immédiateté:** lorsqu’un utilisateur recherche de l’aide, il veut la voir s’afficher instantanément. Si votre application comporte des pages permettant de signaler des bogues, de formuler des commentaires, de visualiser les Conditions de service ou d’autres fonctions similaires, elles doivent être présentes sous forme de liens ou de notes de bas de page sur la page d’aide principale, et non sous forme d’éléments d’importance.

-   **Cohérence:** quel que soit le type de votre contenu d’aide, ce dernier fait partie intégrante de votre application et doit donc être traité comme n’importe quelle autre partie de l’interface utilisateur. L’aide que vous proposez est soumise aux mêmes principes de facilité d’utilisation, d’accessibilité et de présentation graphique que le reste de votre application.

## <span id="types_of_help"></span><span id="TYPES_OF_HELP"></span>Types d’aide

Il existe troisgrandes catégories de contenu d’aide, chacune présentant des avantages variés et convenant à différents usages. Vous pouvez combiner ces différents types d’aide dans votre application selon vos besoins.

### <span id="instructional_ui"></span><span id="INSTRUCTIONAL_UI"></span>Interface utilisateur d’instruction

Dans l’idéal, les utilisateurs doivent pouvoir tirer parti de toutes les fonctions essentielles de votre application sans aucune instruction. Toutefois, il peut arriver que votre application dépende d’une action spécifique ou que des fonctionnalités secondaires de votre application ne soient pas immédiatement évidentes. Dans ces cas, utilisez une interface utilisateur d’instructions pour former les utilisateurs aux procédures d’exécution de tâches spécifiques.

[Pour plus d’informations, voir les recommandations en matière de conception d’une interface utilisateur d’instructions.](instructional-ui.md)

### <span id="in_app_help"></span><span id="IN_APP_HELP"></span>Aide dans l’application

La méthode standard de présentation de l’aide consiste à afficher cette dernière dans l’application à la demande de l’utilisateur. Vous pouvez implémenter ce type d’aide de plusieurs façons, par exemple dans des pages d’aide ou des descriptions informatives. Cette méthode est parfaitement adaptée aux contenus d’aide à usage général qui répondent directement et simplement aux questions d’un utilisateur.

[Pour plus d’informations, voir les recommandations en matière de conception d’aide dans l’application.](in-app-help.md)

### <span id="external_help"></span><span id="EXTERNAL_HELP"></span>Aide externe

Pour permettre aux utilisateurs d’accéder à des didacticiels détaillés, à des fonctions avancées ou à des bibliothèques de rubriques d’aide trop volumineuses pour tenir dans votre application, l’inclusion de liens vers des pages web externes constitue la solution idéale. Dans la mesure du possible, ces liens doivent être utilisés avec parcimonie, car ils font sortir l’utilisateur de l’application.

[Pour plus d’informations, voir les recommandations en matière de conception d’aide externe.](external-help.md)

\[Cet article contient des informations propres aux applications de plateforme Windows universelle (UWP) et à Windows10. Pour obtenir de l’aide concernant Windows8.1, téléchargez le [document PDF de recommandations pour Windows8.1](https://go.microsoft.com/fwlink/p/?linkid=258743) (en anglais).\]



<!--HONumber=Aug16_HO3-->


