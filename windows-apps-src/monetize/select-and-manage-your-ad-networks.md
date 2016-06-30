---
author: mcleanbyron
ms.assetid: 86D9D3CF-8FDC-4B67-881B-DF33A1BEE8BF
description: "Avant d’utiliser la médiation publicitaire, vous devez configurer des comptes auprès de chaque réseau publicitaire que vous souhaitez utiliser dans vos applications."
title: "Sélectionner et gérer vos réseaux publicitaires"
translationtype: Human Translation
ms.sourcegitcommit: ec7ce299545de8e5c167e1934fb9a0b4f4370948
ms.openlocfilehash: 49c9b8e60da9239c948265fb22563013019da259

---

# Sélectionner et gérer vos réseaux publicitaires


\[ Mise à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

Avant d’utiliser la médiation publicitaire, vous devez configurer des comptes auprès de chaque réseau publicitaire que vous souhaitez utiliser dans vos applications. Nous vous recommandons de procéder ainsi avant d’ajouter le contrôle Ad Mediator à votre projet Microsoft Visual Studio. Nous vous recommandons également de configurer des comptes avec autant de réseaux publicitaires que possible pour que vous disposiez de la flexibilité maximale pour optimiser les performances de médiation publicitaire.

## Réseaux publicitaires et types de projet pris en charge

Actuellement, la médiation publicitaire est prise en charge pour les réseaux publicitaires et types de projet suivants.

|  Réseau publicitaire    | Application de plateforme Windows universelle (UWP) en C# ou Visual Basic avec XAML | Application Windows 8.1 en C# ou Visual Basic avec XAML | Application Windows Phone 8.1 en C# ou Visual Basic avec XAML | Application Windows Phone 8.x Silverlight |               
|-------------------------------------------------------------------------|----------------------------------------------------|----------------------------------------------------------|-----------------------------------|---------------|
| [Microsoft](#microsoft)                         | **Pris en charge**                                      | **Pris en charge**                                            | **Pris en charge**                     | **Pris en charge** |
| [AdDuplex](#adduplex)                                                   | **Pris en charge**                                      | **Pris en charge**                                            | **Pris en charge**                     | **Pris en charge** |
| [Smaato](#smaato)                                                       | Non pris en charge                                      | **Pris en charge**                                            | **Pris en charge**                     | **Pris en charge** |
| [AdMob (Google)](#admob)                                                | Non pris en charge                                      | Non pris en charge                                            | Non pris en charge                     | **Pris en charge** |
| [Inneractive](#inneractive)                                             | Non pris en charge                                      | Non pris en charge                                            | Non pris en charge                     | **Pris en charge** |
| [Vserv VMAX](#vserv)                                                    | Non pris en charge                                      | Non pris en charge                                            | **Prise en charge**                     | Non pris en charge | 

Certains types de projet, tels que C++ avec XAML et JavaScript avec HTML, ne sont actuellement pas pris en charge par la médiation publicitaire. Pour ces types de projet, vous pouvez afficher des publicités de Microsoft sans utiliser de médiation publicitaire. Pour plus d’informations, voir [AdControl et XAML et .NET](https://msdn.microsoft.com/library/mt313186.aspx) et [AdControl en HTML 5 et JavaScript](https://msdn.microsoft.com/library/mt313130.aspx).

## Paramètres spécifiques et détails de chaque réseau


Voici les détails spécifiques dont vous aurez besoin pour chaque réseau publicitaire, notamment sur la configuration de votre compte et l’intégration d’une application. Nous répertorions également les paramètres que vous devez spécifier lorsque vous [soumettez votre application et configurez la médiation publicitaire](submit-your-app-and-configure-ad-mediation.md) (et/ou dans les services connectés pour [tester l’implémentation de votre médiation publicitaire](test-your-ad-mediation-implementation.md)). Pour plus d’informations sur l’utilisation d’un réseau publicitaire spécifique, visitez son site web.

Outre les paramètres obligatoires, chaque réseau publicitaire possède des paramètres facultatifs supplémentaires que vous pouvez définir via le code de votre application. Pour obtenir des exemples, voir [Paramètres facultatifs des réseaux publicitaires](#optionalparameters) plus loin dans cette rubrique. Pour obtenir la liste complète des paramètres facultatifs, consultez la documentation fournie par chaque réseau publicitaire. Certains de ces paramètres sont ignorés ou remplacés par la médiation publicitaire, et sont répertoriés dans les sections ci-dessous. Par exemple, les paramètres associés à l’emplacement actuel sont généralement remplacés par les informations sur l’emplacement du client, qui est déterminé par la fonctionnalité de localisation de l’application.

Notez que si vous [ajoutez le contrôle Ad Mediator](add-and-use-the-ad-mediator-control.md) et spécifiez les réseaux publicitaires à utiliser, Visual Studio tente de récupérer les assemblys requis par programme pour certains réseaux publicitaires. Si des assemblys ne peuvent pas être récupérés automatiquement, vous pouvez les installer en utilisant les liens ci-dessous correspondant à chaque réseau publicitaire.

### Microsoft

| Site web                        | Pour configurer les paramètres de réseau publicitaire, utilisez la page [Monétiser grâce aux publicités](https://msdn.microsoft.com/library/windows/apps/mt170658) sur le [tableau de bord du Centre de développement Windows](https://dev.windows.com/overview).   |
|--------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Emplacement du Kit de développement logiciel (SDK)                   | [SDK d’engagement et de monétisation de la Boutique Microsoft](http://aka.ms/store-em-sdk).                                                                                                                                                                                                                         |
| Intégration d’une application              | Ajoutez le contrôle de médiation publicitaire à votre application et soumettez-la au tableau de bord du Centre de développement Windows.                                                                                                                                                                                                            |
| Paramètres obligatoires            | ApplicationId et AdUnitId : ces paramètres sont automatiquement renseignés lorsque vous soumettez votre package d’application, en fonction du contenu de votre application. Toutefois, si vous le souhaitez, vous pouvez modifier ces paramètres lorsque vous [soumettez votre application et configurez la médiation publicitaire](submit-your-app-and-configure-ad-mediation.md). <br> <br> Height et Width (requis uniquement pour Windows Phone 8 Silverlight et Windows Phone 8.1 Silverlight).                                                                                                                                                                                                           |
| Paramètres remplacés/ignorés | Latitude (remplacé)  <br><br> Longitude (remplacé) <br><br> AutoRefreshIntervalInSeconds (ignoré) <br><br> IsAutoRefreshEnabled (ignoré) <br><br> IsAutoCollapsedEnabled (ignoré) <br><br> IsEngaged (ignoré) <br><br> IsSuspended (ignoré) |

 

### AdDuplex

| Site web                        | [http://adduplex.com](http://go.microsoft.com/fwlink/p/?LinkId=518028)      |
|--------------------------------|-----------------------------------------------------------------------------|
| Emplacement du Kit de développement logiciel (SDK)                   | Tout d’abord, laissez le contrôle Ad Mediator récupérer les assemblys via les services connectés en suivant la description indiquée dans [Ajouter et utiliser le contrôle de médiation publicitaire](add-and-use-the-ad-mediator-control.md). Si vous devez les télécharger manuellement, accédez à la section [Getting started](http://go.microsoft.com/fwlink/p/?LinkId=518029) du site web AdDuplex. |
| Intégration d’une application              | Dans le portail AdDuplex, créez une application.                                                                                                                                                                                                                                                                                                        |
| Paramètres requis            | AppKey <br><br> AdUnitId <br><br> Size (requis uniquement pour Windows 8.1 XAML)  |
| Paramètres remplacés/ignorés | Latitude (remplacé) <br><br> Longitude (remplacé) <br><br> AutoRefreshIntervalInSeconds (ignoré) <br><br> IsAutoRefreshEnabled (ignoré) <br><br> IsAutoCollapsedEnabled (ignoré) <br><br> IsEngaged (ignoré) <br><br> IsSuspended (ignoré) |
 

### Smaato

| Site web                        | [http://www.smaato.com/](http://go.microsoft.com/fwlink/p/?LinkId=518030)                                                                                                                                                                                                        |
|--------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Emplacement du Kit de développement logiciel (SDK)                   | Tout d’abord, laissez le contrôle Ad Mediator récupérer les assemblys via les services connectés en suivant la description indiquée dans [Ajouter et utiliser le contrôle de médiation publicitaire](add-and-use-the-ad-mediator-control.md). Si vous devez les télécharger manuellement, accédez à l’onglet Downloads du portail Smaato. |
| Intégration d’une application              | Dans le portail Smaato, accédez à My Spaces et générez un nouvel espace publicitaire.                                                                                                                                                                                                                |
| Paramètres obligatoires            | Pub <br> <br> Adspace <br> <br> Height et Width (requis uniquement pour Windows 8.1 XAML)  |
| Paramètres remplacés/ignorés | Gps (remplacé)                                                                                                                                                                                                                                                                |

 

### AdMob (Google)

| Site web                        | [http://apps.admob.com](http://go.microsoft.com/fwlink/p/?LinkId=518031)                                               |
|--------------------------------|------------------------------------------------------------------------------------------------------------------------|
| Emplacement du Kit de développement logiciel (SDK)                   | Obtenez le Kit de développement logiciel (SDK) Windows Phone 8 sur le [site web Google Mobile Ads SDK](http://go.microsoft.com/fwlink/p/?LinkId=518032). |
| Intégration d’une application              | Dans le portail AdMob, sélectionnez Monétiser une nouvelle application.                                                                          |
| Paramètres obligatoires            | AdUnitID <br> <br> Format                                                                                              |
| Paramètres remplacés/ignorés | Location (remplacé)  <br><br> ForceTesting (ignoré) <br><br> Refresh Rate (ignoré)                                |
 

### Inneractive

| Site web             | [http://inner-active.com](http://go.microsoft.com/fwlink/p/?LinkId=518035)                                                                                                                                                                                                                                                             |
|---------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Emplacement du Kit de développement logiciel (SDK)        | Tout d’abord, laissez le contrôle Ad Mediator récupérer les assemblys via les services connectés en suivant la description indiquée dans [Ajouter et utiliser le contrôle de médiation publicitaire](add-and-use-the-ad-mediator-control.md). Si vous devez les télécharger manuellement, connectez-vous à votre compte et accédez à l’onglet SDK dans le tableau de bord pour télécharger le Kit de développement logiciel (SDK) Windows Phone 8. |
| Intégration d’une application   | Dans le portail Inneractive, créez une application.                                                                                                                                                                                                                                                                                           |
| Paramètres obligatoires | AppID <br> <br> AdType (IaAdType_Banner ou IaAdType_Text)                                                                               |
 

### Vserv VMAX

| Site web             | [http://www.vmax.com](http://www.vmax.com)                                                                                                                                                                                                                                                                         |
|---------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Emplacement du Kit de développement logiciel (SDK)        | Tout d’abord, laissez le contrôle Ad Mediator récupérer les assemblys via les services connectés en suivant la description indiquée dans [Ajouter et utiliser le contrôle de médiation publicitaire](add-and-use-the-ad-mediator-control.md). Si vous devez les télécharger manuellement, accédez à la section [Kit de développement logiciel (SDK)](http://go.microsoft.com/fwlink/?LinkId=627078) du site web VMAX. |
| Intégration d’une application   | Récupérez l’ID Adspot de votre application sur le site web VMAX (cet ID est appelé ZoneId dans les API VMAX).                                                                                                                                                                                                                     |
| Paramètres obligatoires | ZoneId                                                                                                                                                                                                                                                                                                                         |12345

 

## Paramètres facultatifs des réseaux publicitaires


Outre les paramètres obligatoires, chaque réseau publicitaire possède des paramètres facultatifs supplémentaires que vous pouvez définir via le code de votre application. Pour obtenir la liste complète des paramètres facultatifs, consultez la documentation fournie par chaque réseau publicitaire. Pour définir ces paramètres facultatifs dans votre code, utilisez la propriété **AdSdkOptionalParameters** de votre objet **AdMediatorControl**.

L’exemple suivant montre comment définir la propriété [CountryOrRegion](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.countryorregion.aspx) de Microsoft Advertising en fonction du code de pays/région à deux lettres de l’utilisateur.

```CSharp
myAdMediatorControl.AdSdkOptionalParameters[AdSdkNames.MicrosoftAdvertising]["CountryOrRegion"] = "IN";
```

L’exemple suivant montre comment définir les paramètres **Width** et **Height** pour Smaato.

```CSharp
myAdMediatorControl.AdSdkOptionalParameters[AdSdkNames.Smaato]["Width"] = 300;
myAdMediatorControl.AdSdkOptionalParameters[AdSdkNames.Smaato]["Height"] = 250;
```

## Rubriques connexes

* [Ajouter et utiliser le contrôle de médiation publicitaire](add-and-use-the-ad-mediator-control.md)
* [Tester l’implémentation de votre médiation publicitaire](test-your-ad-mediation-implementation.md)
* [Soumettre votre application et configurer une médiation publicitaire](submit-your-app-and-configure-ad-mediation.md)
* [Résoudre les problèmes liés à la médiation publicitaire](troubleshoot-ad-mediation.md)
 

 



<!--HONumber=Jun16_HO4-->


