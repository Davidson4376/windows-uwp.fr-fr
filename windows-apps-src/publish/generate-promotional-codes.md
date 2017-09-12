---
author: jnHs
Description: "Vous pouvez générer des codes promotionnels pour une application ou un module complémentaire que vous avez publiés dans le Windows Store."
title: "Générer des codes promotionnels"
ms.assetid: 9B632266-64EC-4D62-A4C4-55B6643D8750
ms.author: wdg-dev-content
ms.date: 06/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.openlocfilehash: 97d0cb79a00140a7255923131f78c2b3fecff1d9
ms.sourcegitcommit: fadde8afee46238443ec1cb71846d36c91db9fb9
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/21/2017
---
# <a name="generate-promotional-codes"></a>Générer des codes promotionnels


Vous pouvez générer des codes promotionnels pour une application ou un module complémentaire que vous avez publiés dans le Windows Store. Les codes promotionnels permettent d’offrir facilement à des utilisateurs influents un accès gratuit à votre application ou votre module complémentaire. Vous pouvez également utiliser des codes promotionnels dans des scénarios de service client, en offrant aux utilisateurs un accès gratuit à votre application ou votre module complémentaire, ou pour effectuer un [test bêta](beta-testing-and-targeted-distribution.md) dans Windows10.

À chaque code promotionnel correspond une URL donnant droit, que vous pouvez distribuer à un utilisateur ou à un groupe d’utilisateurs. L’utilisateur peut cliquer simplement sur l’URL pour utiliser le code et installer votre application ou module complémentaire à partir du Windows Store.

> [!TIP] 
> Vous pouvez utiliser des [notifications Push ciblées](send-push-notifications-to-your-apps-customers.md) pour distribuer un code promotionnel à un segment de vos clients. Dans ce cas, veillez à utiliser un code promotionnel qui permet à plusieurs clients d'utiliser le même code.

Dans le tableau de bord du Centre de développement Windows, vous pouvez:

-   Commander un ensemble de codes promotionnels pour votre application ou module complémentaire.
-   Télécharger une commande de codes promotionnels complétée.
-   Examiner l’utilisation des codes promotionnels.

> [!NOTE]
> Vous pouvez générer des codes promotionnels même si vous avez sélectionné l'option [Visibilité](set-app-pricing-and-availability.md#visibility) **Empêcher l'acquisition: tout client disposant d'un lien direct peut voir la description du produit dans le Windows Store, mais ne peut le télécharger que s'il possède déjà le produit ou dispose d'un code promotionnel et utilise un appareil Windows10.**

Notez que votre application doit exécuter la phase finale de publication du [processus de certification des applications](the-app-certification-process.md) avant que les clients ne puissent utiliser un code promotionnel pour l’installer.

## <a name="promotional-code-policies"></a>Stratégies de code promotionnel


Tenez compte des stratégies suivantes relatives aux codes promotionnels:

