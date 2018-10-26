---
author: stevewhims
Description: Use the Windows.Globalization.DateTimeFormatting API with custom templates and patterns to display dates and times in exactly the format you wish.
title: Utiliser des modèles de format des dates et heures
ms.assetid: 012028B3-9DA2-4E72-8C0E-3E06BEC3B3FE
label: Use patterns to format dates and times
template: detail.hbs
ms.author: stwhi
ms.date: 11/09/2017
ms.topic: article
keywords: windows10, uwp, globalisation, adaptabilité, localisation
ms.localizationpriority: medium
ms.openlocfilehash: 04a0288d0b28c12eb68cf56225747224e8df9777
ms.sourcegitcommit: 6cc275f2151f78db40c11ace381ee2d35f0155f9
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/26/2018
ms.locfileid: "5563455"
---
# <a name="use-templates-and-patterns-to-format-dates-and-times"></a>Utiliser des modèles de format des dates et heures

Utilisez le classes de l'espace de noms [**Windows.Globalization.DateTimeFormatting**](/uwp/api/windows.globalization.datetimeformatting?branch=live) avec des modèles personnalisés pour afficher les dates et heures dans le format que vous désirez.

## <a name="introduction"></a>Introduction

La classe [**DateTimeFormatter**](/uwp/api/windows.globalization.datetimeformatting?branch=live) offre différentes façons de formater correctement les dates et heures selon les langues et régions du monde. Vous pouvez utiliser les formats standard pour l'année, le mois, le jour, etc. Sinon, vous pouvez transmettre un modèle de format à l'argument *formatTemplate* du constructeur **DateTimeFormatter**, tel que «longdate» ou «mois jour».

Mais lorsque vous souhaitez contrôler davantage l'ordre et le format des composants de l'objet [**DateTime**](/uwp/api/windows.foundation.datetime?branch=live) à afficher, vous pouvez transmettre un modèle de format à l'argument *formatTemplate* du constructeur. Les modèles de format utilisent une syntaxe spéciale qui vous permet d'obtenir les composants individuels d'un objet **DateTime** &mdash;par exemple, simplement le nom du mois ou la valeur de l'année&mdash;afin de pouvoir les disposer dans le format personnalisé de votre choix. En outre, il est possible de localiser le modèle pour l’adapter à d’autres langues et régions.

**Remarque**il s’agit uniquement d’une vue d’ensemble des modèles de format. Pour consulter une description plus complète des modèles et types de formats, voir la section «Remarques» de la classe [**DateTimeFormatter**](/uwp/api/windows.globalization.datetimeformatting?branch=live).

## <a name="the-difference-between-format-templates-and-format-patterns"></a>Différence entre les modèles de format et les motifs de format

Les modèles de format sont des chaînes de format indifférentes à la culture. Par conséquent, si vous construisez un **DateTimeFormatter** à l'aide d'un modèle de format, le formateur affiche les composants de votre format dans le bon ordre en fonction de la langue actuelle. À l'inverse, un motif de format est spécifique à la culture. Si vous construisez un **DateTimeFormatter** à l'aide d'un motif de format, le formateur utilisera le motif de manière très fidèle. Par conséquent, un motif n'est pas nécessairement valide pour toutes les cultures.

Illustrons cette distinction à l'aide d'un exemple. Nous transmettons un simple modèle (et non un motif) de format au constructeur **DateTimeFormatter**. Il s'agit du modèle de format «mois jour».

```csharp
var dateFormatter = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("month day");
```

Ce code crée un formateur basé sur la langue et la région du contexte actuel. Dans un modèle de format, l'ordre des composants importe peu; le formateur les affiche dans le bon ordre pour la langue actuelle. Ainsi il affiche «Janvier1» pour l’anglais (États-Unis), «1janvier» pour le français (France) et «1月1日» pour le japonais.

En revanche, le motif de format est spécifique à la culture. Accédons au motif de format pour notre modèle de format.

```csharp
IReadOnlyList<string> monthDayPatterns = dateFormatter.Patterns;
```

Les résultats diffèrent selon la langue et la région d'exécution. Différentes régions peuvent utiliser des composants distincts dans des ordres différents, avec ou sans espacement et caractères supplémentaires.

```syntax
En-US: "{month.full} {day.integer}"
Fr-FR: "{day.integer} {month.full}"
Ja-JP: "{month.integer}月{day.integer}日"
```

Dans l'exemple ci-dessus, nous avons saisi une chaîne de format indifférente à la culture et avons récupéré une chaîne de format spécifique à la culture (qui consistait en une fonction de la langue et de la région qui était en vigueur lorsque nous l'avons nommée `dateFormatter.Patterns`). Elle suit la chaîne si vous avez construit un **DateTimeFormatter** à partir d'un motif de format spécifique à la culture, elle sera donc uniquement valide pour des langues/régions spécifiques.

```csharp
var dateFormatter = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("{month.full} {day.integer}");
```

Le formateur ci-dessus renvoie des valeurs spécifiques à la culture pour les composants individuels à l’intérieur des crochets {}. Néanmoins, l'ordre des composants d'un motif de format est invariable. Vous obtenez exactement ce que vous demandez, ce qui pourrait convenir ou non d’un point de vue culturel: Ce formateur est valide pour l'anglais (États-Unis), mais pas pour le français (France), ni pour le japonais.

