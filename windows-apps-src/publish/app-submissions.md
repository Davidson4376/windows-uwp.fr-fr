---
author: jnHs
Description: Once you've created your app by reserving a name, you can start working on getting it published. The first step is to create a submission.
title: Soumissions d’applications
ms.assetid: 363BB9E4-4437-4238-A80F-ABDFC70D96E4
keywords: liste de vérification, windows, uwp, soumission, soumettre, jeu, application
ms.author: wdg-dev-content
ms.date: 08/07/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: b3aed7c5fe912155904b3b0da666427e6727d19d
ms.sourcegitcommit: b7e3d222e229cdbf04e837fcb94fb7d84a93de09
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/26/2018
ms.locfileid: "5604897"
---
# <a name="app-submissions"></a>Soumissions d’applications


Après avoir [créé votre application en réservant un nom](create-your-app-by-reserving-a-name.md), vous pouvez commencer à vous occuper de sa publication. La première étape consiste à créer une **soumission**.

Vous pouvez démarrer votre soumission lorsque votre application est terminée et prête pour publication, ou commencer à entrer des informations avant même d’avoir écrit la moindre ligne de code. Mises à jour apportées à votre soumission sont enregistrées, afin de pouvoir y revenir et y travailler à chaque fois que vous êtes prêt.

