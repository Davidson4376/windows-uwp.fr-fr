---
author: muhsinking
Description: Date and time controls let you view and set the date and time. This article provides design guidelines and helps you pick the right control.
title: Recommandations en matière de contrôles de date et d’heure
ms.assetid: 4641FFBB-8D82-4290-94C1-D87617997F61
label: Calendar, date, and time controls
template: detail.hbs
ms.author: mukin
ms.date: 05/19/2017
ms.topic: article
keywords: windows10, uwp
pm-contact: kisai
design-contact: ksulliv
dev-contact: joyate
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 363c60a9aeaaa98d808e015f96e4096cf889a501
ms.sourcegitcommit: bdc40b08cbcd46fc379feeda3c63204290e055af
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/08/2018
ms.locfileid: "6137720"
---
# <a name="calendar-date-and-time-controls"></a>Contrôles de calendrier, de date et d’heure

 

Les contrôles de date et d’heure offrent aux utilisateurs des moyens standards et localisés d’afficher et de définir des valeurs de date et d’heure dans votre application. Cet article fournit des recommandations en matière de conception et vous permet de sélectionner le contrôle approprié.

> **API importantes**: [classe CalendarView](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.aspx), [classe CalendarDatePicker](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendardatepicker.aspx), [classe DatePicker](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.datepicker.aspx), [classe TimePicker](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.timepicker.aspx)

<table>
<th align="left">Galerie de contrôles XAML<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si vous disposez de l'application <strong style="font-weight: semi-bold">Galerie de contrôles XAML</strong>, cliquez ici pour <a href="xamlcontrolsgallery:/category/DataInput">ouvrir l’application et voir ces contrôles en action</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenir l’application Galerie de contrôles XAML (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Obtenir le code source (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="which-date-or-time-control-should-you-use"></a>Quel contrôle de date ou d’heure utiliser?

Quatre contrôles de date et d’heure sont disponibles; le contrôle à utiliser dépend de votre scénario. Utilisez les informations fournies dans cet article pour sélectionner le contrôle adapté à votre application.

&nbsp;|&nbsp;|&nbsp;                                                                                                                      
--------------------|-------|-------------------------------------------------------------------------------------------------------------------------------
Calendar view       |![Exemple d’affichage Calendrier](images/controls_calendar_monthview_small.png)|Pour sélectionner une date unique ou une plage de dates à partir d’un calendrier toujours visible.                   
Sélecteur de dates du calendrier|![Exemple de sélecteur de dates du calendrier](images/calendar-date-picker-closed.png)|Pour sélectionner une date unique à partir d’un calendrier contextuel. 
Sélecteur de dates         |![Exemple de sélecteur de dates](images/date-picker-closed.png)|Pour sélectionner une seule date connue lorsque les informations contextuelles importent peu.
Sélecteur d’heure         |![Exemple de sélecteur d’heure](images/time-picker-closed.png)|Pour sélectionner une seule valeur d’heure.                                        

<!-- This table seems redundant, not sure it's needed.-->

### <a name="calendar-view"></a>Affichage Calendrier

L’affichage **CalendarView** permet à un utilisateur d’afficher un calendrier qu’il peut parcourir par mois, par année ou par décennie, et d’interagir avec celui-ci. Un utilisateur peut sélectionner une seule date ou une plage de dates. Il n’y a pas de surface de sélection et le calendrier est toujours visible.

L’affichage Calendrier se compose de 3affichages distincts: l’affichage mensuel, l’affichage annuel et l’affichage décennal. Par défaut, il s’ouvre avec l’affichage mensuel, mais vous pouvez spécifier l’affichage de votre choix en tant qu’affichage de démarrage.

![Exemple de sélecteur de dates du calendrier](images/calendar-view-3-views.png)

- Si vous avez besoin de permettre à un utilisateur de sélectionner plusieurs dates, vous devez utiliser un affichage **CalendarView**.
- Si vous avez besoin de permettre à un utilisateur de sélectionner une seule date sans que le calendrier soit toujours visible, envisagez d’utiliser un contrôle de type **CalendarDatePicker** ou **DatePicker**.

