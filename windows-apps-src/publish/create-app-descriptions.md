---
Description: La section Description du processus de soumission d’application vous permet de définir le texte et les images qui seront visibles par les clients dans la description de votre application dans le Windows Store.
title: Création des descriptions d’application
ms.assetid: 50D67219-B6C6-4EF0-B76A-926A5F24997D
---

# Création des descriptions d’application


La section **Description** du [processus de soumission d’application](app-submissions.md) vous permet de définir le texte et les [images](app-screenshots-and-images.md) qui seront visibles par les clients dans la description de votre application dans le Windows Store.

Bien que la plupart des champs de la **Description** soient facultatifs, nous vous conseillons de fournir plusieurs images et autant d’informations que possible afin de mettre en valeur le contenu. Dans la page **Description**, vous devez impérativement renseigner une description textuelle et au moins une [capture d’écran](app-screenshots-and-images.md).

Par défaut, nous utilisons la même description (par langue) pour tous les systèmes d’exploitation que vous ciblez. Si vous souhaitez utiliser une description personnalisée pour un système d’exploitation spécifique, vous pouvez [créer des descriptions spécifiques de la plateforme](create-platform-specific-descriptions.md).

## Langues de description

Vous devez remplir la page **Description** dans une langue au minimum. Nous vous recommandons de fournir une description dans chaque langue prise en charge par vos packages. Vous avez cependant la possibilité de supprimer les langues que vous ne souhaitez pas afficher sur cette page. Vous pouvez également fournir des descriptions dans des langues supplémentaires, qui ne sont pas prises en charge par vos packages.

> Remarque Si votre soumission inclut déjà des packages, les [langues](supported-languages.md) prises en charge par ces derniers seront affichées sur la page de présentation de la soumission (sauf si vous les supprimez).

Pour ajouter ou supprimer des langues de description, cliquez sur **Gérer les langues de description** à partir de la page de présentation de la soumission. Si vous avez déjà chargé des packages, leurs langues seront répertoriées dans la section **Langues prises en charge par vos packages**. Pour supprimer une ou plusieurs de ces langues, cliquez sur **Supprimer**. Si vous décidez de fournir une description que vous avez supprimée précédemment, cliquez sur **Ajouter** pour la restaurer.

Pour ajouter ou supprimer des langues qui ne figurent pas dans vos packages, cliquez sur **Gérer les langues supplémentaires** dans la section **Langues de description supplémentaires**. Cochez les cases pour les langues que vous souhaitez ajouter, puis cliquez sur **Mettre à jour**. Les langues que vous avez sélectionnées s’afficheront dans la section **Langues de description supplémentaires**. Pour supprimer une ou plusieurs de ces langues, cliquez sur **Supprimer** (ou cliquez sur **Gérer les langues supplémentaires** et décochez la case pour les langues que vous souhaitez supprimer).

Lorsque vous avez terminé vos sélections, cliquez sur **Enregistrer** pour revenir à la page de présentation de la soumission.

Pour modifier une description, cliquez sur son nom dans la page de présentation de la soumission. Les sections de la page **Description** sont décrites ci-dessous.

## Champs de description par défaut


En haut de la page **Description**, nous affichons les champs associés à votre **description par défaut**. Ces champs sont visibles par tous vos clients, sauf si vous [créez des descriptions spécifiques de la plateforme](create-platform-specific-descriptions.md). Dans ce cas, ces champs sont répétés sous des en-têtes de section pour chaque système d’exploitation cible pour lequel vous avez créé une description spécifique de la plateforme, en-dessous de votre **description par défaut** (qui est affichée aux clients Windows 10, ainsi qu’aux clients utilisant tout système d’exploitation pris en charge par votre application pour lequel vous n’avez pas créé de description personnalisée). Assurez-vous que chaque champ de la section **Description par défaut** contient toutes les informations que vous destinez aux clients de votre application (même si vous voulez que certains de ces champs présentent les mêmes informations pour l’ensemble des systèmes d’exploitation).

Pour plus d’informations, voir [Créer des descriptions spécifiques de la plateforme](create-platform-specific-descriptions.md).

### Description

Le champ de description est l’emplacement où vous pouvez indiquer aux clients ce que fait votre application. Ce champ obligatoire accepte jusqu’à 10 000 caractères.

Pour obtenir des conseils sur la rédaction d’une description attrayante, consultez l’article [Rédiger une description convaincante de l’application](write-a-great-app-description.md).

### Notes de publication

Si vous soumettez votre application pour la première fois, vous laisserez probablement ce champ vide. Si vous proposez une mise à jour d’une application existante, ce champ vous permet d’indiquer aux clients les modifications introduites dans la dernière version. Ce champ est limité à 1 500 caractères.

### Captures d’écran

Dans la plupart des cas, cette section comporte plusieurs champs vous permettant de fournir des captures d’écran pour différents types d’appareil. Vous n’êtes pas tenu de proposer des captures d’écran distinctes pour chaque type d’appareil ; la soumission de votre application ne nécessite qu’une seule capture d’écran (mais vous pouvez fournir jusqu’à neuf captures par type d’appareil). Nous recommandons généralement de fournir des captures d’écran pour tous les types d’appareil pris en charge par votre application afin que les clients sachent à quoi cette dernière ressemblera sur leur appareil.

