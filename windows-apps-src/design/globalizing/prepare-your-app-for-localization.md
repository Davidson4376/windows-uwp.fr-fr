---
author: stevewhims
Description: A localized app is one that can be localized to other markets, languages, or regions without uncovering any functional defects in the app. The most essential property of a localizable app is that its executable code has been cleanly separated from its localizable resources.
title: Rendre votre application localisable
ms.assetid: 06E1D4BB-59EA-4D71-99AC-7CB93D2A58A7
template: detail.hbs
ms.author: stwhi
ms.date: 11/07/2017
ms.topic: article
keywords: windows10, uwp, globalisation, adaptabilité, localisation
ms.localizationpriority: medium
ms.openlocfilehash: 48244889dd927f41d0998214cf1120377c4bb251
ms.sourcegitcommit: 93c0a60cf531c7d9fe7b00e7cf78df86906f9d6e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/21/2018
ms.locfileid: "7561822"
---
# <a name="make-your-app-localizable"></a>Rendre votre application localisable

Une application localisée est une application qui peut être localisée pour d'autres marchés, langues ou régions sans présenter de défaillance fonctionnelle. Le caractère localisable de l'application nécessite que son code exécutable soit proprement séparé de ses ressources localisables. Par conséquent, vous devez identifier les ressources de votre application qui doivent être localisées. Demandez-vous ce qui doit changer si votre application doit être localisée pour d’autres marchés.

Nous vous recommandons également de vous familiariser avec les [directives en matière de globalisation](guidelines-and-checklist-for-globalizing-your-app.md).

## <a name="put-your-strings-into-resources-files-resw"></a>Placer vos chaînes dans les fichiers Ressources (.resw)

Ne codez pas en dur les opérateurs de chaîne dans votre code impératif, dans le balisage XAML ou dans le manifeste du package de votre application. Il convient plutôt de placer vos chaînes dans des fichiers Ressources (.resw) afin de pouvoir les adapter aux différents marchés locaux, quels que soient les codes binaires de votre application. Pour plus de détails, consultez [Localiser les chaînes dans l’interface utilisateur et le manifeste du package d’application](../../app-resources/localize-strings-ui-manifest.md).

Cette rubrique vous présente également comment ajouter des commentaires à votre fichier Ressources (.resw) par défaut. Par exemple, si vous adoptez une voix ou un ton informel, veillez à le stipuler dans les commentaires. Afin de minimiser les dépenses, confirmez également que seules les chaînes devant être traduites sont fournies aux traducteurs.

Définissez correctement la langue par défaut de votre application dans le fichier source du manifeste du package de votre application (le fichier `Package.appxmanifest`). La langue par défaut détermine la langue utilisée lorsque la langue favorite de l'utilisateur ne correspond à aucune des langues prises en charge par votre application. Marquez toutes les ressources avec leur langue, (même celles de votre langue par défaut, par exemple `\Assets\en-us\Logo.png`), de sorte que le système puisse savoir en quelle langue se trouve la ressource et comment elle est utilisée dans des situations particulières.

## <a name="tailor-your-images-and-other-file-resources-for-language"></a>Adapter vos images et autres ressources en fichier pour chaque langue

Dans l’idéal, vous pourrez globaliser vos images&mdash;, c'est-à-dire les désolidariser de la culture. Pour toutes les images et autres ressources en fichier pour lesquels c'est impossible, créez tout autant de variantes que nécessaire et placez les qualificateurs de langue appropriés dans les noms de leur fichier ou dossier. Pour en savoir plus, consultez [Adaptez vos ressources pour la langue, l’échelle, le contraste élevé et d’autres qualificateurs](../../app-resources/tailor-resources-lang-scale-contrast.md)).

Pour réduire les frais de localisation, ne placez aucun texte ou matériel culturel dans les images. Une image appropriée dans votre propre culture peut être offensante ou mal interprétée dans d’autres cultures. Évitez d'utiliser des images spécifiques à une culture, par exemple des boîtes aux lettres, objet différent ou inexistant selon la zone du globe. Évitez les symboles religieux, les animaux, les illustrations politiques ou encore les images spécifiques au genre. Les illustrations de chaire, de parties du corps ou de gestes des mains peuvent également constituer des sujets sensibles. S'il n'est pas possible d'éviter tout ces sujets, vos images doivent être localisées avec minutie. Si vous localisez du texte vers une langue dotée d’un sens de lecture différent du vôtre, l’utilisation d’images et d’effets symétriques facilite la prise en charge de la mise en miroir.

