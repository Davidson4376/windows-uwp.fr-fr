---
author: Jwmsft
Description: "Le sélecteur de dates offre aux utilisateurs une méthode standard de sélection des valeurs des dates localisées à l’aide d’une entrée tactile, de la souris ou du clavier."
title: "Sélecteur de dates"
ms.assetid: d4a01425-4dee-4de3-9a05-3e85c3fc03cb
isNew: True
label: Date picker
template: detail.hbs
ms.author: jimwalk
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
pm-contact: kisai
design-contact: ksulliv
dev-contact: joyate
doc-status: Published
ms.openlocfilehash: 25c49681085946e145d4feb4c8f93d2908f7bcbd
ms.sourcegitcommit: 10d6736a0827fe813c3c6e8d26d67b20ff110f6c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/22/2017
---
# <a name="date-picker"></a>Sélecteur de dates

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 

Le sélecteur de dates offre aux utilisateurs une méthode standard de sélection des valeurs des dates localisées à l’aide d’une entrée tactile, de la souris ou du clavier. 

> **API importantes**: [classe DatePicker](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.datepicker.aspx), [propriété Date](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.datepicker.date.aspx)


## <a name="is-this-the-right-control"></a>Est-ce le contrôle approprié?
Utilisez un sélecteur de dates pour permettre à un utilisateur de choisir une date connue, par exemple, une date de naissance, où le contexte du calendrier n’est pas important.

Pour plus d’informations sur le choix du contrôle de date approprié, voir l’article [Contrôles de date et d’heure](date-and-time.md).

## <a name="examples"></a>Exemples

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



## <a name="related-articles"></a>Articles connexes

- [Contrôles de date et d’heure](date-and-time.md)
- [Sélecteur de dates du calendrier](calendar-date-picker.md)
- [Affichage Calendrier](calendar-view.md)
- [Sélecteur d’heure](time-picker.md)
