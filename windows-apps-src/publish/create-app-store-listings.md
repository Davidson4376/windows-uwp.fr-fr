---
author: jnHs
Description: "La section Descriptions dans le Windows Store  du processus de soumission d’application vous permet de définir le texte et les images visibles par les clients dans la description de votre application dans le Windows Store."
title: "Créer des annonces d’application dans le WindowsStore"
ms.assetid: 50D67219-B6C6-4EF0-B76A-926A5F24997D
translationtype: Human Translation
ms.sourcegitcommit: d24294583d1ec0186cd63138979d40a06b0c7351
ms.openlocfilehash: 9ef2a465cb8f9143775feab163ee1d66d135a6b3

---

# Créer des annonces d’application dans le WindowsStore


La section **Descriptions dans le Windows Store** du [processus de soumission d’application](app-submissions.md) vous permet de définir le texte et les [images](app-screenshots-and-images.md) qui seront visibles par les clients dans la description de votre application dans le Windows Store.

Bien que la plupart des champs d’une **Description dans le Windows Store** soient facultatifs, nous vous conseillons de fournir plusieurs images et autant d’informations que possible afin de mettre en valeur le contenu. Dans la page **Descriptions dans le Windows Store**, vous devez impérativement renseigner une description textuelle et au moins une [capture d’écran](app-screenshots-and-images.md).

Par défaut, nous utilisons la même description dans le Windows Store (par langue) pour tous les systèmes d’exploitation que vous ciblez. Si vous souhaitez utiliser une description personnalisée dans le Windows Store pour un système d’exploitation spécifique, vous pouvez [créer des descriptions dans le Windows Store spécifiques à la plateforme](create-platform-specific-store-listings.md).

## Langues des descriptions dans le Windows Store

Vous devez remplir la page **Description dans le Windows Store** dans une langue au minimum. Nous vous recommandons de fournir une description dans le Windows Store dans chaque langue prise en charge par vos packages. Vous avez cependant la possibilité de supprimer les langues que vous ne souhaitez pas afficher sur cette page. Vous pouvez également fournir des descriptions dans le Windows Store dans des langues supplémentaires, qui ne sont pas prises en charge par vos packages.

> **Remarque** Si votre soumission inclut déjà des packages, les [langues](supported-languages.md) prises en charge par ces derniers seront affichées sur la page de présentation de la soumission (sauf si vous les supprimez).

Pour ajouter ou supprimer des langues de description dans le Windows Store, cliquez sur **Préférences linguistiques des descriptions dans le Windows Store** à partir de la page de présentation de la soumission. Si vous avez déjà chargé des packages, leurs langues sont répertoriées dans la section **Langues prises en charge par vos packages**. Pour supprimer une ou plusieurs de ces langues, cliquez sur **Supprimer**. Si vous décidez plus tard d’inclure une langue précédemment supprimée de cette section, vous pouvez cliquer sur **Ajouter**.

Dans la section **Langues supplémentaires de description dans le Windows Store**, vous pouvez cliquer sur **Gérer les langues supplémentaires** pour ajouter ou supprimer des langues qui ne sont *pas* incluses dans vos packages. Cochez les cases pour les langues que vous souhaitez ajouter, puis cliquez sur **Mettre à jour**. Les langues que vous avez sélectionnées s’afficheront dans la section **Langues supplémentaires de description dans le Windows Store**. Pour supprimer une ou plusieurs de ces langues, cliquez sur **Supprimer** (ou cliquez sur **Gérer les langues supplémentaires** et décochez la case pour les langues que vous souhaitez supprimer).

Lorsque vous avez terminé vos sélections, cliquez sur **Enregistrer** pour revenir à la page de présentation de la soumission.

> **Remarque** Lors de la création d’une description dans le Windows Store dans une langue qui n’est pas prise en charge par vos packages, vous devez indiquer lequel de vos noms d’application réservés doit être affiché dans cette description, dans la mesure où il n’y a pas de package associé dans cette langue à partir duquel extraire le nom. Le nom que vous choisissez ici s’applique seulement à la description du Windows Store pour cette langue et n’a aucune répercussion sur le nom affiché lorsqu’un client installe l’application.

Pour modifier une description dans le Windows Store, cliquez sur le nom de la langue dans la vue d’ensemble de la soumission. Les sections de la page **Description dans le Windows Store** sont décrites ci-dessous.

## Champs de description par défaut dans le Windows Store

Les champs associés à votre description par défaut dans le Windows Store pour la langue sélectionnée se trouvent en haut de la page **Description dans le Windows Store**. Ces champs sont visibles de tous vos clients, sauf si certains packages ciblent des versions antérieures du système d’exploitation (Windows8.x ou version antérieure; Windows Phone8.x ou version antérieure) ou si vous créez des descriptions dans le Windows Store spécifiques à la plateforme incluant différentes captures d’écran ou informations à présenter aux clients sur les versions de système d’exploitation spécifiées. Pour plus d’informations, consultez [Créer des descriptions spécifiques à la plateforme ](create-platform-specific-store-listings.md).

