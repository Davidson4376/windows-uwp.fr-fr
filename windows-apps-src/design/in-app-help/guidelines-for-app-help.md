---
author: QuinnRadich
Description: These guidelines describe how to design effective Help content for your app.
title: Recommandations en matière d’aide de l’application
label: Guidelines for app help
template: detail.hbs
ms.author: quradic
ms.date: 02/08/2017
ms.topic: article
keywords: windows10, uwp
ms.assetid: c3e73f9b-4839-4804-b379-c95b0ca4fbe8
ms.localizationpriority: medium
ms.openlocfilehash: 6e4cb60526fda9495249cd310ad434878941a97d
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/30/2018
ms.locfileid: "5820772"
---
# <a name="guidelines-for-app-help"></a>Recommandations en matière d’aide de l’application



Du fait de la complexité de certaines applications, la fourniture d’une aide efficace sur ces dernières peut améliorer considérablement l’expérience des utilisateurs. L’inclusion d’informations d’aide n’est pas nécessaire pour toutes les applications, et le type d’aide à fournir peut varier sensiblement selon les applications.

Si vous décidez de proposer de l’aide, suivez les recommandations de création ci-après. Des informations d’aide inutiles peuvent se révéler plus perturbantes qu’une aide inexistante.

## <a name="intuitive-design"></a>Conception intuitive

Quelle que soit l’utilité de votre contenu d’aide, elle ne garantit pas à elle seule une expérience de qualité dans votre application. Si l’utilisateur ne parvient pas à découvrir et exploiter immédiatement les fonctions essentielles de votre application, il n’utilisera pas cette dernière. Aucun contenu d’aide ne modifiera cette première impression, aussi complet ou instructif soit-il.

Une conception intuitive et conviviale constitue donc la première étape de la création d’une aide efficace. Elle permet non seulement de maintenir l’intérêt de l’utilisateur suffisamment longtemps pour inciter ce dernier à utiliser des fonctionnalités plus élaborées, mais elle lui offre également une solide connaissance de base des fonctions clés de l’application sur laquelle l’utilisateur pourra s’appuyer pour poursuivre et améliorer son utilisation de l’application.

## <a name="general-instructions"></a>Instructions d’ordre général

Un utilisateur ne recherchera des informations d’aide que s’il a rencontré un problème. Votre contenu d’aide doit donc lui fournir une réponse rapide et efficace à ce problème. Si les informations d’aide ne sont pas immédiatement utiles ou se révèlent trop complexes, les utilisateurs seront plus susceptibles de les ignorer.

Quel que soit le type de votre contenu d’aide, il doit présenter les caractéristiques suivantes:

-   **Intelligibilité:** une aide incompréhensible se révèle plus perturbante qu’une aide inexistante.

-   **Simplicité:** les utilisateurs qui cherchent des informations d’aide veulent obtenir des réponses claires et directes.

-   **Pertinence:** les utilisateurs ne veulent pas avoir besoin de rechercher leur problème spécifique. Ils souhaitent obtenir directement l’aide la plus pertinente (appelée «aide contextuelle») ou une interface facile à parcourir.

-   **Immédiateté:** lorsqu’un utilisateur recherche de l’aide, il veut la voir s’afficher instantanément. Si votre application comporte des pages permettant des signaler des bogues, de formuler des commentaires, de visualiser les Conditions de service ou d’autres fonctions similaires, vous pouvez inclure des liens vers ces pages dans votre contenu d’aide. Toutefois, ces informations doivent être proposées en tant que contenu secondaire sur la page d’aide principale, et non sous la forme d’éléments plus importants ou de même niveau.

-   **Cohérence:** quel que soit le type de votre contenu d’aide, ce dernier fait partie intégrante de votre application et doit donc être traité comme n’importe quelle autre partie de l’interface utilisateur. L’aide que vous proposez est soumise aux mêmes principes de facilité d’utilisation, d’accessibilité et de présentation graphique que le reste de votre application.

## <a name="types-of-help"></a>Types d’aide

Il existe troisgrandes catégories de contenu d’aide, chacune présentant des avantages variés et convenant à différents usages. Vous pouvez combiner ces différents types d’aide dans votre application selon vos besoins.

#### <a name="instructional-ui"></a>Interface utilisateur d’instructions

En règle générale, les utilisateurs doivent pouvoir tirer parti de toutes les fonctions essentielles de votre application sans aucune instruction. Toutefois, votre application peut dépendre de l’utilisation d’un mouvement spécifique ou comporter des fonctionnalités secondaires subtiles. Dans ce cas, utilisez une interface utilisateur d’instructions pour former les utilisateurs aux procédures d’exécution de tâches spécifiques.

[Pour plus d’informations, voir les recommandations en matière de conception d’une interface utilisateur d’instructions.](instructional-ui.md)

#### <a name="in-app-help"></a>Aide dans l’application

La méthode standard de présentation de l’aide consiste à afficher cette dernière dans l’application à la demande de l’utilisateur. Vous pouvez implémenter ce type d’aide de plusieurs façons, par exemple dans des pages d’aide ou des descriptions informatives. Cette méthode est parfaitement adaptée aux contenus d’aide à usage général qui répondent directement et simplement aux questions d’un utilisateur.

[Pour plus d’informations, voir les recommandations en matière de conception d’aide dans l’application.](in-app-help.md)

#### <a name="external-help"></a>Aide externe

Pour permettre aux utilisateurs d’accéder à des didacticiels détaillés, à des fonctions avancées ou à des bibliothèques de rubriques d’aide trop volumineuses pour tenir dans votre application, l’inclusion de liens vers des pages web externes constitue la solution idéale. Dans la mesure du possible, ces liens doivent être utilisés avec parcimonie, car ils font sortir l’utilisateur de l’application.

[Pour plus d’informations, voir les recommandations en matière de conception d’aide externe.](external-help.md)


