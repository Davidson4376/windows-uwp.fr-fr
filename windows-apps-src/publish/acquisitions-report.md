---
author: jnHs
Description: The Acquisitions report in the Windows Dev Center dashboard lets you see who has acquired and installed your app, along with demographic and platform details.
title: Rapport Acquisitions
ms.assetid: 21126362-F3CD-4006-AD3F-82FC88E3B862
ms.author: wdg-dev-content
ms.date: 08/15/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp, acquisitions, ventes d’applications, téléchargements d'applications, installations, entonnoir, acquisition, conversions, canal, vues de pages d'applications
ms.localizationpriority: medium
ms.openlocfilehash: e6b4a3d8a10234e5f95e70f397a4de962a29c929
ms.sourcegitcommit: 72710baeee8c898b5ab77ceb66d884eaa9db4cb8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/11/2018
ms.locfileid: "3848723"
---
# <a name="acquisitions-report"></a>Rapport Acquisitions


Le rapport **Acquisitions** disponible dans le tableau de bord du centre de développement Windows vous permet de déterminer qui acquiert et installe votre application, ainsi que des données démographiques et des informations sur la plateforme et affiche des informations sur la façon dont les clients sur Windows 10 (y compris Xbox) sont arrivés à de votre application Description. Vous pouvez également afficher les données d’acquisition en temps réel pour la dernière période ou 70-deux heures. 

Vous pouvez visualiser ces données dans votre tableau de bord ou [télécharger le rapport](download-analytic-reports.md) et le consulter hors connexion. Vous pouvez également récupérer ces données par programme à l’aide de notre [API REST d’analyse](../monetize/access-analytics-data-using-windows-store-services.md).

Dans ce rapport, une **acquisition** signifie qu’un nouveau client a obtenu une licence pour votre application (qu’elle soit payante ou gratuite). Une **installation** fait référence à l’application installée sur un appareil Windows10.

> [!IMPORTANT]
> Le rapport **Acquisitions** n’inclut aucune donnée sur les remboursements, reprises, rétrofacturations, etc. Pour évaluer les revenus de votre application, consultez l’article [Résumé du paiement](payout-summary.md). Dans la section **Réservé**, cliquez sur le lien **Télécharger les transactions réservées**.
>
> À l’exception des données sur les vues de page (comme décrit ci-dessous), ce rapport n’inclut pas les données relatives aux clients qui acquièrent une application sans être connectés à un compte Microsoft.


## <a name="apply-filters"></a>Appliquer des filtres

Dans la zone supérieure de la page, vous pouvez sélectionner la période sur laquelle portent les données qui vous intéressent. La valeur par défaut est de **30D** (30jours), mais vous pouvez choisir d’afficher les données portant sur des périodes de 3, 6 ou 12mois, ou sur une plage de dates personnalisée que vous spécifiez. Dans la zone en temps réel des données seront affiche pour toutes les options (sauf dans les données **d’application cumulative** ). L’heure de **1 H** et **72 H** périodes s’appliquent uniquement à l’onglet **application quotidienne** du graphique **Acquisitions** et à l’onglet **Acquisitions** du graphique **des marchés** . 

Vous pouvez également développer l’option **Filtres** pour filtrer toutes les données de cette page par marché et/ou par type d’appareil.

-   **Marché**: le filtre par défaut est **Tous les marchés**, mais vous pouvez restreindre les données aux acquisitions relatives à un ou plusieurs marchés.
-   **Type d’appareil**: le paramétrage par défaut est **Tous les appareils**. Si vous souhaitez afficher les données d’acquisitions relatives à un certain type d’appareil uniquement (PC, console ou tablette, par exemple), vous pouvez en choisir un ici.

Les informations figurant dans tous les graphiques répertoriés ci-après correspondent à la plage de dates et à tous les filtres que vous avez sélectionnés. Certaines sections vous permettent également d’appliquer des filtres supplémentaires.


## <a name="acquisitions"></a>Acquisitions

Le graphique **Acquisitions** affiche le nombre total d’acquisitions quotidiennes ou hebdomadaires (chaque fois qu’un nouveau client obtient une licence pour votre application) au cours de la période sélectionnée. (Lorsque vous utilisez l’option **Appliquer les filtres** pour afficher les données portant sur une durée plus étendue, les données d’acquisition sont regroupées par semaine.) Seules les acquisitions effectuées par les clients qui sont connectés avec un compte Microsoft valide figurent dans ce graphique. 

