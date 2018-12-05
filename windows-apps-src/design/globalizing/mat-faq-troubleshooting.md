---
Description: This topic provides answers to frequently-asked questions and issues related to the Multilingual App Toolkit (MAT) 4.0.
title: FAQ et résolution des problèmes du kit de ressources MultilingualAppToolkit
template: detail.hbs
ms.date: 11/13/2017
ms.topic: article
keywords: windows10, uwp, globalisation, adaptabilité, localisation
ms.localizationpriority: medium
ms.openlocfilehash: a39d2b3133714ab784309e131a71219beae4e3c0
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/05/2018
ms.locfileid: "8733494"
---
# <a name="multilingual-app-toolkit-40-faq--troubleshooting"></a>FAQ et résolution des problèmes du kit de ressources MultilingualAppToolkit4.0

Cette rubrique présente des réponses aux questions fréquentes et aux problèmes relatives au kit de ressources MultilingualAppToolkit4.0.

Consultez également [Utiliser le kit de ressources MultilingualAppToolkit4.0](use-mat.md).

**Remarque** Le kit de ressources prend en charge à la fois les fichiers .resw (XAML) et .resjson (JavaScript). Mais cette rubrique traitement uniquement des fichiers .resw. Les fichiers .resw sont également appelés fichiers Ressources. Il contient des chaînes, qu'elles soient dans la langue par défaut ou traduites vers une autre langue. Le dossier contenant un fichier .resw est généralement nommé pour la valeur d’une balise de langue.

## <a name="do-i-need-resw-files-in-multiple-languages"></a>Ai-je besoin de fichiers .resw dans plusieurs langues?

Non. Il n'est pas nécessaire de disposer de fichiers .resw dans diverses langues. C'est l'un des avantages du kit de ressources. Le kit de ressource gère et synchronise les ressources de votre application à l'aide de fichiers .xlf. Ainsi, les défis associés à la préservation de la synchronicité d'un contenu parmi plusieurs fichiers .resw disparaissent.

À cause des projets contenant des fichiers .resw et .xlf, les traductions provenant de fichiers .xlf sont ignorées. Dans ce cas, un avertissement est affiché pendant la génération et vous informe que les traductions .xlf ne sont pas incluses dans l'application finale. Les fichiers .resw et .xlf correspondent lorsqu'ils partage une langue cible ayant le même code de langue. La paire de fichiers `Strings\de-DE\Resources.resw` et `<project-name>.de-DE.xlf` (contenant `target-language="de-DE"`) est un bon exemple.

## <a name="can-i-have-resw-files-in-multiple-languages"></a>Puis-je utiliser des fichiers .resw dans plusieurs langues?

Vous le pouvez, mais nous ne le recommandons pas. Si vous souhaitez inclure les fichiers .resw dans plusieurs langues dans votre projet et utiliser le kit de ressources, assurez-vous de ne pas avoir de fichiers .resw et .xlf correspondants.

## <a name="i-dont-see-an-option-in-the-tools-menu-to-enable-the-multilingual-app-toolkit"></a>Dans le menu Outils, je ne vois aucune option permettant d'activer le kit de ressources MultilingualAppToolkit

Essayez les étapes suivantes.

- Avant d'ouvrir le menu **Outils**, veillez à sélectionner le nœud de projet, et non le nœud solution.
- Vérifiez que l'extension du kit de ressources est installé à l'aide du manager d'extension VisualStudio.
- Vérifiez que votre projet est un projet UWP.

## <a name="when-i-build-my-project-i-dont-see-a-message-saying-that-a-multilingual-app-toolkit-build-has-started"></a>Lorsque je génère mon projet, je n'obtiens aucun message stipulant que la génération du kit de ressources MultilingualAppToolkit a démarrée

Vérifiez que vous avez activé le kit MAT pour votre projet. Dans le menu **Outils**, sélectionnez **Multilingual App Toolkit** > **Activer la sélection**. Si votre projet a été activé avec une version précédente, désactivez et réactivez le MAT à l'aide du menu **Outils**. Ainsi, le projet sera mis à jour et fonctionnera avec la nouvelle version du kit de ressources.

