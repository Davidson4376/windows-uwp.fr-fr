---
Description: Utilisez une étiquette pour indiquer à l’utilisateur ce qu’il doit entrer dans un contrôle adjacent. Vous pouvez également étiqueter un groupe de contrôles associés ou afficher un texte d’instructions à côté de ce type de groupe.
title: Étiquettes
ms.assetid: CFACCCD4-749F-43FB-947E-2591AE673804
label: Labels
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: miguelrb
design-contact: ksulliv
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 4345daf5b879fed7ba9805e4a448c473299031d7
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57654144"
---
# <a name="labels"></a>Étiquettes

 

Une étiquette correspond au nom ou au titre d’un contrôle ou d’un groupe de contrôles associés.

> **API importantes** : Propriété d’en-tête [classe TextBlock](https://msdn.microsoft.com/library/windows/apps/br209652)

En XAML, de nombreux contrôles disposent d’une propriété Header intégrée que vous utilisez pour afficher l’étiquette. Pour les contrôles dépourvus de propriété Header, ou pour étiqueter des groupes de contrôles, vous pouvez utiliser un élément [TextBlock](https://msdn.microsoft.com/library/windows/apps/br209652).

![capture d’écran du contrôle d’étiquette standard](images/label-standard.png)

## <a name="recommendations"></a>Recommandations


-   Utilisez une étiquette pour indiquer à l’utilisateur ce qu’il doit entrer dans un contrôle adjacent. Vous pouvez également étiqueter un groupe de contrôles associés ou afficher un texte d’instructions à côté de ce type de groupe.
-   Si vous étiquetez des contrôles, écrivez l’étiquette sous forme de nom ou de groupe nominal, et non sous la forme d’une phrase ou d’un texte d’instructions. Évitez d’utiliser le signe deux-points ou tout autre signe de ponctuation.
-   Lorsque vous insérez un texte d’instructions dans une étiquette, vous pouvez utiliser davantage de caractères, ainsi que des signes de ponctuation.


## <a name="get-the-sample-code"></a>Obtenir l’exemple de code
* [XAML UI principes de base, exemple](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/XamlUIBasics)

## <a name="related-topics"></a>Rubriques connexes
* [Contrôles de texte](text-controls.md)
* [Propriété de TextBox.Header](https://msdn.microsoft.com/library/windows/apps/dn252861)
* [Propriété de PasswordBox.Header](https://msdn.microsoft.com/library/windows/apps/dn299051)
* [Propriété de ToggleSwitch.Header](https://msdn.microsoft.com/library/windows/apps/br209713)
* [Propriété de DatePicker.Header](https://msdn.microsoft.com/library/windows/apps/dn279460)
* [Propriété de TimePicker.Header](https://msdn.microsoft.com/library/windows/apps/dn299286)
* [Propriété de Slider.Header](https://msdn.microsoft.com/library/windows/apps/dn252829)
* [Propriété de ComboBox.Header](https://msdn.microsoft.com/library/windows/apps/dn279416)
* [Propriété de RichEditBox.Header](https://msdn.microsoft.com/library/windows/apps/dn252726)
* [Classe de TextBlock](https://msdn.microsoft.com/library/windows/apps/br209652)

 

 




