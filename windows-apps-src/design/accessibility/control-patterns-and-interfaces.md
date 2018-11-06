---
author: Xansky
Description: Lists the Microsoft UI Automation control patterns, the classes that clients use to access them, and the interfaces providers use to implement them.
ms.assetid: 2091883C-5D0C-44ED-936A-709022926A42
title: Modèles de contrôle et interfaces
label: Control patterns and interfaces
template: detail.hbs
ms.author: mhopkins
ms.date: 02/08/2017
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: e48045e27e8ee7796f5dda2afb691a9f6e5371b2
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/06/2018
ms.locfileid: "6051228"
---
# <a name="control-patterns-and-interfaces"></a>Modèles de contrôle et interfaces  



Répertorie les modèles de contrôle Microsoft UI Automation, les classes que les clients utilisent pour y accéder, ainsi que les interfaces que les fournisseurs utilisent pour les implémenter.

Le tableau présenté dans cette rubrique décrit les modèles de contrôle Microsoft UI Automation. Par ailleurs, ce tableau recense les classes utilisées par les clients UI Automation pour accéder aux modèles de contrôle et aux interfaces utilisées par les fournisseurs UI Automation pour les implémenter. La colonne **Control pattern** indique le nom du modèle du point de vue du client UI Automation, sous la forme d’une valeur de constante répertoriée dans [**Control Pattern Availability Property Identifiers**](https://msdn.microsoft.com/library/windows/desktop/Ee671199). Du point de vue du fournisseur UI Automation, chacun de ces modèles est un nom de constante [**PatternInterface**](https://msdn.microsoft.com/library/windows/apps/BR242496). La colonne **Class provider interface** indique le nom de l’interface que les fournisseurs implémentent pour proposer ce modèle pour un contrôle XAML personnalisé.

Pour plus d’informations sur l’implémentation d’homologues d’automatisation personnalisés qui exposent des modèles de contrôle et implémentent les interfaces, voir [Homologues d’automatisation personnalisés](custom-automation-peers.md).

Quand vous implémentez un modèle de contrôle, vous devez aussi consulter la documentation sur le fournisseur UI Automation qui décrit certaines des attentes qu’auront les clients concernant un modèle de contrôle, indépendamment de l’infrastructure de l’interface utilisateur utilisée pour l’implémenter. Certaines des informations répertoriées dans la documentation générale du fournisseur UI Automation influenceront votre implémentation de vos homologues et la prise en charge correcte de ce modèle. Reportez-vous à la rubrique [Implémentation de modèles de contrôle UI Automation](https://msdn.microsoft.com/library/windows/desktop/Ee671292), et consultez la page qui documente le modèle que vous envisagez d’implémenter.

| Classe du modèle | Interface du fournisseur de classes | Description |
|-----------------|--------------------------|-------------|
| **Annotation** | [**IAnnotationProvider**](https://msdn.microsoft.com/library/windows/apps/Hh738493) | Utilisé pour exposer les propriétés d’une annotation dans un document. |
| **Dock** | [**IDockProvider**](https://msdn.microsoft.com/library/windows/apps/BR242565) | Utilisé pour les contrôles qui peuvent être ancrés dans un conteneur d’ancrage. Par exemple, les barres d’outils ou les palettes d’outils. |
| **Drag** | [**IDragProvider**](https://msdn.microsoft.com/library/windows/apps/Hh750322) | Utilisé pour prendre en charge les contrôles pouvant être glissés, ou les contrôles qui comportent des éléments pouvant être glissés. |
| **DropTarget** | [**IDropTargetProvider**](https://msdn.microsoft.com/library/windows/apps/Hh750327) | Utilisé pour prendre en charge les contrôles qui peuvent être la cible d’une opération glisser-déplacer. |
| **ExpandCollapse** | [**IExpandCollapseProvider**](https://msdn.microsoft.com/library/windows/apps/BR242568) | Utilisé pour prendre en charge les contrôles qui se développent visuellement pour afficher plus de contenu et qui se réduisent pour masquer du contenu. |
| **Grille** | [**IGridProvider**](https://msdn.microsoft.com/library/windows/apps/BR242578) | Utilisé pour les contrôles qui prennent en charge les fonctionnalités de grille telles que le dimensionnement et le déplacement vers une cellule spécifiée. La grille proprement dite n’implémente pas ce modèle, car bien qu’elle fournisse la disposition, il ne s’agit pas d’un contrôle. |
| **GridItem** | [**IGridItemProvider**](https://msdn.microsoft.com/library/windows/apps/BR242572) | Utilisé pour les contrôles dont les grilles contiennent des cellules. |
| **Invoke** | [**IInvokeProvider**](https://msdn.microsoft.com/library/windows/apps/BR242582) | Utilisé pour les contrôles qui peuvent être appelés, tels que [**Button**](https://msdn.microsoft.com/library/windows/apps/BR209265). |
| **ItemContainer** | [**IItemContainerProvider**](https://msdn.microsoft.com/library/windows/apps/BR242583) | Permet aux applications de rechercher un élément dans un conteneur, tel qu’une liste virtualisée. |
| **MultipleView** | [**IMultipleViewProvider**](https://msdn.microsoft.com/library/windows/apps/BR242585) | Utilisé pour les contrôles qui peuvent basculer entre plusieurs représentations du même jeu d’informations, de données ou d’enfants. |
| **ObjectModel** | [**IObjectModelProvider**](https://msdn.microsoft.com/library/windows/apps/Dn251815) | Utilisé pour exposer un pointeur à un modèle objet sous-jacent d’un document. |
| **RangeValue** | [**IRangeValueProvider**](https://msdn.microsoft.com/library/windows/apps/BR242590) | Utilisé pour les contrôles disposant d’une plage de valeurs qui peut s’appliquer au contrôle. Par exemple, un contrôle Spinner contenant des années peut avoir une plage s’échelonnant de 1900 à l’année en cours, pendant qu’un autre contrôle Spinner représentant les mois aura une plage allant de 1 à 12. |
| **Scroll** | [**IScrollProvider**](https://msdn.microsoft.com/library/windows/apps/BR242601) | Utilisé pour les contrôles qui peuvent défiler. Tel est le cas, par exemple, pour un contrôle qui dispose de barres de défilement actives lorsque la quantité d’informations est trop importante pour être affichée dans la zone affichable du contrôle. |
| **ScrollItem** | [**IScrollItemProvider**](https://msdn.microsoft.com/library/windows/apps/BR242599) | Utilisé pour les contrôles qui disposent d’éléments individuels dans une liste déroulante. Par exemple, un contrôle de liste qui dispose d’éléments individuels dans la liste modifiable, comme un contrôle zone de liste modifiable. |
| **Selection** | [**ISelectionProvider**](https://msdn.microsoft.com/library/windows/apps/BR242616) | Utilisé pour les contrôles conteneur de sélection. Par exemple, [**ListBox**](https://msdn.microsoft.com/library/windows/apps/BR242868) et [**ComboBox**](https://msdn.microsoft.com/library/windows/apps/BR209348). |
| **SelectionItem** | [**ISelectionItemProvider**](https://msdn.microsoft.com/library/windows/apps/BR242610) | Utilisé pour les éléments individuels dans les contrôles conteneur de sélection, tels que les zones de liste et zones de liste modifiable. |
| **Spreadsheet** | [**ISpreadsheetProvider**](https://msdn.microsoft.com/library/windows/apps/Dn251821) | Utilisé pour exposer le contenu d’une feuille de calcul ou d’un autre document de type grille. |
| **SpreadsheetItem** | [**ISpreadsheetItemProvider**](https://msdn.microsoft.com/library/windows/apps/Dn251817) | Utilisé pour exposer les propriétés d’une cellule se trouvant dans une feuille de calcul ou un autre document de type grille. |
| **Styles** | [**IStylesProvider**](https://msdn.microsoft.com/library/windows/apps/Dn251823) | Utilisé pour décrire un élément d’interface utilisateur ayant un style, une couleur de remplissage, un motif de remplissage ou une forme spécifiques. |
| **SynchronizedInput** | [**ISynchronizedInputProvider**](https://msdn.microsoft.com/library/windows/apps/Dn279198) | Permet à des applications clientes UI Automation de diriger l’entrée de souris ou de clavier vers un élément spécifique de l’interface utilisateur. |
| **Table** | [**ITableProvider**](https://msdn.microsoft.com/library/windows/apps/BR242623) | Utilisé pour les contrôles qui disposent d’une grille et d’informations d’en-tête. Par exemple, un contrôle de calendrier tabulaire. |
| **TableItem** | [**ITableItemProvider**](https://msdn.microsoft.com/library/windows/apps/BR242620) | Utilisé pour les éléments d’un tableau. |
| **Text** | [**ITextProvider**](https://msdn.microsoft.com/library/windows/apps/BR242627) | Utilisé pour les contrôles d’édition et les documents qui exposent des informations textuelles. Voir aussi [**ITextRangeProvider**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.provider.itextrangeprovider) et [**ITextProvider2**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.provider.itextprovider2). |
| **TextChild** | [**ITextChildProvider**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.provider.itextchildprovider) | Utilisé pour accéder à l’ancêtre le plus proche d’un élément qui prend en charge le modèle de contrôle **Text**. |
| **TextEdit** | Aucune classe managée disponible | Fournit l’accès à un contrôle qui modifie du texte, par exemple un contrôle qui effectue une correction automatique ou permet une composition d’entrée via un éditeur de méthode d’entrée (IME). |
| **TextRange** | [**ITextRangeProvider**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.provider.itextrangeprovider) | Fournit l’accès à une étendue de texte continu au sein d’un conteneur de texte qui implémente [**ITextProvider**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.provider.itextprovider). Voir aussi [**ITextRangeProvider2**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.provider.itextrangeprovider2). |
| **Toggle** | [**IToggleProvider**](https://msdn.microsoft.com/library/windows/apps/BR242653) | Utilisé pour les contrôles dans lesquels il est possible de faire basculer l’état. Par exemple, [**CheckBox**](https://msdn.microsoft.com/library/windows/apps/BR209316) et les éléments de menu qui peuvent être activés. |
| **Transform** | [**ITransformProvider**](https://msdn.microsoft.com/library/windows/apps/BR242656) | Utilisé pour les contrôles qui peuvent être redimensionnés, déplacés et pivotés. Les principales utilisations du modèle de contrôle Transform se font dans les applications de conception et de dessin, les formulaires et les éditeurs graphiques. |
| **Value** | [**IValueProvider**](https://msdn.microsoft.com/library/windows/apps/BR242663) | Permet aux clients d’obtenir ou de définir une valeur sur des contrôles qui ne prennent pas en charge une plage de valeurs. |
| **VirtualizedItem** | [**IVirtualizedItemProvider**](https://msdn.microsoft.com/library/windows/apps/BR242668) | Expose les éléments figurant à l’intérieur de conteneurs qui sont virtualisés et doivent être entièrement accessibles en tant qu’éléments UI Automation. |
| **Window** | [**IWindowProvider**](https://msdn.microsoft.com/library/windows/apps/BR242670) | Expose des informations spécifiques aux fenêtres, concept fondamental du système d’exploitation Microsoft Windows. Les fenêtres et les boîtes de dialogue enfants sont des exemples de contrôles correspondant à des fenêtres. |

> [!NOTE]
> Vous ne trouverez pas nécessairement des implémentations de tous ces modèles dans les contrôles XAML existants. Certains de ces modèles ont des interfaces uniquement pour prendre en charge la parité avec la définition de l’infrastructure UIAutomation générale des modèles, ainsi que pour prendre en charge des scénarios d’homologues d’automation qui nécessiteront une implémentation purement personnalisée pour prendre en charge ce modèle.

> [!NOTE]
> Les applications du WindowsPhoneStore ne prennent pas en charge tous les modèles de contrôle UIAutomation répertoriés ici. **Annotation**, **Dock**, **Drag**, **DropTarget** et **ObjectModel** font partie des modèles non pris en charge.

<span id="related_topics"/>

## <a name="related-topics"></a>Rubriques connexes  
* [Homologues d’automatisation personnalisés](custom-automation-peers.md)
* [Accessibilité](accessibility.md) 
