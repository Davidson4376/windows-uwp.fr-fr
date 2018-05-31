---
author: mijacobs
Description: This article lists and provides usage guidance for the glyphs that come with the Segoe MDL2 Assets font.
Search.Refinement.TopicID: 184
title: Recommandations en matière d’icônes Segoe MDL2
ms.assetid: DFB215C2-8A61-4957-B662-3B1991AC9BE1
label: Segoe MDL2 icons
template: detail.hbs
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 3da419c3193a3aa4481033d8489643f2daf40eba
ms.sourcegitcommit: 91511d2d1dc8ab74b566aaeab3ef2139e7ed4945
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/30/2018
ms.locfileid: "1817640"
---
# <a name="segoe-mdl2-icons"></a>Icônes Segoe MDL2

 

Cet article répertorie les icônes fournies par la police Segoe MDL2 Assets. 

> **API importantes**: [**énumération Symbol**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.symbol), [**classe FontIcon**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.fonticon)

## <a name="about-segoe-mdl2-assets"></a>À propos de la police Segoe MDL2 Assets

Depuis la publication de Windows10, la police Segoe MDL2 Assets a remplacé la police d’icône Segoe UI Symbol Windows8/8.1. <!-- It can be used in much the same manner as the older font, but many glyphs have been redrawn in the Windows 10 icon style with the font’s metrics set so that icons are aligned within the font’s em-square instead of on a typographic baseline. --> (La police **Segoe UI Symbol** reste disponible comme ressource «héritée», mais nous vous recommandons de mettre à jour votre application de façon à utiliser la nouvelle police **Segoe MDL2 Assets**).

La plupart des icônes et des contrôles d’interface utilisateur inclus dans la police **Segoe MDL2 Assets** sont mappés à la zone d’utilisation privée d’Unicode. La zone d’utilisation privée permet aux développeurs de polices d’affecter des valeurs Unicode privées à des glyphes qui ne correspondent pas à des points de code existants. Cette opération peut s’avérer utile lors de la création d’une police de symboles, mais elle génère un problème d’interopérabilité. Si la police n’est pas disponible, les glyphes n’apparaissent pas. Utilisez ces glyphes uniquement lorsque vous pouvez spécifier la police **Segoe MDL2 Assets**.

Utilisez ces glyphes uniquement si vous pouvez spécifier explicitement la police **Segoe MDL2 Assets**. Si vous utilisez des vignettes, vous ne pouvez pas utiliser ces glyphes car vous ne pouvez pas spécifier la police des vignettes et les glyphes de la zone d’utilisation privée ne sont pas disponibles via font-fallback.

À la différence de la police **Segoe UI Symbol**, les icônes de la police **Segoe MDL2 Assets** ne sont pas conçues pour être alignées sur du texte. Cela signifie que certaines anciennes «astuces» comme les flèches de divulgation progressive ne s’appliquent plus. De même, dans la mesure où toutes les nouvelles icônes sont dimensionnées et positionnées de la même manière, elles ne peuvent pas avoir une chasse nulle. Nous avons simplement veillé à ce qu’elles fonctionnent en tant que jeu. Théoriquement, vous pouvez superposer deux icônes conçues en tant que jeu. Elles se fondent. Nous pouvons faire cela pour permettre une colorisation dans le code. Par exemple, les icônes U+EA3A et U+EA3B ont été créées pour le badge de la vignette de démarrage. Comme elles sont déjà centrées, le remplissage du cercle peut changer de couleur en fonction de l’état.

## <a name="layering-and-mirroring"></a>Superposition et mise en miroir

Tous les glyphes de la police **Segoe MDL2 Assets** ayant la même à largeur fixe avec une hauteur constante et un point d’origine à gauche, des effets de superposition et de colorisation peuvent être obtenus en dessinant des glyphes directement l’un par-dessus l’autre. Cet exemple montre un contour noir dessiné sur le cœur rouge en largeur nulle.

![Utilisation d’un glyphe en largeur nulle](images/segoe-ui-symbol-layering.png)

De nombreuses icônes offrent également des formes en miroir utilisables dans des langues qui s’écrivent de droite à gauche, comme l’arabe, l’hébreu et le persan.

## <a name="using-the-icons"></a>Utilisation des icônes
Si vous développez une application en C#/VB/C++ et XAML, vous pouvez utiliser les glyphes spécifiés de la police Segoe MDL2 Assets avec l’[énumération Symbol](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.symbol). 

```xaml
<SymbolIcon Symbol="GlobalNavigationButton"/>
```

Si vous souhaitez utiliser un glyphe de la police **Segoe MDL2 Assets** qui n’est pas inclus dans l’énumération Symbol, utilisez un objet [**FontIcon**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.fonticon).

```xaml
<FontIcon FontFamily="Segoe MDL2 Assets" Glyph="&#xE700;"/>
```

## <a name="how-do-i-get-this-font"></a>Comment obtenir cette police?
Pour obtenir la police Segoe MDL2 Assets, vous devez installer Windows10. 

## <a name="icon-list"></a>Liste des icônes
Gardez à l’esprit que la police **Segoe MDL2 Assets** comprend de nombreuses autres icônes que nous pouvons afficher ici. La plupart des icônes sont destinées à un usage spécifique et ne sont généralement pas utilisées ailleurs.


