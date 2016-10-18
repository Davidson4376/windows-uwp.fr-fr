---
author: jnHs
Description: "En gérant vos modules complémentaires en bloc, vous pouvez en modifier plusieurs simultanément, plutôt que de soumettre chaque mise à jour séparément."
title: "Gérer les modules complémentaires en bloc"
translationtype: Human Translation
ms.sourcegitcommit: 3afdf00864e023d913b635beef0c506735881b23
ms.openlocfilehash: 9d387cf3a7850301660a672e3255a762ecd3bd4a


---

# Gérer les modules complémentaires en bloc

> **Important** Pour le moment, cette fonctionnalité est disponible uniquement pour les comptes de développeur qui ont rejoint le [Programme Insider du Centre de développement](dev-center-insider-program.md). L’implémentation de cette fonctionnalité peut évoluer avant qu’elle ne soit disponible pour tous les développeurs. Cette documentation préliminaire fournit certaines informations de base sur le fonctionnement de la fonctionnalité.

En gérant vos modules complémentaires en bloc, vous pouvez en modifier plusieurs simultanément, plutôt que de soumettre chaque mise à jour séparément. Vous pouvez accéder à cette fonctionnalité à partir de la page de vue d’ensemble de votre application en cliquant sur **Gérer les modules complémentaires en bloc**.

## Exportation des informations actuelles sur les modules complémentaires

Pour commencer, vous devez télécharger un fichier de modèle .csv. Si vous avez déjà créé des modules complémentaires, ce fichier inclut des informations les concernant. Si ce n’est pas le cas, il s’agira d’un fichier vide qui vous permettra d’entrer des informations pour les nouveaux modules complémentaires.

Pour générer et télécharger ce fichier de modèle, cliquez sur **Exporter des modules complémentaires** et enregistrez le fichier .csv sur votre ordinateur.

Le fichier .csv contient les colonnes suivantes: 

| Nom de la colonne               | Description                            | Obligatoire?      |
|---------------------------|----------------------------------|----------------------|
| ID de produit    |  [L’ID de produit](set-your-add-on-product-id.md#product-id) unique du module complémentaire.  | Oui. Ne peut pas être modifié après la publication du module complémentaire. |
| Action |L’action que vous souhaitez appliquer lorsque vous importez le modèle. Les valeurs prises en charge sont **Submit** (pour soumettre un nouveau module complémentaire ou mettre à jour un module complémentaire déjà publié) et **CreateDraft** (pour enregistrer les modifications sans les soumettre au WindowsStore). |  Oui |
| Type de produit  | Le [type de produit](set-your-add-on-product-id.md#product-type) du module complémentaire. Les valeurs prises en charge sont **Consumable** ou **Durable**. |   Oui. Ne peut pas être modifié après la publication du module complémentaire. |
| Durée de vie du produit  | Pour un module complémentaire de type Durable, il s’agit soit de **Forever** (pour un produit qui n’expire jamais) soit d’une durée définie. Les valeurs de durée possibles sont: **1day, 3days, 5days, 7days, 14days, 30days, 60days, 90days, 180days, 365days**    | Oui (si le type de produit est Durable) |
| Type de contenu  | Le [type de contenu](enter-add-on-properties.md#content-type) du module complémentaire. Pour la plupart des modules complémentaires, il s’agit de **ElectronicSoftwareDownload**. Les autres valeurs possibles sont: **ElectronicBooks, ElectronicMagazineSingleIssue, ElectronicNewspaperSingleIssue, MusicDownload, MusicStreaming, OnlineDataStorageServices, VideoDownload, VideoStreaming, SoftwareAsAService** |    Oui |
| Balise   | Informations facultatives de [balise](enter-add-on-properties.md#custom-developer-data) (également appelées **données personnalisés du développeur**) utilisées dans l’implémentation de votre application. | Non |
| Prix de base    | Le [niveau de prix](set-add-on-pricing-and-availability.md#base-price) auquel vous souhaitez proposer le module complémentaire. Doit avoir la valeur **Free** ou un niveau de prix valide au format **0.99USD**. | Oui |
| Date de publication  | La date à laquelle vous souhaitez publier le module complémentaire. Les valeurs possibles sont **Immediate**, **Manual**, ou une chaîne de date conforme à la [norme ISO8601](http://go.microsoft.com/fwlink/p/?LinkId=817237). | Oui |
| Titres    | Le nom que les clients verront pour le module complémentaire, précédé du code de langue et d’un point-virgule. Par exemple, pour utiliser le titre «Exemple de titre» en anglais/États-Unis, vous devez saisir *en-us;Exemple de titre*. Séparez les titres supplémentaires pour d’autres langues par des points-virgules. Chaque titre doit comporter au maximum 100caractères.  | Oui |
|Descriptions   | Informations supplémentaires facultatives à présenter aux clients, précédées du code langue-région et d’un point-virgule. Par exemple, pour utiliser la description «Il s’agit d’un exemple» en anglais/États-Unis, vous devez saisir *en-us;Il s’agit d’un exemple*. Séparez les titres supplémentaires pour d’autres langues par des points-virgules. Chaque description doit comporter au maximum 200caractères.    | Non |
| Marchés | Un ou plusieurs [marchés](define-pricing-and-market-selection.md#windows-store-consumer-markets) sur lesquels vous souhaitez proposer le module complémentaire. Séparez les marchés par des points-virgules. |  Oui |
|Mots clés | [Mots-clés](enter-add-on-properties.md#keywords) facultatifs utilisés pour l’implémentation de votre application. | Non |

## Importation des modules complémentaires

Avant de pouvoir importer des modifications, vous devez mettre à jour le fichier .csv téléchargé avec les modifications que vous souhaitez apporter.

Pour modifier les modules complémentaires que vous avez déjà publiés, mettez à jour les valeurs que vous souhaitez modifier dans votre copie de la feuille de calcul. Vous pouvez supprimer les lignes des modules complémentaires que vous ne souhaitez pas mettre à jour, ou les laisser inchangées. Veuillez noter que si ce module complémentaire est déjà en cours de soumission, vous ne serez pas en mesure d’apporter des modifications à l’aide du fichier .csv.

> **Important** Lors de la soumission de mises à jour pour des modules complémentaires que vous avez déjà publiés, vous ne pouvez pas modifier les champs **ID de produit** et **Type de produit**.

Pour soumettre un nouveau module complémentaire, ajoutez une nouvelle ligne et entrez les informations concernant votre nouveau module complémentaire. Veillez à saisir toutes les informations requises. 

Une fois les modifications apportées, enregistrez le fichier .csv (avec le même nom de fichier), puis téléchargez-le en le faisant glisser dans le champ spécifié (ou cliquez sur **Parcourir vos fichiers**). Un résumé des modifications s’affiche, ainsi que toutes les erreurs qui doivent être résolues avant la soumission. Une fois que vous avez vérifié que les informations sont correctes, vous pouvez cliquer sur **Envoyer au Store**. Chaque module complémentaire passera par le processus de soumission en utilisant les informations que vous avez fournies.




<!--HONumber=Aug16_HO3-->


