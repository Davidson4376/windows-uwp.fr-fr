---
Description: The previous topic (How the Resource Management System matches and chooses resources) looks at qualifier-matching in general. This topic focuses on language-tag-matching in more detail.
title: Comment le système de gestion des ressources met en correspondance les balises de langue
template: detail.hbs
ms.date: 11/02/2017
ms.topic: article
keywords: windows10, uwp, ressources, image, MRT, qualificateur
ms.localizationpriority: medium
ms.openlocfilehash: 4914a448432206e2418fe110c0b49517a7145e0b
ms.sourcegitcommit: a3dc929858415b933943bba5aa7487ffa721899f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/07/2018
ms.locfileid: "8783283"
---
# <a name="how-the-resource-management-system-matches-language-tags"></a>Comment le système de gestion des ressources met en correspondance les balises de langue

La rubrique précédente ([Comment le système de gestion des ressources met en correspondance et sélectionne les ressources](how-rms-matches-and-chooses-resources.md)) aborde la correspondance entre les qualificateurs de manière générale. Cette rubrique se concentre plus particulièrement sur la correspondance entre les balises de langue.

## <a name="introduction"></a>Introduction

Les ressources dotées de qualificateurs de balise de langue sont comparées et notées en fonction de la liste de langues d’exécution de l’application. Pour obtenir des définitions des listes de langues différentes, consultez [Comprendre les langues de profil utilisateur et les langues du manifeste de l’application](../design/globalizing/manage-language-and-region.md). La mise en correspondance pour la première langue d’une liste se produit avant la mise en correspondance de la deuxième langue d’une liste, même pour d’autres variantes régionales. Par exemple, une ressource pour en-GB est choisie à la place d’une ressource fr-CA si la langue d’exécution de l’application est en-US. Une ressource fr-CA est choisie uniquement s’il n’y aucune ressource pour une forme d’en (notez que la langue par défaut de l’application n’a pas pu être définie sur une forme d’en dans ce cas).