### Description

Le champ de description est l’emplacement où vous pouvez indiquer aux clients ce que fait votre application. Ce champ obligatoire accepte jusqu’à 10000 caractères.

Pour obtenir des conseils sur la rédaction d’une description attrayante, consultez l’article [Rédiger une description convaincante de l’application](write-a-great-app-description.md).

### Notes de publication

Si vous soumettez votre application pour la première fois, vous laisserez probablement ce champ vide. Si vous proposez une mise à jour d’une application existante, ce champ vous permet d’indiquer aux clients les modifications introduites dans la dernière version. Ce champ est limité à 1500 caractères.

### Captures d’écran

Dans la plupart des cas, cette section comporte plusieurs champs vous permettant de fournir des captures d’écran pour différents types d’appareil. Vous n’êtes pas tenu de proposer des captures d’écran distinctes pour chaque type d’appareil ; la soumission de votre application ne nécessite qu’une seule capture d’écran (mais vous pouvez fournir jusqu’à neuf captures par type d’appareil). Nous recommandons généralement de fournir des captures d’écran pour tous les types d’appareil pris en charge par votre application afin que les clients sachent à quoi cette dernière ressemblera sur leur appareil.

Pour plus d’informations, voir l’article [Images et captures d’écran de l’application](app-screenshots-and-images.md).

### Icône de vignette d’application

Cette icône est utilisée lors de l’affichage de la description de votre application dans le Windows Store aux clients sur Windows Phone8.1 et versions antérieures (et dans certaines dispositions de Windows Store pour les clients sur Windows10). Cette icône doit prendre la forme d’un fichier .png de 300x300pixels.

Pour plus d’informations, consultez [Icône de vignette d’application](app-screenshots-and-images.md#app-tile-icon).

### Fonctionnalités de l’application

Il s’agit de courts résumés des principales fonctionnalités de votre application. Ces dernières sont présentées et décrites au client sous la forme d’une liste à puce dans la description de votre application dans le Windows Store. Chacune de ces informations est limitée à 200caractères. Vous pouvez spécifier jusqu’à 20fonctionnalités.

**Remarque** Ces informations apparaissant sous la forme d’une liste à puces, n’ajoutez pas vos propres puces.

### Configuration système supplémentaire requise

Si nécessaire, vous pouvez décrire les configurations matérielles requises par votre application pour fonctionner correctement (en plus des informations fournies dans la section **Configuration système requise** de [Propriétés de l’application](enter-app-properties.md#system-requirements). Ces informations sont particulièrement importantes si votre application nécessite du matériel qui peut ne pas être présent sur tous les ordinateurs.

 Vous pouvez entrer jusqu’à 11éléments pour les deux champs **Matériel minimum** et **Matériel recommandé**.  Les configurations matérielles sont présentées au client sous la forme d’une liste à puces dans la description de votre application. Chacune de ces informations est limitée à 200caractères. Les informations que vous entrez ici sont visibles des clients qui consultent la description de votre application dans le Windows Store sur Windows10, version1607 ou ultérieure, en même temps que la configuration requise indiquée sur la page des propriétés du produit.

**Remarque** Ces informations apparaissant sous la forme d’une liste à puces, n’ajoutez pas vos propres puces.

## Champs partagés

Les champs décrits ci-après sont tous partagés et s’appliquent à l’ensemble de vos descriptions du Windows Store dans une langue donnée, quel que soit le système d’exploitation, même si vous [créez des descriptions dans le Windows Store spécifiques à la plateforme](create-platform-specific-store-listings.md).

### Mots clés

Les mots clés sont des mots uniques ou des phrases courtes que vos clients ne voient pas, mais qui permettent d’afficher votre application dans les résultats de recherche liés à ces mots clés. Vous pouvez inclure jusqu’à 7 mots clés de 30 caractères maximum chacun.

Si vous voulez ajouter des mots clés, pensez aux termes que vos clients sont susceptibles d’utiliser pour rechercher des applications comme la vôtre, en particulier si ces mots ne figurent pas dans le nom de votre application. Veillez à n’utiliser aucun mot clé non véritablement pertinent pour votre application.

### Informations de copyright et de marque déposée

Si vous voulez fournir des informations supplémentaires de droits d’auteur et/ou de marque, entrez-les ici. Ce champ est limité à 200caractères.

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

**Important** Microsoft ne fournit aucune politique de confidentialité par défaut pour votre application. De même, votre application n’est couverte par aucune politique de confidentialité Microsoft. Pour déterminer si votre application nécessite une politique de confidentialité, passez en revue le [Contrat du développeur de l’application](https://msdn.microsoft.com/library/windows/apps/hh694058) et les [politiques du Windows Store](https://msdn.microsoft.com/library/windows/apps/dn764944.aspx#pol_10_5_1).



<!--HONumber=Aug16_HO5-->


