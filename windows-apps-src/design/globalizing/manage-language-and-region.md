---
Description: Cette rubrique définit la liste de termes utilisateur profil langue, liste de langue manifeste d’application et liste des langues application runtime. Nous utiliserons ces termes dans cette rubriques et parmi d'autres concernant les fonctionnalités. Il est donc important que vous en compreniez le sens.
title: Comprendre les langues de profil utilisateur et les langues du manifeste de l’application
ms.assetid: 22D3A937-736A-4121-8285-A55DED56E594
template: detail.hbs
ms.date: 11/08/2017
ms.topic: article
keywords: windows 10, uwp, globalisation, adaptabilité, localisation
ms.localizationpriority: medium
ms.openlocfilehash: d782e8cd64cb976df964c72199964c1d349d527e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57605574"
---
# <a name="understand-user-profile-languages-and-app-manifest-languages"></a>Comprendre les langues de profil utilisateur et les langues du manifeste de l’application
Pour configurer une liste ordonnée de langues d'affichage ou simplement une seule langue d'affichage favorites, les utilisateurs Windows peuvent accéder à **Paramètres** > **Heure et langue** > **Région et langue**. Les langues peuvent comporter des variantes régionales. Par exemple, vous pouvez sélectionner l'espagnol d'Espagne, l'espagnol du Mexique, l'espagnol des États-Unis, etc.

Ils peuvent également suivre le chemin **Paramètres** > **Heure et langue** > **Région et langue** mais en dehors de la langue, les utilisateurs peuvent spécifier leur localisation (appelée Région) dans le monde. Notez que le paramètre de la langue d'affichage (et de la variante régionale) n'est pas déterminant dans le paramètre de région, et vice versa. Par exemple, un utilisateur peut habiter actuellement en France mais choisir Español (México) en tant que langue d'affichage Windows favorite.