Évitez d'utiliser du texte dans les images et de la parole dans les fichiers audio/vidéo.

## <a name="the-use-of-color-in-your-app"></a>L'utilisation des couleurs dans votre application

Sélectionnez les couleurs avec soin. L'utilisation d'associations de couleurs correspondant à des drapeaux nationaux ou des mouvements politiques peut être problématique. Les choix de couleurs nécessitent probablement l'examen d'experts culturels. L'utilisation de couleurs implique également des problèmes d'accessibilité. Si vous utilisez la couleur pour transmettre un sens, vous devez transmettre les mêmes informations par d'autres moyens: la taille, la forme ou encore le libellé.

## <a name="consider-factoring-your-strings-into-sentences"></a>Tenir compte de la factorisation de vos chaînes en phrases

Utilisez des chaînes de taille appropriée. Les chaînes courtes sont plus faciles à traduire. Elles permettent le recyclage de la traduction (qui constitue une économie dans la mesure où la même chaîne n'est pas envoyée au localiseur plus d'une fois). En outre, il est probable que les chaînes d'une longueur extrême ne seront pas pris en charge par les outils de localisation.

Mais le risque de réutiliser une chaîne dans des contextes différents entre en conflit avec cette directive. Selon le contexte, même de simples mots comme &quot;sur&quot; et &quot;hors&quot; peut être traduit différemment. En anglais, «on» et «off» peuvent être utilisés pour activer et désactiver le mode Avion, la fonctionnalité bluetooth et des appareils. En revanche, en italien, la traduction dépend du contexte de ce qui est activé et désactivé. Vous devez créer une paire de chaînes pour chaque contexte. Vous pouvez réutiliser des chaînes si les deux contextes sont identiques. Par exemple, vous pouvez réutiliser la chaîne «Volume» pour le volume des effets sonores et le volume de la musique, car les deux font référence à l’intensité sonore. Vous ne devez pas réutiliser la même chaîne pour faire référence au volume du disque dur, car le contexte et la signification sont différents, et le mot peut être traduit différemment.

De plus, une chaîne comme «text» ou «fax» peut être utilisée en tant que verbe ou substantif en anglais, ce qui peut compliquer le processus de traduction. Créez plutôt deux chaînes distinctes pour la forme verbale et la forme nominale. Lorsque vous n’êtes pas certain que les contextes sont les mêmes, ne prenez pas de risque et utilisez deux chaînes distinctes.

En bref, factorisez vos chaînes en morceaux fonctionnant dans tous les contextes. Dans certaines cas, une chaîne devra constituer une phrase entière.

Considérez la chaîne suivante: «le {0} pas pu être synchronisé.»

Divers mots pourraient remplacer {0}, par exemple «rendez-vous», «tâche» ou «document». Si cet exemple ne pose aucun problème en anglais, il n’en va pas de même dans tous ces cas pour sa traduction en allemand, par exemple. Remarquez que dans les phrases allemandes suivantes, certains mots dans la chaîne de modèle («Der», «Die», «Das») doivent correspondre au mot paramétré:

| Anglais                                    | Allemand                                           |
|:------------------------------------------ |:------------------------------------------------ |
| The appointment could not be synchronized. | Der Termin konnte nicht synchronisiert werden.   |
| The task could not be synchronized.        | Die Aufgabe konnte nicht synchronisiert werden.  |
| The document could not be synchronized.    | Das Dokument konnte nicht synchronisiert werden. |

Un autre exemple, la phrase «me rappeler dans {0} minute (s).» Si «minute(s)» fonctionne en français, cela n’est pas forcément le cas avec d’autres langues. Par exemple, le polonais utilise «minuta», «minuty» ou «minut» selon le contexte.

Pour résoudre ce problème, localisez la phrase entière, plutôt qu’un mot individuel. Cela peut paraître comme une charge de travail supplémentaire et une solution dépourvue d’élégance, mais il s’agit de la solution optimale pour les raisons suivantes :

-   Un message grammaticalement correct sera affiché pour toutes les langues.
-   Votre traducteur n’aura pas besoin de vous demander par quoi les chaînes seront remplacées.
-   Vous n’aurez pas besoin d’implémenter un correctif de code onéreux lorsqu’un problème de ce type se révélera une fois votre application terminée.

