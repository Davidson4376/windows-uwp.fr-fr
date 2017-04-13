---
author: msatranjr
title: "Demander une clé d’authentification de cartes"
description: "Votre application Windows universelle doit être authentifiée pour pouvoir utiliser le MapControl et les services cartographiques dans l’espace de noms Windows.Services.Maps."
ms.assetid: 13B400D7-E13F-4F07-ACC3-9C34087F0F73
ms.author: misatran
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "Windows10, uwp, clé d’authentification de cartes, contrôle de cartes"
ms.openlocfilehash: 42078becbc5853787ca057dcbfb58b8d8de7967d
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="request-a-maps-authentication-key"></a>Demander une clé d’authentification de cartes


\[ Mise à jour pour les applications UWP sur Windows10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


Votre [application Windows universelle](https://msdn.microsoft.com/library/windows/apps/dn894631) doit être authentifiée pour pouvoir utiliser le [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004) et les services cartographiques dans l’espace de noms [**Windows.Services.Maps**](https://msdn.microsoft.com/library/windows/apps/dn636979). Pour authentifier votre application, vous devez spécifier une clé d’authentification de cartes. Cette rubrique décrit comment demander une clé d’authentification de cartes au [Centre de développement Bing Cartes](https://www.bingmapsportal.com/) et comment l’ajouter à votre application.

**Conseil** Pour en savoir plus sur l’utilisation de cartes dans votre application, téléchargez l’exemple suivant à partir du [référentiel Windows-universal-samples](http://go.microsoft.com/fwlink/p/?LinkId=619979) sur GitHub :

-   [Exemple de carte pour la plateforme Windows universelle (UWP, Universal Windows Platform)](http://go.microsoft.com/fwlink/p/?LinkId=619977)

## <a name="get-a-key"></a>Obtenir une clé


Le [Centre de développement Bing Cartes](https://www.bingmapsportal.com/) permet de créer et de gérer des clés d’authentification de cartes pour vos applications universelles Windows.

Pour créer une clé

1.  Dans votre navigateur, accédez au Centre de développement Bing Cartes ([https://www.bingmapsportal.com](https://www.bingmapsportal.com/)).

2.  Si vous êtes invité à vous connecter, entrez votre compte Microsoft, puis cliquez sur **Se connecter**.

3.  Choisissez le compte à associer à votre compte Bing Cartes. Si vous voulez utiliser votre compte Microsoft, cliquez sur **Oui**. Sinon, cliquez sur **Se connecter avec un autre compte**.

4.  Si vous n’avez pas encore de compte Bing Cartes, créez-en un. Renseignez les champs suivants : **Nom du compte**, **Nom du contact**, **Nom de la société**, **Adresse e-mail** et **Numéro de téléphone**. Après avoir accepté les conditions d’utilisation, cliquez sur **Créer**.

5.  Dans le menu **Mon compte**, cliquez sur **Créer ou afficher des clés**.

6.  Cliquez sur le lien **Pour créer une clé**.

7.  Complétez le formulaire **Créer une clé**, puis cliquez sur **Créer**.

    -   **Nom de l’application :** le nom de votre application.
    -   **URL de l’application (facultatif) :** l’URL de votre application.
    -   **Type de clé :** sélectionnez **De base** ou **Entreprise**.
    -   **Type d’application :** sélectionnez **Application Windows universelle** pour utiliser la clé dans votre application Windows universelle.

    Voici un exemple de formulaire:

    ![Exemple du formulaire Créer une clé.](images/createkeydialog.png)

8.  Une fois que vous avez cliqué sur **Créer**, la nouvelle clé s’affiche en dessous du formulaire **Créer une clé**. Copiez-la dans un emplacement sûr ou ajoutez-la immédiatement à votre application, comme indiqué à l’étape suivante.

## <a name="add-the-key-to-your-app"></a>Ajouter la clé à votre application


La clé d’authentification de carte est nécessaire pour utiliser le [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004) et les services de carte ([**Windows.Services.Maps**](https://msdn.microsoft.com/library/windows/apps/dn636979)) dans votre application Windows universelle. Le cas échéant, ajoutez-la aux objets contrôle de carte et services de carte.

### <a name="to-add-the-key-to-a-map-control"></a>Pour ajouter la clé à un contrôle de carte

Pour authentifier le [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004), définissez la propriété [**MapServiceToken**](https://msdn.microsoft.com/library/windows/apps/dn637036) sur la valeur de clé d’authentification. Vous pouvez définir cette propriété dans le code ou dans le balisage XAML, en fonction de vos préférences. Pour plus d’informations sur l’utilisation du **MapControl**, voir [Afficher des cartes avec des vues 2D, 3D et Streetside](display-maps.md).

-   Cet exemple définit le **MapServiceToken** sur la valeur de la clé d’authentification dans le code.

    ```cs
    MapControl1.MapServiceToken = "abcdef-abcdefghijklmno";
    ```

-   Cet exemple définit le **MapServiceToken** sur la valeur de la clé d’authentification dans le balisage XAML.

    ```xml
    <Maps:MapControl x:Name="MapControl1" MapServiceToken="abcdef-abcdefghijklmno"/>
    ```

### <a name="to-add-the-key-to-map-services"></a>Pour ajouter la clé aux services cartographiques

Pour utiliser les services dans l’espace de noms [**Windows.Services.Maps**](https://msdn.microsoft.com/library/windows/apps/dn636979), définissez la propriété [**ServiceToken**](https://msdn.microsoft.com/library/windows/apps/dn636977) sur la valeur de clé d’authentification. Pour plus d’informations sur l’utilisation des services cartographiques, voir [Afficher des itinéraires et indications](routes-and-directions.md) et [Effectuer un géocodage et un géocodage inverse](geocoding.md).

-   Cet exemple définit le **ServiceToken** sur la valeur de la clé d’authentification dans le code.

    ```cs
    MapService.ServiceToken = "abcdef-abcdefghijklmno";
    ```

## <a name="related-topics"></a>Rubriques connexes

* [Centre de développement Bing Cartes](https://www.bingmapsportal.com/)
* [Exemple de carte UWP](http://go.microsoft.com/fwlink/p/?LinkId=619977)
* [Recommandations en matière de conception pour les cartes](https://msdn.microsoft.com/library/windows/apps/dn596102)
* [Vidéos de la build 2015: utilisation des cartes et de la localisation sur un téléphone, une tablette et un PC dans vos applications Windows](https://channel9.msdn.com/Events/Build/2015/2-757)
* [Exemple d’application de trafic UWP](http://go.microsoft.com/fwlink/p/?LinkId=619982)
