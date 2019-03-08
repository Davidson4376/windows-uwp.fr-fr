---
title: Installer une application UWP à partir d’un serveur IIS
description: Ce didacticiel montre comment configurer un serveur IIS, vérifiez que votre application web peut héberger des packages d’application et appeler et utiliser efficacement de programme d’installation de l’application.
ms.date: 05/30/2018
ms.topic: article
keywords: Windows 10, uwp, programme d’installation de l’application, AppInstaller, effectuer un chargement indépendant, liées packages définis, facultatifs, le serveur IIS
ms.localizationpriority: medium
ms.openlocfilehash: 6a4512229a29a7adc59d6b61edd596eaeb56a5a8
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57623564"
---
# <a name="install-a-uwp-app-from-an-iis-server"></a>Installer une application UWP à partir d’un serveur IIS

Ce didacticiel montre comment configurer un serveur IIS, vérifiez que votre application web peut héberger des packages d’application et appeler et utiliser efficacement de programme d’installation de l’application.

L’application Programme d'installation d'application permet aux développeurs et professionnels de l’informatique de distribuer des applications Windows 10 en les hébergeant sur leurs propres Réseau de diffusion de contenu (CDN). Cela est utile pour les entreprises qui ne veulent pas ou n'ont pas besoin de publier leurs applications dans le Microsoft Store, mais qui souhaitent tirer parti de la plateforme Windows 10 de déploiement et de création de packages. 

## <a name="setup"></a>Configurer

Pour passer avec succès par le biais de ce didacticiel, vous devrez les éléments suivants :

1. Visual Studio 2017  
2. Outils de développement Web et IIS 
3. Package d’application UWP : le package d’application que vous allez distribuer

