---
author: jnHs
Description: The Store listings section of the app submission process is where you provide the text and images that customers will see when viewing your app's listing in the Microsoft Store.
title: Créer des descriptions d’application dans le Store
ms.assetid: 50D67219-B6C6-4EF0-B76A-926A5F24997D
ms.author: wdg-dev-content
ms.date: 06/18/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp, description, page du store, notes de publication, titre
ms.localizationpriority: medium
ms.openlocfilehash: bd3585b1a2fee2a00c99990b32902b524f3318da
ms.sourcegitcommit: 753dfcd0f9fdfc963579dd0b217b445c4b110a18
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/27/2018
ms.locfileid: "2862283"
---
# <a name="create-app-store-listings"></a>Créer des descriptions d’application dans le Store


La section **Descriptions dans le Store** du [processus de soumission d’application](app-submissions.md) vous permet de définir le texte et les [images](app-screenshots-and-images.md) qui seront visibles par les clients dans la description de votre application dans le Microsoft Store.

Un grand nombre des champs d'une **Description dans le Store** sont facultatifs, mais nous vous conseillons de fournir plusieurs images et autant d’informations que possible afin que votre description soit attrayante. La condition minimale requise pour que l'étape **Description dans le Store** puisse être considérée comme terminée est une description textuelle et au moins une [capture d’écran](app-screenshots-and-images.md#screenshots). Pour certaines soumissions, les champs [Politique de confidentialité](#privacy-policy) et [Coordonnées du support technique](#support-contact-info) sont également requis. 

> [!TIP]
> Vous pouvez, si vous le souhaitez, [importer et exporter des descriptions dans le Store](import-and-export-store-listings.md), si vous préférez entrer vos informations de description hors connexion dans un fichier.csv, plutôt que de renseigner ces informations et de charger les fichiers directement dans le tableau de bord. L’option d’importation et d’exportation peut être particulièrement pratique si vous avez des descriptions dans de nombreuses langues, dans la mesure où elle vous permet d’effectuer plusieurs mises à jour à la fois. 

Par défaut, nous utilisons la même description dans le Store (par langue) pour tous les systèmes d’exploitation que vous ciblez. Si vous souhaitez utiliser une description personnalisée dans le Store pour un système d’exploitation spécifique pris en charge par votre soumission, vous pouvez [créer des descriptions dans le Store spécifiques à la plateforme](create-platform-specific-store-listings.md). Votre description par défaut est toujours affichée pour les clients sur Windows10.

## <a name="store-listing-languages"></a>Langues des descriptions dans le Store

Vous devez remplir la page **Description dans le Store** dans une langue au minimum. Nous vous recommandons de fournir une description dans le Store dans chaque langue prise en charge par vos packages. Vous avez cependant la possibilité de supprimer les langues que vous ne souhaitez pas afficher sur cette page. Vous pouvez également créer des descriptions dans le Store dans des langues supplémentaires qui ne sont pas prises en charge par vos packages.

> [!NOTE]
> Si votre soumission inclut déjà des packages, nous affichons les [langues](supported-languages.md) prises en charge par ces derniers sur la page de vue d’ensemble de la soumission (sauf si vous les supprimez).

Pour ajouter ou supprimer des langues de description dans le Store, cliquez sur **Ajouter/Supprimer des langues** à partir de la page de présentation de la soumission. Si vous avez déjà chargé des packages, leurs langues sont répertoriées dans la section **Langues prises en charge par vos packages**. Pour supprimer une ou plusieurs de ces langues, cliquez sur **Supprimer**. Si vous décidez plus tard d’inclure une langue précédemment supprimée de cette section, vous pouvez cliquer sur **Ajouter**.

Dans la section **Langues supplémentaires de description dans le Store**, vous pouvez cliquer sur **Gérer les langues supplémentaires** pour ajouter ou supprimer des langues qui ne sont *pas* incluses dans vos packages. Cochez les cases pour les langues que vous souhaitez ajouter, puis cliquez sur **Mettre à jour**. Les langues que vous avez sélectionnées s’afficheront dans la section **Langues supplémentaires de description dans le Store**. Pour supprimer une ou plusieurs de ces langues, cliquez sur **Supprimer** (ou cliquez sur **Gérer les langues supplémentaires** et décochez la case pour les langues que vous souhaitez supprimer).

Lorsque vous avez terminé vos sélections, cliquez sur **Enregistrer** pour revenir à la page de vue d’ensemble de la soumission.

## <a name="add-and-edit-store-listing-info"></a>Ajouter et modifier des informations d’annonce

Pour modifier une liste des magasins, sélectionnez le nom de la langue dans la page Vue d’ensemble d’envoi.

Les champs associés à votre description par défaut dans le Store pour la langue sélectionnée se trouvent en haut de la page **Description dans le Store**. Ces champs sont visibles de tous vos clients, sauf si certains packages ciblent des versions antérieures du système d’exploitation (Windows8.x ou version antérieure; Windows Phone8.x ou version antérieure) ou si vous créez des descriptions dans le Store spécifiques à la plateforme incluant différentes captures d’écran ou informations à présenter aux clients sur les versions de système d’exploitation spécifiées. Pour plus d’informations, consultez [Créer des descriptions spécifiques à la plateforme ](create-platform-specific-store-listings.md).

## <a name="product-name"></a>Nom du produit

Cette zone de liste déroulante vous permet de spécifier quel nom doit être utilisé dans la liste magasin (si vous avez réservé plus d’un nom pour l’application).

Si vous avez téléchargé les modules dans la même langue que la liste magasin sur lequel vous travaillez, le nom utilisé dans les packages est sélectionné. Si vous devez [Renommer l’application](manage-app-names.md#rename-an-app-that-has-already-been-published) une fois qu’il est déjà été publié, vous pouvez sélectionner un autre nom réservé lorsque vous créez une nouvelle soumission des packages qui utilisent le nouveau nom.

Si vous n’avez pas téléchargé les packages de la langue en cours et vous avez réservé plusieurs noms, vous devez sélectionner l’un de vos noms réservés à une application, car il n’est pas un package associé dans cette langue à partir de laquelle extraire le nom.

> [!NOTE]
> Le **nom du produit** que vous sélectionnez uniquement s’applique à la liste dans la langue des magasins dans lequel vous travaillez. Il n’influe pas sur le nom affiché lorsqu’un client installe l’application; Ce nom est fourni à partir du manifeste du package qui est installé. Pour éviter toute confusion, nous vous recommandons que de chaque langue packages et Store listing le même nom.

## <a name="description"></a>Description

Le champ de description est l’emplacement où vous pouvez indiquer aux clients ce que fait votre application. Ce champ obligatoire accepte jusqu’à 10000 caractères.

Pour obtenir des conseils sur la rédaction d’une description attrayante, consultez l’article [Rédiger une description convaincante de l’application](write-a-great-app-description.md).

<span id="release-notes" />

## <a name="whats-new-in-this-version"></a>Nouveautés de cette version

Si vous soumettez votre application pour la première fois, laissez ce champ vide. Si vous proposez une mise à jour d’une application existante, ce champ vous permet d’indiquer aux clients les modifications introduites dans la dernière version. Ce champ est limité à 1500 caractères. (Ce champ s’appelait auparavant **Notes de publication**).

## <a name="app-features"></a>Fonctionnalités de l’application

Il s’agit de courts résumés des principales fonctionnalités de votre application. Ces informations sont présentées au client sous la forme d’une liste à puce dans la section **Fonctionnalités** de la description de votre application dans le Store, en complément de la **Description**. Chacune de ces informations est limitée à 200caractères par fonctionnalité. Vous pouvez spécifier jusqu’à 20fonctionnalités.

> [!NOTE]
> Les fonctionnalités de votre application apparaissant sous la forme d’une liste à puces dans la description du Store, n’ajoutez pas vos propres puces.

## <a name="screenshots"></a>Captures d’écran

Une capture d’écran est obligatoire pour soumettre votre application. Nous vous recommandons de fournir au moins quatrecaptures d’écran pour chaque type d’appareils pris en charge par votre application afin que les utilisateurs puissent voir à quoi cette dernière ressemblera sur leur type d’appareil.

Pour plus d’informations, voir l’article [Images et captures d’écran de l’application](app-screenshots-and-images.md#screenshots).


## <a name="store-logos"></a>Logos Store 

Les logos Store sont des images facultatives que vous pouvez charger pour améliorer la présentation de votre application aux clients. Vous pouvez également spécifier que seules les images que vous chargez ici doivent être utilisées dans la description de votre application dans le Store pour les clients sous Windows10 (y compris Xbox), plutôt que d’autoriser le Store à utiliser les images de logo disponibles dans les packages de votre application.

> [!IMPORTANT]
> Si votre application prend en charge Xbox ou WindowsPhone8.1 ou une version antérieure, vous devez fournir certaines images ici dans l’ordre pour que la description s'affiche correctement dans le Store. 

Pour plus d’informations, voir [Logos Store](app-screenshots-and-images.md#store-logos).


## <a name="additional-art-assets"></a>Illustrations supplémentaires

Vous pouvez soumettre des ressources supplémentaires pour votre produit, y compris des bandes-annonces et des images publicitaires. Elles sont toutes facultatives, mais nous vous recommandons d'en charger autant que possible. Ces images permettent de donner aux clients une meilleure idée de votre produit et de rendre sa description plus attrayante.

Pour plus d’informations, voir [Illustrations supplémentaires](app-screenshots-and-images.md#additional-art-assets).

<a id="supplemental-information" />

## <a name="supplemental-fields"></a>Champs supplémentaires

Les champs de cette section sont tous facultatifs. Passez en revue les informations ci-dessous pour déterminer si ces informations ont du sens pour votre soumission. En particulier, le champ **Brève description** est recommandée pour la plupart des soumissions. D’autres champs aident à fournir une expérience optimale pour votre produit dans différents scénarios.

### <a name="short-title"></a>Titre court

Version raccourcie du nom de votre produit. S’il est fourni, ce nom plus court peut apparaître à divers emplacements sur XboxOne (pendant l’installation, dans les succès, etc.) à la place du titre complet de votre produit.

Ce champ est limité à 50caractères.


### <a name="sort-title"></a>Titre du tri

Si votre produit peut être trié par ordre alphabétique ou orthographié de différentes manières, vous pouvez entrer une autre version ici. Cela permet aux clients de trouver votre produit plus rapidement s’ils tapent cette version lors de la recherche. 

Ce champ est limité à 255caractères.


### <a name="voice-title"></a>Titre vocal

Autre nom pour votre produit qui, s’il est fourni, peut être utilisé dans l’expérience audio sur XboxOne lors de l’utilisation de Kinect ou d’un casque.

Ce champ est limité à 255caractères.


### <a name="short-description"></a>Description courte

Une description plus courte et plus accrocheuse peut être utilisé dans la partie supérieure de la description dans le Store de votre produit. Si elle n'est pas fournie, le premier paragraphe (500caractères maximum) de votre [description](#description) la plus longue sera utilisé. Dans la mesure où votre description apparaît également sous ce texte, nous vous conseillons de fournir une brève description avec un texte différent avant que votre description dans le Store ne soit pas répétitive.

Concernant les jeux, la brève description peut également apparaître dans la section Informations du Hub de jeux sur XboxOne.

Pour obtenir les meilleurs résultats, conservez votre description courte sous 270 caractères. Le champ a une limite de 500 caractères, mais dans certains affichages, seuls les premiers 270 caractères seront affichera (avec un lien pour afficher le reste de la description courte disponible).


### <a name="additional-system-requirements"></a>Configuration système supplémentaire requise

Si nécessaire, vous pouvez décrire les configurations matérielles requises par votre application pour fonctionner correctement (en plus des informations fournies dans la section **Configuration système requise** de [Propriétés de l’application](enter-app-properties.md#system-requirements). Ces informations sont particulièrement importantes si votre application nécessite du matériel qui peut ne pas être présent sur tous les ordinateurs. Par exemple, si votre application ne fonctionne correctement qu'avec un matériel USB externe comme une imprimante3D ou un microcontrôleur, nous vous conseillons de les entrer ici. Les informations que vous entrez sont visibles par les clients qui consultent la description de votre application dans le Store sur Windows10, version1607 ou ultérieure (y compris Xbox), en même temps que la configuration requise indiquée sur la page des propriétés du produit. 

Vous pouvez entrer jusqu’à 11éléments pour les deux champs **Matériel minimum** et **Matériel recommandé**. Ils sont présentés au client sous la forme d’une liste à puces dans votre Description dans le Store. Chacune de ces informations est limitée à 200caractères.

> [!NOTE]
> Étant donné que votre configuration système requise supplémentaire apparaît sous la forme d’une liste à puces dans votre description dans le Store, n’ajoutez pas vos propres puces.


<span id="shared-fields" />

## <a name="additional-information"></a>Informations complémentaires

Les éléments décrits ci-dessous aident les clients à découvrir et comprendre votre produit. Les informations que vous entrez ici s’appliquent à l’ensemble de vos descriptions du Store dans une langue donnée, quel que soit le système d’exploitation, même si vous [créez des descriptions dans le Store spécifiques à la plateforme](create-platform-specific-store-listings.md). (Cette section était précédemment appelée **Champs partagés**).

### <a name="search-terms"></a>Termes de recherche

Les termes de recherche (anciennement appelés mots-clés) sont des mots uniques ou des phrases courtes que vos clients ne voient pas, mais qui vous permettent de rendre votre application détectable dans le Store lorsque les clients effectuent une recherche avec ces termes. Vous pouvez inclure jusqu’à 7termes de recherche d’un maximum de 30caractères chacun, et vous pouvez utiliser jusqu’à 21mots distincts dans l’ensemble des termes de recherche.

Lorsque vous ajoutez des termes de recherche, pensez aux termes que vos clients sont susceptibles d’utiliser pour rechercher des applications comme la vôtre, en particulier si ces mots ne figurent pas dans le nom de votre application. Veillez à n’utiliser aucun terme de recherche non véritablement pertinent pour votre application.

### <a name="copyright-and-trademark-info"></a>Informations de copyright et de marque déposée

Si vous voulez fournir des informations supplémentaires de droits d’auteur et/ou de marque, entrez-les ici. Ce champ est limité à 200caractères.


### <a name="additional-license-terms"></a>Termes de licence supplémentaires

Laissez ce champ vide si vous voulez que votre application possède une licence conforme aux **Termes du contrat de licence d’application standard** (vers lesquels pointe un lien situé dans le [Contrat du développeur de l’application](https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement)).

Si vos termes de contrat de licence diffèrent de ceux du **Contrat de licence d’application standard**, entrez-les ici.

Si vous entrez une URL dans ce champ, elle apparaît aux clients sous la forme d’un lien sur lequel ces derniers peuvent cliquer pour lire les termes supplémentaires du contrat de licence. Ceci est utile si les termes supplémentaires de votre contrat de licence sont très longs, ou si vous souhaitez inclure des liens hypertexte fonctionnels ou une mise en forme dans les termes supplémentaires de votre contrat de licence.

Vous pouvez saisir jusqu’à 10000caractères de texte dans ce champ. Dans ce cas, ces termes supplémentaires du contrat de licence s’afficheront aux clients sous la forme de texte brut.


### <a name="developed-by"></a>Développé par

Si vous souhaitez inclure un champ **Développé par** dans la description de votre application dans le Store, entrez ce texte ici. (Le champ **Publié par** répertorie le nom d’affichage de l’éditeur associé à votre compte, que vous fournissiez ou non une valeur pour le champ **Développé par**.)

Ce champ est limité à 255caractères.
 

<span id="privacy-policy" />

> [!NOTE]
> Les champs **Politique de confidentialité**, **Site web** et **Coordonnées du support technique** sont désormais situés dans la page [Propriétés](enter-app-properties.md).