## <a name="other-considerations-for-strings"></a>Autres éléments à prendre en considération pour les chaînes

Évitez les expressions familières et les métaphores dans les chaînes dont vous êtes l'auteur dans votre langue par défaut. Le langage propre à une tranche de population, telle qu’une culture ou une classe d’âge, peut être difficile à comprendre: seuls les représentants de cette tranche de population emploient ce langage. De la même façon, les métaphores peuvent avoir un sens pour une personne, mais ne rien évoquer pour une autre. Par exemple, &quot;vendanger&quot; a un sens précis dans le jargon du football, qui échappe aux non-initiés.

N’utilisez pas de jargon, d’abréviations ni d’acronymes. Un langage technique aura moins de chances d’être compris d’un public non initié ou de personnes issues d’autres cultures ou régions et il sera difficile à traduire. Ce vocabulaire n’est pas utilisé dans les conversations de tous les jours. Un langage technique s’affiche souvent dans les messages d’erreur pour identifier les problèmes matériels et logiciels, mais vos chaînes doivent être techniques *uniquement si l’utilisateur a besoin de ce niveau d’informations et peut agir en conséquence ou trouver une personne qui le fera*.

L'utilisation d'une voix ou d'un ton informel dans vos chaînes est un bon choix. Dans votre fichier Ressources (.resw) par défaut, vous pouvez utiliser les commentaires pour indiquer cette intention.

## <a name="pseudo-localization"></a>Pseudo-localisation

Peuso-localisez votre application pour découvrir tout problème d'adaptabilité. La pseudo-localisation est un type de localisation test ou de test de divulgation. Vous produisez un ensemble de ressources qui ne sont pas réellement traduites. Elles en ont seulement l'apparence. Vos chaînes sont environ 40% plus longues que les chaînes de votre langue par défaut. Par exemple, elles contiennent des séparateurs afin que vous puissiez avoir un aperçu de leur possible tronquage dans l'interface utilisateur.

## <a name="geopolitical-awareness"></a>Conscience géopolitique

Évitez toute infraction politique dans les cartes ou en faisant référence à des régions. Parfois, les cartes peuvent inclure des frontières nationales ou régionales controversées et représentent une source fréquente d’infraction politique. Veillez à ce que toute interface utilisateur utilisée pour la sélection d’une nation lui fasse référence en tant que &quot;pays/région&quot;. Répertorier un territoire contesté dans une liste intitulée &quot;pays&quot;&mdash;comme sous la forme d'une adresse&mdash;peut offenser certains utilisateurs.

## <a name="language--and-region-changed-events"></a>Événements modifiés selon la langue et la région

