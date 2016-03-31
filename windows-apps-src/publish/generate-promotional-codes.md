---
Description: Vous pouvez générer des codes promotionnels pour une application ou un produit in-app que vous avez publiés dans le Windows Store.
title: Générer des codes promotionnels
ms.assetid: 9B632266-64EC-4D62-A4C4-55B6643D8750
---

# Générer des codes promotionnels


Vous pouvez générer des codes promotionnels pour une application ou un produit in-app que vous avez publiés dans le Windows Store. Les codes promotionnels permettent d’offrir facilement à des utilisateurs influents un accès gratuit à votre application ou votre produit in-app. Vous pouvez également utiliser des codes promotionnels dans des scénarios de service client, en offrant aux utilisateurs un accès gratuit à votre application ou votre produit in-app, ou pour effectuer un [test bêta](beta-testing-and-targeted-distribution.md) dans Windows 10.

À chaque code promotionnel correspond une URL donnant droit, que vous pouvez distribuer à un utilisateur. L’utilisateur peut cliquer simplement sur l’URL pour utiliser le code et installer votre application ou produit in-app à partir du Windows Store.

Dans le tableau de bord du Centre de développement Windows, vous pouvez :

-   Commander un ensemble de codes promotionnels pour votre application.
-   Télécharger une commande de codes promotionnels complétée.
-   Examiner l’utilisation des codes promotionnels pour vos applications, notamment :
    -   Récapitulatif des commandes de codes promotionnels pour toutes vos applications (dans la page **Présentation du tableau de bord**) et pour chaque application (dans la page **Vue d’ensemble de l’application** pour chaque application).
    -   Résumé détaillé des commandes de code promotionnel pour chaque application (sur la page **Codes promotionnels** pour chaque application).

> **Remarque** Vous pouvez générer des codes promotionnels même si vous avez sélectionné l’option **Masquer cette application et empêcher l’acquisition. Les clients disposant d’un code promotionnel peuvent la télécharger sur les appareils Windows 10** de la page du tableau de bord [Tarification et disponibilité](set-app-pricing-and-availability.md) relative à votre application. Votre application doit exécuter la phase finale de publication du [processus de certification des applications](the-app-certification-process.md) avant que les utilisateurs ne puissent utiliser un code promotionnel pour l’installer.

## Stratégies de code promotionnel


Tenez compte des stratégies suivantes relatives aux codes promotionnels :

