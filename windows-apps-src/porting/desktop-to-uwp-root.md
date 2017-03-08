---
author: awkoren
Description: Prise en main de Desktop to UWP Bridge et conversion de votre application de bureau Windows (Win32, WPF, Windows Forms) en application de plateforme Windows universelle (UWP).
Search.Product: eADQiWindows 10XVcnh
title: Porter votre application de bureau vers la plateforme Windows universelle (UWP) avec Pont du bureau
ms.author: alkoren
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp"
ms.assetid: 74373c24-f948-43bb-aa85-01e2e8e87162
translationtype: Human Translation
ms.sourcegitcommit: 5645eee3dc2ef67b5263b08800b0f96eb8a0a7da
ms.openlocfilehash: dd9f45b0ddcc201053ed8e35908da66443e47d72
ms.lasthandoff: 02/08/2017

---

# <a name="bring-your-desktop-app-to-the-universal-windows-platform-uwp-with-the-desktop-bridge"></a>Porter votre application de bureau vers la plateforme Windows universelle (UWP) avec Pont du bureau

Prise en main de Desktop to UWP Bridge et conversion de votre application de bureau Windows en application de plateforme Windows universelle (UWP).

Desktop Bridge regroupe un ensemble de technologies qui vous permettent de convertir votre application de bureau ou jeu Windows (par exemple, Win32, Windows Forms ou WPF) en application ou jeu UWP. Après la conversion, votre application de bureau Windows est empaquetée, soumise à maintenance et déployée sous la forme d’un package d’application UWP (un fichier .appx ou un package .appxbundle) ciblant Windows 10 Desktop.

La technologie qui permet aux applications de bureau d’être converties en packages UWP est constituée de deux parties. La première est le processus de conversion, qui prend vos éléments binaires existants et les empaquète sous forme de package UWP. Votre code est toujours le même, il est simplement empaqueté différemment. La deuxième partie comprend les technologies d’exécution de la mise à jour anniversaire Windows qui permettent à un package UWP d’avoir des fichiers exécutables qui s’exécutent en confiance totale et non dans un conteneur d’application. En outre, cette technologie confère à une application convertie une identité de package, nécessaire pour utiliser certaines API UWP.

## <a name="benefits"></a>Bénéfices

Voici quelques-uns des avantages de la conversion de votre application de bureau Windows : 

**Rationalisation du déploiement**. Le pont offre aux applications et jeux une expérience de déploiement exceptionnelle qui garantit aux utilisateurs des installations et des mises à jour fiables. Si l’utilisateur choisit de désinstaller l’application, il est entièrement supprimé sans laisser aucune trace. Cela réduit le temps consacré à la création d’expériences d’installation et à l’actualisation des utilisateurs.

**Mises à jour automatiques et licences**. Votre application peut participer à la gestion de licences intégrées du Windows Store et aux solutions de mises à jour automatiques. La mise à jour automatique est un mécanisme extrêmement fiable et efficace, car seules les parties modifiées de fichiers sont téléchargées.

**Augmentation de la portée et simplification de la monétisation**. Choisir une distribution via le Windows Store permet de toucher des millions d’utilisateurs de Windows 10 en mesure d’acquérir des applications, des jeux et d’effectuer des achats dans l’application à l’aide d’options de paiement locales.

**Ajout de fonctionnalités UWP**.  Vous pouvez ajouter des fonctionnalités UWP au package de votre application à votre propre rythme, comme une interface utilisateur XAML, des mises à jour de vignette en direct, des tâches en arrière-plan UWP, des services d’application, et bien plus encore.

**Élargissement des cas d’utilisation sur l’appareil**. Le pont vous permet de faire migrer graduellement leur code vers la plateforme Windows universelle pour atteindre chaque appareil Windows 10, y compris les téléphones, la console Xbox One et le casque HoloLens.

## <a name="prepare"></a>Préparation

Desktop to UWP Bridge est conçu pour simplifier l’utilisation. Vous n’avez pas beaucoup d’opérations à effectuer pour préparer votre application au processus de conversion. Toutefois, avant la conversion, vous devez connaître les avertissements et situations uniques. Consultez l’article [Préparer votre application pour Desktop to UWP Bridge](desktop-to-uwp-prepare.md) et résolvez les problèmes qui s’appliquent à votre application avant de poursuivre.

## <a name="convert"></a>Convertir

Plusieurs options s’offrent à vous pour la conversion de votre application.

**Desktop App Converter (DAC)**. DAC est un outil qui convertit et signe automatiquement votre application. L’utilisation de DAC est pratique et automatique ; cet outil est très utile si votre application apporte beaucoup de modifications au système or en cas d’incertitude sur ce que fait votre programme d’installation. Pour débuter, consultez l’article relatif à [Desktop App Converter](desktop-to-uwp-run-desktop-app-converter.md). 

**Conversion manuelle**. Si votre application est installée à l’aide de xcopy ou si vous connaissez les modifications que le programme d’installation de votre application apporte au système, la conversion manuelle peut être un choix plus simple. Cela implique la création d’un fichier manifeste, l’exécution de l’outil MakeAppx.exe et la signature de votre package d’application. Pour plus d’informations sur la conversion manuelle, consultez [Convertir manuellement votre application en UWP à l’aide de Desktop Bridge](desktop-to-uwp-manual-conversion.md). 

**Programme d’installation tiers**. Plusieurs produits et programmes d’installation tiers populaires prennent maintenant en charge Desktop Bridge et peuvent générer des programmes d’installation MSI ou des packages d’application convertis en quelques clics seulement. Voici quelques exemples : 