Veillez à ce que le composant «Générer une tâche pour toutes les éditions VisualStudio» soit installé. Ce composant de génération est installé avec l'extension, mais il peut être désélectionné manuellement pendant l'installation. Ce composant est obligatoire pour la mise à jour des fichiers .xlf et l'ajout de la traduction dans le fichier PRI. Lorsque ce composant est installé et fonctionne correctement, ces messages de génération apparaissent.

```dosbatch
1> Multilingual App Toolkit build started.
1> Multilingual App Toolkit build completed successfully.
```

## <a name="the-toolkit-is-reporting-that-it-didnt-locate-any-xliff-language-files-during-the-build"></a>Le kit de ressources signale qu'il ne parvient pas à localiser les fichiers linguistiques XLIFF pendant la génération

```dosbatch
No XLIFF language files were found. The app will not contain any localized resources.
```

Ce message s'affiche lorsque le kit de ressources ne trouve aucun fichier ayant l'extension .xlf dans le projet. Le kit de ressources génère et conserve ces fichiers dans le dossier `MultilingualResources` par défaut. Ils peuvent être déplacé, mais il est préférable de les laisser dans ce dossier. Cela permet à MultilingualEditor de localiser les fichiers de métadonnées connexes.

## <a name="my-xlf-file-is-not-included-in-the-list-of-files-processed-by-the-toolkit-during-build"></a>Mon fichier .xlf ne figure pas dans la liste des fichiers traités par le kit de ressources pendant la génération

Si vous excluez manuellement un certain fichier .xlf du projet pour ensuite le réinclure, il est probable que le type de fichier ne soit pas défini correctement. Dans VisualStudio, sélectionnez le fichier et examinez la fenêtre Propriétés. L'action Générer du fichier doit être définie sur XliffResource et Copier vers le répertoire de sortie doit être défini sur Ne pas copier. C'est ainsi que la référence doit se présenter dans votre fichier de projet.

```xml
<XliffResource Include="MultilingualResources\<project-name>.fr-FR.xlf" />
```

## <a name="ive-added-xlf-based-languages-where-are-my-strings"></a>J’ai ajouté des langues basées sur .xlf. Où sont mes chaînes?

Votre fichier Ressources (.resw) de langue par défaut constitue le «schéma» canonique des chaînes utilisées par votre application. Le fichiers de ressources traduites peut contenir toutes les chaînes ou un sous-ensemble de ces chaînes.

Lorsque vous générez votre projet, vos fichiers Ressources et vos fichiers .xlf sont synchronisés.

- Les fichiers .xlf mis à jour afin d'intégrer toute chaîne ajoutée ou supprimée, ou tout fichier Ressources ajouté ou supprimé.
- Les fichiers Ressources sont mis à jour pour intégrer toutes les chaînes traduites dans les fichiers .xlf.

Ce processus est expliqué en détail dans [Utiliser le kit de ressources MultilingualAppToolkit4.0](use-mat.md).

## <a name="when-i-build-my-project-the-xlf-files-remain-empty"></a>Lorsque je génère mon projet, les fichiers .xlf restent vides

Pour pouvoir utiliser MAT efficacement, votre application doit être localisable. Ce processus est expliqué en détail dans [Utiliser le kit de ressources MultilingualAppToolkit4.0](use-mat.md).

## <a name="what-is-microsoft-translator"></a>Qu’est-ce que MicrosoftTranslator?

