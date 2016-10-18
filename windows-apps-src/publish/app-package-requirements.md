---
author: jnHs
Description: "Suivez ces instructions pour préparer les packages de votre application et les envoyer au Windows Store."
title: "Exigences relatives au package de l’application"
ms.assetid: 651B82BA-9D0C-45AC-8997-88CD93DC903C
translationtype: Human Translation
ms.sourcegitcommit: c15d4153f6ae83cc7bf1ae02d834bd07189e38ab
ms.openlocfilehash: 250e94c2766227cabad791db6d994bcfb1a2ac33

---

# Exigences relatives au package de l’application

Suivez ces instructions pour préparer les packages de votre application et les envoyer au Windows Store.

## Avant de générer le package de votre application pour le Windows Store

Veillez à [tester votre application à l’aide du Kit de certification des applications Windows](https://msdn.microsoft.com/library/windows/apps/mt186449). Nous vous recommandons également de tester votre application sur différents types de matériels. Notez que votre application ne peut être installée et exécutée que sur des ordinateurs disposant de licences de développeur, tant que nous ne l’avons pas certifiée et rendue accessible sur le Windows Store.

## Génération du package d’application à l’aide de Microsoft Visual Studio

Si vous utilisez Visual Studio comme environnement de développement, vous disposez déjà d’outils intégrés pour créer rapidement et facilement un package d’application. Pour plus d’informations, consultez [Empaquetage d’applications](https://msdn.microsoft.com/library/windows/apps/mt270969).

> **Remarque** Vérifiez que tous vos noms de fichiers utilisent le format ANSI. 


Quand vous créez votre package dans Visual Studio, vérifiez que vous êtes connecté au même compte que celui associé à votre compte de développeur. Les informations contenues dans certaines parties du manifeste du package font référence à votre compte. Ces informations sont automatiquement détectées et ajoutées.

Quand vous générez les packages de votre application, Visual Studio peut créer un fichier .appx ou .appxupload (ou encore un fichier .xap pour Windows Phone 8.1 et versions antérieures). Pour les applications qui ciblent Windows 10, chargez toujours le fichier .appxupload dans la page [Packages](upload-app-packages.md). Pour plus d’informations sur l’empaquetage d’applications pour UWP pour le Windows Store, consultez [Empaquetage d’applications Windows universelles pour Windows 10](http://go.microsoft.com/fwlink/p/?LinkId=620193 ).

Il n’est pas nécessaire que les packages de votre application soient signés avec un certificat provenant d’une autorité de certification approuvée.

### Ensembles d’applications

Pour les applications ciblant Windows 8.1, Windows Phone 8.1 et versions ultérieures, Visual Studio peut générer un ensemble d’applications (.appxbundle) afin de réduire la taille de l’application téléchargée par les utilisateurs. Cela peut être utile si vous avez défini des ressources propres à une langue, plusieurs ressources de mise à l’échelle d’images ou encore des ressources qui s’appliquent à des versions spécifiques de Microsoft DirectX.

> **Remarque** Une offre groupée d’applications peut contenir vos packages pour toutes les architectures. Vous ne devez soumettre qu’un seul ensemble par système d’exploitation ciblé.


Avec un ensemble d’applications, un utilisateur ne télécharge que les fichiers pertinents, et non toutes les ressources disponibles. Pour plus d’informations sur les ensembles d’applications, voir [Empaquetage d’applications](https://msdn.microsoft.com/library/windows/apps/mt270969) et [Empaquetage d’applications Windows universelles pour Windows 10](http://go.microsoft.com/fwlink/p/?LinkId=620193 ).

## Génération manuelle du package d’application

Si vous n’utilisez pas Visual Studio pour créer votre package, vous devez [créer manuellement votre manifeste de package](https://msdn.microsoft.com/library/windows/apps/br211476).

Pour obtenir des détails complets et les conditions requises concernant le manifeste, consultez la documentation [Manifeste du package d’application](https://msdn.microsoft.com/library/windows/apps/br211474). Pour obtenir la certification, votre manifeste doit suivre le schéma du manifeste du package.

Votre manifeste doit inclure des informations spécifiques concernant votre compte et votre application. Pour trouver ces informations, consultez [Visualiser les détails d’identité d’application](view-app-identity-details.md) dans la section **Gestion de l’application** de la page de présentation de votre application, dans le tableau de bord.

> **Remarque** Les valeurs du manifeste respectent la casse. Les espaces et autres symboles de ponctuation doivent également correspondre. Saisissez les valeurs correctement et vérifiez-les pour vous assurer qu’elles sont correctes.


Les ensembles d’applications utilisent un manifeste différent. Pour obtenir les détails et conditions requises concernant les manifestes d’offres groupées d’applications, consultez la documentation relative au [manifeste d’offre groupée](https://msdn.microsoft.com/library/windows/apps/dn263089).

> **Conseil** Veillez à exécuter le [Kit de certification des applications Windows](https://msdn.microsoft.com/library/windows/apps/mt186449) avant de soumettre vos packages. Vous pouvez ainsi déterminer si votre manifeste présente des problèmes susceptibles de faire échouer la certification ou la soumission.


Si votre application possède plusieurs packages, les éléments suivants du manifeste de l’application doivent être identiques dans chaque package (par système d’exploitation ciblé):

-   [**Package/Capacités**](https://msdn.microsoft.com/library/windows/apps/br211422)
-   [**Package/Dépendances**](https://msdn.microsoft.com/library/windows/apps/br211428)
-   [**Package/Ressources**](https://msdn.microsoft.com/library/windows/apps/br211462)

## Exigences relatives au format des packages

Les packages de votre application doivent être conformes aux exigences ci-après.

| Propriété du package de l’application | Exigence                                                          |
|----------------------|----------------------------------------------------------------------|
| Taille de package         | .appxbundle : 25 Go maximum par offre groupée <br>Packages .appx ciblant Windows 8.1 : 8 Go maximum par package <br> Packages .appx ciblant Windows 8 : 2 Go maximum par package <br> Packages .appx ciblant Windows Phone 8.1 : 4 Go maximum par package <br> Packages .xap : 1 Go maximum par package                                                                           |
| Hachages de mappage de bloc     | Algorithme SHA2-256                                                   |
 

## Fichier XML StoreManifest

Le fichier StoreManifest.xml est un fichier de configuration facultatif qui peut être inclus dans les packages d’application. Son objet est d’activer des fonctionnalités, par exemple en déclarant que votre application est une application pour périphériques du Windows Store ou en déclarant des critères dont dépend un package à appliquer à un appareil. Ces fonctionnalités ne sont pas couvertes par le manifeste du package. Le fichier StoreManifest.xml est envoyé avec le package d’application, et doit se situer dans le dossier racine du projet principal de votre application. Pour plus d’informations, voir [Schéma StoreManifest](https://msdn.microsoft.com/library/windows/apps/mt617325).

 

 







<!--HONumber=Aug16_HO3-->


