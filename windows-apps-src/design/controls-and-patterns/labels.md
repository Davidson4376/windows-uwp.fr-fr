---
author: Jwmsft
Description: Use a label to indicate to the user what they should enter into an adjacent control. You can also label a group of related controls, or display instructional text near a group of related controls.
title: Étiquettes
ms.assetid: CFACCCD4-749F-43FB-947E-2591AE673804
label: Labels
template: detail.hbs
ms.author: jimwalk
ms.date: 05/19/2017
ms.topic: article
keywords: windows10, uwp
pm-contact: miguelrb
design-contact: ksulliv
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: c17b2a539a01572bed984b86f72f5439896fac87
ms.sourcegitcommit: e38b334edb82bf2b1474ba686990f4299b8f59c7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/15/2018
ms.locfileid: "6847491"
---
# <a name="labels"></a>Étiquettes

 

Une étiquette correspond au nom ou au titre d’un contrôle ou d’un groupe de contrôles associés.

> **API importantes**: propriété Header, [classe TextBlock](https://msdn.microsoft.com/library/windows/apps/br209652)

En XAML, de nombreux contrôles disposent d’une propriété Header intégrée que vous utilisez pour afficher l’étiquette. Pour les contrôles dépourvus de propriété Header, ou pour étiqueter des groupes de contrôles, vous pouvez utiliser un élément [TextBlock](https://msdn.microsoft.com/library/windows/apps/br209652).

![capture d’écran du contrôle d’étiquette standard](images/label-standard.png)

## <a name="recommendations"></a>Recommandations


-   Utilisez une étiquette pour indiquer à l’utilisateur ce qu’il doit entrer dans un contrôle adjacent. Vous pouvez également étiqueter un groupe de contrôles associés ou afficher un texte d’instructions à côté de ce type de groupe.
-   Si vous étiquetez des contrôles, écrivez l’étiquette sous forme de nom ou de groupe nominal, et non sous la forme d’une phrase ou d’un texte d’instructions. Évitez d’utiliser le signe deux-points ou tout autre signe de ponctuation.
-   Lorsque vous insérez un texte d’instructions dans une étiquette, vous pouvez utiliser davantage de caractères, ainsi que des signes de ponctuation.


## <a name="get-the-sample-code"></a>Obtenir l’exemple de code
* [Exemple d’éléments de base d’une interface utilisateur XAML](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/XamlUIBasics)

## <a name="related-topics"></a>Rubriques connexes
* [Contrôles de texte](text-controls.md)
* [Propriété TextBox.Header](https://msdn.microsoft.com/library/windows/apps/dn252861)
* [Propriété PasswordBox.Header](https://msdn.microsoft.com/library/windows/apps/dn299051)
* [Propriété ToggleSwitch.Header](https://msdn.microsoft.com/library/windows/apps/br209713)
* [Propriété DatePicker.Header](https://msdn.microsoft.com/library/windows/apps/dn279460)
* [Propriété TimePicker.Header](https://msdn.microsoft.com/library/windows/apps/dn299286)
* [Propriété Slider.Header](https://msdn.microsoft.com/library/windows/apps/dn252829)
* [Propriété ComboBox.Header](https://msdn.microsoft.com/library/windows/apps/dn279416)
* [Propriété RichEditBox.Header](https://msdn.microsoft.com/library/windows/apps/dn252726)
* [Classe TextBlock](https://msdn.microsoft.com/library/windows/apps/br209652)

 

 