-   Vous pouvez générer des codes promotionnels pour toute application ou tout produit in-app que vous avez publiés dans le Windows Store. Les utilisateurs peuvent utiliser les codes sur toutes les versions de Windows prises en charge par votre application ou produit in-app.
-   Les codes promotionnels expirent 6 mois après la date que leur commande.
-   Pour chaque application ou produit in-app, vous pouvez générer jusqu’à 250 codes promotionnels tous les 6 mois. La période de six mois commence à la soumission de la première commande de code promotionnel.
-   Vous devez respecter les exigences définies dans le [Contrat du développeur de l’application](https://msdn.microsoft.com/library/windows/apps/hh694058), notamment la section **3k. Codes promotionnels**.

## Commander des codes promotionnels


Pour commander des codes promotionnels pour une application ou un produit in-app que vous avez publiés dans le Windows Store :

1.  Dans le tableau de bord du Centre de développement Windows, effectuez l’une des opérations suivantes :
    -   Dans la page **Vue d’ensemble de l’application** pour votre application, recherchez la section **Codes promotionnels**, puis cliquez sur **Commander des codes**.
    -   Dans n’importe quelle page du tableau de bord pour votre application, dans le menu de navigation à gauche, développez **Monétisation**, puis cliquez sur **Codes promotionnels**. Dans la page **Codes promotionnels**, cliquez sur **Commander des codes**.

2.  Dans la page **Nouvelle commande de codes promotionnels**, entrez les informations suivantes :
    -   Sélectionnez l’application ou le produit in-app pour lequel vous voulez générer des codes.
    -   Spécifiez un nom pour la commande. Ce nom permet de différencier les différentes commandes de codes lors de l’examen des données d’utilisation de votre code promotionnel.
    -   Indiquez le nombre de codes à commander.

3.  Cliquez sur **Commander des codes**. La commande est soumise et le tableau de bord vous amène à la page **Codes promotionnels** dans laquelle la nouvelle commande est mentionnée comme **En attente** dans le tableau récapitulatif des commandes de codes promotionnels.

Les codes promotionnels sont généralement disponibles en téléchargement dans les 60 minutes suivant la soumission de la commande. Le traitement de certaines commandes peut cependant parfois prendre plus de temps. Une fois la commande exécutée et les codes disponibles en téléchargement, le statut de la commande devient **Disponible**.

## Télécharger et distribuer des codes promotionnels


Pour télécharger une commande exécutée de codes promotionnels et distribuer les codes aux utilisateurs de votre application :

1.  Dans le tableau de bord du Centre de développement Windows, revenez à la page **Codes promotionnels** de votre application (développez **Monétisation**, puis cliquez sur **Codes promotionnels**).
2.  Vérifiez que l’état de votre commande est **Disponible**. Cliquez sur le lien **Télécharger** correspondant à votre commande, puis enregistrez le fichier fourni sur votre ordinateur. Ce fichier contient des informations sur votre commande de codes promotionnels sous la forme de valeurs séparées par des tabulations (TSV).
3.  Ouvrez le fichier TSV dans l’éditeur de votre choix. Pour une expérience optimale, ouvrez le fichier TSV dans une application capable d’afficher les données dans une structure tabulaire, telle que Microsoft Excel. Vous pouvez cependant ouvrir ce fichier dans tout éditeur de texte.

    Le fichier contient les colonnes de données suivantes pour chaque code :

    -   **Nom du produit** : nom de l’application ou du produit in-app auquel le code est associé.
    -   **Nom de la commande** : nom de la commande dans laquelle ce code a été exécuté.
    -   **Code promotionnel** : code proprement dit. Il s’agit d’une chaîne de 5x5 caractères alphanumériques séparés par des traits d’union. Par exemple :

        DM3GY-M2GYM-6YMW6-4QHHT-23W2Z

    -   **URL donnant droit** : URL permettant à un utilisateur d’utiliser le code et d’installer votre application ou produit in-app. L’URL est au format suivant :

        https://account.microsoft.com/billing/redeem?mstoken=&lt;promotional_code>

    -   **Date de commande** : date à laquelle vous avez commandé ce code.
    -   **Date d’expiration** : date d’expiration de ce code.
    -   **ID de code** : ID unique de ce code.
    -   **ID de commande** : ID unique de la commande exécutée relative à ce code.
    -   **Accordé à** : champ vide dans lequel vous pouvez entrer une valeur identifiant l’utilisateur auquel vous avez donné le code.

4.  Distribuez les URL donnant droit à vos utilisateurs via le mode de communication de votre choix (par exemple, message électronique, SMS ou carte imprimée). Nous vous recommandons d’inclure dans votre communication les éléments suivants :
    -   Explication relative à l’application ou au produit in-app auquel le code promotionnel a trait et, éventuellement, description de la raison pour laquelle l’utilisateur reçoit le code.
    -   URL donnant droit correspondant au code.
    -   Instructions invitant l’utilisateur à accéder à l’URL donnant droit, à se connecter à l’aide de son compte Microsoft, et à suivre les consignes de téléchargement et d’installation de votre application.

## Expérience utilisateur d’échange du code


Une fois l’URL donnant droit distribuée à un utilisateur, les étapes suivantes décrivent comment celui-ci doit procéder pour utiliser le code afin d’obtenir votre application.

1.  L’utilisateur clique sur l’URL donnant droit.

    Le navigateur s’ouvre sur une page authentifiée **Utiliser votre code** à l’adresse <https://account.microsoft.com/billing/redeem>. Cette page inclut une description de l’application à laquelle l’utilisateur est sur le point d’accéder.

2.  L’utilisateur clique sur **Échanger**.

    Le navigateur accède à une page **Merci** contenant un lien **Obtenir** ***&lt;your app name&gt;***.

    > **Remarque** Si votre application n’est pas encore publiée à ce stade, les utilisateurs recevront un message d’erreur.

3.  L’utilisateur clique sur **Obtenir** ***&lt;your app name&gt;***.

4.  Si l’utilisateur se connecte à partir d’un ordinateur sur lequel le Windows Store pour Windows 10 ou Windows 8.1 est installé, le Windows Store s’ouvre à la page de présentation de l’application. L’utilisateur peut cliquer **Installer** pour installer l’application gratuitement.

    Si l’utilisateur se connecte à partir d’un ordinateur ou d’un appareil sur lequel Windows Store n’est pas installé, le navigateur ouvre la page web du Windows Store pour l’application. L’utilisateur peut cliquer **Installer** pour installer l’application gratuitement.

    > **Remarque** Dans certains cas, la page de l’application peut afficher un bouton **Acheter** à la place du bouton **Installer**, même si l’application a été correctement acquise par le biais du code promotionnel. L’utilisateur peut alors cliquer sur **Acheter** pour installer l’application gratuitement.

## Passer en revue vos codes promotionnels


Plusieurs méthodes permettent de passer en revue l’utilisation d’un code promotionnel.

-   Pour obtenir un récapitulatif des commandes de codes promotionnels pour toutes vos applications, accédez à la page **Présentation du tableau de bord** et consultez la section **Codes promotionnels**. Cette section présente, pour toutes vos applications, les codes promotionnels actifs restants, le nombre total de codes promotionnels utilisés, et le nombre total de commandes de codes promotionnels passées.
-   Pour consulter un récapitulatif des commandes de codes promotionnels pour une application spécifique, accédez à la page **Vue d'ensemble de l'application** pour cette application, puis consultez la section **Codes promotionnels**. Cette section présente les codes promotionnels actifs restants, le nombre total de codes promotionnels utilisés et le nombre total de commandes de codes promotionnels que vous avez passées pour l'application.
-   Pour consulter un récapitulatif détaillé des commandes de codes promotionnels pour une application spécifique, accédez à la page **Codes promotionnels** de l'application (développez **Monétisation**, puis cliquez sur **Codes promotionnels**). Vous pouvez consulter les détails suivants pour tous les codes promotionnels actifs et inactifs de l’application :
    -   Nom de la commande
    -   Nom de l’application ou du produit un-app
    -   Date de commande
    -   Date d’expiration
    -   État

Vous pouvez également télécharger une commande active à partir de ce tableau.

 

 




<!--HONumber=Mar16_HO1-->
