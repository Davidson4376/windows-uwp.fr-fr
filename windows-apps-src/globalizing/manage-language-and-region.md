---
Contrôlez la façon dont Windows sélectionne les ressources de l’interface utilisateur et formate les éléments de l’interface utilisateur de l’application, en utilisant les différents paramètres de langue et de région fournis par Windows.
Gérer la langue et la région
ms.assetid: 22D3A937-736A-4121-8285-A55DED56E594
Gérer la langue et la région
template: detail.hbs
---

# Gérer la langue et la région


\[ Article mis à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]


**API importantes**

-   [**Windows.Globalization**](https://msdn.microsoft.com/library/windows/apps/br206813)
-   [**Windows.ApplicationModel.Resources**](https://msdn.microsoft.com/library/windows/apps/br206022)
-   [**Espace de noms WinJS.Resources**](https://msdn.microsoft.com/library/windows/apps/br229779)

Contrôlez la façon dont Windows sélectionne les ressources de l’interface utilisateur et formate les éléments de l’interface utilisateur de l’application, en utilisant les différents paramètres de langue et de région fournis par Windows.

## <span id="Introduction"> </span> <span id="introduction"> </span> <span id="INTRODUCTION"> </span>Introduction


Pour obtenir un exemple d’application montrant comment gérer les paramètres de langue et de région, voir [Exemple de ressources d’application et de localisation](http://go.microsoft.com/fwlink/p/?linkid=231501).

Un utilisateur Windows n’a pas besoin de choisir uniquement une langue dans un ensemble limité de langues. En revanche, l’utilisateur peut indiquer à Windows qu’il parle une langue donnée, même si Windows n’est pas traduit dans cette langue. L’utilisateur peut même spécifier qu’il peut parler plusieurs langues.

Un utilisateur Windows peut spécifier son emplacement géographique, qui peut être n’importe où dans le monde. En outre, l’utilisateur peut spécifier qu’il parle une langue quelconque dans un emplacement géographique quelconque. L’emplacement géographique et la langue ne se limitent pas mutuellement. Le simple fait qu’un utilisateur parle le français ne signifie pas qu’il se trouve en France et, réciproquement, le simple fait qu’un utilisateur est en France ne signifie pas qu’il préfère parler français.

Un utilisateur Windows peut exécuter des applications dans une langue complètement différente de celle de Windows. Par exemple, l’utilisateur peut exécuter une application en espagnol alors que Windows s’exécute en français.

Pour les applications du Windows Store, une langue est représentée sous la forme d’une [balise de langue BCP-47](http://go.microsoft.com/fwlink/p/?linkid=227302). La plupart des API de Windows Runtime, HTML et XAML peuvent renvoyer ou accepter la représentation sous forme de chaînes de ces balises de langue BCP-47. Consultez également la [liste de langues IANA](http://go.microsoft.com/fwlink/p/?linkid=227303).

Pour obtenir la liste des balises de langue prises en charge par le Windows Store, voir [Langues prises en charge](https://msdn.microsoft.com/library/windows/apps/jj657969).

## <span id="Tasks"> </span> <span id="tasks"> </span> <span id="TASKS"> </span>Tâches


### <span id="Users_can_set_their_language_preferences."> </span> <span id="users_can_set_their_language_preferences."> </span> <span id="USERS_CAN_SET_THEIR_LANGUAGE_PREFERENCES."> </span>Les utilisateurs peuvent définir leurs préférences linguistiques.

La liste des préférences linguistiques de l’utilisateur est une liste ordonnée des langues de l’utilisateur, classées par ordre de préférence.

L’utilisateur définit cette liste dans **Paramètres** &gt;**Heure et langue** &gt; **Région et langue**. Il peut également utiliser le **Panneau de configuration** &gt; **Horloge, langue et région**.

La liste des préférences linguistiques de l’utilisateur peut contenir plusieurs langues et des variantes régionales ou spécifiques. Par exemple, l’utilisateur peut préférer fr-CA, mais peut également comprendre en-GB.

### <span id="Specify_the_supported_languages_in_the_app_s_manifest."> </span> <span id="specify_the_supported_languages_in_the_app_s_manifest."> </span> <span id="SPECIFY_THE_SUPPORTED_LANGUAGES_IN_THE_APP_S_MANIFEST."> </span>Spécifiez les langues prises en charge dans le manifeste de l’application.

Spécifiez la liste des langues prises en charge de votre application dans [**Resources element**](https://msdn.microsoft.com/library/windows/apps/dn934770) du fichier manifeste de l’application (en général Package.appxmanifest), ou Visual Studio génère automatiquement la liste des langues dans le fichier manifeste en fonction des langues trouvées dans le projet. Le manifeste doit décrire précisément les langues prises en charge au niveau approprié de granularité. Les langues qui figurent dans le manifeste sont les langues présentées aux utilisateurs dans le Windows Store.

### <span id="Specify_the_default_language."> </span> <span id="specify_the_default_language."> </span> <span id="SPECIFY_THE_DEFAULT_LANGUAGE."> </span>Spécifiez la langue par défaut.

Ouvrez package.appxmanifest dans Visual Studio, accédez à l’onglet **Application** et définissez votre langue par défaut sur la langue que vous utilisez pour créer votre application.

Une application utilise la langue par défaut quand elle ne prend en charge aucune des langues que l’utilisateur a choisies. Visual Studio utilise la langue par défaut pour ajouter des métadonnées aux ressources marquées dans cette langue, ce qui permet aux ressources appropriées d’être choisies au moment de l’exécution.

La propriété de langue par défaut doit également être définie en tant que première langue dans le manifeste pour définir de façon appropriée la langue de l’application (décrite à l’étape Créez la liste des langues de l’application plus bas). Les ressources dans la langue par défaut doivent toujours être qualifiées avec leur langue (par exemple, en-US/logo.png). La langue par défaut ne spécifie pas la langue implicite des ressources non qualifiées. Pour plus d’informations, voir [Comment nommer des ressources à l’aide de qualificateurs](https://msdn.microsoft.com/library/windows/apps/xaml/hh965324).

### <span id="Qualify_resources_with_their_language."> </span> <span id="qualify_resources_with_their_language."> </span> <span id="QUALIFY_RESOURCES_WITH_THEIR_LANGUAGE."> </span>Qualifiez des ressources avec leur langue.

Définissez soigneusement votre public ainsi que la langue et l’emplacement des utilisateurs que vous voulez cibler. De nombreuses personnes vivant dans une région ne préfèrent pas utiliser la langue principale de cette région. Par exemple, la langue principale de millions de foyers aux États-Unis est l’espagnol.

Pour qualifier des ressources avec une langue :

-   Incluez un script lorsqu’il n’y a aucune valeur de script de suppression définie pour la langue. Voir le [registre des sous-balises de langues IANA](http://go.microsoft.com/fwlink/p/?linkid=227303) pour plus de détails sur les balises de langue. Par exemple, utilisez zh-Hant, zh-Hant-TW ou zh-Hans et non zh-CN ou zh-TW.
-   Marquez tout le contenu linguistique avec une langue. La propriété de projet de langue par défaut n’est pas la langue des ressources non marquées (c’est-à-dire, une langue neutre) ; elle indique quelle ressource de langue marquée doit être choisie si aucune autre ressource de langue marquée ne correspond à l’utilisateur.

Marquez les ressources avec une représentation précise du contenu.

-   Windows procède à une mise en correspondance complexe des langues, en prenant en compte les variantes régionales (par exemple en-US est mis en correspondance avec en-GB), pour que les applications puissent librement marquer les ressources avec une représentation précise du contenu, Windows faisant correspondre la langue appropriée pour chaque utilisateur.
-   Le Windows Store présente aux utilisateurs de l’application le contenu du manifeste.
-   Sachez que certains outils et composants tels que les outils de traduction automatique peuvent trouver des balises de langue spécifiques, telles que des informations de dialecte régional, ce qui s’avère utile pour analyser certaines données.
-   Veillez à marquer les ressources avec des informations complètes, surtout lorsque plusieurs variantes de langue sont disponibles. Par exemple, marquez les ressources avec en-GB et en-US si les deux langues sont spécifiques à la région.
-   Dans le cas des langues pour lesquelles existent un seul dialecte standard il n’est pas nécessaire d’ajouter la région. Le balisage général est conseillé dans certaines situations, par exemple il est préférable de marquer des ressources avec ja au lieu de ja-JP.

Il peut arriver parfois que certaines ressources ne doivent pas être localisées.

-   Les ressources telles que les chaînes d’interface utilisateur existantes dans toutes les langues doivent être marquées avec la langue dans laquelle elles sont. Veillez cependant à ce que toutes ces ressources soient disponibles dans la langue par défaut. Il n’est pas nécessaire de spécifier une ressource neutre (c’est-à-dire une ressource qui n’est pas marquée avec une langue).
-   Concernant les ressources qui sont fournies dans un sous-ensemble des langues pour l’application entière (localisation partielle), spécifiez les langues dans lesquelles ces ressources sont disponibles et veillez à ce que toutes ces ressources soient disponibles dans la langue par défaut. Windows sélectionne la meilleure langue possible pour l’utilisateur en fonction de toutes les langues parlées par celui-ci dans leur ordre de préférence. Par exemple, l’intégralité de l’interface utilisateur d’une application peut ne pas être localisée en catalan, si l’application dispose d’un ensemble complet de ressources en espagnol. Pour les utilisateurs qui parlent le catalan en premier puis l’espagnol, les ressources non disponibles en catalan apparaissent en espagnol.
-   Si parmi les ressources, certaines sont spécifiques à certaines langues, alors qu’une ressource est commune à toutes les langues, la ressource qui doit être utilisée pour toutes les langues doit être marquée avec la balise de langue indéterminée 'und'. Windows interprète la balise de langue 'und' de la même manière que '\*', c’est-à-dire qu’il choisit la langue principale de l’application après toute langue spécifique. Par exemple, si quelques ressources (telles que la largeur d’un élément) sont différentes pour le finnois, alors que les autres ressources sont les mêmes pour toutes les langues, la ressource en finnois doit être marquée avec la balise de langue pour le finnois et les autres ressources doivent être marquées avec 'und'.
-   Pour les ressources qui sont basées sur le script d’une langue au lieu de la langue elle-même, comme une police ou une hauteur de texte, utilisez la balise de langue indéterminée avec un script spécifique : 'und-&lt;script&gt;'. Par exemple, pour les polices latines utilisez und-Latn\\fonts.css et pour les polices cyrilliques utilisez und-Cryl\\fonts.css.

### <span id="Create_the_application_language_list."> </span> <span id="create_the_application_language_list."> </span> <span id="CREATE_THE_APPLICATION_LANGUAGE_LIST."> </span>Créez la liste des langues de l’application.

Lors de l’exécution, le système détermine les préférences de langue de l’utilisateur que l’application déclare prendre en charge dans son manifeste et crée une *liste des langues de l’application*. Il utilise cette liste pour déterminer en quelle(s) langue(s) doit être l’application. La liste détermine les langues qui sont utilisées pour les dates, les heures, les nombres et les ressources système de l’application, ainsi que pour d’autres composants. Par exemple, le système de gestion des ressources ([**Windows.ApplicationModel.Resources**](https://msdn.microsoft.com/library/windows/apps/br206022), [**Windows.ApplicationModel.Resources.Core**](https://msdn.microsoft.com/library/windows/apps/br225039) et [**WinJS.Resources Namespace**](https://msdn.microsoft.com/library/windows/apps/br229779)) charge les ressources d’interface utilisateur en fonction de la langue de l’application. [
            **Windows.Globalization**](https://msdn.microsoft.com/library/windows/apps/br206813) choisit également les formats en fonction de la liste des langues de l’application. La liste des langues de l’application est disponible en utilisant [**Windows.Globalization.ApplicationLanguages.Languages**](https://msdn.microsoft.com/library/windows/apps/hh972396).

La mise en correspondance des langues avec les ressources est difficile. Nous vous recommandons de laisser Windows s’en charger, car de nombreux composants facultatifs dans une balise de langue influencent dans la pratique la priorité de correspondance.

Voici des exemples de composants facultatifs dans une balise de langue :

-   Script pour supprimer des langues de script. Par exemple, en-Latn-US correspond à en-US.
-   Région. Par exemple, en-en-US correspond à en.
-   Variantes. Par exemple, de-DE-1996 correspond à de-DE.
-   -x et autres extensions. Par exemple, en-US-x-Pirate correspond à en-US.

Il y a également de nombreux composants dans une balise de langue qui ne sont pas au format xx ou xx-yy et tous ne correspondent pas.

-   zh-Hant ne correspond pas à zh-Hans.

Windows hiérarchise la correspondance des langues d’une façon logique. Par exemple, en-US correspond, par ordre de priorité, à en-US, en, en-GB, et ainsi de suite.

-   Windows assure la correspondance avec les langues régionales. Par exemple, en-US correspond à en-US, en, et en-\*.
-   Windows dispose de données supplémentaires pour établir des affinités dans une région, par exemple la région principale pour une langue. Par exemple, fr-FR convient mieux pour fr-BE que pour fr-CA.
-   Vous pourrez obtenir gratuitement d’autres améliorations de correspondance de langue dans Windows via les API Windows.

La mise en correspondance pour la première langue d’une liste se produit avant la mise en correspondance de la deuxième langue d’une liste, même pour d’autres variantes régionales. Par exemple, une ressource pour en-GB est choisie à la place d’une ressource fr-CA si la langue de l’application est en-US. En revanche s’il n’y a aucune ressource pour une forme de en, une ressource pour fr-CA est choisie.

La liste des langues de l’application est définie sur la variante régionale de l’utilisateur, même si celle-ci est différente de la variante régionale que l’application fournissait. Par exemple, si l’utilisateur parle en-GB alors que l’application prend en charge en-US, la liste des langues de l’application inclura en-GB. Cela garantit que les dates, les heures et les nombres seront formatés d’une manière plus proche des attentes de l’utilisateur (en-GB), mais les ressources d’interface utilisateur continuent à être chargées (en raison de la mise en correspondance des langues) dans la langue prise en charge par l’application (en-US).

La liste des langues de l’application comporte les éléments suivants :

1.  **(Facultatif) Langue principale de substitution.** Le paramètre [**PrimaryLanguageOverride**](https://msdn.microsoft.com/library/windows/apps/hh972398) est un paramètre de substitution simple pour les applications qui donnent aux utilisateurs leur propre choix de langue indépendant ou les applications qui ont une bonne raison de remplacer les langues par défaut. Pour en savoir plus, voir [Exemple de ressources d’application et de localisation](http://go.microsoft.com/fwlink/p/?linkid=231501).
2.  **Langues de l’utilisateur prises en charge par l’application.** Il s’agit de la liste des préférences linguistiques de l’utilisateur, classées par ordre de préférence. Elle est filtrée par la liste des langues prises en charge dans le manifeste de l’application. Le filtrage des langues de l’utilisateur par celles prises en charge par l’application préserve la cohérence entre les Kits de développement logiciel (SDK), les bibliothèques de classes, les packages d’infrastructure dépendants et l’application.
3.  **Si 1 et 2 sont vides, la langue par défaut ou la première langue prise en charge par l’application.** Si l’utilisateur ne parle aucune des langues prises en charge par l’application, la langue sélectionnée pour l’application est la première langue prise en charge par l’application.

Voir la section Remarques ci-dessous pour consulter des exemples.

### <span id="Set_the_HTTP_Accept_Language_header."> </span> <span id="set_the_http_accept_language_header."> </span> <span id="SET_THE_HTTP_ACCEPT_LANGUAGE_HEADER."> </span>Définissez l’en-tête HTTP Accept-Language.

Les requêtes HTTP envoyées à partir d’applications du Windows Store et d’applications de bureau dans des requêtes web standard et XMLHttpRequest (XHR) utilisent l’en-tête HTTP Accept-Language standard. Par défaut, l’en-tête HTTP est défini conformément aux préférences linguistiques de l’utilisateur, dans l’ordre de préférence de l’utilisateur, comme spécifié dans **Paramètres** &gt; **Heure et langue** &gt; **Région et langue**. Chaque langue de la liste est développée pour inclure les éléments neutres de la langue et un poids (q). Par exemple, la liste des langues d’un utilisateur comprenant fr-FR et en-US produit un en-tête Accept-Language HTTP comprenant fr-FR, fr, en-US, en (« fr-FR,fr;q=0.8,en-US;q=0.5,en;q=0.3 »).

### <span id="Use_the_APIs_in_the_Windows.Globalization_namespace."> </span> <span id="use_the_apis_in_the_windows.globalization_namespace."> </span> <span id="USE_THE_APIS_IN_THE_WINDOWS.GLOBALIZATION_NAMESPACE."> </span>Utilisez les API dans l’espace de noms Windows.Globalization.

En règle générale, les éléments d’API dans l’espace de noms [**Windows.Globalization**](https://msdn.microsoft.com/library/windows/apps/br206813) utilisent la liste des langues de l’application pour déterminer la langue. Si aucune de ces langues ne possède de format correspondant, les paramètres régionaux de l’utilisateur sont utilisés. Il s’agit des paramètres régionaux utilisés pour l’horloge système. Les paramètres régionaux utilisateur sont disponibles dans **Paramètres** &gt; **Heure et langue** &gt; **Région et langue** &gt; **Paramètres de date, d’heure et régionaux supplémentaires** &gt; **Région : Modifier les formats de date, d’heure ou de nombre**. Les API **Windows.Globalization** acceptent également une substitution pour spécifier une liste de langues, à utiliser à la place de la liste des langues de l’application.

[
            **Windows.Globalization**](https://msdn.microsoft.com/library/windows/apps/br206813) possède également un objet [**Language**](https://msdn.microsoft.com/library/windows/apps/br206804) qui est fourni en tant qu’objet Application d’assistance. Il permet aux applications d’inspecter des détails relatifs à la langue, tels que le script de la langue, le nom complet et le nom natif.

### <span id="Use_geographic_region_when_appropriate."> </span> <span id="use_geographic_region_when_appropriate."> </span> <span id="USE_GEOGRAPHIC_REGION_WHEN_APPROPRIATE."> </span>Utilisez la région lorsque cela est approprié.

À la place de la langue, vous pouvez utiliser le paramètre de région de résidence de l’utilisateur pour choisir le contenu à présenter à l’utilisateur. Par exemple, une application d’actualités pourra afficher par défaut un contenu provenant du lieu de résidence de l’utilisateur, qui est défini à l’installation de Windows et qui est disponible dans l’interface utilisateur Windows sous **Région : Modifier les formats de date, d’heure ou de nombre** comme indiqué dans la tâche précédente. Vous pouvez récupérer le paramètre actuel de la région de résidence de l’utilisateur à l’aide de [**Windows.System.UserProfile.GlobalizationPreferences.HomeGeographicRegion**](https://msdn.microsoft.com/library/windows/apps/br241829).

[
            **Windows.Globalization**](https://msdn.microsoft.com/library/windows/apps/br206813) possède également un objet [**GeographicRegion**](https://msdn.microsoft.com/library/windows/apps/br206795) qui est fourni en tant qu’objet Application d’assistance. Il permet aux applications d’inspecter les détails relatifs à une région particulière, tels que son nom complet, son nom natif et les devises utilisées.

## <span id="Remarks"> </span> <span id="remarks"> </span> <span id="REMARKS"> </span>Remarques


Le tableau ci-dessous contient des exemples de ce que l’utilisateur verrait dans l’interface utilisateur de l’application lorsque différents paramètres de langue et de région sont configurés.

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
<th align="left">Langues prises en charge par l’application (définies dans le manifeste)</th>
<th align="left">Langues de l’utilisateur (définies dans le Panneau de configuration)</th>
<th align="left">Langue principale de substitution de l’application (en option)</th>
<th align="left">Langues de l’application</th>
<th align="left">Ce que l’utilisateur voit dans l’application</th>
</tr>
</thead>
<tbody>
<tr>
<td align="left">Anglais (GB) (par défaut) ; allemand (Allemagne)</td>
<td align="left">Anglais (GB)</td>
<td align="left">aucune</td>
<td align="left">Anglais (GB)</td>
<td align="left">Interface utilisateur : anglais (GB)<br>Dates/Heures/Nombres : anglais (GB)</td>
</tr>
<tr>
<td align="left">Allemand (Allemagne) (par défaut) ; français (France) ; italien (Italie)</td>
<td align="left">Français (Autriche)</td>
<td align="left">aucune</td>
<td align="left">Français (Autriche)</td>
<td align="left">Interface utilisateur : français (France) (langue de base pour français (Autriche))<br>Dates/Heures/Nombres : français (Autriche)</td>
</tr>
<tr>
<td align="left">Anglais (US) (par défaut) ; français (France) ; anglais (GB)</td>
<td align="left">Anglais (Canada) ; français (Canada)</td>
<td align="left">aucune</td>
<td align="left">Anglais (Canada) ; français (Canada)</td>
<td align="left">Interface utilisateur : Anglais (US) (langue de base pour anglais (Canada))<br>Dates/Heures/Nombres : anglais (Canada)</td>
</tr>
<tr>
<td align="left">Espagnol (Espagne) (par défaut) ; espagnol (Mexique) ; espagnol (Amérique latine) ; portugais (Brésil)</td>
<td align="left">Anglais (É.U.)</td>
<td align="left">aucune</td>
<td align="left">Espagnol (Espagne)</td>
<td align="left">Interface utilisateur : espagnol (Espagne) (utilise le paramètre par défaut car aucune langue de base n’est disponible pour l’anglais)<br>Dates/Heures/Nombres : espagnol (Espagne)</td>
</tr>
<tr>
<td align="left">Catalan (par défaut) ; espagnol (Espagne) ; français (France)</td>
<td align="left">Catalan ; français (France)</td>
<td align="left">aucune</td>
<td align="left">Catalan ; français (France)</td>
<td align="left">Interface utilisateur : principalement en catalan et un peu en français (France) car toutes les chaînes ne sont pas en catalan<br>Dates/Heures/Nombres : catalan</td>
</tr>
<tr>
<td align="left">Anglais (GB) (par défaut) ; français (France) ; allemand (Allemagne)</td>
<td align="left">Allemand (Allemagne) ; anglais (GB)</td>
<td align="left">Anglais (GB) (choisi par l’utilisateur dans l’interface utilisateur de l’application)</td>
<td align="left">Anglais (GB) ; allemand (Allemagne)</td>
<td align="left">Interface utilisateur : anglais (GB) (langue de substitution)<br>Dates/Heures/Nombres : anglais (GB)</td>
</tr>
</tbody>
</table>

 

## <span id="related_topics"> </span>Rubriques connexes


* [Balise de langue BCP-47](http://go.microsoft.com/fwlink/p/?linkid=227302)
* [Liste de langues IANA](http://go.microsoft.com/fwlink/p/?linkid=227303)
* [Exemple de ressources d’application et de localisation](http://go.microsoft.com/fwlink/p/?linkid=231501)
* [Langues prises en charge](https://msdn.microsoft.com/library/windows/apps/jj657969)
 

 





<!--HONumber=Mar16_HO1-->


