---
Description: Après avoir créé votre application en réservant un nom, vous pouvez commencer à vous occuper de sa publication. La première étape consiste à créer une soumission.
title: Soumissions d’application
ms.assetid: 363BB9E4-4437-4238-A80F-ABDFC70D96E4
keywords: liste de vérification, windows, uwp, soumission, soumettre, jeu, application
ms.date: 10/31/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: b98ea7f1d28c4fcd63cd2d4706905578b240e126
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57643284"
---
# <a name="app-submissions"></a>Soumissions d’application


Après avoir [créé votre application en réservant un nom](create-your-app-by-reserving-a-name.md), vous pouvez commencer à vous occuper de sa publication. La première étape consiste à créer une **soumission**.

Vous pouvez démarrer votre soumission lorsque votre application est terminée et prête pour publication, ou commencer à entrer des informations avant même d’avoir écrit la moindre ligne de code. Mises à jour que vous apportez à votre soumission sont enregistrés, afin de pouvoir y revenir et travailler dessus chaque fois que vous êtes prêt.

> [!NOTE]
> Vous devez disposer d’un actif [compte de développeur](https://go.microsoft.com/fwlink/p/?LinkId=615100) dans [partenaires](https://partner.microsoft.com/dashboard) afin de proposer des applications dans le Microsoft Store.

Une fois que votre application est publiée, vous pouvez publier une version mise à jour en créant une autre soumission dans Partner Center. Le fait de créer une soumission vous permet d'introduire et de publier tous les changements nécessaires, que vous chargiez d'autres packages ou que vous changiez juste des détails comme le prix ou la catégorie. Pour créer une nouvelle soumission d’une application publiée, cliquez sur **mise à jour** en regard de l’envoi le plus récent affiché sur son **vue d’ensemble** page. Vous pouvez également [supprimer une application à partir du Store](guidance-for-app-package-management.md#removing-an-app-from-the-store) si vous devez le faire (et puis rendre disponible plus tard, si vous le souhaitez).

> [!NOTE]
> Cette section de la documentation décrit comment créer une soumission de l’application Centre de partenaires. Sinon, vous pouvez utiliser [l’API de soumission au Microsoft Store](../monetize/create-and-manage-submissions-using-windows-store-services.md) pour automatiser la soumission d’applications.

> [!IMPORTANT]
> À compter du 31 octobre 2018, produits nouvellement créé ne peut pas inclure les packages ciblant 8.x/Windows de Windows Phone 8.x ou une version antérieure. Pour plus d’informations, consultez ce [billet de blog](https://blogs.windows.com/buildingapps/2018/08/20/important-dates-regarding-apps-with-windows-phone-8-x-and-earlier-and-windows-8-8-1-packages-submitted-to-microsoft-store/#SzKghBbqDMlmAO4c.97).

## <a name="app-submission-checklist"></a>Liste de vérification relative à la soumission d’une application

Voici la liste des informations que vous pouvez fournir quand vous soumettez votre application, avec des liens vers des informations complémentaires.

Les éléments que vous devez obligatoirement fournir ou spécifier sont signalés ci-dessous. Certains sont facultatifs ou ont des valeurs par défaut que vous pouvez modifier selon vos besoins. Vous n’êtes pas obligé de travailler sur ces sections dans l’ordre indiqué ici.

### <a name="pricing-and-availability-page"></a>Page Tarification et disponibilité
| Nom du champ                    | Remarques                                       | Informations supplémentaires                                                             |
|-------------------------------|---------------------------------------------|---------------------------------------------------------------------------|
| **Markets**                   | Default : Tous les marchés possible  | [Définir la sélection de tarification et de marché](define-pricing-and-market-selection.md)         |
| **Audience**                | Default : Public non privé | [Audience](choose-visibility-options.md#audience) |
| **Détectabilité**                | Default : Rendre cette application disponible et détectable dans le Store | [Détectabilité](choose-visibility-options.md#discoverability) |
| **Planification**                  | Default : Version dès que possible        | [Configurer la planification de la version précise](configure-precise-release-scheduling.md) |
| **Prix de base**                | Obligatoire                                    | [Définir et planifier la tarification d’app](set-and-schedule-app-pricing.md)              |
| **Évaluation gratuite**                | Default : Aucune version d’évaluation gratuite                      | [Évaluation gratuite](set-app-pricing-and-availability.md#free-trial)              |
| **Tarif réduit**              | Facultatif                                    | [Commercialiser des applications et composants additionnels](put-apps-and-add-ons-on-sale.md)           |
| **Licence d’entreprise**    | Default : Autoriser l’acquisition de volume par les organisations | [Options de licence d’organisation](organizational-licensing.md)        |
      |


### <a name="properties-page"></a>Page Propriétés

| Nom du champ                    | Remarques                                       | Informations supplémentaires                                                             |
|-------------------------------|---------------------------------------------|---------------------------------------------------------------------------|
| **Catégorie et sous-catégorie**  | Obligatoire                                    | [Table de catégorie et sous-catégorie](category-and-subcategory-table.md)       |
| **URL de stratégie de confidentialité**            | Obligatoire pour plusieurs applications. Voir le [contrat du développeur d'applications](https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement) et les [stratégies du Microsoft Store](https://docs.microsoft.com/en-us/legal/windows/agreements/store-policies#105-personal-information) | [URL de stratégie de confidentialité](enter-app-properties.md#privacy-policy-url)        |
| **Site Web**                   | Facultatif                                    | [Site Web](enter-app-properties.md#website)                   |
| **Prise en charge les informations de contact**      | Obligatoire si votre produit est disponible sur Xbox ; dans le cas contraire facultatif (mais recommandé)                                   | [Prise en charge les informations de contact](enter-app-properties.md#support-contact-info)              |
| **Paramètres de jeu**             | Facultatif (applicable uniquement aux jeux)         | [Paramètres de jeu](enter-app-properties.md#game-settings) |
| **Mode d’affichage**             | Facultatif                   | [Mode d’affichage](enter-app-properties.md#display-mode) |
| **Déclarations de produit**          | Default : Les clients peuvent installer cette application sur des lecteurs de remplacement ou de stockage amovible ; Windows peut inclure des données de cette application dans les sauvegardes automatiques vers OneDrive | [Déclarations de produit](app-declarations.md) |
| **Configuration système requise**      | Facultatif                                    | [Configuration système requise](enter-app-properties.md#system-requirements)      |

<span/>

### <a name="age-ratings-page"></a>Page Classification par âge

| Nom du champ                    | Remarques                                       | Informations supplémentaires                          |
|-------------------------------|---------------------------------------------|----------------------------------------|
| **Classes d’âge**               | Obligatoire                                    | [Classes d’âge](age-ratings.md)          |

<span/>

### <a name="packages-page"></a>Page Packages

| Nom du champ                    | Remarques                                  | Informations supplémentaires                          |
|-------------------------------|----------------------------------------|----------------------------------------|
| **Contrôle de téléchargement de package**    | Obligatoire (au moins un package)        | [Télécharger des packages d’application](upload-app-packages.md) |
| **Disponibilité de famille de périphériques** | Par défaut : basée sur les packages       | [Disponibilité de famille de périphériques](device-family-availability.md) |
| **Déploiement de package progressive**   | Facultatif (pour les mises à jour uniquement)            | [Déploiement de package progressive](gradual-package-rollout.md) |
| **Mise à jour obligatoire**          | Facultatif (pour les mises à jour uniquement)            | [Mise à jour obligatoire](upload-app-packages.md#mandatory-update)


### <a name="store-listings"></a>Descriptions dans le Windows Store

Vous devez indiquer toutes les informations requises pour au moins l’une des langues prises en charge par votre application. Nous vous recommandons de fournir des [descriptions dans le Windows Store](create-app-store-listings.md) dans toutes les langues prises en charge de votre application, et vous pouvez également [fournir des descriptions dans le Windows Store dans d’autres langues](create-app-store-listings.md#store-listing-languages). Pour faciliter la gestion de plusieurs descriptions pour le même produit, vous pouvez [importer et exporter des descriptions du Store](import-and-export-store-listings.md).

| Nom du champ                    | Remarques                                       | Informations supplémentaires                                                     |
|-------------------------------|---------------------------------------------|-------------------------------------------------------------------|
| **Description**               | Obligatoire                                    | [Rédigez une description de l’application réussie](write-a-great-app-description.md) |
| **Quelles sont les nouveautés dans cette version**   | Facultatif                                 | [Notes de publication](create-app-store-listings.md#whats-new-in-this-version)       |
| **Fonctionnalités de l’application**              | Facultatif                                    | [Fonctionnalités du produit](create-app-store-listings.md#product-features)         |
| **Captures d’écran**               | Obligatoire (au moins une capture d’écran, quatre ou plus recommandées)          | [Captures d’écran](app-screenshots-and-images.md#screenshots)          |
| **Logos de Store**               | Recommandé ; obligatoire pour certaines versions du système d’exploitation | [Logos de Store](app-screenshots-and-images.md#store-logos)             |
| **Codes de fin**                  | Facultatif                                    | [Codes de fin](app-screenshots-and-images.md#trailers)                | 
| **Image de Windows 10 et Xbox (art de Super héros de 16:9)**     | Recommandé        | [Windows 10 et Xbox image (art de Super héros de 16:9)
](app-screenshots-and-images.md#windows-10-and-xbox-image-169-super-hero-art) |
| **Images de Xbox**     | Requis pour l’affichage correct si vous publiez sur Xbox        | [Images Xbox
](app-screenshots-and-images.md#xbox-images) |
| **Champs supplémentaires**  | Facultatif                                    | [Champs supplémentaires](create-app-store-listings.md#supplemental-fields) 
| **Termes de recherche**              | Facultatif                                    | [Termes de recherche](create-app-store-listings.md#search-terms)         |
| **Informations de copyright et de marque** | Facultatif                                 | [Informations de copyright et de marque](create-app-store-listings.md#copyright-and-trademark-info) |
| **Termes du contrat de licence supplémentaires**  | Facultatif                                    | [Termes du contrat de licence supplémentaires](create-app-store-listings.md#additional-license-terms) |
| **Développé par**              | Facultatif                                    | [Développé par](create-app-store-listings.md#developed-by)                   |


<span/>

### <a name="submission-options-page"></a>Page des options de soumission

| Nom du champ                    | Remarques                                       | Informations supplémentaires                                                     |
|-------------------------------|---------------------------------------------|-------------------------------------------------------------------|
| **Options de blocage de publication**     | Default : Publier cette soumission dès qu’il passe de certification (ou par dates que vous avez sélectionné dans la section Planification)      | [Options de blocage de publication](manage-submission-options.md#publishing-hold-options)    
| **Notes relatives à la certification**     | Recommandé          | [Notes relatives à la certification](notes-for-certification.md)             |
| **Fonctionnalités limitées**     | Requis si votre produit déclare les [des capacités restreintes](../packaging/app-capability-declarations.md#restricted-capabilities)    | [Fonctionnalités limitées](manage-submission-options.md#publishing-hold-options)       

<span/>

> [!NOTE]
> Pour plus d’informations sur la publication d’applications métier directement à l’attention des entreprises, voir [Distribuer des applications métier aux entreprises](distribute-lob-apps-to-enterprises.md).
