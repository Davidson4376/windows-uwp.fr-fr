---
Description: Le sélecteur de dates offre aux utilisateurs une méthode standard de sélection des valeurs des dates localisées à l’aide d’une entrée tactile, de la souris ou du clavier. 
title: Sélecteur de dates
ms.assetid: d4a01425-4dee-4de3-9a05-3e85c3fc03cb
isNew: true
label: Date picker
template: detail.hbs
---

# Sélecteur de dates

Le sélecteur de dates offre aux utilisateurs une méthode standard de sélection des valeurs des dates localisées à l’aide d’une entrée tactile, de la souris ou du clavier. 

<span class="sidebar_heading" style="font-weight: bold;">API importantes</span>

-   [**Classe DatePicker**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.datepicker.aspx)
-   [**Propriété Date**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.datepicker.date.aspx)

## Est-ce le contrôle approprié ?
Utilisez un sélecteur de dates pour permettre à un utilisateur de choisir une date connue, par exemple, une date de naissance, où le contexte du calendrier n’est pas important.

Pour plus d’informations sur le choix du contrôle de date approprié, voir l’article [Contrôles de date et d’heure](date-and-time.md).

## Exemples

Le point d’entrée affiche la date choisie, et lorsque l’utilisateur sélectionne ce point d’entrée, la surface du sélecteur s’agrandit à la verticale à partir du milieu pour que l’utilisateur effectue une sélection. Le sélecteur de dates se superpose aux autres éléments de l’interface utilisateur ; il ne les ferme pas.

![Exemple de développement du sélecteur de date](images/controls_datepicker_expand.png)

## Créer un sélecteur de dates

Cet exemple montre comment créer un sélecteur de dates simple avec un en-tête.

```xaml
<DatePicker x:Name=birthDatePicker Header="Date of birth"/>
```

```csharp
DatePicker birthDatePicker = new DatePicker();
birthDatePicker.Header = "Date of birth";
```

Le sélecteur de dates qui en résulte se présente comme suit :

![Exemple de sélecteur de dates](images/date-picker-closed.png)

> **Remarque** Pour obtenir des informations importantes sur les valeurs de date, voir [Valeurs de date, d’heure et de calendrier](date-and-time.md#datetime-and-calendar-values) dans l’article relatif aux contrôles de date et d’heure.



## Articles connexes

- [Contrôles de date et d’heure](date-and-time.md)
- [Sélecteur de dates du calendrier](calendar-date-picker.md)
- [Affichage Calendrier](calendar-view.md)
- [Sélecteur d’heure](time-picker.md)


<!--HONumber=Mar16_HO4-->