Facultatif : [Projet de démarrage](https://github.com/AppInstaller/MySampleWebApp) sur GitHub. Cela est utile si vous n’avez pas travailler avec des packages d’application, mais quand même apprendre à utiliser cette fonctionnalité.

## <a name="step-1---install-iis-and-aspnet"></a>Étape 1 : installer IIS et ASP.NET 

[Internet Information Services](https://www.iis.net/) est une fonctionnalité de Windows qui peut être installée via le menu Démarrer. Dans **menu Démarrer** recherchez **ou désactiver des fonctionnalités Windows activer**.

Recherchez et sélectionnez **Internet Information Services** pour installer IIS.

> [!NOTE]
> Vous n’avez pas besoin de sélectionner toutes les cases à cocher sous Internet Information Services. Uniquement celles sélectionnées lorsque vous vérifiez **Internet Information Services** sont suffisantes.

Vous devez également installer ASP.NET 4.5 ou ultérieure. Pour l’installer, recherchez **Internet Information Services -> World Wide Web Services -> fonctionnalités de développement d’applications**. Sélectionnez une version d’ASP.NET qui est supérieur ou égal à ASP.NET 4.5.

![Installation d’ASP.NET](images/install-asp.png)

## <a name="step-2---install-visual-studio-2017-and-web-development-tools"></a>Étape 2 : installer Visual Studio 2017 et les outils de développement Web 

[Installez Visual Studio 2017](https://docs.microsoft.com/visualstudio/install/install-visual-studio) si vous le n'avez pas déjà installé. Si vous disposez déjà de Visual Studio 2017, assurez-vous que les charges de travail suivants sont installés. Si les charges de travail ne sont pas présents sur votre installation, suivre la procédure à l’aide de Visual Studio Installer (trouvé dans le menu Démarrer).  

Pendant l’installation, sélectionnez **ASP.NET et développement Web** et des autres charges de travail qui vous intéresse. 

Une fois l’installation terminée, lancez Visual Studio et créez un nouveau projet (**fichier** -> **nouveau projet**).

## <a name="step-3---build-a-web-app"></a>Étape 3 : créer une application Web

Lancez Visual Studio 2017 en tant que **administrateur** et créez un nouveau **Visual C# Web Application** project avec une **vide** modèle de projet. 

![Nouveau projet](images/sample-web-app.png)

## <a name="step-4---configure-iis-with-our-web-app"></a>Étape 4 : Configurez IIS avec notre application Web 

Dans l’Explorateur de solutions, cliquez avec le bouton droit sur le projet racine et sélectionnez **propriétés**.

Dans les propriétés de l’application web, sélectionnez le **Web** onglet. Dans le **serveurs** , choisissez **IIS Local** dans la liste déroulante menu et cliquez sur **créer un répertoire virtuel**. 

![onglet Web](images/web-tab.png)

## <a name="step-5---add-an-app-package-to-a-web-application"></a>Étape 5 : ajout d’un package d’application à une application web 

Ajoutez le package d’application que vous vous apprêtez à distribuer dans l’application web. Vous pouvez utiliser le package d’application qui fait partie de la liste fournie [packages du projet starter](https://github.com/AppInstaller/MySampleWebApp/tree/master/MySampleWebApp/packages) sur GitHub si vous n’avez pas un package d’application. Le certificat (MySampleApp.cer) avec lequel le package a été signé se trouve également avec l’exemple sur GitHub. Vous devez disposer du certificat installé sur votre appareil avant d’installer l’application (étape 9).

Dans l’application web de projet de démarrage, un nouveau dossier a été ajouté à l’application web appelée `packages` qui contient les packages d’application à distribuer. Pour créer le dossier dans Visual Studio, un clic droit sur la racine de l’Explorateur de solutions, sélectionnez **ajouter** -> **nouveau dossier** et nommez-le `packages`. Pour ajouter des packages d’application dans le dossier, avec le bouton droit, cliquez sur le `packages` dossier et sélectionnez **ajouter** -> **élément existant...**  et accédez à l’emplacement de package d’application. 

![ajouter le package](images/add-package.png)

## <a name="step-6---create-a-web-page"></a>Étape 6 : créer une Page Web

Cet exemple d’application web utilise le code HTML simple. Vous êtes libre de créer votre application web comme requis par vos besoins. 

Cliquez avec le bouton droit sur le projet racine de l’Explorateur de solutions, sélectionnez **ajouter** -> **un nouvel élément**, puis ajoutez une nouvelle **HTML Page** à partir de la **Web**section.

Une fois que la page HTML est créée, cliquez avec le bouton droit sur la page HTML dans l’Explorateur de solutions et sélectionnez **définir comme Page de démarrage**.  

Double-cliquez sur le fichier HTML pour l’ouvrir dans la fenêtre d’éditeur de code. Dans ce didacticiel, seuls les éléments requis dans la page web pour appeler l’application de programme d’installation de l’application avec succès pour installer une application Windows 10 servira. 

Inclure le code HTML suivant dans votre page web. La clé pour appeler correctement le programme d’installation de l’application consiste à utiliser le modèle personnalisé qui programme d’installation de l’application s’enregistre avec le système d’exploitation : `ms-appinstaller:?source=`. Consultez l’exemple de code ci-dessous pour plus d’informations.

> [!NOTE]
> Vérifiez que le chemin d’URL spécifié après le schéma personnalisé correspond à l’Url de projet dans l’onglet web de votre solution Visual Studio.
 
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

## <a name="step-7---configure-the-web-app-for-app-package-mime-types"></a>Étape 7 : configurer l’application web pour les types MIME de package app

Ouvrez le **Web.config** fichier à partir de l’Explorateur de solutions, puis ajoutez les lignes suivantes dans le `<configuration>` élément. 

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

## <a name="step-8---add-loopback-exemption-for-app-installer"></a>Étape 8 : ajouter une exemption de bouclage pour le programme d’installation de l’application

En raison de l’isolement réseau, les applications UWP comme programme d’installation de l’application sont limitées à utiliser les adresses IP de bouclage comme http://localhost/. Lorsque vous utilisez le serveur IIS local, le programme d’installation de l’application doit être ajouté à la liste d’exempter de bouclage. 

Pour ce faire, ouvrez **invite de commandes** comme un **administrateur** et entrez les informations suivantes : ''' CheckNetIsolation.exe LoopbackExempt de ligne de commande - a-n="microsoft.desktopappinstaller_8wekyb3d8bbwe »
```

To verify that the app is added to the exempt list, use the following command to display the apps in the loopback exempt list: 
```Command Line
CheckNetIsolation.exe LoopbackExempt -s
```

Vous devriez trouver `microsoft.desktopappinstaller_8wekyb3d8bbwe` dans la liste.

Une fois la validation locale de l’installation de l’application via le programme d’installation de l’application est terminée, vous pouvez supprimer l’exemption du bouclage que vous avez ajoutée dans cette étape en :

' Ligne de commande CheckNetIsolation.exe LoopbackExempt -d-n="microsoft.desktopappinstaller_8wekyb3d8bbwe »
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