Par défaut, nous affichons la vue de **l’application tous les jours** , qui comprend près de données en temps réel. Vous pouvez également visualiser le nombre total d’acquisitions sur toute la durée de vie de votre application en sélectionnant **App cumulative**. Vous avez alors accès au total cumulé de l’ensemble des acquisitions effectuées depuis la première publication de votre application.

Si vous le souhaitez, vous pouvez filtrer les résultats par acquisition émanant du client ou du WindowsStore sur le web et/ou par version du système d’exploitation.

> [!NOTE]
> Vous pouvez également récupérer ces données par programme à l’aide de la méthode [Obtenir des acquisitions d’applications](../monetize/get-app-acquisitions.md) dans notre [API REST d’analyse](../monetize/access-analytics-data-using-windows-store-services.md).

Dans la vue de **l’application de tous les jours** , lorsque le **30D** période est activée, vous pouvez voir des marqueurs de cercle. Ces représentent une augmentation importante ou la diminution une valeur donnée dont nous pensons que vous devrez connaître. La date à laquelle le cercle apparaît représente la fin de la semaine dans lequel nous avons détecté une augmentation significative ou une baisse par rapport à la semaine. Pour afficher plus d’informations sur ce qui a changé, survolez le cercle.  

> [!TIP]
> Vous pouvez afficher des informations plus liées à des modifications importantes au cours des 30 derniers jours dans le [rapport de perspectives](insights-report.md).

## <a name="installs"></a>Installations

Le graphique **Installations** affiche le nombre de fois où nous avons détecté que des clients ont installé votre application sur des appareilsWindows10 (y compris sur des consoles XboxOne) au cours de la période sélectionnée. Nous vous présentons le nombre total d’installations, ainsi qu’un graphique affichant le nombre d’installations par jour ou par semaine (selon la durée que vous avez sélectionnée). Si vous le souhaitez, vous pouvez filtrer les résultats par version de package spécifique.

Le nombre total d’installations comprend les éléments suivants:
-   **Installations sur plusieurs appareilsWindows10.** Par exemple, si un même client installe votre application sur deuxPC Windows10 et sur une console XboxOne, troisinstallations sont comptabilisées.
-   **Réinstallations.** Par exemple, si un client installe votre application aujourd’hui, la désinstalle demain puis la réinstalle le mois prochain, 2installations sont consignées.

Le total ne comprend pas les éléments suivants:
-   **Installations sur des appareils non Windows10.** Si votre application prend en charge des versions antérieures du système d’exploitation, telles que Windows8.x ou WindowsPhone8.x, nous ne comptabilisons pas les installations sur ces appareils.
-   **Désinstallations.** Lorsqu’un client désinstalle votre application de son appareil, nous ne le soustrayons pas du nombre total d’installations.
-   **Mises à jour.** Par exemple, si un client installe votre application à une date donnée, puis installe une mise à jour une semaine plus tard, nous ne comptabilisons qu’une seule installation.
-   **Préinstallations.** Si un client achète un appareil sur lequel votre application est préinstallée, nous ne le comptabilisons pas comme une installation.
-   **Installations initiées par le système.** Si Windows installe votre application automatiquement, pour quelque raison que ce soit, nous ne le comptabilisons pas comme une installation.

> [!NOTE]
> Vous pouvez également récupérer ces données par programme à l’aide de la méthode [Obtenir des installations d’application](../monetize/get-app-installs.md) dans notre [API REST d’analyse](../monetize/access-analytics-data-using-windows-store-services.md).

## <a name="acquisition-funnel"></a>Entonnoir d’acquisition

Le graphique **Entonnoir d’acquisition** vous indique le nombre de clients ayant suivi chaque étape de l’entonnoir, depuis la visualisation de la page du WindowsStore jusqu’à l’utilisation de l’application, ainsi que le taux de conversion. Ces données peuvent vous aider à identifier les zones dans lesquelles vous pouvez vous investir davantage pour augmenter le nombre d’acquisitions, d’installations ou d’utilisations de votre application.

> [!IMPORTANT]
> La section **Entonnoir d’acquisition** affiche uniquement les données relatives aux clients utilisant Windows10 (y compris Xbox) et portant sur les 90derniers jours.

