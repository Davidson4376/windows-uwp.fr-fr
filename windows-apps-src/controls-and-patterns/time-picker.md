---
author: Jwmsft
Description: "Le sélecteur d’heure offre aux utilisateurs une méthode standard de sélection d’une heure à l’aide d’une entrée tactile, de la souris ou du clavier."
title: "Sélecteur d’heure"
ms.assetid: 5124ecda-09e6-449e-9d4a-d969dca46aa3
label: Time picker
template: detail.hbs
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.openlocfilehash: 83ef423ce4e6fd57726879dd83a704c47cc43744
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="time-picker"></a>Sélecteur d’heure
<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 

Le sélecteur d’heure offre aux utilisateurs une méthode standard de sélection d’une heure à l’aide d’une entrée tactile, de la souris ou du clavier. 

<div class="important-apis" >
<b>API importantes</b><br/>
<ul>
<li>[**Classe TimePicker**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.timepicker.aspx)</li>
<li>[**Propriété Time**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.timepicker.time.aspx)</li>
</ul>
</div>

## <a name="is-this-the-right-control"></a>Est-ce le contrôle approprié?
Utilisez un sélecteur d’heure pour permettre à un utilisateur de sélectionner une valeur d’heure unique.

Pour plus d’informations sur le choix du contrôle approprié, voir l’article [Contrôles de date et d’heure](date-and-time.md).

## <a name="examples"></a>Exemples

Le point d’entrée affiche l’heure choisie, et lorsque l’utilisateur sélectionne ce point d’entrée, la surface du sélecteur s’agrandit à la verticale à partir du milieu pour que l’utilisateur puisse effectuer une sélection. Le sélecteur d’heure se superpose aux autres éléments de l’interface utilisateur; il ne les ferme pas.

![Exemple de développement du sélecteur d’heure](images/controls_timepicker_expand.png)

## <a name="create-a-time-picker"></a>Créer un sélecteur d’heure

Cet exemple indique comment créer un sélecteur d’heure simple avec un en-tête.

```xaml
<TimePicker x:Name=arrivalTimePicker Header="Arrival time"/>
```

```csharp
TimePicker arrivalTimePicker = new TimePicker();
arrivalTimePicker.Header = "Arrival time";
```

Le sélecteur d’heure qui en résulte se présente comme suit:

![Exemple de sélecteur d’heure](images/time-picker-closed.png)

> [!NOTE]
> Pour obtenir des informations importantes sur les valeurs de date et d’heure, voir [Valeurs DateTime et Calendar](date-and-time.md#datetime-and-calendar-values) dans l’article *Contrôles de date et d’heure*.



## <a name="related-topics"></a>Rubriques connexes

- [Contrôles de date et d’heure](date-and-time.md)
- [Sélecteur de dates du calendrier](calendar-date-picker.md)
- [Affichage Calendrier](calendar-view.md)
- [Sélecteur de dates](date-picker.md)
