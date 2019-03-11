---
Description: Le rapport d’Acquisitions dans Partner Center vous permet de voir qui a acquis et installé votre application, ainsi que des données démographiques et les détails de la plateforme.
title: Rapport sur les acquisitions
ms.assetid: 21126362-F3CD-4006-AD3F-82FC88E3B862
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, acquisitions, ventes d’applications, téléchargements d'applications, installations, entonnoir, acquisition, conversions, canal, vues de pages d'applications
ms.localizationpriority: medium
ms.openlocfilehash: 33d5885c5161793807bf32f62ff2df4bab5b2c1d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57653064"
---
# <a name="acquisitions-report"></a>Rapport sur les acquisitions


Le **Acquisitions** signaler dans [partenaires](https://partner.microsoft.com/dashboard) vous permet de voir qui a acquis et installé votre application, ainsi que des données démographiques et des détails de la plateforme et affiche des informations sur la façon les clients sur Windows 10 (y compris Xbox) sont arrivés à la liste de votre application. Vous pouvez également afficher près de données d’acquisition en temps réel pour la dernière période ou soixante-dix deux heures. 

Vous pouvez afficher ces données dans le centre de partenaires, ou [télécharger le rapport](download-analytic-reports.md) à afficher en mode hors connexion. Vous pouvez également récupérer ces données par programme à l’aide de notre [API REST d’analyse](../monetize/access-analytics-data-using-windows-store-services.md).

Dans ce rapport, une **acquisition** signifie qu’un nouveau client a obtenu une licence pour votre application (qu’elle soit payante ou gratuite). Une **installation** fait référence à l’application installée sur un appareil Windows 10.

> [!IMPORTANT]
> Le **Acquisitions** rapport n’inclut pas les données sur les remboursements, retournements, facturation, etc. Pour estimer le résultat de l’application, visitez [synthèse des paiements](payout-summary.md). Dans la section **Réservé**, cliquez sur le lien **Télécharger les transactions réservées**.
>
> À l’exception des données sur les vues de page (comme décrit ci-dessous), ce rapport n’inclut pas les données relatives aux clients qui acquièrent une application sans être connectés à un compte Microsoft.


## <a name="apply-filters"></a>Appliquer les filtres

Dans la zone supérieure de la page, vous pouvez sélectionner la période sur laquelle portent les données qui vous intéressent. La valeur par défaut est de **30D** (30 jours), mais vous pouvez choisir d’afficher les données portant sur des périodes de 3, 6 ou 12 mois, ou sur une plage de dates personnalisée que vous spécifiez. Quasiment en temps réel les données seront affichées pour toutes les options (sauf dans **application cumulative** données). Le **1H** et **72 heures** périodes de temps s’appliquent uniquement au **application quotidiennement** onglet de la **Acquisitions** graphique et en le  **Acquisitions** onglet de la **marchés** graphique. 

Vous pouvez également développer l’option **Filtres** pour filtrer toutes les données de cette page par marché et/ou par type d’appareil.

-   **Marché**: Le filtre par défaut est **tous les marchés**, mais vous pouvez limiter les données à des acquisitions dans un ou plusieurs marchés.
-   **Type d’appareil**: Le paramètre par défaut est **tous les appareils**. Si vous souhaitez afficher les données d’acquisitions relatives à un certain type d’appareil uniquement (PC, console ou tablette, par exemple), vous pouvez en choisir un ici.

Les informations figurant dans tous les graphiques répertoriés ci-après correspondent à la plage de dates et à tous les filtres que vous avez sélectionnés. Certaines sections vous permettent également d’appliquer des filtres supplémentaires.


## <a name="acquisitions"></a>Acquisitions

Le graphique **Acquisitions** affiche le nombre total d’acquisitions quotidiennes ou hebdomadaires (chaque fois qu’un nouveau client obtient une licence pour votre application) au cours de la période sélectionnée. (Lorsque vous utilisez l’option **Appliquer des filtres** afin d’afficher les données portant sur une période plus étendue, les données d’acquisition sont regroupées par semaine.) Uniquement les acquisitions effectuées par les clients qui sont connectés avec un compte Microsoft valide sont incluses dans ce graphique. 

Par défaut, nous affichons le **application quotidiennement** vue, qui comprend près de données en temps réel. Vous pouvez également visualiser le nombre total d’acquisitions sur toute la durée de vie de votre application en sélectionnant **App cumulative**. Vous avez alors accès au total cumulé de l'ensemble des acquisitions effectuées depuis la première publication de votre application.

**Chiffre d’affaires brut** pour votre application (à partir d’octobre 2016 - présents) sont également disponibles dans ce graphique, indiquant le volume total des coûts de ventes d’applications (en USD). Notez que cette quantité ne tient pas compte pour n’importe quel remboursements retournements, rétrofacturation, etc.

Si vous le souhaitez, vous pouvez filtrer les résultats par acquisition émanant du client ou du Windows Store sur le web et/ou par version du système d’exploitation.

> [!NOTE]
> Vous pouvez également récupérer ces données par programme à l’aide de la méthode [Obtenir des acquisitions d’applications](../monetize/get-app-acquisitions.md) dans notre [API REST d’analyse](../monetize/access-analytics-data-using-windows-store-services.md).

Dans le **application quotidiennement** vue, lorsque le **30D** période de temps est sélectionnée, vous pouvez voir les marqueurs de cercle. Ceux-ci représentent une augmentation significative ou diminuent en une valeur de donnée qui nous pensons que vous souhaitez savoir sur. La date sur lequel apparaît le cercle représente la fin de la semaine dans lequel nous avons détecté une augmentation significative ou une baisse par rapport à la semaine auparavant. Pour afficher plus de détails sur ce qui a changé, pointez sur le cercle.  

> [!TIP]
> Vous pouvez afficher davantage d’insights relatifs à des modifications significatives sur les 30 derniers jours dans le [rapport Insights](insights-report.md).

## <a name="installs"></a>Installations

Le graphique **Installations** affiche le nombre de fois où nous avons détecté que des clients ont installé votre application sur des appareils Windows 10 (y compris sur des consoles Xbox One) au cours de la période sélectionnée. Nous vous présentons le nombre total d’installations, ainsi qu’un graphique affichant le nombre d’installations par jour ou par semaine (selon la durée que vous avez sélectionnée). Si vous le souhaitez, vous pouvez filtrer les résultats par version de package spécifique.

Le nombre total d’installations comprend les éléments suivants :
-   **Installe sur plusieurs appareils Windows 10.** Par exemple, si un même client installe votre application sur deux PC Windows 10 et sur une console Xbox One, trois installations sont comptabilisées.
-   **Réinstalle.** Par exemple, si un client installe votre application aujourd’hui, la désinstalle demain puis la réinstalle le mois prochain, 2 installations sont consignées.

Le total ne comprend pas les éléments suivants :
-   **Installe sur les appareils Windows 10.** Si votre application prend en charge des versions antérieures du système d’exploitation, telles que Windows 8.x ou Windows Phone 8.x, nous ne comptabilisons pas les installations sur ces appareils.
-   **Désinstalle.** Lorsqu’un client désinstalle votre application de son appareil, nous ne le soustrayons pas du nombre total d’installations.
-   **Met à jour.** Par exemple, si un client installe votre application à une date donnée, puis installe une mise à jour une semaine plus tard, nous ne comptabilisons qu’une seule installation.
-   **Sont préinstallés.** Si un client achète un appareil sur lequel votre application est préinstallée, nous ne le comptabilisons pas comme une installation.
-   **Initiée par le système installe.** Si Windows installe votre application automatiquement, pour quelque raison que ce soit, nous ne le comptabilisons pas comme une installation.

> [!NOTE]
> Vous pouvez également récupérer ces données par programme à l’aide de la méthode [Obtenir des installations d’application](../monetize/get-app-installs.md) dans notre [API REST d’analyse](../monetize/access-analytics-data-using-windows-store-services.md).

## <a name="acquisition-funnel"></a>Entonnoir d’acquisition

Le graphique **Entonnoir d’acquisition** vous indique le nombre de clients ayant suivi chaque étape de l’entonnoir, depuis la visualisation de la page du Windows Store jusqu’à l’utilisation de l’application, ainsi que le taux de conversion. Ces données peuvent vous aider à identifier les zones dans lesquelles vous pouvez vous investir davantage pour augmenter le nombre d’acquisitions, d’installations ou d’utilisations de votre application.

> [!IMPORTANT]
> La section **Entonnoir d’acquisition** affiche uniquement les données relatives aux clients utilisant Windows 10 (y compris Xbox) et portant sur les 90 derniers jours.

Les étapes de l’entonnoir sont les suivantes :

- **Affichages de page**: Ce nombre représente le nombre total de visites de l’objet de Store de votre application, y compris les personnes qui ne sont pas connecté avec un compte Microsoft. Cette valeur ne prend pas en compte les clients qui ont choisi de ne pas fournir ces informations à Microsoft.
- **Acquisitions**: Le nombre de nouveaux clients qui ont obtenu une licence pour votre application (lorsqu’il est connecté avec leur compte Microsoft) dans les 48 heures d’affichage de son référencement Store.
- **Installe**: Le nombre de clients qui ont installé l’application après son achat.
- **utilisation**: Le nombre de clients qui utilisaient l’application après l’avoir installé.

Si vous le souhaitez, vous pouvez filtrer les résultats par sexe et/ou par âge, ainsi que par ID de campagne personnalisée.

> [!NOTE]
> Vous pouvez également récupérer ces données par programme à l’aide de la méthode [Obtenir les données relatives à l’entonnoir d’acquisition d’applications](../monetize/get-acquisition-funnel-data.md) dans notre [API REST d’analyse](../monetize/access-analytics-data-using-windows-store-services.md).

## <a name="markets"></a>Marchés

Le graphique **Marchés** indique le nombre total d’acquisitions ou d’installations au cours de la période sélectionnée pour chaque marché dans lequel votre application est disponible. Vous pouvez choisir d’afficher les données concernant les **Acquisitions** ou les **Installations**.

Vous pouvez visualiser ces données sous la forme visuelle **Carte** ou **Tableau**. La représentation sous forme de tableau affiche cinq marchés à la fois, soit dans l’ordre alphabétique, soit par nombre maximal ou minimal d’acquisitions ou d’installations. Vous pouvez également télécharger ces données afin de visualiser d’un seul coup d’œil les informations relatives à la totalité des marchés.


## <a name="customer-demographic"></a>Données démographiques sur les clients

Le graphique **Données démographiques sur les clients** présente des informations démographiques sur les personnes ayant acheté votre application. Vous pouvez voir le nombre d’acquisitions (au cours de la période sélectionnée) effectuées par des personnes appartenant à un certain groupe d’âge et par sexe.

> [!NOTE]
> Certains clients ont choisi de ne pas partager ces informations. Si nous n’avons pas pu déterminer le groupe d’âge ni le sexe, l’acquisition entre dans la catégorie **Inconnu**.

 

## <a name="app-page-views-and-conversions-by-channel"></a>Vues et conversions de la page de l’application par canal

Le **affichages de pages d’application et les conversions par canal** graphique vous permet de voir comment les clients sur Windows 10 est arrivé à la liste de votre application pour la période de temps sélectionnée.

Dans ce graphique, un *canal* désigne la méthode à l’aide de laquelle un client a accédé à la page de description de votre application (par exemple, navigation et recherche dans le Windows Store, lien à partir d’un site web externe, lien figurant dans l’une de vos campagnes personnalisées, etc.). Les canaux suivants sont inclus :

-   **Trafic de Store :** Le client a été de navigation ou de recherche dans le Store lorsqu’ils affichent la liste de votre application.
-   **Campagne personnalisée :** Le client a suivi un lien qui a utilisé un [ID de campagne personnalisé](create-a-custom-app-promotion-campaign.md).
-   **Autres :** Le client suivi un lien externe (sans les ID de campagne personnalisée) à partir d’un site Web à la liste de votre application ou le client a suivi un lien à partir d’un moteur de recherche à la liste de votre application.

Un *mode page* signifie qu’un client a consulté votre page de l’application Store listing, via le Store basée sur le web ou depuis l’app Store sur Windows 10. Ce nombre inclut les vues par des utilisateurs qui ne sont pas connectés avec un compte Microsoft. Certains clients ont choisi de ne pas fournir ces informations à Microsoft.

Une *conversion* signifie qu’un nouveau client (connecté avec un compte Microsoft) a obtenu une licence pour votre application (qu’elle soit payante ou gratuite).

Les nombres de vues de page et de conversions ne correspondent pas à des nombres de clients uniques. Pour plus d’informations sur les taux de conversion, consultez le graphique [Entonnoir d’acquisition](#acquisition-funnel).

> [!NOTE]
> Il est possible que des clients soient arrivés à la description de votre application en cliquant sur une campagne personnalisée qui n’a pas été créée par vos soins. Nous marquons chaque vue de page dans une session avec l’ID de campagne à partir duquel l’utilisateur a accédé au Windows Store. Puis nous attribuons à cet ID de campagne les conversions appropriées parmi toutes les acquisitions effectuées dans les 24 heures. Par conséquent, le nombre total de conversions peut être plus élevé que celui de vos ID de campagne, et certaines de vos conversions ou conversions d’extension peuvent présenter un nombre de vues de page nul.

> [!NOTE]
> Vous pouvez également récupérer ces données par programme à l’aide de la méthode [Obtenir les conversions d’applications par canal](../monetize/get-app-conversions-by-channel.md) dans notre [API REST d’analyse](../monetize/access-analytics-data-using-windows-store-services.md).

## <a name="app-page-views-and-conversions-by-campaign-id"></a>Vues et conversions de la page de l’application par ID de campagne

Le graphique **Conversions et accès aux pages de l’application par ID de campagne** vous permet d’effectuer le suivi des conversions et des vues de page, comme décrit ci-dessus, pour chacune de vos [campagnes de promotion personnalisées](create-a-custom-app-promotion-campaign.md). Les ID de campagne offrant les meilleurs résultats sont affichés, et vous pouvez utiliser les filtres pour exclure ou inclure des ID de campagne spécifiques.

## <a name="total-campaign-conversions"></a>Conversions totale de campagnes

Le graphique **Conversions totales de campagnes** indique le nombre total de conversions d’applications et d’extensions issues de toutes les campagnes personnalisées sur la période sélectionnée.





 

 
