---
Description: Une application localisée est une application qui peut être localisée pour d'autres marchés, langues ou régions sans présenter de défaillance fonctionnelle. Le caractère localisable de l'application nécessite que son code exécutable soit proprement séparé de ses ressources localisables.
title: Rendre votre application localisable
ms.assetid: 06E1D4BB-59EA-4D71-99AC-7CB93D2A58A7
template: detail.hbs
ms.date: 11/07/2017
ms.topic: article
keywords: windows 10, uwp, globalisation, adaptabilité, localisation
ms.localizationpriority: medium
ms.openlocfilehash: fb16f682e5e1f57196737a6e15a9ffbadbfd0e84
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57626984"
---
# <a name="make-your-app-localizable"></a>Rendre votre application localisable

Une application localisée est une application qui peut être localisée pour d'autres marchés, langues ou régions sans présenter de défaillance fonctionnelle. Le caractère localisable de l'application nécessite que son code exécutable soit proprement séparé de ses ressources localisables. Par conséquent, vous devez identifier les ressources de votre application qui doivent être localisées. Demandez-vous ce qui doit changer si votre application doit être localisée pour d’autres marchés.

Nous vous recommandons également de vous familiariser avec les [directives en matière de globalisation](guidelines-and-checklist-for-globalizing-your-app.md).

## <a name="put-your-strings-into-resources-files-resw"></a>Placer vos chaînes dans les fichiers Ressources (.resw)

Ne pas les littéraux de chaîne de coder en dur dans votre code impératif, le balisage XAML, ni dans le manifeste de package de votre application. Il convient plutôt de placer vos chaînes dans des fichiers Ressources (.resw) afin de pouvoir les adapter aux différents marchés locaux, quels que soient les codes binaires de votre application. Pour plus de détails, consultez [Localiser les chaînes dans l’interface utilisateur et le manifeste du package d’application](../../app-resources/localize-strings-ui-manifest.md).

Cette rubrique vous présente également comment ajouter des commentaires à votre fichier Ressources (.resw) par défaut. Par exemple, si vous adoptez une voix ou un ton informel, veillez à le stipuler dans les commentaires. Afin de minimiser les dépenses, confirmez également que seules les chaînes devant être traduites sont fournies aux traducteurs.

Définissez correctement la langue par défaut de votre application dans le fichier source du manifeste du package de votre application (le fichier `Package.appxmanifest`). La langue par défaut détermine la langue utilisée lorsque la langue favorite de l'utilisateur ne correspond à aucune des langues prises en charge par votre application. Marquez toutes les ressources avec leur langue, (même celles de votre langue par défaut, par exemple `\Assets\en-us\Logo.png`), de sorte que le système puisse savoir en quelle langue se trouve la ressource et comment elle est utilisée dans des situations particulières.

## <a name="tailor-your-images-and-other-file-resources-for-language"></a>Adapter vos images et autres ressources en fichier pour chaque langue

Dans l’idéal, vous pourrez globaliser vos images&mdash;, c'est-à-dire les désolidariser de la culture. Pour toutes les images et autres ressources en fichier pour lesquels c'est impossible, créez tout autant de variantes que nécessaire et placez les qualificateurs de langue appropriés dans les noms de leur fichier ou dossier. Pour en savoir plus, consultez [Adaptez vos ressources pour la langue, l’échelle, le contraste élevé et d’autres qualificateurs](../../app-resources/tailor-resources-lang-scale-contrast.md)).

Pour réduire les frais de localisation, ne placez aucun texte ou matériel culturel dans les images. Une image appropriée dans votre propre culture peut être offensante ou mal interprétée dans d’autres cultures. Évitez d'utiliser des images spécifiques à une culture, par exemple des boîtes aux lettres, objet différent ou inexistant selon la zone du globe. Évitez les symboles religieux, les animaux, les illustrations politiques ou encore les images spécifiques au genre. Les illustrations de chaire, de parties du corps ou de gestes des mains peuvent également constituer des sujets sensibles. S'il n'est pas possible d'éviter tout ces sujets, vos images doivent être localisées avec minutie. Si vous localisez du texte vers une langue dotée d’un sens de lecture différent du vôtre, l’utilisation d’images et d’effets symétriques facilite la prise en charge de la mise en miroir.

