---
author: msatranjr
title: "Vue d’ensemble des cartes et de l’emplacement"
description: "Cette section explique comment afficher des cartes, utiliser les services de carte, rechercher l’emplacement et configurer une limite géographique dans votre application. Cette section vous montre également comment lancer l’application CartesWindows pour obtenir une carte, un itinéraire ou un ensemble d’itinéraires détaillés spécifique."
ms.assetid: F4C1F094-CF46-4B15-9D80-C1A26A314521
translationtype: Human Translation
ms.sourcegitcommit: a3240047ec77ada0c5f6b5586eee2404353889f6
ms.openlocfilehash: 327185d655d8495901cfa9fa0a99c0af3a8cdae0

---

# Vue d’ensemble des cartes et de l’emplacement


\[ Article mis à jour pour les applications UWP sur Windows10. Pour les articles sur Windows8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]


Cette section explique comment afficher des cartes, utiliser les services de carte, rechercher l’emplacement et configurer une limite géographique dans votre application. Cette section vous montre également comment lancer l’application Cartes Windows pour obtenir une carte, un itinéraire ou un ensemble d’itinéraires détaillés spécifique.

> **Conseil** Pour en savoir plus sur l’utilisation des cartes et de l’emplacement dans votre application, téléchargez les exemples suivants à partir du [référentiel Windows-universal-samples](http://go.microsoft.com/fwlink/p/?LinkId=619979) sur GitHub.
-   [Exemple de carte pour la plateforme Windows universelle (UWP)](http://go.microsoft.com/fwlink/p/?LinkId=619977)
-   [Exemple de géolocalisation UWP](http://go.microsoft.com/fwlink/p/?linkid=533278)

 

## Afficher des cartes


Affichez des cartes avec des vues2D, 3D ou Streetview dans votre application à l’aide des API de l’espace de noms [**Windows.UI.Xaml.Controls.Maps**](https://msdn.microsoft.com/library/windows/apps/dn610751). Vous pouvez marquer des points d’intérêt sur la carte grâce à des punaises, des images, des formes ou des éléments d’interface utilisateur XAML. Vous pouvez également superposer des images sous forme de vignettes ou remplacer entièrement les images de la carte.

| Rubrique | Description |
|-------|-------------|
| [Demander une clé d’authentification de cartes](authentication-key.md) | Pour pouvoir utiliser le [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004) et les services de carte dans l’espace de noms [**Windows.Services.Maps**](https://msdn.microsoft.com/library/windows/apps/dn636979), votre application doit être authentifiée. Pour authentifier votre application, vous devez spécifier une clé d’authentification de cartes. Cet article décrit comment demander une clé d’authentification de cartes au [Centre de développement Bing Cartes](https://www.bingmapsportal.com/) et comment l’ajouter à votre application. |
| [Contrôle de carte](controls-map.md) | Le contrôle de carte peut afficher des cartes routières et des vues aériennes, des itinéraires, des résultats de recherche et des informations sur la circulation. |
| [Afficher des cartes avec des vues2D, 3D et Streetside](display-maps.md) | Affichez des cartes personnalisables dans votre application en utilisant la classe [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004). Cette rubrique présente également des vues3D aériennes et Streetside. |
| [Afficher les points d’intérêt sur une carte](display-poi.md) | Ajoutez des points d’intérêt à une carte à l’aide de punaises, d’images, de formes et d’éléments d’interface utilisateur XAML. |
| [Superposer des images sous forme de vignettes sur une carte](overlay-tiled-images.md) | Superposez des images sous forme de vignettes tierces ou personnalisées sur une carte à l’aide de sources de vignettes. Utilisez des sources de vignette pour superposer des informations spécifiques (informations météorologiques, démographiques, sismiques...), ou pour remplacer entièrement la carte par défaut. |



## Accéder aux services de carte

Ajoutez des itinéraires, des indications et des fonctionnalités de géocodage à votre application grâce aux API de l’espace de noms [**Windows.Services.Maps**](https://msdn.microsoft.com/library/windows/apps/dn636979). Vous pouvez également aider l’utilisateur à gérer des cartes hors connexion en lançant l’application Paramètres de façon à ce qu’elle s’ouvre directement à la page appropriée.

| Rubrique | Description |
|-----------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Demander une clé d’authentification de cartes](authentication-key.md) | Pour pouvoir utiliser le [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004) et les services de carte dans l’espace de noms [**Windows.Services.Maps**](https://msdn.microsoft.com/library/windows/apps/dn636979), votre application doit être authentifiée. Pour authentifier votre application, vous devez spécifier une clé d’authentification de cartes. Cet article décrit comment demander une clé d’authentification de cartes au [Centre de développement Bing Cartes](https://www.bingmapsportal.com/) et comment l’ajouter à votre application. |
| [Afficher des centres d’intérêt sur une carte](display-poi.md) | Ajoutez des centres d’intérêt à une carte à l’aide de punaises, d’images, de formes et d’éléments d’interface utilisateur XAML. |
| [Afficher des itinéraires et indications](routes-and-directions.md) | Demandez des itinéraires et indications, et affichez-les dans votre application. |
| [Effectuer un géocodage et un géocodage inverse](geocoding.md) | Convertissez des adresses en emplacements géographiques (géocodage) et des emplacements géographiques en adresses (géocodage inverse) en appelant les méthodes de la classe [**MapLocationFinder**](https://msdn.microsoft.com/library/windows/apps/dn627550) dans l’espace de noms [**Windows.Services.Maps**](https://msdn.microsoft.com/library/windows/apps/dn636979). |


## Obtenir l’emplacement de l’utilisateur

Utilisez les API de l’espace de noms [**Windows.Devices.Geolocation**](https://msdn.microsoft.com/library/windows/apps/br225603) pour obtenir l’emplacement actuel de l’utilisateur et être notifié lors de changements d’emplacement dans votre application. Des éléments de ces API sont aussi fréquemment utilisés dans des paramètres des API de cartes. Les API de l’espace de noms [**Windows.Devices.Geolocation.Geofencing**](https://msdn.microsoft.com/library/windows/apps/dn263744) notifient votre application quand l’utilisateur franchit une limite géographique (entre dans une zone géographique prédéfinie ou en sort).

| Rubrique | Description |
|-------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Demander une clé d’authentification de cartes](authentication-key.md) | Pour pouvoir utiliser le [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004) et les services de carte dans l’espace de noms [**Windows.Services.Maps**](https://msdn.microsoft.com/library/windows/apps/dn636979), votre application doit être authentifiée. Pour authentifier votre application, vous devez spécifier une clé d’authentification de cartes. Cet article décrit comment demander une clé d’authentification de cartes au [Centre de développement Bing Cartes](https://www.bingmapsportal.com/) et comment l’ajouter à votre application. |
| [Recommandations en matière de conception pour les applications prenant en charge la géolocalisation](guidelines-and-checklist-for-detecting-location.md) | Recommandations en matière de performance des applications qui nécessitent de géolocaliser un utilisateur. |
| [Obtenir l’emplacement de l’utilisateur](get-location.md) | Demandez l’accès à l’emplacement de l’utilisateur, puis récupérez-le. |
| [Recommandations en matière de conception pour le géorepérage](guidelines-for-geofencing.md) | Recommandations en matière de performances pour les applications qui utilisent la fonctionnalité de géorepérage. |
| [Configurer une limite géographique](set-up-a-geofence.md) | Configurez une limite géographique dans votre application et découvrez comment gérer les notifications au premier plan et en arrière-plan. |

## Lancer l’application CartesWindows

Votre application peut lancer l’application Cartes Windows, comme illustré ici, pour afficher des cartes et des itinéraires détaillés spécifiques. Au lieu de fournir des fonctionnalités de carte directement dans votre application, envisagez d’utiliser l’application Cartes Windows. Pour plus d’informations, voir [Lancer l’application CartesWindows](https://msdn.microsoft.com/library/windows/apps/mt228341).

![Exemple de l’application Cartes Windows.](images/mapnyc.png)

## Rubriques connexes

* [Exemple de carte UWP](http://go.microsoft.com/fwlink/p/?LinkId=619977)
* [Exemple de géolocalisation UWP](http://go.microsoft.com/fwlink/p/?linkid=533278)
* [Centre pour développeurs Bing Cartes](https://www.bingmapsportal.com/)
* [Obtenir l’emplacement actuel](get-location.md)
* [Recommandations en matière de conception pour les applications prenant en charge la géolocalisation](guidelines-and-checklist-for-detecting-location.md)
* [Recommandations en matière de conception pour les cartes](controls-map.md)
* [Recommandations en matière conception pour les applications prenant en charge la confidentialité](https://msdn.microsoft.com/library/windows/apps/hh768223)
* [Vidéos de la build 2015: utilisation de cartes et de localisation sur un téléphone, une tablette et un PC dans vos applications Windows](https://channel9.msdn.com/Events/Build/2015/2-757)
* [Exemple d’application de trafic UWP](http://go.microsoft.com/fwlink/p/?LinkId=619982)






<!--HONumber=Aug16_HO5-->


