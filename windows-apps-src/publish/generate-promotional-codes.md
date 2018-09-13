---
author: jnHs
Description: You can generate promotional codes for an app or add-on that you have published in the Microsoft Store.
title: Générer des codes promotionnels
ms.assetid: 9B632266-64EC-4D62-A4C4-55B6643D8750
ms.author: wdg-dev-content
ms.date: 08/24/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp, code promotionnel, codes promotionnels, jeton, jetons
ms.localizationpriority: medium
ms.openlocfilehash: 37263794ffed6660f71c5e16195e992588c16d4a
ms.sourcegitcommit: c8f6866100a4b38fdda8394ea185b02d7af66411
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/13/2018
ms.locfileid: "3957561"
---
# <a name="generate-promotional-codes"></a>Générer des codes promotionnels


Vous pouvez générer des codes promotionnels pour une application ou un module complémentaire que vous avez publiés dans le Microsoft Store. Les codes promotionnels permettent d’offrir facilement à des utilisateurs influents un accès gratuit à votre application ou votre module complémentaire. Vous pouvez également utiliser des codes promotionnels dans des scénarios de service client, en offrant aux utilisateurs un accès gratuit à votre application ou votre module complémentaire, ou pour effectuer un [test bêta](beta-testing-and-targeted-distribution.md) dans Windows10. 

Chaque code promotionnel a une URL donnant droit correspondante unique laquelle un client peut cliquer pour pouvoir utiliser le code et installer votre application ou un module complémentaire à partir du Microsoft Store.  Notez que votre application doit exécuter la phase finale de publication du [processus de certification des applications](the-app-certification-process.md) avant que les clients ne puissent utiliser un code promotionnel pour l’installer.

Vous pouvez générer des codes à usage unique (et distribuer une pour chaque client), ou vous pouvez choisir de générer un code qui peut servir plusieurs fois par un certain nombre de clients.

> [!TIP]
> Vous pouvez utiliser des [notifications Push ciblées](send-push-notifications-to-your-apps-customers.md) pour distribuer un code promotionnel à un segment de vos clients. Dans ce cas, veillez à utiliser un code promotionnel qui permet à plusieurs clients d'utiliser le même code.


## <a name="promotional-code-policies"></a>Stratégies de code promotionnel

Tenez compte des stratégies suivantes relatives aux codes promotionnels:

-   Vous pouvez générer des codes promotionnels pour toute application ou tout module complémentaire (à l’exception des modules complémentaires d’abonnement) que vous avez publiés dans le Microsoft Store. Les clients peuvent utiliser les codes sur n’importe quelle version de Windows prise en charge par votre application ou module complémentaire.
-   Les codes promotionnels expirent 6mois après la date de commande (sauf si vous choisissez une date d’expiration antérieure).
-   Pour chaque application ou module complémentaire, vous pouvez générer des codes pour permettre jusqu’à 1600échanges tous les 6mois. La période de 6mois commence lorsque la première commande de code promotionnel est envoyée, même si vous choisissez une date d’expiration antérieure. Le total de 1600échanges par produit s’applique aux codes à usage unique et aux codes qui peuvent être utilisés plusieurs fois.
-   Vous devez respecter les exigences définies dans le [Contrat du développeur de l’application](https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement), notamment la section **3k. Codes promotionnels**.