Évitez d'utiliser du texte dans les images et de la parole dans les fichiers audio/vidéo.

## <a name="the-use-of-color-in-your-app"></a>L'utilisation des couleurs dans votre application

Sélectionnez les couleurs avec soin. L'utilisation d'associations de couleurs correspondant à des drapeaux nationaux ou des mouvements politiques peut être problématique. Les choix de couleurs nécessitent probablement l'examen d'experts culturels. L'utilisation de couleurs implique également des problèmes d'accessibilité. Si vous utilisez la couleur pour transmettre un sens, vous devez transmettre les mêmes informations par d'autres moyens : la taille, la forme ou encore le libellé.

## <a name="consider-factoring-your-strings-into-sentences"></a>Tenir compte de la factorisation de vos chaînes en phrases

Utilisez des chaînes de taille appropriée. Les chaînes courtes sont plus faciles à traduire. Elles permettent le recyclage de la traduction (qui constitue une économie dans la mesure où la même chaîne n'est pas envoyée au localiseur plus d'une fois). En outre, il est probable que les chaînes d'une longueur extrême ne seront pas pris en charge par les outils de localisation.

Mais le risque de réutiliser une chaîne dans des contextes différents entre en conflit avec cette directive. Selon le contexte, même de simples mots comme &quot;sur&quot; et &quot;hors&quot; peut être traduit différemment. En anglais, « on » et « off » peuvent être utilisés pour activer et désactiver le mode Avion, la fonctionnalité Bluetooth et des appareils. En revanche, en italien, la traduction dépend du contexte de ce qui est activé et désactivé. Vous devez créer une paire de chaînes pour chaque contexte. Vous pouvez réutiliser des chaînes si les deux contextes sont identiques. Par exemple, vous pouvez réutiliser la chaîne « Volume » pour le volume des effets sonores et le volume de la musique, car les deux font référence à l’intensité sonore. Vous ne devez pas réutiliser la même chaîne pour faire référence au volume du disque dur, car le contexte et la signification sont différents, et le mot peut être traduit différemment.

De plus, une chaîne comme « text » ou « fax » peut être utilisée en tant que verbe ou substantif en anglais, ce qui peut compliquer le processus de traduction. Créez plutôt deux chaînes distinctes pour la forme verbale et la forme nominale. Lorsque vous n’êtes pas certain que les contextes sont les mêmes, ne prenez pas de risque et utilisez deux chaînes distinctes.

En bref, factorisez vos chaînes en morceaux fonctionnant dans tous les contextes. Dans certaines cas, une chaîne devra constituer une phrase entière.

Considérez la chaîne suivante : « Le {0} pas pu être synchronisé. »

Une variété de mots pourrait remplacer {0}, comme « rendez-vous », « tâche » ou « document ». Si cet exemple ne pose aucun problème en anglais, il n’en va pas de même dans tous ces cas pour sa traduction en allemand, par exemple. Remarquez que dans les phrases allemandes suivantes, certains mots dans la chaîne de modèle (« Der », « Die », « Das ») doivent correspondre au mot paramétré :

| Anglais                                    | Allemand                                           |
|:------------------------------------------ |:------------------------------------------------ |
| The appointment could not be synchronized. | Der Termin konnte nicht synchronisiert werden.   |
| The task could not be synchronized.        | Die Aufgabe konnte nicht synchronisiert werden.  |
| The document could not be synchronized.    | Das Dokument konnte nicht synchronisiert werden. |

Autre exemple, considérez la phrase « me le rappeler dans {0} minute (s). » Si « minute(s) » fonctionne en français, cela n’est pas forcément le cas avec d’autres langues. Par exemple, le polonais utilise « minuta », « minuty » ou « minut » selon le contexte.

