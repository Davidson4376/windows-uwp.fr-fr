---
author: stevewhims
Description: Design and develop your app in such a way that it functions appropriately on systems with different language and culture configurations.
Search.Refinement.TopicID: 180
title: Directives en matière de globalisation
ms.assetid: 0342DC3F-DDD1-4DD4-872E-A4EC340CAE79
template: detail.hbs
ms.author: stwhi
ms.date: 11/02/2017
ms.topic: article
keywords: windows10, uwp, globalisation, adaptabilité, localisation
ms.localizationpriority: medium
ms.openlocfilehash: 177332515db26eca7cef7a7be75c5752a239a8f1
ms.sourcegitcommit: cd00bb829306871e5103db481cf224ea7fb613f0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/01/2018
ms.locfileid: "5886888"
---
# <a name="guidelines-for-globalization"></a>Directives en matière de globalisation

Concevez et développez votre application afin qu'elle fonctionne de manière appropriée sur les systèmes avec des configurations de langue et de culture différentes. Utilisez les API de [**Globalization**](/uwp/api/Windows.Globalization?branch=live) pour formater les données; évitez toute hypothèse sur votre code à propos de la langue, de la région, de la classification des caractères, des systèmes d'écriture, des formatages date/heure, des chiffres, des devises, des poids et des règles de tri.

