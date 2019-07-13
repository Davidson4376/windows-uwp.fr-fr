---
title: Préparer votre application pour le changement d’ère du Japon
description: Découvrez le changement d’ère du Japon de mai 2019 et comment préparer votre application.
ms.assetid: 5A945F9A-8632-4038-ADD6-C0568091EF27
ms.date: 04/26/2019
ms.topic: article
keywords: windows 10, uwp, adaptabilité, localisation, Japon, ère
ms.localizationpriority: high
ms.openlocfilehash: 7e8250ccae96ed835aba2a2a993fdde9ae31a884
ms.sourcegitcommit: 139717a79af648a9231821bdfcaf69d8a1e6e894
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67714121"
---
# <a name="prepare-your-application-for-the-japanese-era-change"></a>Préparer votre application pour le changement d’ère du Japon

> [!NOTE]
> Le nom de la nouvelle ère a été annoncé le 1er avril 2019 : Reiwa (令和). Le 25 avril, Microsoft a publié des packages pour les différents systèmes d’exploitation Windows contenant la clé de Registre mise à jour avec le nom de la nouvelle ère. Mettez à jour votre appareil, vérifiez votre Registre pour voir s’il possède la nouvelle clé, puis testez votre application. Consultez [cet article de support](https://support.microsoft.com/help/4469068/summary-of-new-japanese-era-updates-kb4469068) pour vous assurer que votre système d’exploitation doit avoir reçu la clé de Registre mise à jour.

Le calendrier japonais est divisé en ères. Depuis presque le début de l’âge informatique moderne, nous sommes dans l’ère Heisei ; toutefois, le 1er mai 2019, une nouvelle ère a commencé. Dans la mesure où il s’agit de la première fois depuis plusieurs décennies qu’une ère change, les logiciels prenant en charge le calendrier japonais doivent être testés, pour vérifier qu’ils fonctionnent correctement depuis le début de la nouvelle ère.

Dans les sections suivantes, vous allez découvrir ce que vous pouvez faire pour préparer et tester votre application pour la nouvelle ère.

> [!NOTE]
> Nous vous recommandons d’utiliser un ordinateur de test pour ce faire, car les modifications que vous apporterez auront un impact sur le comportement de l’intégralité de la machine.

## <a name="add-a-registry-key-for-the-new-era"></a>Ajouter une clé de Registre pour la nouvelle ère

> [!NOTE]
> Les instructions suivantes sont destinées aux appareils qui n’ont pas encore été mis à jour avec la nouvelle clé de Registre. Tout d’abord, vérifiez si votre appareil contient la clé de Registre et, si ce n’est pas le cas, procédez au test en suivant les instructions suivantes.

Il est important de tester les problèmes de compatibilité avant le changement d’ère, et vous pouvez le faire maintenant à l’aide du nom de la nouvelle ère. Pour ce faire, ajoutez une clé de Registre pour la nouvelle ère dans l’**Éditeur du Registre** :

1. Accédez à **Ordinateur\HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Nls\Calendars\Japanese\Eras**.
2. Sélectionnez **Édition > Nouveau > Valeur de chaîne** et donnez-lui le nom **2019 05 01**.
3. Cliquez avec le bouton droit sur la clé, puis sélectionnez **Modifier**.
4. Dans le champ **Données de la valeur**, entrez **令和_令_Reiwa_R** (vous pouvez faire un copier-coller à partir d’ici pour faciliter la tâche).

Consultez [Gestion des ères pour le calendrier japonais](https://docs.microsoft.com/windows/desktop/Intl/era-handling-for-the-japanese-calendar) pour en savoir plus sur le format de ces clés de Registre.

Le nom de la nouvelle ère a été annoncé le 1er avril 2019. Le 25 avril, une mise à jour avec une nouvelle clé de Registre pour les versions Windows prises en charge contenant le nom a été publiée, ce qui vous permet de valider que votre application le gère correctement. Cette mise à jour est répercutée aux versions antérieures de Windows 10 ainsi que Windows 8 et 7.

Vous pouvez supprimer votre clé de Registre d’espace réservé une fois que vous avez terminé de tester votre application. Cela permet de garantir qu’elle n’interfère pas avec la nouvelle clé de Registre qui est ajoutée lors de la mise à jour de Windows.

## <a name="change-your-devices-calendar-format"></a>Changer le format de calendrier de votre appareil

Une fois que vous avez ajouté la clé de Registre pour la nouvelle ère, vous devez configurer votre appareil pour utiliser le calendrier japonais. Vous n’avez pas besoin de disposer d’un appareil en japonais pour effectuer cette opération. Pour réaliser des tests approfondis, vous pouvez installer le module linguistique japonais également, mais cela n’est pas nécessaire pour le test de base.

Pour configurer votre appareil pour utiliser le calendrier japonais :

1. Ouvrez **intl.cpl** (recherchez ce fichier dans la barre de recherche Windows).
2. Dans la liste déroulante **Format**, sélectionnez **Japonais (Japon)** .
3. Sélectionnez **Paramètres supplémentaires**.
4. Sélectionnez l’onglet **Date**.
5. Dans la liste déroulante **Type de calendrier**, sélectionnez **和暦** (*wareki*, le calendrier japonais). Il doit être la deuxième option.
6. Cliquez sur **OK**.
7. Cliquez sur **OK** dans la fenêtre **Région**.

Votre appareil doit maintenant être configuré pour utiliser le calendrier japonais, et qu’il reflète les ères présentes dans le Registre. Voici un exemple de ce que vous pouvez voir maintenant dans l’angle inférieur droit de l’écran :

![Date et heure au format de calendrier japonais](images/japanese-calendar-format.png)

## <a name="adjust-your-devices-clock"></a>Ajuster l’horloge de votre appareil

Pour vérifier que votre application fonctionne avec la nouvelle ère, vous devez avancer l’horloge de votre ordinateur au 1er mai 2019 ou à une date ultérieure. Les instructions suivantes concernent Windows 10, mais Windows 8 et 7 devraient fonctionner de la même façon :

1. Cliquez avec le bouton droit dans la zone de date et d’heure dans l’angle inférieur droit de l’écran.
2. Sélectionnez **Ajuster la date/l’heure**.
3. Dans l’application Paramètres, sous **Modifier la date et l’heure**, sélectionnez **Modifier**.
4. Définissez la date sur le 1er mai 2019 ou sur une date ultérieure.

> [!NOTE]
> Il est possible que les paramètres de votre organisation vous empêchent de modifier la date ; si tel est le cas, contactez votre administrateur. Vous pouvez également définir votre clé de Registre d’espace réservé sur une date passée.

## <a name="test-your-application"></a>Tester votre application

Maintenant, testez la façon dont votre application gère la nouvelle ère. Vérifiez les emplacements où s’affiche la date, comme les horodatages et les sélecteurs de date. Assurez-vous que l’ère est correcte avant le 1er mai 2019 (Heisei, 平成) et après (Reiwa, 令和).

### <a name="gannen-"></a>*Gannen* (元年)

Le format du calendrier japonais est généralement **&lt;Nom de l’ère&gt; &lt;Année de l’ère&gt;** . Par exemple, l’année 2018 correspond à **Heisei 30** (平成30年).  Toutefois, la première année d’une ère est spéciale ; au lieu d’être **&lt;Nom de l’ère&gt; 1**, elle correspond à **&lt;Nom de l’ère&gt; 元年** (*gannen*). Ainsi, la première année de l’ère Heisei serait 平成元年 (*Heisei gannen*). Assurez-vous que votre application gère correctement la première année de la nouvelle ère et affiche correctement 令和元年.

## <a name="related-apis"></a>API associées

Il existe plusieurs API WinRT, .NET et Win32 qui seront mises à jour pour gérer le changement d’ère ; si vous les utilisez, vous ne devriez pas rencontrer de problèmes. Toutefois, même si vous vous reposez entièrement sur ces API, il est toujours judicieux de tester votre application et de vérifier que vous obtenez le comportement souhaité, surtout si vous réalisez des opérations particulières, telles qu’une analyse.

Vous pouvez suivre les mises à jour apportées au système d’exploitation et aux SDK sur la page [Mises à jour pour le changement d’ère du Japon en mai 2019](https://support.microsoft.com/help/4470918/updates-for-may-2019-japan-era-change).

Les API suivantes seront impactées :

### <a name="winrt"></a>WinRT

* [Espace de noms Windows.Globalization](https://docs.microsoft.com/uwp/api/windows.globalization)
  * [Classe Calendar](https://docs.microsoft.com/uwp/api/windows.globalization.calendar)
    * [Méthode AddDays](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.adddays)
    * [Méthode AddEras](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.adderas)
    * [Méthode AddHours](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.addhours)
    * [Méthode AddMinutes](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.addminutes)
    * [Méthode AddMonths](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.addmonths)
    * [Méthode AddNanoseconds](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.addnanoseconds)
    * [Méthode AddPeriods](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.addperiods)
    * [Méthode AddSeconds](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.addseconds)
    * [Méthode AddWeeks](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.addweeks)
    * [Méthode AddYears](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.addyears)
    * [Propriété Era](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.era)
    * [Méthode EraAsString](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.eraasstring)
    * [Propriété FirstYearInThisEra](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.firstyearinthisera)
    * [Propriété LastEra](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.lastera)
    * [Propriété LastYearInThisEra](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.lastyearinthisera)
    * [Propriété NumberOfYearsInThisEra](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.numberofyearsinthisera)
* [Espace de noms Windows.Globalization.DateTimeFormatting](https://docs.microsoft.com/uwp/api/windows.globalization.datetimeformatting)
  * [Classe DateTimeFormatter](https://docs.microsoft.com/uwp/api/windows.globalization.datetimeformatting.datetimeformatter)
    * [Méthode Format](https://docs.microsoft.com/uwp/api/windows.globalization.datetimeformatting.datetimeformatter.format)

### <a name="net"></a>.NET

* [Espace de noms System](https://docs.microsoft.com/dotnet/api/system)
  * [Struct DateTime](https://docs.microsoft.com/dotnet/api/system.datetime)
  * [Struct DateTimeOffset](https://docs.microsoft.com/dotnet/api/system.datetimeoffset)
* [Espace de noms System.Globalization](https://docs.microsoft.com/dotnet/api/system.globalization)
  * [Classe Calendar](https://docs.microsoft.com/dotnet/api/system.globalization.calendar)
  * [Classe DateTimeFormatInfo](https://docs.microsoft.com/dotnet/api/system.globalization.datetimeformatinfo)
  * [Classe JapaneseCalendar](https://docs.microsoft.com/dotnet/api/system.globalization.japanesecalendar)
  * [Classe JapaneseLunisolarCalendar](https://docs.microsoft.com/dotnet/api/system.globalization.japaneselunisolarcalendar)

### <a name="win32"></a>Win32

* [En-tête datetimeapi.h](https://docs.microsoft.com/windows/desktop/api/datetimeapi/)
  * [Fonction GetDateFormatA](https://docs.microsoft.com/windows/desktop/api/datetimeapi/nf-datetimeapi-getdateformata)
  * [Fonction GetDateFormatEx](https://docs.microsoft.com/windows/desktop/api/datetimeapi/nf-datetimeapi-getdateformatex)
  * [Fonction GetDateFormatW](https://docs.microsoft.com/windows/desktop/api/datetimeapi/nf-datetimeapi-getdateformatw)
* [En-tête winnls.h](https://docs.microsoft.com/windows/desktop/api/winnls/)
  * [Fonction EnumDateFormatsA](https://docs.microsoft.com/windows/desktop/api/winnls/nf-winnls-enumdateformatsa)
  * [Fonction EnumDateFormatsExA](https://docs.microsoft.com/windows/desktop/api/winnls/nf-winnls-enumdateformatsexa)
  * [Fonction EnumDateFormatsExEx](https://docs.microsoft.com/windows/desktop/api/winnls/nf-winnls-enumdateformatsexex)
  * [Fonction EnumDateFormatsExW](https://docs.microsoft.com/windows/desktop/api/winnls/nf-winnls-enumdateformatsexw)
  * [Fonction EnumDateFormatsW](https://docs.microsoft.com/windows/desktop/api/winnls/nf-winnls-enumdateformatsw)
  * [Fonction GetCalendarInfoA](https://docs.microsoft.com/windows/desktop/api/winnls/nf-winnls-getcalendarinfoa)
  * [Fonction GetCalendarInfoEx](https://docs.microsoft.com/windows/desktop/api/winnls/nf-winnls-getcalendarinfoex)
  * [Fonction GetCalendarInfoW](https://docs.microsoft.com/windows/desktop/api/winnls/nf-winnls-getcalendarinfow)

## <a name="see-also"></a>Voir également

* [Gestion d’ères pour le calendrier japonais](https://docs.microsoft.com/windows/desktop/Intl/era-handling-for-the-japanese-calendar)
* [Moment An 2000 du calendrier japonais](https://blogs.msdn.microsoft.com/shawnste/2018/04/12/the-japanese-calendars-y2k-moment/)
* [Utilisation du Registre pour tester la nouvelle ère japonaise sur Windows](https://blogs.msdn.microsoft.com/shawnste/2018/08/07/using-the-registry-to-test-the-new-japanese-era-on-windows/)
* [Gannen vs Ichinen](https://blogs.msdn.microsoft.com/shawnste/2018/11/12/gannen-vs-ichinen/)
* [Mises à jour pour le changement d’ère du Japon en mai 2019](https://support.microsoft.com/help/4470918/updates-for-may-2019-japan-era-change)
* [Gestion d’une nouvelle ère dans le calendrier japonais dans .NET](https://devblogs.microsoft.com/dotnet/handling-a-new-era-in-the-japanese-calendar-in-net/)
