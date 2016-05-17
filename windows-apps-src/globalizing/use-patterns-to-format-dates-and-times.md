---
author: DelfCo
Description: Utilisez l’API Windows.Globalization.DateTimeFormatting avec des modèles personnalisés pour afficher les dates et heures dans le format souhaité.
title: Utiliser des modèles de format des dates et heures
ms.assetid: 012028B3-9DA2-4E72-8C0E-3E06BEC3B3FE
label: Use patterns to format dates and times
template: detail.hbs
---

# Utiliser des modèles de format des dates et heures





**API importantes**

-   [**Windows.Globalization.DateTimeFormatting**](https://msdn.microsoft.com/library/windows/apps/br206859)
-   [**DateTimeFormatter**](https://msdn.microsoft.com/library/windows/apps/br206828)
-   [**DateTime**](https://msdn.microsoft.com/library/windows/apps/br206576)

Utilisez l’API [**Windows.Globalization.DateTimeFormatting**](https://msdn.microsoft.com/library/windows/apps/br206859) avec des modèles personnalisés pour afficher les dates et heures dans le format souhaité.

## <span id="Introduction"></span><span id="introduction"></span><span id="INTRODUCTION"></span>Introduction


[
            **Windows.Globalization.DateTimeFormatting**](https://msdn.microsoft.com/library/windows/apps/br206859) offre différentes façons de formater correctement les dates et heures selon les langues et régions du monde. Vous pouvez utiliser des formats standard pour l’année, le mois, le jour, etc., ou recourir à des modèles de chaîne standard, tels que « date_longue » ou « jour mois ».

Toutefois, si vous voulez mieux contrôler l’ordre et le format des éléments de la chaîne [**DateTime**](https://msdn.microsoft.com/library/windows/apps/br206576) à afficher, vous pouvez utiliser une syntaxe spéciale pour le paramètre du modèle de chaîne, appelée « modèle ». La syntaxe modèle vous permet d’obtenir des constituants individuels d’un objet **DateTime**, seulement le nom du mois ou la valeur de l’année, par exemple, afin de les afficher dans n’importe quel format personnalisé de votre choix. En outre, il est possible de localiser le modèle pour l’adapter à d’autres langues et régions.

**Remarque** Il s’agit d’une vue d’ensemble des modèles de format. Pour consulter une description plus complète des modèles et types de formats, voir la section « Remarques » de la classe [**DateTimeFormatter**](https://msdn.microsoft.com/library/windows/apps/br206828).

 

## <span id="What_you_need_to_know"></span><span id="what_you_need_to_know"></span><span id="WHAT_YOU_NEED_TO_KNOW"></span>Ce que vous devez savoir


Il est important de noter que lorsque vous utilisez des modèles, vous créez un format personnalisé qui n’est pas forcément valide dans toutes les cultures. Par exemple, considérez le modèle « jour mois » :

**C#**
```CSharp
var datefmt = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("month day");
```
**JavaScript**
```JavaScript
var datefmt = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("month day");
```

Ce code crée un formateur basé sur la langue et la région du contexte actuel. Il affiche donc toujours le jour et le mois ensemble dans un format international approprié. Par exemple, il affiche « January 1» pour l’anglais (États-Unis), mais « 1er janvier» pour le français (France) et « 1月1日 » pour le japonais. Ceci est dû au fait que le modèle est basé sur une chaîne de modèle propre à la culture, qui est accessible par le biais de la propriété pattern :

**C#**
```CSharp
var monthdaypattern = datefmt.Patterns;
```
**JavaScript**
```JavaScript
var monthdaypattern = datefmt.patterns;
```

Les résultats diffèrent selon la langue et la région du formateur. Notez que différentes régions peuvent utiliser des constituants distincts dans des ordres différents, avec ou sans espacement et caractères supplémentaires :

``` syntax
En-US: "{month.full} {day.integer}"
Fr-FR: "{day.integer} {month.full}"
Ja-JP: "{month.integer}月{day.integer}日"
```

Vous pouvez utiliser les modèles pour construire un élément [**DateTimeFormatter**](https://msdn.microsoft.com/library/windows/apps/br206828) personnalisé, par exemple celui-ci basé sur le modèle anglais (États-Unis) :

**C#**
```CSharp
var datefmt = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("{month.full} {day.integer}");
```
**JavaScript**
```JavaScript
var datefmt = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("{month.full} {day.integer}");
```

Windows renvoie des valeurs propres à la culture pour les constituants individuels indiqués entre crochets {}. En revanche, avec la syntaxe modèle, l’ordre des constituants est invariable. Vous obtenez exactement ce que vous demandez, ce qui pourrait ne pas convenir d’un point de vue culturel :

``` syntax
En-US: January 1
Fr-FR: janvier 1 (inappropriate for France; non-standard order)
Ja-JP: 1月1 (inappropriate for Japan; the day symbol is missing)
```

En outre, la cohérence des modèles n’est pas garantie dans le temps. Les pays ou régions peuvent apporter des changements à leurs systèmes calendaires, ce qui modifie un modèle de format. Windows met à jour la sortie des formateurs pour tenir compte de ces changements. C’est pourquoi, vous ne devez utiliser la syntaxe modèle pour la mise en forme de [**DateTime**](https://msdn.microsoft.com/library/windows/apps/br206576) que :

-   si vous n’êtes pas dépendant d’une sortie particulière pour un format ;
-   si vous n’avez pas besoin que le format respecte une norme propre à une culture ;
-   si vous voulez précisément que le modèle soit invariable dans toutes les cultures ;
-   si vous avez l’intention de localiser le modèle.

Pour résumer les différences entre les modèles de chaîne standard et les modèles de chaîne non standard :

**Modèles de chaîne, comme « mois jour » :**

-   Représentation abstraite d’un format [**DateTime**](https://msdn.microsoft.com/library/windows/apps/br206576) qui inclut des valeurs pour le mois et le jour, dans un certain ordre.
-   Garantie de retourner un format standard valide pour toutes les valeurs langue-région prises en charge par Windows.
-   Garantie de produire une chaîne formatée adaptée à la langue-région donnée.
-   Les combinaisons de constituants ne sont pas toutes valides. Par exemple, il n’existe aucun modèle de chaîne pour « jour_de_la_semaine jour ».

**Modèles de chaîne, tels que « {month.full} {day.integer} » :**

-   Chaîne explicitement ordonnée qui exprime le nom complet du mois, suivi d’un espace, suivi par l’entier du jour, dans cet ordre.
-   Peut ne pas correspondre à un format standard valable pour toute paire langue-région.
-   Pas de garantie d’être adapté d’un point de vue culturel.
-   Toute combinaison de constituants peut être spécifiée dans n’importe quel ordre.

## <span id="Tasks"></span><span id="tasks"></span><span id="TASKS"></span>Tâches


Supposons que vous souhaitiez afficher le mois et le jour courants avec l’heure courante dans un format spécifique. Par exemple, vous souhaitez que les utilisateurs américains voient une chaîne de ce type :

``` syntax
June 25 | 1:38 PM
```

La partie date correspond au modèle « mois jour », et la partie heure correspond au modèle « minute heure ». Vous pouvez donc créer un format personnalisé qui concatène les éléments composant ces modèles.

Tout d’abord, obtenez les formateurs pour les modèles de date et heure pertinents, puis les modèles de ces modèles :

**C#**
```CSharp
// Get formatters for the date part and the time part.
var mydate = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("month day");
var mytime = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("hour minute");

// Get the patterns from these formatters.
var mydatepattern = mydate.Patterns[0];
var mytimepattern = mytime.Patterns[0];
```
**JavaScript**
```JavaScript
// Get formatters for the date part and the time part.
var dtf = Windows.Globalization.DateTimeFormatting;
var mydate = dtf.DateTimeFormatter("month day");
var mytime = dtf.DateTimeFormatter("hour minute");

// Get the patterns from these formatters.
var mydatepattern = mydate.patterns[0];
var mytimepattern = mytime.patterns[0];
```

Votre format personnalisé doit être stocké dans une chaîne de ressource localisable. Par exemple, la chaîne pour l’anglais (États-Unis) serait « {date} | {time} ». Les localisateurs peuvent ajuster cette chaîne selon les besoins. Ils peuvent par exemple changer l’ordre des constituants, si une langue ou une région spécifiques utilisent plus naturellement l’heure avant la date. Ils peuvent également remplacer le caractère « | » par tout autre caractère de séparation. Lors de l’exécution, vous remplacez les parties {date} et {time} de la chaîne par le modèle pertinent :

**C#**
```CSharp
// Assemble the custom pattern. This string comes from a resource, and should be localizable. 
var resourceLoader = new Windows.ApplicationModel.Resources.ResourceLoader();
var mydateplustime = resourceLoader.GetString("date_plus_time");
mydateplustime = mydateplustime.replace("{date}", mydatepattern);
mydateplustime = mydateplustime.replace("{time}", mytimepattern);
```
**JavaScript**
```JavaScript
// Assemble the custom pattern. This string comes from a resource, and should be localizable. 
var mydateplustime = WinJS.Resources.getString("date_plus_time");
mydateplustime = mydateplustime.replace("{date}", mydatepattern);
mydateplustime = mydateplustime.replace("{time}", mytimepattern);
```

Vous pouvez ensuite construire un nouveau formateur basé sur le modèle personnalisé :

**C#**
```CSharp
// Get the custom formatter.
var mydateplustimefmt = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter(mydateplustime);
```
**JavaScript**
```JavaScript
// Get the custom formatter.
var mydateplustimefmt = new dtf.DateTimeFormatter(mydateplustime);
```

## <span id="related_topics"></span>Rubriques connexes


* [Exemple de mise en forme des dates et heures](http://go.microsoft.com/fwlink/p/?LinkId=231618)
* [**Windows.Globalization.DateTimeFormatting**](https://msdn.microsoft.com/library/windows/apps/br206859)
* [**Windows.Foundation.DateTime**](https://msdn.microsoft.com/library/windows/apps/br206576)
 

 





<!--HONumber=May16_HO2-->


