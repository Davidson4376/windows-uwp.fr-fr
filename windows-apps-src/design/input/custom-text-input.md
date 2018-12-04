---
Description: The core text APIs in the Windows.UI.Text.Core namespace enable a Universal Windows Platform (UWP) app to receive text input from any text service supported on Windows devices.
title: Vue d’ensemble de la saisie de texte personnalisé
ms.assetid: 58F5F7AC-6A4B-45FC-8C2A-942730FD7B74
label: Custom text input
template: detail.hbs
keywords: clavier, texte, Core Text, texte personnalisé, Text Services Framework, entrées, interactions utilisateur
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 161278dc5fe0bb8c7d4c790def6a9f7ba88b83d2
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/03/2018
ms.locfileid: "8480995"
---
# <a name="custom-text-input"></a>Saisie de texte personnalisé



Les API Core Text de l’espace de noms [**Windows.UI.Text.Core**](https://msdn.microsoft.com/library/windows/apps/dn958238) activent une application sur la plateforme Windows universelle (UWP) pour recevoir une entrée de texte à partir de n’importe quel service de texte pris en charge sur les appareils Windows. Ces API sont similaires aux API [Structure des services de texte](https://msdn.microsoft.com/library/windows/desktop/ms629032), dans la mesure où l’application n’a pas besoin de connaissances détaillées des services de texte. Cela permet à l’application de recevoir du texte dans n’importe quelle langue et à partir de n’importe quel type d’entrée, comme la saisie sur clavier, la saisie vocale ou la saisie à l’aide d’un stylet.

> **API importantes**: [**Windows.UI.Text.Core**](https://msdn.microsoft.com/library/windows/apps/dn958238), [**CoreTextEditContext**](https://msdn.microsoft.com/library/windows/apps/dn958158)

## <a name="why-use-core-text-apis"></a>Pourquoi utiliser les API Core Text?


Pour de nombreuses applications, les contrôles de zone de texte XAML ou HTML sont suffisants pour la saisie et l’édition de texte. Toutefois, si votre application gère les scénarios de texte complexes, comme une application de traitement de texte, vous aurez peut-être besoin d’un contrôle d’édition de texte personnalisé. Vous pouvez utiliser les API de clavier [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) pour créer votre système de contrôle d’édition de texte, mais elles ne permettent pas de recevoir du texte basé sur la composition, qui est requis pour prendre en charge des langues d’Asie orientale.

Si vous avez besoin de créer un système de contrôle d’édition de texte, utilisez plutôt les API [**Windows.UI.Text.Core**](https://msdn.microsoft.com/library/windows/apps/dn958238). Ces API sont conçues pour vous offrir une grande souplesse dans le traitement de la saisie de texte, dans n’importe quelle langue. Elles vous permettent également de bénéficier de l’expérience de texte la plus adaptée à votre application. Les contrôles de saisie et d’édition de texte conçus avec les API Core Text peuvent recevoir du texte à partir de toutes les méthodes de saisie de texte qui existent sur les appareils Windows, des éditeurs de méthode d’entrée (IME) basés sur la [structure des services de texte](https://msdn.microsoft.com/library/windows/desktop/ms629032) et de l’écriture manuscrite sur PC au clavier WordFlow (qui offre des fonctionnalités de correction automatique, de prédiction et de dictée) sur les appareils mobiles.

## <a name="architecture"></a>Architecture


Voici une représentation simple du système de saisie de texte.

-   L’application représente une application UWP hébergeant un contrôle d’édition personnalisé généré à l’aide des API Core Text.
-   Les API [**Windows.UI.Text.Core**](https://msdn.microsoft.com/library/windows/apps/dn958238) facilitent la communication avec les services de texte via Windows. La communication entre le contrôle d’édition de texte et les services de texte est gérée principalement via un objet [**CoreTextEditContext**](https://msdn.microsoft.com/library/windows/apps/dn958158) qui fournit les méthodes et événements visant à faciliter la communication.

![diagramme de l’architecture core text](images/coretext/architecture.png)

## <a name="text-ranges-and-selection"></a>Sélection et plages de texte


Les contrôles d’édition fournissent un espace pour la saisie de texte et les utilisateurs peuvent modifier du texte n’importe où dans cet espace. Cet article vise à présenter le système de positionnement de texte utilisé par l’API Core Text, ainsi que la représentation des plages et des sélections dans ce système.

### <a name="application-caret-position"></a>Position d’insertion de l’application

Les plages de texte utilisées avec les API Core Text sont exprimées en termes de position d’insertion. La «position d’insertion de l’application (ACP)» est un nombre (basé sur zéro) indiquant le nombre de caractères à partir du début du texte, juste avant le point d’insertion, comme indiqué ici.

![exemple de diagramme de flux de texte](images/coretext/stream-1.png)
### <a name="text-ranges-and-selection"></a>Sélection et plages de texte

Les plages de texte et les sélections sont représentées par la structure [**CoreTextRange**](https://msdn.microsoft.com/library/windows/apps/dn958201), qui comporte deux champs :

| Champ                  | Type de données                                                                 | Description                                                                      |
|------------------------|---------------------------------------------------------------------------|----------------------------------------------------------------------------------|
| **StartCaretPosition** | **Number** \[JavaScript\] | **System.Int32** \[.NET\] | **int32** \[C++\] | La position de départ d’une plage correspond à l’ACP située juste avant le premier caractère. |
| **EndCaretPosition**   | **Number** \[JavaScript\] | **System.Int32** \[.NET\] | **int32** \[C++\] | La position de fin d’une plage correspond à l’ACP située juste après le dernier caractère.     |

 

Par exemple, dans la plage de texte présentée précédemment, la plage \[0, 5\] correspond au mot «Hello». **StartCaretPosition** doit toujours être inférieur ou égal à **EndCaretPosition**. La plage \[5, 0\] n’est pas valide.

### <a name="insertion-point"></a>Point d’insertion

La position d’insertion actuelle, souvent appelée «point d’insertion», est représentée par la définition d’un champ **StartCaretPosition** égal au champ **EndCaretPosition**.

### <a name="noncontiguous-selection"></a>Sélection non contiguë

Certains contrôles d’édition prennent en charge les sélections non contiguës. Par exemple, les applications Microsoft Office prennent en charge les sélections arbitraires multiples, et de nombreux éditeurs de code source permettent la sélection de colonnes. Toutefois, les API Core Text n’acceptent pas les sélections non contiguës. Les contrôles d’édition doivent uniquement signaler une sélection contiguë, qui correspond le plus souvent à la sous-plage active des sélections non contiguës.

Prenons l’exemple du flux de texte suivant:

![exemple de diagramme de flux de texte](images/coretext/stream-2.png) Il existe deux sélections: \[0, 1\] et \[6, 11\]. Le contrôle d’édition doit signaler seulement l’une d’elles: soit \[0, 1\], soit \[6, 11\].

## <a name="working-with-text"></a>Utilisation du texte


La classe [**CoreTextEditContext**](https://msdn.microsoft.com/library/windows/apps/dn958158) permet un flux de texte entre Windows et les contrôles d’édition via l’événement [**TextUpdating**](https://msdn.microsoft.com/library/windows/apps/dn958176), l’événement [**TextRequested**](https://msdn.microsoft.com/library/windows/apps/dn958175) et la méthode [**NotifyTextChanged**](https://msdn.microsoft.com/library/windows/apps/dn958172).

Votre système de contrôle d’édition reçoit le texte via les événements [**TextUpdating**](https://msdn.microsoft.com/library/windows/apps/dn958176) qui sont générés lorsque les utilisateurs utilisent les méthodes de saisie de texte telles que les claviers, la saisie vocale ou les éditeurs IME.

Lorsque vous modifiez le texte dans votre système de contrôle d’édition, par exemple en collant du texte dans le système de contrôle, vous devez le signaler à Windows en appelant [**NotifyTextChanged**](https://msdn.microsoft.com/library/windows/apps/dn958172).

Si le service de texte a besoin du nouveau texte, un événement [**TextRequested**](https://msdn.microsoft.com/library/windows/apps/dn958175) est déclenché. Vous devez indiquer le nouveau texte dans le gestionnaire d’événements **TextRequested**.

### <a name="accepting-text-updates"></a>Accepter des mises à jour de texte

Votre système de contrôle d’édition accepte généralement les demandes de mises à jour de texte, dans la mesure où elles contiennent le texte que l’utilisateur souhaite saisir. Dans le gestionnaire d’événements [**TextUpdating**](https://msdn.microsoft.com/library/windows/apps/dn958176), votre système de contrôle d’édition est censé effectuer les actions suivantes :

1.  Insérer le texte spécifié dans [**CoreTextTextUpdatingEventArgs.Text**](https://msdn.microsoft.com/library/windows/apps/dn958236) à la position indiquée dans [**CoreTextTextUpdatingEventArgs.Range**](https://msdn.microsoft.com/library/windows/apps/dn958234).
2.  Placer la sélection à la position indiquée dans [**CoreTextTextUpdatingEventArgs.NewSelection**](https://msdn.microsoft.com/library/windows/apps/dn958233).
3.  Indiquer au système que la mise à jour a été correctement effectuée en définissant [**CoreTextTextUpdatingEventArgs.Result**](https://msdn.microsoft.com/library/windows/apps/dn958235) sur [**CoreTextTextUpdatingResult.Succeeded**](https://msdn.microsoft.com/library/windows/apps/dn958237).

Par exemple, voici l’état d’un contrôle d’édition avant que l’utilisateur tape «d». Le point d’insertion est à \[10, 10\].

![exemple de diagramme de flux de texte](images/coretext/stream-3.png) Lorsque l’utilisateur tape «d», un événement [**TextUpdating**](https://msdn.microsoft.com/library/windows/apps/dn958176) est déclenché avec les données [**CoreTextTextUpdatingEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn958229) suivantes:

-   [**Range**](https://msdn.microsoft.com/library/windows/apps/dn958234) = \[10, 10\]
-   [**Text**](https://msdn.microsoft.com/library/windows/apps/dn958236) = « d »
-   [**NewSelection**](https://msdn.microsoft.com/library/windows/apps/dn958233) = \[11, 11\]

Dans votre système de contrôle d’édition, appliquez les modifications indiquées et définissez [**Result**](https://msdn.microsoft.com/library/windows/apps/dn958235) sur **Succeeded**. Voici l’état du contrôle une fois que les modifications sont appliquées.

![exemple de diagramme de flux de texte](images/coretext/stream-4.png)
### <a name="rejecting-text-updates"></a>Refuser des mises à jour de texte

Il vous est parfois impossible d’appliquer les mises à jour de texte, car la plage concernée est une zone du contrôle d’édition qui ne doit pas être modifiée. Dans ce cas, vous ne devez pas appliquer les modifications. Au lieu de cela, indiquez au système que la mise à jour a échoué en définissant [**CoreTextTextUpdatingEventArgs.Result**](https://msdn.microsoft.com/library/windows/apps/dn958235) sur [**CoreTextTextUpdatingResult.Failed**](https://msdn.microsoft.com/library/windows/apps/dn958237).

Par exemple, imaginons un système de contrôle d’édition qui accepte uniquement une adresse de messagerie. Les espaces doivent être rejetés, car les adresses de messagerie ne peuvent pas contenir d’espaces. De ce fait, quand des événements [**TextUpdating**](https://msdn.microsoft.com/library/windows/apps/dn958176) sont déclenchés pour la touche Espace, il vous suffit de définir [**Result**](https://msdn.microsoft.com/library/windows/apps/dn958235) sur **Failed** dans votre système de contrôle d’édition.

### <a name="notifying-text-changes"></a>Signaler des modifications de texte

Parfois, votre système de contrôle d’édition apporte des modifications au texte lorsque le texte est collé ou corrigé automatiquement. Dans ces cas, vous devez signaler ces modifications aux services de texte en appelant la méthode [**NotifyTextChanged**](https://msdn.microsoft.com/library/windows/apps/dn958172).

Par exemple, voici l’état d’un contrôle d’édition avant que l’utilisateur colle le mot «World». Le point d’insertion est à \[6, 6\].

![exemple de diagramme de flux de texte](images/coretext/stream-5.png) L’utilisateur exécute l’action Coller et le système de contrôle d’édition se retrouve avec le texte suivant:

![exemple de diagramme de flux de texte](images/coretext/stream-4.png) Lorsque cela se produit, vous devez appeler [**NotifyTextChanged**](https://msdn.microsoft.com/library/windows/apps/dn958172) avec ces arguments:

-   *modifiedRange* = \[6, 6\]
-   *newLength* = 5
-   *newSelection* = \[11, 11\]

Un ou plusieurs événements [**TextRequested**](https://msdn.microsoft.com/library/windows/apps/dn958175) se déclenchent. Vous devez les gérer pour mettre à jour le texte que les services de texte utilisent.

### <a name="overriding-text-updates"></a>Ignorer des mises à jour de texte

Dans votre système de contrôle d’édition, vous souhaiterez peut-être ignorer une mise à jour de texte pour utiliser des fonctionnalités de correction automatique.

Par exemple, imaginons un système de contrôle d’édition qui fournit une fonctionnalité de correction qui formalise les contractions. Voici l’état du contrôle d’édition avant que l’utilisateur appuie sur la touche Espace pour déclencher la correction. Le point d’insertion est à \[3, 3\].

![exemple de diagramme de flux de texte](images/coretext/stream-6.png) L’utilisateur appuie sur la touche Espace et un événement [**TextUpdating**](https://msdn.microsoft.com/library/windows/apps/dn958176) est déclenché. Le système de contrôle d’édition accepte la mise à jour de texte. Voici l’état que le contrôle d’édition affiche pendant un court instant avant la fin de la correction. Le point d’insertion est à \[4, 4\].

![exemple de diagramme de flux de texte](images/coretext/stream-7.png) En dehors du gestionnaire d’événements [**TextUpdating**](https://msdn.microsoft.com/library/windows/apps/dn958176), le système de contrôle d’édition effectue la correction suivante. Voici l’état du contrôle d’édition après la fin de la correction. Le point d’insertion est à \[5, 5\].

![exemple de diagramme de flux de texte](images/coretext/stream-8.png) Lorsque cela se produit, vous devez appeler [**NotifyTextChanged**](https://msdn.microsoft.com/library/windows/apps/dn958172) avec ces arguments:

-   *modifiedRange* = \[1, 2\]
-   *newLength* = 2
-   *newSelection* = \[5, 5\]

Un ou plusieurs événements [**TextRequested**](https://msdn.microsoft.com/library/windows/apps/dn958175) se déclenchent. Vous devez les gérer pour mettre à jour le texte que les services de texte utilisent.

### <a name="providing-requested-text"></a>Fournir le texte demandé

Les services de texte doivent disposer du texte approprié pour proposer des fonctionnalités comme la correction automatique ou la prédiction, en particulier si le texte existait déjà dans le système de contrôle d’édition, par exemple parce qu’il avait été créé lors du chargement d’un document, ou parce qu’il avait été inséré par le système de contrôle d’édition comme expliqué dans les sections précédentes. Par conséquent, chaque fois qu’un événement [**TextRequested**](https://msdn.microsoft.com/library/windows/apps/dn958175) est déclenché, vous devez fournir le texte qui se trouve actuellement dans votre système de contrôle d’édition pour la plage spécifiée.

Il peut arriver que le champ [**Range**](https://msdn.microsoft.com/library/windows/apps/dn958227) dans [**CoreTextTextRequest**](https://msdn.microsoft.com/library/windows/apps/dn958221) indique une plage que votre système de contrôle d’édition ne peut pas prendre en charge telle quelle. C’est par exemple le cas si le **Range** est supérieur à la taille du contrôle d’édition au moment de l’événement [**TextRequested**](https://msdn.microsoft.com/library/windows/apps/dn958175) ou si la fin du **Range** est hors limites. Dans ces cas, vous devez indiquer la plage adéquate, qui correspond généralement à un sous-ensemble de la plage requise.

## <a name="related-articles"></a>Articles connexes

**Exemples**
* [Exemple de contrôle d’édition personnalisé](https://go.microsoft.com/fwlink/?linkid=831024) 
 **Exemples d’archive**
* [Exemple de modification de texte XAML](http://go.microsoft.com/fwlink/p/?LinkID=251417)


