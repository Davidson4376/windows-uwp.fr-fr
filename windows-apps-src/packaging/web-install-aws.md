---
title: Hébergement de packages d’application UWP sur AWS pour l’installation web
description: Didacticiel pour la configuration de serveur web AWS valider l’installation de l’application via le programme d’installation d’une application
ms.date: 05/30/2018
ms.topic: article
keywords: Windows 10, Windows 10, UWP, charger de manière indépendante du programme d’installation, AppInstaller, application, liés packages définis, facultatifs, AWS
ms.localizationpriority: medium
ms.openlocfilehash: 53fe01a1c1a825377e886e042b4eef3868cbf5eb
ms.sourcegitcommit: 89ff8ff88ef58f4fe6d3b1368fe94f62e59118ad
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/30/2018
ms.locfileid: "8211721"
---
# <a name="hosting-uwp-app-packages-on-aws-for-web-install"></a>Hébergement de packages d’application UWP sur AWS pour l’installation web

L’application Programme d'installation d'application permet aux développeurs et professionnels de l’informatique de distribuer des applications Windows10 en les hébergeant sur leurs propres Réseau de diffusion de contenu (CDN). Cela est utile pour les entreprises qui ne veulent pas ou n'ont pas besoin de publier leurs applications dans le Microsoft Store, mais qui souhaitent tirer parti de la plateforme Windows10 de déploiement et de création de packages.

Cette rubrique décrit les étapes permettant de configurer un site Web Amazon Web Services (AWS) pour héberger des packages app UWP et comment utiliser l’application programme d’installation de l’application pour installer les packages d’application.

## <a name="setup"></a>Configuration

Pour suivre correctement ce didacticiel, vous aurez besoin des éléments suivants:
 
1. Abonnement AWS 
2. Page Web
3. Package d’application UWP: le package d’application que vous allez distribuer