Pour résoudre ce problème, localisez la phrase entière, plutôt qu’un mot individuel. Cela peut paraître comme une charge de travail supplémentaire et une solution dépourvue d’élégance, mais il s’agit de la solution optimale pour les raisons suivantes :

- Un message grammaticalement correct sera affiché pour toutes les langues.
- Votre traducteur n’aura pas besoin de vous demander par quoi les chaînes seront remplacées.
- Vous n’aurez pas besoin d’implémenter un correctif de code onéreux lorsqu’un problème de ce type se révélera une fois votre application terminée.

## <a name="other-considerations-for-strings"></a>Autres éléments à prendre en considération pour les chaînes

Évitez les expressions familières et les métaphores dans les chaînes dont vous êtes l'auteur dans votre langue par défaut. Le langage propre à une tranche de population, telle qu’une culture ou une classe d’âge, peut être difficile à comprendre : seuls les représentants de cette tranche de population emploient ce langage. De la même façon, les métaphores peuvent avoir un sens pour une personne, mais ne rien évoquer pour une autre. Par exemple, &quot;vendanger&quot; a un sens précis dans le jargon du football, qui échappe aux non-initiés.

N’utilisez pas de jargon, d’abréviations ni d’acronymes. Un langage technique aura moins de chances d’être compris d’un public non initié ou de personnes issues d’autres cultures ou régions et il sera difficile à traduire. Ce vocabulaire n’est pas utilisé dans les conversations de tous les jours. Un langage technique s’affiche souvent dans les messages d’erreur pour identifier les problèmes matériels et logiciels, mais vos chaînes doivent être techniques *uniquement si l’utilisateur a besoin de ce niveau d’informations et peut agir en conséquence ou trouver une personne qui le fera*.

L'utilisation d'une voix ou d'un ton informel dans vos chaînes est un bon choix. Dans votre fichier Ressources (.resw) par défaut, vous pouvez utiliser les commentaires pour indiquer cette intention.

## <a name="pseudo-localization"></a>Pseudo-localisation

Peuso-localisez votre application pour découvrir tout problème d'adaptabilité. La pseudo-localisation est un type de localisation test ou de test de divulgation. Vous produisez un ensemble de ressources qui ne sont pas réellement traduites. Elles en ont seulement l'apparence. Vos chaînes sont environ 40 % plus longues que les chaînes de votre langue par défaut. Par exemple, elles contiennent des séparateurs afin que vous puissiez avoir un aperçu de leur possible tronquage dans l'interface utilisateur.

## <a name="deployment-considerations"></a>Considérations relatives au déploiement

Lorsque vous installez une application qui contient les données de la langue localisée, vous constaterez peut-être que seule la langue par défaut est disponible pour l’application même si vous avez inclus au départ les ressources pour plusieurs langues. Il s’agit, car le processus d’installation est optimisé pour installer uniquement les ressources de langue qui correspond à la langue actuelle et la culture de l’appareil. Par conséquent, si votre appareil est configuré pour en-US, uniquement les ressources de langue en-US sont installés avec votre application.

> [!NOTE]
> Il n’est pas possible d’installer la prise en charge de langues supplémentaires pour votre application après l’installation initiale. Si vous modifiez la langue par défaut après l’installation d’une application, l’application continue d’utiliser uniquement les ressources de langue d’origine.