Pour plus d’informations, voir l’article [Images et captures d’écran de l’application](app-screenshots-and-images.md).

### Icône de vignette d’application

L’icône de vignette d’application est utilisée en cas d’affichage de la description de votre application à l’intention des clients qui utilisent Windows Phone 8.1 ou une version antérieure (ainsi que pour les clients utilisant Windows 10 si vos packages ciblent uniquement Windows Phone 8.1 ou une version antérieure). Cette icône doit prendre la forme d’un fichier .png de 300 x 300 pixels.

### Fonctionnalités de l’application

Il s’agit de courts résumés des principales fonctionnalités de votre application. Ces dernières sont présentées et décrites au client sous la forme d’une liste à puce dans la description de votre application. Chacune de ces informations est limitée à 200 caractères. Vous pouvez spécifier jusqu’à 20 fonctionnalités.

**Remarque** Ces informations apparaissant sous la forme d’une liste à puces, n’ajoutez pas vos propres puces.

 

### Matériel recommandé

Décrivez les configurations matérielles requises pour le fonctionnement de votre application. Ces informations sont particulièrement importantes si votre application nécessite du matériel qui peut ne pas être présent sur tous les ordinateurs. Les configurations matérielles sont présentées au client sous la forme d’une liste à puces dans la description de votre application. Chacune de ces informations est limitée à 200 caractères. Vous pouvez spécifier jusqu’à 11 éléments.

**Remarque** Ces informations apparaissant sous la forme d’une liste à puces, n’ajoutez pas vos propres puces.

 

## Champs partagés


Contrairement aux champs décrits ci-dessus, les champs de la section **Champs partagés** ne sont pas personnalisables pour une plateforme spécifique. Les champs décrits ci-après sont tous partagés et s’appliquent à l’ensemble de vos descriptions dans une langue donnée, quel que soit le système d’exploitation, même si vous [créez des descriptions spécifiques de la plateforme](create-platform-specific-descriptions.md).

### Mots clés

Les mots clés sont des mots uniques ou des phrases courtes que vos clients ne voient pas, mais qui permettent d’afficher votre application dans les résultats de recherche liés à ces mots clés. Vous pouvez inclure jusqu’à 8 mots clés de 45 caractères maximum chacun.

Si vous voulez ajouter des mots clés, pensez aux termes que vos clients sont susceptibles d’utiliser pour rechercher des applications comme la vôtre, en particulier si ces mots ne figurent pas dans le nom de votre application. Veillez à n’utiliser aucun mot clé non véritablement pertinent pour votre application.

### Informations de copyright et de marque déposée

Si vous voulez fournir des informations supplémentaires de droits d’auteur et/ou de marque, entrez-les ici. Ce champ est limité à 200 caractères.

### Termes de licence supplémentaires

Laissez ce champ vide si vous voulez que votre application possède une licence conforme aux **Termes du contrat de licence d’application standard** (vers lesquels pointe un lien situé dans le [Contrat du développeur de l’application](https://msdn.microsoft.com/library/windows/apps/hh694058)).

Si vos termes de contrat de licence diffèrent de ceux du **Contrat de licence d’application standard**, entrez-les ici.

Si vous entrez une URL dans ce champ, elle apparaît aux clients sous la forme d’un lien sur lequel ces derniers peuvent cliquer pour lire les termes supplémentaires du contrat de licence. Ceci est utile si les termes supplémentaires de votre contrat de licence sont très longs, ou si vous souhaitez inclure des liens hypertexte fonctionnels ou une mise en forme dans les termes supplémentaires de votre contrat de licence.

Vous pouvez saisir jusqu’à 10 000 caractères de texte dans ce champ. Dans ce cas, ces termes supplémentaires du contrat de licence s’afficheront aux clients sous la forme de texte brut.

### Site web

Entrez l’URL de la page web de votre application. Cette URL doit pointer vers une page de votre propre site web, et non vers la description web de votre application dans le Windows Store.

### Coordonnées du support technique

Entrez l’URL de la page web où vos clients peuvent obtenir un support concernant votre application (ou une adresse e-mail pour contacter le support technique).

**Important** Microsoft n’offre pas à vos clients de support technique pour votre application.

 

### Politique de confidentialité

Si vous disposez d’une politique de confidentialité pour votre application, entrez son URL dans ce champ. Votre application doit respecter les lois et règles applicables et proposer une politique de confidentialité s’il y a lieu.

**Important** Microsoft ne fournit aucune politique de confidentialité par défaut pour votre application. De même, votre application n’est couverte par aucune politique de confidentialité Microsoft. Pour déterminer si votre application nécessite une politique de confidentialité, passez en revue le [Contrat du développeur de l’application](https://msdn.microsoft.com/library/windows/apps/hh694058) et les [politiques du Windows Store](https://msdn.microsoft.com/en-us/library/windows/apps/dn764944.aspx#pol_10_5_1).


<!--HONumber=Mar16_HO5-->