Facultatif: [Projet de démarrage](https://github.com/AppInstaller/MySampleWebApp) sur GitHub. Cela est utile si vous n’avez pas de package d’application ou de page web à utiliser, mais que vous souhaitez apprendre à utiliser cette fonctionnalité.

Ce didacticiel empruntera comment configurer une page web et des packages d’hôte. Cela nécessite un abonnement AWS. En fonction de l’échelle de votre opération, vous pouvez utiliser leur abonnement gratuit à suivre ce didacticiel. 

## <a name="step-1---aws-membership"></a>Étape 1: l’appartenance AWS
Pour obtenir un abonnement AWS, visitez la [page de détails du compte AWS](https://aws.amazon.com/free/). Dans le cadre de ce didacticiel, vous pouvez utiliser un abonnement gratuit.

## <a name="step-2---create-an-amazon-s3-bucket"></a>Étape 2: créer un compartiment Amazon S3

Amazon Simple Storage Service (S3) est une AWS offre pour recueillir, stocker et analyser les données. Compartiments S3 sont un moyen pratique de packages d’application UWP hôte et les pages web pour la distribution. 

Après la connexion à AWS avec vos informations d’identification, sous `Services` trouver `S3`. 

Sélectionnez le **compartiment créer**et entrez un **nom de la plage** de votre site Web. Suivez les instructions de la boîte de dialogue pour définir les propriétés et des autorisations. Pour vous assurer que votre application UWP peut être distribuée à partir de votre site Web, activez **lire** et **écrire** des autorisations pour votre compartiment et sélectionnez **accorder l’accès en lecture publique à ce profil d’appel**.

![Définir des autorisations sur Amazon S3 compartiment](images/aws-permissions.png) 

Passez en revue le résumé pour vous assurer que les options sélectionnées sont répercutées. Cliquez sur le **compartiment créer** pour cette étape est terminée. 

## <a name="step-3---upload-uwp-app-package-and-web-pages-to-an-s3-bucket"></a>Étape 3: télécharger le package d’application UWP et les pages web pour une plage de S3

L’une vous avez créé un compartiment S3 Amazon, vous serez en mesure de visualiser dans votre affichage S3 Amazon. Voici un exemple de quoi ressemble notre compartiment de démonstration:

![Affichage de compartiment Amazon S3](images/aws-post-create.png)

Nous sommes maintenant prêts à charger les packages d’application et des pages web que nous souhaiterions pour héberger dans notre compartiment Amazon S3. 

Cliquez sur le compartiment nouvellement créé pour charger du contenu. Le compartiment est actuellement vide dans la mesure où rien n’a été encore chargé. Cliquez sur le bouton **Télécharger** , puis sélectionnez les packages d’application et les fichiers de page web que vous souhaitez télécharger.

> [!NOTE]
> Vous pouvez utiliser le package de l’application qui fait partie du référentiel [Projet de démarrage](https://github.com/AppInstaller/MySampleWebApp) sur GitHub si vous n’avez pas de package de l’application. Le certificat (MySampleApp.cer) avec lequel le package a été signé se trouve également avec l’exemple sur GitHub. Le certificat doit être installé sur votre appareil avant que vous puissiez installer l’application.

![Télécharger le package d’application](images/aws-upload-package.png)

Comme pour les autorisations pour la création d’un compartiment Amazon S3, le contenu dans le compartiment doit également avoir **lire**, **écrire**et autorisations **accorder l’accès en lecture publique à cet objet (s)** .

Si vous souhaitez tester le chargement d’une page web, mais que vous n’en avez pas, vous pouvez utiliser l’exemple de page html (default.html) à partir du [Projet de démarrage](https://github.com/AppInstaller/MySampleWebApp/blob/master/MySampleWebApp/default.html).

> [!IMPORTANT]
> Avant de le charger la page web, vérifiez que la référence de package d’application dans votre page web est correcte. 

Pour obtenir la référence de package d’application, télécharger le package d’application tout d’abord et copiez l’URL de package d’application. Modifier la page web html afin de refléter le chemin de package d’application approprié. Consultez l’exemple de code pour plus d’informations. 

Sélectionnez le fichier de package d’application téléchargé pour obtenir le lien de référence pour le package d’application, il doit être identique à cet exemple:

![chemin d’accès du package téléchargé](images/aws-package-path.png)

**Copier** le lien vers l’application de package et ajoutez la référence dans votre page web. 

```html
<html>
    <head>
        <meta charset="utf-8" />
        <title> Install My Sample App</title>
    </head>
    <body>
        <a href="ms-appinstaller:?source=https://s3-us-west-2.amazonaws.com/appinstaller-aws-demo/MySampleApp.appxbundle"> Install My Sample App</a>
    </body>
</html>
```
Charger le fichier html dans votre compartiment Amazon S3. Pensez à définir des autorisations pour autoriser l’accès **lecture** et **écriture** .

## <a name="step-4---test"></a>Étape 4: Test

Une fois que la page web est chargée dans votre compartiment Amazon S3, obtenez le lien vers la page web en sélectionnant le fichier html téléchargé.

Utilisez le lien pour ouvrir la page web. Dans la mesure où nous avons défini des autorisations pour accorder l’accès public pour le package d’application et de la page web, quiconque disposant du lien vers la page web sera en mesure d’y accéder et d’installer vos packages d’application UWP à l’aide du programme d’installation de l’application. Notez que le programme d’installation d’application fait partie de la plateforme Windows 10. En tant que développeur, vous n’avez pas besoin d’ajouter n’importe quel code supplémentaires ou des fonctionnalités à votre application pour permettre l’utilisation du programme d’installation de l’application. 

## <a name="troubleshooting"></a>Résolution des problèmes

### <a name="app-installer-fails-to-install"></a>Programme d’installation de l’application ne parvient pas à installer 

Installation d’une application échouera si le certificat signé avec le package d’application n’est pas installé sur l’appareil. Pour résoudre ce problème, vous devez installer le certificat avant l’installation de l’application. Si vous hébergez un package d’application pour une distribution publique, il est recommandé de signer votre package d’application avec un certificat à partir d’une autorité de certification. 