> [!NOTE]
> Vous devez disposer d’un [compte de développeur](http://go.microsoft.com/fwlink/p/?LinkId=615100) pour accéder au [Centre de développement Windows](https://partner.microsoft.com/dashboard) et soumettre des applications dans le Microsoft Store.

Une fois votre application publiée, vous pouvez publier une version mise à jour en créant une autre soumission dans le tableau de bord. Le fait de créer une soumission vous permet d'introduire et de publier tous les changements nécessaires, que vous chargiez d'autres packages ou que vous changiez juste des détails comme le prix ou la catégorie. Pour créer une soumission pour une application publiée, cliquez sur **Mettre à jour** en regard de la soumission la plus récente affichée sur la page de présentation de l’application. Vous pouvez également [Supprimer une application à partir du magasin](guidance-for-app-package-management.md#removing-an-app-from-the-store) si vous avez besoin pour effectuer cette opération (et mettez-le à disposition plus tard, si vous souhaitez obtenir).

> [!NOTE]
> Cette section de la documentation explique comment créer une soumission d’applications sur le tableau de bord du Centre de développement. Sinon, vous pouvez utiliser [l’API de soumission au Microsoft Store](../monetize/create-and-manage-submissions-using-windows-store-services.md) pour automatiser la soumission d’applications.

## <a name="app-submission-checklist"></a>Liste de vérification relative à la soumission d’une application

Voici la liste des informations que vous pouvez fournir quand vous soumettez votre application, avec des liens vers des informations complémentaires.

Les éléments que vous devez obligatoirement fournir ou spécifier sont signalés ci-dessous. Certains sont facultatifs ou ont des valeurs par défaut que vous pouvez modifier selon vos besoins. Vous n’êtes pas obligé de travailler sur ces sections dans l’ordre indiqué ici.

### <a name="pricing-and-availability-page"></a>Page Tarification et disponibilité
| Nom du champ                    | Remarques                                       | Informations supplémentaires                                                             |
|-------------------------------|---------------------------------------------|---------------------------------------------------------------------------|
| **Marchés**                   | Par défaut: tous les marchés possibles  | [Définition des prix et sélection du marché](define-pricing-and-market-selection.md)         |
| **Public**                | Par défaut: public non privé | [Public](choose-visibility-options.md#audience) |
| **Détectabilité**                | Par défaut: rendre cette application accessible et détectable dans le Store | [Détectabilité](choose-visibility-options.md#discoverability) |
| **Planification**                  | Par défaut: publication dès que possible        | [Configurer une planification précise de la publication](configure-precise-release-scheduling.md) |
| **Prix de base**                | Obligatoire                                    | [Définition et planification du prix de l’application](set-and-schedule-app-pricing.md)              |
| **Essai gratuit**                | Par défaut: aucune version d'essai gratuit                      | [Essai gratuit](set-app-pricing-and-availability.md#free-trial)              |
| **Prix de vente**              | Facultatif                                    | [Commercialiser des applications et composants additionnels](put-apps-and-add-ons-on-sale.md)           |
| **Gestion des licences organisationnelles**    | Par défaut : autoriser l'acquisition en volume par des organisations | [Options de gestion des licences organisationnelles](organizational-licensing.md)        |
      |


### <a name="properties-page"></a>Page Propriétés

| Nom du champ                    | Remarques                                       | Informations supplémentaires                                                             |
|-------------------------------|---------------------------------------------|---------------------------------------------------------------------------|
| **Catégorie et sous-catégorie**  | Obligatoire                                    | [Tableau des catégories et sous-catégories](category-and-subcategory-table.md)       |
| **URL de la politique de confidentialité**            | Obligatoire pour plusieurs applications. Voir le [contrat du développeur d'applications](https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement) et les [stratégies du Microsoft Store](https://docs.microsoft.com/en-us/legal/windows/agreements/store-policies#105-personal-information) | [URL de la politique de confidentialité](enter-app-properties.md#privacy-policy-url)        |
| **site web**                   | Facultatif                                    | [Site web](enter-app-properties.md#website)                   |
| **Coordonnées du support technique**      | Obligatoire si votre produit est disponible sur Xbox; dans le cas contraire facultatif (mais recommandé)                                   | [Coordonnées du support technique](enter-app-properties.md#support-contact-info)              |
| **Paramètres de jeux**             | Facultatif (applicable uniquement aux jeux)         | [Paramètres de jeux](enter-app-properties.md#game-settings) |
| **Mode d’affichage**             | Facultatif                   | [Mode d’affichage](enter-app-properties.md#display-mode) |
| **Déclarations de produit**          | Par défaut: les clients peuvent installer cette application sur un autre lecteur ou dispositif de stockage. Windows peut inclure les données de cette application dans les sauvegardes automatiques sur OneDrive | [Déclarations de produit](app-declarations.md) |
| **Configuration requise**      | Facultatif                                    | [Configuration requise](enter-app-properties.md#system-requirements)      |

<span/>

### <a name="age-ratings-page"></a>Page Classification par âge

| Nom du champ                    | Remarques                                       | Informations supplémentaires                          |
|-------------------------------|---------------------------------------------|----------------------------------------|
| **Classification par âge**               | Obligatoire                                    | [Classification par âge](age-ratings.md)          |

<span/>

### <a name="packages-page"></a>Page Packages

| Nom du champ                    | Remarques                                  | Informations supplémentaires                          |
|-------------------------------|----------------------------------------|----------------------------------------|
| **Contrôle du chargement des packages**    | Obligatoire (au moins un package)        | [Chargement des packages d’application](upload-app-packages.md) |
| **Disponibilité de la famille d’appareils** | Par défaut: basée sur les packages       | [Disponibilité de la famille d’appareils](device-family-availability.md) |
| **Lancement de package progressif**   | Facultatif (pour les mises à jour uniquement)            | [Lancement de package progressif](gradual-package-rollout.md) |
| **Mise à jour obligatoire**          | Facultatif (pour les mises à jour uniquement)            | [Mise à jour obligatoire](upload-app-packages.md#mandatory-update)


### <a name="store-listings"></a>Descriptions dans le Windows Store

Vous devez indiquer toutes les informations requises pour au moins l’une des langues prises en charge par votre application. Nous vous recommandons de fournir des [descriptions dans le Store](create-app-store-listings.md) dans toutes les langues prises en charge par votre application, et vous pouvez également [fournir des descriptions dans le Store dans d’autres langues](create-app-store-listings.md#store-listing-languages). Pour faciliter la gestion de plusieurs descriptions pour le même produit, vous pouvez [importer et exporter des descriptions du Store](import-and-export-store-listings.md).

| Nom du champ                    | Remarques                                       | Informations supplémentaires                                                     |
|-------------------------------|---------------------------------------------|-------------------------------------------------------------------|
| **Description**               | Obligatoire                                    | [Rédiger une description convaincante de l’application](write-a-great-app-description.md) |
| **Nouveautés de cette version**   | Facultatif                                 | [Notes de publication](create-app-store-listings.md#whats-new-in-this-version)       |
| **Fonctionnalités de l’application**              | Facultatif                                    | [Fonctionnalités de l’application](create-app-store-listings.md#app-features)         |
| **Captures d'écran**               | Obligatoire (au moins une capture d’écran, quatre ou plus recommandées)          | [Captures d'écran](app-screenshots-and-images.md#screenshots)          |
| **Logos Store**               | Recommandé; obligatoire pour certaines versions du système d’exploitation | [Logos Store](app-screenshots-and-images.md#store-logos)             |
| **Illustrations supplémentaires**     | Recommandé (surtout pour certaines versions du système d’exploitation)         | [Illustrations supplémentaires](app-screenshots-and-images.md#additional-art-assets) |
| **Bandes-annonces**                  | Facultatif                                    | [Bandes-annonces](app-screenshots-and-images.md#trailers)                | 
| **Champs supplémentaires**  | Facultatif                                    | [Informations supplémentaires](create-app-store-listings.md#supplemental-fields) 
| **Termes de recherche**              | Facultatif                                    | [Termes de recherche](create-app-store-listings.md#search-terms)         |
| **Informations de copyright et de marque déposée** | Facultatif                                 | [Informations de copyright et de marque déposée](create-app-store-listings.md#copyright-and-trademark-info) |
| **Termes de licence supplémentaires**  | Facultatif                                    | [Termes de licence supplémentaires](create-app-store-listings.md#additional-license-terms) |
| **Développé par**              | Facultatif                                    | [Développé par](create-app-store-listings.md#developed-by)                   |
| **Descriptions dans le Store spécifiques à la plateforme** | Facultatif                               | [Créer des descriptions dans le Store spécifiques à la plateforme](create-platform-specific-store-listings.md)  |

<span/>

### <a name="submission-options-page"></a>Page des options de soumission

| Nom du champ                    | Remarques                                       | Informations supplémentaires                                                     |
|-------------------------------|---------------------------------------------|-------------------------------------------------------------------|
| **Options de mise en attente de publication**     | Par défaut: publier cette soumission dès qu’elle a obtenu la certification (ou selon les dates que vous avez sélectionnées dans la section Planification)      | [Options de mise en attente de publication](manage-submission-options.md#publishing-hold-options)    
| **Notes pour la certification**     | Recommandé          | [Notes pour la certification](notes-for-certification.md)             |
| **Fonctionnalités restreintes**     | Obligatoire si votre produit déclare certaines [fonctionnalités restreintes](../packaging/app-capability-declarations.md#restricted-capabilities)    | [Fonctionnalités restreintes](manage-submission-options.md#publishing-hold-options)       

<span/>

> [!NOTE]
> Pour plus d’informations sur la publication d’applications métier directement à l’attention des entreprises, voir [Distribuer des applications métier aux entreprises](distribute-lob-apps-to-enterprises.md).