Pour les applications UWP, une langue est représentée sous la forme d’une [balise de langue BCP-47](https://go.microsoft.com/fwlink/p/?linkid=227302). Par exemple, la balise de langue BCP-47 « en-US » correspond à l'anglais (États-Unis) dans **Paramètres**. Les API UWP adéquates acceptent et retournent les représentations de chaînes des balises de langue BCP-47.

Consultez également le [registre des sous-balises de langues IANA](https://go.microsoft.com/fwlink/p/?linkid=227303).

Les trois sections suivantes définissent les termes « liste de langues de profil utilisateur », « liste de langues de manifeste d'application » et « liste de langues d'exécution de l'application ». Nous utiliserons ces termes dans cette rubriques et parmi d'autres concernant les fonctionnalités. Il est donc important que vous en compreniez le sens.

## <a name="user-profile-language-list"></a>Liste de langues de profil utilisateur
La liste de langue de profil utilisateur désigne la liste qui est configurée par l'utilisateur dans **Paramètres** > **Heure et langue** > **Région et langues** > **Langues**. Dans le code, vous pouvez utiliser la propriété [**GlobalizationPreferences.Languages**](/uwp/api/windows.system.userprofile.globalizationpreferences.Languages) pour accéder à la liste de langue de profil utilisateur en tant que liste des chaînes en lecture seule, dans laquelle chaque chaîne est une unique [balise de langue BCP-47](https://go.microsoft.com/fwlink/p/?linkid=227302) telle que « en-US » ou « ja-JP ».

```csharp
    IReadOnlyList<string> userLanguages = Windows.System.UserProfile.GlobalizationPreferences.Languages;
```

## <a name="app-manifest-language-list"></a>Liste de langues de manifeste d’application
La liste de langues de manifeste d'application désigne la liste des langues que votre application déclare (ou déclarera) prendre en charge. Cette liste se développe à mesure des progrès de votre application tout au long du cycle de vie du développement, jusqu'à la localisation.

La liste est déterminée lors de la compilation, mais vous disposez de deux options pour contrôler le cours exact des événements. La première option consiste à laisser Visual Studio déterminer la liste à partir des fichiers de votre projet. Pour cela, définissez d'abord la **Langue par défaut** de votre application dans l'onglet **Application** du fichier source du manifeste du package d'application (`Package.appxmanifest`). Ensuite, vérifiez que le même fichier contient cette configuration (comportement par défaut).

```xml
  <Resources>
    <Resource Language="x-generate" />
  </Resources>
```

Chaque fois que Visual Studio produit votre fichier du manifeste du package d'application intégré (`AppxManifest.xml`), il étend cet élément `Resource` unique dans le fichier source dans une union de tous les qualificateurs de langue trouvés dans votre projet (consultez [Adaptez vos ressources pour la langue, l'échelle, le contraste élevé et d'autres qualificateurs](../../app-resources/tailor-resources-lang-scale-contrast.md)). Par exemple, si vous avez commencé à localiser et que vous disposez de ressources de chaînes, d'image et/ou de fichier dont les noms de dossier ou de fichier comprennent « en-US», « ja-JP » et « fr-FR », alors votre fichier `AppxManifest.xml` intégré contiendra le suivant (la première saisie dans la liste est la langue par défaut que vous avez définie).

```xml
  <Resources>
    <Resource Language="EN-US" />
    <Resource Language="JA-JP" />
    <Resource Language="FR-FR" />
  </Resources>
```

La deuxième option consiste à remplacer cet élément « généré par x » `<Resource>` unique dans votre fichier source du manifeste du package d'application (`Package.appxmanifest`) dans la liste étendue d'éléments `<Resource>` (en faisant attention à répertorier la langue par défaut en premier lieu). Cette option implique davantage de maintenance, mais elle est probablement la plus appropriée si vous utilisez un système intégré personnalisé.

Pour commencer, votre liste de langues de manifeste d'application contient une seule langue. Il s'agit peut-être de en-US. Mais finalement&mdash;à mesure que vous configurez manuellement votre manifeste ou ajoutez des ressources traduites à votre projet&mdash;cette liste se développe.

Lorsque votre application est présente dans le Microsoft Store, les langues de la liste de langues du manifeste d'application sont les langues affichées aux clients. Pour obtenir la liste des balises de langue BCP-47 prises en charge par le Microsoft Store, consultez [Langues prises en charge](../../publish/supported-languages.md).

Dans le code, vous pouvez utiliser la propriété [**ApplicationLanguages.ManifestLanguages**](/uwp/api/windows.globalization.applicationlanguages.ManifestLanguages) pour accéder à la liste de langues du manifeste d'application en tant que liste de chaînes en lecture seule, dans laquelle chaque chaîne est une balise de langue BCP-47 unique.

```csharp
    IReadOnlyList<string> userLanguages = Windows.Globalization.ApplicationLanguages.ManifestLanguages;
```

## <a name="app-runtime-language-list"></a>Liste de langues d'exécution d’application
La troisième liste d'intérêt de langue constitue l'intersection des deux listes que nous venons de décrire. Lors de l'exécution, la liste des langues pour lesquels votre application a déclaré une prise en charge (la liste de langues du manifeste d'application) est comparée à la liste des langues pour lesquels l'utilisateur a indiqué une préférence (la liste de langues de profil utilisateur). La liste de langues d'exécution d'application est définie à cette intersection (si l'intersection n'est pas vide) ou simplement sur la langue par défaut de l'application (si l'intersection est vide).

De manière plus spécifique, la liste de langues d'exécution d'application est constituée de ces éléments.

1.  **(Facultatif) Remplacement de la langue principale**. Le paramètre [**PrimaryLanguageOverride**](/uwp/api/Windows.Globalization.ApplicationLanguages.PrimaryLanguageOverride) est un paramètre de substitution simple pour les applications qui donnent aux utilisateurs leur propre choix de langue indépendant ou les applications qui ont une bonne raison de remplacer les langues par défaut. Pour en savoir plus, voir [Exemple de ressources d’application et de localisation](https://go.microsoft.com/fwlink/p/?linkid=231501).
2.  **Les langues de l'utilisateur qui sont prises en charge par l'application**. Il s'agit de la liste de langues de profile utilisateur filtrée par la liste de langues du manifeste d'application. Le filtrage des langues de l’utilisateur par celles prises en charge par l’application préserve la cohérence entre les Kits de développement logiciel (SDK), les bibliothèques de classes, les packages d’infrastructure dépendants et l’application.
3.  **Si 1 et 2 sont vides, la langue par défaut ou la première langue prise en charge par l’application**. Si la liste de langues du profil utilisateur ne contient aucune des langues prises en charge par l’application, la langue d'exécution de l’application est la première langue prise en charge par l’application.

Dans le code, vous pouvez utilisez la propriété [ResourceContext.QualifierValues](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.QualifierValues) pour accéder à la liste de langues d'exécution d'application sous la forme d'une chaîne contenant une liste des balises de langue BCP-47 délimitée par des points-virgules.

```csharp
    string runtimeLanguages = Windows.ApplicationModel.Resources.Core.ResourceContext.GetForCurrentView().QualifierValues["Language"];
```

Vous pouvez également accéder à cette liste de chaînes en lecture seule, chacune des chaîne contenant une balise de langue BCP-47 unique. Pour cela, vous pouvez utilisez la propriété [**ResourceContext.Languages**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.Languages) ou la propriété [**ApplicationLanguages.Languages**](/uwp/api/windows.globalization.applicationlanguages.Languages).

```csharp
    IReadOnlyList<string> runtimeLanguages = Windows.ApplicationModel.Resources.Core.ResourceContext.GetForCurrentView().Languages;

    runtimeLanguages = Windows.Globalization.ApplicationLanguages.Languages;
```

La liste de langues d'exécution d'application détermine les ressources que Windows charge pour votre application. Elle détermine également les langues utilisées pour formater les dates, heures, chiffres et autres composants. Consultez [Globaliser vos formats de date/heure/chiffres](use-global-ready-formats.md).

**Remarque** Si la langue du profil utilisateur et la langue du manifeste d'application sont des variantes régionales l'une de l'autre, la variante régionale de l'utilisateur est utilisée en tant que langue d'exécution d'application. Par exemple, si l’utilisateur préfère l'en-GB alors que l’application prend en charge l'en-US, la langue d'exécution d'application est donc en-GB. Cela garantit que les dates, les heures et les nombres seront formatés d’une manière plus proche des attentes de l’utilisateur (en-GB), mais les ressources localisées continuent à être chargées (en raison de la mise en correspondance des langues) dans la langue prise en charge par l’application (en-US).

## <a name="qualify-resource-files-with-their-language"></a>Qualifier les fichiers de ressource avec leur langue
Nommez vos fichiers de ressources ou leurs dossiers avec les qualificateurs de ressource linguistique. Pour en savoir plus sur les qualificateurs de ressource, consultez [Adaptez vos ressources pour la langue, l’échelle, le contraste élevé et d’autres qualificateurs](../../app-resources/tailor-resources-lang-scale-contrast.md)). Un fichier de ressources peut être une image (ou autre ressource), ou il peut être un fichier de conteneur de ressources, comme un *.resw* qui contient les chaînes de texte.

**Remarque** même les ressources dans la langue par défaut de votre application doivent spécifier le qualificateur de langue. Par exemple, si la langue par défaut de votre application est anglais (États-Unis), qualifiez ensuite vos ressources en tant que `\Assets\Images\en-US\logo.png`.

- Windows effectue une correspondance complexes, y compris au-delà des variantes régionaux telles qu’en-US et en-GB. Par conséquent, incluez l’étiquette de sous-chemin de région comme il convient. Consultez [Comment le système de gestion des ressources met en correspondance les balises de langue](../../app-resources/how-rms-matches-lang-tags.md).
- Spécifiez une balise de sous-chemin de script de langue dans le champ qualificateur lorsqu’il n’existe aucune valeur supprimer-Script défini pour le langage. Par exemple, au lieu de zh-CN ou zh-TW, utiliser zh-Hant, zh-Hant-TW ou zh-Hans (pour plus d’informations, consultez le [Registre de sous-balises de langage IANA](https://go.microsoft.com/fwlink/p/?linkid=227303)).
- Pour les langages qui ont un dialecte standard unique, il est inutile d’inclure le qualificateur de la région. Par exemple, utilisez ja plutôt que ja-JP.
- Certains outils et composants tels que les outils de traduction automatique peuvent trouver des balises de langue spécifiques, telles que des informations de langue régionale, ce qui s’avère utile pour analyser certaines données.

### <a name="not-all-resources-need-to-be-localized"></a>Pas toutes les ressources doivent être localisées

Localisation ne peut pas être nécessaire pour toutes les ressources.

- Au minimum, vérifiez que toutes les ressources existent dans la langue par défaut.
- Un sous-ensemble de certaines ressources peut suffire pour un langage étroitement lié (localisation partielle). Par exemple, il n'est peut-être pas possible de localiser intégralité de l’interface utilisateur d’une application en catalan si l’application dispose d’un ensemble complet de ressources en espagnol. Pour les utilisateurs qui parlent Catalan, puis sur espagnol, les ressources qui ne sont pas disponibles dans Catalan apparaissent en espagnol.
- Certaines ressources peuvent nécessiter des exceptions pour des langues spécifiques, bien que la majorité des autres ressources de carte d’à une ressource commune. Dans ce cas, marquer la ressource doit être utilisé pour toutes les langues avec la balise de langue indéterminé « und ». Windows interprète la balise de langue « und » comme caractère générique (semblable à «\*») dans la mesure où il correspond à la langue d’application supérieure après n’importe quel autre correspondance spécifique. Par exemple, si quelques ressources sont différentes pour le finnois, alors que les autres ressources sont les mêmes pour toutes les langues, la ressource en finnois doit être marquée avec la balise de langue pour le finnois et les autres ressources doivent être marquées avec 'und'.
- Pour les ressources qui sont basées sur un script de langue, tel qu’une police ou la hauteur du texte, utilisent la balise de langue indéterminé avec un script spécifié : « und -&lt;script&gt;». Par exemple, pour les polices latines, utilisez `und-Latn\\fonts.css`. Pour les polices cyrilliques, utilisez `und-Cryl\\fonts.css`.

## <a name="set-the-http-accept-language-request-header"></a>Définissez l’en-tête de requête HTTP Accept-Language.
Soyez attentif aux services Web que vous appelez et ayant la même étendue de localisation que votre application. Les requêtes HTTP envoyées à partir d’applications UWP et d’applications de bureau dans des requêtes web standard et XMLHttpRequest (XHR) utilisent la requête d'en-tête HTTP Accept-Language standard. Par défaut, l'en-tête HTTP est définie selon la liste de langues de profil utilisateur. Chaque langue de la liste est développée pour inclure les éléments neutres de la langue et un poids (q). Par exemple, la liste des langues d’un utilisateur comprenant fr-FR et en-US produit un en-tête de requête Accept-Language HTTP comprenant fr-FR, fr, en-US, en (« fr-FR,fr;q=0.8,en-US;q=0.5,en;q=0.3 »). Néanmoins, si votre application météo (par exemple) affiche l'interface utilisateur en français (France) mais que la première langue de la liste de langues favorites de l'utilisateur et l'allemand, vous devez explicitement demander au service le français (France) afin de rester cohérent dans votre application.

## <a name="apis-in-the-windowsglobalization-namespace"></a>API de l’espace de noms Windows.Globalization
En règle générale, les API de l’espace de noms [**Windows.Globalization**](/uwp/api/windows.globalization?branch=live) utilisent la liste de langues d'exécution d’application pour déterminer la langue. Si aucune de ces langues ne possède de format correspondant, alors les paramètres régionaux de l’utilisateur sont utilisés. Il s’agit des paramètres régionaux utilisés pour l’horloge système. Les paramètres régionaux utilisateur sont disponible à partir de **paramètres** > **heure et langue** > **région et langue**  >  **Supplémentaire date, heure et les paramètres régionaux** > **région : Modifier la date, d’heure ou de formats de nombres**. Les API **Windows.Globalization** permettent aussi les substitution pour spécifier une liste de langues à utiliser à la place de la liste de langues d'exécution d'application.

L'utilisation de la classe [**Langue**](/uwp/api/windows.globalization.language?branch=live) vous permet d’inspecter une langue particulière, notamment le script de la langue, le nom complet et le nom natif.

## <a name="use-geographic-region-when-appropriate"></a>Utiliser la région lorsque cela est approprié
Dans **Paramètres** > **Heure et langue** > **Région et langue** > **Pays ou région**, l'utilisateur peut spécifier sa localisation dans le monde. Vous pouvez utiliser ce paramètre au lieu de la langue pour choisir le contenu à présenter à l’utilisateur. Par exemple, une application d'actualités peut afficher par défaut le contenu pour cette région.

Dans le code, vous pouvez accéder à ce paramètre à l’aide de la propriété [**Windows.System.UserProfile.GlobalizationPreferences.HomeGeographicRegion**](/uwp/api/windows.system.userprofile.globalizationpreferences.HomeGeographicRegion).

L'utilisation de la classe [**GeographicRegion**](/uwp/api/windows.globalization.geographicregion?branch=live) vous permet d’inspecter les détails relatifs à une région particulière, tels que son nom d’affichage, son nom natif et les devises utilisées.

## <a name="examples"></a>Exemples
Le tableau ci-dessous contient des exemples de ce que l’utilisateur verrait dans l’interface utilisateur de votre application lorsque différents paramètres de langue et de région sont configurés.

<table border="1">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="20%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Liste de langues de manifeste d’application</th>
<th align="left">Liste de langues de profil utilisateur</th>
<th align="left">Langue principale de substitution de l’application (en option)</th>
<th align="left">Liste de langues d'exécution d’application</th>
<th align="left">Ce que l’utilisateur voit dans l’application</th>
</tr>
</thead>
<tbody>
<tr>
<td align="left">Anglais (GB) (par défaut) ; allemand (Allemagne)</td>
<td align="left">Anglais (GB)</td>
<td align="left">aucune</td>
<td align="left">Anglais (GB)</td>
<td align="left">INTERFACE UTILISATEUR : Anglais (GB)<br>Dates/heures/nombres : Anglais (GB)</td>
</tr>
<tr>
<td align="left">Allemand (Allemagne) (par défaut) ; français (France) ; italien (Italie)</td>
<td align="left">Français (Autriche)</td>
<td align="left">aucune</td>
<td align="left">Français (Autriche)</td>
<td align="left">INTERFACE UTILISATEUR : Français (France) (de secours du Français (Autriche))<br>Dates/heures/nombres : Français (Autriche)</td>
</tr>
<tr>
<td align="left">Anglais (US) (par défaut) ; français (France) ; anglais (GB)</td>
<td align="left">Anglais (Canada) ; français (Canada)</td>
<td align="left">aucune</td>
<td align="left">Anglais (Canada) ; français (Canada)</td>
<td align="left">INTERFACE UTILISATEUR : Anglais (États-Unis) (de secours de l’anglais (Canada))<br>Dates/heures/nombres : Anglais (Canada)</td>
</tr>
<tr>
<td align="left">Espagnol (Espagne) (par défaut) ; espagnol (Mexique) ; espagnol (Amérique latine) ; portugais (Brésil)</td>
<td align="left">Anglais (É.U.)</td>
<td align="left">aucune</td>
<td align="left">Espagnol (Espagne)</td>
<td align="left">INTERFACE UTILISATEUR : Espagnol (Espagne) (utilise par défaut depuis aucun secours disponible pour l’anglais)<br>Dates/Heures/Nombres : espagnol (Espagne)</td>
</tr>
<tr>
<td align="left">Catalan (par défaut) ; espagnol (Espagne) ; français (France)</td>
<td align="left">Catalan ; français (France)</td>
<td align="left">aucune</td>
<td align="left">Catalan ; français (France)</td>
<td align="left">INTERFACE UTILISATEUR : Principalement Catalan et certains Français (France) étant pas toutes les chaînes dans Catalan<br>Dates/heures/nombres : Catalan</td>
</tr>
<tr>
<td align="left">Anglais (GB) (par défaut) ; français (France) ; allemand (Allemagne)</td>
<td align="left">Allemand (Allemagne) ; anglais (GB)</td>
<td align="left">Anglais (GB) (choisi par l’utilisateur dans l’interface utilisateur de l’application)</td>
<td align="left">Anglais (GB) ; allemand (Allemagne)</td>
<td align="left">INTERFACE UTILISATEUR : Anglais (en Go) (remplacement du langage)<br>Dates/Heures/Nombres : anglais (GB)</td>
</tr>
</tbody>
</table>

>[!NOTE]
> Pour obtenir la liste des codes de pays/région standard utilisés par Microsoft, consultez le [liste de pays/région officielle](https://globalready.azurewebsites.net/marketreadiness/OfficialCountryregion).

## <a name="important-apis"></a>API importantes
* [GlobalizationPreferences.Languages](/uwp/api/windows.system.userprofile.globalizationpreferences.Languages)
* [ApplicationLanguages.ManifestLanguages](/uwp/api/windows.globalization.applicationlanguages.ManifestLanguages)
* [PrimaryLanguageOverride](/uwp/api/Windows.Globalization.ApplicationLanguages.PrimaryLanguageOverride)
* [ResourceContext.QualifierValues](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.QualifierValues)
* [ResourceContext.Languages](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.Languages)
* [ApplicationLanguages.Languages](/uwp/api/windows.globalization.applicationlanguages.Languages)
* [Windows.Globalization](/uwp/api/windows.globalization?branch=live)
* [Langue](/uwp/api/windows.globalization.language?branch=live)
* [GlobalizationPreferences.HomeGeographicRegion](/uwp/api/windows.system.userprofile.globalizationpreferences.HomeGeographicRegion)
* [GeographicRegion](/uwp/api/windows.globalization.geographicregion?branch=live)

## <a name="related-topics"></a>Rubriques connexes
* [Balise de langue de BCP-47](https://go.microsoft.com/fwlink/p/?linkid=227302)
* [Registre de sous-balises de langage IANA](https://go.microsoft.com/fwlink/p/?linkid=227303)
* [Personnaliser vos ressources pour la langue, l’échelle, le contraste élevé et d’autres qualificateurs](../../app-resources/tailor-resources-lang-scale-contrast.md)
* [Langues prises en charge](../../publish/supported-languages.md)
* [Globaliser vos formats de date/heure/nombre](use-global-ready-formats.md)
* [Comment le système de gestion des ressources met en correspondance les balises de langue](../../app-resources/how-rms-matches-lang-tags.md)

## <a name="samples"></a>Exemples
* [Exemple de localisation et de ressources d’application](https://go.microsoft.com/fwlink/p/?linkid=231501)
