---
author: jnHs
Description: "Après avoir créé votre application en réservant un nom, vous pouvez commencer à vous occuper de sa publication. La première étape consiste à créer une soumission."
title: "Soumissions d’application"
ms.assetid: 363BB9E4-4437-4238-A80F-ABDFC70D96E4
keywords: "liste de vérification"
translationtype: Human Translation
ms.sourcegitcommit: 5f975d0a99539292e1ce91ca09dbd5fac11c4a49
ms.openlocfilehash: ce9858da15cac0e34a2bb2c68c25ba63ec79af4c

---

# Soumissions d’application


Après avoir [créé votre application en réservant un nom](create-your-app-by-reserving-a-name.md), vous pouvez commencer à vous occuper de sa publication. La première étape consiste à créer une **soumission**.

Vous pouvez démarrer votre soumission lorsque votre application est terminée et prête pour publication, ou commencer à entrer des informations avant même d’avoir écrit la moindre ligne de code. Votre soumission est enregistrée sur votre tableau de bord. Vous pouvez y travailler à votre meilleure convenance.

Une fois votre application publiée, vous pouvez publier une version mise à jour en créant une autre soumission dans le tableau de bord. Le fait de créer une soumission vous permet d'introduire et de publier tous les changements nécessaires, que vous chargiez d'autres packages ou que vous changiez juste des détails comme le prix ou la catégorie. Pour créer une soumission pour une application, cliquez sur **Mettre à jour** en regard de la soumission plus récente affichée sur la page de présentation de l’application.

> **Remarque**  Cette section de la documentation explique comment créer une soumission d’application sur le tableau de bord du Centre de développement. Sinon, vous pouvez utiliser [l’API de soumission du Windows Store](../monetize/create-and-manage-submissions-using-windows-store-services.md) pour automatiser les soumissions d’application.

## Liste de vérification relative à la soumission d’une application


Voici la liste des informations que vous pouvez fournir quand vous soumettez votre application, avec des liens vers des informations complémentaires.

Les éléments que vous devez obligatoirement fournir ou spécifier sont signalés ci-dessous. Certains sont facultatifs ou ont des valeurs par défaut que vous pouvez modifier selon vos besoins.