Les étapes de l’entonnoir sont les suivantes:

- **Consultations de la page**: cette valeur représente le nombre total de vues de la description de votre application dans le WindowsStore, y compris par les utilisateurs qui ne sont pas connectés avec un compte Microsoft. Cette valeur ne prend pas en compte les clients qui ont choisi de ne pas fournir ces informations à Microsoft.
- **Acquisitions**: nombre de nouveaux clients ayant obtenu une licence pour votre application (une fois connectés avec leur compte Microsoft) moins de 48heures après avoir visualisé la description de l’application dans le WindowsStore.
- **Installations**: nombre de clients ayant installé l’application après l’avoir acquise.
- **Utilisation**: nombre de clients ayant utilisé l’application après l’avoir installée.

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

 

## <a name="app-page-views-and-conversions-by-channel"></a>Conversions et accès aux pages de l’application par canal

Le graphique **Conversions et accès aux pages de l’application par canal** vous permet de déterminer la façon dont les clients qui utilisent Windows10 ont accédé à la description de votre application au cours de la période sélectionnée.

Dans ce graphique, un *canal* désigne la méthode à l’aide de laquelle un client a accédé à la page de description de votre application (par exemple, navigation et recherche dans le WindowsStore, lien à partir d’un site web externe, lien figurant dans l’une de vos campagnes personnalisées, etc.). Les canaux suivants sont inclus :

-   **Trafic du magasin :** le client naviguait ou effectuait une recherche dans le Windows Store lorsqu'ils a consulté la description de votre application.
-   **Campagne personnalisée :** le client suivait un lien qui a utilisé un [ID de campagne personnalisée](create-a-custom-app-promotion-campaign.md).
-   **Autres:** le client suivait un lien externe (sans ID de campagne personnalisée) d’un site web vers la description de votre application ou d’un moteur de recherche vers la description de votre application.

Une *vue de page* signifie qu’un client a consulté la description de votre application dans le Windows Store, soit via le Windows Store sur le web, soit depuis l’application du Windows Store sur Windows10. Ce nombre inclut les vues par des utilisateurs qui ne sont pas connectés avec un compte Microsoft. Certains clients ont choisi de ne pas fournir ces informations à Microsoft.

Une *conversion* signifie qu’un nouveau client (connecté avec un compte Microsoft) a obtenu une licence pour votre application (qu’elle soit payante ou gratuite).

Les nombres de vues de page et de conversions ne correspondent pas à des nombres de clients uniques. Pour plus d’informations sur les taux de conversion, consultez le graphique [Entonnoir d’acquisition](#acquisition-funnel).

> [!NOTE]
> Il est possible que des clients aient accédé à la description de votre application en cliquant sur une campagne personnalisée non créée par vos soins. Nous marquons chaque vue de page dans une session avec l’ID de campagne à partir duquel l’utilisateur a accédé au WindowsStore. Puis nous attribuons à cet ID de campagne les conversions appropriées parmi toutes les acquisitions effectuées dans les 24heures. Par conséquent, le nombre total de conversions peut être plus élevé que celui de vos ID de campagne, et certaines de vos conversions ou conversions d’extension peuvent présenter un nombre de vues de page nul.

> [!NOTE]
> Vous pouvez également récupérer ces données par programme à l’aide de la méthode [Obtenir les conversions d’applications par canal](../monetize/get-app-conversions-by-channel.md) dans notre [API REST d’analyse](../monetize/access-analytics-data-using-windows-store-services.md).

## <a name="app-page-views-and-conversions-by-campaign-id"></a>Conversions et accès aux pages de l’application par ID de campagne

Le graphique **Conversions et accès aux pages de l’application par ID de campagne** vous permet d’effectuer le suivi des conversions et des vues de page, comme décrit ci-dessus, pour chacune de vos [campagnes de promotion personnalisées](create-a-custom-app-promotion-campaign.md). Les ID de campagne offrant les meilleurs résultats sont affichés, et vous pouvez utiliser les filtres pour exclure ou inclure des ID de campagne spécifiques.

## <a name="total-campaign-conversions"></a>Conversions totale de campagnes

Le graphique **Conversions totales de campagnes** indique le nombre total de conversions d’applications et d’extensions issues de toutes les campagnes personnalisées sur la période sélectionnée.





 

 
