---
author: DelfCo
Description: "Suivez ces meilleures pratiques en globalisant vos applications pour un public plus large, et en localisant vos applications pour un marché spécifique."
Search.Refinement.TopicID: 180
title: Indications de globalisation et de localisation
ms.assetid: 0342DC3F-DDD1-4DD4-872E-A4EC340CAE79
label: Do's and don'ts
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: a3924fef520d7ba70873d6838f8e194e5fc96c62
ms.openlocfilehash: 72849c304d2150fd7fe6768181a504f94ef98d5f

---

# <a name="globalization-and-localization-dos-and-donts"></a>Pratiques conseillées et déconseillées en matière de globalisation et de localisation
<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

Suivez ces meilleures pratiques en globalisant vos applications pour un public plus large, et en localisant vos applications pour un marché spécifique.

<div class="important-apis" >
<b>API importantes</b><br/>
<ul>
<li>[**Globalisation**](https://msdn.microsoft.com/library/windows/apps/br206813)</li>
<li>[**Globalization.NumberFormatting**](https://msdn.microsoft.com/library/windows/apps/br226136)</li>
<li>[**Globalization.DateTimeFormatting**](https://msdn.microsoft.com/library/windows/apps/br206859)</li>
<li>[**Ressources**](https://msdn.microsoft.com/library/windows/apps/br206022)</li>
<li>[**Resources.Core**](https://msdn.microsoft.com/library/windows/apps/br225039)</li>
</ul>
</div>



## <a name="globalization"></a>Globalisation

Préparez votre application pour l’adapter simplement à différents marchés. Pour ce faire, choisissez des termes et des images mondialement appropriés pour votre interface utilisateur, utilisez des API [**Globalization**](https://msdn.microsoft.com/library/windows/apps/br206813) pour mettre en forme les données de l’application et évitez les hypothèses fondées sur un lieu géographique ou une langue.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Recommandation</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>Utilisez les formats corrects pour les nombres, les dates, les heures, les adresses et les numéros de téléphone.</p></td>
<td align="left"><p>Le format utilisé pour les nombres, les dates, les heures et d’autres formes de données varie entre les cultures, les régions, les langues et les marchés. Si vous affichez des nombres, des dates, des heures ou d’autres données, utilisez les API [<strong>Globalization</strong>](https://msdn.microsoft.com/library/windows/apps/br206813) pour obtenir le format adapté à un public particulier.</p></td>
</tr>
<tr class="even">
<td align="left"><p>Prenez en charge les formats de papier internationaux.</p></td>
<td align="left"><p>Les formats de papier les plus courants diffèrent entre les pays. Par conséquent, si vous incluez des fonctionnalités qui dépendent du format de papier, telles que l’impression, veillez à prendre en charge et tester les formats internationaux courants.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>Prenez en charge les unités de mesure et les devises internationales.</p></td>
<td align="left"><p>Des unités et des échelles différentes sont utilisées dans différents pays, bien que les systèmes métrique et impérial soient les plus populaires. Si vous utilisez des mesures, telles que la longueur, la température ou la surface, obtenez la mesure dans le système correct en utilisant la propriété [<strong>CurrenciesInUse</strong>](https://msdn.microsoft.com/library/windows/apps/br206793).</p></td>
</tr>
<tr class="even">
<td align="left"><p>Affichez correctement le texte et les polices.</p></td>
<td align="left"><p>Les paramètres optimaux de police, taille de police et direction du texte varient entre les différents marchés.</p>
<p>Pour plus d’informations, voir [<strong>Ajuster la disposition et les polices, et prendre en charge le sens du flux DàG</strong>](adjust-layout-and-fonts--and-support-rtl.md).</p></td>
</tr>
<tr class="odd">
<td align="left"><p>Utilisez Unicode pour le codage des caractères.</p></td>
<td align="left"><p>Par défaut, les versions récentes de Microsoft Visual Studio utilisent le codage de caractères Unicode pour tous les documents. Si vous utilisez un autre éditeur, veillez à enregistrer les fichiers sources dans les codages de caractères Unicode appropriés. Toutes les API Windows Runtime retournent des chaînes codées selon le codage de caractères UTF-16.</p></td>
</tr>
<tr class="even">
<td align="left"><p>Enregistrez la langue de saisie.</p></td>
<td align="left"><p>Lorsque votre application demande aux utilisateurs d’entrer du texte, enregistrez la langue de saisie. Cela garantit que, lorsque l’entrée est affichée ultérieurement, elle sera présentée à l’utilisateur dans le format approprié. Utilisez la propriété [<strong>CurrentInputMethodLanguage</strong>](https://msdn.microsoft.com/library/windows/apps/hh700658) pour obtenir la langue d’entrée actuelle.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>N’utilisez pas la langue pour supposer l’emplacement géographique d’un utilisateur et n’utilisez pas l’emplacement géographique pour supposer la langue d’un utilisateur.</p></td>
<td align="left"><p>Dans Windows, la langue et l’emplacement géographique d’un utilisateur sont des concepts distincts. Un utilisateur peut parler une variante régionale d’une langue, telle qu’en-gb pour l’anglais parlé en Grande-Bretagne, tout en étant situé dans un autre pays ou région. Déterminez si vos applications requièrent une connaissance de la langue de l’utilisateur, par exemple pour le texte de l’interface utilisateur, ou de son emplacement géographique, par exemple pour des problèmes de licence.</p>
<p>Pour plus d’informations, voir [<strong>Gérer la langue et la région</strong>](manage-language-and-region.md).</p></td>
</tr>
<tr class="even">
<td align="left"><p>N’utilisez pas d’expressions familières ni de métaphores.</p></td>
<td align="left"><p>Le langage propre à une tranche de population, telle qu’une culture ou une classe d’âge, peut être difficile à comprendre, car seuls les représentants de cette tranche de population emploient ce langage. De la même façon, les métaphores peuvent avoir un sens pour une personne, mais ne rien évoquer pour une autre. Par exemple, &quot;vendanger&quot; a un sens précis dans le jargon du football, qui échappe aux non-initiés. Si vous envisagez de localiser votre application et que vous adoptez un ton informel, veillez à expliquer clairement aux traducteurs le sens et le style à reproduire.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>N’utilisez pas de jargon, d’abréviations ni d’acronymes.</p></td>
<td align="left"><p>Un langage technique aura moins de chances d’être compris d’un public non initié ou de personnes issues d’autres cultures ou régions et il sera difficile à traduire. Ce vocabulaire n’est pas utilisé dans les conversations de tous les jours. Un langage technique apparaît souvent dans les messages d’erreur pour identifier les problèmes matériels et logiciels. Parfois, cela s’avère nécessaire, mais vous devriez reformuler les chaînes pour qu’elles ne soient pas techniques.</p></td>
</tr>
<tr class="even">
<td align="left"><p>N’utilisez pas d’images susceptibles d’être offensantes.</p></td>
<td align="left"><p>Des images appropriées dans votre propre culture peuvent être offensantes ou mal interprétées dans d’autres cultures. Évitez l’utilisation de symboles religieux, d’animaux ou d’associations de couleurs correspondant à des drapeaux nationaux ou des mouvements politiques.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>Évitez toute infraction politique dans les cartes ou en faisant référence à des régions.</p></td>
<td align="left"><p>Les cartes peuvent inclure des frontières nationales ou régionales controversées et représentent une source fréquente d’infraction politique. Veillez à ce que toute interface utilisateur utilisée pour la sélection d’une nation lui fasse référence en tant que &quot;pays/région&quot;. Placer un territoire contesté dans une liste intitulée &quot;Pays&quot;, comme dans un formulaire de déclaration d’adresse, pourrait vous attirer des ennuis.</p></td>
</tr>
<tr class="even">
<td align="left"><p>N’utilisez pas la seule comparaison de chaînes pour comparer des balises de langue.</p></td>
<td align="left"><p>Les balises de langue BCP-47 sont complexes. Il existe un certain nombre de problèmes lors de la comparaison de balises de langue, dont notamment des problèmes de correspondance entre les informations de script, les balises héritées et les différentes variantes régionales. Le système de gestion des ressources dans Windows s’occupe des correspondances pour vous. Vous pouvez spécifier un ensemble de ressources dans des langues quelconques et le système choisit celle qui est appropriée pour l’utilisateur et l’application.</p>
<p>Pour plus d’informations sur la gestion des ressources, voir [<strong>Définition des ressources d’application</strong>](https://msdn.microsoft.com/library/windows/apps/xaml/hh965321).</p></td>
</tr>
<tr class="odd">
<td align="left"><p>Ne supposez pas que le classement est toujours alphabétique.</p></td>
<td align="left"><p>Pour les langues qui n’utilisent pas le script latin, le classement se base sur des choses telles que la prononciation, le nombre de coups de crayon et d’autres facteurs. Mêmes les langues qui utilisent le script latin n’utilisent pas toujours le classement alphabétique. Par exemple, dans certaines cultures, un annuaire n’est pas trié par ordre alphabétique. Le système peut gérer le classement pour vous, mais si vous créez votre propre algorithme de tri, veillez à prendre en compte les méthodes de classement utilisées dans vos marchés cibles.</p></td>
</tr>
</tbody>
</table>

 

## <a name="localization"></a>Localisation

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Recommandation</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>Séparez les ressources telles que les chaînes d’interface utilisateur et les images du code.</p></td>
<td align="left"><p>Concevez vos applications de telle sorte que les ressources, telles que les chaînes et les images, soient séparées de votre code. Cela permet de les gérer, localiser et personnaliser de manière indépendante pour différents facteurs d’échelle, diverses options d’accessibilité et une multitude d’autres contextes d’ordinateur et d’utilisateur.</p>
<p>Séparez les ressources de type chaîne du code de votre application pour créer un code base unique, indépendant de la langue. Séparez toujours les chaînes du code d’application et du balisage. Placez-les dans un fichier de ressources, tel qu’un fichier ResW ou ResJSON.</p>
<p>Utilisez l’infrastructure des ressources de Windows pour gérer la sélection des ressources les plus appropriées de façon à correspondre à l’environnement de l’utilisateur.</p></td>
</tr>
<tr class="even">
<td align="left"><p>Isolez les autres fichiers de ressources localisables.</p></td>
<td align="left"><p>Prenez les autres fichiers qui requièrent une localisation, tels que les images qui contiennent du texte à traduire ou qui doivent être changées en raison de sensibilités culturelles, et placez-les dans des dossiers mentionnant les noms des langues.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>Définissez votre langue par défaut et marquez toutes vos ressources, même celles dans votre langue par défaut.</p></td>
<td align="left"><p>Définissez toujours la langue par défaut de vos applications dans le manifeste de l’application (package.appxmanifest). La langue par défaut détermine la langue utilisée lorsque l’utilisateur ne parle aucune des langues prises en charge dans l’application. Marquez les ressources linguistiques par défaut, par exemple en-us/Logo.png, avec leur langue, de sorte que le système puisse savoir en quelle langue se trouve la ressource et comment elle est utilisée dans des situations particulières.</p></td>
</tr>
<tr class="even">
<td align="left"><p>Déterminez les ressources de votre application qui requièrent une localisation.</p></td>
<td align="left"><p>Qu’est-ce qui doit changer si votre application doit être localisée pour d’autres marchés ? Les chaînes de texte doivent être traduites dans d’autres langues. Les images ont peut-être besoin d’être adaptées pour d’autres cultures. Déterminez la manière dont la localisation affecte d’autres ressources que votre application utilise, telles que les contenus audio et vidéo.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>Utilisez des identificateurs de ressource dans le code et le balisage pour faire référence aux ressources.</p></td>
<td align="left"><p>Au lieu d’utiliser des opérateurs de chaîne ou des noms de fichiers spécifiques pour les images dans votre balisage, utilisez les références aux ressources. Veillez à utiliser des identificateurs uniques pour chaque ressource. Pour plus d’informations, voir [<strong>Comment nommer des ressources à l’aide de qualificateurs</strong>](https://msdn.microsoft.com/library/windows/apps/xaml/Hh965324).</p>
<p>Écoutez les événements déclenchés en cas de changements dans le système et si ce dernier commence à utiliser un ensemble distinct de qualificateurs. Retraitez le document afin de charger les ressources correctes.</p></td>
</tr>
<tr class="even">
<td align="left"><p>Autorisez l’augmentation de la taille du texte.</p></td>
<td align="left"><p>Allouez dynamiquement les tampons de texte, car la taille du texte peut augmenter une fois traduit. Si vous devez utiliser des tampons statiques, définissez-les de très grande taille (en doublant peut-être la longueur de la chaîne anglaise) pour palier l’extension éventuelle des chaînes après traduction. L’espace disponible est peut-être limité pour une interface utilisateur. Pour s’adapter aux langues localisées, faites en sorte que la longueur de vos chaînes soit approximativement 40 % supérieure à celle que l’anglais nécessite. Pour les chaînes de texte vraiment courtes, telles que des mots uniques, vous pouvez avoir besoin de 300 % d’espace supplémentaire. En outre, si vous permettez à un contrôle de prendre en charge plusieurs lignes et le retour automatique à la ligne, cela laisse plus d’espace pour afficher chaque chaîne.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>Prenez en charge la mise en miroir.</p></td>
<td align="left"><p>L’alignement de texte et le sens de lecture peuvent être de gauche à droite, comme en français, ou de droite à gauche (RTL, right-to-left), comme en arabe ou en hébreu. Si vous localisez votre produit dans des langues qui utilisent un sens de lecture différent du vôtre, veillez à ce que la disposition de vos éléments d’interface utilisateur prenne en charge la mise en miroir. Même des éléments tels que les boutons Précédent, les effets de transition de l’interface utilisateur et les images peuvent avoir à être mis en miroir.</p>
<p>Pour plus d’informations, voir [<strong>Ajuster la disposition et les polices, et prendre en charge le sens du flux DàG</strong>](adjust-layout-and-fonts--and-support-rtl.md).</p></td>
</tr>
<tr class="even">
<td align="left"><p>Commentez les chaînes.</p></td>
<td align="left"><p>Veillez à ce que les chaînes soient commentées de façon appropriée et que seules les chaînes qui doivent être traduites soient fournies aux traducteurs. Une sur-localisation est une source courante de problèmes.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>Utilisez des chaînes courtes.</p></td>
<td align="left"><p>Les chaînes courtes sont plus faciles à traduire et permettent le recyclage de la traduction. Le recyclage de la traduction permet d’économiser de l’argent en évitant l’envoi d’une chaîne déjà traduite au traducteur.</p>
<p>Les chaînes de plus de 8 192 caractères peuvent ne pas être prises en charge par certains outils de localisation, alors conservez des chaînes de 4 000 caractères au plus.</p></td>
</tr>
<tr class="even">
<td align="left"><p>Rédigez des chaînes contenant une phrase complète.</p></td>
<td align="left"><p>Rédigez des chaînes contenant une phrase complète, au lieu de scinder la phrase en mots individuels, car la traduction des mots peut dépendre de leur position dans une phrase. De plus, ne supposez pas qu’une expression dotée de plusieurs paramètres conservera ces paramètres dans le même ordre pour chaque langue.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>Optimisez les fichiers image et audio pour la localisation.</p></td>
<td align="left"><p>Réduisez les coûts de localisation en évitant l’utilisation de texte dans les images ou de paroles dans les fichiers audio. Si vous localisez du texte vers une langue dotée d’un sens de lecture différent du vôtre, l’utilisation d’images et d’effets symétriques facilite la prise en charge de la mise en miroir.</p></td>
</tr>
<tr class="even">
<td align="left"><p>Ne réutilisez pas des chaînes dans des contextes différents.</p></td>
<td align="left"><p>Ne réutilisez pas des chaînes dans des contextes différents, car même des mots simples tels que &quot;on&quot; et &quot;off&quot; peuvent être traduits différemment selon le contexte.</p></td>
</tr>
</tbody>
</table>

 

## <a name="related-articles"></a>Articles connexes


**Exemples**
* [Exemple de ressources d’application et de localisation](http://go.microsoft.com/fwlink/p/?linkid=254478)
* [Exemple de préférences de globalisation](http://go.microsoft.com/fwlink/p/?linkid=231608)
 

 






<!--HONumber=Dec16_HO2-->


