---
Description: Design your app to be global-ready by appropriately formatting dates, times, numbers, phone numbers, and currencies. You'll then be able later to adapt your app for additional cultures, regions, and languages in the global market.
title: Globaliser vos formats de date/heure/chiffres
ms.assetid: 6ECE8BA4-9A7D-49A6-81EE-AB2BE7F0254F
template: detail.hbs
ms.date: 11/07/2017
ms.topic: article
keywords: windows10, uwp, globalisation, adaptabilité, localisation
ms.localizationpriority: medium
ms.openlocfilehash: e8a2b0125944a8a4db66b41d26fcd4a0aa35b5b2
ms.sourcegitcommit: a3dc929858415b933943bba5aa7487ffa721899f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/07/2018
ms.locfileid: "8781177"
---
# <a name="globalize-your-datetimenumber-formats"></a>Globaliser vos formats de date/heure/chiffres

Concevez votre application dans une perspective de globalisation en mettant correctement en forme dates, heures, nombres, numéros de téléphone et devises. Plus tard, vous serez en mesure d’adapter votre application à d’autres cultures, à d’autres régions et à d’autres langues pour le marché international.

## <a name="introduction"></a>Introduction

Lors de la création de votre application, si vous adoptez une pensée plus large qu'une unique langue et culture, vous obtiendrez moins (ou pas) de problèmes inattendues lorsque votre application s'étendra à de nouveaux marchés. Par exemple, les dates, les heures, les nombres, les calendriers, les devises, les numéros de téléphone, les unités de mesure et les formats du papier sont des éléments susceptibles de s’afficher différemment selon la culture ou la langue.

Les régions et cultures différentes emploient des formats de date et d'heure différents. Ces formats incluent les conventions en ce qui concerne l’ordre d’affichage du jour et du mois dans la date, la séparation des heures et des minutes dans l’heure, et même le signe de ponctuation utilisé en tant que séparateur. En outre, les dates peuvent être affichées dans différents formats longs («mercredi28mars2012») ou formats courts («28/03/12»), ce qui est déterminé par la culture. Enfin, les noms et les abréviations des jours de la semaine et des mois de l’année sont propres à chaque langue.

Vous pouvez prévisualiser les formats utilisés pour différentes langues. Accédez à **Paramètres** > **Heure et langue** > **Région et langue**, puis cliquez sur **Paramètres de date, d'heure et régionaux supplémentaires** > **Modifier les formats de date, d’heure ou de nombre**. Dans l'onglet **Formats**, sélectionnez une langue dans le menu déroulant **Format** et prévisualisez les formats dans **Exemples**.

