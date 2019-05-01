---
Description: Le rapport d’intégrité dans Partner Center vous permet d’obtenir les données relatives aux performances et qualité de votre application, y compris les incidents et les événements qui ne répond pas.
title: Rapport sur l’intégrité
ms.assetid: 4F671543-1E91-4E59-88A3-638E3E64539A
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, intégrité, incidents, blocages, intégrité de l’application, données d’intégrité, trace de pile, fichier cab, échec, échecs, pdb, symboles
ms.localizationpriority: medium
ms.openlocfilehash: 3c9e58cae32bc5303222deecab2cc470e0b68b71
ms.sourcegitcommit: fca0132794ec187e90b2ebdad862f22d9f6c0db8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/24/2019
ms.locfileid: "63790833"
---
# <a name="health-report"></a>Rapport sur l’intégrité

Le **intégrité** signaler dans [partenaires](https://partner.microsoft.com/dashboard) permet d’obtenir des données relatives aux performances et qualité de votre application, y compris les incidents et les événements qui ne répond pas. Vous pouvez afficher ces données dans le centre de partenaires, ou [télécharger le rapport](download-analytic-reports.md) à afficher en mode hors connexion. Le cas échéant, vous pouvez visualiser les traces de pile et/ou les fichiers CAB pour un débogage supplémentaire.

Vous pouvez également récupérer par programme les données de ce rapport à l’aide de l’[API REST d’analyse du Microsoft Store](../monetize/access-analytics-data-using-windows-store-services.md).


## <a name="apply-filters"></a>Appliquer les filtres

Dans la zone supérieure de la page, vous pouvez sélectionner la période sur laquelle portent les données qui vous intéressent. La valeur par défaut est de **72H** (72 heures), mais vous pouvez choisir **30D** à la place si vous souhaitez afficher les données relatives aux 30 derniers jours. Notez que les données s’affichent dans votre fuseau horaire local de le **72 heures** vue et au format UTC pour le **30D** vue.

Vous pouvez également développer l’option **Filtres** pour filtrer toutes les données de cette page par version de package, par marché et/ou par type d’appareil.

-   **Version du package**: Le paramètre par défaut est **tous les**. Si votre application comporte plusieurs packages, vous pouvez en choisir un ici.
-   **Marché**: Le filtre par défaut est **tous les marchés**, mais vous pouvez limiter les données à un ou plusieurs marchés.
-   **Type d’appareil**: Le paramètre par défaut est **tous les**, mais vous pouvez choisir d’afficher les données pour qu’un seul type d’appareil spécifique. Notez que la catégorie **Autre** inclut les appareils dont la marque/le modèle est reconnu(e), mais que nous ne sommes pas en mesure d’inclure dans une des catégories prédéfinies pour ce filtre. Pour ces appareils, le modèle d’appareil peut être affiché dans la section **Journal des échecs** du rapport **Détails de l’échec**.  
-   **Version du système d’exploitation**: La valeur par défaut est **versions tous les systèmes d’exploitation**, mais vous pouvez choisir une version du système d’exploitation spécifique.
-   **Système d’exploitation de version**: La valeur par défaut est **système d’exploitation de toutes les versions commerciales**, mais vous pouvez choisir une version spécifique du **version du système d’exploitation**.
-   **Bac à sable**: La valeur par défaut est **Retail**, mais pour les produits qui utilisent plusieurs bacs à sable de développement (par exemple, des jeux qui s’intègrent avec Xbox Live), vous pouvez choisir ici une valeur spécifique. (Si votre produit n’utilise pas de bacs à sable, ce filtre affichera uniquement **Retail** et ne sera pas applicable.)
-   **Architecture**: La valeur par défaut est **toutes les architectures**, mais vous pouvez choisir un type d’architecture système spécifique. Ce filtre est disponible uniquement lorsque **30D** est sélectionné.
-   **PRAID**: Le paramètre par défaut est **tous les**, mais si vous avez défini plusieurs package relative ID d’application (PRAIDs) lors de la création de votre package d’application, vous pouvez choisir d’afficher uniquement les données associées à un seul PRAID. Ce filtre n’apparaît pas si vous n’avez pas défini plusieurs PRAID.

Les informations figurant dans tous les graphiques répertoriés ci-après correspondent à la plage de dates et à tous les filtres que vous avez sélectionnés. Certaines sections vous permettent également d’appliquer des filtres supplémentaires.


## <a name="failure-hits"></a>Occurrences des échecs

Le graphique **Occurrences des échecs** affiche le nombre d’incidents et d’événements quotidiens recensés par les clients sur votre application au cours de la période sélectionnée. Les différents événements survenus dans votre application font l’objet d’un suivi par type : incidents, absences de réponse, exceptions JavaScript et défaillances mémoire.

Lorsque le **30D** période de temps est sélectionnée, vous pouvez voir les marqueurs de cercle. Ceux-ci représentent une augmentation significative ou diminuent en une valeur de donnée qui nous pensons que vous souhaitez savoir sur. La date sur lequel apparaît le cercle représente la fin de la semaine dans lequel nous avons détecté une augmentation significative ou une baisse par rapport à la semaine auparavant. Pour afficher plus de détails sur ce qui a changé, pointez sur le cercle.  

> [!TIP]
> Vous pouvez afficher davantage d’insights relatifs à des modifications significatives sur les 30 derniers jours dans le [rapport Insights](insights-report.md).

## <a name="failure-hits-by-market"></a>Occurrences des échecs par marché

Le graphique **Occurrences des échecs par marché** présente le nombre total d’incidents et d’événements survenus par marché au cours de la période sélectionnée.

Vous pouvez visualiser ces données sous la forme visuelle **Carte** ou **Tableau**. La représentation sous forme de tableau affiche cinq marchés à la fois, soit dans l’ordre alphabétique, soit par nombre maximal ou minimal de sessions utilisateur. Vous pouvez également télécharger ces données afin de visualiser d’un seul coup d’œil les informations relatives à la totalité des marchés.


## <a name="package-version"></a>Version du package

Le graphique **Version du package** présente le nombre total d’incidents et d’événements survenus par version du package au cours de la période sélectionnée. Par défaut, la version du package ayant donné lieu au plus grand nombre d’occurrences apparaît en premier dans le graphique. Vous pouvez inverser cet ordre en cliquant sur la flèche dans la colonne **Occurrences** de ce graphique.

## <a name="failures"></a>Échecs

Le graphique **Échecs** présente le nombre total d’incidents et d’événements survenus par nom d’échec au cours de la période sélectionnée. Chaque nom d’échec est constitué de quatre parties : une ou plusieurs classes de problème, un code de vérification d’exception/d’erreur, le nom de l’image/du pilote où l’échec s’est produit et le nom de la fonction associée. Par défaut, l’échec ayant donné lieu au plus grand nombre d’occurrences apparaît en premier dans le graphique. Vous pouvez inverser cet ordre en cliquant sur la flèche dans la colonne **Occurrences** de ce graphique. Pour chaque échec, le pourcentage du nombre total d’échecs apparaît également.

> [!TIP]
> Dans certains cas, une entrée peut être affichée pour **Inconnu** dans cette section. Cela se produit lorsqu’en dépit de nos efforts nous ne pouvons pas collecter tous les détails pour un ou plusieurs échecs, qui seront tous regroupés sous **Inconnu**. Le plus souvent, cela se produit en raison de contraintes de stockage, mais cela peut également être le résultat de paramètres de confidentialité d’un appareil, de problèmes de connexion réseau, de vidages sur incident partiels/incorrects ou d’autres facteurs.
>
> Si vous voyez **!unknown** dans un nom d’échec, cela signifie que les symboles n’étaient pas présents et que nous n’avons pas pu identifier le nom d’échec. Veillez à inclure les symboles dans votre package pour obtenir une analyse précise des échecs. Voir [Configurer un package d’application](../packaging/packaging-uwp-apps.md#configure-an-app-package). En revanche, si les noms d’échec incluent **!unknown_error_in_** et **!unknown_function**, cela signifie que nous n’avons pas pu collecter des informations complètes pour diverses autres raisons.

Pour afficher le rapport **Détails de l’échec** d’un échec particulier, sélectionnez le nom de l’échec. Si vous avez inclus des fichiers de symboles, le rapport **Détails de l’échec** inclut le nombre d’occurrences de l’échec au cours du dernier mois, ainsi qu’un journal des échecs répertoriant les détails relatifs aux occurrences (date, version du package, type d’appareil, modèle d’appareil, version de système d’exploitation) et un lien vers la trace de la pile et/ou le fichier CAB, s’il est disponible.

> [!TIP]
> Les fichiers CAB sont uniquement disponibles lorsque l’échec s’est produit sur un ordinateur utilisant la build Windows Insider. Par conséquent, certains échecs n’incluront pas l’option de téléchargement de fichiers CAB. Pour afficher uniquement les échecs qui ont des fichiers CAB, sélectionnez **les échecs de téléchargements** dans le filtre de section. Vous pouvez également cliquer sur le **liens** en-tête dans le **journal des échecs** pour trier les résultats afin que les défaillances qui incluent des fichiers CAB apparaissent en haut de la liste.

Sur le **détails de l’échec** page, vous verrez également le **pile prévalence** graphique qui affiche les piles supérieurs qui a contribué à l’échec, classé par pourcentage, et le **appareil configuration (30D)** graphique, qui fournit des détails sur la configuration des périphériques qui les a échoué. 


## <a name="crash-free-sessions-and-devices-30d"></a>Exempt de blocage des sessions et les périphériques (30D)

Le **exempt de blocage des sessions et les périphériques** graphique affiche le pourcentage d’appareils ou les sessions utilisateur qui n’a pas rencontré un incident au cours des 30 derniers jours. Ces informations vous aidera à comprendre comment largement vos incidents affectent vos utilisateurs. Par exemple, une application peut avoir 10 000 incidents dans une journée. Si 90 % de vos appareils sont affectés, puis vous probablement que classer comme critiques et agir pour corriger ce problème tout de suite. Toutefois, si qui représente uniquement les 5 % des appareils à l’aide de votre application, la priorité peut être inférieure.

Ce graphique comporte deux onglets :
- **Appareils libre d’incident**: Affiche le pourcentage d’appareils uniques qui n’a pas rencontré un échec sur chaque jour (au cours des 30 derniers jours).
- **Incident exempt de sessions**: Affiche le pourcentage de sessions utilisateur unique qui n’a pas rencontré un échec sur chaque jour (au cours des 30 derniers jours).


 

 