| Recommandation | Description |
| ------------- | ----------- |
| Tenez compte de la culture lors de la manipulation et de la comparaison des chaînes. | Par exemple, ne modifiez pas la casse des chaînes avant de les avoir comparées. Consultez [Recommendations en matière d'utilisation des chaînes](/dotnet/standard/base-types/best-practices-strings?branch=live#recommendations_for_string_usage). |
| Ne partez pas du principe que l'assemblage (tri) des chaînes et d'autres données se fait toujours par ordre alphabétique. | Pour les langues qui n’utilisent pas le script latin, le collage se base sur des facteurs tels que la prononciation ou le nombre de coups de crayon. Mêmes les langues qui utilisent le script latin n’utilisent pas toujours le classement alphabétique. Par exemple, dans certaines cultures, un annuaire n’est pas trié par ordre alphabétique. Windows peut gérer le classement pour vous. Mais si vous créez votre propre algorithme de tri, veillez à prendre en compte les méthodes de classement utilisées dans vos marchés cibles. |
| Utilisez les formats appropriés pour les nombres, les dates, les heures, les adresses et les numéros de téléphone. | Ces formats varient selon les cultures, régions, langues et les marchés. Si vous affichez ces données, utilisez les API de [**Globalisation**](/uwp/api/Windows.Globalization?branch=live) pour obtenir le format approprié à un public particulier. Consultez [Globaliser vos formats de date/heure/chiffres](use-global-ready-formats.md). L'ordre d'affichage des noms de familles et des noms spécifiques ainsi que le format des adresses peut également varier. Utilisez les affichages standard pour la date, l'heure et les chiffres. Utilisez les contrôles standard de sélecteur de date, d'heure et de chiffres. Utilisez les informations standard pour les adresses. |
| Prenez en charge les unités de mesure et les devises internationales. | Des unités et des échelles différentes sont utilisées dans différents pays, bien que les systèmes métrique et impérial soient les plus populaires. Veillez à prendre en charge la mesure système correcte si vous utilisez des mesures, telles que la longueur, la température et la superficie. Utilisez la propriété [**GeographicRegion.CurrenciesInUse**](/uwp/api/windows.globalization.geographicregion.CurrenciesInUse) pour obtenir l'ensemble des devises utilisées dans une région. |
| Utilisez Unicode pour le codage des caractères. | Par défaut, Microsoft Visual Studio utilise le codage de caractères Unicode pour tous les documents. Si vous utilisez un autre éditeur, veillez à enregistrer les fichiers sources dans les codages de caractères Unicode appropriés. Toutes les API UWP retournent des chaînes codées selon le codage de caractères UTF-16. |
| Prenez en charge les formats de papier internationaux. | Les formats de papier les plus courants diffèrent entre les pays. Par conséquent, si vous incluez des fonctionnalités qui dépendent du format de papier telles que l’impression, veillez à prendre en charge et tester les formats internationaux courants. |
| Enregistrez la langue du clavier ou de l’IME. | Lorsque votre application invite l'utilisateur à saisir du texte, enregistrez la balise de langue pour le type de clavier actuellement activé ou l'éditeur de mode de saisi (IME). Cela garantit que, lorsque la saisie est affichée ultérieurement, elle sera présentée à l’utilisateur dans le format approprié. Utilisez la propriété [**Language.CurrentInputMethodLanguageTag**](/uwp/api/windows.globalization.language.CurrentInputMethodLanguageTag) pour obtenir la langue d’entrée actuelle. |
| N’utilisez pas la langue pour supposer la région d’un utilisateur et n’utilisez pas la région pour supposer la langue d’un utilisateur. | La langue et la région sont des concepts distincts. Un utilisateur peut parler une variante régionale d’une langue, telle qu’en-GB pour l’anglais parlé en Grande-Bretagne, tout en étant situé dans un autre pays ou région. Déterminez si votre application requière une connaissance de la langue de l’utilisateur (par exemple pour le texte de l’interface utilisateur) ou de son emplacement géographique (par exemple pour la licence). Pour plus d'informations, consultez [Comprendre les langues de profil utilisateur et les langues du manifeste de l’application](manage-language-and-region.md). |
| Les règles de comparaison des balises de langue ne sont pas insignifiantes. | [Les balises de langue BCP-47](http://go.microsoft.com/fwlink/p/?linkid=227302) sont complexes. Il existe un certain nombre de problèmes lors de la comparaison de balises de langue, dont notamment des problèmes de correspondance entre les informations de script, les balises héritées et les différentes variantes régionales. Le système de gestion des ressources dans Windows s’occupe des correspondances pour vous. Vous pouvez spécifier un ensemble de ressources dans des langues quelconques et le système choisit celle qui est appropriée pour l’utilisateur et l’application. Consultez [Ressources d'application et système de gestion des ressources](../../app-resources/index.md) et [Comment le système de gestion de ressources fait correspondre les balises de langue](../../app-resources/how-rms-matches-lang-tags.md). |
| Il convient de concevoir votre interface utilisateur pour accommoder les différentes longueurs de texte et tailles de police, ainsi que les contrôles de saisie de texte. | Dans la mesure où la longueur des chaînes traduites vers différentes langues peuvent varier, vos contrôle d'interface utilisateur devront se dimensionner de façon dynamique à leur contenu. Les caractères courants dans d'autres langues incluent certains symboles inférieurs ou supérieurs qui ne sont pas courants en Anglais (par exemple, Å ou Ņ). Utilisez les tailles de police et les hauteurs de ligne standard pour fournir un espace vertical adéquat. N'oubliez pas que les polices d'autres langues peuvent nécessite des tailles de police minimum plus grande pour rester lisible. Consultez les clases de l'espace de noms [Windows.Globalization.Fonts](/uwp/api/windows.globalization.fonts?branch=live). |
| Prenez en charge la mise en miroir de l'ordre de lecture. | L’alignement de texte et le sens de lecture peuvent être de gauche à droite (par exemple, comme en français) ou de droite à gauche (DàG, comme en arabe ou en hébreu). Si vous localisez votre produit dans des langues qui utilisent un sens de lecture différent du vôtre, veillez à ce que la disposition de vos éléments d’interface utilisateur prenne en charge la mise en miroir. Même des éléments tels que les boutons Précédent, les effets de transition de l’interface utilisateur et les images peuvent avoir à être mis en miroir. Pour plus d’informations, consultez [Ajuster la disposition et les polices, prendre en charge l'écriture DàG](adjust-layout-and-fonts--and-support-rtl.md). |
| Affichez correctement le texte et les polices. | Les paramètres optimaux de police, taille de police et direction du texte varient en fonction des différents marchés. Pour plus d’informations, consultez [**Ajuster la disposition et les polices, prendre en charge l'écriture DàG**](adjust-layout-and-fonts--and-support-rtl.md) et [Polices internationales](loc-international-fonts.md). |

## <a name="important-apis"></a>API importantes
 
* [Globalisation](/uwp/api/Windows.Globalization?branch=live)
* [GeographicRegion.CurrenciesInUse](/uwp/api/windows.globalization.geographicregion.CurrenciesInUse)
* [Language.CurrentInputMethodLanguageTag](/uwp/api/windows.globalization.language.CurrentInputMethodLanguageTag)
* [Windows.Globalization.Fonts](/uwp/api/windows.globalization.fonts?branch=live)

## <a name="related-topics"></a>Rubriquesassociées

* [Recommendations en matière d'utilisation des chaînes](/dotnet/standard/base-types/best-practices-strings?branch=live#recommendations_for_string_usage)
* [Globaliser vos formats de date/heure/chiffres](use-global-ready-formats.md)
* [Comprendre les langues de profil utilisateur et les langues du manifeste de l’application](manage-language-and-region.md)
* [Balises de langue BCP-47](http://go.microsoft.com/fwlink/p/?linkid=227302)
* [Ressources d’application et système de gestion des ressources](../../app-resources/index.md)
* [Comment le système de gestion des ressources met en correspondance les balises de langue](../../app-resources/how-rms-matches-lang-tags.md)
* [Ajuster la disposition et les polices, et prendre en charge l'écriture DàG](adjust-layout-and-fonts--and-support-rtl.md)
* [Polices internationales](loc-international-fonts.md)
* [Rendre votre application localisable](prepare-your-app-for-localization.md)

## <a name="samples"></a>Exemples

* [Exemple de préférences de globalisation](http://go.microsoft.com/fwlink/p/?linkid=231608)