MicrosoftTranslator est un service cloud qui fournit des traductions automatiques. La traduction automatique est une parfaite solution d'accès à la traduction lorsque la traduction humaine n'est pas envisageable. Pour en savoir plus, consultez la page [MicrosoftTranslator](http://go.microsoft.com/fwlink/p/?LinkId=258220).

Le kit de ressources utilise le service MicrosoftTranslator dans le but de vous fournir des suggestions de traduction. Vous pouvez découvrir les langues prises en charge par MicrosoftTranslator lorsque l'icône MicrosoftTranslator est affiché dans la boîte de dialogue Langues de traduction.

Vous pouvez traduire rapidement votre application à l'aide de MicrosoftTranslator, à partir de MultilingualEditor, en sélectionnant une chaîne et en cliquant sur **Traduire**.

## <a name="what-is-pseudo-language-and-what-are-pseudo-resource-trackers"></a>Qu'est-ce que la pseudo-langue et en quoi consistent les outils de suivi des pseudo-ressources?

La pseudo-langue est une modification artificielle du produit de logiciel. Elle vise à simuler une véritable localisation de langue. Vous trouverez plus de détails concernant la pseudo-langue et les outils de suivi des pseudo-ressources dans [Utiliser le kit de ressources MultilingualAppToolkit4.0](use-mat.md).

## <a name="how-do-i-set-my-language-preference-to-pseudo-language-so-that-i-can-test-my-pseudo-locd-strings"></a>Comment puis-je définir les préférences de langue pour la pseudo-langue afin de pouvoir tester mes chaînes pseudo-localisées?

Ce processus est expliqué dans [Utiliser le kit de ressources MultilingualAppToolkit4.0](use-mat.md).

## <a name="what-kind-of-localizability-issues-can-i-find-using-pseudo-language"></a>Quels types de problèmes d'adaptabilité pourrais-je rencontrer en utilisant les pseudo-langues?

Ce processus est expliqué dans [Utiliser le kit de ressources MultilingualAppToolkit4.0](use-mat.md).

## <a name="im-not-seeing-any-translations-when-i-launch-my-app-or-my-app-is-only-partially-translated"></a>Lorsque je lance mon application, je ne vois aucun traductions ou mon application est partiellement traduite

Ouvrez le fichier .xlf dans MultilingualEditor pour vérifier la présence des traductions. Lorsque les chaînes du fichier .resw en langue par défaut sont explicitement modifiées, toutes les traductions correspondantes sont supprimées des fichiers .xlf. Ainsi, la correspondance de la traduction avec la chaîne source est garantie. Traduisez les chaînes dans les fichiers .xlf et relancez la génération pour mettre à jour les fichiers .resw en langue autre que la langue par défaut.

Si vos chaînes sont traduites dans les fichiers .xlf sans figurer dans votre application, générez votre projet à nouveau pour mettre à jour les fichiers .resw en langue autre que la langue par défaut. VisualStudio optimise la commande Générer pour générer uniquement les fichiers ayant été modifiés depuis la dernière génération.

Vérifiez votre ordre de préférence de langue. Dans les **Paramètres**, vérifiez que la langue que vous souhaitez tester figure en haut de votre liste de préférence de langue.

## <a name="the-toolkit-is-reporting-error--0x80004004-in-the-build-output"></a>Dans le résultat de génération, le kit de ressources signale l'erreur 0x80004004

```dosbatch
Merge of Loc PRI file failed calling makepri.exe: "0x80004004"
```

Ce message peut s'afficher lorsqu'un format de région entre en conflit avec l'opération de génération du kit de ressources. Une solution de contournement existe: lors de la génération, il suffit de modifier la langue dans les **Paramètres** pour la langue en-US.


## <a name="the-toolkit-is-reporting-error--0x80004005-in-the-build-output"></a>Dans le résultat de génération, le kit de ressources signale l'erreur 0x80004005

```dosbatch
Merge of Loc PRI file failed calling makepri.exe: "0x80004005"
```

Ce message peut s'afficher lorsque le fichier .xlf contient une langue cible non prise en charge. Par exemple, «zh-cht» est incorrect (basculez vers «zh-hant»), et «zh-chs» est incorrect (basculez vers «zh-hans»).

## <a name="is-there-a-way-to-find-out-more-information-about-the-errors-im-seeing"></a>Existe-t-il un moyen d'obtenir plus d'informations sur les erreurs qui s'affichent?

Oui. Vous pouvez activer d'activer la journalisation des descriptions plus détaillée dans VisualStudio. Cliquez sur **Outils** > **Options** > **Projets et solutions** > **Générer et exécuter**. Modifiez la valeur de **Niveau de détail du résultat de la génération de projet MSBuild**: de Minimum à Normal ou Élevé.

L'exécution de MSBuild depuis la ligne de commande peut également provoquer des messages supplémentaires.

```dosbatch
msbuild /t:rebuild <project-name>
```

## <a name="import-translation-failed"></a>Échec de l'importation de la traduction

Le processus d'importation réalise une validation de base préalable à l'importation. Ainsi, les informations culturelles de la cible dans les fichiers en cours d'importation correspondent aux fichiers .xlf existants. Dans MultilingualEditor, ouvrez les fichiers .xlf et veillez à ce que les informations culturelles correspondent.

## <a name="what-if-my-translator-doesnt-have-windows-10-andor-visual-studio-andor-the-multilingual-app-toolkit-installed"></a>Et si mon traducteur n'a pas installé Windows10 et/ou Visual Studio et/ou le kit de ressources MultilingualAppToolkit ?

Lorsque vous sélectionnez **Résultat: destinataire par e-mail** dans la boîte de dialogue Exporter la chaîne, l'e-mail comprend un lien de téléchargement et d'installation du kit de ressources MultilingualAppToolkit(MAT)4.0. Votre traducteur est toujours en mesure d'installer l'outil MAT 4.0 autonome MultilingualEditor, même sans Windows10 ou VisualStudio.

Pour plus d'informations, consultez [Utiliser le kit de ressources MultilingualAppToolkit4.0](use-mat.md).

## <a name="what-happened-to-the-markuprulesxml-and-resourceslocksxml-files"></a>Qu'est-il arrivé aux fichiers `MarkupRules.xml` et `ResourcesLocks.xml`?

Le kit de ressources MultilingualAppToolkit4.0 n'utilise pas les fichiers de verrouillage de ressource propriétaires. Au lieu de cela, la balise XLIFF1.2 `<mrk>` est directement ajoutée au fichier .xlf pour identifier les chaînes qui ne sont pas modifiées pendant la traduction automatique. Ainsi, le fichier XLIFF est autonome et le verrouillage des ressources par fichier est possible.

Ces fichiers de support supplémentaires ne sont plus nécessaires. Si vous en disposez, vous pouvez les supprimer en toute sécurité.

## <a name="what-happened-to-the-tpx-file"></a>Qu'est-il arrivé au fichier .tpx?

Le fichier .tpx fourni un moyen simple d'inclure les fichiers `MarkupRules.xml` et `ResourcesLocks.xml` lorsque le fichier .xlf a été envoyé pour la traduction. Cette fonctionnalité n’est plus nécessaire.

Si vous souhaitez récupérer certaines fichiers .tpx, il suffit de renommer l'extension de fichier .tpx avec l'extension .zip. Ainsi, vous êtes en mesure d'ouvrir et d'extraire les contenus avec l'Explorateur de fichier ou tout autre outil .zip compatible.

## <a name="i-think-ive-done-everything-right-but-it-still-isnt-working"></a>Je pense avoir suivi toutes les procédures à la lettre, pourtant ça ne fonctionne toujours pas

Essayez les étapes suivantes.

1. Ajoutez des traductions à l'aide d'une des méthodes déjà décrites.
2. Videz le fichier .pri (consultez [Options de ligne de commande MakePri.exe](../../app-resources/makepri-exe-command-options.md)) pour découvrir si vos traductions se trouvent dans le fichier .pri. Les traductions apparaissent avec le code de langue et la valeur traduite, comme ceci.
   ```xml
   <Candidate qualifiers="Language-QPS-PLOC" type="String">
       <Value>[!!_Ŝéãřćĥ_!!]</Value>
   </Candidate>
   ```
3. Générez à partir d'une invite de commande; l'erreur obtenue peut contenir plus de détails que ce qui est signalé dans le résultat de génération.

## <a name="my-app-failed-certification-to-the-microsoft-store"></a>Mon application a échoué dans la certification du Microsoft Store

Avant de commencer le processus de certification du MicrosoftStore, vous devez exclure le fichier `<project-name>.qps-ploc.xlf` de votre projet. La pseudo-langue est utilisée pour détecter les potentiels problèmes ou bogues d'adaptabilité, mais elle n'est pas valide en tant que langue du MicrosoftStore. Si elle est pas supprimée, votre application échouera pendant le processus de certification du MicrosoftStore.

## <a name="related-topics"></a>Rubriquesassociées

* [Utiliser le kit de ressources MultilingualAppToolkit4.0](use-mat.md)
* [Microsoft Translator](http://go.microsoft.com/fwlink/p/?LinkId=258220)
* [Options de ligne de commande MakePri.exe](../../app-resources/makepri-exe-command-options.md)
