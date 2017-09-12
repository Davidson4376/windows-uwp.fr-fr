---
author: jnHs
Description: "La section Descriptions dans le Windows Store  du processus de soumission d’application vous permet de définir le texte et les images visibles par les clients dans la description de votre application dans le Windows Store."
title: "Créer des annonces d’application dans le WindowsStore"
ms.assetid: 50D67219-B6C6-4EF0-B76A-926A5F24997D
ms.author: wdg-dev-content
ms.date: 08/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.openlocfilehash: 004169178c906ac892865569fd2ed483bd2471fa
ms.sourcegitcommit: 6c6f3c265498d7651fcc4081c04c41fafcbaa5e7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/09/2017
---
# <a name="create-app-store-listings"></a>Créer des annonces d’application dans le WindowsStore


La section **Descriptions dans le Windows Store** du [processus de soumission d’application](app-submissions.md) vous permet de définir le texte et les [images](app-screenshots-and-images.md) qui seront visibles par les clients dans la description de votre application dans le Windows Store.

> [!NOTE]
> Nous avons récemment mis à jour les options de cette page. Si vous disposiez d’une soumission en cours avant que ces nouvelles options ne deviennent disponibles, il est possible que cette soumission continue d’afficher les anciennes options. Si vous souhaitez utiliser les nouvelles options pour cette application, vous pouvez supprimer cette soumission, puis en créer une autre. Dans le cas contraire, les dernières options seront disponibles avec la prochaine mise à jour une fois que vous aurez publié la soumission en cours.

