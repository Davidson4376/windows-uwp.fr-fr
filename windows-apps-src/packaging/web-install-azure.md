---
author: c-don
title: Installation d'une application UWP depuis un serveur web Azure
description: Ce didacticiel montre comment configurer un serveur web Azure, vérifier que votre application web peut héberger des packages d’application et appeler et utiliser le Programme d'installation d'application efficacement.
ms.author: cdon
ms.date: 09/30/2018
ms.topic: article
keywords: windows10, uwp, programme d’installation d’application, appinstaller, charger une version test, ensemble connexe, packages facultatifs, serveur web Azure
ms.localizationpriority: medium
ms.openlocfilehash: b7ea002686199b992af45af775f53c96fd108a13
ms.sourcegitcommit: cd00bb829306871e5103db481cf224ea7fb613f0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/31/2018
ms.locfileid: "5879768"
---
# <a name="install-a-uwp-app-from-an-azure-web-app"></a>Installer une application UWP à partir d’une application Web Azure

L’application Programme d'installation d'application permet aux développeurs et professionnels de l’informatique de distribuer des applications Windows10 en les hébergeant sur leurs propres Réseau de diffusion de contenu (CDN). Cela est utile pour les entreprises qui ne veulent pas ou n'ont pas besoin de publier leurs applications dans le Microsoft Store, mais qui souhaitent tirer parti de la plateforme Windows10 de déploiement et de création de packages.

Cette rubrique explique comment configurer un serveur web Azure afin d'héberger des packages d'application UWP et comment utiliser l’application Programme d'installation d'application pour installer des packages d’application.

Dans ce didacticiel, nous voyons comment configurer un serveur IIS pour vérifier localement que votre application web peut correctement héberger les packages d’application et appeler et utiliser efficacement l’application Programme d'installation d'application. Nous aurons également des didacticiels montrant comment héberger correctement vos applications web sur les services de cloud web populaires dans le champ (Azure et AWS) afin de vérifier qu’elles respectent la configuration requise par le Programme d'installation d'application pour réaliser des installations web. Ce didacticiel pas à pas ne nécessite aucune compétence particulière et est très facile à suivre. 

## <a name="setup"></a>Configuration

Pour suivre correctement ce didacticiel, vous aurez besoin des éléments suivants:
 
1. un abonnement à Microsoft Azure 
2. Package d’application UWP: le package d’application que vous allez distribuer

