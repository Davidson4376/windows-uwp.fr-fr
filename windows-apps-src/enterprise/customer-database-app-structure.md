---
title: Structure de base de données d’application client
description: Passez en revue la structure du didacticiel de base de données client, et pourquoi il a construit la façon dont il est.
keywords: entreprise, didacticiel, clients, données, crud
ms.date: 05/07/2018
ms.topic: article
ms.localizationpriority: med
ms.openlocfilehash: b1f8f8c8a2fd1522d8c304a45514d5257543f222
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57656614"
---
# <a name="customer-database-app-structure"></a>Structure de base de données d’application client

Complexes line of business applications ont souvent nombre de pages et les fonctionnalités et un grand nombre de lignes de code. Pour cette raison, il est vital que vous concevez votre application autour d’une structure prévisible. Il existe plusieurs modèles de conception d’applications qui sont appropriées pour les applications d’entreprise, mais ils sont générés dans le but de créer une application à grande échelle plus facile à comprendre et utiliser.

Bien que le [didacticiel de base de données client](customer-database-tutorial.md) présente une application à page unique par souci de simplicité, il implémente le modèle de conception d’application Model-View-ViewModel (MVVM) afin de tirer parti de ces idées en action. Comme son nom l’indique, le modèle de conception MVVM sépare la logique d’application principale en trois catégories :

* Les modèles sont les classes qui contiennent des données de l’application.
* Les vues sont l’interface utilisateur de n’importe quelle page donnée.
* ViewModels fournissent la logique d’application. Cela peut inclure des actions de l’utilisateur à partir de la vue de gestion et/ou la gestion des interactions avec les modèles.

Bien que cette application n’est pas un exemple parfait et architypical de MVVM, il montre les principes fondamentaux de la séparation des préoccupations en action. [Extraire l’application ici.](https://github.com/Microsoft/windows-tutorials-customer-database)

## <a name="application-structure"></a>Structure de l’application

Une fois que vous ouvrez l’application, commencez par examiner le **l’Explorateur de solutions.** Partie de ce que vous voyez il doit être familier si vous avez travaillé avec une application UWP avant, mais vous verrez également une collection de dossiers qui contiennent un composants de l’application.

![Application de point de départ dans l’Explorateur de solutions](images/customer-database-tutorial/solution-explorer.png)

### <a name="views"></a>Vues

Interface utilisateur de tous les l’application est définie dans le dossier Views. Étant donné que notre didacticiel est maintenant une application à page unique, cela ne signifie qu’un seul affichage - **CustomerListPage**. Il a XAML UI balisage et code-behind de xaml.cs - ces deux fichiers constituent une seule vue. Vous allez ajouter des éléments d’interface utilisateur à **CustomerListPage.xaml**.

> [!NOTE]
> Vous pouvez remarquer que cette application n’a pas une MainPage. C’est parce que nous spécifions dans **App.xaml.cs** que l’application doit lancer **CustomerListPage** lors du lancement.

### <a name="viewmodels"></a>ViewModels

Bien que cette application n’a qu’une seule vue, est a deux ViewModels. Pourquoi est-ce ?

**CustomerListPageViewModel.cs** est un ViewModel standard dans le modèle MVVM. Il est où se trouve la logique fondamentale de la page de l’application, et la page vous travaillerez avec le meilleur parti dans le didacticiel. Chaque action d’interface utilisateur prise par l’utilisateur est passée à travers la vue à ce ViewModel pour traitement.

**CustomerViewModel.cs**, toutefois, n’est pas associé à n’importe quelle vue spécifique. Au lieu de cela, il associe un concept de programmation (les propriétés qui ont été modifiées) avec les données contenues dans modèle d’un client.

### <a name="models"></a>Modèles

Cette application contient trois modèles, qui stockent les données de l’application et fournissent des interfaces pour interagir avec le référentiel. Bien que ce sont les parties critiques de l’application, ils ne sont pas quelque chose que vous allez modifier directement dans le didacticiel.

La plus importante est **Customer.cs**, qui décrit la structure de données de client que vous allez utiliser dans le didacticiel.

> [!NOTE]
> Le didacticiel ignore le *E-mail* et *téléphone* propriétés de l’objet client. Si vous souhaitez aller au-delà de ce qui est présenté ici, l’ajout de ces deux propriétés dans l’interface utilisateur de l’application est la première étape.

### <a name="repository"></a>local

Le dossier de référentiel contient des classes qui construisent et d’interagissent avec la base de données SQLite locale. Pour ce didacticiel, la base de données SQLite est présentée sous-est. Bien que vous allez ajouter dans le code pour **CustomerListPageViewModel.cs** pour appeler les méthodes définies par ces classes, vous ne devrez pas apporter des modifications afin de les configurer.

Pour plus d’informations sur SQLite dans UWP, [consultez cet article](../data-access/sqlite-databases.md).

Si vous essayez de la section « Pour aller plus loin » du didacticiel, voici où vous allez créer une classe pour vous connecter à la base de données reste à distance. Il implémente également la **ICustomerRepository** interface définie dans la section modèles, mais il aura un aspect très différent que son homologue de SQLite.

### <a name="other-elements"></a>Autres éléments

Comme d’habitude pour les applications UWP, le comportement de lancement d’application est défini dans le **App.xaml.cs** classe. La plupart du code ici est le code par défaut pour n’importe quelle application UWP. Mais nous avons déjà apporté quelques petites modifications :

* Nous avons spécifié que l’application doit afficher **CustomerListPage** lors du lancement.
* Nous avons créé un objet de référentiel, qui contiendra la source de données que nous utilisons.
* Nous avons ajouté un **SQLiteDatabase** (méthode), qui initialise la base de données local et le définit comme le référentiel spécifié.

Si vous essayez de la section « Pour aller plus loin », vous allez ajouter une méthode similaire pour initialiser un objet de référentiel de REST. Étant donné que nous avez séparés nos préoccupations sont à l’aide de la même interface définie pour les opérations de SQLite et REST, ce sera le seul code existant, que vous devez modifier pour utiliser REST au lieu de SQLite dans votre application.

## <a name="next-steps"></a>Étapes suivantes

Si vous avez déjà effectué le didacticiel, vous pouvez consulter le [exemple complet d’application](https://github.com/Microsoft/Windows-appsample-customers-orders-database) pour voir comment ces fonctionnalités sont implémentées sur une plus grande échelle.

Sinon, maintenant que vous savez Pourquoi tout est là, vous devez [retour au didacticiel](customer-database-tutorial.md) et fonctionnent avec la structure que nous venons d’examiner.