<table style="background-color: white; color: black">

 <tr>
  <td>Symbole</td>
  <td>Point de code Unicode</td>
  <td>Description</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e700.png" alt="GlobalNavButton" /></td>
  <td>E700</td>
  <td>GlobalNavigationButton</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e701.png" alt="Wifi" /></td>
  <td>E701</td>
  <td>Wifi</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e702.png" alt="Bluetooth" /></td>
  <td>E702</td>
  <td>Bluetooth</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e703.png" alt="Connect" /></td>
  <td>E703</td>
  <td>Connect</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e704.png" alt="InternetSharing" /></td>
  <td>E704</td>
  <td>InternetSharing</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e705.png" alt="VPN" /></td>
  <td>E705</td>
  <td>VPN</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e706.png" alt="Brightness" /></td>
  <td>E706</td>
  <td>Brightness</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e707.png" alt="MapPin" /></td>
  <td>E707</td>
  <td>MapPin</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e708.png" alt="QuietHours" /></td>
  <td>E708</td>
  <td>QuietHours</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e709.png" alt="Airplane" /></td>
  <td>E709</td>
  <td>Airplane</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e70a.png" alt="Tablet" /></td>
  <td>E70A</td>
  <td>Tablet</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e70b.png" alt="QuickNote" /></td>
  <td>E70B</td>
  <td>QuickNote</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e70c.png" alt="RememberedDevice" /></td>
  <td>E70C</td>
  <td>RememberedDevice</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e70d.png" alt="ChevronDown" /></td>
  <td>E70D</td>
  <td>ChevronDown</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e70e.png" alt="ChevronUp" /></td>
  <td>E70E</td>
  <td>ChevronUp</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e70f.png" alt="Edit" /></td>
  <td>E70F</td>
  <td>Edit</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e710.png" alt="Add" /></td>
  <td>E710</td>
  <td>Add</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e711.png" alt="Cancel" /></td>
  <td>E711</td>
  <td>Cancel</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e712.png" alt="More" /></td>
  <td>E712</td>
  <td>More</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e713.png" alt="Settings" /></td>
  <td>E713</td>
  <td>Settings</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e714.png" alt="Video" /></td>
  <td>E714</td>
  <td>Video</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e715.png" alt="Mail" /></td>
  <td>E715</td>
  <td>Mail</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e716.png" alt="People" /></td>
  <td>E716</td>
  <td>People</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e717.png" alt="Phone" /></td>
  <td>E717</td>
  <td>Phone</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e718.png" alt="Pin" /></td>
  <td>E718</td>
  <td>Pin</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e719.png" alt="Shop" /></td>
  <td>E719</td>
  <td>Shop</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e71a.png" alt="Stop" /></td>
  <td>E71A</td>
  <td>Stop</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e71b.png" alt="Link" /></td>
  <td>E71B</td>
  <td>Link</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e71c.png" alt="Filter" /></td>
  <td>E71C</td>
  <td>Filter</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e71d.png" alt="AllApps" /></td>
  <td>E71D</td>
  <td>AllApps</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e71e.png" alt="Zoom" /></td>
  <td>E71E</td>
  <td>Zoom</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e71f.png" alt="ZoomOut" /></td>
  <td>E71F</td>
  <td>ZoomOut</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e720.png" alt="Microphone" /></td>
  <td>E720</td>
  <td>Microphone</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e721.png" alt="Search" /></td>
  <td>E721</td>
  <td>Search</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e722.png" alt="Camera" /></td>
  <td>E722</td>
  <td>Camera</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e723.png" alt="Attach" /></td>
  <td>E723</td>
  <td>Attach</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e724.png" alt="Send" /></td>
  <td>E724</td>
  <td>Send</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e725.png" alt="SendFill" /></td>
  <td>E725</td>
  <td>SendFill</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e726.png" alt="WalkSolid" /></td>
  <td>E726</td>
  <td>WalkSolid</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e727.png" alt="InPrivate" /></td>
  <td>E727</td>
  <td>InPrivate</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e728.png" alt="FavoriteList" /></td>
  <td>E728</td>
  <td>FavoriteList</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e729.png" alt="PageSolid" /></td>
  <td>E729</td>
  <td>PageSolid</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e72a.png" alt="Forward" /></td>
  <td>E72A</td>
  <td>Forward</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e72b.png" alt="Back" /></td>
  <td>E72B</td>
  <td>Back</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e72c.png" alt="Refresh" /></td>
  <td>E72C</td>
  <td>Refresh</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e72d.png" alt="Share" /></td>
  <td>E72D</td>
  <td>Share</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e72e.png" alt="Lock" /></td>
  <td>E72E</td>
  <td>Lock</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e730.png" alt="ReportHacked" /></td>
  <td>E730</td>
  <td>ReportHacked</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e734.png" alt="FavoriteStar" /></td>
  <td>E734</td>
  <td>FavoriteStar</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e735.png" alt="FavoriteStarFill" /></td>
  <td>E735</td>
  <td>FavoriteStarFill</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e738.png" alt="Remove" /></td>
  <td>E738</td>
  <td>Remove</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e739.png" alt="Checkbox" /></td>
  <td>E739</td>
  <td>Checkbox</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e73a.png" alt="CheckboxComposite" /></td>
  <td>E73A</td>
  <td>CheckboxComposite</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e73b.png" alt="CheckboxFill" /></td>
  <td>E73B</td>
  <td>CheckboxFill</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e73c.png" alt="CheckboxIndeterminate" /></td>
  <td>E73C</td>
  <td>CheckboxIndeterminate</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e73d.png" alt="CheckboxCompositeReversed" /></td>
  <td>E73D</td>
  <td>CheckboxCompositeReversed</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e73e.png" alt="CheckMark" /></td>
  <td>E73E</td>
  <td>CheckMark</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e73f.png" alt="BackToWindow" /></td>
  <td>E73F</td>
  <td>BackToWindow</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e740.png" alt="FullScreen" /></td>
  <td>E740</td>
  <td>FullScreen</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e741.png" alt="ResizeTouchLarger" /></td>
  <td>E741</td>
  <td>ResizeTouchLarger</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e742.png" alt="ResizeTouchSmaller" /></td>
  <td>E742</td>
  <td>ResizeTouchSmaller</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e743.png" alt="ResizeMouseSmall" /></td>
  <td>E743</td>
  <td>ResizeMouseSmall</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e744.png" alt="ResizeMouseMedium" /></td>
  <td>E744</td>
  <td>ResizeMouseMedium</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e745.png" alt="ResizeMouseWide" /></td>
  <td>E745</td>
  <td>ResizeMouseWide</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e746.png" alt="ResizeMouseTall" /></td>
  <td>E746</td>
  <td>ResizeMouseTall</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e747.png" alt="ResizeMouseLarge" /></td>
  <td>E747</td>
  <td>ResizeMouseLarge</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e748.png" alt="SwitchUser" /></td>
  <td>E748</td>
  <td>SwitchUser</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e749.png" alt="Print" /></td>
  <td>E749</td>
  <td>Print</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e74a.png" alt="Up" /></td>
  <td>E74A</td>
  <td>Up</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e74b.png" alt="Down" /></td>
  <td>E74B</td>
  <td>Down</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e74c.png" alt="OEM" /></td>
  <td>E74C</td>
  <td>OEM</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e74d.png" alt="Delete" /></td>
  <td>E74D</td>
  <td>Delete</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e74e.png" alt="Save" /></td>
  <td>E74E</td>
  <td>Save</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e74f.png" alt="Mute" /></td>
  <td>E74F</td>
  <td>Mute</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e750.png" alt="BackSpaceQWERTY" /></td>
  <td>E750</td>
  <td>BackSpaceQWERTY</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e751.png" alt="ReturnKey" /></td>
  <td>E751</td>
  <td>ReturnKey</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e752.png" alt="UpArrowShiftKey" /></td>
  <td>E752</td>
  <td>UpArrowShiftKey</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e753.png" alt="Cloud" /></td>
  <td>E753</td>
  <td>Cloud</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e754.png" alt="Flashlight" /></td>
  <td>E754</td>
  <td>Flashlight</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e755.png" alt="RotationLock" /></td>
  <td>E755</td>
  <td>RotationLock</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e756.png" alt="CommandPrompt" /></td>
  <td>E756</td>
  <td>CommandPrompt</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e759.png" alt="SIPMove" /></td>
  <td>E759</td>
  <td>SIPMove</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e75a.png" alt="SIPUndock" /></td>
  <td>E75A</td>
  <td>SIPUndock</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e75b.png" alt="SIPRedock" /></td>
  <td>E75B</td>
  <td>SIPRedock</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e75c.png" alt="EraseTool" /></td>
  <td>E75C</td>
  <td>EraseTool</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e75d.png" alt="UnderscoreSpace" /></td>
  <td>E75D</td>
  <td>UnderscoreSpace</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e75e.png" alt="GripperTool" /></td>
  <td>E75E</td>
  <td>GripperTool</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e75f.png" alt="Dialpad" /></td>
  <td>E75F</td>
  <td>Dialpad</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e760.png" alt="PageLeft" /></td>
  <td>E760</td>
  <td>PageLeft</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e761.png" alt="PageRight" /></td>
  <td>E761</td>
  <td>PageRight</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e762.png" alt="MultiSelect" /></td>
  <td>E762</td>
  <td>MultiSelect</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e763.png" alt="KeyboardLeftHanded" /></td>
  <td>E763</td>
  <td>KeyboardLeftHanded</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e764.png" alt="KeyboardRightHanded" /></td>
  <td>E764</td>
  <td>KeyboardRightHanded</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e765.png" alt="KeyboardClassic" /></td>
  <td>E765</td>
  <td>KeyboardClassic</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e766.png" alt="KeyboardSplit" /></td>
  <td>E766</td>
  <td>KeyboardSplit</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e767.png" alt="Volume" /></td>
  <td>E767</td>
  <td>Volume</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e768.png" alt="Play" /></td>
  <td>E768</td>
  <td>Play</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e769.png" alt="Pause" /></td>
  <td>E769</td>
  <td>Pause</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e76b.png" alt="ChevronLeft" /></td>
  <td>E76B</td>
  <td>ChevronLeft</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e76c.png" alt="ChevronRight" /></td>
  <td>E76C</td>
  <td>ChevronRight</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e76d.png" alt="InkingTool" /></td>
  <td>E76D</td>
  <td>InkingTool</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e76e.png" alt="Emoji2" /></td>
  <td>E76E</td>
  <td>Emoji2</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e76f.png" alt="GripperBarHorizontal" /></td>
  <td>E76F</td>
  <td>GripperBarHorizontal</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e770.png" alt="System" /></td>
  <td>E770</td>
  <td>System</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e771.png" alt="Personalize" /></td>
  <td>E771</td>
  <td>Personalize</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e772.png" alt="Devices" /></td>
  <td>E772</td>
  <td>Devices</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e773.png" alt="SearchAndApps" /></td>
  <td>E773</td>
  <td>SearchAndApps</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e774.png" alt="Globe" /></td>
  <td>E774</td>
  <td>Globe</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e775.png" alt="TimeLanguage" /></td>
  <td>E775</td>
  <td>TimeLanguage</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e776.png" alt="EaseOfAccess" /></td>
  <td>E776</td>
  <td>EaseOfAccess</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e777.png" alt="UpdateRestore" /></td>
  <td>E777</td>
  <td>UpdateRestore</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e778.png" alt="HangUp" /></td>
  <td>E778</td>
  <td>HangUp</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e779.png" alt="ContactInfo" /></td>
  <td>E779</td>
  <td>ContactInfo</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e77a.png" alt="Unpin" /></td>
  <td>E77A</td>
  <td>Unpin</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e77b.png" alt="Contact" /></td>
  <td>E77B</td>
  <td>Contact</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e77c.png" alt="Memo" /></td>
  <td>E77C</td>
  <td>Memo</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e77f.png" alt="Paste" /></td>
  <td>E77F</td>
  <td>Paste</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e780.png" alt="PhoneBook" /></td>
  <td>E780</td>
  <td>PhoneBook</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e781.png" alt="LEDLight" /></td>
  <td>E781</td>
  <td>LEDLight</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e783.png" alt="Error" /></td>
  <td>E783</td>
  <td>Error</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e784.png" alt="GripperBarVertical" /></td>
  <td>E784</td>
  <td>GripperBarVertical</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e785.png" alt="Unlock" /></td>
  <td>E785</td>
  <td>Unlock</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e786.png" alt="Slideshow" /></td>
  <td>E786</td>
  <td>Slideshow</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e787.png" alt="Calendar" /></td>
  <td>E787</td>
  <td>Calendar</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e788.png" alt="GripperResize" /></td>
  <td>E788</td>
  <td>GripperResize</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e789.png" alt="Megaphone" /></td>
  <td>E789</td>
  <td>Megaphone</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e78a.png" alt="Trim" /></td>
  <td>E78A</td>
  <td>Trim</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e78b.png" alt="NewWindow" /></td>
  <td>E78B</td>
  <td>NewWindow</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e78c.png" alt="SaveLocal" /></td>
  <td>E78C</td>
  <td>SaveLocal</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e790.png" alt="Color" /></td>
  <td>E790</td>
  <td>Color</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e791.png" alt="DataSense" /></td>
  <td>E791</td>
  <td>DataSense</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e792.png" alt="SaveAs" /></td>
  <td>E792</td>
  <td>SaveAs</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e793.png" alt="Light" /></td>
  <td>E793</td>
  <td>Light</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e799.png" alt="AspectRatio" /></td>
  <td>E799</td>
  <td>AspectRatio</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e7a5.png" alt="DataSenseBar" /></td>
  <td>E7A5</td>
  <td>DataSenseBar</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e7a6.png" alt="Redo" /></td>
  <td>E7A6</td>
  <td>Redo</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e7a7.png" alt="Undo" /></td>
  <td>E7A7</td>
  <td>Undo</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e7a8.png" alt="Crop" /></td>
  <td>E7A8</td>
  <td>Crop</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e7ac.png" alt="OpenWith" /></td>
  <td>E7AC</td>
  <td>OpenWith</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e7ad.png" alt="Rotate" /></td>
  <td>E7AD</td>
  <td>Rotate</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e7b5.png" alt="SetlockScreen" /></td>
  <td>E7B5</td>
  <td>SetlockScreen</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e7b7.png" alt="MapPin2" /></td>
  <td>E7B7</td>
  <td>MapPin2</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e7b8.png" alt="Package" /></td>
  <td>E7B8</td>
  <td>Package</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e7ba.png" alt="Warning" /></td>
  <td>E7BA</td>
  <td>Warning</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e7bc.png" alt="ReadingList" /></td>
  <td>E7BC</td>
  <td>ReadingList</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e7be.png" alt="Education" /></td>
  <td>E7BE</td>
  <td>Education</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e7bf.png" alt="ShoppingCart" /></td>
  <td>E7BF</td>
  <td>ShoppingCart</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e7c0.png" alt="Train" /></td>
  <td>E7C0</td>
  <td>Train</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e7c1.png" alt="Flag" /></td>
  <td>E7C1</td>
  <td>Flag</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e7c3.png" alt="Page" /></td>
  <td>E7C3</td>
  <td>Page</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e7c4.png" alt="Multitask" /></td>
  <td>E7C4</td>
  <td>Multitask</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e7c5.png" alt="BrowsePhotos" /></td>
  <td>E7C5</td>
  <td>BrowsePhotos</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e7c6.png" alt="HalfStarLeft" /></td>
  <td>E7C6</td>
  <td>HalfStarLeft</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e7c7.png" alt="HalfStarRight" /></td>
  <td>E7C7</td>
  <td>HalfStarRight</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e7c8.png" alt="Record" /></td>
  <td>E7C8</td>
  <td>Record</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e7c9.png" alt="TouchPointer" /></td>
  <td>E7C9</td>
  <td>TouchPointer</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e7de.png" alt="LangJPN" /></td>
  <td>E7DE</td>
  <td>LangJPN</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e7e3.png" alt="Ferry" /></td>
  <td>E7E3</td>
  <td>Ferry</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e7e6.png" alt="Highlight" /></td>
  <td>E7E6</td>
  <td>Highlight</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e7e7.png" alt="ActionCenterNotification" /></td>
  <td>E7E7</td>
  <td>ActionCenterNotification</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e7e8.png" alt="PowerButton" /></td>
  <td>E7E8</td>
  <td>PowerButton</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e7ea.png" alt="ResizeTouchNarrower" /></td>
  <td>E7EA</td>
  <td>ResizeTouchNarrower</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e7eb.png" alt="ResizeTouchShorter" /></td>
  <td>E7EB</td>
  <td>ResizeTouchShorter</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e7ec.png" alt="DrivingMode" /></td>
  <td>E7EC</td>
  <td>DrivingMode</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e7ed.png" alt="RingerSilent" /></td>
  <td>E7ED</td>
  <td>RingerSilent</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e7ee.png" alt="OtherUser" /></td>
  <td>E7EE</td>
  <td>OtherUser</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e7ef.png" alt="Admin" /></td>
  <td>E7EF</td>
  <td>Admin</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e7f0.png" alt="CC" /></td>
  <td>E7F0</td>
  <td>CC</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e7f1.png" alt="SDCard" /></td>
  <td>E7F1</td>
  <td>SDCard</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e7f2.png" alt="CallForwarding" /></td>
  <td>E7F2</td>
  <td>CallForwarding</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e7f3.png" alt="SettingsDisplaySound" /></td>
  <td>E7F3</td>
  <td>SettingsDisplaySound</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e7f4.png" alt="TVMonitor" /></td>
  <td>E7F4</td>
  <td>TVMonitor</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e7f5.png" alt="Speakers" /></td>
  <td>E7F5</td>
  <td>Speakers</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e7f6.png" alt="Headphone" /></td>
  <td>E7F6</td>
  <td>Headphone</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e7f7.png" alt="DeviceLaptopPic" /></td>
  <td>E7F7</td>
  <td>DeviceLaptopPic</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e7f8.png" alt="DeviceLaptopNoPic" /></td>
  <td>E7F8</td>
  <td>DeviceLaptopNoPic</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e7f9.png" alt="DeviceMonitorRightPic" /></td>
  <td>E7F9</td>
  <td>DeviceMonitorRightPic</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e7fa.png" alt="DeviceMonitorLeftPic" /></td>
  <td>E7FA</td>
  <td>DeviceMonitorLeftPic</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e7fb.png" alt="DeviceMonitorNoPic" /></td>
  <td>E7FB</td>
  <td>DeviceMonitorNoPic</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e7fc.png" alt="Game" /></td>
  <td>E7FC</td>
  <td>Game</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e7fd.png" alt="HorizontalTabKey" /></td>
  <td>E7FD</td>
  <td>HorizontalTabKey</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e802.png" alt="StreetsideSplitMinimize" /></td>
  <td>E802</td>
  <td>StreetsideSplitMinimize</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e803.png" alt="StreetsideSplitExpand" /></td>
  <td>E803</td>
  <td>StreetsideSplitExpand</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e804.png" alt="Car" /></td>
  <td>E804</td>
  <td>Car</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e805.png" alt="Walk" /></td>
  <td>E805</td>
  <td>Walk</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e806.png" alt="Bus" /></td>
  <td>E806</td>
  <td>Bus</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e809.png" alt="TiltUp" /></td>
  <td>E809</td>
  <td>TiltUp</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e80a.png" alt="TiltDown" /></td>
  <td>E80A</td>
  <td>TiltDown</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e80c.png" alt="RotateMapRight" /></td>
  <td>E80C</td>
  <td>RotateMapRight</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e80d.png" alt="RotateMapLeft" /></td>
  <td>E80D</td>
  <td>RotateMapLeft</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e80f.png" alt="Home" /></td>
  <td>E80F</td>
  <td>Home</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e811.png" alt="ParkingLocation" /></td>
  <td>E811</td>
  <td>ParkingLocation</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e812.png" alt="MapCompassTop" /></td>
  <td>E812</td>
  <td>MapCompassTop</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e813.png" alt="MapCompassBottom" /></td>
  <td>E813</td>
  <td>MapCompassBottom</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e814.png" alt="IncidentTriangle" /></td>
  <td>E814</td>
  <td>IncidentTriangle</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e815.png" alt="Touch" /></td>
  <td>E815</td>
  <td>Touch</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e816.png" alt="MapDirections" /></td>
  <td>E816</td>
  <td>MapDirections</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e819.png" alt="StartPoint" /></td>
  <td>E819</td>
  <td>StartPoint</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e81a.png" alt="StopPoint" /></td>
  <td>E81A</td>
  <td>StopPoint</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e81b.png" alt="EndPoint" /></td>
  <td>E81B</td>
  <td>EndPoint</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e81c.png" alt="History" /></td>
  <td>E81C</td>
  <td>History</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e81d.png" alt="Location" /></td>
  <td>E81D</td>
  <td>Location</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e81e.png" alt="MapLayers" /></td>
  <td>E81E</td>
  <td>MapLayers</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e81f.png" alt="Accident" /></td>
  <td>E81F</td>
  <td>Accident</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e821.png" alt="Work" /></td>
  <td>E821</td>
  <td>Work</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e822.png" alt="Construction" /></td>
  <td>E822</td>
  <td>Construction</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e823.png" alt="Recent" /></td>
  <td>E823</td>
  <td>Recent</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e825.png" alt="Bank" /></td>
  <td>E825</td>
  <td>Bank</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e826.png" alt="DownloadMap" /></td>
  <td>E826</td>
  <td>DownloadMap</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e829.png" alt="InkingToolFill2" /></td>
  <td>E829</td>
  <td>InkingToolFill2</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e82a.png" alt="HighlightFill2" /></td>
  <td>E82A</td>
  <td>HighlightFill2</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e82b.png" alt="EraseToolFill" /></td>
  <td>E82B</td>
  <td>EraseToolFill</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e82c.png" alt="EraseToolFill2" /></td>
  <td>E82C</td>
  <td>EraseToolFill2</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e82d.png" alt="Dictionary" /></td>
  <td>E82D</td>
  <td>Dictionary</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e82e.png" alt="DictionaryAdd" /></td>
  <td>E82E</td>
  <td>DictionaryAdd</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e82f.png" alt="ToolTip" /></td>
  <td>E82F</td>
  <td>ToolTip</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e830.png" alt="ChromeBack" /></td>
  <td>E830</td>
  <td>ChromeBack</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e835.png" alt="ProvisioningPackage" /></td>
  <td>E835</td>
  <td>ProvisioningPackage</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e836.png" alt="AddRemoteDevice" /></td>
  <td>E836</td>
  <td>AddRemoteDevice</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e839.png" alt="Ethernet" /></td>
  <td>E839</td>
  <td>Ethernet</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e83a.png" alt="&nbsp;ShareBroadband" /></td>
  <td>E83A</td>
  <td>&nbsp;ShareBroadband</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e83b.png" alt="DirectAccess" /></td>
  <td>E83B</td>
  <td>DirectAccess</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e83c.png" alt="&nbsp;DialUp" /></td>
  <td>E83C</td>
  <td>&nbsp;DialUp</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e83d.png" alt="DefenderApp" /></td>
  <td>E83D</td>
  <td>DefenderApp</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e83e.png" alt="BatteryCharging9" /></td>
  <td>E83E</td>
  <td>BatteryCharging9</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e83f.png" alt="Battery10" /></td>
  <td>E83F</td>
  <td>Battery10</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e840.png" alt="Pinned" /></td>
  <td>E840</td>
  <td>Pinned</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e841.png" alt="PinFill" /></td>
  <td>E841</td>
  <td>PinFill</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e842.png" alt="PinnedFill" /></td>
  <td>E842</td>
  <td>PinnedFill</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e843.png" alt="PeriodKey" /></td>
  <td>E843</td>
  <td>PeriodKey</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e844.png" alt="PuncKey" /></td>
  <td>E844</td>
  <td>PuncKey</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e845.png" alt="RevToggleKey" /></td>
  <td>E845</td>
  <td>RevToggleKey</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e846.png" alt="RightArrowKeyTime1" /></td>
  <td>E846</td>
  <td>RightArrowKeyTime1</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e847.png" alt="RightArrowKeyTime2" /></td>
  <td>E847</td>
  <td>RightArrowKeyTime2</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e848.png" alt="LeftQuote" /></td>
  <td>E848</td>
  <td>LeftQuote</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e849.png" alt="RightQuote" /></td>
  <td>E849</td>
  <td>RightQuote</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e84a.png" alt="DownShiftKey" /></td>
  <td>E84A</td>
  <td>DownShiftKey</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e84b.png" alt="UpShiftKey" /></td>
  <td>E84B</td>
  <td>UpShiftKey</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e84c.png" alt="PuncKey0" /></td>
  <td>E84C</td>
  <td>PuncKey0</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e84d.png" alt="PuncKeyLeftBottom" /></td>
  <td>E84D</td>
  <td>PuncKeyLeftBottom</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e84e.png" alt="RightArrowKeyTime3" /></td>
  <td>E84E</td>
  <td>RightArrowKeyTime3</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e84f.png" alt="RightArrowKeyTime4" /></td>
  <td>E84F</td>
  <td>RightArrowKeyTime4</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e850.png" alt="Battery0" /></td>
  <td>E850</td>
  <td>Battery0</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e851.png" alt="Battery1" /></td>
  <td>E851</td>
  <td>Battery1</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e852.png" alt="Battery2" /></td>
  <td>E852</td>
  <td>Battery2</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e853.png" alt="Battery3" /></td>
  <td>E853</td>
  <td>Battery3</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e854.png" alt="Battery4" /></td>
  <td>E854</td>
  <td>Battery4</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e855.png" alt="Battery5" /></td>
  <td>E855</td>
  <td>Battery5</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e856.png" alt="Battery6" /></td>
  <td>E856</td>
  <td>Battery6</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e857.png" alt="Battery7" /></td>
  <td>E857</td>
  <td>Battery7</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e858.png" alt="Battery8" /></td>
  <td>E858</td>
  <td>Battery8</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e859.png" alt="Battery9" /></td>
  <td>E859</td>
  <td>Battery9</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e85a.png" alt="BatteryCharging0" /></td>
  <td>E85A</td>
  <td>BatteryCharging0</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e85b.png" alt="BatteryCharging1" /></td>
  <td>E85B</td>
  <td>BatteryCharging1</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e85c.png" alt="BatteryCharging2" /></td>
  <td>E85C</td>
  <td>BatteryCharging2</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e85d.png" alt="BatteryCharging3" /></td>
  <td>E85D</td>
  <td>BatteryCharging3</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e85e.png" alt="BatteryCharging4" /></td>
  <td>E85E</td>
  <td>BatteryCharging4</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e85f.png" alt="BatteryCharging5" /></td>
  <td>E85F</td>
  <td>BatteryCharging5</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e860.png" alt="BatteryCharging6" /></td>
  <td>E860</td>
  <td>BatteryCharging6</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e861.png" alt="BatteryCharging7" /></td>
  <td>E861</td>
  <td>BatteryCharging7</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e862.png" alt="BatteryCharging8" /></td>
  <td>E862</td>
  <td>BatteryCharging8</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e863.png" alt="BatterySaver0" /></td>
  <td>E863</td>
  <td>BatterySaver0</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e864.png" alt="BatterySaver1" /></td>
  <td>E864</td>
  <td>BatterySaver1</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e865.png" alt="BatterySaver2" /></td>
  <td>E865</td>
  <td>BatterySaver2</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e866.png" alt="BatterySaver3" /></td>
  <td>E866</td>
  <td>BatterySaver3</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e867.png" alt="BatterySaver4" /></td>
  <td>E867</td>
  <td>BatterySaver4</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e868.png" alt="BatterySaver5" /></td>
  <td>E868</td>
  <td>BatterySaver5</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e869.png" alt="BatterySaver6" /></td>
  <td>E869</td>
  <td>BatterySaver6</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e86a.png" alt="BatterySaver7" /></td>
  <td>E86A</td>
  <td>BatterySaver7</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e86b.png" alt="BatterySaver8" /></td>
  <td>E86B</td>
  <td>BatterySaver8</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e86c.png" alt="SignalBars1" /></td>
  <td>E86C</td>
  <td>SignalBars1</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e86d.png" alt="SignalBars2" /></td>
  <td>E86D</td>
  <td>SignalBars2</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e86e.png" alt="SignalBars3" /></td>
  <td>E86E</td>
  <td>SignalBars3</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e86f.png" alt="SignalBars4" /></td>
  <td>E86F</td>
  <td>SignalBars4</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e870.png" alt="SignalBars5" /></td>
  <td>E870</td>
  <td>SignalBars5</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e871.png" alt="SignalNotConnected" /></td>
  <td>E871</td>
  <td>SignalNotConnected</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e872.png" alt="Wifi1" /></td>
  <td>E872</td>
  <td>Wifi1</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e873.png" alt="Wifi2" /></td>
  <td>E873</td>
  <td>Wifi2</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e874.png" alt="Wifi3" /></td>
  <td>E874</td>
  <td>Wifi3</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e875.png" alt="SIMLock" /></td>
  <td>E875</td>
  <td>SIMLock</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e876.png" alt="SIMMissing" /></td>
  <td>E876</td>
  <td>SIMMissing</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e877.png" alt="Vibrate" /></td>
  <td>E877</td>
  <td>Vibrate</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e878.png" alt="RoamingInternational" /></td>
  <td>E878</td>
  <td>RoamingInternational</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e879.png" alt="RoamingDomestic" /></td>
  <td>E879</td>
  <td>RoamingDomestic</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e87a.png" alt="CallForwardInternational" /></td>
  <td>E87A</td>
  <td>CallForwardInternational</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e87b.png" alt="CallForwardRoaming" /></td>
  <td>E87B</td>
  <td>CallForwardRoaming</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e87c.png" alt="JpnRomanji" /></td>
  <td>E87C</td>
  <td>JpnRomanji</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e87d.png" alt="JpnRomanjiLock" /></td>
  <td>E87D</td>
  <td>JpnRomanjiLock</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e87e.png" alt="JpnRomanjiShift" /></td>
  <td>E87E</td>
  <td>JpnRomanjiShift</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e87f.png" alt="JpnRomanjiShiftLock" /></td>
  <td>E87F</td>
  <td>JpnRomanjiShiftLock</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e880.png" alt="StatusDataTransfer" /></td>
  <td>E880</td>
  <td>StatusDataTransfer</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e881.png" alt="StatusDataTransferVPN" /></td>
  <td>E881</td>
  <td>StatusDataTransferVPN</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e882.png" alt="StatusDualSIM2" /></td>
  <td>E882</td>
  <td>StatusDualSIM2</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e883.png" alt="StatusDualSIM2VPN" /></td>
  <td>E883</td>
  <td>StatusDualSIM2VPN</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e884.png" alt="StatusDualSIM1" /></td>
  <td>E884</td>
  <td>StatusDualSIM1</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e885.png" alt="StatusDualSIM1VPN" /></td>
  <td>E885</td>
  <td>StatusDualSIM1VPN</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e886.png" alt="StatusSGLTE" /></td>
  <td>E886</td>
  <td>StatusSGLTE</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e887.png" alt="StatusSGLTECell" /></td>
  <td>E887</td>
  <td>StatusSGLTECell</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e888.png" alt="StatusSGLTEDataVPN" /></td>
  <td>E888</td>
  <td>StatusSGLTEDataVPN</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e889.png" alt="StatusVPN" /></td>
  <td>E889</td>
  <td>StatusVPN</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e88a.png" alt="WifiHotspot" /></td>
  <td>E88A</td>
  <td>WifiHotspot</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e88b.png" alt="LanguageKor" /></td>
  <td>E88B</td>
  <td>LanguageKor</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e88c.png" alt="LanguageCht" /></td>
  <td>E88C</td>
  <td>LanguageCht</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e88d.png" alt="LanguageChs" /></td>
  <td>E88D</td>
  <td>LanguageChs</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e88e.png" alt="USB" /></td>
  <td>E88E</td>
  <td>USB</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e88f.png" alt="InkingToolFill" /></td>
  <td>E88F</td>
  <td>InkingToolFill</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e890.png" alt="View" /></td>
  <td>E890</td>
  <td>View</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e891.png" alt="HighlightFill" /></td>
  <td>E891</td>
  <td>HighlightFill</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e892.png" alt="Previous" /></td>
  <td>E892</td>
  <td>Previous</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e893.png" alt="Next" /></td>
  <td>E893</td>
  <td>Next</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e894.png" alt="Clear" /></td>
  <td>E894</td>
  <td>Clear</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e895.png" alt="Sync" /></td>
  <td>E895</td>
  <td>Sync</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e896.png" alt="Download" /></td>
  <td>E896</td>
  <td>Download</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e897.png" alt="Help" /></td>
  <td>E897</td>
  <td>Help</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e898.png" alt="Upload" /></td>
  <td>E898</td>
  <td>Upload</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e899.png" alt="Emoji" /></td>
  <td>E899</td>
  <td>Emoji</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e89a.png" alt="TwoPage" /></td>
  <td>E89A</td>
  <td>TwoPage</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e89b.png" alt="LeaveChat" /></td>
  <td>E89B</td>
  <td>LeaveChat</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e89c.png" alt="MailForward" /></td>
  <td>E89C</td>
  <td>MailForward</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e89e.png" alt="RotateCamera" /></td>
  <td>E89E</td>
  <td>RotateCamera</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e89f.png" alt="ClosePane" /></td>
  <td>E89F</td>
  <td>ClosePane</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e8a0.png" alt="OpenPane" /></td>
  <td>E8A0</td>
  <td>OpenPane</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e8a1.png" alt="PreviewLink" /></td>
  <td>E8A1</td>
  <td>PreviewLink</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e8a2.png" alt="AttachCamera" /></td>
  <td>E8A2</td>
  <td>AttachCamera</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e8a3.png" alt="ZoomIn" /></td>
  <td>E8A3</td>
  <td>ZoomIn</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e8a4.png" alt="Bookmarks" /></td>
  <td>E8A4</td>
  <td>Bookmarks</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e8a5.png" alt="Document" /></td>
  <td>E8A5</td>
  <td>Document</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e8a6.png" alt="ProtectedDocument" /></td>
  <td>E8A6</td>
  <td>ProtectedDocument</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e8a7.png" alt="OpenInNewWindow" /></td>
  <td>E8A7</td>
  <td>OpenInNewWindow</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e8a8.png" alt="MailFill" /></td>
  <td>E8A8</td>
  <td>MailFill</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e8a9.png" alt="ViewAll" /></td>
  <td>E8A9</td>
  <td>ViewAll</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e8aa.png" alt="VideoChat" /></td>
  <td>E8AA</td>
  <td>VideoChat</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e8ab.png" alt="Switch" /></td>
  <td>E8AB</td>
  <td>Switch</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e8ac.png" alt="Rename" /></td>
  <td>E8AC</td>
  <td>Rename</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e8ad.png" alt="Go" /></td>
  <td>E8AD</td>
  <td>Go</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e8ae.png" alt="SurfaceHub" /></td>
  <td>E8AE</td>
  <td>SurfaceHub</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e8af.png" alt="Remote" /></td>
  <td>E8AF</td>
  <td>Remote</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e8b0.png" alt="Click" /></td>
  <td>E8B0</td>
  <td>Click</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e8b1.png" alt="Shuffle" /></td>
  <td>E8B1</td>
  <td>Shuffle</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e8b2.png" alt="Movies" /></td>
  <td>E8B2</td>
  <td>Movies</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e8b3.png" alt="SelectAll" /></td>
  <td>E8B3</td>
  <td>SelectAll</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e8b4.png" alt="Orientation" /></td>
  <td>E8B4</td>
  <td>Orientation</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e8b5.png" alt="Import" /></td>
  <td>E8B5</td>
  <td>Import</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e8b6.png" alt="ImportAll" /></td>
  <td>E8B6</td>
  <td>ImportAll</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e8b7.png" alt="Folder" /></td>
  <td>E8B7</td>
  <td>Folder</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e8b8.png" alt="Webcam" /></td>
  <td>E8B8</td>
  <td>Webcam</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e8b9.png" alt="Picture" /></td>
  <td>E8B9</td>
  <td>Picture</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e8ba.png" alt="Caption" /></td>
  <td>E8BA</td>
  <td>Caption</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e8bb.png" alt="ChromeClose" /></td>
  <td>E8BB</td>
  <td>ChromeClose</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e8bc.png" alt="ShowResults" /></td>
  <td>E8BC</td>
  <td>ShowResults</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e8bd.png" alt="Message" /></td>
  <td>E8BD</td>
  <td>Message</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e8be.png" alt="Leaf" /></td>
  <td>E8BE</td>
  <td>Leaf</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e8bf.png" alt="CalendarDay" /></td>
  <td>E8BF</td>
  <td>CalendarDay</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e8c0.png" alt="CalendarWeek" /></td>
  <td>E8C0</td>
  <td>CalendarWeek</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e8c1.png" alt="Characters" /></td>
  <td>E8C1</td>
  <td>Characters</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e8c2.png" alt="MailReplyAll" /></td>
  <td>E8C2</td>
  <td>MailReplyAll</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e8c3.png" alt="Read" /></td>
  <td>E8C3</td>
  <td>Read</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e8c4.png" alt="ShowBcc" /></td>
  <td>E8C4</td>
  <td>ShowBcc</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e8c5.png" alt="HideBcc" /></td>
  <td>E8C5</td>
  <td>HideBcc</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e8c6.png" alt="Cut" /></td>
  <td>E8C6</td>
  <td>Cut</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e8c8.png" alt="Copy" /></td>
  <td>E8C8</td>
  <td>Copy</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e8c9.png" alt="Important" /></td>
  <td>E8C9</td>
  <td>Important</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e8ca.png" alt="MailReply" /></td>
  <td>E8CA</td>
  <td>MailReply</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e8cb.png" alt="Sort" /></td>
  <td>E8CB</td>
  <td>Sort</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e8cc.png" alt="MobileTablet" /></td>
  <td>E8CC</td>
  <td>MobileTablet</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e8cd.png" alt="DisconnectDrive" /></td>
  <td>E8CD</td>
  <td>DisconnectDrive</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e8ce.png" alt="MapDrive" /></td>
  <td>E8CE</td>
  <td>MapDrive</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e8cf.png" alt="ContactPresence" /></td>
  <td>E8CF</td>
  <td>ContactPresence</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e8d0.png" alt="Priority" /></td>
  <td>E8D0</td>
  <td>Priority</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e8d1.png" alt="GotoToday" /></td>
  <td>E8D1</td>
  <td>GotoToday</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e8d2.png" alt="Font" /></td>
  <td>E8D2</td>
  <td>Font</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e8d3.png" alt="FontColor" /></td>
  <td>E8D3</td>
  <td>FontColor</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e8d4.png" alt="Contact2" /></td>
  <td>E8D4</td>
  <td>Contact2</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e8d5.png" alt="FolderFill" /></td>
  <td>E8D5</td>
  <td>FolderFill</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e8d6.png" alt="Audio" /></td>
  <td>E8D6</td>
  <td>Audio</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e8d7.png" alt="Permissions" /></td>
  <td>E8D7</td>
  <td>Permissions</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e8d8.png" alt="DisableUpdates" /></td>
  <td>E8D8</td>
  <td>DisableUpdates</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e8d9.png" alt="Unfavorite" /></td>
  <td>E8D9</td>
  <td>Unfavorite</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e8da.png" alt="OpenLocal" /></td>
  <td>E8DA</td>
  <td>OpenLocal</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e8db.png" alt="Italic" /></td>
  <td>E8DB</td>
  <td>Italic</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e8dc.png" alt="Underline" /></td>
  <td>E8DC</td>
  <td>Underline</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e8dd.png" alt="Bold" /></td>
  <td>E8DD</td>
  <td>Bold</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e8de.png" alt="MoveToFolder" /></td>
  <td>E8DE</td>
  <td>MoveToFolder</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e8df.png" alt="LikeDislike" /></td>
  <td>E8DF</td>
  <td>LikeDislike</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e8e0.png" alt="Dislike" /></td>
  <td>E8E0</td>
  <td>Dislike</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e8e1.png" alt="Like" /></td>
  <td>E8E1</td>
  <td>Like</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e8e2.png" alt="AlignRight" /></td>
  <td>E8E2</td>
  <td>AlignRight</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e8e3.png" alt="AlignCenter" /></td>
  <td>E8E3</td>
  <td>AlignCenter</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e8e4.png" alt="AlignLeft" /></td>
  <td>E8E4</td>
  <td>AlignLeft</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e8e5.png" alt="OpenFile" /></td>
  <td>E8E5</td>
  <td>OpenFile</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e8e6.png" alt="ClearSelection" /></td>
  <td>E8E6</td>
  <td>ClearSelection</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e8e7.png" alt="FontDecrease" /></td>
  <td>E8E7</td>
  <td>FontDecrease</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e8e8.png" alt="FontIncrease" /></td>
  <td>E8E8</td>
  <td>FontIncrease</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e8e9.png" alt="FontSize" /></td>
  <td>E8E9</td>
  <td>FontSize</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e8ea.png" alt="CellPhone" /></td>
  <td>E8EA</td>
  <td>CellPhone</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e8eb.png" alt="Reshare" /></td>
  <td>E8EB</td>
  <td>Reshare</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e8ec.png" alt="Tag" /></td>
  <td>E8EC</td>
  <td>Tag</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e8ed.png" alt="RepeatOne" /></td>
  <td>E8ED</td>
  <td>RepeatOne</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e8ee.png" alt="RepeatAll" /></td>
  <td>E8EE</td>
  <td>RepeatAll</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e8ef.png" alt="Calculator" /></td>
  <td>E8EF</td>
  <td>Calculator</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e8f0.png" alt="Directions" /></td>
  <td>E8F0</td>
  <td>Directions</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e8f1.png" alt="Library" /></td>
  <td>E8F1</td>
  <td>Library</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e8f2.png" alt="ChatBubbles" /></td>
  <td>E8F2</td>
  <td>ChatBubbles</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e8f3.png" alt="PostUpdate" /></td>
  <td>E8F3</td>
  <td>PostUpdate</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e8f4.png" alt="NewFolder" /></td>
  <td>E8F4</td>
  <td>NewFolder</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e8f5.png" alt="CalendarReply" /></td>
  <td>E8F5</td>
  <td>CalendarReply</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e8f6.png" alt="UnsyncFolder" /></td>
  <td>E8F6</td>
  <td>UnsyncFolder</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e8f7.png" alt="SyncFolder" /></td>
  <td>E8F7</td>
  <td>SyncFolder</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e8f8.png" alt="BlockContact" /></td>
  <td>E8F8</td>
  <td>BlockContact</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e8f9.png" alt="SwitchApps" /></td>
  <td>E8F9</td>
  <td>SwitchApps</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e8fa.png" alt="AddFriend" /></td>
  <td>E8FA</td>
  <td>AddFriend</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e8fb.png" alt="Accept" /></td>
  <td>E8FB</td>
  <td>Accept</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e8fc.png" alt="GoToStart" /></td>
  <td>E8FC</td>
  <td>GoToStart</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e8fd.png" alt="BulletedList" /></td>
  <td>E8FD</td>
  <td>BulletedList</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e8fe.png" alt="Scan" /></td>
  <td>E8FE</td>
  <td>Scan</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e8ff.png" alt="Preview" /></td>
  <td>E8FF</td>
  <td>Preview</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e904.png" alt="ZeroBars" /></td>
  <td>E904</td>
  <td>ZeroBars</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e905.png" alt="OneBar" /></td>
  <td>E905</td>
  <td>OneBar</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e906.png" alt="TwoBars" /></td>
  <td>E906</td>
  <td>TwoBars</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e907.png" alt="ThreeBars" /></td>
  <td>E907</td>
  <td>ThreeBars</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e908.png" alt="FourBars" /></td>
  <td>E908</td>
  <td>FourBars</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e909.png" alt="World" /></td>
  <td>E909</td>
  <td>World</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e90a.png" alt="Comment" /></td>
  <td>E90A</td>
  <td>Comment</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e90b.png" alt="MusicInfo" /></td>
  <td>E90B</td>
  <td>MusicInfo</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e90c.png" alt="DockLeft" /></td>
  <td>E90C</td>
  <td>DockLeft</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e90d.png" alt="DockRight" /></td>
  <td>E90D</td>
  <td>DockRight</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e90e.png" alt="DockBottom" /></td>
  <td>E90E</td>
  <td>DockBottom</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e90f.png" alt="Repair" /></td>
  <td>E90F</td>
  <td>Repair</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e910.png" alt="Accounts" /></td>
  <td>E910</td>
  <td>Accounts</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e911.png" alt="DullSound" /></td>
  <td>E911</td>
  <td>DullSound</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e912.png" alt="Manage" /></td>
  <td>E912</td>
  <td>Manage</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e913.png" alt="Street" /></td>
  <td>E913</td>
  <td>Street</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e914.png" alt="Printer3D" /></td>
  <td>E914</td>
  <td>Printer3D</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e915.png" alt="RadioBullet" /></td>
  <td>E915</td>
  <td>RadioBullet</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e916.png" alt="Stopwatch" /></td>
  <td>E916</td>
  <td>Stopwatch</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e91b.png" alt="Photo" /></td>
  <td>E91B</td>
  <td>Photo</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e91c.png" alt="ActionCenter" /></td>
  <td>E91C</td>
  <td>ActionCenter</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e91f.png" alt="FullCircleMask" /></td>
  <td>E91F</td>
  <td>FullCircleMask</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e921.png" alt="ChromeMinimize" /></td>
  <td>E921</td>
  <td>ChromeMinimize</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e922.png" alt="ChromeMaximize" /></td>
  <td>E922</td>
  <td>ChromeMaximize</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e923.png" alt="ChromeRestore" /></td>
  <td>E923</td>
  <td>ChromeRestore</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e924.png" alt="Annotation" /></td>
  <td>E924</td>
  <td>Annotation</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e925.png" alt="BackSpaceQWERTYSm" /></td>
  <td>E925</td>
  <td>BackSpaceQWERTYSm</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e926.png" alt="BackSpaceQWERTYMd" /></td>
  <td>E926</td>
  <td>BackSpaceQWERTYMd</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e927.png" alt="Swipe" /></td>
  <td>E927</td>
  <td>Swipe</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e928.png" alt="Fingerprint" /></td>
  <td>E928</td>
  <td>Fingerprint</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e929.png" alt="Handwriting" /></td>
  <td>E929</td>
  <td>Handwriting</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e92c.png" alt="ChromeBackToWindow" /></td>
  <td>E92C</td>
  <td>ChromeBackToWindow</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e92d.png" alt="ChromeFullScreen" /></td>
  <td>E92D</td>
  <td>ChromeFullScreen</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e92e.png" alt="KeyboardStandard" /></td>
  <td>E92E</td>
  <td>KeyboardStandard</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e92f.png" alt="KeyboardDismiss" /></td>
  <td>E92F</td>
  <td>KeyboardDismiss</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e930.png" alt="Completed" /></td>
  <td>E930</td>
  <td>Completed</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e931.png" alt="ChromeAnnotate" /></td>
  <td>E931</td>
  <td>ChromeAnnotate</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e932.png" alt="Label" /></td>
  <td>E932</td>
  <td>Label</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e933.png" alt="IBeam" /></td>
  <td>E933</td>
  <td>IBeam</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e934.png" alt="IBeamOutline" /></td>
  <td>E934</td>
  <td>IBeamOutline</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e935.png" alt="FlickDown" /></td>
  <td>E935</td>
  <td>FlickDown</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e936.png" alt="FlickUp" /></td>
  <td>E936</td>
  <td>FlickUp</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e937.png" alt="FlickLeft" /></td>
  <td>E937</td>
  <td>FlickLeft</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e938.png" alt="FlickRight" /></td>
  <td>E938</td>
  <td>FlickRight</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e939.png" alt="FeedbackApp" /></td>
  <td>E939</td>
  <td>FeedbackApp</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e93c.png" alt="MusicAlbum" /></td>
  <td>E93C</td>
  <td>MusicAlbum</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e93e.png" alt="Streaming" /></td>
  <td>E93E</td>
  <td>Streaming</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e943.png" alt="Code" /></td>
  <td>E943</td>
  <td>Code</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e944.png" alt="ReturnToWindow" /></td>
  <td>E944</td>
  <td>ReturnToWindow</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e945.png" alt="LightningBolt" /></td>
  <td>E945</td>
  <td>LightningBolt</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e946.png" alt="Info" /></td>
  <td>E946</td>
  <td>Info</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e947.png" alt="CalculatorMultiply" /></td>
  <td>E947</td>
  <td>CalculatorMultiply</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e948.png" alt="CalculatorAddition" /></td>
  <td>E948</td>
  <td>CalculatorAddition</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e949.png" alt="CalculatorSubtract" /></td>
  <td>E949</td>
  <td>CalculatorSubtract</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e94a.png" alt="CalculatorDivide" /></td>
  <td>E94A</td>
  <td>CalculatorDivide</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e94b.png" alt="CalculatorSquareroot" /></td>
  <td>E94B</td>
  <td>CalculatorSquareroot</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e94c.png" alt="CalculatorPercentage" /></td>
  <td>E94C</td>
  <td>CalculatorPercentage</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e94d.png" alt="CalculatorNegate" /></td>
  <td>E94D</td>
  <td>CalculatorNegate</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e94e.png" alt="CalculatorEqualTo" /></td>
  <td>E94E</td>
  <td>CalculatorEqualTo</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e94f.png" alt="CalculatorBackspace" /></td>
  <td>E94F</td>
  <td>CalculatorBackspace</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e950.png" alt="Component" /></td>
  <td>E950</td>
  <td>Component</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e951.png" alt="DMC" /></td>
  <td>E951</td>
  <td>DMC</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e952.png" alt="Dock" /></td>
  <td>E952</td>
  <td>Dock</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e953.png" alt="MultimediaDMS" /></td>
  <td>E953</td>
  <td>MultimediaDMS</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e954.png" alt="MultimediaDVR" /></td>
  <td>E954</td>
  <td>MultimediaDVR</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e955.png" alt="MultimediaPMP" /></td>
  <td>E955</td>
  <td>MultimediaPMP</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e956.png" alt="PrintfaxPrinterFile" /></td>
  <td>E956</td>
  <td>PrintfaxPrinterFile</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e957.png" alt="Sensor" /></td>
  <td>E957</td>
  <td>Sensor</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e958.png" alt="StorageOptical" /></td>
  <td>E958</td>
  <td>StorageOptical</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e95a.png" alt="Communications" /></td>
  <td>E95A</td>
  <td>Communications</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e95b.png" alt="Headset" /></td>
  <td>E95B</td>
  <td>Headset</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e95d.png" alt="Projector" /></td>
  <td>E95D</td>
  <td>Projector</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e95e.png" alt="Health" /></td>
  <td>E95E</td>
  <td>Health</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e960.png" alt="Webcam2" /></td>
  <td>E960</td>
  <td>Webcam2</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e961.png" alt="Input" /></td>
  <td>E961</td>
  <td>Input</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e962.png" alt="Mouse" /></td>
  <td>E962</td>
  <td>Mouse</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e963.png" alt="Smartcard" /></td>
  <td>E963</td>
  <td>Smartcard</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e964.png" alt="SmartcardVirtual" /></td>
  <td>E964</td>
  <td>SmartcardVirtual</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e965.png" alt="MediaStorageTower" /></td>
  <td>E965</td>
  <td>MediaStorageTower</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e966.png" alt="ReturnKeySm" /></td>
  <td>E966</td>
  <td>ReturnKeySm</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e967.png" alt="GameConsole" /></td>
  <td>E967</td>
  <td>GameConsole</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e968.png" alt="Network" /></td>
  <td>E968</td>
  <td>Network</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e969.png" alt="StorageNetworkWireless" /></td>
  <td>E969</td>
  <td>StorageNetworkWireless</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e96a.png" alt="StorageTape" /></td>
  <td>E96A</td>
  <td>StorageTape</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e96d.png" alt="ChevronUpSmall" /></td>
  <td>E96D</td>
  <td>ChevronUpSmall</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e96e.png" alt="ChevronDownSmall" /></td>
  <td>E96E</td>
  <td>ChevronDownSmall</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e96f.png" alt="ChevronLeftSmall" /></td>
  <td>E96F</td>
  <td>ChevronLeftSmall</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e970.png" alt="ChevronRightSmall" /></td>
  <td>E970</td>
  <td>ChevronRightSmall</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e971.png" alt="ChevronUpMed" /></td>
  <td>E971</td>
  <td>ChevronUpMed</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e972.png" alt="ChevronDownMed" /></td>
  <td>E972</td>
  <td>ChevronDownMed</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e973.png" alt="ChevronLeftMed" /></td>
  <td>E973</td>
  <td>ChevronLeftMed</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e974.png" alt="ChevronRightMed" /></td>
  <td>E974</td>
  <td>ChevronRightMed</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e975.png" alt="Devices2" /></td>
  <td>E975</td>
  <td>Devices2</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e976.png" alt="ExpandTile" /></td>
  <td>E976</td>
  <td>ExpandTile</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e977.png" alt="PC1" /></td>
  <td>E977</td>
  <td>PC1</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e978.png" alt="PresenceChicklet" /></td>
  <td>E978</td>
  <td>PresenceChicklet</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e979.png" alt="PresenceChickletVideo" /></td>
  <td>E979</td>
  <td>PresenceChickletVideo</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e97a.png" alt="Reply" /></td>
  <td>E97A</td>
  <td>Reply</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e97b.png" alt="SetTile" /></td>
  <td>E97B</td>
  <td>SetTile</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e97c.png" alt="Type" /></td>
  <td>E97C</td>
  <td>Type</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e97d.png" alt="Korean" /></td>
  <td>E97D</td>
  <td>Coréen</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e97e.png" alt="HalfAlpha" /></td>
  <td>E97E</td>
  <td>HalfAlpha</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e97f.png" alt="FullAlpha" /></td>
  <td>E97F</td>
  <td>FullAlpha</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e980.png" alt="Key12On" /></td>
  <td>E980</td>
  <td>Key12On</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e981.png" alt="ChineseChangjie" /></td>
  <td>E981</td>
  <td>ChineseChangjie</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e982.png" alt="QWERTYOn" /></td>
  <td>E982</td>
  <td>QWERTYOn</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e983.png" alt="QWERTYOff" /></td>
  <td>E983</td>
  <td>QWERTYOff</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e984.png" alt="ChineseQuick" /></td>
  <td>E984</td>
  <td>ChineseQuick</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e985.png" alt="Japanese" /></td>
  <td>E985</td>
  <td>Japonais</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e986.png" alt="FullHiragana" /></td>
  <td>E986</td>
  <td>FullHiragana</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e987.png" alt="FullKatakana" /></td>
  <td>E987</td>
  <td>FullKatakana</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e988.png" alt="HalfKatakana" /></td>
  <td>E988</td>
  <td>HalfKatakana</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e989.png" alt="ChineseBoPoMoFo" /></td>
  <td>E989</td>
  <td>ChineseBoPoMoFo</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e98a.png" alt="ChinesePinyin" /></td>
  <td>E98A</td>
  <td>ChinesePinyin</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e98f.png" alt="ConstructionCone" /></td>
  <td>E98F</td>
  <td>ConstructionCone</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e990.png" alt="XboxOneConsole" /></td>
  <td>E990</td>
  <td>XboxOneConsole</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e992.png" alt="Volume0" /></td>
  <td>E992</td>
  <td>Volume0</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e993.png" alt="Volume1" /></td>
  <td>E993</td>
  <td>Volume1</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e994.png" alt="Volume2" /></td>
  <td>E994</td>
  <td>Volume2</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e995.png" alt="Volume3" /></td>
  <td>E995</td>
  <td>Volume3</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e996.png" alt="BatteryUnknown" /></td>
  <td>E996</td>
  <td>BatteryUnknown</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e998.png" alt="WifiAttentionOverlay" /></td>
  <td>E998</td>
  <td>WifiAttentionOverlay</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e99a.png" alt="Robot" /></td>
  <td>E99A</td>
  <td>Robot</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e9a1.png" alt="TapAndSend" /></td>
  <td>E9A1</td>
  <td>TapAndSend</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e9a8.png" alt="PasswordKeyShow" /></td>
  <td>E9A8</td>
  <td>PasswordKeyShow</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e9a9.png" alt="PasswordKeyHide" /></td>
  <td>E9A9</td>
  <td>PasswordKeyHide</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e9aa.png" alt="BidiLtr" /></td>
  <td>E9AA</td>
  <td>BidiLtr</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e9ab.png" alt="BidiRtl" /></td>
  <td>E9AB</td>
  <td>BidiRtl</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e9ac.png" alt="ForwardSm" /></td>
  <td>E9AC</td>
  <td>ForwardSm</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e9ad.png" alt="CommaKey" /></td>
  <td>E9AD</td>
  <td>CommaKey</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e9ae.png" alt="DashKey" /></td>
  <td>E9AE</td>
  <td>DashKey</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e9af.png" alt="DullSoundKey" /></td>
  <td>E9AF</td>
  <td>DullSoundKey</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e9b0.png" alt="HalfDullSound" /></td>
  <td>E9B0</td>
  <td>HalfDullSound</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e9b1.png" alt="RightDoubleQuote" /></td>
  <td>E9B1</td>
  <td>RightDoubleQuote</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e9b2.png" alt="LeftDoubleQuote" /></td>
  <td>E9B2</td>
  <td>LeftDoubleQuote</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e9b3.png" alt="PuncKeyRightBottom" /></td>
  <td>E9B3</td>
  <td>PuncKeyRightBottom</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e9b4.png" alt="PuncKey1" /></td>
  <td>E9B4</td>
  <td>PuncKey1</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e9b5.png" alt="PuncKey2" /></td>
  <td>E9B5</td>
  <td>PuncKey2</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e9b6.png" alt="PuncKey3" /></td>
  <td>E9B6</td>
  <td>PuncKey3</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e9b7.png" alt="PuncKey4" /></td>
  <td>E9B7</td>
  <td>PuncKey4</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e9b8.png" alt="PuncKey5" /></td>
  <td>E9B8</td>
  <td>PuncKey5</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e9b9.png" alt="PuncKey6" /></td>
  <td>E9B9</td>
  <td>PuncKey6</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e9ba.png" alt="PuncKey9" /></td>
  <td>E9BA</td>
  <td>PuncKey9</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e9bb.png" alt="PuncKey7" /></td>
  <td>E9BB</td>
  <td>PuncKey7</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e9bc.png" alt="PuncKey8" /></td>
  <td>E9BC</td>
  <td>PuncKey8</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e9ca.png" alt="Frigid" /></td>
  <td>E9CA</td>
  <td>Frigid</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e9d9.png" alt="Diagnostic" /></td>
  <td>E9D9</td>
  <td>Diagnostic</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/e9f3.png" alt="Process" /></td>
  <td>E9F3</td>
  <td>Process</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ea14.png" alt="DisconnectDisplay" /></td>
  <td>EA14</td>
  <td>DisconnectDisplay</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ea1f.png" alt="Info2" /></td>
  <td>EA1F</td>
  <td>Info2</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ea21.png" alt="ActionCenterAsterisk" /></td>
  <td>EA21</td>
  <td>ActionCenterAsterisk</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ea24.png" alt="Beta" /></td>
  <td>EA24</td>
  <td>Beta</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ea35.png" alt="SaveCopy" /></td>
  <td>EA35</td>
  <td>SaveCopy</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ea37.png" alt="List" /></td>
  <td>EA37</td>
  <td>List</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ea38.png" alt="Asterisk" /></td>
  <td>EA38</td>
  <td>Asterisk</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ea39.png" alt="ErrorBadge" /></td>
  <td>EA39</td>
  <td>ErrorBadge</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ea3a.png" alt="CircleRing" /></td>
  <td>EA3A</td>
  <td>CircleRing</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ea3b.png" alt="CircleFill" /></td>
  <td>EA3B</td>
  <td>CircleFill</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ea40.png" alt="AllAppsMirrored" /></td>
  <td>EA40</td>
  <td>AllAppsMirrored</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ea41.png" alt="BookmarksMirrored" /></td>
  <td>EA41</td>
  <td>BookmarksMirrored</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ea42.png" alt="BulletedListMirrored" /></td>
  <td>EA42</td>
  <td>BulletedListMirrored</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ea43.png" alt="CallForwardInternationalMirrored" /></td>
  <td>EA43</td>
  <td>CallForwardInternationalMirrored</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ea44.png" alt="CallForwardRoamingMirrored" /></td>
  <td>EA44</td>
  <td>CallForwardRoamingMirrored</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ea47.png" alt="ChromeBackMirrored" /></td>
  <td>EA47</td>
  <td>ChromeBackMirrored</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ea48.png" alt="ClearSelectionMirrored" /></td>
  <td>EA48</td>
  <td>ClearSelectionMirrored</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ea49.png" alt="ClosePaneMirrored" /></td>
  <td>EA49</td>
  <td>ClosePaneMirrored</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ea4a.png" alt="ContactInfoMirrored" /></td>
  <td>EA4A</td>
  <td>ContactInfoMirrored</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ea4b.png" alt="DockRightMirrored" /></td>
  <td>EA4B</td>
  <td>DockRightMirrored</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ea4c.png" alt="DockLeftMirrored" /></td>
  <td>EA4C</td>
  <td>DockLeftMirrored</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ea4e.png" alt="ExpandTileMirrored" /></td>
  <td>EA4E</td>
  <td>ExpandTileMirrored</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ea4f.png" alt="GoMirrored" /></td>
  <td>EA4F</td>
  <td>GoMirrored</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ea50.png" alt="GripperResizeMirrored" /></td>
  <td>EA50</td>
  <td>GripperResizeMirrored</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ea51.png" alt="HelpMirrored" /></td>
  <td>EA51</td>
  <td>HelpMirrored</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ea52.png" alt="ImportMirrored" /></td>
  <td>EA52</td>
  <td>ImportMirrored</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ea53.png" alt="ImportAllMirrored" /></td>
  <td>EA53</td>
  <td>ImportAllMirrored</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ea54.png" alt="LeaveChatMirrored" /></td>
  <td>EA54</td>
  <td>LeaveChatMirrored</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ea55.png" alt="ListMirrored" /></td>
  <td>EA55</td>
  <td>ListMirrored</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ea56.png" alt="MailForwardMirrored" /></td>
  <td>EA56</td>
  <td>MailForwardMirrored</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ea57.png" alt="MailReplyMirrored" /></td>
  <td>EA57</td>
  <td>MailReplyMirrored</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ea58.png" alt="MailReplyAllMirrored" /></td>
  <td>EA58</td>
  <td>MailReplyAllMirrored</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ea5b.png" alt="OpenPaneMirrored" /></td>
  <td>EA5B</td>
  <td>OpenPaneMirrored</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ea5c.png" alt="OpenWithMirrored" /></td>
  <td>EA5C</td>
  <td>OpenWithMirrored</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ea5e.png" alt="ParkingLocationMirrored" /></td>
  <td>EA5E</td>
  <td>ParkingLocationMirrored</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ea5f.png" alt="ResizeMouseMediumMirrored" /></td>
  <td>EA5F</td>
  <td>ResizeMouseMediumMirrored</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ea60.png" alt="ResizeMouseSmallMirrored" /></td>
  <td>EA60</td>
  <td>ResizeMouseSmallMirrored</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ea61.png" alt="ResizeMouseTallMirrored" /></td>
  <td>EA61</td>
  <td>ResizeMouseTallMirrored</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ea62.png" alt="ResizeTouchNarrowerMirrored" /></td>
  <td>EA62</td>
  <td>ResizeTouchNarrowerMirrored</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ea63.png" alt="SendMirrored" /></td>
  <td>EA63</td>
  <td>SendMirrored</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ea64.png" alt="SendFillMirrored" /></td>
  <td>EA64</td>
  <td>SendFillMirrored</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ea65.png" alt="ShowResultsMirrored" /></td>
  <td>EA65</td>
  <td>ShowResultsMirrored</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ea69.png" alt="Media" /></td>
  <td>EA69</td>
  <td>Media</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ea6a.png" alt="SyncError" /></td>
  <td>EA6A</td>
  <td>SyncError</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ea6c.png" alt="Devices3" /></td>
  <td>EA6C</td>
  <td>Devices3</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ea80.png" alt="Lightbulb" /></td>
  <td>EA80</td>
  <td>Lightbulb</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ea81.png" alt="StatusCircle" /></td>
  <td>EA81</td>
  <td>StatusCircle</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ea82.png" alt="StatusTriangle" /></td>
  <td>EA82</td>
  <td>StatusTriangle</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ea83.png" alt="StatusError" /></td>
  <td>EA83</td>
  <td>StatusError</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ea84.png" alt="StatusWarning" /></td>
  <td>EA84</td>
  <td>StatusWarning</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ea86.png" alt="Puzzle" /></td>
  <td>EA86</td>
  <td>Puzzle</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ea89.png" alt="CalendarSolid" /></td>
  <td>EA89</td>
  <td>CalendarSolid</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ea8a.png" alt="HomeSolid" /></td>
  <td>EA8A</td>
  <td>HomeSolid</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ea8b.png" alt="ParkingLocationSolid" /></td>
  <td>EA8B</td>
  <td>ParkingLocationSolid</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ea8c.png" alt="ContactSolid" /></td>
  <td>EA8C</td>
  <td>ContactSolid</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ea8d.png" alt="ConstructionSolid" /></td>
  <td>EA8D</td>
  <td>ConstructionSolid</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ea8e.png" alt="AccidentSolid" /></td>
  <td>EA8E</td>
  <td>AccidentSolid</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ea8f.png" alt="Ringer" /></td>
  <td>EA8F</td>
  <td>Ringer</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ea91.png" alt="ThoughtBubble" /></td>
  <td>EA91</td>
  <td>ThoughtBubble</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ea92.png" alt="HeartBroken" /></td>
  <td>EA92</td>
  <td>HeartBroken</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ea93.png" alt="BatteryCharging10" /></td>
  <td>EA93</td>
  <td>BatteryCharging10</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ea94.png" alt="BatterySaver9" /></td>
  <td>EA94</td>
  <td>BatterySaver9</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ea95.png" alt="BatterySaver10" /></td>
  <td>EA95</td>
  <td>BatterySaver10</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ea97.png" alt="CallForwardingMirrored" /></td>
  <td>EA97</td>
  <td>CallForwardingMirrored</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ea98.png" alt="MultiSelectMirrored" /></td>
  <td>EA98</td>
  <td>MultiSelectMirrored</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ea99.png" alt="Broom" /></td>
  <td>EA99</td>
  <td>Broom</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/eadf.png" alt="Trackers" /></td>
  <td>EADF</td>
  <td>Trackers</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/eb05.png" alt="PieSingle" /></td>
  <td>EB05</td>
  <td>PieSingle</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/eb0f.png" alt="StockDown" /></td>
  <td>EB0F</td>
  <td>StockDown</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/eb11.png" alt="StockUp" /></td>
  <td>EB11</td>
  <td>StockUp</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/eb42.png" alt="Drop" /></td>
  <td>EB42</td>
  <td>Drop</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/eb47.png" alt="BusSolid" /></td>
  <td>EB47</td>
  <td>BusSolid</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/eb48.png" alt="FerrySolid" /></td>
  <td>EB48</td>
  <td>FerrySolid</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/eb49.png" alt="StartPointSolid" /></td>
  <td>EB49</td>
  <td>StartPointSolid</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/eb4a.png" alt="StopPointSolid" /></td>
  <td>EB4A</td>
  <td>StopPointSolid</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/eb4b.png" alt="EndPointSolid" /></td>
  <td>EB4B</td>
  <td>EndPointSolid</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/eb4c.png" alt="AirplaneSolid" /></td>
  <td>EB4C</td>
  <td>AirplaneSolid</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/eb4d.png" alt="TrainSolid" /></td>
  <td>EB4D</td>
  <td>TrainSolid</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/eb4e.png" alt="WorkSolid" /></td>
  <td>EB4E</td>
  <td>WorkSolid</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/eb4f.png" alt="ReminderFill" /></td>
  <td>EB4F</td>
  <td>ReminderFill</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/eb50.png" alt="Reminder" /></td>
  <td>EB50</td>
  <td>Reminder</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/eb51.png" alt="Heart" /></td>
  <td>EB51</td>
  <td>Heart</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/eb52.png" alt="HeartFill" /></td>
  <td>EB52</td>
  <td>HeartFill</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/eb55.png" alt="EthernetError" /></td>
  <td>EB55</td>
  <td>EthernetError</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/eb56.png" alt="EthernetWarning" /></td>
  <td>EB56</td>
  <td>EthernetWarning</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/eb57.png" alt="StatusConnecting1" /></td>
  <td>EB57</td>
  <td>StatusConnecting1</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/eb58.png" alt="StatusConnecting2" /></td>
  <td>EB58</td>
  <td>StatusConnecting2</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/eb59.png" alt="StatusUnsecure" /></td>
  <td>EB59</td>
  <td>StatusUnsecure</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/eb5a.png" alt="WifiError0" /></td>
  <td>EB5A</td>
  <td>WifiError0</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/eb5b.png" alt="WifiError1" /></td>
  <td>EB5B</td>
  <td>WifiError1</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/eb5c.png" alt="WifiError2" /></td>
  <td>EB5C</td>
  <td>WifiError2</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/eb5d.png" alt="WifiError3" /></td>
  <td>EB5D</td>
  <td>WifiError3</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/eb5e.png" alt="WifiError4" /></td>
  <td>EB5E</td>
  <td>WifiError4</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/eb5f.png" alt="WifiWarning0" /></td>
  <td>EB5F</td>
  <td>WifiWarning0</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/eb60.png" alt="WifiWarning1" /></td>
  <td>EB60</td>
  <td>WifiWarning1</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/eb61.png" alt="WifiWarning2" /></td>
  <td>EB61</td>
  <td>WifiWarning2</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/eb62.png" alt="WifiWarning3" /></td>
  <td>EB62</td>
  <td>WifiWarning3</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/eb63.png" alt="WifiWarning4" /></td>
  <td>EB63</td>
  <td>WifiWarning4</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/eb66.png" alt="Devices4" /></td>
  <td>EB66</td>
  <td>Devices4</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/eb67.png" alt="NUIIris" /></td>
  <td>EB67</td>
  <td>NUIIris</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/eb68.png" alt="NUIFace" /></td>
  <td>EB68</td>
  <td>NUIFace</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/eb7e.png" alt="EditMirrored" /></td>
  <td>EB7E</td>
  <td>EditMirrored</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/eb82.png" alt="NUIFPStartSlideHand" /></td>
  <td>EB82</td>
  <td>NUIFPStartSlideHand</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/eb83.png" alt="NUIFPStartSlideAction" /></td>
  <td>EB83</td>
  <td>NUIFPStartSlideAction</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/eb84.png" alt="NUIFPContinueSlideHand" /></td>
  <td>EB84</td>
  <td>NUIFPContinueSlideHand</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/eb85.png" alt="NUIFPContinueSlideAction" /></td>
  <td>EB85</td>
  <td>NUIFPContinueSlideAction</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/eb86.png" alt="NUIFPRollRightHand" /></td>
  <td>EB86</td>
  <td>NUIFPRollRightHand</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/eb87.png" alt="NUIFPRollRightHandAction" /></td>
  <td>EB87</td>
  <td>NUIFPRollRightHandAction</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/eb88.png" alt="NUIFPRollLeftHand" /></td>
  <td>EB88</td>
  <td>NUIFPRollLeftHand</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/eb89.png" alt="NUIFPRollLeftAction" /></td>
  <td>EB89</td>
  <td>NUIFPRollLeftAction</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/eb8a.png" alt="NUIFPPressHand" /></td>
  <td>EB8A</td>
  <td>NUIFPPressHand</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/eb8b.png" alt="NUIFPPressAction" /></td>
  <td>EB8B</td>
  <td>NUIFPPressAction</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/eb8c.png" alt="NUIFPPressRepeatHand" /></td>
  <td>EB8C</td>
  <td>NUIFPPressRepeatHand</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/eb8d.png" alt="NUIFPPressRepeatAction" /></td>
  <td>EB8D</td>
  <td>NUIFPPressRepeatAction</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/eb90.png" alt="StatusErrorFull" /></td>
  <td>EB90</td>
  <td>StatusErrorFull</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/eb91.png" alt="MultitaskExpanded" /></td>
  <td>EB91</td>
  <td>MultitaskExpanded</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/eb95.png" alt="Certificate" /></td>
  <td>EB95</td>
  <td>Certificate</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/eb96.png" alt="BackSpaceQWERTYLg" /></td>
  <td>EB96</td>
  <td>BackSpaceQWERTYLg</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/eb97.png" alt="ReturnKeyLg" /></td>
  <td>EB97</td>
  <td>ReturnKeyLg</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/eb9d.png" alt="FastForward" /></td>
  <td>EB9D</td>
  <td>FastForward</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/eb9e.png" alt="Rewind" /></td>
  <td>EB9E</td>
  <td>Rewind</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/eb9f.png" alt="Photo2" /></td>
  <td>EB9F</td>
  <td>Photo2</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/eba0.png" alt="&nbsp;MobBattery0" /></td>
  <td>EBA0</td>
  <td>&nbsp;MobBattery0</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/eba1.png" alt="&nbsp;MobBattery1" /></td>
  <td>EBA1</td>
  <td>&nbsp;MobBattery1</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/eba2.png" alt="&nbsp;MobBattery2" /></td>
  <td>EBA2</td>
  <td>&nbsp;MobBattery2</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/eba3.png" alt="&nbsp;MobBattery3" /></td>
  <td>EBA3</td>
  <td>&nbsp;MobBattery3</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/eba4.png" alt="&nbsp;MobBattery4" /></td>
  <td>EBA4</td>
  <td>&nbsp;MobBattery4</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/eba5.png" alt="&nbsp;MobBattery5" /></td>
  <td>EBA5</td>
  <td>&nbsp;MobBattery5</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/eba6.png" alt="&nbsp;MobBattery6" /></td>
  <td>EBA6</td>
  <td>&nbsp;MobBattery6</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/eba7.png" alt="&nbsp;MobBattery7" /></td>
  <td>EBA7</td>
  <td>&nbsp;MobBattery7</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/eba8.png" alt="&nbsp;MobBattery8" /></td>
  <td>EBA8</td>
  <td>&nbsp;MobBattery8</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/eba9.png" alt="&nbsp;MobBattery9" /></td>
  <td>EBA9</td>
  <td>&nbsp;MobBattery9</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ebaa.png" alt="MobBattery10" /></td>
  <td>EBAA</td>
  <td>MobBattery10</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ebab.png" alt="&nbsp;MobBatteryCharging0" /></td>
  <td>EBAB</td>
  <td>&nbsp;MobBatteryCharging0</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ebac.png" alt="&nbsp;MobBatteryCharging1" /></td>
  <td>EBAC</td>
  <td>&nbsp;MobBatteryCharging1</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ebad.png" alt="&nbsp;MobBatteryCharging2" /></td>
  <td>EBAD</td>
  <td>&nbsp;MobBatteryCharging2</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ebae.png" alt="&nbsp;MobBatteryCharging3" /></td>
  <td>EBAE</td>
  <td>&nbsp;MobBatteryCharging3</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ebaf.png" alt="&nbsp;MobBatteryCharging4" /></td>
  <td>EBAF</td>
  <td>&nbsp;MobBatteryCharging4</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ebb0.png" alt="&nbsp;MobBatteryCharging5" /></td>
  <td>EBB0</td>
  <td>&nbsp;MobBatteryCharging5</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ebb1.png" alt="&nbsp;MobBatteryCharging6" /></td>
  <td>EBB1</td>
  <td>&nbsp;MobBatteryCharging6</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ebb2.png" alt="&nbsp;MobBatteryCharging7" /></td>
  <td>EBB2</td>
  <td>&nbsp;MobBatteryCharging7</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ebb3.png" alt="&nbsp;MobBatteryCharging8" /></td>
  <td>EBB3</td>
  <td>&nbsp;MobBatteryCharging8</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ebb4.png" alt="&nbsp;MobBatteryCharging9" /></td>
  <td>EBB4</td>
  <td>&nbsp;MobBatteryCharging9</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ebb5.png" alt="&nbsp;MobBatteryCharging10" /></td>
  <td>EBB5</td>
  <td>&nbsp;MobBatteryCharging10</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ebb6.png" alt="&nbsp;MobBatterySaver0" /></td>
  <td>EBB6</td>
  <td>&nbsp;MobBatterySaver0</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ebb7.png" alt="&nbsp;MobBatterySaver1" /></td>
  <td>EBB7</td>
  <td>&nbsp;MobBatterySaver1</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ebb8.png" alt="&nbsp;MobBatterySaver2" /></td>
  <td>EBB8</td>
  <td>&nbsp;MobBatterySaver2</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ebb9.png" alt="&nbsp;MobBatterySaver3" /></td>
  <td>EBB9</td>
  <td>&nbsp;MobBatterySaver3</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ebba.png" alt="&nbsp;MobBatterySaver4" /></td>
  <td>EBBA</td>
  <td>&nbsp;MobBatterySaver4</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ebbb.png" alt="&nbsp;MobBatterySaver5" /></td>
  <td>EBBB</td>
  <td>&nbsp;MobBatterySaver5</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ebbc.png" alt="&nbsp;MobBatterySaver6" /></td>
  <td>EBBC</td>
  <td>&nbsp;MobBatterySaver6</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ebbd.png" alt="&nbsp;MobBatterySaver7" /></td>
  <td>EBBD</td>
  <td>&nbsp;MobBatterySaver7</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ebbe.png" alt="&nbsp;MobBatterySaver8" /></td>
  <td>EBBE</td>
  <td>&nbsp;MobBatterySaver8</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ebbf.png" alt="&nbsp;MobBatterySaver9" /></td>
  <td>EBBF</td>
  <td>&nbsp;MobBatterySaver9</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ebc0.png" alt="&nbsp;MobBatterySaver10" /></td>
  <td>EBC0</td>
  <td>&nbsp;MobBatterySaver10</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ebc3.png" alt="DictionaryCloud" /></td>
  <td>EBC3</td>
  <td>DictionaryCloud</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ebc4.png" alt="ResetDrive" /></td>
  <td>EBC4</td>
  <td>ResetDrive</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ebc5.png" alt="VolumeBars" /></td>
  <td>EBC5</td>
  <td>VolumeBars</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ebc6.png" alt="Project" /></td>
  <td>EBC6</td>
  <td>Project</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ebd2.png" alt="AdjustHologram" /></td>
  <td>EBD2</td>
  <td>AdjustHologram</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ebd4.png" alt="WifiCallBars" /></td>
  <td>EBD4</td>
  <td>WifiCallBars</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ebd5.png" alt="WifiCall0" /></td>
  <td>EBD5</td>
  <td>WifiCall0</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ebd6.png" alt="WifiCall1" /></td>
  <td>EBD6</td>
  <td>WifiCall1</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ebd7.png" alt="WifiCall2" /></td>
  <td>EBD7</td>
  <td>WifiCall2</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ebd8.png" alt="WifiCall3" /></td>
  <td>EBD8</td>
  <td>WifiCall3</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ebd9.png" alt="WifiCall4" /></td>
  <td>EBD9</td>
  <td>WifiCall4</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ebde.png" alt="DeviceDiscovery" /></td>
  <td>EBDE</td>
  <td>DeviceDiscovery</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ebe6.png" alt="WindDirection" /></td>
  <td>EBE6</td>
  <td>WindDirection</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ebe7.png" alt="RightArrowKeyTime0" /></td>
  <td>EBE7</td>
  <td>RightArrowKeyTime0</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ebfc.png" alt="TabletMode" /></td>
  <td>EBFC</td>
  <td>TabletMode</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ebfd.png" alt="StatusCircleLeft" /></td>
  <td>EBFD</td>
  <td>StatusCircleLeft</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ebfe.png" alt="StatusTriangleLeft" /></td>
  <td>EBFE</td>
  <td>StatusTriangleLeft</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ebff.png" alt="StatusErrorLeft" /></td>
  <td>EBFF</td>
  <td>StatusErrorLeft</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ec00.png" alt="StatusWarningLeft" /></td>
  <td>EC00</td>
  <td>StatusWarningLeft</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ec02.png" alt="MobBatteryUnknown" /></td>
  <td>EC02</td>
  <td>MobBatteryUnknown</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ec05.png" alt="NetworkTower" /></td>
  <td>EC05</td>
  <td>NetworkTower</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ec06.png" alt="CityNext" /></td>
  <td>EC06</td>
  <td>CityNext</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ec07.png" alt="CityNext2" /></td>
  <td>EC07</td>
  <td>CityNext2</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ec08.png" alt="Courthouse" /></td>
  <td>EC08</td>
  <td>Courthouse</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ec09.png" alt="Groceries" /></td>
  <td>EC09</td>
  <td>Groceries</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ec0a.png" alt="Sustainable" /></td>
  <td>EC0A</td>
  <td>Sustainable</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ec0b.png" alt="BuildingEnergy" /></td>
  <td>EC0B</td>
  <td>BuildingEnergy</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ec11.png" alt="ToggleFilled" /></td>
  <td>EC11</td>
  <td>ToggleFilled</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ec12.png" alt="ToggleBorder" /></td>
  <td>EC12</td>
  <td>ToggleBorder</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ec13.png" alt="SliderThumb" /></td>
  <td>EC13</td>
  <td>SliderThumb</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ec14.png" alt="ToggleThumb" /></td>
  <td>EC14</td>
  <td>ToggleThumb</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ec15.png" alt="MiracastLogoSmall" /></td>
  <td>EC15</td>
  <td>MiracastLogoSmall</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ec16.png" alt="MiracastLogoLarge" /></td>
  <td>EC16</td>
  <td>MiracastLogoLarge</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ec19.png" alt="PLAP" /></td>
  <td>EC19</td>
  <td>PLAP</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ec1b.png" alt="Badge" /></td>
  <td>EC1B</td>
  <td>Badge</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ec1e.png" alt="SignalRoaming" /></td>
  <td>EC1E</td>
  <td>SignalRoaming</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ec20.png" alt="MobileLocked" /></td>
  <td>EC20</td>
  <td>MobileLocked</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ec24.png" alt="InsiderHubApp" /></td>
  <td>EC24</td>
  <td>InsiderHubApp</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ec25.png" alt="PersonalFolder" /></td>
  <td>EC25</td>
  <td>PersonalFolder</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ec26.png" alt="HomeGroup" /></td>
  <td>EC26</td>
  <td>HomeGroup</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ec27.png" alt="MyNetwork" /></td>
  <td>EC27</td>
  <td>MyNetwork</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ec31.png" alt="KeyboardFull" /></td>
  <td>EC31</td>
  <td>KeyboardFull</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ec37.png" alt="MobSignal1" /></td>
  <td>EC37</td>
  <td>MobSignal1</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ec38.png" alt="MobSignal2" /></td>
  <td>EC38</td>
  <td>MobSignal2</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ec39.png" alt="MobSignal3" /></td>
  <td>EC39</td>
  <td>MobSignal3</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ec3a.png" alt="MobSignal4" /></td>
  <td>EC3A</td>
  <td>MobSignal4</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ec3b.png" alt="MobSignal5" /></td>
  <td>EC3B</td>
  <td>MobSignal5</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ec3c.png" alt="MobWifi1" /></td>
  <td>EC3C</td>
  <td>MobWifi1</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ec3d.png" alt="MobWifi2" /></td>
  <td>EC3D</td>
  <td>MobWifi2</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ec3e.png" alt="MobWifi3" /></td>
  <td>EC3E</td>
  <td>MobWifi3</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ec3f.png" alt="MobWifi4" /></td>
  <td>EC3F</td>
  <td>MobWifi4</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ec40.png" alt="MobAirplane" /></td>
  <td>EC40</td>
  <td>MobAirplane</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ec41.png" alt="MobBluetooth" /></td>
  <td>EC41</td>
  <td>MobBluetooth</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ec42.png" alt="MobActionCenter" /></td>
  <td>EC42</td>
  <td>MobActionCenter</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ec43.png" alt="MobLocation" /></td>
  <td>EC43</td>
  <td>MobLocation</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ec44.png" alt="MobWifiHotspot" /></td>
  <td>EC44</td>
  <td>MobWifiHotspot</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ec45.png" alt="LanguageJpn" /></td>
  <td>EC45</td>
  <td>LanguageJpn</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ec46.png" alt="MobQuietHours" /></td>
  <td>EC46</td>
  <td>MobQuietHours</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ec47.png" alt="MobDrivingMode" /></td>
  <td>EC47</td>
  <td>MobDrivingMode</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ec48.png" alt="SpeedOff" /></td>
  <td>EC48</td>
  <td>SpeedOff</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ec49.png" alt="SpeedMedium" /></td>
  <td>EC49</td>
  <td>SpeedMedium</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ec4a.png" alt="SpeedHigh" /></td>
  <td>EC4A</td>
  <td>SpeedHigh</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ec4e.png" alt="ThisPC" /></td>
  <td>EC4E</td>
  <td>ThisPC</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ec4f.png" alt="MusicNote" /></td>
  <td>EC4F</td>
  <td>MusicNote</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ec50.png" alt="FileExplorer" /></td>
  <td>EC50</td>
  <td>FileExplorer</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ec51.png" alt="FileExplorerApp" /></td>
  <td>EC51</td>
  <td>FileExplorerApp</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ec52.png" alt="LeftArrowKeyTime0" /></td>
  <td>EC52</td>
  <td>LeftArrowKeyTime0</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ec54.png" alt="MicOff" /></td>
  <td>EC54</td>
  <td>MicOff</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ec55.png" alt="MicSleep" /></td>
  <td>EC55</td>
  <td>MicSleep</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ec56.png" alt="MicError" /></td>
  <td>EC56</td>
  <td>MicError</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ec57.png" alt="PlaybackRate1x" /></td>
  <td>EC57</td>
  <td>PlaybackRate1x</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ec58.png" alt="PlaybackRateOther" /></td>
  <td>EC58</td>
  <td>PlaybackRateOther</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ec59.png" alt="CashDrawer" /></td>
  <td>EC59</td>
  <td>CashDrawer</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ec5a.png" alt="BarcodeScanner" /></td>
  <td>EC5A</td>
  <td>BarcodeScanner</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ec5b.png" alt="ReceiptPrinter" /></td>
  <td>EC5B</td>
  <td>ReceiptPrinter</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ec5c.png" alt="MagStripeReader" /></td>
  <td>EC5C</td>
  <td>MagStripeReader</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ec61.png" alt="CompletedSolid" /></td>
  <td>EC61</td>
  <td>CompletedSolid</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ec64.png" alt="CompanionApp" /></td>
  <td>EC64</td>
  <td>CompanionApp</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ec6d.png" alt="SwipeRevealArt" /></td>
  <td>EC6D</td>
  <td>SwipeRevealArt</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ec71.png" alt="MicOn" /></td>
  <td>EC71</td>
  <td>MicOn</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ec72.png" alt="MicClipping" /></td>
  <td>EC72</td>
  <td>MicClipping</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ec74.png" alt="TabletSelected" /></td>
  <td>EC74</td>
  <td>TabletSelected</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ec75.png" alt="MobileSelected" /></td>
  <td>EC75</td>
  <td>MobileSelected</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ec76.png" alt="LaptopSelected" /></td>
  <td>EC76</td>
  <td>LaptopSelected</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ec77.png" alt="TVMonitorSelected" /></td>
  <td>EC77</td>
  <td>TVMonitorSelected</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ec7a.png" alt="DeveloperTools" /></td>
  <td>EC7A</td>
  <td>DeveloperTools</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ec7e.png" alt="MobCallForwarding" /></td>
  <td>EC7E</td>
  <td>MobCallForwarding</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ec7f.png" alt="MobCallForwardingMirrored" /></td>
  <td>EC7F</td>
  <td>MobCallForwardingMirrored</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ec80.png" alt="BodyCam" /></td>
  <td>EC80</td>
  <td>BodyCam</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ec81.png" alt="PoliceCar" /></td>
  <td>EC81</td>
  <td>PoliceCar</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ec87.png" alt="Draw" /></td>
  <td>EC87</td>
  <td>Draw</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ec88.png" alt="DrawSolid" /></td>
  <td>EC88</td>
  <td>DrawSolid</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ec8a.png" alt="LowerBrightness" /></td>
  <td>EC8A</td>
  <td>LowerBrightness</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ec8f.png" alt="ScrollUpDown" /></td>
  <td>EC8F</td>
  <td>ScrollUpDown</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ec92.png" alt="DateTime" /></td>
  <td>EC92</td>
  <td>DateTime</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/eca5.png" alt="Tiles" /></td>
  <td>ECA5</td>
  <td>Tiles</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/eca7.png" alt="PartyLeader" /></td>
  <td>ECA7</td>
  <td>PartyLeader</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ecaa.png" alt="AppIconDefault" /></td>
  <td>ECAA</td>
  <td>AppIconDefault</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ecc4.png" alt="AddSurfaceHub" /></td>
  <td>ECC4</td>
  <td>AddSurfaceHub</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ecc5.png" alt="DevUpdate" /></td>
  <td>ECC5</td>
  <td>DevUpdate</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ecc6.png" alt="Unit" /></td>
  <td>ECC6</td>
  <td>Unit</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ecc8.png" alt="AddTo" /></td>
  <td>ECC8</td>
  <td>AddTo</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ecc9.png" alt="RemoveFrom" /></td>
  <td>ECC9</td>
  <td>RemoveFrom</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ecca.png" alt="RadioBtnOff" /></td>
  <td>ECCA</td>
  <td>RadioBtnOff</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/eccb.png" alt="RadioBtnOn" /></td>
  <td>ECCB</td>
  <td>RadioBtnOn</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/eccc.png" alt="RadioBullet2" /></td>
  <td>ECCC</td>
  <td>RadioBullet2</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/eccd.png" alt="ExploreContent" /></td>
  <td>ECCD</td>
  <td>ExploreContent</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ece7.png" alt="ScrollMode" /></td>
  <td>ECE7</td>
  <td>ScrollMode</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ece8.png" alt="ZoomMode" /></td>
  <td>ECE8</td>
  <td>ZoomMode</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ece9.png" alt="PanMode" /></td>
  <td>ECE9</td>
  <td>PanMode</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ecf0.png" alt="WiredUSB&nbsp;" /></td>
  <td>ECF0</td>
  <td>WiredUSB&nbsp;</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ecf1.png" alt="WirelessUSB" /></td>
  <td>ECF1</td>
  <td>WirelessUSB</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ecf3.png" alt="USBSafeConnect" /></td>
  <td>ECF3</td>
  <td>USBSafeConnect</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ed0c.png" alt="ActionCenterNotificationMirrored" /></td>
  <td>ED0C</td>
  <td>ActionCenterNotificationMirrored</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ed0d.png" alt="ActionCenterMirrored" /></td>
  <td>ED0D</td>
  <td>ActionCenterMirrored</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ed10.png" alt="ResetDevice" /></td>
  <td>ED10</td>
  <td>ResetDevice</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ed15.png" alt="Feedback" /></td>
  <td>ED15</td>
  <td>Feedback</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ed1e.png" alt="Subtitles" /></td>
  <td>ED1E</td>
  <td>Subtitles</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ed1f.png" alt="SubtitlesAudio" /></td>
  <td>ED1F</td>
  <td>SubtitlesAudio</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ed28.png" alt="CalendarMirrored" /></td>
  <td>ED28</td>
  <td>CalendarMirrored</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ed2a.png" alt="eSIM" /></td>
  <td>ED2A</td>
  <td>eSIM</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ed2b.png" alt="eSIMNoProfile" /></td>
  <td>ED2B</td>
  <td>eSIMNoProfile</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ed2c.png" alt="eSIMLocked" /></td>
  <td>ED2C</td>
  <td>eSIMLocked</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ed2d.png" alt="eSIMBusy" /></td>
  <td>ED2D</td>
  <td>eSIMBusy</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ed2e.png" alt="SignalError" /></td>
  <td>ED2E</td>
  <td>SignalError</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ed2f.png" alt="StreamingEnterprise" /></td>
  <td>ED2F</td>
  <td>StreamingEnterprise</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ed30.png" alt="Headphone0" /></td>
  <td>ED30</td>
  <td>Headphone0</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ed31.png" alt="Headphone1" /></td>
  <td>ED31</td>
  <td>Headphone1</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ed32.png" alt="Headphone2" /></td>
  <td>ED32</td>
  <td>Headphone2</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ed33.png" alt="Headphone3" /></td>
  <td>ED33</td>
  <td>Headphone3</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ed39.png" alt="KeyboardBrightness" /></td>
  <td>ED39</td>
  <td>KeyboardBrightness</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ed3a.png" alt="KeyboardLowerBrightness" /></td>
  <td>ED3A</td>
  <td>KeyboardLowerBrightness</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ed3c.png" alt="SkipBack10" /></td>
  <td>ED3C</td>
  <td>SkipBack10</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ed3d.png" alt="SkipForward30" /></td>
  <td>ED3D</td>
  <td>SkipForward30</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ed41.png" alt="TreeFolderFolder" /></td>
  <td>ED41</td>
  <td>TreeFolderFolder</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ed42.png" alt="TreeFolderFolderFill" /></td>
  <td>ED42</td>
  <td>TreeFolderFolderFill</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ed43.png" alt="TreeFolderFolderOpen" /></td>
  <td>ED43</td>
  <td>TreeFolderFolderOpen</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ed44.png" alt="TreeFolderFolderOpenFill" /></td>
  <td>ED44</td>
  <td>TreeFolderFolderOpenFill</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ed47.png" alt="MultimediaDMP" /></td>
  <td>ED47</td>
  <td>MultimediaDMP</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ed4c.png" alt="KeyboardOneHanded" /></td>
  <td>ED4C</td>
  <td>KeyboardOneHanded</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ed4d.png" alt="Narrator" /></td>
  <td>ED4D</td>
  <td>Narrator</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ed53.png" alt="EmojiTabPeople" /></td>
  <td>ED53</td>
  <td>EmojiTabPeople</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ed54.png" alt="EmojiTabSmilesAnimals" /></td>
  <td>ED54</td>
  <td>EmojiTabSmilesAnimals</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ed55.png" alt="EmojiTabCelebrationObjects" /></td>
  <td>ED55</td>
  <td>EmojiTabCelebrationObjects</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ed56.png" alt="EmojiTabFoodPlants" /></td>
  <td>ED56</td>
  <td>EmojiTabFoodPlants</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ed57.png" alt="EmojiTabTransitPlaces" /></td>
  <td>ED57</td>
  <td>EmojiTabTransitPlaces</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ed58.png" alt="EmojiTabSymbols" /></td>
  <td>ED58</td>
  <td>EmojiTabSymbols</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ed59.png" alt="EmojiTabTextSmiles" /></td>
  <td>ED59</td>
  <td>EmojiTabTextSmiles</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ed5a.png" alt="EmojiTabFavorites" /></td>
  <td>ED5A</td>
  <td>EmojiTabFavorites</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ed5b.png" alt="EmojiSwatch" /></td>
  <td>ED5B</td>
  <td>EmojiSwatch</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ed5c.png" alt="ConnectApp" /></td>
  <td>ED5C</td>
  <td>ConnectApp</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ed5d.png" alt="CompanionDeviceFramework" /></td>
  <td>ED5D</td>
  <td>CompanionDeviceFramework</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ed5e.png" alt="Ruler" /></td>
  <td>ED5E</td>
  <td>Ruler</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ed5f.png" alt="FingerInking" /></td>
  <td>ED5F</td>
  <td>FingerInking</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ed60.png" alt="StrokeErase" /></td>
  <td>ED60</td>
  <td>StrokeErase</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ed61.png" alt="PointErase" /></td>
  <td>ED61</td>
  <td>PointErase</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ed62.png" alt="ClearAllInk" /></td>
  <td>ED62</td>
  <td>ClearAllInk</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ed63.png" alt="Pencil" /></td>
  <td>ED63</td>
  <td>Pencil</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ed64.png" alt="Marker" /></td>
  <td>ED64</td>
  <td>Marker</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ed65.png" alt="InkingCaret" /></td>
  <td>ED65</td>
  <td>InkingCaret</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ed66.png" alt="InkingColorOutline" /></td>
  <td>ED66</td>
  <td>InkingColorOutline</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ed67.png" alt="InkingColorFill" /></td>
  <td>ED67</td>
  <td>InkingColorFill</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/eda2.png" alt="HardDrive" /></td>
  <td>EDA2</td>
  <td>HardDrive</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/eda3.png" alt="NetworkAdapter" /></td>
  <td>EDA3</td>
  <td>NetworkAdapter</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/eda4.png" alt="Touchscreen" /></td>
  <td>EDA4</td>
  <td>Touchscreen</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/eda5.png" alt="NetworkPrinter" /></td>
  <td>EDA5</td>
  <td>NetworkPrinter</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/eda6.png" alt="CloudPrinter" /></td>
  <td>EDA6</td>
  <td>CloudPrinter</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/eda7.png" alt="KeyboardShortcut" /></td>
  <td>EDA7</td>
  <td>KeyboardShortcut</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/eda8.png" alt="BrushSize" /></td>
  <td>EDA8</td>
  <td>BrushSize</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/eda9.png" alt="NarratorForward" /></td>
  <td>EDA9</td>
  <td>NarratorForward</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/edaa.png" alt="NarratorForwardMirrored" /></td>
  <td>EDAA</td>
  <td>NarratorForwardMirrored</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/edab.png" alt="SyncBadge12" /></td>
  <td>EDAB</td>
  <td>SyncBadge12</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/edac.png" alt="RingerBadge12" /></td>
  <td>EDAC</td>
  <td>RingerBadge12</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/edad.png" alt="AsteriskBadge12" /></td>
  <td>EDAD</td>
  <td>AsteriskBadge12</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/edae.png" alt="ErrorBadge12" /></td>
  <td>EDAE</td>
  <td>ErrorBadge12</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/edaf.png" alt="CircleRingBadge12" /></td>
  <td>EDAF</td>
  <td>CircleRingBadge12</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/edb0.png" alt="CircleFillBadge12" /></td>
  <td>EDB0</td>
  <td>CircleFillBadge12</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/edb1.png" alt="ImportantBadge12" /></td>
  <td>EDB1</td>
  <td>ImportantBadge12</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/edb3.png" alt="MailBadge12" /></td>
  <td>EDB3</td>
  <td>MailBadge12</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/edb4.png" alt="PauseBadge12" /></td>
  <td>EDB4</td>
  <td>PauseBadge12</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/edb5.png" alt="PlayBadge12" /></td>
  <td>EDB5</td>
  <td>PlayBadge12</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/edc6.png" alt="PenWorkspace" /></td>
  <td>EDC6</td>
  <td>PenWorkspace</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ede1.png" alt="Export" /></td>
  <td>EDE1</td>
  <td>Export</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ede2.png" alt="ExportMirrored" /></td>
  <td>EDE2</td>
  <td>ExportMirrored</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/edfb.png" alt="CaligraphyPen" /></td>
  <td>EDFB</td>
  <td>CaligraphyPen</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ee35.png" alt="ReplyMirrored" /></td>
  <td>EE35</td>
  <td>ReplyMirrored</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ee3f.png" alt="LockscreenDesktop" /></td>
  <td>EE3F</td>
  <td>LockscreenDesktop</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ee40.png" alt="Multitask16" /></td>
  <td>EE40</td>
  <td>Multitask16</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ee4a.png" alt="Play36" /></td>
  <td>EE4A</td>
  <td>Play36</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ee56.png" alt="PenPalette" /></td>
  <td>EE56</td>
  <td>PenPalette</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ee57.png" alt="GuestUser" /></td>
  <td>EE57</td>
  <td>GuestUser</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ee63.png" alt="SettingsBattery" /></td>
  <td>EE63</td>
  <td>SettingsBattery</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ee64.png" alt="TaskbarPhone" /></td>
  <td>EE64</td>
  <td>TaskbarPhone</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ee65.png" alt="LockScreenGlance" /></td>
  <td>EE65</td>
  <td>LockScreenGlance</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ee71.png" alt="ImageExport" /></td>
  <td>EE71</td>
  <td>ImageExport</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ee77.png" alt="WifiEthernet" /></td>
  <td>EE77</td>
  <td>WifiEthernet</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ee79.png" alt="ActionCenterQuiet" /></td>
  <td>EE79</td>
  <td>ActionCenterQuiet</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ee7a.png" alt="ActionCenterQuietNotification" /></td>
  <td>EE7A</td>
  <td>ActionCenterQuietNotification</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ee92.png" alt="TrackersMirrored" /></td>
  <td>EE92</td>
  <td>TrackersMirrored</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ee93.png" alt="DateTimeMirrored" /></td>
  <td>EE93</td>
  <td>DateTimeMirrored</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ee94.png" alt="Wheel" /></td>
  <td>EE94</td>
  <td>Wheel</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ef15.png" alt="PenWorkspaceMirrored" /></td>
  <td>EF15</td>
  <td>PenWorkspaceMirrored</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ef16.png" alt="PenPaletteMirrored" /></td>
  <td>EF16</td>
  <td>PenPaletteMirrored</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ef17.png" alt="StrokeEraseMirrored" /></td>
  <td>EF17</td>
  <td>StrokeEraseMirrored</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ef18.png" alt="PointEraseMirrored" /></td>
  <td>EF18</td>
  <td>PointEraseMirrored</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ef19.png" alt="ClearAllInkMirrored" /></td>
  <td>EF19</td>
  <td>ClearAllInkMirrored</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ef1f.png" alt="BackgroundToggle" /></td>
  <td>EF1F</td>
  <td>BackgroundToggle</td>
 </tr>
 <tr><td><img src="images/segoe-mdl/ef20.png" alt="Marquee" /></td>
  <td>EF20</td>
  <td>Marquee</td>
 </tr>