Le mécanisme de notation utilise les données incluses dans le registre de sous-balises [BCP-47](http://go.microsoft.com/fwlink/p/?linkid=227302) et d’autres sources de données. Il utilise une échelle de notation avec différentes qualités de correspondance et, lorsque plusieurs candidats sont disponibles, il sélectionne le candidat avec le meilleur score.

Ainsi, vous pouvez utiliser des balises avec des termes génériques pour un contenu linguistique, avec néanmoins la possibilité de spécifier un contenu spécifique si nécessaire. Par exemple, votre application peut contenir plusieurs chaînes anglaises qui sont communes aux États-Unis, à la Grande-Bretagne et à d’autres régions. Le marquage de ces chaînes avec la balise «en» (Anglais) réduit les besoins en espace et en localisation. Lorsqu’une distinction s’avère nécessaire, par exemple dans une chaîne contenant le mot «color/colour» en anglais, les versions américaine et britannique peuvent être marquées séparément à l’aide des sous-balises de langue et de région, «en-US» et «en-GB «, respectivement.

## <a name="language-tags"></a>Balises de langue

Les langues sont identifiées à l’aide de balises de langue BCP-47 normalisées et correctement formées. Les composants des sous-balises sont définis dans le registre de sous-balises BCP-47. La structure standard d’une balise de langue BCP-47 comporte un ou plusieurs des éléments de sous-balise suivants.

- Sous-balise de langue (obligatoire).
- Sous-balise de script (qui peut être déduite de la valeur par défaut spécifiée dans le registre de sous-balises).
- Sous-balise de région (facultative).
- Sous-balise de variante (facultative).

D’autres éléments de sous-balise peuvent être présents, mais ils auront très peu d’impact lors de la mise en correspondance des langues. Aucune plage de langues n’est définie à l’aide du caractère générique («*»), par exemple «en-*».

## <a name="matching-two-languages"></a>Mise en correspondance de deux langues

Chaque fois que Windows compare deux langues, cette opération est généralement effectuée dans le contexte d’un processus plus large. Il peut s’agir de l’évaluation de plusieurs langues, par exemple, lorsque Windows génère la liste des langues de l’application (voir [Comprendre les langues de profil utilisateur et les langues du manifeste de l’application](../design/globalizing/manage-language-and-region.md)). Pour ce faire, Windows met en correspondance plusieurs langues figurant dans les préférences utilisateur et les langues spécifiées dans le manifeste de l’application. La comparaison peut également être effectuée dans le contexte de l’évaluation de la langue avec d’autres qualificateurs pour une ressource particulière. Par exemple, lorsque Windows résout une ressource de fichier spécifique particulier en contexte de ressource spécifique, avec le lieu de résidence de l’utilisateur ou l’échelle ou la résolution ppp actuelles de l’appareil comme autres facteurs (en plus de la langue) pris en compte dans la sélection de la ressource.

Lorsque deux balises de langue sont comparées, un score est attribué à la comparaison en fonction de la précision de la correspondance.

| Correspondance | Score | Exemple |
| ----- | ----- | ------- |
| Correspondance exacte | Le plus élevé | en-AU: en-AU |
| Correspondance de variante (langue, script, région, variante) |  | en-AU-variante1: en-AU-variante1-t-ja |
| Correspondance de région (langue, script, région) |  | en-AU: en-AU-variante1 |
| Correspondance partielle (langue, script) |  |  |
| -Correspondance de macrorégion |  | en-AU: en-053 |
| - Correspondance indépendante de la région |  | en-AU: en |
| -Correspondance d’affinité orthographique (prise en charge limitée) |  | en-AU: en-GB |
| - Correspondance de région par défaut |  | en-AU: en-US |
| - N’importe quelle correspondance de région |  | en-AU: en-CA |
| Langue indéterminée (toute correspondance de langue) |  | en-AU: und |
| Aucune correspondance (non-concordance de script ou non-concordance de balise de langue principale) | Le plus bas | en-AU: fr-FR |

### <a name="exact-match"></a>Correspondance exacte

Les balises sont identiques (tous les éléments de sous-balises correspondent). Une comparaison peut être promue à ce type de correspondance à partir d’une correspondance de variante ou de région. Par exemple, en-US correspond à en-US.

### <a name="variant-match"></a>Correspondance de variante

Les balises correspondent pour les sous-balises de langue, script, région et variante, mais elles diffèrent à un autre titre.

### <a name="region-match"></a>Correspondance de région

Les balises correspondent pour les sous-balises de langue, script, et variante, mais elles diffèrent à un autre titre. Par exemple, de-DE-1996correspond à de-DE et en-US-x-Pirate correspond à en-US.

### <a name="partial-matches"></a>Correspondances partielles

Les balises correspondent pour les sous-balises de langue et de script, mais elles diffèrent pour la région ou une autre sous-balise. Par exemple, en-US correspond à en ou en-US, correspond à en-\*.

#### <a name="macro-region-match"></a>Correspondance de macrorégion

Les balises correspondent pour les sous-balises de langue et de script, les deux balises ont des sous-balises de région et l’une de ces sous-balises correspond à une macrorégion qui englobe l’autre région. Les sous-balises de macrorégion sont toujours numériques et sont dérivées des codes de pays et de région de la Classification M49 de la Division de statistique de l’ONU. Pour plus d’informations sur les relations englobantes, voir [Composition des régions macro-géographiques (continentales), des sous-régions géographiques et des groupements économiques ou autres sélectionnés](http://go.microsoft.com/fwlink/p/?LinkId=247929).

**Remarque: ** les codes de l’ONU pour les «groupements économiques» ou les «autres groupements» ne sont pas pris en charge dans BCP-47.
 
**Remarque: ** une balise dotée de la sous-balise de macrorégion «001» est considérée comme équivalente à une balise indépendante de la région. Par exemple, il est considéré que «es-001» et «es» sont synonymes.

#### <a name="region-neutral-match"></a>Correspondance indépendante de la région

Les balises correspondent pour les sous-balises de langue et de script et une seule balise comporte une balise de région. Une correspondance parente est préférée à d’autres correspondances partielles.

#### <a name="orthographic-affinity-match"></a>Correspondance d’affinité orthographique

Les balises correspondent pour les sous-balises de langue et de script et les sous-balises de région ont une affinité orthographique. L’affinité repose sur les données (gérées dans Windows) qui définissent les régions ayant des affinités propres à la langue, par exemple, «en-IE» et «en-GB».

#### <a name="preferred-region-match"></a>Correspondance de région par défaut

Les balises correspondent pour les sous-balises de langue et de script et une des sous-balises de région est la sous-balise de région par défaut pour la langue. Par exemple, «fr-FR» est la région par défaut pour la sous-balise «fr». Ainsi, fr-FR convient mieux pour fr-BE que pour fr-CA. Cela repose sur les données (gérées dans Windows) qui définissent une région par défaut pour chaque langue dans laquelle Windows est localisé.

#### <a name="sibling-match"></a>Correspondance sœur

Les balises correspondent pour les sous-balises de langue et de script et elles ont toutes deux des sous-balises de région, mais aucune relation n’est définie entre elles. S’il existe plusieurs correspondances sœurs, la dernière sœur énumérée l’emportera, en l’absence d’une meilleure correspondance.

### <a name="undetermined-language"></a>Langue indéterminée

Une ressource peut avoir la balise «und» pour indiquer qu’elle ne correspond à aucune langue. Cette balise peut également être utilisée avec une balise de script pour filtrer les correspondances basées sur le script. Par exemple, «und-Latn» correspondra à n’importe quelle balise de langue qui utilise un script latin. Pour plus d’informations, voir ci-dessous.

### <a name="script-mismatch"></a>Non-concordance de script

Lorsque les balises correspondent pour la balise de langue principale, mais pas le script, la paire est considérée comme n’étant pas une correspondance et un score inférieur à celui d’une correspondance valide lui est attribué.

### <a name="no-match"></a>Aucune correspondance

Les sous-balises de langue principale qui ne correspondent pas reçoivent un score plus bas que celui d’une correspondance valide. Par exemple, zh-Hant ne correspond pas à zh-Hans.

## <a name="examples"></a>Exemples

La langue utilisateur "zh-Hans-CN" (Chinois simplifié (Chine)) correspond aux ressources suivantes dans l’ordre de priorité indiqué. Un X n’indique qu’il n’a pas de correspondance.

![Correspondance pour Chinois simplifié (Chine)](images/language_matching_1.png)

1. Exact match; 2. & 3. Region match; 4. Parent match; 5. Sibling match.

Lorsqu’une valeur de script de suppression est définie pour une sous-balise de langue dans le registre de sous-balises BCP-47, une correspondance existe et elle prend la valeur du code de script supprimé. Par exemple, en-Latn-US correspond à en-US. Dans l’exemple suivant, la langue de l’utilisateur est «en-AU» (Anglais (Australie)).

![Correspondance pour Anglais (Australie)](images/language_matching_2.png)

1. Exact match; 2. Macro region match; 3. Region-neutral match; 4. Orthographic affinity match; 5. Preferred region match; 6. Sibling match.

## <a name="matching-a-language-to-a-language-list"></a>Mise en correspondance d’une langue et d’une liste de langues

Dans certains cas, la mise en correspondance est effectuée dans le cadre d’un processus plus large de mise en correspondance d’une langue et d’une liste de langues. Par exemple, il peut exister une correspondance entre une seule ressource de langue et la liste de langues d’une application. Le score de la correspondance est pondéré en fonction de la position de la première langue correspondante dans la liste. Plus la position de la langue est basse dans la liste, plus le score est faible.

Lorsque la liste de langues contient deux ou plusieurs variantes régionales ayant les mêmes sous-balises de langue et de script, les comparaisons de la première balise de langue sont notées uniquement pour des correspondances exactes, de variante et de région. La notation des correspondances partielles est reportée à la dernière variante régionale. Cela permet aux utilisateurs de contrôler avec précision le comportement des correspondances pour leur liste de langues. Ce comportement peut autoriser la préférence d’une correspondance exacte pour un élément secondaire de la liste par rapport à une correspondance partielle pour le premier élément de la liste, s’il existe une troisième élément qui correspond à la langue et au script du premier. Voici un exemple.

- Liste de langues (dans l’ordre): «pt-PT» (Portugais (Portugal)), «en-US» (Anglais (États-Unis)), «pt-BR» (Portugais (Brésil)).
- Ressources: «en-US», «pt-BR».
- Ressource avec le score le plus élevé: «en-US».
- Description: la comparaison commence par «pt-PT», mais ne trouve pas de correspondance exacte. En raison de la présence de «pt-BR» dans la liste de langues de l’utilisateur, la correspondance partielle est reportée jusqu’à la comparaison avec «pt-BR». La comparaison de langue suivante est «en-US», qui a une correspondance exacte. Par conséquent, la ressource qui l’emporte est «en-US».

OU

- Liste de langues (dans l’ordre): «es-MX» (Espagnol (Mexique)), «es-HO» (Espagnol (Honduras)).
- Ressources: «en-ES», «es-HO».
- Ressource avec le score le plus élevé: «es-HO».

## <a name="undetermined-language-und"></a>Langue indéterminée («und»)

La balise de langue «und» peut être utilisée pour spécifier une ressource qui correspond à n’importe quelle langue en l’absence d’une meilleure correspondance. Elle peut être considérée comme similaire à la plage de langues BCP-47 «*» ou «*-&lt;script&gt;». Voici un exemple.

- Liste de langues: «en-US», «zh-Hans-CN».
- Ressources: «zh-Hans-CN», «und».
- Ressource avec le score le plus élevé: «und».
- Description: la comparaison commence par «en-US», mais ne trouve pas de correspondance basée sur «en» (partielle ou meilleure). Dans la mesure où il existe une ressource marquée «und», l’algorithme de correspondance l’utilise.

La balise «und» permet à plusieurs langues de partager une seule ressource et à des langues individuelles d’être considérées comme des exceptions. Voici un exemple.

- Liste de langues: «zh-Hans-CN», «en-US».
- Ressources: «zh-Hans-CN», «und».
- Ressource avec le score le plus élevé: «zh-Hans-CN».
- Description: la comparaison trouve une correspondance exacte pour le premier élément et par conséquent, elle ne recherche pas la ressource marquée «und».

Vous pouvez utiliser «und» avec une balise de script pour filtrer les ressources par script. Voici un exemple.

- Liste de langues: «ru».
- Ressources: «und-Latn», «und-Cyrl», «und-Arab».
- Ressource avec le score le plus élevé: «und-Cyrl».
- Description: la comparaison ne trouve pas de correspondance pour «ru» (partielle ou meilleure) et par conséquent, sélectionne la correspondance avec la balise de langue «und». La valeur de script de suppression «Cyrl» associée à la balise de langue «ru» correspond à la ressource «und-Cyrl».

## <a name="orthographic-regional-affinity"></a>Affinité régionale orthographique

Lorsque deux balises de langue dotées de sous-balises de région différentes sont mises en correspondance, des paires de régions spécifiques peuvent avoir une affinité plus élevée entre elles plutôt qu’avec d’autres. Les seuls groupes avec affinité pris en charge sont pour la langue Anglais («en»). Les sous-balises de région «PH» (Philippines) et «LR» (Libéria) ont une affinité orthographique avec la sous-balise de région «US». Toutes les autres sous-balises de région ont une affinité avec la sous-balise de région «GB» (Royaume-Uni). Par conséquent, lorsque les ressources «en-US» et «en-GB» sont disponibles, une liste de langues comportant «en-HK» (Anglais (Hong Kong R.A.S.)) obtiendra un score plus élevé avec les ressources «en-GB» qu’avec les ressources «en-US».

## <a name="handling-languages-with-many-regional-variants"></a>Gestion de langues avec de nombreuses variantes régionales

Quelques langues ont des communautés plus importantes dans certaines régions qui utilisent des variantes de cette langue, par exemple l’anglais, le français et l’espagnol, qui comptent parmi les langues le plus souvent prises en charge dans les applications multilingues. Les différences régionales peuvent inclure des différences orthographiques (par exemple, «color» et «colour» en anglais), ou des différences dialectales comme le vocabulaire (par exemple, «truck» et «lorry» en anglais).

Ces langues avec des variantes régionales importantes représentent un défi lors de la conception d’une application destinée au marché international: «combien de variantes régionales faut-il prendre en charge?» «Lesquelles?» «Quelle est la solution la plus rentable pour gérer ces ressources de variantes régionales pour mon application?» Les réponses à ces questions sortent du cadre de cette rubrique. Toutefois, les mécanismes de correspondance de langues de Windows offrent des fonctionnalités susceptibles de faciliter la gestion des variantes régionales.

Les applications prennent souvent en charge une seule variante d’une langue donnée. Prenons l’exemple d’une application qui inclut des ressources pour une seule variante d’anglais qui doivent être utilisées par des anglophones, quelle que soit leur région. Dans ce cas, la balise «en», sans aucune sous-balise de région reflète l’utilisation anticipée. Mais, auparavant, les applications ont peut-être utilisé une balise comme «en-US» qui inclut une sous-balise de région. Dans ce cas, le scénario suivant fonctionne également: l’application utilise une seule variante de langue anglaise et Windows gère la correspondance d’une ressource marquée pour une variante régionale avec une préférence de langue utilisateur pour une autre variante régionale de façon appropriée.

Toutefois, si deux ou plusieurs variantes régionales doivent être prises en charge, une différence telle que «en» et «en-US» peut avoir un impact significatif sur l’expérience utilisateur. Il est donc important de choisir avec soin les sous-balises de région à utiliser.

Supposons que vous voulez fournir des localisations en français différentes pour le Canada et l’Europe. Pour le français canadien, vous pouvez utiliser «fr-CA». Pour les utilisateurs francophones d’Europe, la localisation utilisera Français (France) et vous pouvez donc utiliser «fr-FR». Mais quelle version obtiendra un utilisateur résidant en Belgique et dont la préférence de langue est «fr-BE»? La région «BE» est différente de «FR» et de «CA», ce qui suggère une correspondance avec «n’importe quelle région» pour les deux. Toutefois, la France étant la région par défaut pour le français, la variante «fr-FR» sera considérée comme étant la meilleure correspondance dans ce cas.

Supposons que vous avez d’abord localisé votre application pour une seule variante de français, en utilisant des chaînes en Français (France), mais en les qualifiant de manière générique en «fr», puis que vous souhaitiez ajouter la prise en charge du Français (Canada). Il vous suffira probablement de retraduire certaines ressources en français canadien. Vous pouvez continuer à utiliser toutes les ressources d’origine en conservant le qualificateur «fr», en ajoutant quelques nouvelles ressources avec le qualificateur «fr-CA». Si la préférence de langue de l’utilisateur est «fr-CA», les ressources «fr-CA» auront un score plus élevé que les ressources «fr». En revanche, si la préférence de langue de l’utilisateur est une autre variante de français, les ressources indépendantes de la région «fr» seront une meilleure correspondance que les ressources «fr-CA».

Autre exemple, supposons que vous voulez fournir des localisations espagnoles distinctes pour les utilisateurs hispanophones d’Espagne et d’Amérique latine. Supposons également que les traductions destinées à l’Amérique latine ont été effectuées par un fournisseur résidant au Mexique. Faut-il utiliser «es-ES» (Espagne) et «es-MX» (Mexique) pour les deux ensembles de ressources? Cela risque de poser problème aux hispanophones d’autres régions d’Amérique latine comme l’Argentine ou la Colombie, car ils auraient des ressources «es-ES». Dans ce cas, il existe une meilleure solution: vous pouvez utiliser la sous-balise de macrorégion «es-419» pour indiquer que les ressources sont destinées aux hispanophones se trouvant dans n’importe quelle région d’Amérique latine ou des Caraïbes.

Les balises de langue indépendantes de la région et les sous-balises de macrorégion peuvent s’avérer très efficaces lorsque vous souhaitez prendre en charge plusieurs variantes régionales. Pour réduire le nombre de ressources distinctes nécessaires, vous pouvez qualifier une ressource spécifique de façon à refléter la plus large couverture voulue pour celle-ci. Ensuite, affectez à une ressource applicable à grande échelle une variante plus spécifique, comme requis. Une ressource avec un qualificateur de langue indépendant de la région sera utilisée pour les utilisateurs de toutes les variantes régionales, sauf s’il existe une autre ressource avec un qualificateur plus spécifique à une région qui s’applique à cet utilisateur. Par exemple, une ressource «en» correspond à un utilisateur dont la langue est l’anglais australien, mais une ressource avec «en-053» (Anglais tel qu’utilisé en Australie ou en Nouvelle-Zélande) sera une meilleure correspondance pour cet utilisateur, tandis qu’une ressource dotée de la balise «en-AU» sera la meilleure correspondance possible.

L’anglais requiert une attention particulière. Si une application ajoute la localisation pour deux variantes d’anglais, il s’agira probablement de l’anglais des États-Unis et du Royaume-Uni, ou l’anglais «international». Comme indiqué plus haut, certaines régions situées hors des États-Unis utilisent les conventions orthographiques de ce pays et ceci est pris en considération dans les correspondances de langues effectuées par Windows. Dans ce scénario, il n’est pas recommandé d’utiliser la balise indépendante de la région «en» pour une des variantes; il est préférable d’utiliser «en-GB» et «en-US» à la place. (Si une ressource donnée ne requiert pas de variantes séparées, la balise «en» peut toutefois être utilisée.) Si la balise «en-GB» ou «en-US» est remplacée par «en», cela interférera avec l’affinité régionale orthographique fournie par Windows. Si une troisième localisation en anglais est ajoutée, utilisez une sous-balise spécifique ou de macrorégion pour les variantes supplémentaires, comme nécessaire (par exemple, «en-CA», «en-AU» ou «en-053»), mais continuez à utiliser en-GB» et «en-US».

## <a name="related-topics"></a>Rubriques associées

* [Comment le système de gestion des ressources met en correspondance et sélectionne les ressources](how-rms-matches-and-chooses-resources.md)
* [BCP-47](http://go.microsoft.com/fwlink/p/?linkid=227302)
* [Comprendre les langues de profil utilisateur et les langues du manifeste de l’application](../design/globalizing/manage-language-and-region.md)
* [Composition des régions macro-géographiques (continentales), des sous-régions géographiques et des groupements économiques ou autres sélectionnés](http://go.microsoft.com/fwlink/p/?LinkId=247929)
