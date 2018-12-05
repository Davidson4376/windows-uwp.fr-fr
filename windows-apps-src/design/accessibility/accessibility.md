---
Description: Introduces accessibility concepts that relate to Universal Windows Platform (UWP) apps.
ms.assetid: C89D79C2-B830-493D-B020-F3FF8EB5FFDD
title: Accessibilité
label: Accessibility
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 2f9cdfb8a20e273d5d9e5819fc1e28aba97e4296
ms.sourcegitcommit: c01c29cd97f1cbf050950526e18e15823b6a12a0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/05/2018
ms.locfileid: "8692983"
---
# <a name="accessibility"></a>Accessibilité  



Présente les concepts d’accessibilité associés aux applications de plateforme Windows universelle (UWP).

L’accessibilité consiste en des expériences rendant votre application disponible aux utilisateurs qui utilisent les technologies au sein d’un large éventail d’environnements et valorisent votre interface utilisateur pour satisfaire des expériences et des besoins variés. Pour certaines situations, les exigences en matière d’accessibilité sont imposées par la loi. Il est toutefois préférable de gérer les aspects liés à l’accessibilité quelles que soient les exigences juridiques, afin que votre application ait l’audience la plus étendue possible. Il existe également une déclaration du MicrosoftStore concernant l’accessibilité de votre application.

> [!NOTE]
> Déclarer l’application comme accessible n’est pertinent que pour le MicrosoftStore.

| Article | Description |
|---------|-------------|
| [Vue d’ensemble de l’accessibilité](accessibility-overview.md) | Cet article est une vue d’ensemble des concepts et technologies associés aux scénarios d’accessibilité des applications UWP. |
| [Conception de logiciels inclusifs](designing-inclusive-software.md) | En savoir plus sur l’évolution de la conception inclusive avec les applications UWP pour Windows10.  Concevez et développez un logiciel inclusif en tenant compte de l’accessibilité. |
| [Développement d’applications Windows inclusives](developing-inclusive-windows-apps.md) | Cet article fait office de feuille de route pour développer des applications UWP accessibles. |
| [Test de l’accessibilité](accessibility-testing.md) | Procédures de test à appliquer pour s’assurer de l’accessibilité de votre application UWP. |
| [Accessibilité dans le WindowsStore](accessibility-in-the-store.md) | Décrit la configuration requise pour la déclaration de votre application UWP comme étant accessible dans le Microsoft Store. |
| [Liste de vérification de l’accessibilité](accessibility-checklist.md) | Fournit une liste de vérification pour vous aider à garantir que votre applicationUWP est accessible. |
| [Présenter des informations d’accessibilité élémentaires](basic-accessibility-information.md) | Les informations d’accessibilité élémentaires sont souvent classées en trois catégories : nom, rôle et valeur. Cette rubrique décrit le code qui aide votre application à exposer les informations élémentaires nécessaires aux technologies d’assistance. |
| [Accessibilité du clavier](keyboard-accessibility.md) | Si votre application ne fournit pas un bon accès par le clavier, les non-voyants ou les utilisateurs ayant des problèmes de mobilité peuvent rencontrer des difficultés à utiliser votre application ou risquent de ne pas pouvoir l’utiliser du tout. |
| [Repères et en-têtes](landmarks-and-headings.md) | Les repères et les en-têtes définissent les sections d’une interface utilisateur qui optimisent la navigation pour les utilisateurs de technologies d’assistance telles que les lecteurs d’écran. |
| [Thèmes à contraste élevé](high-contrast-themes.md) | Décrit les étapes nécessaires pour s’assurer que votre application UWP est utilisable quand un thème à contraste élevé est actif. |
| [Exigences de texte accessible](accessible-text-requirements.md) | Cette rubrique décrit les meilleures pratiques relatives à l’accessibilité du texte dans une application, en garantissant que les couleurs et de l’arrière-plan respectent le coefficient de contraste nécessaire. Elle traite également des rôles MicrosoftUIAutomation que peuvent avoir les éléments de texte dans une applicationUWP et des meilleures pratiques relatives au texte des graphiques. |
| [Pratiques d’accessibilité à éviter](practices-to-avoid.md) | Répertorie les pratiques à éviter si vous voulez créer une applicationUWP accessible. |
| [Homologues d’automation personnalisés](custom-automation-peers.md) | Décrit le concept des homologues d’automatisation pour UIAutomation, et la manière dont vous pouvez fournir une prise en charge de l’automatisation pour votre propre classe d’interface utilisateur personnalisée. |
| [Modèles de contrôle et interfaces](control-patterns-and-interfaces.md) | Répertorie les modèles de contrôle MicrosoftUIAutomation, les classes que les clients utilisent pour y accéder, ainsi que les interfaces que les fournisseurs utilisent pour les implémenter. |

## <a name="related-topics"></a>Rubriques connexes  
* [**Windows.UI.Xaml.Automation**](https://msdn.microsoft.com/library/windows/apps/BR209179) 
* [Prise en main du Narrateur](https://support.microsoft.com/en-us/help/22798/windows-10-narrator-get-started)