* [InstallShield de Flexera](http://www.flexerasoftware.com/producer/products/software-installation/installshield-software-installer)
* [WiX de FireGiant](https://www.firegiant.com/r/appx) 
* [Advanced Installer de Caphyon](http://www.advancedinstaller.com/uwp-app-package)
* [RAD Studio d’Embarcadero](https://www.embarcadero.com/products/rad-studio/windows-10-store-desktop-bridge) 
* [InstallAware](https://www.installaware.com/appx.htm)

Pour plus d’informations, visitez le site web correspondant à chaque programme d’installation. 

## <a name="enhance"></a>Améliorer 

Vous pouvez valoriser votre application de bureau convertie avec un large éventail d’API UWP pour ajouter des fonctionnalités telles que des vignettes, des notifications push et bien d’autres. Pour des exemples de code complets, consultez les référentiels [Exemples de pont d’application de bureau en UWP](https://github.com/Microsoft/DesktopBridgeToUWP-Samples) et [Exemples d’applications de plateforme Windows universelle (UWP)](https://github.com/Microsoft/Windows-universal-samples) sur GitHub. Pour afficher une liste complète des API prises en charge, passez en revue [API UWP prises en charge pour les applications de bureau converties avec Desktop Bridge](desktop-to-uwp-supported-api.md). 

En plus d’appeler des API UWP, votre application peut être étendue avec des fonctionnalités accessibles uniquement aux applications converties. Ceci inclut les scénarios tels que le lancement d’un processus lors de la connexion de l’utilisateur et l’intégration dans l’Explorateur de fichiers, et sont conçues pour faciliter la transition entre l’application de bureau d’origine et un package d’application UWP complet. Pour plus d’informations, consultez [Extensions d’application Desktop Bridge](desktop-to-uwp-extensions.md). 

## <a name="migrate"></a>Migrer

À l’aide du pont, vous pouvez migrer progressivement votre ancien code vers UWP tout en conservant la possibilité d’exécuter et de publier votre application sur le Bureau Windows. Une fois la migration vers UWP entièrement terminées (et que votre application ne contient plus de composants WPF/Win32), vous pouvez atteindre tous les appareils Windows, y compris les téléphones, Xbox One et HoloLens.

## <a name="debug"></a>Déboguer

Vous pouvez déboguer votre application à l’aide de Visual Studio. Consultez [Déboguer des applications converties avec Desktop Bridge](desktop-to-uwp-debug.md) pour une aide détaillée. 

Si les détails internes du fonctionnement interne de Desktop Bridge vous intéressent, consultez [Fonctionnement détaillé de Desktop Bridge](desktop-to-uwp-behind-the-scenes.md). 

## <a name="distribute"></a>Distribuer

Vous pouvez distribuer votre application à l’aide du Windows Store ou via un chargement indépendant. Pour des détails complets, consultez [Distribuer des applications converties avec Desktop Bridge](desktop-to-uwp-distribute.md). N’oubliez pas que vous devez signer votre application avant de la déployer pour les utilisateurs. Pour des instructions détaillées, consultez [Signer une application convertie avec Desktop Bridge](desktop-to-uwp-signing.md). 

## <a name="support-and-feedback"></a>Support et commentaires

Si vous rencontrez des problèmes lors de la conversion de votre application, vous pouvez visiter les [forums](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/home?forum=wpdevelop) pour obtenir de l’aide. 

Pour transmettre des commentaires ou faire des suggestions sur les fonctionnalités, soumettez ou publiez un vote favorable sur [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial). 

## <a name="in-this-section"></a>Dans cette section

| Rubrique | Description |
|-------|-------------|
| [Desktop App Converter](desktop-to-uwp-run-desktop-app-converter.md) | Montre comment exécuter Desktop App Converter. |
| [Convertir manuellement votre application en UWP à l’aide de Desktop Bridge](desktop-to-uwp-manual-conversion.md) | Découvrez comment créer un package et un manifeste d’application manuellement. |
| [Extensions d’application Desktop Bridge](desktop-to-uwp-extensions.md) | Améliorez votre application de bureau convertie avec des extensions pour activer des fonctionnalités telles que les tâches de démarrage et l’intégration dans l’Explorateur de fichiers. |
| [API UWP prises en charge pour les applications converties avec Desktop Bridge](desktop-to-uwp-supported-api.md) | Découvrez les API UWP qui sont utilisables pour votre application de bureau convertie. |
| [Guide d’empaquetage Pont du bureau pour les applications de bureau .NET avec Visual Studio](desktop-to-uwp-packaging-dot-net.md) | Configurez votre solution Visual Studio de façon à pouvoir modifier, déboguer et empaqueter votre application .NET. | 
| [Déboguer des applications converties avec Pont du bureau](desktop-to-uwp-debug.md) | Explique les options de débogage de votre application convertie. | 
| [Signer une application convertie avec Desktop Bridge](desktop-to-uwp-signing.md) | Découvrez comment signer votre package d’application converti avec un certificat. |
| [Distribuer des applications converties avec Desktop Bridge](desktop-to-uwp-distribute.md) | Découvrez comment distribuer votre application convertie aux utilisateurs.  |
| [Fonctionnement détaillé de Desktop Bridge](desktop-to-uwp-behind-the-scenes.md) | Découvrez de manière plus approfondie le fonctionnement interne de Desktop to UWP Bridge. | 
| [Problèmes connus avec Desktop Bridge](desktop-to-uwp-known-issues.md) | Répertorie les problèmes connus avec Desktop to UWP Bridge. | 
| [Exemples de code de pont d’application de bureau en UWP](https://github.com/Microsoft/DesktopBridgeToUWP-Samples) | Exemples de code sur GitHub illustrant les fonctionnalités d’applications converties. |