-   Vous pouvez générer des codes promotionnels pour toute application ou tout module complémentaire que vous avez publiés dans le Windows Store. Les clients peuvent utiliser les codes sur toutes les versions de Windows prises en charge par votre application ou module complémentaire.
-   Les codes promotionnels expirent 6mois après la date de commande (sauf si vous choisissez une date d’expiration antérieure).
-   Pour chaque application ou module complémentaire, vous pouvez générer des codes pour permettre jusqu’à 1600échanges tous les 6mois. La période de 6mois commence lorsque la première commande de code promotionnel est envoyée, même si vous choisissez une date d’expiration antérieure. Le total de 1600échanges par produit s’applique aux codes à usage unique et aux codes qui peuvent être utilisés plusieurs fois.
-   Vous devez respecter les exigences définies dans le [Contrat du développeur de l’application](https://msdn.microsoft.com/library/windows/apps/hh694058), notamment la section **3k. Codes promotionnels**.

## <a name="order-promotional-codes"></a>Commander des codes promotionnels


Pour commander des codes promotionnels pour une application ou une extension que vous avez publiés dans le WindowsStore:

1.  Dans le menu de navigation de gauche du tableau de bord du Centre de développement Windows, développez **Attract**, puis sélectionnez **Promo codes**.

2.   Dans la page **Codes promotionnels**, cliquez sur **Commander des codes**.

3.  Dans la page **Nouvelle commande de codes promotionnels**, entrez les informations suivantes:
    -   Sélectionnez l’application ou le module complémentaire pour lequel vous voulez générer des codes.
    -   Spécifiez un nom pour la commande. Ce nom permet de différencier les différentes commandes de codes lors de l’examen des données d’utilisation de votre code promotionnel.
    -   Sélectionnez le type de commande. Vous pouvez choisir de générer plusieurs codes promotionnels pouvant servir une seule fois ou un seul code promotionnel pouvant servir plusieurs fois. 
    -   Spécifiez le nombre de codes à commander (si vous générez un ensemble de codes) ou le nombre de fois où que le code peut être utilisé (si vous générez un code à utiliser plusieurs fois).
    -   Indiquez à quel moment les codes promotionnels doivent devenir actifs. Pour choisir une date et une heure de début précises, décochez la case **Les codes sont immédiatement actifs**. Sinon, les codes seront actifs immédiatement.
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
    -   **URL donnant droit**: URL permettant à un client d’utiliser le code et d’installer votre application ou module complémentaire. L’URL est au format suivant: http://go.microsoft.com/fwlink/?LinkId=532540&mstoken=&lt;code_promotionnel>
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

Une fois que vous distribuez un code promotionnel (ou son URL donnant droit) à un client, celui-ci peut utiliser cette URL pour obtenir le produit gratuitement. Lorsqu'il clique sur l'URL donnant droit, une page authentifiée **Utiliser votre code** s'ouvre à l’adresse <https://account.microsoft.com/billing/redeem>. Cette page inclut une description de l’application à laquelle l’utilisateur est sur le point d’accéder. Si le client n’est pas connecté à son compte Microsoft, il peut être invité à le faire. Votre client peut également consulter <https://account.microsoft.com/billing/redeem> et saisir le code directement.

> [!IMPORTANT]
> Nous vous recommandons de ne pas distribuer les codes promotionnels à vos clients tant que votre produit n'a pas terminé le processus de publication (même si vous avez sélectionné **Rendre ce produit disponible mais non détectable dans le Windows Store**). Les clients verront une erreur s’ils tentent d’utiliser un code promotionnel pour un produit qui n’a pas encore été publié.

Lorsque le client clique sur **Utiliser**, le Windows Store s’ouvre à la page Vue d’ensemble de l’application (sur un appareil Windows10 ou Windows8.1), dans laquelle il peut cliquer sur **Installer** pour télécharger et installer l’application gratuitement. Si le client se connecte à partir d’un ordinateur ou d’un appareil sur lequel Windows Store n’est pas installé, le lien ouvre la page web du Windows Store pour l’application. Le code sera appliqué au compte Microsoft du client, afin que celui-ci puisse télécharger ultérieurement l’application sur un appareil Windows (associé au même compte Microsoft) gratuitement.

> [!NOTE]
> Dans certains cas, le client peut voir un bouton **Acheter** à la place du bouton **Installer**, même si l’application a été correctement acquise par le biais du code promotionnel. Le client peut alors cliquer sur **Acheter** pour installer l’application gratuitement.


## <a name="review-your-promotional-codes"></a>Passer en revue vos codes promotionnels

Pour consulter un récapitulatif détaillé des commandes de codes promotionnels pour une application et son module complémentaire, accédez à la page **Codes promotionnels** de l'application (développez **Monétisation**, puis cliquez sur **Codes promotionnels**). Vous pouvez consulter les détails suivants pour tous les codes promotionnels actifs et inactifs de l’application:
    -   Nom de la commande
    -   Application ou module complémentaire
    -   Date de début
    -   Date d’expiration
    -   Disponible
    -   Utilisé

Vous pouvez également télécharger une commande à partir de ce tableau, comme décrit ci-dessus. 

 




