---
author: jnHs
Description: "En gérant les produits in-app en bloc, vous pouvez modifier plusieurs produits in-app simultanément, plutôt que de soumettre chaque mise à jour séparément."
title: "Gérer des produits in-app en bloc"
ms.sourcegitcommit: 475371dd55aa111f3743c03dc1600e8cfdbeb5b0
ms.openlocfilehash: ae4d4ed33b9bd10a2b01b336c942ad3212de6533


---

# Gérer des produits in-app en bloc

> **Important** Pour le moment, cette fonctionnalité est disponible uniquement pour les comptes de développeur qui ont rejoint le [Programme Insider du Centre de développement](dev-center-insider-program.md). L’implémentation de cette fonctionnalité peut évoluer avant qu’elle ne soit disponible pour tous les développeurs. Cette documentation préliminaire fournit certaines informations de base sur le fonctionnement de la fonctionnalité.

En gérant les produits in-app en bloc, vous pouvez modifier plusieurs produits in-app simultanément, plutôt que de soumettre chaque mise à jour séparément. Vous pouvez accéder à cette fonctionnalité à partir de la page de vue d’ensemble de votre application en cliquant sur **Gérer les produits in-app en bloc**.

## Exporter les informations sur le produit in-app actuel

Pour commencer, vous devez télécharger un fichier de modèle .csv. Si vous avez déjà créé des produits in-app, ce fichier inclut des informations les concernant. Si ce n’est pas le cas, il s’agira d’un fichier vide qui vous permettra d’entrer des informations pour les nouveaux produits in-app. 

Pour générer et télécharger ce fichier de modèle, cliquez sur **Exporter des produits in-app** et enregistrez le fichier .csv sur votre ordinateur.

Le fichier .csv contient les colonnes suivantes: 

| Nom de la colonne               | Description                            | Obligatoire?      |
|---------------------------|----------------------------------|----------------------|
| ID de produit    |  L’[ID de produit](set-your-iap-product-id.md#product-id) unique du produit in-app.  | Oui. Ne peut pas être modifié après la publication du produit in-app. |
| Action |L’action que vous souhaitez appliquer lorsque vous importez le modèle. Les valeurs prises en charge sont **Submit** (pour soumettre un nouveau produit in-app ou mettre à jour un produit in-app déjà publié) et **CreateDraft** (pour enregistrer les modifications sans les soumettre au Windows Store). |    Oui |
| Type de produit  | Le [type du produit](set-your-iap-product-id.md#product-type) in-app. Les valeurs prises en charge sont **Consumable** ou **Durable**. | Oui. Ne peut pas être modifié après la publication du produit in-app. |
| Durée de vie du produit  | Pour un produit in-app Durable, il s’agit soit de **Forever** (pour un produit qui n’expire jamais) ou d’une durée définie. Les valeurs de durée possibles sont: **1day, 3days, 5days, 7days, 14days, 30days, 60days, 90days, 180days, 365days**   | Oui (si le type de produit est Durable) |
| Type de contenu  | Le [type de contenu](enter-iap-properties.md#content-type) du produit in-app. Pour la plupart des produits in-app, il s’agit de **ElectronicSoftwareDownload**. Les autres valeurs possibles sont: **ElectronicBooks, ElectronicMagazineSingleIssue, ElectronicNewspaperSingleIssue, MusicDownload, MusicStreaming, OnlineDataStorageServices, VideoDownload, VideoStreaming, SoftwareAsAService** | Oui |
| Balise   | [Balise](enter-iap-properties.md#tag) facultative utilisée pour l’implémentation de votre application. | Non |
| Prix de base    | Le [niveau de prix](set-iap-pricing-and-availability.md#base-price) auquel vous souhaitez proposer le produit in-app. Doit avoir la valeur **Free** ou un niveau de prix valide au format **0.99USD**. |   Oui |
| Date de publication  | La date à laquelle vous souhaitez publier le produit in-app. Les valeurs possibles sont **Immediate**, **Manual**, ou une chaîne de date conforme à la [norme ISO8601](http://go.microsoft.com/fwlink/p/?LinkId=817237). | Oui |
| Titres    | Le nom que les clients verront pour le produit in-app, précédé par le code de langue et un point-virgule. Par exemple, pour utiliser le titre «Exemple de titre» en anglais/États-Unis, vous devez saisir *en-us;Exemple de titre*. Séparez les titres supplémentaires pour d’autres langues par des points-virgules. Chaque titre doit comporter au maximum 100caractères.     | Oui |
|Descriptions   | Informations supplémentaires facultatives à présenter aux clients, précédées du code langue-région et d’un point-virgule. Par exemple, pour utiliser la description «Il s’agit d’un exemple» en anglais/États-Unis, vous devez saisir *en-us;Il s’agit d’un exemple*. Séparez les titres supplémentaires pour d’autres langues par des points-virgules. Chaque description doit comporter au maximum 200caractères.    | Non |
| Marchés | Un ou plusieurs [marchés](define-pricing-and-market-selection.md#windows-store-consumer-markets) sur lesquels vous souhaitez proposer le produit in-app. Séparez les marchés par des points-virgules. | Oui |
|Mots clés | [Mots-clés](enter-iap-properties.md#keywords) facultatifs utilisés pour l’implémentation de votre application. | Non |

## Importer des produits in-app

Avant de pouvoir importer des modifications, vous devez mettre à jour le fichier .csv téléchargé avec les modifications que vous souhaitez apporter.

Pour modifier les produits in-app que vous avez déjà publiés, mettez à jour les valeurs que vous souhaitez modifier dans votre copie de la feuille de calcul. Vous pouvez supprimer les lignes des produits in-app que vous ne souhaitez pas mettre à jour, ou les laisser inchangées. Veuillez noter que si ce produit in-app est déjà en cours de soumission, vous ne serez pas en mesure d’apporter des modifications à l’aide du fichier .csv.

> **Important** Lors de la soumission de mises à jour pour des produits in-app que vous avez déjà publiés, vous ne pouvez pas modifier les champs **ID de produit** et **Type de produit**.

Pour soumettre un nouveau produit in-app, ajoutez une nouvelle ligne et entrez les informations concernant votre nouveau produit in-app. Veillez à saisir toutes les informations requises. 

Une fois les modifications apportées, enregistrez le fichier .csv (avec le même nom de fichier), puis téléchargez-le en le faisant glisser dans le champ spécifié (ou cliquez sur **Parcourir vos fichiers**). Un résumé des modifications s’affiche, ainsi que toutes les erreurs qui doivent être résolues avant la soumission. Une fois que vous avez vérifié que les informations sont correctes, vous pouvez cliquer sur **Envoyer au Store**. Chaque produit in-app passera par le processus de soumission en utilisant les informations que vous avez fournies.




<!--HONumber=Jun16_HO4-->