Si vous souhaitez vérifier que toutes les ressources linguistiques sont disponibles après l’installation, créez un fichier de configuration du package d’application qui spécifie que certaines ressources sont nécessaires pendant l’installation (y compris les ressources de langue). Cette fonctionnalité d’installation optimisé est automatiquement activée lors .appxbundle de votre application est généré lors de l’empaquetage. Pour plus d’informations, consultez [vous assurer que les ressources sont installées sur un appareil requière ou si un appareil](https://docs.microsoft.com/en-us/previous-versions/dn482043(v=vs.140)).

Si vous le souhaitez, pour que toutes les ressources sont installés (et pas seulement un sous-ensemble), vous pouvez désactiver la génération de .appxbundle lorsque vous empaquetez votre application. Cela n’est toutefois recommandé car il peut augmenter l’heure d’installation de votre application.

Désactiver la génération automatique de la .appxbundle en définissant l’attribut « Générer un lot d’applications » à « jamais » :

1. Dans Visual Studio, cliquez sur le nom du projet
2. Sélectionnez **Store** -> **créer des packages d’application...**
3. Dans le **créer vos Packages** boîte de dialogue, sélectionnez **je souhaite créer des packages à télécharger dans le Microsoft Store à l’aide d’un nouveau nom d’application** puis cliquez sur **suivant**.
4. Dans le **sélectionner un nom d’application** boîte de dialogue, sélectionnez/Créer une application de noms pour votre package.
5. Dans le **sélectionner et configurer des Packages** boîte de dialogue, définissez **générer le lot d’application** à **jamais**.

## <a name="geopolitical-awareness"></a>Conscience géopolitique

Évitez toute infraction politique dans les cartes ou en faisant référence à des régions. Parfois, les cartes peuvent inclure des frontières nationales ou régionales controversées et représentent une source fréquente d’infraction politique. Veillez à ce que toute interface utilisateur utilisée pour la sélection d’une nation lui fasse référence en tant que &quot;pays/région&quot;. Répertorier un territoire contesté dans une liste intitulée &quot;pays&quot;&mdash;comme sous la forme d'une adresse&mdash;peut offenser certains utilisateurs.

## <a name="language--and-region-changed-events"></a>Événements modifiés selon la langue et la région

Abonnez-vous aux événements notifiés lorsque les paramètres de langue et de région du système changent. Ainsi, vous êtes en mesure de recharger les ressources, le cas échéant. Pour plus de détails, consultez [Mise à jour de chaînes suite à des événements de modification de valeur de qualificateur](../../app-resources/localize-strings-ui-manifest.md#updating-strings-in-response-to-qualifier-value-change-events) et [Mise à jour d'images suite à des événements de modification de valeur de qualificateur](../../app-resources/images-tailored-for-scale-theme-contrast.md#updating-images-in-response-to-qualifier-value-change-events).

## <a name="ensure-the-correct-parameter-order-when-formatting-strings"></a>Veiller au bon ordre de paramètre lors du formatage des chaînes

Ne supposez pas que toutes les langues expriment les paramètres dans le même ordre. Par exemple, examinez ce format.

```csharp
    string.Format("Every {0} {1}", monthName, dayNumber); // For example, "Every April 1".
```

La chaîne de format de cet exemple fonctionne pour l'anglais (États-Unis). Mais elle n'est pas appropriée pour l'allemand (Allemagne) dans la mesure où le jour et le mois sont affichés dans l'ordre inverse. Vérifiez que le traducteur sait l’intention de chacun des paramètres afin qu’ils inverser l’ordre des éléments de format dans la chaîne de format (par exemple, «{1} {0}») en fonction de la langue cible.

## <a name="dont-over-localize"></a>Ne sur-localisez pas.

Soumettez uniquement un langage naturel au traducteurs ; n'envoyez pas de langage de programmation ou de balisage. Une balise `<link>` n’est pas un langage naturel. Prenons les exemples suivants.

| Ne localisez pas ceci                   | Localisez ceci |
|:--------------------------------------- |:-------------------------- |
| &lt;lien&gt;conditions d’utilisation&lt;/lien&gt;   | conditions d’utilisation               |
| &lt;lien&gt;politique de confidentialité&lt;/lien&gt; | politique de confidentialité             |

L'inclusion de la balise `<link>` dans votre fichier Ressources (.resw) signifie également qu'elle sera probablement traduite. La traduction de la balise la rendrait non valide. Si vous disposez de longues chaînes nécessitant l'inclusion de balisage afin de conserver le contexte et de garantir l'ordre, stipulez clairement dans les commentaires ce qu'il ne faut pas traduire.

## <a name="choose-an-appropriate-translation-approach"></a>Choisir une approche de traduction appropriée

Une fois que les chaînes ont été séparées en fichiers de ressources, elles peuvent être traduites. Le moment idéal pour traduire des chaînes est après la finalisation de ces chaînes dans le projet, laquelle survient habituellement vers la fin d’un projet. Vous pouvez adopter des approches différentes du processus de traduction. Cela peut dépendre du volume des chaînes à traduire, le nombre de langues cibles et la manière dont la traduction sera réalisée (par exemple, en interne ou en faisant appel à un prestataire externe).

Envisagez ces options.

- **Les fichiers de ressources peuvent être traduites en les ouvrant directement dans le projet.** Cette approche fonctionne bien pour un projet qui possède un volume réduit de chaînes devant être traduites en deux ou trois langues. Elle est adaptée pour un scénario dans lequel un développeur parlerait plusieurs langues et accepterait de traiter le processus de traduction. Cette approche a l’avantage d’être rapide, de ne requérir aucun outil et de minimiser les risques d’erreurs de traduction. Mais elle n’est pas évolutive. En particulier, les ressources dans les différentes langues peuvent aisément se désynchroniser, ce qui peut entraîner de mauvaises expériences utilisateur et des difficultés de maintenance.
- **Les fichiers de ressources de chaîne sont au format texte XML ou ResJSON, donc pu être remis pour la traduction à l’aide de n’importe quel éditeur de texte. Les versions traduites des fichiers sont copiés puis réintégré dans le projet.** Cette approche présente un risque, car les traducteurs pourraient modifier accidentellement les balises XML. Cependant, elle permet le déroulement de la traduction en dehors du projet Microsoft Visual Studio. Cette approche pourrait bien fonctionner pour les projets devant être traduits vers un nombre réduit de langues. Le format XLIFF est un format XML spécifiquement conçu pour être utilisé en localisation et devrait être bien pris en charge par les prestataires de localisation et les outils de localisation. Vous pouvez utiliser le [Kit de ressources pour application multilingue](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/jj572370.aspx) pour générer des fichiers XLIFF à partir d’autres fichiers de ressources, tels que les fichiers .resw ou .resjson.

> [!NOTE]
> Localisation peut également être nécessaire pour les autres ressources, y compris les images et les fichiers audio.

Vous devez également envisager les éléments suivants :

- **Outils de localisation** un certain nombre d’outils de localisation sont disponible pour l’analyse des fichiers de ressources et n’autorisant que les chaînes traduisibles à être modifié par des traducteurs. Cette approche réduit le risque qu’un traducteur modifie accidentellement les balises XML. Elle présente cependant l’inconvénient d’introduire un nouvel outil et un nouveau processus dans le processus de localisation. Un outil de localisation est approprié pour les projets avec un volume élevé de chaînes, mais un nombre réduit de langues. Pour en savoir plus, voir [Comment utiliser le Kit de ressources pour application multilingue](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/jj572370.aspx).
- **Les fournisseurs de localisation** envisagez d’utiliser un fournisseur de localisation si votre application contient des chaînes étendues qui doivent être traduites en un grand nombre de langues. Un prestataire de localisation peut conseiller des outils et des processus, ainsi que traduire vos fichiers de ressources. Il s’agit d’une solution idéale, mais c’est aussi l’option la plus coûteuse et elle peut augmenter le délai de traitement de votre contenu traduit.

## <a name="keep-access-keys-and-labels-consistent"></a>Utiliser des touches d’accès rapide et des étiquettes cohérentes

La « synchronisation » des touches d’accès rapide utilisées dans les options d’accessibilité avec l’affichage des touches d’accès rapide localisées relève du défi, car les deux ressources de type chaîne sont classées dans deux sections distinctes. Veillez à fournir des commentaires pour la chaîne d’étiquette tels que : `Make sure that the emphasized shortcut key  is synchronized with the access key.`

## <a name="support-furigana-for-japanese-strings-that-can-be-sorted"></a>Prendre en charge les furigana pour les chaînes japonaises qui peuvent être triées

Les caractères japonais kanji possèdent la propriété d’avoir plusieurs lectures (prononciations) selon le mot dans lesquels ils sont utilisés. Cela génère des problèmes lorsque vous tentez de trier des objets nommés en japonais, tels que des noms d’applications, de fichiers, de chansons, etc. Dans le passé, les kanji japonais ont généralement été triés dans un ordre compréhensible par les machines, nommé XJIS. Malheureusement, comme cet ordre de tri n’est pas phonétique, il n’est pas très utile pour les hommes.

Les *furigana* contournent ce problème en permettant à l’utilisateur ou au créateur de spécifier la prononciation des caractères utilisés. Si vous utilisez la procédure suivante pour ajouter des furigana au nom de votre application, vous pouvez garantir qu’elle sera classée à l’emplacement approprié dans la liste des applications. Si le nom de votre application contient des caractères kanji et qu’aucun furigana n’est fourni lorsque la langue de l’interface utilisateur ou l’ordre de tri est défini sur le japonais, Windows s’efforce de générer la prononciation appropriée. Toutefois, il est possible de classer les noms d’applications contenant des prononciations rares ou uniques sous une prononciation plus courante. Ainsi, la meilleure pratique pour les applications japonaises (notamment celles dont le nom contient des caractères kanji) consiste à fournir une version avec furigana de leur nom d’application dans le cadre du processus de localisation japonaise.

1. Ajoutez « ms-resource:Appname » comme nom complet du package et nom complet de l’application.
2. Créez un dossier ja-JP sous le dossier « strings » et ajoutez deux fichiers de ressources comme suit :

    ``` syntax
    strings\
        en-us\
        ja-jp\
            Resources.altform-msft-phonetic.resw
            Resources.resw
    ```

3. Dans Resources.resw pour ja-JP général : Ajouter une ressource de chaîne pour Appname « 希蒼 »
4. Dans Resources.altform-msft-phonetic.resw pour les ressources de furigana japonais : Valoriser furigana pour AppName « のあ »

L’utilisateur peut rechercher le nom d’application « 希蒼 » en utilisant à la fois la valeur de furigana « のあ » (noa) et la valeur phonétique (en utilisant la fonction **GetPhonetic** à partir de l’éditeur de méthode d’entrée (IME)) « まれあお » (mare-ao).

Le classement suit le format de l’application **Région** du Panneau de configuration :

- Sous un paramètre régional utilisateur japonais,
  - Si les furigana sont activés, alors le nom « 希蒼 » est classé sous « の ».
  - En l’absence de furigana, alors le nom « 希蒼 » est classé sous « ま ».
- Sous un paramètre régional d’utilisateur non japonais,
  - Si les furigana sont activés, alors le nom « 希蒼 » est classé sous « の ».
  - En l’absence de furigana, alors le nom « 希蒼 » est classé sous « 漢字 ».

## <a name="related-topics"></a>Rubriques connexes

- [Instructions pour la globalisation](guidelines-and-checklist-for-globalizing-your-app.md)
- [Localiser les chaînes dans votre manifeste de package d’application et de l’interface utilisateur](../../app-resources/localize-strings-ui-manifest.md)
- [Adapter vos ressources de langue, mise à l’échelle, contraste élevé et autres qualificateurs](../../app-resources/tailor-resources-lang-scale-contrast.md)
- [Ajuster la disposition et les polices, et prendre en charge le sens du flux DàG](adjust-layout-and-fonts--and-support-rtl.md)
- [Événements de modification de la mise à jour des images en réponse à la valeur de qualificateur](../../app-resources/images-tailored-for-scale-theme-contrast.md#updating-images-in-response-to-qualifier-value-change-events)

## <a name="samples"></a>Exemples

- [Exemple de localisation et de ressources d’application](https://go.microsoft.com/fwlink/p/?linkid=254478)
