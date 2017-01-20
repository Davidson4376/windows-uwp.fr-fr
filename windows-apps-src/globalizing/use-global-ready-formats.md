---
author: DelfCo
Description: "Développez une app. dans une perspective de globalisation en mettant correctement en forme dates, heures, nombres, numéros de téléphone et devises."
title: Utiliser des formats compatibles avec la globalisation
ms.assetid: 6ECE8BA4-9A7D-49A6-81EE-AB2BE7F0254F
label: Use global-ready formats
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: a3924fef520d7ba70873d6838f8e194e5fc96c62
ms.openlocfilehash: eb524ae7369874e8a2f81cd4a8cbb112829387c8

---

# <a name="use-global-ready-formats"></a>Utiliser des formats compatibles avec la globalisation

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

Développez une app. dans une perspective de globalisation en mettant correctement en forme dates, heures, nombres, numéros de téléphone et devises. Cela vous permet d’adapter ultérieurement votre application à d’autres cultures, à d’autres régions et à d’autres langues pour le marché international.

<div class="important-apis" >
<b>API importantes</b><br/>
<ul>
<li>[**Windows.Globalization.Calendar**](https://msdn.microsoft.com/library/windows/apps/br206724)</li>
<li>[**Windows.Globalization.DateTimeFormatting**](https://msdn.microsoft.com/library/windows/apps/br206859)</li>
<li>[**Windows.Globalization.NumberFormatting**](https://msdn.microsoft.com/library/windows/apps/br226136)</li>
<li>[**Windows.Globalization.PhoneNumberFormatting**](https://msdn.microsoft.com/library/windows/apps/Windows.Globalization.PhoneNumberFormatting)</li>
</ul>
</div>


## <a name="introduction"></a>Introduction

Beaucoup de développeurs créent spontanément leurs applications en pensant seulement à les adapter à leurs propres langue et culture. Or, lorsqu’une application commence à avoir du succès sur d’autres marchés, il peut s’avérer difficile de l’adapter ensuite pour d’autres langues et régions. Par exemple, les dates, les heures, les nombres, les calendriers, les devises, les numéros de téléphone, les unités de mesure et les formats du papier sont des éléments susceptibles de s’afficher différemment selon la culture ou la langue.

Le processus d’adaptation d’une application à de nouveaux marchés peut être simplifié si vous prenez en compte certains éléments au cours de son développement.

## <a name="format-dates-and-times-appropriately"></a>Mettre correctement en forme les dates et les heures

Il existe de nombreux formats d’affichage possibles pour les dates et les heures. Selon les régions et les cultures, les conventions diffèrent en ce qui concerne l’ordre d’affichage du jour et du mois dans la date, la séparation des heures et des minutes dans l’heure, et même le signe de ponctuation utilisé en tant que séparateur. En outre, les dates peuvent être affichées dans différents formats longs (« mercredi 28 mars 2012 ») ou formats courts (« 28/03/12 »), ce qui est déterminé par la culture. Pour finir, les noms et les abréviations des jours de la semaine et des mois de l’année sont propres à chaque langue.

Pour permettre aux utilisateurs de sélectionner une date ou une heure, utilisez les contrôles standard de type [sélecteur de date et heure](https://msdn.microsoft.com/library/windows/apps/hh465466). Ces contrôles appliquent automatiquement les formats de date et d’heure qui sont associés à la langue et à la région par défaut de l’utilisateur.

Si vous souhaitez afficher vous-même les dates et l’heure, utilisez les formateurs [**Date/Time**](https://msdn.microsoft.com/library/windows/apps/br206859) et [**Number**](https://msdn.microsoft.com/library/windows/apps/br226136) qui permettent d’afficher automatiquement les dates, l’heure et les nombres dans le format par défaut de l’utilisateur. Le code ci-dessous met en forme une valeur DateTime dans le format associé à la langue et région par défaut. Par exemple, si la date du jour est le 3 juin 2012, le formateur affiche « 6/3/2012 » si l’utilisateur a choisi le format par défaut « Anglais (États-Unis) », mais « 03.06.2012 » s’il a choisi le format « Allemand (Allemagne) » :

```CSharp
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

## <a name="format-numbers-and-currencies-appropriately"></a>Mettre correctement en forme les nombres et les devises

La mise en forme des nombres varie en fonction des cultures. Les différences de mise en forme peuvent concerner le nombre de décimales affichées, le caractère servant de séparateur décimal et le symbole monétaire. Utilisez le formateur [**NumberFormatting**](https://msdn.microsoft.com/library/windows/apps/br226136) pour afficher les décimales, les pourcentages ou les nombres par mille, et les devises. Dans la plupart des cas, vous devrez simplement afficher les nombres et les devises en fonction des préférences utilisateur actuelles. Cependant, vous pouvez également choisir d’utiliser des formateurs pour afficher une devise d’une région ou d’un format spécifique.

Le code suivant donne un exemple de présentation des devises en fonction de la langue et de la région préférées de l’utilisateur ou pour un système monétaire donné :

```CSharp
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

## <a name="use-a-culturally-appropriate-calendar"></a>Utiliser un calendrier adapté à la culture

Le calendrier peut être différent selon les régions et les langues. Le calendrier grégorien n’est pas le calendrier utilisé par défaut dans toutes les régions. Dans certaines régions, les utilisateurs choisissent parfois d’autres calendriers, comme le calendrier japonais basé sur les ères ou le calendrier lunaire arabe. Les dates et l’heure dans le calendrier dépendent également des fuseaux horaires et de l’heure d’été/hiver.

Utilisez les contrôles standard de type [sélecteur de dates et d’heure](https://msdn.microsoft.com/library/windows/apps/hh465466) pour permettre aux utilisateurs de sélectionner des dates dans un calendrier affiché au format par défaut. Dans le cas de scénarios plus complexes nécessitant d’effectuer directement des opérations sur les dates du calendrier, Windows.Globalization propose une classe [**Calendar**](https://msdn.microsoft.com/library/windows/apps/br206724) qui permet de représenter correctement le calendrier selon la culture, la région et le type de calendrier.

## <a name="format-phone-numbers-appropriately"></a>Mettre correctement en forme les numéros de téléphone
Les numéros de téléphone affichent une mise en forme différente selon les régions. Le nombre de chiffres, la façon dont ils sont regroupés et la signification de certaines parties du numéro de téléphone varient d’un pays à l’autre. À compter de Windows 10, version 1607, vous pouvez utiliser [**PhoneNumberFormatting**](https://msdn.microsoft.com/library/windows/apps/Windows.Globalization.PhoneNumberFormatting) pour adapter le format des numéros de téléphone à la région active.

[**PhoneNumberInfo**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.globalization.phonenumberformatting.phonenumberinfo.aspx) permet d’analyser une chaîne de chiffres pour déterminer si elle forme un numéro de téléphone valide dans la région active et de comparer deux nombres pour déterminer s’ils sont égaux. Par ailleurs, il permet d’extraire les différentes parties fonctionnelles du numéro de téléphone, telles que l’indicatif du pays ou d’indicatif régional.

[**PhoneNumberFormatter**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.globalization.phonenumberformatting.phonenumberformatter.aspx) permet de mettre en forme une chaîne de chiffres ou un élément PhoneNumberInfo en vue de l’afficher, même quand ladite chaîne de chiffres représente un numéro de téléphone partiel. (Vous pouvez utiliser cette mise en forme de numéro partiel pour mettre en forme un numéro à mesure que l’utilisateur le saisit.) 

Le code ci-dessous montre comment utiliser PhoneNumberFormatter pour mettre en forme un numéro de téléphone à mesure qu’il est saisi. Chaque fois que le texte est modifié dans un TextBox nommé gradualInput, le contenu de la zone de texte est mis en forme en fonction de la région par défaut active et s’affiche dans un TextBlock nommé outBox. À titre de démonstration, la chaîne est également mise en forme pour la région Nouvelle-Zélande et s’affiche dans un TextBlock nommé NZOutBox.
    
```csharp
    using Windows.Globalization;

    PhoneNumberFormatter currentFormatter, NZFormatter;

    public MainPage()
    {
        this.InitializeComponent();

        // Use the default formatter for the current region
        currentFormatter = new PhoneNumberFormatter();

        // Create an explicit formatter for New Zealand. 
        // Note that you must check the results of TryCreate before you use the formatter.
        PhoneNumberFormatter.TryCreate("NZ", out NZFormatter);

    }

    private void gradualInput_TextChanged(object sender, TextChangedEventArgs e)
    {
        // Format for the default region into outBox.
        outBox.Text = currentFormatter.FormatPartialString(gradualInput.Text);

        // If the NZFormatter was created successfully, format the partial string for the NZOutBox.
        if(NZFormatter != null)
        {
            NZOutBox.Text = NZFormatter.FormatPartialString(gradualInput.Text);
        }
    }
```    

## <a name="respect-the-users-language-and-cultural-preferences"></a>Respecter les préférences culturelles et linguistiques de l’utilisateur

Pour les scénarios dans lesquels les fonctionnalités offertes varient en fonction des préférences définies par l’utilisateur pour la langue, la région ou la culture, Windows fournit la classe [**Windows.System.UserProfile.GlobalizationPreferences**](https://msdn.microsoft.com/library/windows/apps/br241825) qui permet d’accéder à ces préférences. Si nécessaire, utilisez la classe **GlobalizationPreferences** pour obtenir les préférences actuelles de l’utilisateur (emplacement géographique, langues par défaut, devises par défaut, etc.).

## <a name="related-topics"></a>Rubriques connexes

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



<!--HONumber=Dec16_HO2-->