> [!NOTE]
> Vous pouvez utiliser des codes promotionnels même si votre application n’est pas disponible pour les clients (autrement dit, si vous avez sélectionné l’option **rendre ce produit disponible mais non détectable dans le Windows Store** avec le **Stop acquisition: tout client ayant un lien direct peut voir Windows Store du produit Description, mais ils peuvent le télécharger que s’il possède déjà le produit, ou ont un code promotionnel et utilisent un appareil Windows 10** option dans la section de [détectabilité](choose-visibility-options.md#discoverability) de votre soumission). Avec cette option, les clients doivent se trouver sur Windows 10 (y compris Xbox) afin d’acquérir votre produit avec un code promotionnel.


## <a name="order-promotional-codes"></a>Commander des codes promotionnels

Pour commander des codes promotionnels pour une application ou un module complémentaire:

1.  Dans le menu de navigation de gauche du tableau de bord du Centre de développement Windows, développez **Attract**, puis sélectionnez **Promo codes**.

2.   Dans la page **Codes promotionnels**, cliquez sur **Commander des codes**.

3.  Dans la page **Nouvelle commande de codes promotionnels**, entrez les informations suivantes:
    -   Sélectionnez l’application ou le module complémentaire pour lequel vous voulez générer des codes. (Notez que vous ne pouvez pas générer des codes promotionnels pour les modules complémentaires d’abonnement).
    -   Spécifiez un nom pour la commande. Ce nom permet de différencier les différentes commandes de codes lors de l’examen des données d’utilisation de votre code promotionnel.
    -   Sélectionnez le type de commande. Vous pouvez choisir de générer plusieurs codes promotionnels pouvant servir une seule fois ou un seul code promotionnel pouvant servir plusieurs fois.
    -   Spécifiez le nombre de codes à commander (si vous générez un ensemble de codes) ou le nombre de fois où que le code peut être utilisé (si vous générez un code à utiliser plusieurs fois).
    -   Indiquez à quel moment les codes promotionnels doivent devenir actifs. Pour choisir une date et une heure de début précises, décochez la case **Les codes sont immédiatement actifs**. Dans le cas contraire, les codes deviendront actifs immédiatement (bien que votre produit doit avoir terminé le processus de publication dans l’ordre pour un client d’utiliser le code).
    -   Indiquez à quel moment les codes promotionnels doivent expirer. Pour choisir une date et une heure d’expiration précises antérieures à 6mois, décochez la case **Les codes expirent au bout de 6mois**.

4.  Cliquez sur **Commander des codes**. Vous êtes alors renvoyé à la page **codes promotionnels**, dans laquelle vous pouvez voir votre nouvelle commande dans le tableau récapitulatif des commandes de codes promotionnels pour cette application.


## <a name="download-and-distribute-promotional-codes"></a>Télécharger et distribuer des codes promotionnels

Pour télécharger une commande de codes promotionnels complétée et distribuer les codes aux clients:

1.  Dans le menu de navigation de gauche du tableau de bord du Centre de développement Windows, développez **Attract**, puis sélectionnez **Promo codes**.
2.  Cliquez sur le lien **Télécharger** correspondant à la commande de codes promotionnels, puis enregistrez le fichier généré sur votre ordinateur. Ce fichier contient des informations sur votre commande de codes promotionnels sous la forme de valeurs séparées par des tabulations (.tsv).
3.  Ouvrez le fichier.tsv dans l’éditeur de votre choix. Pour bénéficier d’une expérience optimale, ouvrez le fichier.tsv dans une application pouvant afficher les données dans une structure tabulaire, telle que MicrosoftExcel. Toutefois, vous pouvez ouvrir ce fichier dans tout éditeur de texte.

    Le fichier contient les colonnes de données ci-après pour chaque code:

    -   **Nom du produit**: nom de l’application ou du module complémentaire auquel le code est associé.
    -   **Nom de la commande**: nom de la commande dans laquelle ce code a été généré.
    -   **Code promotionnel**: code proprement dit. Il s’agit d’une chaîne de 5x5 caractères alphanumériques séparés par des traits d’union. Par exemple: DM3GY-M2GYM-6YMW6-4QHHT-23W2Z
    -   **URL donnant droit**: URL permettant à un client d’utiliser le code et d’installer votre application ou module complémentaire. L’URL a le format suivant: http://go.microsoft.com/fwlink/?LinkId=532540&mstoken=&lt; code_promotionnel >
    -   **Date de début**: date à laquelle ce code devient actif.
    -   **Date d’expiration**: date à laquelle ce code expire.
    -   **ID de code**: ID unique de ce code.
    -   **ID de commande**: ID unique de la commande dans laquelle ce code a été généré.
    -   **Accordé à**: champ vide que vous pouvez utiliser pour garder une trace du client auquel vous avez donné le code.
    -   **Disponible**: le nombre de fois où le code reste disponible (au moment où le fichier a été généré).
    -   **Utilisé**: nombre de fois où le code a été utilisé (au moment où le fichier a été généré).

4.  Distribuez les URL donnant droit à vos clients via le mode de communication de votre choix (par exemple, notifications ciblées, message électronique, SMS ou cartes imprimées). Nous vous recommandons d’inclure dans votre communication les éléments suivants:
    -   Explication relative à l’application ou au module complémentaire auquel le code promotionnel a trait et, éventuellement, description de la raison pour laquelle le client reçoit le code.
    -   URL donnant droit correspondant au code.
    -   Instructions invitant le client à accéder à l’URL donnant droit, à se connecter à l’aide de son compte Microsoft, et à suivre les consignes de téléchargement et d’installation de votre application.


## <a name="code-redemption-user-experience"></a>Expérience utilisateur d’échange du code

Une fois que vous distribuez un code promotionnel (ou son URL donnant droit) à un client, ils peuvent sur l’URL pour obtenir le produit gratuitement. Lorsqu’il clique sur l'URL donnant droit, une page authentifiée **Utiliser votre code** s’ouvre à l’adresse <https://account.microsoft.com/billing/redeem>. Cette page inclut une description de l’application à laquelle l’utilisateur est sur le point d’accéder. Si le client n’est pas connecté à son compte Microsoft, il peut être invité à le faire. Votre client peut également consulter <https://account.microsoft.com/billing/redeem> et entrer le code directement.

> [!IMPORTANT]
> Nous vous recommandons de ne pas distribuer les codes promotionnels à vos clients tant que votre produit n'a pas terminé le processus de publication (même si vous avez sélectionné **Rendre ce produit disponible mais non détectable dans le Windows Store**). Les clients verront une erreur s’ils tentent d’utiliser un code promotionnel pour un produit qui n’a pas encore été publié.

Lorsque le client clique sur **Utiliser**, le MicrosoftStore s’ouvre à la page Vue d’ensemble de l’application (sur un appareil Windows10 ou Windows8.1), dans laquelle il peut cliquer sur **Installer** pour télécharger et installer l’application gratuitement. Si le client se connecte à partir d’un ordinateur ou d’un appareil sur lequel Microsoft Store n’est pas installé, le lien ouvre la page web du Microsoft Store pour l’application. Le code sera appliqué au compte Microsoft du client, afin que celui-ci puisse télécharger ultérieurement l’application sur un appareil Windows (associé au même compte Microsoft) gratuitement.

> [!NOTE]
> Dans certains cas, un client peut voir un bouton **acheter** au lieu de l' **installer**, même si l’application a été correctement acquise par le biais du code promotionnel. Le client peut alors cliquer sur **Acheter** pour installer l’application gratuitement.


## <a name="review-your-promotional-codes"></a>Passer en revue vos codes promotionnels

Pour consulter un récapitulatif détaillé des commandes de codes promotionnels pour vos applications et modules complémentaires, accédez à la page **Codes promotionnels** de l'application (dans le menu de navigation de gauche du tableau de bord du Centre de développement, développez **Attract**, puis sélectionnez **Promo codes**). Vous pouvez consulter les détails suivants pour tous vos codes promotionnels actifs et inactifs:
-   Nom de la commande
-   Application ou module complémentaire
-   Date de début
-   Date d’expiration
-   Disponible
-   Utilisé

Vous pouvez également [télécharger](#download-and-distribute-promotional-codes) une commande à partir de ce tableau.

 
