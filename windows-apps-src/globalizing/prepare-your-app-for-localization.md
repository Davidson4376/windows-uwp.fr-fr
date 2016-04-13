---
Description: Préparez votre application en vue de sa localisation pour d’autres marchés, langues ou régions.
title: Préparer votre application en vue de sa localisation
ms.assetid: 06E1D4BB-59EA-4D71-99AC-7CB93D2A58A7
label: Prepare your app for localization
template: detail.hbs
---

# Préparer votre application en vue de sa localisation


\[ Mise à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]


Préparez votre application en vue de sa localisation pour d’autres marchés, langues ou régions. Avant de commencer, veillez à lire l’intégralité des [pratiques conseillées et déconseillées](guidelines-and-checklist-for-globalizing-your-app.md).

## <span id="use_resource_files_and_qualifiers."> </span> <span id="USE_RESOURCE_FILES_AND_QUALIFIERS."> </span>Utilisez des qualificateurs et des fichiers de ressources.


Veillez à spécifier les chaînes d’interface utilisateur de votre application dans des fichiers de ressources au lieu de les placer dans votre code. Pour plus d’informations, voir [Placer des chaînes d’interface utilisateur dans des ressources](put-ui-strings-into-resources.md).

Spécifiez des images ou d’autres ressources de fichiers avec la balise de langue appropriée dans leur fichier ou dossier. Soyez conscient que la localisation d’images, de contenu audio ou de contenu vidéo exige une quantité significative de ressources système, si bien que la solution optimale consiste à utiliser des ressources multimédias neutres chaque fois que vous le pouvez. Pour plus d’informations, voir [Comment nommer des ressources à l’aide de qualificateurs](https://msdn.microsoft.com/library/windows/apps/xaml/hh965324).

## <span id="add_contextual_comments."> </span> <span id="ADD_CONTEXTUAL_COMMENTS."> </span>Ajoutez des commentaires contextuels.


Ajoutez des commentaires de localisation dans les fichiers de ressources de votre application. Ces commentaires sont visibles par le traducteur et doivent fournir des informations contextuelles qui aideront le traducteur à traduire avec précision les ressources. Les commentaires doivent également fournir des informations de contrainte suffisantes sur la ressource de manière à ce que la traduction ne bloque pas le logiciel. Les commentaires peuvent également être enregistrés par l’outil Makepri.exe.

**XAML :** les fichiers Resw (ressources créées dans Visual Studio pour les applications en XAML) intègrent un élément « comment ». Par exemple :

```XAML
<data name="String1">
    <value>Hello World</value>
    <comment>A greeting (This is a comment to the localizer)</comment>
</data>
```

**HTML :** les fichiers Resjson (ressources créées dans Visual Studio pour les applications en HTML) autorisent les métadonnées dans les champs qui commencent par un trait de soulignement, tels que les commentaires :

```json
{
    "String1"  : "Hello World",
    "_String1.comment" : "A greeting (This is a comment to the localizer)"
}
```

## <span id="localize_sentences_instead_of_words."> </span> <span id="LOCALIZE_SENTENCES_INSTEAD_OF_WORDS."> </span>Localisez des phrases et non des mots.


Prenons pour exemple la chaîne suivante : « The {0} could not be synchronized ».

Divers mots pourraient remplacer {0} : rendez-vous, tâche, document, etc. Si cet exemple ne pose aucun problème en anglais, il n’en va pas de même dans tous ces cas pour sa traduction en allemand. Remarquez que dans les phrases allemandes suivantes, certains mots dans la chaîne de modèle (« Der », « Die », « Das ») doivent correspondre au mot paramétré :

| Anglais                                    | Allemand                                           |
|:------------------------------------------ |:------------------------------------------------ |
| The appointment could not be synchronized. | Der Termin konnte nicht synchronisiert werden.   |
| The task could not be synchronized.        | Die Aufgabe konnte nicht synchronisiert werden.  |
| The document could not be synchronized.    | Das Dokument konnte nicht synchronisiert werden. |

 

Autre exemple : la phrase « Me rappeler dans {0} minute(s). » Si « minute(s) » fonctionne en français, cela n’est pas le cas avec d’autres langues. Par exemple, le polonais utilise « minuta », « minuty » ou « minut » selon le contexte.

Pour résoudre ce problème, localisez la phrase entière, plutôt qu’un mot individuel. Cela peut paraître comme une charge de travail supplémentaire et une solution dépourvue d’élégance, mais il s’agit de la solution optimale pour les raisons suivantes :

-   Un message d’erreur propre sera affiché pour toutes les langues.
-   Votre traducteur n’aura pas besoin de vous demander par quoi les chaînes seront remplacées.
-   Vous n’aurez pas besoin d’implémenter un correctif de code onéreux lorsqu’un problème de ce type se révélera une fois votre application terminée.

## <span id="ensure_the_correct_parameter_order."> </span> <span id="ENSURE_THE_CORRECT_PARAMETER_ORDER."> </span>Garantissez l’ordre correct des paramètres.


Ne supposez pas que toutes les langues utilisent les paramètres dans le même ordre. Prenons pour exemple la chaîne « Every %s %s », où le premier %s est remplacé par le nom d’un mois et le second %s par la date dans le mois. Cet exemple ne pose pas de problème en anglais, mais ne fonctionne pas lorsque l’application est localisée en allemand, où la date et le mois doivent être placés dans l’ordre inverse.

Pour résoudre ce problème, remplacez la chaîne par « Every %1 %2 », afin que l’ordre soit interchangeable selon la langue.

## <span id="don_t_over_localize."> </span> <span id="DON_T_OVER_LOCALIZE."> </span>Ne sur-localisez pas.


Localisez les chaînes, mais pas les balises. Prenons les exemples suivants :

| Chaîne sur-localisée                   | Chaîne correctement localisée |
|:--------------------------------------- |:-------------------------- |
| &lt;link&gt;conditions d’utilisation&lt;/link&gt;   | conditions d’utilisation               |
| &lt;link&gt;politique de confidentialité&lt;/link&gt; | politique de confidentialité             |

 

Le fait que la balise précédente &lt;link&gt; soit intégrée aux ressources signifie qu’elle sera aussi localisée. Cela a pour effet de rendre la balise non valide. Seules les chaînes proprement dites doivent être localisées. En règle générale, vous devriez considérer les balises comme du code qui doit être maintenu à part du contenu localisable. Toutefois, certaines chaînes longues doivent inclure un balisage pour conserver le contexte et garantir l’ordre correct.

## <span id="do_not_use_the_same_strings_in_dissimilar_contexts."> </span> <span id="DO_NOT_USE_THE_SAME_STRINGS_IN_DISSIMILAR_CONTEXTS."> </span>N’utilisez pas de chaînes identiques dans des contextes dissemblables.


La réutilisation d’une chaîne peut sembler être la meilleure solution, mais cela peut causer des problèmes de localisation si un mot ou une expression ont des significations ou des contextes différents.

Vous pouvez réutiliser des chaînes si les deux contextes sont identiques. Par exemple, vous pouvez réutiliser la chaîne « Volume » pour le volume des effets sonores et le volume de la musique, car les deux font référence à l’intensité sonore. Vous ne devez pas réutiliser la même chaîne pour faire référence au volume du disque dur, car le contexte et la signification sont différents, et le mot peut être traduit différemment.

L’utilisation des chaînes « on » et « off » en est un autre exemple. En anglais, « on » et « off » peuvent être utilisés pour activer et désactiver le mode Avion, la fonctionnalité Bluetooth et des appareils. En revanche, en italien, la traduction dépend du contexte de ce qui est activé et désactivé. Vous devez créer une paire de chaînes pour chaque contexte.

De plus, une chaîne comme « text » ou « fax » peut être utilisée en tant que verbe ou substantif en anglais, ce qui peut compliquer le processus de traduction. Créez plutôt deux chaînes distinctes pour la forme verbale et la forme nominale. Lorsque vous n’êtes pas certain que les contextes sont les mêmes, ne prenez pas de risque et utilisez deux chaînes distinctes.

## <span id="identify_resources_with_unique_attributes."> </span> <span id="IDENTIFY_RESOURCES_WITH_UNIQUE_ATTRIBUTES."> </span>Identifiez les ressources à l’aide d’attributs uniques.


Les identificateurs de ressources ne respectent pas la casse et doivent être uniques pour chaque fichier de ressources. Lorsque vous accédez à une ressource, utilisez l’identificateur de ressource et non pas la valeur réelle de la ressource. Les identificateurs de ressources ne changent pas, mais les valeurs réelles des ressources changent selon la langue.

Veillez à utiliser des identificateurs de ressources significatifs pour fournir un contexte supplémentaire pour la traduction.

Ne modifiez pas les identificateurs de ressources après l’envoi en traduction des ressources de type chaîne. Les équipes de localisation utilisent l’identificateur de ressource pour effectuer le suivi des ajouts, des suppressions et des mises à jour dans les ressources. Les modifications apportées dans les identificateurs de ressources (appelées également « transition des identificateurs de ressources ») nécessitent la retraduction des chaînes, car il semblera que des chaînes auront été supprimées et d’autres ajoutées.

## <span id="choose_an_appropriate_translation_approach."> </span> <span id="CHOOSE_AN_APPROPRIATE_TRANSLATION_APPROACH."> </span>Choisissez une approche de traduction appropriée.


Une fois que les chaînes ont été séparées en fichiers de ressources, elles peuvent être traduites. Le moment idéal pour traduire des chaînes est après la finalisation de ces chaînes dans le projet, laquelle survient habituellement vers la fin d’un projet. Vous pouvez adopter des approches différentes du processus de traduction. Cela peut dépendre du volume des chaînes à traduire, le nombre de langues cibles et la manière dont la traduction sera réalisée (par exemple, en interne ou en faisant appel à un prestataire externe).

Considérez les options suivantes :

-   **Les fichiers de ressources peuvent être traduits en étant ouverts directement dans le projet.** Cette approche fonctionne bien pour un projet qui possède un volume réduit de chaînes et qui a besoin d’être traduit en deux ou trois langues. Elle est adaptée pour un scénario dans lequel un développeur parlerait plusieurs langues et accepterait de traiter le processus de traduction. Cette approche a l’avantage d’être rapide, de ne requérir aucun outil et de minimiser les risques d’erreurs de traduction, mais elle n’est pas évolutive. En particulier, les ressources dans les différentes langues peuvent aisément se désynchroniser, ce qui peut entraîner de mauvaises expériences utilisateur et des difficultés de maintenance.
-   **Les fichiers de ressources de type chaîne sont au format texte XML ou ResJSON et peuvent être confiés pour traduction à l’aide d’un éditeur de texte. Les fichiers traduits pourront alors être copiés en retour dans le projet.** Cette approche présente un risque, car les traducteurs pourraient modifier accidentellement les balises XML. Cependant, elle permet le déroulement de la traduction en dehors du projet Microsoft Visual Studio. Cette approche pourrait bien fonctionner pour les projets devant être traduits vers un nombre réduit de langues. Le format XLIFF est un format XML spécifiquement conçu pour être utilisé en localisation et devrait être bien pris en charge par les prestataires de localisation et les outils de localisation. Vous pouvez utiliser le [Kit de ressources pour application multilingue](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/jj572370.aspx) pour générer des fichiers XLIFF à partir d’autres fichiers de ressources, tels que les fichiers .resw ou .resjson.

Un transfert vers des traducteurs peut être nécessaire pour d’autres fichiers, tels que des images ou des fichiers audio. En général, nous déconseillons de créer des fichiers dépendant de la culture, car ils peuvent s’avérer difficiles à localiser.

De plus, considérez les suggestions suivantes :

-   **Utilisez un outil de localisation.** De nombreux outils de localisation sont disponibles pour analyser les fichiers de ressources et permettre la modification par les traducteurs des seules chaînes traduisibles. Cette approche réduit le risque qu’un traducteur modifie accidentellement les balises XML. Elle présente cependant l’inconvénient d’introduire un nouvel outil et un nouveau processus dans le processus de localisation. Un outil de localisation est approprié pour les projets avec un volume élevé de chaînes, mais un nombre réduit de langues. Pour en savoir plus, voir [Comment utiliser le Kit de ressources pour application multilingue](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/jj572370.aspx).
-   **Faites appel à un prestataire de localisation.** Considérez l’utilisation d’un prestataire de localisation si votre projet contient un volume élevé de chaînes et doit être traduit en de nombreuses langues. Un prestataire de localisation peut conseiller des outils et des processus, ainsi que traduire vos fichiers de ressources. Il s’agit d’une solution idéale, mais c’est aussi l’option la plus coûteuse et elle peut augmenter le délai de traitement de votre contenu traduit.
-   **Maintenez vos traducteurs informés.** Informez les traducteurs sur les chaînes pouvant être considérées comme un substantif ou un verbe. Expliquez les mots fabriqués aux traducteurs à l’aide d’outils terminologiques. Conservez des chaînes grammaticalement correctes, non ambiguës et aussi peu techniques que possible pour éviter les confusions.

## <span id="keep_access_keys_and_labels_consistent."> </span> <span id="KEEP_ACCESS_KEYS_AND_LABELS_CONSISTENT."> </span>Utilisez des touches d’accès rapide et des étiquettes cohérentes.


La « synchronisation » des touches d’accès rapide utilisées dans les options d’accessibilité avec l’affichage des touches d’accès rapide localisées relève du défi, car les deux ressources de type chaîne sont classées dans deux sections distinctes. Ajoutez des commentaires pour la chaîne d’étiquette, par exemple : `Make sure that the emphasized shortcut key  is synchronized with the access key.`

**HTML :**

Vous pouvez suivre l’implémentation illustrée ci-dessous. Là encore, n’oubliez pas de commenter correctement la chaîne d’étiquette pour l’associer à la définition de touche d’accès rapide.

```HTML
<label id="theLabel" data-win-res="{accessKey: 'theLabelAccessKey'}" for="xPrinterRedirection" accessKey="L">The <u>L</u>abel</label>
<input type="checkbox" value="OFF" id="xPrinterRedirection" name="xPrinterRedirection" />
```

## <span id="support_furigana_for_japanese_strings_that_can_be_sorted."> </span> <span id="SUPPORT_FURIGANA_FOR_JAPANESE_STRINGS_THAT_CAN_BE_SORTED."> </span>Prenez en charge les furigana pour les chaînes japonaises qui peuvent être triées.


Les caractères japonais kanji possèdent la propriété unique d’avoir plusieurs prononciations selon le mot et le contexte dans lesquels ils sont utilisés. Cela génère des problèmes lorsque vous tentez de trier des objets nommés en japonais, tels que des noms d’applications, de fichiers, de chansons, etc. Dans le passé, les kanji japonais ont généralement été triés dans un ordre compréhensible par les machines, nommé XJIS. Malheureusement, comme cet ordre de tri n’est pas phonétique, il n’est pas très utile pour les hommes.

Les furigana contournent ce problème en permettant à l’utilisateur ou au créateur de spécifier la prononciation des caractères utilisés. Si vous utilisez la procédure suivante pour ajouter des furigana au nom de votre application, vous pouvez garantir qu’elle sera classée à l’emplacement approprié dans la liste des applications. Si le nom de votre application contient des caractères kanji et qu’aucun furigana n’est fourni lorsque la langue de l’interface utilisateur ou l’ordre de tri est défini sur le japonais, Windows s’efforce pour générer la prononciation appropriée. Toutefois, il est possible de classer les noms d’applications contenant des prononciations rares ou uniques sous une prononciation plus courante. Ainsi, la meilleure pratique pour les applications japonaises (notamment celles contenant des caractères kanji dans leur nom) consiste à fournir une version avec furigana de leur nom d’application dans le cadre du processus de localisation japonaise.

1.  Ajoutez « ms-resource:Appname » comme nom complet du package et nom complet de l’application.
2.  Créez un dossier ja-JP sous le dossier « strings » et ajoutez deux fichiers de ressources comme suit :

    ``` syntax
    strings\
        en-us\
        ja-jp\
            Resources.altform-msft-phonetic.resw
            Resources.resw
    ```

3.  Dans Resources.resw pour le dossier général ja-JP : ajoutez une ressource de type chaîne pour le nom d’application « 希蒼 ».
4.  Dans Resources.altform-msft-phonetic.resw pour les ressources japonaises avec furigana : ajoutez la valeur de furigana pour le nom d’application « のあ ».

L’utilisateur peut rechercher le nom d’application « 希蒼 » en utilisant à la fois la valeur de furigana « のあ » (noa) et la valeur phonétique (en utilisant la fonction **GetPhonetic** à partir de l’éditeur de méthode d’entrée (IME)) « まれあお » (mare-ao).

Le classement suit le format de l’application **Région** du Panneau de configuration :

-   Sous les paramètres régionaux d’utilisateur japonais,
    -   Si les furigana sont activés, le nom « 希蒼 » est classé sous « の ».
    -   En l’absence de furigana, le nom « 希蒼 » est classé sous « ま ».
-   Sous les paramètres régionaux d’utilisateur non japonais,
    -   Si les furigana sont activés, le nom « 希蒼 » est classé sous « の ».
    -   En l’absence de furigana, le nom « 希蒼 » est classé sous « 漢字 ».

## <span id="related_topics"> </span>Rubriques connexes


* [Pratiques conseillées et déconseillées en matière de globalisation et de localisation](guidelines-and-checklist-for-globalizing-your-app.md)
* [Placer des chaînes d’interface utilisateur dans des ressources](put-ui-strings-into-resources.md)
* [Comment nommer des ressources à l’aide de qualificateurs](https://msdn.microsoft.com/library/windows/apps/xaml/hh965324)
 

 





<!--HONumber=Mar16_HO4-->