Bien que la plupart des champs d’une **Description dans le Windows Store** soient facultatifs, nous vous conseillons de fournir plusieurs images et autant d’informations que possible afin de mettre en valeur le contenu. Dans la page **Descriptions dans le WindowsStore**, vous devez impérativement fournir une description textuelle et au moins une [capture d’écran](app-screenshots-and-images.md#screenshots).

> [!TIP]
> Vous pouvez également [importer et exporter des descriptions dans le WindowsStore](import-and-export-store-listings.md) si vous souhaitez entrer vos informations de description hors connexion dans un fichier.csv, plutôt que de renseigner ces informations directement dans le tableau de bord. Cette opération peut se révéler particulièrement adaptée si vous disposez de descriptions en différentes langues.

Par défaut, nous utilisons la même description dans le WindowsStore (par langue) pour tous les systèmes d’exploitation que vous ciblez. Si vous souhaitez utiliser une description personnalisée dans le Windows Store pour un système d’exploitation spécifique, vous pouvez [créer des descriptions dans le Windows Store spécifiques à la plateforme](create-platform-specific-store-listings.md). Votre description par défaut est toujours affichée pour les clients sur Windows10.

## <a name="store-listing-languages"></a>Langues des descriptions dans le Windows Store

Vous devez remplir la page **Description dans le Windows Store** dans une langue au minimum. Nous vous recommandons de fournir une description dans le Windows Store dans chaque langue prise en charge par vos packages. Vous avez cependant la possibilité de supprimer les langues que vous ne souhaitez pas afficher sur cette page. Vous pouvez également créer des descriptions dans le WindowsStore dans des langues supplémentaires qui ne sont pas prises en charge par vos packages.

> [!NOTE]
> Si votre soumission inclut déjà des packages, nous affichons les [langues](supported-languages.md) prises en charge par ces derniers sur la page de vue d’ensemble de la soumission (sauf si vous les supprimez).

Pour ajouter ou supprimer des langues de description dans le Windows Store, cliquez sur **Ajouter/Supprimer des langues** à partir de la page de présentation de la soumission. Si vous avez déjà chargé des packages, leurs langues sont répertoriées dans la section **Langues prises en charge par vos packages**. Pour supprimer une ou plusieurs de ces langues, cliquez sur **Supprimer**. Si vous décidez plus tard d’inclure une langue précédemment supprimée de cette section, vous pouvez cliquer sur **Ajouter**.

Dans la section **Langues supplémentaires de description dans le Windows Store**, vous pouvez cliquer sur **Gérer les langues supplémentaires** pour ajouter ou supprimer des langues qui ne sont *pas* incluses dans vos packages. Cochez les cases pour les langues que vous souhaitez ajouter, puis cliquez sur **Mettre à jour**. Les langues que vous avez sélectionnées s’afficheront dans la section **Langues supplémentaires de description dans le Windows Store**. Pour supprimer une ou plusieurs de ces langues, cliquez sur **Supprimer** (ou cliquez sur **Gérer les langues supplémentaires** et décochez la case pour les langues que vous souhaitez supprimer).

Lorsque vous avez terminé vos sélections, cliquez sur **Enregistrer** pour revenir à la page de vue d’ensemble de la soumission.

> [!NOTE]
> Lorsque vous créez une description dans le WindowsStore dans une langue qui n’est pas prise en charge par vos packages, vous devez indiquer lequel de vos noms d’application réservés doit s’afficher dans cette description, dans la mesure où il n’existe aucun package associé dans cette langue à partir duquel extraire le nom. Le nom que vous choisissez ici s’applique seulement à la description du Windows Store pour cette langue et n’a aucune répercussion sur le nom affiché lorsqu’un client installe l’application.

Pour modifier une description dans le Windows Store, cliquez sur le nom de la langue dans la page de présentation de la soumission.

Les champs associés à votre description par défaut dans le Windows Store pour la langue sélectionnée se trouvent en haut de la page **Description dans le Windows Store**. Ces champs sont visibles de tous vos clients, sauf si certains packages ciblent des versions antérieures du système d’exploitation (Windows8.x ou version antérieure; Windows Phone8.x ou version antérieure) ou si vous créez des descriptions dans le Windows Store spécifiques à la plateforme incluant différentes captures d’écran ou informations à présenter aux clients sur les versions de système d’exploitation spécifiées. Pour plus d’informations, consultez [Créer des descriptions spécifiques à la plateforme ](create-platform-specific-store-listings.md).

## <a name="description"></a>Description

Le champ de description est l’emplacement où vous pouvez indiquer aux clients ce que fait votre application. Ce champ obligatoire accepte jusqu’à 10000 caractères.

Pour obtenir des conseils sur la rédaction d’une description attrayante, consultez l’article [Rédiger une description convaincante de l’application](write-a-great-app-description.md).

## <a name="release-notes"></a>Notes de publication

Si vous soumettez votre application pour la première fois, vous laisserez probablement ce champ vide. Si vous proposez une mise à jour d’une application existante, ce champ vous permet d’indiquer aux clients les modifications introduites dans la dernière version. Ce champ est limité à 1500 caractères.

## <a name="screenshots"></a>Captures d’écran

Une capture d’écran est obligatoire pour soumettre votre application. Nous vous recommandons de fournir au moins une capture d’écran pour chaque type d'appareil qui prend en charge votre application.

Pour plus d’informations, voir l’article [Images et captures d’écran de l’application](app-screenshots-and-images.md#screenshots).

## <a name="store-logos"></a>Logos Windows Store 

Les logos Windows Store sont des images facultatives que vous pouvez charger pour améliorer la présentation de votre application aux clients. Vous pouvez également spécifier que seules les images que vous chargez ici doivent être utilisées dans la description de votre application dans le Windows Store pour les clients Windows10, plutôt que d’autoriser le Windows Store à utiliser les images de logo disponibles dans les packages de votre application.

> [!IMPORTANT]
> Si votre application prend en charge Xbox ou WindowsPhone8.1 ou une version antérieure, vous devez fournir certaines images ici dans l’ordre pour que la description s'affiche correctement dans le Windows Store. 

Pour plus d’informations, voir [Logos Windows Store](app-screenshots-and-images.md#store-logos).

## <a name="additional-art-assets"></a>Illustrations supplémentaires

Vous pouvez soumettre des ressources supplémentaires pour votre produit, y compris des bandes-annonces et des images publicitaires. Elles sont toutes facultatives, mais nous vous recommandons d'en charger autant que possible. Ces images permettent de donner aux clients une meilleure idée de votre produit et de rendre sa description plus attrayante.

Pour plus d’informations, voir [Illustrations supplémentaires](app-screenshots-and-images.md#additional-art-assets).

## <a name="additional-information"></a>Informations complémentaires

Les champs de cette section sont tous facultatifs, mais ils permettent d'aider les clients à mieux comprendre la fonction de votre application et ce qui est nécessaire pour une expérience optimale. Nous vous conseillons d'examiner les options décrites ci-dessous et de donner aux clients les informations nécessaires sur votre application ou qui peuvent les inciter à la télécharger.

### <a name="app-features"></a>Fonctionnalités de l’application

Il s’agit de courts résumés des principales fonctionnalités de votre application. Ces dernières sont présentées et décrites au client sous la forme d’une liste à puce pour compléter la description de votre application dans le Windows Store. Chacune de ces informations est limitée à 200caractères. Vous pouvez spécifier jusqu’à 20fonctionnalités.

> [!NOTE]
> Les fonctionnalités de votre application apparaissant sous la forme d’une liste à puces, n’ajoutez pas vos propres puces.

### <a name="additional-system-requirements"></a>Configuration système supplémentaire requise

Si nécessaire, vous pouvez décrire les configurations matérielles requises par votre application pour fonctionner correctement (en plus des informations fournies dans la section **Configuration système requise** de [Propriétés de l’application](enter-app-properties.md#system-requirements). Ces informations sont particulièrement importantes si votre application nécessite du matériel qui peut ne pas être présent sur tous les ordinateurs.

Vous pouvez entrer jusqu’à 11éléments pour les deux champs **Matériel minimum** et **Matériel recommandé**.  Les configurations matérielles sont présentées au client sous la forme d’une liste à puces dans la description de votre application. Chacune de ces informations est limitée à 200caractères.

Les informations que vous entrez ici sont visibles des clients qui consultent la description de votre application dans le Windows Store sur Windows10, version1607 ou ultérieure, en même temps que la configuration requise indiquée sur la page des propriétés du produit.

> [!NOTE]
> Étant donné que votre configuration système requise supplémentaire apparaît sous la forme d’une liste à puces dans votre description dans le WindowsStore, n’ajoutez pas vos propres puces.

### <a name="developed-by"></a>Développé par

Si vous souhaitez inclure un champ **Développé par** dans la description de votre application dans le WindowsStore, entrez ce texte ici. (Le champ **Publié par** répertorie le nom d’affichage de l’éditeur associé à votre compte, que vous fournissiez ou non une valeur pour le champ **Développé par**.)

Ce champ est limité à 255caractères.


## <a name="shared-fields"></a>Champs partagés

Les éléments décrits ci-dessous aident les clients à découvrir et comprendre votre produit. Les informations que vous entrez ici s’appliquent à l’ensemble de vos descriptions du Windows Store dans une langue donnée, quel que soit le système d’exploitation, même si vous [créez des descriptions dans le Windows Store spécifiques à la plateforme](create-platform-specific-store-listings.md).

### <a name="search-terms"></a>Termes de recherche

Les termes de recherche (anciennement appelés mots clés) sont des mots uniques ou des phrases courtes que vos clients ne voient pas, mais qui permettent d’afficher votre application dans les résultats de recherche liés au terme. Vous pouvez inclure jusqu’à 7termes de recherche d’un maximum de 30caractères chacun, et vous pouvez utiliser jusqu’à 21mots distincts dans l’ensemble des termes de recherche.

Lorsque vous ajoutez des termes de recherche, pensez aux termes que vos clients sont susceptibles d’utiliser pour rechercher des applications comme la vôtre, en particulier si ces mots ne figurent pas dans le nom de votre application. Veillez à n’utiliser aucun terme de recherche non véritablement pertinent pour votre application.


### <a name="privacy-policy"></a>Politique de confidentialité

Si vous disposez d’une politique de confidentialité pour votre application, entrez son URL dans ce champ. Votre application doit respecter les lois et règles applicables et proposer une politique de confidentialité s’il y a lieu.

> [!IMPORTANT]
> Microsoft ne fournit aucune politique de confidentialité par défaut pour votre application. De même, votre application n’est couverte par aucune politique de confidentialité Microsoft. Pour déterminer si votre application nécessite une politique de confidentialité, passez en revue le [Contrat du développeur de l’application](https://msdn.microsoft.com/library/windows/apps/hh694058) et les [politiques du Windows Store](https://msdn.microsoft.com/library/windows/apps/dn764944.aspx#pol_10_5_1).

### <a name="copyright-and-trademark-info"></a>Informations de copyright et de marque déposée

Si vous voulez fournir des informations supplémentaires de droits d’auteur et/ou de marque, entrez-les ici. Ce champ est limité à 200caractères.

### <a name="additional-license-terms"></a>Termes de licence supplémentaires

Laissez ce champ vide si vous voulez que votre application possède une licence conforme aux **Termes du contrat de licence d’application standard** (vers lesquels pointe un lien situé dans le [Contrat du développeur de l’application](https://msdn.microsoft.com/library/windows/apps/hh694058)).

Si vos termes de contrat de licence diffèrent de ceux du **Contrat de licence d’application standard**, entrez-les ici.

Si vous entrez une URL dans ce champ, elle apparaît aux clients sous la forme d’un lien sur lequel ces derniers peuvent cliquer pour lire les termes supplémentaires du contrat de licence. Ceci est utile si les termes supplémentaires de votre contrat de licence sont très longs, ou si vous souhaitez inclure des liens hypertexte fonctionnels ou une mise en forme dans les termes supplémentaires de votre contrat de licence.

Vous pouvez saisir jusqu’à 10000caractères de texte dans ce champ. Dans ce cas, ces termes supplémentaires du contrat de licence s’afficheront aux clients sous la forme de texte brut.

### <a name="website"></a>Site web

Entrez l’URL de la page web de votre application. Cette URL doit pointer vers une page de votre propre site web, et non vers la description web de votre application dans le Windows Store.

### <a name="support-contact-info"></a>Coordonnées du support technique

Entrez l’URL de la page web où vos clients peuvent obtenir un support concernant votre application (ou une adresse e-mail pour contacter le support technique).

> [!IMPORTANT]
> Microsoft n’offre à vos clients aucun support technique pour votre application.