Abonnez-vous aux événements notifiés lorsque les paramètres de langue et de région du système changent. Ainsi, vous êtes en mesure de recharger les ressources, le cas échéant. Pour plus de détails, consultez [Mise à jour de chaînes suite à des événements de modification de valeur de qualificateur](../../app-resources/localize-strings-ui-manifest.md#updating-strings-in-response-to-qualifier-value-change-events) et [Mise à jour d'images suite à des événements de modification de valeur de qualificateur](../../app-resources/images-tailored-for-scale-theme-contrast.md#updating-images-in-response-to-qualifier-value-change-events).

## <a name="ensure-the-correct-parameter-order-when-formatting-strings"></a>Veiller au bon ordre de paramètre lors du formatage des chaînes

Ne supposez pas que toutes les langues expriment les paramètres dans le même ordre. Par exemple, examinez ce format.

```csharp
    string.Format("Every {0} {1}", monthName, dayNumber); // For example, "Every April 1".
```

La chaîne de format de cet exemple fonctionne pour l'anglais (États-Unis). Mais elle n'est pas appropriée pour l'allemand (Allemagne) dans la mesure où le jour et le mois sont affichés dans l'ordre inverse. Assurez-vous que le traducteur connaisse l’intention de chacun des paramètres afin qu’ils peuvent inverser l’ordre des éléments de format dans la chaîne de format (par exemple, «{1} {0}») en fonction de la langue cible.

## <a name="dont-over-localize"></a>Ne sur-localisez pas.

Soumettez uniquement un langage naturel au traducteurs; n'envoyez pas de langage de programmation ou de balisage. Une balise `<link>` n’est pas un langage naturel. Prenons les exemples suivants.

| Ne localisez pas ceci                   | Localisez ceci |
|:--------------------------------------- |:-------------------------- |
| &lt;lien&gt;conditions d’utilisation&lt;/lien&gt;   | conditions d’utilisation               |
| &lt;lien&gt;politique de confidentialité&lt;/lien&gt; | politique de confidentialité             |

L'inclusion de la balise `<link>` dans votre fichier Ressources (.resw) signifie également qu'elle sera probablement traduite. La traduction de la balise la rendrait non valide. Si vous disposez de longues chaînes nécessitant l'inclusion de balisage afin de conserver le contexte et de garantir l'ordre, stipulez clairement dans les commentaires ce qu'il ne faut pas traduire.

## <a name="choose-an-appropriate-translation-approach"></a>Choisir une approche de traduction appropriée

Une fois que les chaînes ont été séparées en fichiers de ressources, elles peuvent être traduites. Le moment idéal pour traduire des chaînes est après la finalisation de ces chaînes dans le projet, laquelle survient habituellement vers la fin d’un projet. Vous pouvez adopter des approches différentes du processus de traduction. Cela peut dépendre du volume des chaînes à traduire, le nombre de langues cibles et la manière dont la traduction sera réalisée (par exemple, en interne ou en faisant appel à un prestataire externe).

Envisagez ces options.

- **Les fichiers de ressources peuvent être traduits en étant ouverts directement dans le projet.** Cette approche fonctionne bien pour un projet qui possède un volume réduit de chaînes devant être traduites en deux ou trois langues. Elle est adaptée pour un scénario dans lequel un développeur parlerait plusieurs langues et accepterait de traiter le processus de traduction. Cette approche a l’avantage d’être rapide, de ne requérir aucun outil et de minimiser les risques d’erreurs de traduction. Mais elle n’est pas évolutive. En particulier, les ressources dans les différentes langues peuvent aisément se désynchroniser, ce qui peut entraîner de mauvaises expériences utilisateur et des difficultés de maintenance.
- **Les fichiers de ressources de type chaîne sont au format texte XML ou ResJSON et peuvent être confiés pour traduction à l’aide d’un éditeur de texte. Les fichiers traduits pourront alors être copiés en retour dans le projet.** Cette approche présente un risque, car les traducteurs pourraient modifier accidentellement les balises XML. Cependant, elle permet le déroulement de la traduction en dehors du projet Microsoft Visual Studio. Cette approche pourrait bien fonctionner pour les projets devant être traduits vers un nombre réduit de langues. Le format XLIFF est un format XML spécifiquement conçu pour être utilisé en localisation et devrait être bien pris en charge par les prestataires de localisation et les outils de localisation. Vous pouvez utiliser le [Kit de ressources pour application multilingue](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/jj572370.aspx) pour générer des fichiers XLIFF à partir d’autres fichiers de ressources, tels que les fichiers .resw ou .resjson.

Un transfert vers des traducteurs peut être nécessaire pour d’autres fichiers, tels que des images ou des fichiers audio.

De plus, considérez ces suggestions.

- **Utilisez un outil de localisation.** De nombreux outils de localisation sont disponibles pour analyser les fichiers de ressources et permettre la modification par les traducteurs des seules chaînes traduisibles. Cette approche réduit le risque qu’un traducteur modifie accidentellement les balises XML. Elle présente cependant l’inconvénient d’introduire un nouvel outil et un nouveau processus dans le processus de localisation. Un outil de localisation est approprié pour les projets avec un volume élevé de chaînes, mais un nombre réduit de langues. Pour en savoir plus, consultez [Comment utiliser le kit de ressources MultilingualApToolkit](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/jj572370.aspx).
- **Faites appel à un prestataire de localisation.** Considérez l’utilisation d’un prestataire de localisation si votre projet contient un volume élevé de chaînes et doit être traduit en de nombreuses langues. Un prestataire de localisation peut conseiller des outils et des processus, ainsi que traduire vos fichiers de ressources. Il s’agit d’une solution idéale, mais c’est aussi l’option la plus coûteuse et elle peut augmenter le délai de traitement de votre contenu traduit.

## <a name="keep-access-keys-and-labels-consistent"></a>Utiliser des touches d’accès rapide et des étiquettes cohérentes

La «synchronisation» des touches d’accès rapide utilisées dans les options d’accessibilité avec l’affichage des touches d’accès rapide localisées relève du défi, car les deux ressources de type chaîne sont classées dans deux sections distinctes. Ajoutez des commentaires pour la chaîne d’étiquette, par exemple: `Make sure that the emphasized shortcut key  is synchronized with the access key.`

## <a name="support-furigana-for-japanese-strings-that-can-be-sorted"></a>Prendre en charge les furigana pour les chaînes japonaises qui peuvent être triées

Les caractères japonais kanji possèdent la propriété d’avoir plusieurs lectures (prononciations) selon le mot dans lesquels ils sont utilisés. Cela génère des problèmes lorsque vous tentez de trier des objets nommés en japonais, tels que des noms d’applications, de fichiers, de chansons, etc. Dans le passé, les kanji japonais ont généralement été triés dans un ordre compréhensible par les machines, nommé XJIS. Malheureusement, comme cet ordre de tri n’est pas phonétique, il n’est pas très utile pour les hommes.

Les *furigana* contournent ce problème en permettant à l’utilisateur ou au créateur de spécifier la prononciation des caractères utilisés. Si vous utilisez la procédure suivante pour ajouter des furigana au nom de votre application, vous pouvez garantir qu’elle sera classée à l’emplacement approprié dans la liste des applications. Si le nom de votre application contient des caractères kanji et qu’aucun furigana n’est fourni lorsque la langue de l’interface utilisateur ou l’ordre de tri est défini sur le japonais, Windows s’efforce de générer la prononciation appropriée. Toutefois, il est possible de classer les noms d’applications contenant des prononciations rares ou uniques sous une prononciation plus courante. Ainsi, la meilleure pratique pour les applications japonaises (notamment celles dont le nom contient des caractères kanji) consiste à fournir une version avec furigana de leur nom d’application dans le cadre du processus de localisation japonaise.

1.  Ajoutez «ms-resource:Appname» comme nom complet du package et nom complet de l’application.
2.  Créez un dossier ja-JP sous le dossier «strings» et ajoutez deux fichiers de ressources comme suit:

    ``` syntax
    strings\
        en-us\
        ja-jp\
            Resources.altform-msft-phonetic.resw
            Resources.resw
    ```

3.  Dans Resources.resw pour le dossier général ja-JP: ajoutez une ressource de type chaîne pour le nom d’application «希蒼».
4.  Dans Resources.altform-msft-phonetic.resw pour les ressources japonaises avec furigana: ajoutez la valeur de furigana pour le nom d’application «のあ».

L’utilisateur peut rechercher le nom d’application « 希蒼 » en utilisant à la fois la valeur de furigana « のあ » (noa) et la valeur phonétique (en utilisant la fonction **GetPhonetic** à partir de l’éditeur de méthode d’entrée (IME)) « まれあお » (mare-ao).

Le classement suit le format de l’application **Région** du Panneau de configuration:

-   Sous un paramètre régional utilisateur japonais,
    -   Si les furigana sont activés, alors le nom «希蒼» est classé sous «の».
    -   En l’absence de furigana, alors le nom «希蒼» est classé sous «ま».
-   Sous un paramètre régional d’utilisateur non japonais,
    -   Si les furigana sont activés, alors le nom «希蒼» est classé sous «の».
    -   En l’absence de furigana, alors le nom «希蒼» est classé sous «漢字».

## <a name="related-topics"></a>Rubriquesassociées

* [Directives en matière de globalisation](guidelines-and-checklist-for-globalizing-your-app.md)
* [Localiser les chaînes dans l’interface utilisateur et le manifeste du package d’application](../../app-resources/localize-strings-ui-manifest.md)
* [Adaptez vos ressources pour la langue, l’échelle, le contraste élevé et d’autres qualificateurs](../../app-resources/tailor-resources-lang-scale-contrast.md)
* [Ajuster la disposition et les polices, et prendre en charge le sens du flux DàG](adjust-layout-and-fonts--and-support-rtl.md)
* [Mise à jour d’images suite à des événements de modification de valeur de qualificateur](../../app-resources/images-tailored-for-scale-theme-contrast.md#updating-images-in-response-to-qualifier-value-change-events)

## <a name="samples"></a>Exemples

* [Exemple de ressources d’application et de localisation](http://go.microsoft.com/fwlink/p/?linkid=254478)
