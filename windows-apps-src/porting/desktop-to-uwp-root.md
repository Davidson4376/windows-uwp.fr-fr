---
author: awkoren
Description: "Préparez votre application de bureau Windows (Win32, WPF, Windows Forms) pour convertir une application UWP en utilisant les extensions de conversion de bureau."
Search.Product: eADQiWindows 10XVcnh
title: Convertir votre application de bureau en une application UWP
translationtype: Human Translation
ms.sourcegitcommit: ff8cd3ab5e38cfc3a2b5fabaad15f78a5f2620f2
ms.openlocfilehash: 6cf6367ed0f6acea0f87ac36a050e874425423b1

---

# Convertir votre application de bureau en une application UWP

\[Certaines informations concernent la version préliminaire de produits susceptibles d’être considérablement modifiés d’ici leur commercialisation. Microsoft ne donne aucune garantie, expresse ou implicite, concernant les informations fournies ici.\]

Préparez votre application de bureau Windows (Win32, WPF, Windows Forms) pour convertir une application UWP en utilisant les extensions de conversion de bureau.

## Avantages de la conversion de votre application vers UWP

Une plateforme Windows universelle (UWP) utilisant des extensions de conversion de bureau constitue un pont qui vous permet de convertir votre application de bureau Windows (comme Win32, Windows Forms et WPF) ou un jeu en une application ou un jeu de plateforme Windows universelle (UWP). Pour plus d’informations, voir le [Guide des applications UWP](https://msdn.microsoft.com/library/windows/apps/dn894631.aspx). Après la conversion, votre application de bureau Windows est empaquetée, soumise à maintenance et déployée sous la forme d’un package d’application UWP (un fichier.appx ou un package.appxbundle) ciblant Windows10 Desktop.

La technologie qui permet aux applications de bureau d’être converties en packages UWP est constituée de deux parties. La première est le convertisseur d’application de bureau, qui prend vos éléments binaires existants et les empaquète sous forme de package UWP. Votre code est toujours le même, il est simplement empaqueté différemment. La deuxième partie comprend les technologies d’exécution de la mise à jour anniversaire Windows qui permettent à un package UWP d’avoir des fichiers exécutables qui s’exécutent en confiance totale et non dans un conteneur d’application. En outre, cette technologie confère à une application convertie une identité de package, nécessaire pour utiliser certaines API UWP.

Voici quelques-uns des avantages de la conversion de votre application de bureau Windows.

* L’expérience d’installation de votre application est beaucoup plus fluide pour vos clients. Vous pouvez la déployer sur les ordinateurs à l’aide du chargement indépendant (consultez [Charger la version test d’applications métier dans Windows 10](https://technet.microsoft.com/library/mt269549.aspx)); elle ne laissera aucune trace après sa désinstallation. À long terme, vous serez également en mesure de publier votre application dans le Windows Store.

* Étant donné que votre application convertie dispose d’une identité de package, vous pouvez appeler plus d’API UWP que jamais, même à partir de la partition de confiance totale. Accédez à la liste complète des [API UWP prises en charge pour les applications de bureau converties](desktop-to-uwp-supported-api.md). 

* Vous pouvez ajouter des fonctionnalités UWP au package de votre application à votre propre rythme, comme une interface utilisateur XAML, des mises à jour de vignette en direct, des tâches en arrière-plan UWP, des services d’application, et bien plus encore. Toutes les fonctionnalités disponibles pour toute autre application UWP sont disponibles pour votre application.

* Si vous choisissez de déplacer l’ensemble des fonctionnalités de votre application hors de la partition de confiance totale de l’application et dans la partition de conteneur d’application, votre application pourra s’exécuter sur tout appareil Windows10.

* En tant qu’application UWP, votre application peut effectuer tout ce qu’elle savait faire comme application de bureau Windows. Elle interagit avec une vue virtualisée du Registre et du système de fichiers, impossible à distinguer du Registre et du système de fichiers réels.

* Votre application peut participer à la gestion de licences intégrées du Windows Store et aux solutions de mises à jour automatiques. La mise à jour automatique est un mécanisme extrêmement fiable et efficace, car seules les parties modifiées de fichiers sont téléchargées.

## Préparer votre application de bureau pour la conversion vers UWP

Il est possible que vous n’ayez que peu à faire pour préparer votre application pour le processus de conversion. N’oubliez pas que Windows Store gère le Gestionnaire de licences et les mises à jour automatiques pour vous, par conséquent, vous pouvez supprimer ces fonctionnalités de votre code de base. Si l’une de ces situations s’applique à votre application, vous devez résoudre ce problème avant la conversion.

+ __Votre application utilise une version de .NET antérieure à 4.6.1__. Seul .NET4.6.1 est pris en charge. Vous devez recibler votre application vers .NET4.6.1 avant la conversion. 

+ __Votre application s’exécute toujours avec des privilèges élevés de sécurité__. Votre application a besoin de travailler lors de l’exécution en tant qu’utilisateur interactif. Les utilisateurs qui installent votre application depuis le Windows Store ne sont peut-être pas des administrateurs système. Par conséquent, exiger que votre application s’exécute avec élévation de privilèges signifie qu’elle ne s’exécutera pas correctement pour les utilisateurs standard.

+ __Votre application nécessite un pilote en mode noyau ou un service Windows__. Le pont est approprié pour une application, mais il ne prend pas en charge un pilote en mode noyau ou un service Windows devant être exécutés sous un compte système. Au lieu d’un service Windows, utilisez une [tâche en arrière-plan](https://msdn.microsoft.com/windows/uwp/launch-resume/create-and-register-a-background-task).

+ __Les modules de votre application sont chargés in-process sur les processus ne figurant pas dans votre package AppX__. Cela n’est pas autorisé, ce qui signifie que les extensions in-process, par exemple les [extensions d’environnement](https://msdn.microsoft.com/library/windows/desktop/dd758089.aspx), ne sont pas prises en charge. Mais si vous disposez de deux applications dans le même package, vous pouvez créer une communication entre processus entre elles.

+ __Votre application appelle [SetDllDirectory](https://msdn.microsoft.com/library/windows/desktop/ms686203) ou [AddDllDirectory](https://msdn.microsoft.com/library/windows/desktop/hh310513)__. Ces fonctions ne sont actuellement pas prises en charge pour les applications converties. Nous travaillons sur l’ajout d’une prise en charge dans une version future. Pour contourner le problème, vous pouvez copier tous les fichiers.dll que vous localisiez à l’aide de ces fonctions vers la racine de votre package. 

+ __Votre application utilise un ID de modèle utilisateur de l’application (AUMID) personnalisé__. Si votre processus appelle [SetCurrentProcessExplicitAppUserModelID](https://msdn.microsoft.com/library/windows/desktop/dd378422.aspx) pour définir son propre AUMID, il peut uniquement utiliser l’AUMID généré pour lui par l’environnement de modèle de l’application/du package AppX. Vous ne pouvez pas définir d’AUMID personnalisés.

+ __Votre application modifie la ruche du Registre HKEY_LOCAL_MACHINE (HKLM)__. Toute tentative par votre application de créer une clé HKLM ou d’en ouvrir une pour modification donnera lieu à une erreur d’accès refusé. N’oubliez pas que votre application dispose de sa propre vue privée virtualisée du Registre. La notion de ruche du Registre de l’utilisateur ou de l’ordinateur (la définition de HKLM) ne s’applique pas. Vous devez trouver un autre moyen d’obtenir ce pour quoi vous utilisiez HKLM, comme l’écriture sur HKEY_CURRENT_USER (HKCU) à la place.

+ __Votre application utilise une sous-clé de Registre ddeexec comme moyen de lancement d’une autre application__. Utilisez plutôt l’un des gestionnaires de verbe DelegateExecute tel que configuré par les différentes extensions activables* dans votre [manifeste du package de l’application](https://msdn.microsoft.com/library/windows/apps/br211474.aspx).

+ __Votre application écrit dans le dossier AppData dans le but de partager des données avec une autre application__. Après la conversion, AppData est redirigé vers le magasin de données de l’application locale, qui est un magasin privé pour chaque application UWP. Utilisez un autre moyen de partager des données entre processus. Pour plus d’informations, voir [Stocker et récupérer des paramètres et autres données d’application](https://msdn.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data).

+ __Votre application écrit dans le répertoire d’installation de votre application__. Par exemple, votre application écrit dans un fichier journal que vous avez placé dans le même répertoire que votre fichier exe. Cela n’est pas pris en charge; vous devez donc trouver un autre emplacement, comme le magasin de données de l’application locale.

+ __L’installation de votre application requiert l’intervention de l’utilisateur__. Le programme d’installation de votre application doit être en mesure de s’exécuter en silence, et il doit installer tous les éléments requis qui ne sont pas disponibles par défaut sur une nouvelle image de système d’exploitation.

+ __Votre application utilise le répertoire de travail actuel__. Lors de l’exécution, votre application convertie ne dispose pas du même répertoire de travail que celui spécifié précédemment dans le raccourci .LNK sur votre bureau. Vous devez modifier votre répertoire de travail actuel lors de l’exécution si le fait de disposer du répertoire correct est important pour le bon fonctionnement de votre application.

+ __Votre application requiert UIAccess__. Si votre application spécifie `UIAccess=true` dans l’élément `requestedExecutionLevel` du manifeste de contrôle de compte d’utilisateur, la conversion vers UWP n’est pas prise en charge actuellement. Pour plus d’informations, voir [Vue d’ensemble de la sécurité UI Automation](https://msdn.microsoft.com/library/ms742884.aspx).

+ __Votre application est déjà une application UWP complète et veut appeler un processus de confiance totale à partir du package d’application__. L’utilisation du pont en «arrière» n’est pas prise en charge et la certification Windows Store des applications UWP tentant d’appeler des processus de confiance totale échoue. Pour appliquer ce scénario dans une application chargée de manière indépendante, commencez par inclure la déclaration ```<desktop:Extension>``` dans le manifeste de votre application (qui inclut l’attribut *EntryPoint* avec la valeur *Windows.FullTrustApplication*). Définissez ensuite la valeur ```<TargetDeviceFamily>``` de votre application sur *Windows.Desktop*. Cela indique que votre application cible uniquement la famille d’appareils de bureau, et non tous les appareils UWP. Si votre application retourne une erreur *APPX0501: Erreur de validation* avec ces paramètres en place, vérifiez votre famille d’appareils cible. Votre application cible probablement la famille *Windows.Universal*, et non *Windows.Desktop*. 

+ __Votre application expose des objets COM ou des assemblys GAC pour que ces derniers soient utilisés par d’autres processus__. Dans la version actuelle, votre application ne peut pas exposer des objets COM ou des assemblys GAC pour une utilisation par des processus issus de fichiers exécutables externes à votre package AppX. Les processus issus du package peuvent enregistrer et utiliser des objets COM et des assemblys GAC normalement, mais ceux-ci ne seront pas visibles en externe. Cela signifie que les scénarios d’interopération, comme OLE, ne fonctionneront pas si ces derniers sont appelés par des processus externes. 

+ __Votre application lie des bibliothèques Cruntime d’une manière non prise en charge__. La bibliothèque runtime C/C++ Microsoft fournit des routines de programmation pour le système d’exploitation MicrosoftWindows. Ces routines automatisent de nombreuses tâches de programmation courantes qui ne sont pas fournies par les langages C et C++. Si votre application utilise la bibliothèque runtime C/C++, vous devez vous assurer que la liaison de celle-ci est prise en charge. 
    
    VisualStudio2015 prend en charge à la fois la liaison dynamique (pour que votre code puisse utiliser des fichiers DLL courants) et la liaison statique (pour lier la bibliothèque directement dans votre code) à la version actuelle du CRT. Si possible, nous vous recommandons d’utiliser la liaison dynamique avec VisualStudio2015 pour votre application. 

    La prise en charge des versions antérieures de VisualStudio peut varier. Pour plus d’informations, voir le tableau suivant: 

    <table>
    <th>Version de VisualStudio</td><th>Liaison dynamique</th><th>Liaison statique</th></th>
    <tr><td>2005 (VC8)</td><td>Non pris en charge</td><td>Pris en charge</td>
    <tr><td>2008 (VC9)</td><td>Non pris en charge</td><td>Pris en charge</td>
    <tr><td>2010 (VC10)</td><td>Pris en charge</td><td>Pris en charge</td>
    <tr><td>2012 (VC11)</td><td>Prise en charge</td><td>Non pris en charge</td>
    <tr><td>2013 (VC12)</td><td>Prise en charge</td><td>Non pris en charge</td>
    <tr><td>2015 (VC14)</td><td>Pris en charge</td><td>Pris en charge</td>
    </table>
    
    Remarque: dans tous les cas, vous devez créer une liaison vers la toute dernière version de CRT disponible publiquement.

+ __Votre application installe et charge des assemblys à partir du dossier Windows côte-à-côte__. Par exemple, votre application utilise des bibliothèques RuntimeC VC8 ou VC9 et les lie dynamiquement à partir du dossier Windows côte-à-côte, ce qui signifie que votre code utilise les fichiers DLL courants à partir d’un dossier partagé. Ceci n’est pas pris en charge. Vous devez les lier statiquement en créant un lien vers les fichiers de bibliothèque redistribuables directement dans votre code.

## Démarrer le processus de conversion

Plusieurs options s’offrent à vous pour la conversion de votre application.

* **Desktop App Converter (DAC)**. DAC est un outil qui convertit et signe automatiquement votre application. L’utilisation de DAC est pratique et automatique; cet outil est très utile si votre application apporte beaucoup de modifications au système or en cas d’incertitude sur ce que fait votre programme d’installation. Pour commencer à utiliser DAC, voir [Desktop App Converter](desktop-to-uwp-run-desktop-app-converter.md). 

* **Conversion manuelle**. Si votre application est installée à l’aide de xcopy ou si vous connaissez les modifications que le programme d’installation de votre application apporte au système, la conversion manuelle peut être un choix plus simple. Cela implique la création d’un fichier manifeste, l’exécution de l’outil MakeAppx.exe et la signature de votre package d’application. Pour plus d’informations sur la façon d’effectuer une conversion manuelle, voir [Convertir manuellement votre application de bureau Windows en une application de plateforme Windows universelle (UWP)](desktop-to-uwp-manual-conversion.md). 

* **Programme d’installation tiers**. Trois programmes d’installation tiers populaires ([InstallShield de Flexera](http://www.flexerasoftware.com/producer/products/software-installation/installshield-software-installer), [WiX de FireGiant](https://www.firegiant.com/r/appx) et [Advanced Installer de Caphyon](http://www.advancedinstaller.com/uwp-app-package)) prennent maintenant en charge Desktop Bridge et peuvent générer à la fois un programme d’installation MSI et un package d’application convertie en quelques clics seulement. Pour plus d’informations, visitez le site web correspondant à chaque programme d’installation. 

## Obtenir du support et envoyer des commentaires

Si vous rencontrez des problèmes lors de la conversion de votre application, vous pouvez visiter les [forums](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/home?forum=wpdevelop) pour obtenir de l’aide. 

Pour transmettre des commentaires ou faire des suggestions sur les fonctionnalités, consultez le forum [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial). 

## Dans cette section

| Rubrique | Description |
|-------|-------------|
| [Desktop App Converter](desktop-to-uwp-run-desktop-app-converter.md) | Montre comment exécuter Desktop App Converter. Il est possible que vous n’ayez que peu (voire rien) à faire pour préparer votre application pour le processus de conversion. |
| [Convertir manuellement votre application de bureau](desktop-to-uwp-manual-conversion.md) | Découvrez comment créer un package et un manifeste d’application manuellement. |
| [Extensions d’applications de bureau converties](desktop-to-uwp-extensions.md) | Améliorez votre application de bureau convertie avec des extensions pour activer des fonctionnalités telles que les tâches de démarrage, l’intégration dans l’Explorateur de fichiers, etc. |
| [API UWP prises en charge pour les applications de bureau converties](desktop-to-uwp-supported-api.md) | Découvrez quelles API UWP peuvent être utilisées pour votre application de bureau convertie. |
| [Signer votre application de bureau convertie](desktop-to-uwp-signing.md) | Découvrez comment signer votre package.appx converti avec un certificat. |
| [Déployer et déboguer votre application UWP convertie](desktop-to-uwp-deploy-and-debug.md) | Obtenez de l’aide pour le déploiement et le débogage de votre application après sa conversion. En outre, si vous êtes curieux de connaître certains éléments internes des extensions de conversion de bureau, cette rubrique est faite pour vous. |
| [Pont d’application de bureau pour les exemples de code UWP](https://github.com/Microsoft/DesktopBridgeToUWP-Samples) | Exemples de code sur GitHub illustrant les fonctionnalités d’applications converties. |


<!--HONumber=Sep16_HO3-->