``` syntax
En-US: January 1
Fr-FR: janvier 1 (inappropriate for France; non-standard order)
Ja-JP: 1月1 (inappropriate for Japan; the day symbol 日 is missing)
```

De plus, un motif qui s'applique correctement aujourd'hui pourrait ne pas s'appliquer à l'avenir. Les pays ou régions peuvent apporter des changements à leurs systèmes calendaires, ce qui modifie un modèle de format. Windows met à jour la sortie des formateurs en fonction des modèles de format pour tenir compte de ces changements. Par conséquent, vous devriez utiliser la syntaxe de motif uniquement si au moins l'une de ces conditions s'applique.

-   Si vous n’êtes pas dépendant d’une sortie particulière pour un format ;
-   si vous n’avez pas besoin que le format respecte une norme propre à une culture ;
-   Si vous voulez précisément que le modèle soit invariable dans toutes les cultures ;
-   Vous avez l'intention de localiser la chaîne de motif de format actuelle.

Voici un résumé de la distinction entre les modèles de format et les motifs de format.

**Modèles de format, comme « mois jour »**

-   Représentation abstraite d’un format [DateTime](/uwp/api/windows.foundation.datetime?branch=live) qui inclut des valeurs pour le mois, le jour, etc., dans un certain ordre.
-   Garantie de retourner un format standard valide pour toutes les valeurs langue-région prises en charge par Windows.
-   Garantie de produire une chaîne formatée adaptée à la langue-région donnée.
-   Les combinaisons de composants ne sont pas toutes valides. Par exemple, «jourdelasemaine jour» n'est pas un format valide.

**Modèles de format, tels que «{month.full} {day.integer}»**

-   Chaîne explicitement ordonnée qui exprime le nom complet du mois, suivi d’un espace, suivi par l’entier du jour, dans cet ordre ou tout autre motif de format spécifique que vous spécifiez.
-   Peut ne pas correspondre à un format standard valable pour toute paire langue-région.
-   Pas de garantie d’être adapté d’un point de vue culturel.
-   Toute combinaison de composants peut être spécifiée dans n’importe quel ordre.

## <a name="examples"></a>Exemples

Supposons que vous souhaitiez afficher le mois et le jour courants avec l’heure courante dans un format spécifique. Par exemple, vous souhaitez que les utilisateurs américains voient une chaîne de ce type:

``` syntax
June 25 | 1:38 PM
```

La partie date correspond au modèle de format «mois jour», et la partie heure correspond au modèle de format «minute heure». Ainsi, vous pouvez construire des formateurs pour les modèles de format de date et d'heure adéquat, puis concaténer leurs sorties ensemble à l'aide d'une chaîne de format localisable.

```csharp
var dateToFormat = System.DateTime.Now;
var resourceLoader = Windows.ApplicationModel.Resources.ResourceLoader.GetForCurrentView();

var dateFormatter = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("month day");
var timeFormatter = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("hour minute");

var date = dateFormatter.Format(dateToFormat);
var time = timeFormatter.Format(dateToFormat);

string output = string.Format(resourceLoader.GetString("CustomDateTimeFormatString"), date, time);
```

`CustomDateTimeFormatString` est un identificateur de ressource faisant référence à une ressource localisable dans un fichier Ressources (.resw). Pour une langue par défaut de l’anglais (États-Unis), il serait défini à une valeur de «{0} | {1}«et un commentaire indiquerait que»{0}» correspond à la date et de «{1}«est le temps. De cette manière, les traducteurs sont en mesure d'ajuster les éléments de format selon les besoins. Ils peuvent par exemple changer l’ordre des éléments, si une langue ou une région spécifique utilise plus naturellement l’heure avant la date. Ils peuvent également remplacer le caractère «|» par tout autre caractère de séparation.

Il existe une autre manière d'implémenter cet exemple: il suffit d'interroger les deuxformateurs concernant leurs motifs de format, de les concaténer ensemble, puis de construire un troisième formateur à partir du motif de format résultant.

```csharp
var resourceLoader = Windows.ApplicationModel.Resources.ResourceLoader.GetForCurrentView();

var dateFormatter = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("month day");
var timeFormatter = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("hour minute");

string dateFormatterPattern = dateFormatter.Patterns[0];
string timeFormatterPattern = timeFormatter.Patterns[0];

string pattern = string.Format(resourceLoader.GetString("CustomDateTimeFormatString"), dateFormatterPattern, timeFormatterPattern);

var patternFormatter = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter(pattern);

string output = patternFormatter.Format(System.DateTime.Now);
```

## <a name="important-apis"></a>API importantes

* [Windows.Globalization.DateTimeFormatting](/uwp/api/windows.globalization.datetimeformatting?branch=live)
* [DateTimeFormatter](/uwp/api/windows.globalization.datetimeformatting?branch=live)
* [DateTime](/uwp/api/windows.foundation.datetime?branch=live)

## <a name="related-topics"></a>Rubriquesassociées

* [Exemple de mise en forme des dates et heures](http://go.microsoft.com/fwlink/p/?LinkId=231618)