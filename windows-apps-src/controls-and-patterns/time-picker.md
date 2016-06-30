---
author: Jwmsft
Description: "Le sélecteur d’heure offre aux utilisateurs une méthode standard de sélection d’une heure à l’aide d’une entrée tactile, de la souris ou du clavier."
title: "Sélecteur d’heure"
ms.assetid: 5124ecda-09e6-449e-9d4a-d969dca46aa3
label: Time picker
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: a4e9a90edd2aae9d2fd5d7bead948422d43dad59
ms.openlocfilehash: 5056a9f304ca21c977b9cc65b8ead007eccd4288

---

# Sélecteur d’heure

Le sélecteur d’heure offre aux utilisateurs une méthode standard de sélection d’une heure à l’aide d’une entrée tactile, de la souris ou du clavier. 

<span class="sidebar_heading" style="font-weight: bold;">API importantes</span>

-   [**Classe TimePicker**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.timepicker.aspx)
-   [**Propriété Time**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.timepicker.time.aspx)

## Est-ce le contrôle approprié ?
Utilisez un sélecteur d’heure pour permettre à un utilisateur de sélectionner une valeur d’heure unique.

Pour plus d’informations sur le choix du contrôle approprié, voir l’article [Contrôles de date et d’heure](date-and-time.md).

## Exemples

Le point d’entrée affiche l’heure choisie, et lorsque l’utilisateur sélectionne ce point d’entrée, la surface du sélecteur s’agrandit à la verticale à partir du milieu pour que l’utilisateur puisse effectuer une sélection. Le sélecteur d’heure se superpose aux autres éléments de l’interface utilisateur ; il ne les ferme pas.

![Exemple de développement du sélecteur d’heure](images/controls_timepicker_expand.png)

## Créer un sélecteur d’heure

Cet exemple indique comment créer un sélecteur d’heure simple avec un en-tête.

```xaml
<TimePicker x:Name=arrivalTimePicker Header="Arrival time"/>
```

```csharp
TimePicker arrivalTimePicker = new TimePicker();
arrivalTimePicker.Header = "Arrival time";
```

Le sélecteur d’heure qui en résulte se présente comme suit :

![Exemple de sélecteur d’heure](images/time-picker-closed.png)

> **Remarque** &nbsp;&nbsp;Pour obtenir des informations importantes sur les valeurs de date et d’heure, consultez [Valeurs DateTime et Calendar](date-and-time.md#datetime-and-calendar-values) dans l’article *Contrôles de date et d’heure*.



## Rubriques connexes

- [Contrôles de date et d’heure](date-and-time.md)
- [Sélecteur de dates du calendrier](calendar-date-picker.md)
- [Affichage Calendrier](calendar-view.md)
- [Sélecteur de dates](date-picker.md)



<!--HONumber=Jun16_HO4-->


