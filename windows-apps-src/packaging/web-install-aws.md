---
title: Hébergement de packages d’application UWP sur AWS pour l’installation web
description: Didacticiel de configuration de serveur web AWS valider l’installation de l’application via le programme d’installation de l’application
ms.date: 05/30/2018
ms.topic: article
keywords: Windows 10, Windows 10, UWP, chargement de version test programme d’installation, AppInstaller, application, de lots définis, facultatifs, AWS associés
ms.localizationpriority: medium
ms.openlocfilehash: 53fe01a1c1a825377e886e042b4eef3868cbf5eb
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57628054"
---
# <a name="hosting-uwp-app-packages-on-aws-for-web-install"></a>Hébergement de packages d’application UWP sur AWS pour l’installation web

L’application Programme d'installation d'application permet aux développeurs et professionnels de l’informatique de distribuer des applications Windows 10 en les hébergeant sur leurs propres Réseau de diffusion de contenu (CDN). Cela est utile pour les entreprises qui ne veulent pas ou n'ont pas besoin de publier leurs applications dans le Microsoft Store, mais qui souhaitent tirer parti de la plateforme Windows 10 de déploiement et de création de packages.

Cette rubrique décrit les étapes pour configurer un site Web d’Amazon Web Services (AWS) pour héberger des packages application UWP et comment utiliser l’application du programme d’installation de l’application pour installer les packages d’application.

## <a name="setup"></a>Configurer

Pour suivre correctement ce didacticiel, vous aurez besoin des éléments suivants :
 
1. Abonnement AWS 
2. Page Web
3. Package d’application UWP : le package d’application que vous allez distribuer

Facultatif : [Projet de démarrage](https://github.com/AppInstaller/MySampleWebApp) sur GitHub. Cela est utile si vous n’avez pas de package d’application ou de page web à utiliser, mais que vous souhaitez apprendre à utiliser cette fonctionnalité.

Ce didacticiel passe en revue la configuration d’une page web et héberger des packages sur AWS. Cette opération nécessite un abonnement AWS. En fonction de la mise à l’échelle de votre opération, vous pouvez utiliser leur appartenance gratuit pour suivre ce didacticiel. 

## <a name="step-1---aws-membership"></a>Étape 1 - appartenance AWS
Pour obtenir un abonnement AWS, visitez le [page Détails du compte AWS](https://aws.amazon.com/free/). Dans le cadre de ce didacticiel, vous pouvez utiliser un abonnement gratuit.

## <a name="step-2---create-an-amazon-s3-bucket"></a>Étape 2 : créer un compartiment Amazon S3

Amazon Simple Storage Service (S3) est une offre pour recueillir, stocker et analyse des données AWS. S3 compartiments sont un moyen pratique d’héberger des packages application UWP et des pages web pour la distribution. 

Après vous être connecté à AWS avec vos informations d’identification sous `Services` trouver `S3`. 

Sélectionnez **créer compartiment**, puis entrez un **nom du compartiment** pour votre site Web. Suivez les invites de la boîte de dialogue pour définir les propriétés et les autorisations. Pour vous assurer que votre application UWP peut être distribuée à partir de votre site Web, vous devez activer **en lecture** et **écrire** autorisations pour votre compartiment, puis sélectionnez **accorder l’accès en lecture public pour ce compartiment** .

![Définir des autorisations sur le compartiment Amazon S3](images/aws-permissions.png) 

Passez en revue le résumé de s’assurer que les options sélectionnées sont répercutées. Cliquez sur **créer compartiment** pour terminer cette étape. 

## <a name="step-3---upload-uwp-app-package-and-web-pages-to-an-s3-bucket"></a>Étape 3 : télécharger le package d’application UWP et des pages web dans un compartiment S3

Un que vous avez créé un compartiment Amazon S3, vous serez en mesure de voir dans votre vue Amazon S3. Voici un exemple de l’aspect de compartiment de notre démonstration :

![Vue de compartiment Amazon S3](images/aws-post-create.png)

Nous sommes maintenant prêts à télécharger les packages d’application et les pages web que nous aimerions pour héberger notre compartiment Amazon S3. 

Cliquez sur le compartiment nouvellement créé à charger du contenu. Le compartiment est actuellement vide, car rien n’a encore été chargé. Cliquez sur le **télécharger** bouton et sélectionnez les packages d’application et les fichiers de page web que vous voulez charger.

> [!NOTE]
> Vous pouvez utiliser le package de l’application qui fait partie du référentiel [Projet de démarrage](https://github.com/AppInstaller/MySampleWebApp) sur GitHub si vous n’avez pas de package de l’application. Le certificat (MySampleApp.cer) avec lequel le package a été signé se trouve également avec l’exemple sur GitHub. Le certificat doit être installé sur votre appareil avant que vous puissiez installer l’application.

![Télécharger le package d’application](images/aws-upload-package.png)

Comme pour les autorisations pour la création d’un compartiment Amazon S3, le contenu dans le compartiment devez également **lire**, **écrire**, et **accorder l’accès en lecture public pour cet objet (s)** autorisations.

Si vous souhaitez tester le téléchargement d’une page web, mais que vous n’en avez pas, vous pouvez utiliser l’exemple de page html (default.html) à partir de la [projet de démarrage](https://github.com/AppInstaller/MySampleWebApp/blob/master/MySampleWebApp/default.html).

> [!IMPORTANT]
> Avant de télécharger la page web, vérifiez que la référence de package d’application dans votre page web est correcte. 

Pour obtenir la référence de package d’application, charger d’abord le package d’application et copiez l’URL de package d’application. Modifier la page web html afin de refléter le chemin d’accès du package application appropriée. Consultez l’exemple de code pour plus d’informations. 

Sélectionnez le fichier de package d’application téléchargée pour obtenir le lien de référence au package d’application, il doit être similaire à ceci :

![chemin d’accès du package chargé](images/aws-package-path.png)

**Copie** le lien vers l’application du package et ajouter la référence dans votre page web. 

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
Chargez le fichier html à votre compartiment Amazon S3. N’oubliez pas de définir les autorisations pour permettre **lire** et **écrire** accès.

## <a name="step-4---test"></a>Étape 4 : Test

Une fois la page web est chargé dans votre compartiment Amazon S3, obtient le lien vers la page web en sélectionnant le fichier html téléchargé.

Utilisez le lien pour ouvrir la page web. Dans la mesure où nous définissons les autorisations à accorder l’accès public pour le package d’application et de la page web, toute personne avec le lien vers la page web sera en mesure d’y accéder et installer vos packages d’application UWP à l’aide du programme d’installation de l’application. Notez que le programme d’installation de l’application fait partie de la plateforme Windows 10. En tant que développeur, il est inutile ajouter n’importe quel code supplémentaire ou des fonctionnalités à votre application pour activer l’utilisation du programme d’installation de l’application. 

## <a name="troubleshooting"></a>Résolution des problèmes

### <a name="app-installer-fails-to-install"></a>Programme d’installation de l’application ne parvient pas à installer 

Installation de l’application échoue si le certificat signé avec le package d’application n’est pas installé sur l’appareil. Pour résoudre ce problème, vous devez installer le certificat avant l’installation de l’application. Si vous hébergez un package d’application pour la distribution publique, il est recommandé de signer votre package d’application avec un certificat à partir d’une autorité de certification. 