Cette rubrique utilise les termes «liste de langues de profil utilisateur», «liste de langues de manifeste d'application» et «liste de langues d'exécution de l'application». Pour plus d'informations sur la définition exacte de ces termes et comment accéder à leurs valeurs, consultez [Comprendre les langues de profil utilisateur et les langues du manifeste d'application](manage-language-and-region.md).

## <a name="format-dates-and-times-for-the-app-runtime-language-list"></a>Formater les dates et heures pour la liste de langue d'exécution de l'application

Pour permettre aux utilisateurs de sélectionner une date ou une heure, utilisez les contrôles standard de type [contrôles du calendrier, de la date et de l'heure](../controls-and-patterns/date-and-time.md). Ces contrôles utilisent le meilleur format de date et d'heure pour la liste de langue d'exécution de l'application.

Si vous avez besoin d'afficher les dates et heures vous-mêmes, vous pouvez utiliser la classe [**DateTimeFormatter**](/uwp/api/windows.globalization.datetimeformatting?branch=live). Par défaut, la classe **DateTimeFormatter** utilise automatiquement le meilleur format de date et d'heure pour la liste de langue d'exécution de l'application. Par conséquent, le code ci-dessous formate un paramètre **DateTime** de la meilleure façon pour cette liste. Par exemple, supposez que la liste de langue du manifeste de votre application comprend l'anglais (États-Unis) qui est également la langue par défaut, et l'allemand (Allemagne). Si la date actuelle est le 6novembre2017 et que la liste de langue du profil utilisateur contient d'abord l'allemand (Allemagne), alors le formateur donne la date sous le format «06.11.2017» Si la liste de langue du profil utilisateur contient l'anglais (États-Unis) en premier lieu (ou si elle ne contient ni l'anglais, ni l'allemand), alors le formateur donne la date sous le format «11/6/2017» (puisque la langue «en-US» correspond ou est utilisée par défaut).

```csharp
    // Use the DateTimeFormatter class to display dates and times using basic formatters.

    var shortDateFormatter = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("shortdate");
    var shortTimeFormatter = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("shorttime");

    var dateTimeToFormat = DateTime.Now;

    var shortDate = shortDateFormatter.Format(dateTimeToFormat);
    var shortTime = shortTimeFormatter.Format(dateTimeToFormat);

    var results = "Short Date: " + shortDate + "\n" +
                  "Short Time: " + shortTime;
```

C'est ainsi que vous pouvez tester le code ci-dessous sur votre propre PC.

- Veillez à disposer des fichiers de ressources dans votre projet, qualifiés à la fois pour la langue «en-US» et pour la langue «de-DE» (consultez [Adapter vos ressources pour la langue, l'échelle, le contraste élevé et d'autres qualificateurs](../../app-resources/tailor-resources-lang-scale-contrast.md)).
- Modifiez votre liste de langue de profil utilisateur dans **Paramètres** > **Heure et langue** > **Région et langue** > **Langues**. Ajoutez l'allemand (Allemagne), définissez-la par défaut et exécutez le code à nouveau.

## <a name="format-dates-and-times-for-the-user-profile-language-list"></a>Formater les dates et heures pour la liste de langue de profil utilisateur

N’oubliez pas que, par défaut, **DateTimeFormatter** correspond à la liste de langue d'exécution de l'application. Ainsi, si vous affichez des chaînes telles que «La date est &lt;date&gt;», la langue correspondra au format de date.

Si, pour une quelconque raison, vous souhaitez formater les dates et/ou heures uniquement en fonction de la liste de langue de profil utilisateur, alors vous pouvez y procéder en utilisant un code semblable à l'exemple ci-dessous. Mais si vous y procédez, vous devez comprendre que l'utilisateur peut choisir une langue pour laquelle l'application ne dispose pas de chaînes traduites. Par exemple, si votre application n'est pas localisée en allemand (Allemagne), mais que l'utilisateur choisit cette langue en tant que langue favorite, alors l'affichage engendrera sans doute des chaînes d'apparence étrange, comme «La date est 06.11.2017».

```csharp
    // Use the DateTimeFormatter class to display dates and times using basic formatters.

    var userLanguages = Windows.System.UserProfile.GlobalizationPreferences.Languages;

    var shortDateFormatter = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("shortdate", userLanguages);

    var results = "Short Date: " + shortDateFormatter.Format(DateTime.Now);
```

## <a name="format-numbers-and-currencies-appropriately"></a>Mettre correctement en forme les nombres et les devises

La mise en forme des nombres varie en fonction des cultures. Les différences de mise en forme peuvent concerner le nombre de décimales affichées, le caractère servant de séparateur décimal et le symbole monétaire. Utilisez les classes de l'espace de noms [**NumberFormatting**](/uwp/api/windows.globalization.numberformatting?branch=live) pour afficher les décimales, les pourcentages ou les nombres par mille, et les devises. La plupart du temps, vous souhaiterez que ces classes de formateur utilisent le meilleur format pour le profil utilisateur. Cependant, vous pouvez choisir d’utiliser des formateurs pour afficher une devise d'une région ou d'un format particulier.

Cet exemple illustre comment afficher les devises à la fois selon le profil utilisateur et selon un système de devise spécifique donné.

```csharp
    // This scenario uses the CurrencyFormatter class to format a number as a currency.

    var userCurrency = Windows.System.UserProfile.GlobalizationPreferences.Currencies[0];

    var valueToBeFormatted = 12345.67;

    var userCurrencyFormatter = new Windows.Globalization.NumberFormatting.CurrencyFormatter(userCurrency);
    var userCurrencyValue = userCurrencyFormatter.Format(valueToBeFormatted);

    // Create a formatter initialized to a specific currency,
    // in this case US Dollar (specified as an ISO 4217 code) 
    // but with the default number formatting for the current user.
    var currencyFormatUSD = new Windows.Globalization.NumberFormatting.CurrencyFormatter("USD");
    var currencyValueUSD = currencyFormatUSD.Format(valueToBeFormatted);

    // Create a formatter initialized to a specific currency.
    // In this case it's the Euro with the default number formatting for France.
    var currencyFormatEuroFR = new Windows.Globalization.NumberFormatting.CurrencyFormatter("EUR", new[] { "fr-FR" }, "FR");
    var currencyValueEuroFR = currencyFormatEuroFR.Format(valueToBeFormatted);

    // Results for display.
    var results = "Fixed number (" + valueToBeFormatted + ")\n" +
                    "With user's default currency: " + userCurrencyValue + "\n" +
                    "Formatted US Dollar: " + currencyValueUSD + "\n" +
                    "Formatted Euro (fr-FR defaults): " + currencyValueEuroFR;
```

Vous pouvez tester le code ci-dessous sur votre propre PC en modifiant le pays ou la région depuis **Paramètres** > **Heure et langue** > **Région et langue** > **Pays ou région**. Choisissez un pays ou une région (essayez l'Islande), et exécutez le code à nouveau.

## <a name="use-a-culturally-appropriate-calendar"></a>Utiliser un calendrier adapté à la culture

Le calendrier peut être différent selon les régions et les langues. Le calendrier grégorien n’est pas le calendrier utilisé par défaut dans toutes les régions. Dans certaines régions, les utilisateurs choisissent parfois d’autres calendriers, comme le calendrier japonais basé sur les ères ou le calendrier lunaire arabe. Les dates et l’heure dans le calendrier dépendent également des fuseaux horaires et de l’heure d’été/hiver.

Pour garantir l'utilisation du format de calendrier favori, vous pouvez utiliser les [contrôles de calendrier, date et heure](../controls-and-patterns/date-and-time.md) standard. Dans le cas de scénarios plus complexes nécessitant d’effectuer directement des opérations sur les dates du calendrier, **Windows.Globalization** propose un [**Calendrier**](/uwp/api/windows.globalization.calendar?branch=live) qui permet de représenter correctement le calendrier selon la culture, la région et le type de calendrier.

## <a name="format-phone-numbers-appropriately"></a>Mettre correctement en forme les numéros de téléphone

Les numéros de téléphone affichent une mise en forme différente selon les régions. Le nombre de chiffres, la façon dont ils sont regroupés et la signification de certaines parties du numéro de téléphone varient selon le pays. À compter de Windows10, version1607, vous pouvez utiliser les classes de l'espace de noms [**PhoneNumberFormatting**](/uwp/api/windows.globalization.phonenumberformatting?branch=live) pour adapter le format des numéros de téléphone à la région active.

[**PhoneNumberInfo**](/uwp/api/windows.globalization.phonenumberformatting.phonenumberinfo?branch=live) permet d’analyser une chaîne de chiffres pour déterminer si elle forme un numéro de téléphone valide dans la région active et de comparer deux nombres pour déterminer s’ils sont égaux. Par ailleurs, il permet d’extraire les différentes parties fonctionnelles du numéro de téléphone, telles que l’indicatif du pays ou l’indicatif régional.

[**PhoneNumberFormatter**](/uwp/api/windows.globalization.phonenumberformatting.phonenumberformatter?branch=live) permet de mettre en forme une chaîne de chiffres ou un élément **PhoneNumberInfo** en vue de l’afficher, même quand ladite chaîne de chiffres représente un numéro de téléphone partiel. Vous pouvez utiliser cette mise en forme de numéro partiel pour mettre en forme un numéro à mesure que l’utilisateur le saisit.

L'exemple ci-dessous montre comment utiliser **PhoneNumberFormatter** pour mettre en forme un numéro de téléphone à mesure qu’il est saisi. Chaque fois que le texte est modifié dans un **TextBox** nommé phoneNumberInputTextBox, le contenu de la zone de texte est mis en forme en fonction de la région par défaut active et s’affiche dans un **TextBlock** nommé phoneNumberOutputTextBlock. À titre de démonstration, la chaîne est également mise en forme pour la région Nouvelle-Zélande et s’affiche dans un TextBlock nommé phoneNumberOutputTextBlockNZ.
  
```csharp
    using Windows.Globalization.PhoneNumberFormatting;

    PhoneNumberFormatter currentFormatter, NZFormatter;

    public MainPage()
    {
        this.InitializeComponent();

        // Use the default formatter for the current region
        this.currentFormatter = new PhoneNumberFormatter();

        // Create an explicit formatter for New Zealand. 
        PhoneNumberFormatter.TryCreate("NZ", out this.NZFormatter);
    }

    private void phoneNumberInputTextBox_TextChanged(object sender, TextChangedEventArgs e)
    {
        // Format for the default region.
        this.phoneNumberOutputTextBlock.Text = currentFormatter.FormatPartialString(this.phoneNumberInputTextBox.Text);

        // If the NZFormatter was created successfully, format the partial string for the NZ TextBlock.
        if(this.NZFormatter != null)
        {
            this.phoneNumberOutputTextBlockNZ.Text = this.NZFormatter.FormatPartialString(this.phoneNumberInputTextBox.Text);
        }
    }
```    

Vous pouvez tester le code ci-dessous sur votre propre PC en modifiant le pays ou la région depuis **Paramètres** > **Heure et langue** > **Région et langue** > **Pays ou région**. Choisissez un pays ou une région (par exemple, la Nouvelle-Zélande pour confirmer que les formats correspondent) et exécuter le code à nouveau. Pour les données test, vous pouvez effectuer une recherche sur le Web concernant le numéro de téléphone d'une entreprise en Nouvelle-Zélande.

## <a name="the-users-language-and-cultural-preferences"></a>Les préférences linguistiques et culturelles de l'utilisateur

Pour les scénarios dans lesquels les fonctionnalités offertes varient seulement en fonction des préférences définies par l’utilisateur pour la langue, la région ou la culture, Windows fournit la classe [**Windows.System.UserProfile.GlobalizationPreferences**](/uwp/api/windows.system.userprofile.globalizationpreferences?branch=live) qui permet d’accéder à ces préférences. Si nécessaire, utilisez la classe **GlobalizationPreferences** pour obtenir les préférences actuelles de l’utilisateur (emplacement géographique, langues par défaut, devises par défaut, etc.). Mais n'oubliez pas que si les chaînes/images de votre applications ne sont pas localisées pour la langue favorite de l'utilisateur, alors les dates, heures et autres données formatées pour cette langue favorite ne correspondront pas aux chaînes que vous affichez.

## <a name="important-apis"></a>API importantes

* [DateTimeFormatter](/uwp/api/windows.globalization.datetimeformatting?branch=live)
* [NumberFormatting](/uwp/api/windows.globalization.numberformatting?branch=live)
* [Calendrier](/uwp/api/windows.globalization.calendar?branch=live)
* [PhoneNumberFormatting](/uwp/api/windows.globalization.phonenumberformatting?branch=live)
* [GlobalizationPreferences](/uwp/api/windows.system.userprofile.globalizationpreferences?branch=live)

## <a name="related-topics"></a>Rubriquesassociées

* [Contrôles de calendrier, de date et d’heure](../controls-and-patterns/date-and-time.md)
* [Comprendre les langues de profil utilisateur et les langues du manifeste de l’application](manage-language-and-region.md)
* [Adaptez vos ressources pour la langue, l’échelle, le contraste élevé et d’autres qualificateurs](../../app-resources/tailor-resources-lang-scale-contrast.md)

## <a name="samples"></a>Exemples

* [Exemple de détails et d’opérations mathématiques dans un calendrier](http://go.microsoft.com/fwlink/p/?linkid=231636)
* [Exemple de mise en forme des dates et heures](http://go.microsoft.com/fwlink/p/?linkid=231618)
* [Exemple de préférences de globalisation](http://go.microsoft.com/fwlink/p/?linkid=231608)
* [Exemple de mise en forme et d’analyse des nombres](http://go.microsoft.com/fwlink/p/?linkid=231620)