Facultatif: [Projet de démarrage](https://github.com/AppInstaller/MySampleWebApp) sur GitHub. Cela est utile si vous n’avez pas de package d’application ou de page web à utiliser, mais que vous souhaitez apprendre à utiliser cette fonctionnalité.

### <a name="step-1---get-an-azure-subscription"></a>Étape1: obtenir un abonnement Azure
Pour obtenir un abonnement Azure, visitez la [page de compte Azure](https://azure.microsoft.com/free/). Dans le cadre de ce didacticiel, vous pouvez utiliser un abonnement gratuit.

### <a name="step-2---create-an-azure-web-app"></a>Étape2: créer une application web Azure 
Dans la page du portail Azure, cliquez sur le bouton **+ Créer une ressource**, puis sélectionnez **Application Web**

![créer](images/azure-create-app.png)

Créez un **nom d’application** unique et laissez le reste des champs par défaut. Cliquez sur **Créer** pour terminer l’Assistant de création d’application web. 

![créer une application web](images/azure-create-app-2.png)

### <a name="step-3---hosting-the-app-package-and-the-web-page"></a>Étape3: héberger le package d’application et la page web 
Une fois que l’application web a été créée, vous pouvez y accéder à partir du tableau de bord sur le portail Azure. Dans cette étape, nous allons créer une page web simple avec l’interface graphique utilisateur du portail Azure.

Après avoir sélectionné l’application web qui vient d’être créée depuis le tableau de bord, utilisez le champ de recherche pour rechercher et ouvrir **l'Éditeur App Service**. 

Dans l’éditeur, il y a un fichier `hostingstart.html` par défaut. Cliquez avec le bouton droit dans l’espace vide du volet de l'explorateur de fichiers et sélectionnez **Télécharger les fichiers** pour commencer le chargement des packages de votre application.

> [!NOTE]
> Vous pouvez utiliser le package de l’application qui fait partie du référentiel [Projet de démarrage](https://github.com/AppInstaller/MySampleWebApp) sur GitHub si vous n’avez pas de package de l’application. Le certificat (MySampleApp.cer) avec lequel le package a été signé se trouve également avec l’exemple sur GitHub. Le certificat doit être installé sur votre appareil avant que vous puissiez installer l’application.

![chargement](images/azure-upload-file.png)

Cliquez avec le bouton droit dans l’espace vide du volet de l'explorateur de fichiers et sélectionnez **Nouveaux fichiers** pour créer un nouveau fichier. Nommez le fichier: `default.html`.

Si vous utilisez le package de l’application fourni dans le [Projet de démarrage](https://github.com/AppInstaller/MySampleWebApp), copiez le code HTML suivant sur la page web `default.html` nouvellement créée. Si vous utilisez votre propre package de l’application, modifiez l’URL du service d’application (l’URL après `source=`). Vous pouvez obtenir l’URL du service d’application à partir de la page de vue d’ensemble de votre application dans le portail Azure.

```html
<html>
<head>
    <meta charset="utf-8" />
    <title> Install My Sample App</title>
</head>
<body>
    <a href="ms-appinstaller:?source=https://appinstaller-azure-demo.azurewebsites.net/MySampleApp.appxbundle"> Install My Sample App</a>
</body>
</html>
```

### <a name="step-4---configure-the-web-app-for-app-package-mime-types"></a>Étape4: configurer l’application web pour les types de package de l'application MIME

Ajoutez un nouveau fichier à l'application web nommé: `Web.config`. Ouvrez le fichiers `Web.config` à partir de l’Explorateur et ajoutez les lignes suivantes. 

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <!--This is to allow the web server to serve resources with the appropriate file extension-->
    <staticContent>
      <mimeMap fileExtension=".appx" mimeType="application/appx" />
      <mimeMap fileExtension=".msix" mimeType="application/msix" />
      <mimeMap fileExtension=".appxbundle" mimeType="application/appxbundle" />
      <mimeMap fileExtension=".msixbundle" mimeType="application/msixbundle" />
      <mimeMap fileExtension=".appinstaller" mimeType="application/appinstaller" />
    </staticContent>
  </system.webServer>
</configuration>
```

### <a name="step-5---run-and-test"></a>Étape5: Exécuter et tester

Pour lancer la page web que vous avez créée, utilisez l’URL de l’étape3dans le navigateur, suivie par `/default.html`. 

![edge](images/edge.png)

Cliquez sur «Installer mon exemple d’application» pour lancer le Programme d'installation d'application et installer votre package d’application. 

## <a name="troubleshooting-issues"></a>Résolution des problèmes

### <a name="app-installer-app-fails-to-install"></a>L'application Programme d'installation d'application ne parvient pas à installer 
L'installation de l’application échouera si le certificat avec lequel le package de l’application est signé n’est pas installé sur l’appareil. Pour résoudre ce problème, vous devez installer le certificat avant l’installation de l’application. Si vous hébergez un package de l’application pour une distribution publique, nous vous recommandons de signer votre package de l’application avec un certificat émis par une autorité de certification. 

![certificat de l’application](images/aws-app-cert.png)

### <a name="nothing-happens-when-you-click-the-link"></a>Rien ne se produit lorsque vous cliquez sur le lien 
Assurez-vous que l’application Programme d'installation d'application est installée. Accédez à **Paramètres** -> **Applications et fonctionnalités** et recherchez le Programme d'installation d'application dans la liste des applications installées. 

