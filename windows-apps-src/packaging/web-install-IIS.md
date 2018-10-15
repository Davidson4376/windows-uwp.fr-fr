---
author: laurenhughes
title: Installer une application UWP à partir d’un serveur IIS
description: Ce didacticiel montre comment configurer un serveur IIS, vérifiez que votre application web peut héberger des packages d’application et appeler et utiliser le programme d’installation d’application efficacement.
ms.author: cdon
ms.date: 05/30/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, programme d’installation de l’application, AppInstaller, charger de manière indépendante, liées à des packages définis, qui sont facultatifs, serveur IIS
ms.localizationpriority: medium
ms.openlocfilehash: 214ddd2b55bca1acecbab0a841cf2048335e7b3a
ms.sourcegitcommit: 106aec1e59ba41aae2ac00f909b81bf7121a6ef1
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/15/2018
ms.locfileid: "4610497"
---
# <a name="install-a-uwp-app-from-an-iis-server"></a>Installer une application UWP à partir d’un serveur IIS

Ce didacticiel montre comment configurer un serveur IIS, vérifiez que votre application web peut héberger des packages d’application et appeler et utiliser le programme d’installation d’application efficacement.

L’application Programme d'installation d'application permet aux développeurs et professionnels de l’informatique de distribuer des applications Windows10 en les hébergeant sur leurs propres Réseau de diffusion de contenu (CDN). Cela est utile pour les entreprises qui ne veulent pas ou n'ont pas besoin de publier leurs applications dans le Microsoft Store, mais qui souhaitent tirer parti de la plateforme Windows10 de déploiement et de création de packages. 

## <a name="setup"></a>Configuration

Pour passer correctement par le biais de ce didacticiel, vous devez les éléments suivants:

1. VisualStudio2017  
2. Outils de développement Web et IIS 
3. Package d’application UWP: le package d’application que vous allez distribuer