### <a name="calendar-date-picker"></a>Sélecteur de dates du calendrier

Le sélecteur de dates de calendrier (**CalendarDatePicker**) est un contrôle déroulant optimisé pour la sélection d’une seule date dans un affichage calendrier, dans lequel les informations contextuelles comme le jour de la semaine ou l’exhaustivité du calendrier sont importantes. Vous pouvez modifier le calendrier pour fournir du contexte supplémentaire ou pour limiter les dates disponibles.

Le point d’entrée affiche le texte de l’espace réservé si aucune date n’a été définie. Sinon, il affiche la date choisie. Lorsque l’utilisateur sélectionne le point d’entrée, un affichage de calendrier est développé pour que l’utilisateur sélectionne une date. Cet affichage de calendrier se superpose aux autres éléments de l’interface utilisateur; il ne les ferme pas.

![Exemple de sélecteur de dates du calendrier](images/calendar-date-picker-2-views.png)

- Utilisez un sélecteur de dates de calendrier pour des éléments tels que le choix d’une date de rendez-vous ou de départ. 

### <a name="date-picker"></a>Sélecteur de dates

Le contrôle sélecteur de dates (**DatePicker**) offre une méthode normalisée de sélection d’une date spécifique. 

Le point d’entrée affiche la date choisie, et lorsque l’utilisateur sélectionne ce point d’entrée, la surface du sélecteur s’agrandit à la verticale à partir du milieu pour que l’utilisateur effectue une sélection. Le sélecteur de dates se superpose aux autres éléments de l’interface utilisateur; il ne les ferme pas.

![Exemple de développement du sélecteur de date](images/controls_datepicker_expand.png)

- Utilisez un sélecteur de dates pour permettre à un utilisateur de choisir une date connue, par exemple, une date de naissance, où le contexte du calendrier n’est pas important.

### <a name="time-picker"></a>Sélecteur d’heure

Le sélecteur d’heure (**TimePicker**) permet de sélectionner une valeur d’heure spécifique pour des éléments tels que des heures de rendez-vous ou de départ. Il s’agit d’un affichage statique défini par l’utilisateur ou dans le code. Cependant, il ne se met pas à jour pour indiquer l’heure actuelle. 

Le point d’entrée affiche l’heure choisie, et lorsque l’utilisateur sélectionne ce point d’entrée, la surface du sélecteur s’agrandit à la verticale à partir du milieu pour que l’utilisateur effectue une sélection. Le sélecteur d’heure se superpose aux autres éléments de l’interface utilisateur; il ne les ferme pas.

![Exemple de développement du sélecteur d’heure](images/controls_timepicker_expand.png)

- Utilisez un sélecteur d’heure pour permettre à un utilisateur de sélectionner une valeur d’heure unique.

## <a name="create-a-date-or-time-control"></a>Créez un contrôle de date ou d’heure

Pour plus d’informations et d’exemples propres à chaque contrôle de date ou d’heure, voir les articles suivants.

- [Affichage Calendrier](calendar-view.md)
- [Sélecteur de dates du calendrier](calendar-date-picker.md)
- [Sélecteur de dates](date-picker.md)
- [Sélecteur d’heure](time-picker.md)

### <a name="globalization"></a>Globalisation