### Page Tarification et disponibilité
| Nom du champ                    | Remarques                                       | Informations supplémentaires                                                             |
|-------------------------------|---------------------------------------------|---------------------------------------------------------------------------|
| **Prix de base**                | Obligatoire                                    | [Prix de base](set-app-pricing-and-availability.md#base-price)              |
| **Évaluation gratuite**                | Par défaut : aucune version d'évaluation gratuite                      | [Ajout de versions d’évaluation et d’achats dans l’application](https://msdn.microsoft.com/library/windows/apps/jj193599)  |
| **Marchés et prix personnalisés** | Par défaut : tous les marchés possibles, aucune tarification personnalisée | [Définition des prix et sélection du marché](define-pricing-and-market-selection.md)              |
| **Prix de vente**              | Facultatif                                    | [Commercialiser des applications et composants additionnels](put-apps-and-add-ons-on-sale.md)                                       |
| **Distribution et visibilité** | Par défaut : Rendre votre application accessible dans le Windows Store | [Distribution et visibilité](set-app-pricing-and-availability.md#distribution-and-visibility) |
| **Gestion des licences organisationnelles**    | Par défaut : autoriser l'acquisition en volume par des organisations | [Options de gestion des licences organisationnelles](organizational-licensing.md)                        |
| **Date de publication**                | Par défaut : publication dès que possible      | [Date de publication](set-app-pricing-and-availability.md#publish-date)          |

<span/>

### Page Propriétés de l’application

| Nom du champ                    | Remarques                                       | Informations supplémentaires                                                             |
|-------------------------------|---------------------------------------------|---------------------------------------------------------------------------|
| **Catégorie et sous-catégorie**  | Obligatoire                                    | [Tableau des catégories et sous-catégories](category-and-subcategory-table.md)       |
| **Configuration système**      | Facultatif                                    | [Configuration système](enter-app-properties.md#system-requirements)      |
| **Déclarations d’application**          | Par défaut : les clients peuvent installer cette application sur un autre lecteur ou dispositif de stockage. Windows peut inclure les données de cette application dans les sauvegardes automatiques sur OneDrive | [Déclarations d’application](app-declarations.md) |

<span/>

### Page Classification par âge

| Nom du champ                    | Remarques                                       | Informations supplémentaires                          |
|-------------------------------|---------------------------------------------|----------------------------------------|
| **Classification par âge**               | Obligatoire                                    | [Classification par âge](age-ratings.md)          |

<span/>

### Page Packages

| Nom du champ                    | Remarques                                  | Informations supplémentaires                          |
|-------------------------------|----------------------------------------|----------------------------------------|
| **Contrôle du chargement des packages**    | Obligatoire (au moins un package)        | [Chargement des packages d’application](upload-app-packages.md) |
| **Disponibilité de la famille d’appareils** | Par défaut : basée sur les packages       | [Disponibilité de la famille d’appareils](upload-app-packages.md#device-family-availability) |
| **Lancement de package progressif**   | Facultatif (pour les mises à jour uniquement)            | [Lancement de package progressif](gradual-package-rollout.md) |
| **Mise à jour obligatoire**          | Facultatif (pour les mises à jour uniquement)            | [Mise à jour obligatoire](upload-app-packages.md#mandatory-update)

<span/>

### Descriptions dans le Windows Store

Vous devez indiquer toutes les informations requises pour au moins l’une des langues prises en charge par votre application. Nous vous recommandons de fournir des [descriptions dans le Windows Store](create-app-store-listings.md) dans toutes les langues prises en charge de votre application, et vous pouvez également [fournir des descriptions dans le Windows Store dans d’autres langues](create-app-store-listings.md#store-listing-languages).

| Nom du champ                    | Remarques                                       | Informations supplémentaires                                                     |
|-------------------------------|---------------------------------------------|-------------------------------------------------------------------|
| **Description**               | Obligatoire                                    | [Rédaction d'une description convaincante de l'application](write-a-great-app-description.md) |
| **Notes de publication**             | Facultatif                                    | [Notes de publication](create-app-store-listings.md#release-notes)         |
| **Captures d’écran**               | Obligatoire (au moins une capture d'écran)          | [Images et captures d’écran de l’application](app-screenshots-and-images.md)       |
| **Icône de vignette d’application**             | Facultatif, mais fortement recommandé pour Windows Phone 8.1 et versions antérieures | [Icône de vignette d’application](create-app-store-listings.md#app-tile-icon) |
| **Illustrations publicitaires**       | Facultatif                                    | [Images et captures d’écran de l’application](app-screenshots-and-images.md)       |
| **Fonctionnalités de l’application**              | Facultatif                                    | [Fonctionnalités](create-app-store-listings.md#app-features)               |
| **Configuration système supplémentaire requise**      | Facultatif                                    | [Configuration système supplémentaire requise](create-app-store-listings.md#additional-system-requirements) |
| **Mots clés**                  | Facultatif                                    | [Mots clés](create-app-store-listings.md#keywords)                   |
| **Informations de copyright et de marque déposée** | Facultatif                                 | [Informations de copyright et de marque déposée](create-app-store-listings.md#copyright-and-trademark-info) |
| **Termes de licence supplémentaires**  | Facultatif                                    | [Termes de licence supplémentaires](create-app-store-listings.md#additional-license-terms) |
| **Site web**                   | Facultatif                                    | [Site web](create-app-store-listings.md#website)                     |
| **Coordonnées du support technique**      | Facultatif                                    | [Coordonnées du support technique](create-app-store-listings.md)                |
| **Politique de confidentialité**            | Obligatoire pour certaines applications. Voir le [contrat du développeur d'applications](https://msdn.microsoft.com/library/windows/apps/hh694058) et les [stratégies du Windows Store](https://msdn.microsoft.com/library/windows/apps/dn764944.aspx#pol_10_5_1) | [Politique de confidentialité](create-app-store-listings.md#privacy-policy) |
| **Descriptions dans le Windows Store spécifiques à la plateforme** | Facultatif                               | [Créer des descriptions dans le Windows Store spécifiques à la plateforme](create-platform-specific-store-listings.md) |

<span/>

### Remarques pour la page de certification

| Nom du champ                    | Remarques                                       | Informations supplémentaires                                                     |
|-------------------------------|---------------------------------------------|-------------------------------------------------------------------|
| **Remarques**                     | Facultatif                                    | [Notes de certification](notes-for-certification.md)             |

<span/>

**Remarque**  Pour plus d’informations sur la publication d’applications métier directement à l’attention des entreprises, voir [Distribuer des applications métier aux entreprises](distribute-lob-apps-to-enterprises.md).



<!--HONumber=Aug16_HO5-->