Facultatif: [Projet de démarrage](https://github.com/AppInstaller/MySampleWebApp) sur GitHub. Ceci est utile si vous n’avez pas travailler avec des packages d’application, mais que vous souhaitez savoir comment utiliser cette fonctionnalité.

## <a name="step-1---install-iis-and-aspnet"></a>Étape 1: installer IIS et ASP.NET 

[Internet Information Services](https://www.iis.net/) est une fonctionnalité de Windows qui peut être installée par le biais du menu Démarrer. Dans le **menu Démarrer** recherche **Windows activer ou désactiver des fonctionnalités**.

Recherchez et sélectionnez **Internet Information Services** pour installer IIS.

> [!NOTE]
> Vous n’avez pas besoin de sélectionner toutes les cases à cocher sous Internet Information Services. Uniquement celles sélectionnés lorsque vous vérifiez **Internet Information Services** suffisent.

Vous devez également installer ASP.NET 4.5 ou une version ultérieure. Pour l’installer, recherchez **Internet Information Services -> World Wide Web des Services -> fonctionnalités de développement d’applications**. Sélectionner une version d’ASP.NET qui est supérieure ou égale à ASP.NET 4.5.

![Installez ASP.NET](images/install-asp.png)

## <a name="step-2---install-visual-studio-2017-and-web-development-tools"></a>Étape 2: installer Visual Studio 2017 et les outils de développement Web 

[Installer Visual Studio 2017](https://docs.microsoft.com/visualstudio/install/install-visual-studio) si vous n’avez pas déjà installé il. Si vous avez déjà Visual Studio 2017, vous assurer que les charges de travail suivantes sont installés. Si les charges de travail ne sont pas présents sur votre installation, suivez le long à l’aide de Visual Studio Installer (disponibles dans le menu Démarrer).  

Lors de l’installation, sélectionnez le **développement Web ASP.NET et** et les autres charges de travail qui vous intéresse. 

Une fois que l’installation est terminée, démarrez Visual Studio et créez un nouveau projet (**fichier** -> **Nouveau projet**).

## <a name="step-3---build-a-web-app"></a>Étape 3: créer une application Web

Lancez Visual Studio 2017 en tant **qu’administrateur** et créez un nouveau projet **d’Application Web Visual c#** avec un modèle de projet **vide** . 

![Nouveau projet](images/sample-web-app.png)

## <a name="step-4---configure-iis-with-our-web-app"></a>Étape 4: configurer IIS avec notre application Web 

À partir de l’Explorateur de solutions, cliquez avec le bouton droit sur le projet racine, puis sélectionnez **Propriétés**.

Dans les propriétés de l’application web, sélectionnez l’onglet **Web** . Dans la section **serveurs** , choisissez **IIS Local** dans le menu déroulant, cliquez sur **Créer un répertoire virtuel**. 

![onglet Web](images/web-tab.png)

## <a name="step-5---add-an-app-package-to-a-web-application"></a>Étape 5: ajouter un package d’application pour une application web 

Ajoutez le package d’application que vous envisagez de distribuer les applications web. Vous pouvez utiliser le package d’application qui fait partie des fourni [des packages de projet de démarrage](https://github.com/AppInstaller/MySampleWebApp/tree/master/MySampleWebApp/packages) sur GitHub si vous n’avez pas un package d’application disponible. Le certificat (MySampleApp.cer) avec lequel le package a été signé se trouve également avec l’exemple sur GitHub. Vous devez disposer du certificat est installé sur votre appareil avant l’installation de l’application (étape 9).

Dans l’application web de projet de démarrage, un nouveau dossier a été ajouté à l’application web appelée `packages` qui contient les packages d’application doit être distribué. Pour créer le dossier dans Visual Studio, cliquez avec le bouton droit sur la racine de l’Explorateur de solutions, sélectionnez **Ajouter** -> **Nouveau dossier** et nommez-le `packages`. Pour ajouter des packages d’application dans le dossier, avec le bouton droit, cliquez sur le `packages` dossier et sélectionnez **Ajouter** -> emplacement de package**Élément existant …** et accédez à l’application. 

![Ajouter un package](images/add-package.png)

## <a name="step-6---create-a-web-page"></a>Étape 6: créer une Page Web

Cet exemple d’application web utilise le code HTML simple. Vous êtes libre créer votre application web en tant que nécessaires selon les besoins. 

Cliquez avec le bouton droit sur le projet racine de l’Explorateur de solutions, sélectionnez **Ajouter** -> **Un nouvel élément**, puis ajoutez une nouvelle **HTML Page** à partir de la section **Web** .

Une fois que la page HTML est créée, cliquez avec le bouton droit sur la page HTML dans l’Explorateur de solutions et sélectionnez **Définir comme Page de démarrage**.  

Double-cliquez sur le fichier HTML pour l’ouvrir dans la fenêtre de l’éditeur de code. Dans ce didacticiel, seuls les éléments dans le champ obligatoire dans la page web d’appeler l’application app Installer correctement pour installer une application Windows 10 seront utilisées. 

Incluez le code HTML suivant dans votre page web. La clé à l’appel avec succès le programme d’installation d’application consiste à utiliser le schéma personnalisé que le programme d’installation d’application inscrit avec le système d’exploitation: `ms-appinstaller:?source=`. Consultez l’exemple de code ci-dessous pour plus d’informations.

> [!NOTE]
> Vérifiez que le chemin d’accès de l’URL spécifiée après le schéma personnalisé correspond à l’Url du projet dans l’onglet web de votre solution Visual Studio.
 
```HTML
<html>
<head>
    <meta charset="utf-8" />
    <title> Install Page </title>
</head>
<body>
    <a href="ms-appinstaller:?source=http://localhost/SampleWebApp/packages/MySampleApp.appxbundle"> Install My Sample App</a>
</body>
</html>
```

## <a name="step-7---configure-the-web-app-for-app-package-mime-types"></a>Étape 7: configurer l’application web pour les types MIME package d’application

Ouvrez le fichier **Web.config** à partir de l’Explorateur de solutions et ajoutez les lignes suivantes au sein de le `<configuration>` élément. 

```xml
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
```

## <a name="step-8---add-loopback-exemption-for-app-installer"></a>Étape 8: ajouter l’exemption de bouclage pour le programme d’installation d’application

En raison de l’isolement réseau, les applications UWP comme programme d’installation d’application sont limitées à utiliser des adresses IP en boucle comme http://localhost/. Lorsque vous utilisez un serveur IIS local, le programme d’installation de l’application doit être ajouté à la liste exemption de bouclage. 

Pour ce faire, ouvrez l' **invite de commandes** en tant qu' **administrateur** et entrez les informations suivantes: ''' ligne de commande CheckNetIsolation.exe LoopbackExempt - a-n=microsoft.desktopappinstaller_8wekyb3d8bbwe
```

To verify that the app is added to the exempt list, use the following command to display the apps in the loopback exempt list: 
```Command Line
CheckNetIsolation.exe LoopbackExempt -s
```

Vous devez rechercher `microsoft.desktopappinstaller_8wekyb3d8bbwe` dans la liste.

Une fois la validation locale de l’installation d’application via le programme d’installation de l’application terminée, vous pouvez supprimer l’exemption de bouclage que vous avez ajouté dans cette étape par:

''' Ligne de commande CheckNetIsolation.exe LoopbackExempt -d-n=microsoft.desktopappinstaller_8wekyb3d8bbwe
```

## Step 9 - Run the Web App 

Build and run the web application by clicking on the run button on the VS Ribbon as shown in the image below:

![run](images/run.png)

A web page will open in your browser:

![web page](images/web-page.png)

Click on the link in the web page to launch the App Installer app and install your Windows 10 app package.


## Troubleshooting issues

### Not sufficient privilege 

If running the web app in Visual Studio displays an error such as "You do not have sufficient privilege to access IIS web sites on your machine", you will need to run Visual Studio as an administrator. Close the current instance of Visual Studio and reopen it as an admin.

### Set start page 

If running the web app causes the browser to load with an HTTP 403.14 - Forbidden error, it's because the web app doesn't have a defined start page. Refer to Step 6 in this tutorial to learn how to define a start page.