Le contrôle des dates XAML prend en charge chacun des systèmes de calendrier pris en charge par Windows. Ces calendriers sont définis dans la classe [Windows.Globalization.CalendarIdentifiers](https://msdn.microsoft.com/library/windows/apps/xaml/windows.globalization.calendaridentifiers.aspx). Chaque contrôle utilise le calendrier approprié pour la langue par défaut de votre application. Vous pouvez également définir la propriété **CalendarIdentifier** pour utiliser un système de calendrier spécifique.

Le contrôle de sélecteur d’heure prend en charge chacun des systèmes d’horloge spécifiés dans la classe [Windows.Globalization.ClockIdentifiers](https://msdn.microsoft.com/library/windows/apps/xaml/windows.globalization.clockidentifiers.aspx). Vous pouvez définir la propriété [ClockIdentifier](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.timepicker.clockidentifier.aspx) pour utiliser une horloge au format 12 heures ou 24 heures. La propriété est de type String, mais vous devez utiliser des valeurs qui correspondent aux propriétés de chaîne statique de la classe ClockIdentifiers, c’est-à-dire: TwelveHour (chaîne «12HourClock» et TwentyFourHour (chaîne «24HourClock»). La valeur par défaut est «12HourClock».


### <a name="datetime-and-calendar-values"></a>Valeurs DateTime et Calendar

Les objets Date utilisés dans les contrôles de date et d’heure XAML ont une représentation différente selon votre langage de programmation. 
- C# et Visual Basic utilisent la structure [System.DateTimeOffset](https://msdn.microsoft.com/library/windows/apps/xaml/system.datetimeoffset.aspx) qui fait partie de .NET. 
- C++/CX utilise la structure [Windows::Foundation::DateTime](https://msdn.microsoft.com/library/windows/apps/xaml/br205770.aspx). 

Un concept associé est la classe Calendar qui influence l’interprétation des dates dans le contexte. Toutes les applications Windows Runtime peuvent utiliser la classe [Windows.Globalization.Calendar](https://msdn.microsoft.com/library/windows/apps/xaml/windows.globalization.calendar.aspx). Les applications C# et Visual Basic peuvent également utiliser la classe [System.Globalization.Calendar](https://msdn.microsoft.com/library/windows/apps/xaml/system.globalization.calendar.aspx), qui comporte une fonctionnalité très similaire. (Les applications WindowsRuntime peuvent utiliser la classe Calendar .NET de base, mais pas les implémentations spécifiques; par exemple, GregorianCalendar.)

.NET prend également en charge un type nommé [DateTime](https://msdn.microsoft.com/library/windows/apps/xaml/system.datetime.aspx), qui est implicitement convertible en un [DateTimeOffset](https://msdn.microsoft.com/library/windows/apps/xaml/system.datetimeoffset.aspx). Par conséquent, il est possible que vous voyiez un type «DateTime» utilisé dans le code .NET qui sert à définir des valeurs DateTimeOffset. Pour plus d’informations sur la différence entre DateTime et DateTimeOffset, voir les remarques dans la classe [DateTimeOffset](https://msdn.microsoft.com/library/windows/apps/xaml/system.datetimeoffset.aspx).

> **Remarque**&nbsp;&nbsp;Les propriétés qui acceptent les objets date ne peuvent pas être définies comme chaîne d’attribut XAML, car l’analyseur XAML Windows Runtime ne dispose pas d’une logique de conversion pour convertir les chaînes en dates comme des objets DateTime/DateTimeOffset. Vous définissez généralement ces valeurs dans le code. Une autre technique possible consiste à définir une date disponible sous la forme d’un objet de données ou dans le contexte de données, puis à définir la propriété en tant qu’attribut XAML qui fait référence à une expression d’[extension de balisage \{Binding\}](../../xaml-platform/binding-markup-extension.md), qui peut accéder à la date en tant que données.

## <a name="get-the-sample-code"></a>Obtenir l’exemple de code
* [Exemple d’éléments de base d’une interface utilisateur XAML](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/XamlUIBasics)


## <a name="related-topics"></a>Rubriques connexes

**Pour les développeurs (XAML)**
- [Classe CalendarView](https://msdn.microsoft.com/library/windows/apps/dn890052)
- [Classe CalendarDatePicker](https://msdn.microsoft.com/library/windows/apps/dn950083)
- [Classe DatePicker](https://msdn.microsoft.com/library/windows/apps/dn298584)
- [Classe TimePicker](https://msdn.microsoft.com/library/windows/apps/dn299280)
