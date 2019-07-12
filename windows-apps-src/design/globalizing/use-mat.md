---
Description: Le Multilingual App Toolkit (MAT) 4.0 s’intègre à Microsoft Visual Studio 2019 pour fournir des applications UWP avec la prise en charge, la gestion de fichier de traduction et outils d’édition.
title: Utiliser le kit de ressources Multilingual App Toolkit
template: detail.hbs
ms.date: 01/23/2018
ms.topic: article
keywords: windows 10, uwp, globalisation, adaptabilité, localisation
ms.localizationpriority: medium
ms.openlocfilehash: 428238f9d8a3468ab308841850ac13e8da22961f
ms.sourcegitcommit: 51d884c3646ba3595c016e95bbfedb7ecd668a88
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67820583"
---
# <a name="use-the-multilingual-app-toolkit-40"></a>Utiliser le Kit de ressources Multilingual App Toolkit 4.0

Le Multilingual App Toolkit (MAT) 4.0 s’intègre à Microsoft Visual Studio 2019 pour fournir des applications UWP avec la prise en charge, la gestion de fichier de traduction et outils d’édition. Voici quelques-unes des propositions de valeur du kit de ressources.

- Assistance dans la gestion des modifications de ressources et d'état de la traduction lors du développement.
- Fourniture d'une interface utilisateur pour le choix des langues en fonction des fournisseurs de traduction configurés.
- Prise en charge du format de fichier XLIFF de localisation standard au secteur industriel
- Fourniture d'un moteur de pseudo-langue permettant d'aider à identifier les problèmes de traduction lors du développement.
- Connexion avec le portail linguistique Microsoft pour un accès aisé aux chaînes traduite et à la terminologie.
- Connexion avec Microsoft Translator pour des suggestions de traduction rapides.

## <a name="how-to-use-the-toolkit"></a>Utilisation du kit de ressources

### <a name="step-1-design-your-app-for-globalization-and-localization"></a>Étape 1. Concevoir votre application pour la globalisation et la localisation

Pour pouvoir utiliser MAT efficacement, votre application doit être localisable. Plus précisément, votre projet doit contenir au moins un fichier Ressource (.resw) contenant les chaînes de votre application dans la langue par défaut. Pour plus de détails, consultez [Localiser les chaînes dans l’interface utilisateur et le manifeste du package d’application](../../app-resources/localize-strings-ui-manifest.md). Cela étant, le kit de ressources accélère et facilite l'ajout de langues supplémentaires.

Pour la proposition de valeur de globalisation et de localisation&mdash;, ainsi que les définitions des termes **globalisation**, **adaptabilité**, et **localisation**&mdash;consultez [Globalisation et localisation](globalizing-portal.md).

Consultez également [Directives en matière de globalisation](guidelines-and-checklist-for-globalizing-your-app.md) et [Rendre votre application localisable](prepare-your-app-for-localization.md).

### <a name="step-2-download-and-install-the-multilingual-app-toolkit-40"></a>Étape 2. Télécharger et installer le kit de ressources Multilingual App Toolkit 4.0

Le kit de ressources Multilingual App Toolkit 4.0 (MAT 4.0) est divisé en deux parties, chacune équipée de son propre installateur.

