---
author: DelfCo
Description: "Développez une application dans une perspective de globalisation en mettant correctement en forme les dates, les heures, les nombres et les devises."
title: Utiliser des formats compatibles avec la globalisation
ms.assetid: 6ECE8BA4-9A7D-49A6-81EE-AB2BE7F0254F
label: Use global-ready formats
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: 59e02840c72d8bccda7e318197e4bf45ed667fa4
ms.openlocfilehash: 77b5e7bd412936dd5d8c4bc252771631d6b884cf

---

# <span id="dev_globalizing.use_global-ready_formats"></span>Utiliser des formats compatibles avec la globalisation





**API importantes**

-   [**Windows.Globalization.Calendar**](https://msdn.microsoft.com/library/windows/apps/br206724)
-   [**Windows.Globalization.DateTimeFormatting**](https://msdn.microsoft.com/library/windows/apps/br206859)
-   [**Windows.Globalization.NumberFormatting**](https://msdn.microsoft.com/library/windows/apps/br226136)

Développez une application dans une perspective de globalisation en mettant correctement en forme les dates, les heures, les nombres et les devises. Cela vous permet de l’adapter plus tard à d’autres cultures, à d’autres régions et à d’autres langues pour le marché international.

## <span id="Introduction"></span><span id="introduction"></span><span id="INTRODUCTION"></span>Introduction


Beaucoup de développeurs créent spontanément leurs applications en pensant seulement à les adapter à leurs propres langue et culture. Or, lorsqu’une application commence à avoir du succès sur d’autres marchés, il peut s’avérer difficile de l’adapter ensuite pour d’autres langues et régions. Par exemple, les dates, les heures, les nombres, les calendriers, les devises, les numéros de téléphone, les unités de mesure et les formats du papier sont des éléments susceptibles de s’afficher différemment selon la culture ou la langue.

Le processus d’adaptation d’une application à de nouveaux marchés peut être simplifié si vous prenez en compte certains éléments lorsque vous la développez.

## <span id="Prerequisites"></span><span id="prerequisites"></span><span id="PREREQUISITES"></span>Conditions préalables


[Planification en vue d’un marché international](https://msdn.microsoft.com/library/windows/apps/hh465405)
## <span id="Tasks"></span><span id="tasks"></span><span id="TASKS"></span>Tâches


1.  **Mettez en forme les dates et l’heure de façon appropriée.**

    Il existe de nombreux formats d’affichage possibles pour les dates et l’heure. Selon les régions et les cultures, les conventions diffèrent en ce qui concerne l’ordre d’affichage du jour et du mois dans la date, la séparation des heures et des minutes dans l’heure, et même le signe de ponctuation utilisé en tant que séparateur. En outre, les dates peuvent être affichées dans différents formats longs («mercredi28mars2012») ou formats courts («28/03/12»), ce qui est déterminé par la culture. Pour finir, les noms et les abréviations des jours de la semaine et des mois de l’année sont propres à chaque langue.

    Pour permettre aux utilisateurs de sélectionner une date ou une heure, utilisez les contrôles standard de type [sélecteur de date et heure](https://msdn.microsoft.com/library/windows/apps/hh465466). Ces contrôles appliquent automatiquement les formats de date et d’heure qui sont associés à la langue et à la région par défaut de l’utilisateur.

    Si vous souhaitez afficher vous-même les dates et l’heure, utilisez les formateurs [**Date/Time**](https://msdn.microsoft.com/library/windows/apps/br206859) et [**Number**](https://msdn.microsoft.com/library/windows/apps/br226136) qui permettent d’afficher automatiquement les dates, l’heure et les nombres dans le format par défaut de l’utilisateur. Le code ci-dessous met en forme une valeur DateTime dans le format associé à la langue et région par défaut. Par exemple, si la date du jour est le 3juin2012, le formateur affiche «6/3/2012» si l’utilisateur a choisi le format par défaut «Anglais (États-Unis)», mais «03.06.2012» s’il a choisi le format « Allemand (Allemagne)»:

    **C#**
    ```    CSharp
    // Use the Windows.Globalization.DateTimeFormatting.DateTimeFormatter class
    // to display dates and times using basic formatters.

    // Formatters for dates and times, using shortdate format.
    var sdatefmt = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("shortdate");
    var stimefmt = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("shorttime");

    // Obtain the date that will be formatted.
    var dateToFormat = DateTime.Now;

    // Perform the actual formatting.
    var sdate = sdatefmt.Format(dateToFormat);
    var stime = stimefmt.Format(dateToFormat);

    // Results for display.
    var results = "Short Date: " + sdate + "\n" +
                  "Short Time: " + stime;
    ```
    **JavaScript**
    ```    JavaScript
    // Use the Windows.Globalization.DateTimeFormatting.DateTimeFormatter class
    // to display dates and times using basic formatters.

    // Formatters for dates and times, using shortdate format.
    var sdatefmt = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("shortdate");
    var stimefmt = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("shorttime");

    // Obtain the date that will be formatted.
    var dateToFormat = new Date();

    // Perform the actual formatting.
    var sdate = sdatefmt.format(dateToFormat);
    var stime = stimefmt.format(dateToFormat);

    // Results for display.
    var results = "Short Date: " + sdate + "\n" +
                  "Short Time: " + stime;
    ```

2.  **Mettez en forme les nombres et les devises de façon appropriée.**

    Selon les cultures, la mise en forme des nombres est différente. Les différences de mise en forme peuvent concerner le nombre de décimales affichées, le caractère servant de séparateur décimal et le symbole monétaire. Utilisez le formateur [**NumberFormatting**](https://msdn.microsoft.com/library/windows/apps/br226136) pour afficher les décimales, les pourcentages ou les nombres par mille, et les devises. Dans la plupart des cas, vous devrez simplement afficher les nombres et les devises en fonction des préférences utilisateur actuelles. Cependant, vous pouvez également choisir d’utiliser des formateurs pour afficher une devise d’une région ou d’un format spécifique.

    Le code suivant illustre la façon d’afficher des devises correspondant à la langue et la région par défaut de l’utilisateur, ou à un système monétaire particulier:

    **C#**
    ```    CSharp
    // This scenario uses the Windows.Globalization.NumberFormatting.CurrencyFormatter class
    // to format a number as a currency.

    // Determine the current user's default currency.
    var userCurrency = Windows.System.UserProfile.GlobalizationPreferences.Currencies[0];

    // Number to be formatted.
    var fractionalNumber = 12345.67;

    // Currency formatter using the current user's preference settings for number formatting.
    var userCurrencyFormat = new Windows.Globalization.NumberFormatting.CurrencyFormatter(userCurrency);
    var currencyDefault = userCurrencyFormat.Format(fractionalNumber);

    // Create a formatter initialized to a specific currency,
    // in this case US Dollar (specified as an ISO 4217 code) 
    // but with the default number formatting for the current user.
    var currencyFormatUSD = new Windows.Globalization.NumberFormatting.CurrencyFormatter("USD"); 
    var currencyUSD = currencyFormatUSD.Format(fractionalNumber);

    // Create a formatter initialized to a specific currency.
    // In this case it's the Euro with the default number formatting for France.
    var currencyFormatEuroFR = new Windows.Globalization.NumberFormatting.CurrencyFormatter("EUR", new[] { "fr-FR" }, "FR");
    var currencyEuroFR = currencyFormatEuroFR.Format(fractionalNumber);

    // Results for display.
    var results = "Fixed number (" + fractionalNumber + ")\n" +
                  "With user's default currency: " + currencyDefault + "\n" +
                  "Formatted US Dollar: " + currencyUSD + "\n" +
                  "Formatted Euro (fr-FR defaults): " + currencyEuroFR;
    ```
    **JavaScript**
    ```    JavaScript
    // This scenario uses the Windows.Globalization.NumberFormatting.CurrencyFormatter class
    // to format a number as a currency.

    // Determine the current user's default currency.
    var userCurrency = Windows.System.UserProfile.GlobalizationPreferences.currencies;

    // Number to be formatted.
    var fractionalNumber = 12345.67;

    // Currency formatter using the current user's preference settings for number formatting.
    var userCurrencyFormat = new Windows.Globalization.NumberFormatting.CurrencyFormatter(userCurrency);
    var currencyDefault = userCurrencyFormat.format(fractionalNumber);

    // Create a formatter initialized to a specific currency,
    // in this case US Dollar (specified as an ISO 4217 code) 
    // but with the default number formatting for the current user.
    var currencyFormatUSD = new Windows.Globalization.NumberFormatting.CurrencyFormatter("USD"); 
    var currencyUSD = currencyFormatUSD.format(fractionalNumber);

    // Create a formatter initialized to a specific currency.
    // In this case it's the Euro with the default number formatting for France.
    var currencyFormatEuroFR = new Windows.Globalization.NumberFormatting.CurrencyFormatter("EUR", ["fr-FR"], "FR");
    var currencyEuroFR = currencyFormatEuroFR.format(fractionalNumber);

    // Results for display.
    var results = "Fixed number (" + fractionalNumber + ")\n" +
                  "With user's default currency: " + currencyDefault + "\n" +
                  "Formatted US Dollar: " + currencyUSD + "\n" +
                  "Formatted Euro (fr-FR defaults): " + currencyEuroFR;
    ```

3.  **Utilisez un calendrier adapté à la culture.**

    Le calendrier peut être différent selon les régions et les langues. Le calendrier grégorien n’est pas le calendrier utilisé par défaut dans toutes les régions. Dans certaines régions, les utilisateurs choisissent parfois d’autres calendriers, comme le calendrier japonais basé sur les ères ou le calendrier lunaire arabe. Les dates et l’heure dans le calendrier dépendent également des fuseaux horaires et de l’heure d’été/hiver.

    Utilisez les contrôles standard de type [sélecteur de dates et d’heure](https://msdn.microsoft.com/library/windows/apps/hh465466) pour permettre aux utilisateurs de sélectionner des dates dans un calendrier affiché au format par défaut. Dans le cas de scénarios plus complexes nécessitant d’effectuer directement des opérations portant sur les dates du calendrier, Windows.Globalization fournit une classe [**Calendar**](https://msdn.microsoft.com/library/windows/apps/br206724) qui permet de représenter correctement le calendrier selon la culture, la région et le type de calendrier.

4.  **Respectez les préférences culturelles et linguistiques de l’utilisateur.**

    Pour les scénarios dans lesquels les fonctionnalités offertes varient en fonction des préférences définies par l’utilisateur pour la langue, la région ou la culture, Windows fournit la classe [**Windows.System.UserProfile.GlobalizationPreferences**](https://msdn.microsoft.com/library/windows/apps/br241825) qui permet d’accéder à ces préférences. Si nécessaire, utilisez la classe **GlobalizationPreferences** pour obtenir les préférences actuelles de l’utilisateur (emplacement géographique, langues par défaut, devises par défaut, etc.).

## <span id="related_topics"></span>Rubriques connexes


* [Planifier en vue d’un marché international](https://msdn.microsoft.com/library/windows/apps/hh465405)
* [Recommandations en matière de contrôles de date et d’heure](https://msdn.microsoft.com/library/windows/apps/hh465466)

**Référence**
* [**Windows.Globalization.Calendar**](https://msdn.microsoft.com/library/windows/apps/br206724)
* [**Windows.Globalization.DateTimeFormatting**](https://msdn.microsoft.com/library/windows/apps/br206859)
* [**Windows.Globalization.NumberFormatting**](https://msdn.microsoft.com/library/windows/apps/br226136)
* [**Windows.System.UserProfile.GlobalizationPreferences**](https://msdn.microsoft.com/library/windows/apps/br241825)

**Exemples**
* [Exemple de détails et d’opérations mathématiques dans un calendrier](http://go.microsoft.com/fwlink/p/?linkid=231636)
* [Exemple de mise en forme des dates et heures](http://go.microsoft.com/fwlink/p/?linkid=231618)
* [Exemple de préférences de globalisation](http://go.microsoft.com/fwlink/p/?linkid=231608)
* [Exemple de mise en forme et d’analyse des nombres](http://go.microsoft.com/fwlink/p/?linkid=231620)
 

 






<!--HONumber=Jun16_HO4-->


