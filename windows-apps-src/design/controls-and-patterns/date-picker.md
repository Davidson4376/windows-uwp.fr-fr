---
Description: The date picker gives you a standardized way to let users pick a localized date value using touch, mouse, or keyboard input.
title: Sélecteur de dates
ms.assetid: d4a01425-4dee-4de3-9a05-3e85c3fc03cb
isNew: true
label: Date picker
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows10, uwp
pm-contact: kisai
design-contact: ksulliv
dev-contact: joyate
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 42b061c3c3836e69739c571f6ef47e05c286ea8e
ms.sourcegitcommit: b11f305dbf7649c4b68550b666487c77ea30d98f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/27/2018
ms.locfileid: "7837172"
---
# <a name="date-picker"></a>Sélecteur de dates

 

Le sélecteur de dates offre aux utilisateurs une méthode standard de sélection des valeurs des dates localisées à l’aide d’une entrée tactile, de la souris ou du clavier. 

> **API importantes**: [classe DatePicker](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.datepicker.aspx), [propriété Date](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.datepicker.date.aspx)


## <a name="is-this-the-right-control"></a>Est-ce le contrôle approprié?
Utilisez un sélecteur de dates pour permettre à un utilisateur de choisir une date connue, par exemple, une date de naissance, où le contexte du calendrier n’est pas important.

Pour plus d’informations sur le choix du contrôle de date approprié, voir l’article [Contrôles de date et d’heure](date-and-time.md).

## <a name="examples"></a>Exemples

<table>
<th align="left">Galerie de contrôles XAML<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si vous disposez de l'application <strong style="font-weight: semi-bold">Galerie de contrôles XAML</strong>, cliquez ici pour <a href="xamlcontrolsgallery:/item/DatePicker">ouvrir l’application et voir l'objet DatePicker en action</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenir l’application Galerie de contrôles XAML (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Obtenir le code source (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

Le point d’entrée affiche la date choisie, et lorsque l’utilisateur sélectionne ce point d’entrée, la surface du sélecteur s’agrandit à la verticale à partir du milieu pour que l’utilisateur effectue une sélection. Le sélecteur de dates se superpose aux autres éléments de l’interface utilisateur; il ne les ferme pas.

![Exemple de développement du sélecteur de date](images/controls_datepicker_expand.png)

## <a name="create-a-date-picker"></a>Créer un sélecteur de dates

Cet exemple montre comment créer un sélecteur de dates simple avec un en-tête.

```xaml
<DatePicker x:Name=birthDatePicker Header="Date of birth"/>
```

```csharp
DatePicker birthDatePicker = new DatePicker();
birthDatePicker.Header = "Date of birth";
```

Le sélecteur de dates qui en résulte se présente comme suit:

![Exemple de sélecteur de dates](images/date-picker-closed.png)

> **Remarque**&nbsp;&nbsp;Pour obtenir des informations importantes sur les valeurs de date, voir [Valeurs DateTime et Calendar](date-and-time.md#datetime-and-calendar-values) dans l’article Contrôles de date et d’heure.

## <a name="get-the-sample-code"></a>Obtenir l’exemple de code

- [Exemples de la Galerie de contrôles XAML](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) - Affichez tous les contrôles XAML dans un format interactif.

## <a name="related-articles"></a>Articles connexes

- [Contrôles de date et d’heure](date-and-time.md)
- [Sélecteur de dates du calendrier](calendar-date-picker.md)
- [Affichage Calendrier](calendar-view.md)
- [Sélecteur d’heure](time-picker.md)
