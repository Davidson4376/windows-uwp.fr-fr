---
author: awkoren
Description: "Cet article rappelle les points à connaître avant de convertir une application avec Desktop to UWP Bridge. Il est possible que vous n’ayez que peu à faire pour préparer votre application pour le processus de conversion."
Search.Product: eADQiWindows 10XVcnh
title: "Préparer votre application pour Desktop to UWP Bridge"
translationtype: Human Translation
ms.sourcegitcommit: 8429e6e21319a03fc2a0260c68223437b9aed02e
ms.openlocfilehash: 4cf9c509be52a8b2c03cdaa9ac68b98ba49b7094

---

# Préparer une application pour la conversion avec Desktop Bridge

Cet article rappelle les points à connaître avant de convertir une application avec Desktop to UWP Bridge. Il est possible que vous n’ayez que peu à faire pour préparer votre application pour le processus de conversion. Toutefois, si l’un des éléments ci-dessous s’applique à votre application, vous devez y remédier au préalable. N’oubliez pas que Windows Store gère le Gestionnaire de licences et les mises à jour automatiques pour vous, par conséquent, vous pouvez supprimer ces fonctionnalités de votre code de base.

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

+ __Votre application requiert UIAccess__. Si votre application spécifie `UIAccess=true` dans l’élément `requestedExecutionLevel` du manifeste de contrôle de compte d’utilisateur, la conversion vers UWP n’est pas prise en charge actuellement. Pour plus d’informations, consultez [Vue d’ensemble de la sécurité UI Automation](https://msdn.microsoft.com/library/ms742884.aspx).

+ __Votre application expose des objets COM ou des assemblys GAC pour que ces derniers soient utilisés par d’autres processus__. Dans la version actuelle, votre application ne peut pas exposer des objets COM ou des assemblys GAC pour une utilisation par des processus issus de fichiers exécutables externes à votre package AppX. Les processus issus du package peuvent enregistrer et utiliser des objets COM et des assemblys GAC normalement, mais ceux-ci ne seront pas visibles en externe. Cela signifie que les scénarios d’interopération, comme OLE, ne fonctionneront pas si ces derniers sont appelés par des processus externes. 

+ __Votre application lie des bibliothèques runtimeC d’une manière non prise en charge__. La bibliothèque runtime C/C++ Microsoft fournit des routines de programmation pour le système d’exploitation MicrosoftWindows. Ces routines automatisent de nombreuses tâches de programmation courantes qui ne sont pas fournies par les langages C et C++. Si votre application utilise la bibliothèque runtime C/C++, vous devez vous assurer que la liaison de celle-ci est prise en charge. 
    
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

+ __Votre application installe et charge des assemblys à partir du dossier Windows côte-à-côte__. Par exemple, votre application utilise des bibliothèques runtimeC VC8 ou VC9 et les lie dynamiquement à partir du dossier Windows côte-à-côte, ce qui signifie que votre code utilise les fichiers DLL courants à partir d’un dossier partagé. Ceci n’est pas pris en charge. Vous devez les lier statiquement en créant un lien vers les fichiers de bibliothèque redistribuables directement dans votre code.


<!--HONumber=Nov16_HO1-->