<tr><td><img src="images/segoe-mdl/f003.png" alt="Relationship" /></td><td>F003</td><td>Relationship</td></tr>
<tr><td><img src="images/segoe-mdl/f080.png" alt="DefaultAPN" /></td><td>F080</td><td>DefaultAPN</td></tr>
<tr><td><img src="images/segoe-mdl/f081.png" alt="UserAPN " /></td><td>F081</td><td>UserAPN </td></tr>
<tr><td><img src="images/segoe-mdl/f085.png" alt="DoublePinyin" /></td><td>F085</td><td>DoublePinyin</td></tr>
<tr><td><img src="images/segoe-mdl/f08c.png" alt="BlueLight" /></td><td>F08C</td><td>BlueLight</td></tr>
<tr><td><img src="images/segoe-mdl/f093.png" alt="ButtonA" /></td><td>F093</td><td>ButtonA</td></tr>
<tr><td><img src="images/segoe-mdl/f094.png" alt="ButtonB" /></td><td>F094</td><td>ButtonB</td></tr>
<tr><td><img src="images/segoe-mdl/f095.png" alt="ButtonY" /></td><td>F095</td><td>ButtonY</td></tr>
<tr><td><img src="images/segoe-mdl/f096.png" alt="ButtonX" /></td><td>F096</td><td>ButtonX</td></tr>
<tr><td><img src="images/segoe-mdl/f0ad.png" alt="ArrowUp8" /></td><td>F0AD</td><td>ArrowUp8</td></tr>
<tr><td><img src="images/segoe-mdl/f0ae.png" alt="ArrowDown8" /></td><td>F0AE</td><td>ArrowDown8</td></tr>
<tr><td><img src="images/segoe-mdl/f0af.png" alt="ArrowRight8" /></td><td>F0AF</td><td>ArrowRight8</td></tr>
<tr><td><img src="images/segoe-mdl/f0b0.png" alt="ArrowLeft8" /></td><td>F0B0</td><td>ArrowLeft8</td></tr>
<tr><td><img src="images/segoe-mdl/f0b2.png" alt="QuarentinedItems" /></td><td>F0B2</td><td>QuarentinedItems</td></tr>
<tr><td><img src="images/segoe-mdl/f0b3.png" alt="QuarentinedItemsMirrored" /></td><td>F0B3</td><td>QuarentinedItemsMirrored</td></tr>
<tr><td><img src="images/segoe-mdl/f0b4.png" alt="Protractor" /></td><td>F0B4</td><td>Protractor</td></tr>
<tr><td><img src="images/segoe-mdl/f0b5.png" alt="ChecklistMirrored" /></td><td>F0B5</td><td>ChecklistMirrored</td></tr>
<tr><td><img src="images/segoe-mdl/f0b6.png" alt="StatusCircle7" /></td><td>F0B6</td><td>StatusCircle7</td></tr>
<tr><td><img src="images/segoe-mdl/f0b7.png" alt="StatusCheckmark7" /></td><td>F0B7</td><td>StatusCheckmark7</td></tr>
<tr><td><img src="images/segoe-mdl/f0b8.png" alt="StatusErrorCircle7" /></td><td>F0B8</td><td>StatusErrorCircle7</td></tr>
<tr><td><img src="images/segoe-mdl/f0b9.png" alt="Connected" /></td><td>F0B9</td><td>Connected</td></tr>
<tr><td><img src="images/segoe-mdl/f0c6.png" alt="PencilFill" /></td><td>F0C6</td><td>PencilFill</td></tr>
<tr><td><img src="images/segoe-mdl/f0c7.png" alt="CalligraphyFill" /></td><td>F0C7</td><td>CalligraphyFill</td></tr>
<tr><td><img src="images/segoe-mdl/f0ca.png" alt="QuarterStarLeft" /></td><td>F0CA</td><td>QuarterStarLeft</td></tr>
<tr><td><img src="images/segoe-mdl/f0cb.png" alt="QuarterStarRight" /></td><td>F0CB</td><td>QuarterStarRight</td></tr>
<tr><td><img src="images/segoe-mdl/f0cc.png" alt="ThreeQuarterStarLeft" /></td><td>F0CC</td><td>ThreeQuarterStarLeft</td></tr>
<tr><td><img src="images/segoe-mdl/f0cd.png" alt="ThreeQuarterStarRight" /></td><td>F0CD</td><td>ThreeQuarterStarRight</td></tr>
<tr><td><img src="images/segoe-mdl/f0ce.png" alt="QuietHoursBadge12" /></td><td>F0CE</td><td>QuietHoursBadge12</td></tr>
<tr><td><img src="images/segoe-mdl/f0d2.png" alt="BackMirrored" /></td><td>F0D2</td><td>BackMirrored</td></tr>
<tr><td><img src="images/segoe-mdl/f0d3.png" alt="ForwardMirrored" /></td><td>F0D3</td><td>ForwardMirrored</td></tr>
<tr><td><img src="images/segoe-mdl/f0d5.png" alt="ChromeBackContrast" /></td><td>F0D5</td><td>ChromeBackContrast</td></tr>
<tr><td><img src="images/segoe-mdl/f0d6.png" alt="ChromeBackContrastMirrored" /></td><td>F0D6</td><td>ChromeBackContrastMirrored</td></tr>
<tr><td><img src="images/segoe-mdl/f0d7.png" alt="ChromeBackToWindowContrast" /></td><td>F0D7</td><td>ChromeBackToWindowContrast</td></tr>
<tr><td><img src="images/segoe-mdl/f0d8.png" alt="ChromeFullScreenContrast" /></td><td>F0D8</td><td>ChromeFullScreenContrast</td></tr>
<tr><td><img src="images/segoe-mdl/f0e2.png" alt="GridView" /></td><td>F0E2</td><td>GridView</td></tr>
<tr><td><img src="images/segoe-mdl/f0e3.png" alt="ClipboardList" /></td><td>F0E3</td><td>ClipboardList</td></tr>
<tr><td><img src="images/segoe-mdl/f0e4.png" alt="ClipboardListMirrored" /></td><td>F0E4</td><td>ClipboardListMirrored</td></tr>
<tr><td><img src="images/segoe-mdl/f0e5.png" alt="OutlineQuarterStarLeft" /></td><td>F0E5</td><td>OutlineQuarterStarLeft</td></tr>
<tr><td><img src="images/segoe-mdl/f0e6.png" alt="OutlineQuarterStarRight" /></td><td>F0E6</td><td>OutlineQuarterStarRight</td></tr>
<tr><td><img src="images/segoe-mdl/f0e7.png" alt="OutlineHalfStarLeft" /></td><td>F0E7</td><td>OutlineHalfStarLeft</td></tr>
<tr><td><img src="images/segoe-mdl/f0e8.png" alt="OutlineHalfStarRight" /></td><td>F0E8</td><td>OutlineHalfStarRight</td></tr>
<tr><td><img src="images/segoe-mdl/f0e9.png" alt="OutlineThreeQuarterStarLeft" /></td><td>F0E9</td><td>OutlineThreeQuarterStarLeft</td></tr>
<tr><td><img src="images/segoe-mdl/f0ea.png" alt="OutlineThreeQuarterStarRight" /></td><td>F0EA</td><td>OutlineThreeQuarterStarRight</td></tr>
<tr><td><img src="images/segoe-mdl/f0eb.png" alt="SpatialVolume0" /></td><td>F0EB</td><td>SpatialVolume0</td></tr>
<tr><td><img src="images/segoe-mdl/f0ec.png" alt="SpatialVolume1" /></td><td>F0EC</td><td>SpatialVolume1</td></tr>
<tr><td><img src="images/segoe-mdl/f0ed.png" alt="SpatialVolume2" /></td><td>F0ED</td><td>SpatialVolume2</td></tr>
<tr><td><img src="images/segoe-mdl/f0ee.png" alt="SpatialVolume3" /></td><td>F0EE</td><td>SpatialVolume3</td></tr>
<tr><td><img src="images/segoe-mdl/f0f7.png" alt="OutlineStarLeftHalf" /></td><td>F0F7</td><td>OutlineStarLeftHalf</td></tr>
<tr><td><img src="images/segoe-mdl/f0f8.png" alt="OutlineStarRightHalf" /></td><td>F0F8</td><td>OutlineStarRightHalf</td></tr>
<tr><td><img src="images/segoe-mdl/f0f9.png" alt="ChromeAnnotateContrast" /></td><td>F0F9</td><td>ChromeAnnotateContrast</td></tr>
<tr><td><img src="images/segoe-mdl/f0fb.png" alt="DefenderBadge12" /></td><td>F0FB</td><td>DefenderBadge12</td></tr>
<tr><td><img src="images/segoe-mdl/f103.png" alt="DetachablePC" /></td><td>F103</td><td>DetachablePC</td></tr>
<tr><td><img src="images/segoe-mdl/f108.png" alt="LeftStick" /></td><td>F108</td><td>LeftStick</td></tr>
<tr><td><img src="images/segoe-mdl/f109.png" alt="RightStick" /></td><td>F109</td><td>RightStick</td></tr>
<tr><td><img src="images/segoe-mdl/f10a.png" alt="TriggerLeft" /></td><td>F10A</td><td>TriggerLeft</td></tr>
<tr><td><img src="images/segoe-mdl/f10b.png" alt="TriggerRight" /></td><td>F10B</td><td>TriggerRight</td></tr>
<tr><td><img src="images/segoe-mdl/f10c.png" alt="BumperLeft" /></td><td>F10C</td><td>BumperLeft</td></tr>
<tr><td><img src="images/segoe-mdl/f10d.png" alt="BumperRight" /></td><td>F10D</td><td>BumperRight</td></tr>
<tr><td><img src="images/segoe-mdl/f10e.png" alt="Dpad" /></td><td>F10E</td><td>Dpad</td></tr>
<tr><td><img src="images/segoe-mdl/f110.png" alt="EnglishPunctuation" /></td><td>F110</td><td>EnglishPunctuation</td></tr>
<tr><td><img src="images/segoe-mdl/f111.png" alt="ChinesePunctuation" /></td><td>F111</td><td>ChinesePunctuation</td></tr>
<tr><td><img src="images/segoe-mdl/f119.png" alt="HMD" /></td><td>F119</td><td>HMD</td></tr>
<tr><td><img src="images/segoe-mdl/f126.png" alt="PaginationDotOutline10" /></td><td>F126</td><td>PaginationDotOutline10</td></tr>
<tr><td><img src="images/segoe-mdl/f127.png" alt="PaginationDotSolid10" /></td><td>F127</td><td>PaginationDotSolid10</td></tr>
<tr><td><img src="images/segoe-mdl/f128.png" alt="StrokeErase2" /></td><td>F128</td><td>StrokeErase2</td></tr>
<tr><td><img src="images/segoe-mdl/f129.png" alt="SmallErase" /></td><td>F129</td><td>SmallErase</td></tr>
<tr><td><img src="images/segoe-mdl/f12a.png" alt="LargeErase" /></td><td>F12A</td><td>LargeErase</td></tr>
<tr><td><img src="images/segoe-mdl/f12b.png" alt="FolderHorizontal" /></td><td>F12B</td><td>FolderHorizontal</td></tr>
<tr><td><img src="images/segoe-mdl/f12e.png" alt="MicrophoneListening" /></td><td>F12E</td><td>MicrophoneListening</td></tr>
<tr><td><img src="images/segoe-mdl/f12f.png" alt="StatusExclamationCircle7 " /></td><td>F12F</td><td>StatusExclamationCircle7 </td></tr>
<tr><td><img src="images/segoe-mdl/f136.png" alt="StatusCircleOuter" /></td><td>F136</td><td>StatusCircleOuter</td></tr>
<tr><td><img src="images/segoe-mdl/f137.png" alt="StatusCircleInner" /></td><td>F137</td><td>StatusCircleInner</td></tr>
<tr><td><img src="images/segoe-mdl/f138.png" alt="StatusCircleRing" /></td><td>F138</td><td>StatusCircleRing</td></tr>
<tr><td><img src="images/segoe-mdl/f139.png" alt="StatusTriangleOuter" /></td><td>F139</td><td>StatusTriangleOuter</td></tr>
<tr><td><img src="images/segoe-mdl/f13a.png" alt="StatusTriangleInner" /></td><td>F13A</td><td>StatusTriangleInner</td></tr>
<tr><td><img src="images/segoe-mdl/f13b.png" alt="StatusTriangleExclamation" /></td><td>F13B</td><td>StatusTriangleExclamation</td></tr>
<tr><td><img src="images/segoe-mdl/f13c.png" alt="StatusCircleExclamation" /></td><td>F13C</td><td>StatusCircleExclamation</td></tr>
<tr><td><img src="images/segoe-mdl/f13d.png" alt="StatusCircleErrorX" /></td><td>F13D</td><td>StatusCircleErrorX</td></tr>
<tr><td><img src="images/segoe-mdl/f13e.png" alt="StatusCircleCheckmark" /></td><td>F13E</td><td>StatusCircleCheckmark</td></tr>
<tr><td><img src="images/segoe-mdl/f13f.png" alt="StatusCircleInfo" /></td><td>F13F</td><td>StatusCircleInfo</td></tr>
<tr><td><img src="images/segoe-mdl/f140.png" alt="StatusCircleBlock" /></td><td>F140</td><td>StatusCircleBlock</td></tr>
<tr><td><img src="images/segoe-mdl/f141.png" alt="StatusCircleBlock2" /></td><td>F141</td><td>StatusCircleBlock2</td></tr>
<tr><td><img src="images/segoe-mdl/f142.png" alt="StatusCircleQuestionMark" /></td><td>F142</td><td>StatusCircleQuestionMark</td></tr>
<tr><td><img src="images/segoe-mdl/f143.png" alt="StatusCircleSync" /></td><td>F143</td><td>StatusCircleSync</td></tr>
<tr><td><img src="images/segoe-mdl/f146.png" alt="Dial1" /></td><td>F146</td><td>Dial1</td></tr>
<tr><td><img src="images/segoe-mdl/f147.png" alt="Dial2" /></td><td>F147</td><td>Dial2</td></tr>
<tr><td><img src="images/segoe-mdl/f148.png" alt="Dial3" /></td><td>F148</td><td>Dial3</td></tr>
<tr><td><img src="images/segoe-mdl/f149.png" alt="Dial4" /></td><td>F149</td><td>Dial4</td></tr>
<tr><td><img src="images/segoe-mdl/f14a.png" alt="Dial5" /></td><td>F14A</td><td>Dial5</td></tr>
<tr><td><img src="images/segoe-mdl/f14b.png" alt="Dial6" /></td><td>F14B</td><td>Dial6</td></tr>
<tr><td><img src="images/segoe-mdl/f14c.png" alt="Dial7" /></td><td>F14C</td><td>Dial7</td></tr>
<tr><td><img src="images/segoe-mdl/f14d.png" alt="Dial8" /></td><td>F14D</td><td>Dial8</td></tr>
<tr><td><img src="images/segoe-mdl/f14e.png" alt="Dial9" /></td><td>F14E</td><td>Dial9</td></tr>
<tr><td><img src="images/segoe-mdl/f14f.png" alt="Dial10" /></td><td>F14F</td><td>Dial10</td></tr>
<tr><td><img src="images/segoe-mdl/f150.png" alt="Dial11" /></td><td>F150</td><td>Dial11</td></tr>
<tr><td><img src="images/segoe-mdl/f151.png" alt="Dial12" /></td><td>F151</td><td>Dial12</td></tr>
<tr><td><img src="images/segoe-mdl/f152.png" alt="Dial13" /></td><td>F152</td><td>Dial13</td></tr>
<tr><td><img src="images/segoe-mdl/f153.png" alt="Dial14" /></td><td>F153</td><td>Dial14</td></tr>
<tr><td><img src="images/segoe-mdl/f154.png" alt="Dial15" /></td><td>F154</td><td>Dial15</td></tr>
<tr><td><img src="images/segoe-mdl/f155.png" alt="Dial16" /></td><td>F155</td><td>Dial16</td></tr>
<tr><td><img src="images/segoe-mdl/f156.png" alt="DialShape1" /></td><td>F156</td><td>DialShape1</td></tr>
<tr><td><img src="images/segoe-mdl/f157.png" alt="DialShape2" /></td><td>F157</td><td>DialShape2</td></tr>
<tr><td><img src="images/segoe-mdl/f158.png" alt="DialShape3" /></td><td>F158</td><td>DialShape3</td></tr>
<tr><td><img src="images/segoe-mdl/f159.png" alt="DialShape4" /></td><td>F159</td><td>DialShape4</td></tr>
<tr><td><img src="images/segoe-mdl/f161.png" alt="TollSolid" /></td><td>F161</td><td>TollSolid</td></tr>
<tr><td><img src="images/segoe-mdl/f163.png" alt="TrafficCongestionSolid" /></td><td>F163</td><td>TrafficCongestionSolid</td></tr>
<tr><td><img src="images/segoe-mdl/f164.png" alt="ExploreContentSingle" /></td><td>F164</td><td>ExploreContentSingle</td></tr>
<tr><td><img src="images/segoe-mdl/f165.png" alt="CollapseContent" /></td><td>F165</td><td>CollapseContent</td></tr>
<tr><td><img src="images/segoe-mdl/f166.png" alt="CollapseContentSingle" /></td><td>F166</td><td>CollapseContentSingle</td></tr>
<tr><td><img src="images/segoe-mdl/f167.png" alt="InfoSolid" /></td><td>F167</td><td>InfoSolid</td></tr>
<tr><td><img src="images/segoe-mdl/f168.png" alt="GroupList" /></td><td>F168</td><td>GroupList</td></tr>
<tr><td><img src="images/segoe-mdl/f169.png" alt="CaretBottomRightSolidCenter8" /></td><td>F169</td><td>CaretBottomRightSolidCenter8</td></tr>
<tr><td><img src="images/segoe-mdl/f16a.png" alt="ProgressRingDots" /></td><td>F16A</td><td>ProgressRingDots</td></tr>
<tr><td><img src="images/segoe-mdl/f16b.png" alt="Checkbox14" /></td><td>F16B</td><td>Checkbox14</td></tr>
<tr><td><img src="images/segoe-mdl/f16c.png" alt="CheckboxComposite14" /></td><td>F16C</td><td>CheckboxComposite14</td></tr>
<tr><td><img src="images/segoe-mdl/f16d.png" alt="CheckboxIndeterminateCombo14" /></td><td>F16D</td><td>CheckboxIndeterminateCombo14</td></tr>
<tr><td><img src="images/segoe-mdl/f16e.png" alt="CheckboxIndeterminateCombo" /></td><td>F16E</td><td>CheckboxIndeterminateCombo</td></tr>
<tr><td><img src="images/segoe-mdl/f175.png" alt="StatusPause7" /></td><td>F175</td><td>StatusPause7</td></tr>
<tr><td><img src="images/segoe-mdl/f17f.png" alt="CharacterAppearance" /></td><td>F17F</td><td>CharacterAppearance</td></tr>
<tr><td><img src="images/segoe-mdl/f180.png" alt="Lexicon " /></td><td>F180</td><td>Lexicon </td></tr>
<tr><td><img src="images/segoe-mdl/f182.png" alt="ScreenTime" /></td><td>F182</td><td>ScreenTime</td></tr>
<tr><td><img src="images/segoe-mdl/f191.png" alt="HeadlessDevice" /></td><td>F191</td><td>HeadlessDevice</td></tr>
<tr><td><img src="images/segoe-mdl/f193.png" alt="NetworkSharing" /></td><td>F193</td><td>NetworkSharing</td></tr>
<tr><td><img src="images/segoe-mdl/f1ad.png" alt="WindowsInsider" /></td><td>F1AD</td><td>WindowsInsider</td></tr>
<tr><td><img src="images/segoe-mdl/f1cb.png" alt="ChromeSwitch" /></td><td>F1CB</td><td>ChromeSwitch</td></tr>
<tr><td><img src="images/segoe-mdl/f1cc.png" alt="ChromeSwitchContast" /></td><td>F1CC</td><td>ChromeSwitchContast</td></tr>
<tr><td><img src="images/segoe-mdl/f1d8.png" alt="StatusCheckmark" /></td><td>F1D8</td><td>StatusCheckmark</td></tr>
<tr><td><img src="images/segoe-mdl/f1d9.png" alt="StatusCheckmarkLeft" /></td><td>F1D9</td><td>StatusCheckmarkLeft</td></tr>
<tr><td><img src="images/segoe-mdl/f20c.png" alt="KeyboardLeftAligned" /></td><td>F20C</td><td>KeyboardLeftAligned</td></tr>
<tr><td><img src="images/segoe-mdl/f20d.png" alt="KeyboardRightAligned" /></td><td>F20D</td><td>KeyboardRightAligned</td></tr>
<tr><td><img src="images/segoe-mdl/f210.png" alt="KeyboardSettings" /></td><td>F210</td><td>KeyboardSettings</td></tr>
<tr><td><img src="images/segoe-mdl/f22c.png" alt="IOT" /></td><td>F22C</td><td>IOT</td></tr>
<tr><td><img src="images/segoe-mdl/f259.png" alt="ExploitProtectionSettings" /></td><td>F259</td><td>ExploitProtectionSettings</td></tr>
<tr><td><img src="images/segoe-mdl/f260.png" alt="KeyboardNarrow" /></td><td>F260</td><td>KeyboardNarrow</td></tr>
<tr><td><img src="images/segoe-mdl/f261.png" alt="Keyboard12Key" /></td><td>F261</td><td>Keyboard12Key</td></tr>
<tr><td><img src="images/segoe-mdl/f26b.png" alt="KeyboardDock" /></td><td>F26B</td><td>KeyboardDock</td></tr>
<tr><td><img src="images/segoe-mdl/f26c.png" alt="KeyboardUndock" /></td><td>F26C</td><td>KeyboardUndock</td></tr>
<tr><td><img src="images/segoe-mdl/f26d.png" alt="KeyboardLeftDock" /></td><td>F26D</td><td>KeyboardLeftDock</td></tr>
<tr><td><img src="images/segoe-mdl/f26e.png" alt="KeyboardRightDock" /></td><td>F26E</td><td>KeyboardRightDock</td></tr>
<tr><td><img src="images/segoe-mdl/f270.png" alt="Ear" /></td><td>F270</td><td>Ear</td></tr>
<tr><td><img src="images/segoe-mdl/f271.png" alt="PointerHand" /></td><td>F271</td><td>PointerHand</td></tr>
<tr><td><img src="images/segoe-mdl/f272.png" alt="Bullseye" /></td><td>F272</td><td>Bullseye</td></tr>
<tr><td><img src="images/segoe-mdl/f32a.png" alt="PassiveAuthentication" /></td><td>F32A</td><td>PassiveAuthentication</td></tr>
<tr><td><img src="images/segoe-mdl/f384.png" alt="NetworkOffline" /></td><td>F384</td><td>NetworkOffline</td></tr>
<tr><td><img src="images/segoe-mdl/f385.png" alt="NetworkConnected" /></td><td>F385</td><td>NetworkConnected</td></tr>
<tr><td><img src="images/segoe-mdl/f386.png" alt="NetworkConnectedCheckmark" /></td><td>F386</td><td>NetworkConnectedCheckmark</td></tr>


</table>



## <a name="related-articles"></a>Articles associés

* [Recommandations en matière d’icônes](../style/icons.md)
* [Énumération Symbol](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.Controls.Symbol)
* [Classe FontIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.fonticon)


