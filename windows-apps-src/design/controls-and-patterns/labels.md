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
ms.openlocfilehash: 5996fb15c0d7302c7360c2e45613f0da2720d415
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66364651"
---
# <a name="labels"></a>Étiquettes

 

Une étiquette correspond au nom ou au titre d’un contrôle ou d’un groupe de contrôles associés.

> **API importantes** : Propriété d’en-tête [classe TextBlock](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock)

En XAML, de nombreux contrôles disposent d’une propriété Header intégrée que vous utilisez pour afficher l’étiquette. Pour les contrôles dépourvus de propriété Header, ou pour étiqueter des groupes de contrôles, vous pouvez utiliser un élément [TextBlock](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock).

![capture d’écran du contrôle d’étiquette standard](images/label-standard.png)

## <a name="recommendations"></a>Recommandations


-   Utilisez une étiquette pour indiquer à l’utilisateur ce qu’il doit entrer dans un contrôle adjacent. Vous pouvez également étiqueter un groupe de contrôles associés ou afficher un texte d’instructions à côté de ce type de groupe.
-   Si vous étiquetez des contrôles, écrivez l’étiquette sous forme de nom ou de groupe nominal, et non sous la forme d’une phrase ou d’un texte d’instructions. Évitez d’utiliser le signe deux-points ou tout autre signe de ponctuation.
-   Lorsque vous insérez un texte d’instructions dans une étiquette, vous pouvez utiliser davantage de caractères, ainsi que des signes de ponctuation.


## <a name="get-the-sample-code"></a>Obtenir l’exemple de code
* [XAML UI principes de base, exemple](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/XamlUIBasics)

## <a name="related-topics"></a>Rubriques connexes
* [Contrôles de texte](text-controls.md)
* [Propriété de TextBox.Header](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.header)
* [Propriété de PasswordBox.Header](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.passwordbox.header)
* [Propriété de ToggleSwitch.Header](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.toggleswitch.header)
* [Propriété de DatePicker.Header](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.datepicker.header)
* [Propriété de TimePicker.Header](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.timepicker.header)
* [Propriété de Slider.Header](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.slider.header)
* [Propriété de ComboBox.Header](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.combobox.header)
* [Propriété de RichEditBox.Header](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.richeditbox.header)
* [Classe de TextBlock](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock)

 

 