- [Multilingual App Toolkit 4.0 Extension pour Visual Studio 2017 et versions ultérieures](https://marketplace.visualstudio.com/items?itemName=MultilingualAppToolkit.MultilingualAppToolkit-18308). Il contient l’extension de fond 4.0 pour 2019 a Visual Studio, sous la forme d’un programme d’installation .vsix.
- [Éditeur Multilingual App Toolkit 4.0](https://developer.microsoft.com/en-us/windows/develop/multilingual-app-toolkit). Cette partie contient l'outil Multilingual Editor autonome pour MAT 4.0 sous la forme d'un installateur .msi. Il comprend également l'extension MAT 4.0 pour Visual Studio 2015 et pour Visual Studio 2013.

Si vous utilisez Visual Studio 2017 ou Visual Studio 2019, puis téléchargez et exécutez les deux programmes d’installation, une après l’autre. Si vous utilisez Visual Studio 2015 ou Visual Studio 2013, téléchargez et exécutez l'installateur .msi.

### <a name="step-3-enable-the-multilingual-app-toolkit-for-your-project"></a>Étape 3. Activer le kit de ressources Multilingual App Toolkit pour votre projet

Avant que vous ne puissiez commencer à localiser l'application, le MAT doit être activé pour votre projet. Voici comment activer le kit de ressources.

- Ouvrez la solution de projet dans Visual Studio.
- Sélectionnez le projet souhaité dans l'Explorateur de solutions.
- Dans le menu **Outils**, sélectionnez **Multilingual App Toolkit** > **Activer la sélection**. 

Dans la fenêtre Sortie (affichant la sortie du kit de ressources Multilingual App Toolkit), recherchez le message `Project '<project-name>' was enabled. The project's source culture is '<language-tag>' <language-name>`. Si ce message apparaît, le MAT est prêt à être utilisé.

### <a name="step-4-add-languages-to-your-project"></a>Étape 4. Ajouter des langues à votre projet

Suivez ces étapes pour ajouter des langues à votre projet.

1. Dans l’Explorateur de solutions, cliquez avec le bouton droit sur le projet.
2. Cliquez sur **Multilingual App Toolkit** > **Ajouter des langues de traduction...** .
3. Dans la boîte de dialogue Langues de traduction, sélectionnez les langues que vous souhaitez prendre en charge, puis cliquez sur OK.

Voici les actions par lesquelles le kit de ressources répond.

- Pour chaque langue que vous ajoutez, un nouveau dossier est créé. Il est nommé d'après la [balise de langue BCP-47](https://go.microsoft.com/fwlink/p/?linkid=227302) de la langue. Dans ce dossier, les nouveaux fichiers Ressources (.resw) sont créés pour correspondre aux fichiers contenant les chaînes en langue par défaut.
- Si c’est la première fois que vous ajoutez une langue, un nouveau dossier nommé `MultilingualResources` est ajouté au projet. Dans ce dossier, un fichier .xlf est ajouté pour chaque langue. Les fichiers .xlf contiennent une unité de traduction pour chaque chaîne dans chaque fichier Ressources (.resw) de votre projet.
- La fenêtre Sortie confirme votre ajout des langues.

Chaque fois que vous ajoutez/supprimer un fichier Ressources (.resw) de langue par défaut, ou chaque fois que ajoutez/supprimez une chaîne dans un fichier Ressources (.resw) de langue par défaut, générez à nouveau le projet pour synchroniser à nouveau les fichiers .xlf. Ainsi, les fichiers .xlf contiennent l'association des chaînes dans la langue par défaut.

Les fournisseurs de traduction installés&mdash;tels que le [Portail linguistique Microsoft](https://go.microsoft.com/fwlink/p/?LinkId=330295) et [Microsoft Translator](https://go.microsoft.com/fwlink/p/?LinkId=258220)&mdash;peuvent être utilisés pour traduire les ressources de votre application. Lorsqu'un fournisseur prend en charge une langue spécifique, l'icône du fournisseur s'affiche en regard du nom de la langue dans la boîte de dialogue Langues de traduction.

Dans la boîte de dialogue Langues de traduction, les cases de toutes les langues basées sur .xlf découvertes par le kit de ressources sont cochées au préalable pour indique que la langue est déjà incluse dans le projet.

Une fois la langue ajoutée au projet, elle ne peut pas être supprimée en cochant la case dans la boîte de dialogue Langues de traduction. Pour supprimer une langue, cliquez avec le bouton droit sur le fichier .xlf spécifique à la langue et sélectionnez **Supprimer**. La confirmation a également pour effet de supprimer les fichiers Ressources (.resw) correspondants.

### <a name="step-5-test-your-app-using-pseudo-language"></a>Étape 5. Tester votre application à l’aide d'une pseudo-langue

La pseudo-langue est une modification artificielle du produit de logiciel. Elle vise à simuler une véritable localisation de langue, mais elle reste lisible pour les locuteurs natifs. La pseudo-traduction remplace les caractères et rallonge la chaîne de ressource pour détecter tout problème ou bogue d'adaptabilité potentiel aux prémices du cycle du projet et avant le démarrage de la mise à l'épreuve de la localisation.

Suivez ces étapes pour pseudo-localiser et tester votre projet.

1. Utilisez la boîte de dialogue Langues de traduction pour ajouter une pseudo-langue (Pseudo) [qps-ploc] à votre projet.
2. Dans l'Explorateur de fichiers, cliquez avec le bouton droit sur `<project-name>.qps-ploc.xlf`, puis cliquez sur **Multilingual App Toolkit** > **Générer les traductions automatiques**.
3. Dans **Paramètres** > **Heure et langue** > **Région et langue** > **Langues**, cliquez sur **Ajouter une langue**.
5. Dans la zone de recherche, saisissez `qps-ploc`.
6. Cliquez sur `English (qps-ploc)` pour l'ajouter.
7. Dans la liste des langues, sélectionnez `English (qps-ploc)` et cliquez sur **Définir par défaut**.
8. Testez votre application pseudo-localisée. Par exemple, cherchez les problèmes de disposition de l'interface utilisateur dans lesquels une partie de la chaîne ne s'affiche pas (la chaîne est tronquée) ou dans lesquels certaines chaînes ne sont pas traduites (mais plutôt codées en dur).

Outre le remplacement de caractère et l'expansion, le pseudo-moteur fournit un identificateur de suivi unique pour chaque ressource. Cet indicateur de suivi précède le démarrage de chaque chaîne et est exprimé entre crochets : `[xxxxx]`. Vous pouvez utiliser ces indicateurs de suivi pendant la phase de test d'inspection de l'interface utilisateur visuelle. Ils peuvent aider à superviser des ressources spécifiques dans le produit, en particulier si plusieurs ressources ont un texte similaire ou dupliqué.

Dans l'exemple textuel « Hello, World! » la pseudo-traduction rallonge la chaîne pour qu'elle occupe près de 30 pour cent de plus sur l'écran, puis elle applique le suivi de ressource.

`"Hello World" -> "Ĥèĺļõ Ŵòŗłđ" -> "[!!_Ĥèĺļõ Ŵòŗłđ_!!]" -> "[hJ8s1][!!_Ĥèĺļõ Ŵòŗłđ_!!]"`

### <a name="step-6-translate-your-app-into-selected-languages"></a>Étape 6. Traduire votre application vers une sélection de langues

Le kit de ressources Multilingual App Toolkit est intégré au processus de génération. Lors de la génération, les chaînes mises à jour sont automatiquement ajoutées à chaque fichier de langue .xlf.
Une fois votre application testée à l'aide de la pseudo-langue, trois options différentes permettent de traduire votre application vers d'autres langues en vue de la publication.

#### <a name="option-1-translate-the-strings-yourself"></a>Option 1. Traduire les chaînes vous-même

Vous pouvez utiliser Multilingual Editor pour traduire chaque chaîne individuellement. Comme indiqué précédemment, il est inclus dans [Le programme d’installation .msi](https://developer.microsoft.com/en-us/windows/develop/multilingual-app-toolkit).

- Cliquez avec le bouton droit sur le fichier .xlf que vous souhaitez traduire.
- Cliquez sur **Ouvrir avec...** et sélectionnez Multilingual Editor. Vous pouvez éventuellement cliquer sur **Définir par défaut**.
- Pour chaque chaîne, le champ **Source** affiche la chaîne d’origine dans la langue par défaut. Dans **Traduction**, tapez la chaîne traduite dans la langue appropriée pour le fichier .xlf que vous modifiez.
- Lorsque vous avez terminé, enregistrez et fermez le fichier.

Générez à nouveau votre projet pour engendrer la copie des chaînes traduites dans le fichier Ressources (.resw) correspondant au fichier .xlf que vous venez de modifier.

Vous pouvez également lancer Multilingual Editor de la manière suivante. Revenez à l'accueil, affichez toutes les applications, ouvrez le dossier du kit de ressources Multilingual App Toolkit, puis cliquez sur Multilingual Editor pour le lancer.

#### <a name="option-2-send-the-xlf-files-to-a-third-party-for-translation"></a>Option 2. Envoyer les fichiers .xlf à un tiers pour la traduction

Pour sous-traiter la traduction et la tâche de modification aux traducteurs, sélectionnez les fichiers .xlf souhaités dans l’Explorateur de solutions, cliquez dessus avec le bouton droit, puis cliquez sur **Multilingual App Toolkit** > **Exporter les traductions...** .

Sélectionnez **sortie : Destinataire du message** dans la boîte de dialogue Exportation chaîne ressources, puis cliquez sur OK et vos fichiers seront compressés et attaché à un nouveau message électronique. Sélectionnez **sortie : Emplacement du dossier de fichiers**navigateur pour un dossier et cliquez sur OK, si vous le souhaitez choisir pour les fichiers à être compressés, et cliquez sur OK à nouveau vos fichiers seront (compressé et) enregistré à l’emplacement que vous avez choisi, à l’intérieur d’un nouveau dossier nommé pour votre projet.

Lorsque vos traducteurs ont terminé la traduction et vous ont envoyé les fichiers .xlf traduits, vous pouvez les importer dans votre projet. Sélectionnez les fichiers .xlf souhaité dans l’Explorateur de solutions, effectuez un clic droit, puis cliquez sur **Multilingual App Toolkit** >  **/recyclage de l’importation des traductions...** . Cliquez sur **ajouter**, accédez aux fichiers .xlf ou .zip, puis cliquez sur **importation**.

**Remarque** le processus d'importation réalise une validation de base préalable à l'importation. Ainsi, les informations culturelles de la cible dans les fichiers en cours d'importation correspondent aux fichiers .xlf existants.

Générez à nouveau votre projet pour engendrer la copie des chaînes traduites dans le fichier Ressources (.resw) correspondant au fichier .xlf que vous venez d'importer.

Ces fournisseurs tiers proposent des services de localisation et peuvent vous aider à adapter vos produits.

- [Elanex](https://www.strakertranslations.com/)
- [Keywords Studios](https://www.keywordsstudios.com/)
- [Lionbridge](https://www.lionbridge.com)
- [Moravia](https://www.rws.com/what-we-do/rws-moravia/)
- [SDL](https://www.sdl.com/translate/get-started/instant-quote.html)
- [Welocalize](https://www.welocalize.com/)

> [!NOTE]
> La liste ci-dessus est fournie uniquement à titre indicatif et ne constitue en rien une recommandation. Microsoft n’émet aucune déclaration ni aucune garantie à l’égard de ces fournisseurs ou de leurs services et ne pourra en aucun cas être tenu responsable en cas de recours à ces derniers. Toute question, plainte ou demande relative à ces fournisseurs ou services doivent être adressées au fournisseur adéquat.

#### <a name="option-3-use-the-integrated-translation-services"></a>Option 3. Utiliser les services de traduction intégrés

Certaines services de traduction sont intégrés à l'IDE Visual Studio ainsi qu'à Multilingual Editor. Ces outils fournissent un accès aisé aux services de traduction lors du développement de votre produit et pour la localisation de vos ressources. Pour ce service, vous devez être titulaire d'un abonnement à un compte Azure, comme décrit dans la section [Microsoft Translator bascule vers le portail Azure](https://multilingualapptoolkit.uservoice.com/knowledgebase/articles/1167898-microsoft-translator-moves-to-the-azure-portal).

Pour accéder aux services de traduction dans Visual Studio, dans l'Explorateur de solutions, sélectionnez et cliquez avec le bouton droit sur au moins un fichier .xlf, puis cliquez sur **Générer les traductions automatiques**.

L'outil Multilingual Editor fournit le même support de traduction, ainsi que l'ajout de suggestions interactives qui vous permettent de sélectionner la traduction qui correspond le mieux à vos chaînes de ressource. Une fois la suggestion de traduction fournie, vous pouvez affiner la chaîne selon votre style de traduction.

Deux fournisseurs sont acheminés avec le kit de ressources Multilingual App Toolkit.

- Le fournisseur [Portail linguistique Microsoft](https://go.microsoft.com/fwlink/p/?LinkId=330295) permet le recyclage de la traduction et la concordance terminologique en fonction des traductions du texte de l'interface utilisateur pour les produits et services Microsoft.
- Le fournisseur [Microsoft Translator](https://go.microsoft.com/fwlink/p/?LinkId=258220) permet l'utilisation de services de traduction automatique à la demande.

Vous ainsi que vos traducteurs êtes en mesure de gérer le statut des traductions dans Multilingual Editor fin de réviser ultérieurement les traductions incertaines. Vous pouvez définir l’état de chaque chaîne dans le **propriétés** onglet. Valeurs d’état sont : **Nouvelle**, **nécessite révision**, **traduit**, **finale**, et **déconnectés**. L'indicateur situé à gauche de la ligne affiche le statut. Dans Multilingual Editor, lorsque toutes les lignes sont vertes, la tâche de traduction est terminée.

Générez à nouveau votre projet pour engendrer la copie des chaînes traduites dans le fichier Ressources (.resw) correspondant au fichier .xlf que vous venez d'éditer.

### <a name="step-7-upload-your-app-to-the-microsoft-store"></a>Étape 7. Télécharger votre application sur le Microsoft Store

Avant de commencer le processus de certification du Microsoft Store, vous devez exclure le fichier `<project-name>.qps-ploc.xlf` de votre projet. La pseudo-langue est utilisée pour détecter les potentiels problèmes ou bogues d'adaptabilité, mais elle n'est pas valide en tant que langue du Microsoft Store. Si elle est pas supprimée, votre application échouera pendant le processus de certification du Microsoft Store.

## <a name="related-topics"></a>Rubriques connexes

* [Localiser les chaînes dans l’interface utilisateur et le manifeste du package d’application](../../app-resources/localize-strings-ui-manifest.md)
* [Globalisation et localisation](globalizing-portal.md)
* [Instructions pour la globalisation](guidelines-and-checklist-for-globalizing-your-app.md)
* [Rendez votre application localisable](prepare-your-app-for-localization.md)
* [Balise de langue de BCP-47](https://go.microsoft.com/fwlink/p/?linkid=227302)

## <a name="downloads"></a>Téléchargements

* [Le programme d’installation de Multilingual App Toolkit 4.0 .vsix](https://marketplace.visualstudio.com/items?itemName=MultilingualAppToolkit.MultilingualAppToolkit-18308)
* [Le programme d’installation de Multilingual App Toolkit 4.0 .msi](https://developer.microsoft.com/en-us/windows/develop/multilingual-app-toolkit)

## <a name="translation-services"></a>Services de traduction

* [Portail de langue de Microsoft](https://go.microsoft.com/fwlink/p/?LinkId=330295)
* [Microsoft Translator](https://go.microsoft.com/fwlink/p/?LinkId=258220)
