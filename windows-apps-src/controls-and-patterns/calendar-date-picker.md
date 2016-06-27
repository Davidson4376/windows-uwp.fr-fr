---
author: Jwmsft
Description: "Le sélecteur de dates de calendrier est un contrôle déroulant optimisé pour la sélection d’une seule date dans un affichage calendrier, dans lequel les informations contextuelles comme le jour de la semaine ou l’exhaustivité du calendrier sont importantes."
title: "Sélecteur de dates du calendrier"
ms.assetid: 9e0213e0-046a-4906-ba86-0b49be51ca99
label: Calendar date picker
template: detail.hbs
ms.sourcegitcommit: c183f7390c5b4f99cf0f31426c1431066e1bc96d
ms.openlocfilehash: 75f6bb925db63838e4985df15b50977b93805ffe

---

# Sélecteur de dates du calendrier

Le sélecteur de dates de calendrier est un contrôle déroulant optimisé pour la sélection d’une seule date dans un affichage calendrier, dans lequel les informations contextuelles comme le jour de la semaine ou l’exhaustivité du calendrier sont importantes. Vous pouvez modifier le calendrier pour fournir du contexte supplémentaire ou pour limiter les dates disponibles.

<span class="sidebar_heading" style="font-weight: bold;">API importantes</span>

-   [**Classe TimePicker**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.timepicker.aspx)
-   [**Propriété Time**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.timepicker.time.aspx)

## Est-ce le contrôle approprié ?
Utilisez un **sélecteur de dates du calendrier** pour permettre à un utilisateur de choisir une date unique à partir d’un affichage Calendrier contextuel. Utilisez-le pour des éléments tels que le choix d’une date de rendez-vous ou de départ.

Pour permettre à un utilisateur de choisir une date connue, par exemple, une date de naissance, où le contexte du calendrier n’est pas important, pensez à utiliser un [**sélecteur de dates**](date-picker.md).

Pour plus d’informations sur le choix du contrôle approprié, voir l’article [Contrôles de date et d’heure](date-and-time.md).

## Exemples

Le point d’entrée affiche le texte de l’espace réservé si aucune date n’a été définie. Sinon, il affiche la date choisie. Lorsque l’utilisateur sélectionne le point d’entrée, un affichage de calendrier est développé pour que l’utilisateur sélectionne une date. Cet affichage de calendrier se superpose aux autres éléments de l’interface utilisateur ; il ne les ferme pas.

![Exemple de sélecteur de dates du calendrier](images/calendar-date-picker-2-views.png)

## Créer un sélecteur de dates

```xaml
<CalendarDatePicker x:Name="arrivalCalendarDatePicker" Header="Arrival date"/>
```

```csharp
CalendarDatePicker arrivalCalendarDatePicker = new CalendarDatePicker();
arrivalCalendarDatePicker.Header = "Arrival date";
```

Le sélecteur de dates du calendrier qui en résulte se présente comme suit :

![Exemple de sélecteur de dates du calendrier](images/calendar-date-picker-closed.png)

Le sélecteur de dates du calendrier possède un [**CalendarView**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.aspx) interne pour la sélection de dates. Un sous-ensemble des propriétés CalendarView, comme [**IsTodayHighlighted**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendardatepicker.istodayhighlighted.aspx) et [**FirstDayOfWeek**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendardatepicker.firstdayofweek.aspx), existe sur CalendarDatePicker et est transmis au CalendarView interne pour vous permettre de le modifier. 

Toutefois, vous ne pouvez pas modifier le [**SelectionMode**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.selectionmode.aspx) du CalendarView interne pour autoriser la sélection multiple. Si vous avez besoin de permettre à un utilisateur de sélectionner plusieurs dates ou que vous avez besoin que le calendrier soit toujours visible, envisagez d’utiliser un affichage Calendrier plutôt qu’un sélecteur de dates du calendrier. Voir l’article [Affichage Calendrier](calendar-view.md) pour plus d’informations sur la manière dont vous pouvez modifier l’affichage du calendrier.

### Sélection de dates

Utilisez la propriété [**Date**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendardatepicker.date.aspx) pour obtenir ou définir la date sélectionnée. Par défaut, la propriété Date est **null**. Lorsqu’un utilisateur sélectionne une date dans l’affichage Calendrier, cette propriété est mise à jour. Un utilisateur peut effacer la date en cliquant sur la date sélectionnée dans l’affichage Calendrier pour la désélectionner. 

Vous pouvez définir la date dans votre code comme suit.

```csharp
myCalendarDatePicker.Date = new DateTime(1977, 1, 5);
```

Lorsque vous définissez la date dans le code, la valeur est limitée par les propriétés [**MinDate**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendardatepicker.mindate.aspx) et [**MaxDate**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendardatepicker.maxdate.aspx).
- Si la valeur **Date** est inférieure à **MinDate**, la valeur est définie sur **MinDate**.
- Si la valeur **Date** est supérieure à **MaxDate**, la valeur est définie sur **MaxDate**.

Vous pouvez gérer l’événement [**DateChanged**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendardatepicker.datechanged.aspx) pour être averti lorsque la valeur Date a changé.

> **Remarque** &nbsp;&nbsp;Pour obtenir des informations importantes sur les valeurs de date, consultez [Valeurs DateTime et Calendar](date-and-time.md#datetime-and-calendar-values) dans l’article Contrôles de date et d’heure.

### Définition d’un texte d’en-tête et d’espace réservé

Vous pouvez ajouter un contrôle [**Header**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendardatepicker.header.aspx) (ou étiquette) et un contrôle [**PlaceholderText**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendardatepicker.placeholdertext.aspx) (ou filigrane) au sélecteur de date de calendrier pour indiquer à l’utilisateur son rôle. Si vous souhaitez personnaliser l’aspect de l’en-tête, vous pouvez définir la propriété [**HeaderTemplate**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendardatepicker.headertemplate.aspx) au lieu de la propriété Header.

Le texte d’espace réservé par défaut est « sélectionner une date ». Vous pouvez le supprimer en définissant la propriété PlaceholderText sur une chaîne vide, ou vous pouvez fournir un texte personnalisé comme illustré ici.

```xaml
<CalendarDatePicker x:Name="arrivalCalendarDatePicker" Header="Arrival date" PlaceholderText="Choose your arrival date"/>
```


## Articles connexes

- [Contrôles de date et d’heure](date-and-time.md)
- [Affichage Calendrier](calendar-view.md)
- [Sélecteur de dates](date-picker.md)
- [Sélecteur d’heure](time-picker.md)



<!--HONumber=Jun16_HO3-->


