---
author: normesta
Description: "Cet article rappelle les points à connaître avant de créer un package d'application avec le Pont du bureau. Le travail à fournir pour préparer votre application au processus de création de package peut être minime."
Search.Product: eADQiWindows 10XVcnh
title: "Préparation de la création de package d'application (Pont du bureau)"
ms.author: normesta
ms.date: 05/25/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.assetid: 71a57ca2-ca00-471d-8ad9-52f285f3022e
ms.openlocfilehash: f7337ee7bf78730e2300a11d7d606f328c7c418b
ms.sourcegitcommit: 77bbd060f9253f2b03f0b9d74954c187bceb4a30
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/11/2017
---
# <a name="prepare-to-package-an-app-desktop-bridge"></a>Préparation de la création de package d'application (Pont du bureau)

Cet article rappelle les points à connaître avant de créer un package d'application de bureau. Il est possible que vous n’ayez que peu à faire pour préparer votre application pour le processus de création de package. Toutefois, si l’un des éléments ci-dessous s’applique à votre application, vous devez y remédier au préalable. N’oubliez pas que WindowsStore se charge pour vous de la prise en charge du Gestionnaire de licences et des mises à jour automatiques, par conséquent, vous pouvez supprimer les fonctionnalités relatives à ces tâches de votre code de base.

+ __Votre application utilise une version de .NET antérieure à 4.6.1__. Seul .NET4.6.1 est pris en charge. Vous devez recibler votre application vers .NET4.6.1 avant de l'empaqueter.

+ __Votre application s’exécute toujours avec des privilèges élevés de sécurité__. Votre application a besoin de travailler lors de l’exécution en tant qu’utilisateur interactif. Les utilisateurs qui installent votre application depuis le Windows Store ne sont peut-être pas des administrateurs système. Par conséquent, exiger que votre application s’exécute avec élévation de privilèges signifie qu’elle ne s’exécutera pas correctement pour les utilisateurs standard.

+ __Votre application nécessite un pilote en mode noyau ou un service Windows__. Le pont est approprié pour une application, mais il ne prend pas en charge un pilote en mode noyau ou un service Windows devant être exécutés sous un compte système. Au lieu d’un service Windows, utilisez une [tâche en arrière-plan](https://msdn.microsoft.com/windows/uwp/launch-resume/create-and-register-a-background-task).

+ __Les modules de votre application sont chargés in-process sur les processus ne figurant pas dans votre package d’application Windows__. Cela n’est pas autorisé, ce qui signifie que les extensions in-process, par exemple les [extensions d’environnement](https://msdn.microsoft.com/library/windows/desktop/dd758089.aspx), ne sont pas prises en charge. Mais si vous disposez de deux applications dans le même package, vous pouvez créer une communication entre processus entre elles.

+ __Votre application utilise un ID de modèle utilisateur de l’application (AUMID) personnalisé__. Si votre processus appelle [SetCurrentProcessExplicitAppUserModelID](https://msdn.microsoft.com/library/windows/desktop/dd378422.aspx) pour définir son propre AUMID, il peut uniquement utiliser l’AUMID généré pour lui par l’environnement de modèle de l’application/du package d’application Windows. Vous ne pouvez pas définir d’AUMID personnalisés.

+ __Votre application modifie la ruche du Registre HKEY_LOCAL_MACHINE (HKLM)__. Toute tentative par votre application de créer une clé HKLM ou d’en ouvrir une pour modification donnera lieu à une erreur d’accès refusé. N’oubliez pas que votre application dispose de sa propre vue privée virtualisée du Registre. La notion de ruche du Registre de l’utilisateur ou de l’ordinateur (la définition de HKLM) ne s’applique pas. Vous devez trouver un autre moyen d’obtenir ce pour quoi vous utilisiez HKLM, comme l’écriture sur HKEY_CURRENT_USER (HKCU) à la place.

+ __Votre application utilise une sous-clé de Registre ddeexec comme moyen de lancement d’une autre application__. Utilisez plutôt l’un des gestionnaires de verbe DelegateExecute tel que configuré par les différentes extensions activables* dans votre [manifeste du package de l’application](https://msdn.microsoft.com/library/windows/apps/br211474.aspx).

+ __Votre application écrit dans le dossier AppData ou sur le Registre dans le but de partager des données avec une autre application__. Après la conversion, AppData est redirigé vers le magasin de données de l’application locale, qui est un magasin privé pour chaque application UWP.

  Toutes les entrées écrites par votre application dans la ruche du Registre HKEY_LOCAL_MACHINE sont redirigées vers un fichier binaire isolé et toutes les entrées écrites par votre application dans la ruche du Registre HKEY_CURRENT_USER sont placées dans un emplacement privé par utilisateur et par application. Pour plus d’informations sur la redirection de fichiers et du Registre, consultez [Fonctionnement détaillé de Pont du bureau](desktop-to-uwp-behind-the-scenes.md).  

  Utilisez un autre moyen de partager des données entre processus. Pour plus d’informations, voir [Stocker et récupérer des paramètres et autres données d’application](https://msdn.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data).

+ __Votre application écrit dans le répertoire d’installation de votre application__. Par exemple, votre application écrit dans un fichier journal que vous avez placé dans le même répertoire que votre fichier exe. Cela n’est pas pris en charge; vous devez donc trouver un autre emplacement, comme le magasin de données de l’application locale.

+ __L’installation de votre application requiert l’intervention de l’utilisateur__. Le programme d’installation de votre application doit être en mesure de s’exécuter en silence, et il doit installer tous les éléments requis qui ne sont pas disponibles par défaut sur une nouvelle image de système d’exploitation.

+ __Votre application utilise le répertoire de travail actuel__. Lors de l’exécution, votre application de bureau empaquetée ne dispose pas du même répertoire de travail que celui spécifié précédemment dans le raccourci .LNK sur votre bureau. Vous devez modifier votre répertoire de travail actuel lors de l’exécution si le fait de disposer du répertoire correct est important pour le bon fonctionnement de votre application.

+ __Votre application requiert UIAccess__. Si votre application spécifie `UIAccess=true` dans l’élément `requestedExecutionLevel` du manifeste de contrôle de compte d’utilisateur, la conversion vers UWP n’est pas prise en charge actuellement. Pour plus d’informations, consultez [Vue d’ensemble de la sécurité UI Automation](https://msdn.microsoft.com/library/ms742884.aspx).

+ __Votre application expose les objets COM__. Les processus et extensions peuvent, au sein du package, enregistrer et utiliser les serveurs COM et OLE, pour les modes in-process et out-of-process (OOP).  Creators Update ajoute la prise en charge des COM empaquetés qui offre la possibilité d’inscrire les serveurs OOP COM et OLE qui sont désormais visibles en dehors du package.  Voir [Prise en charge des documents COM et OLE pour le Pont du bureau](https://blogs.windows.com/buildingapps/2017/04/13/com-server-ole-document-support-desktop-bridge/#bjPyETFgtpZBGrS1.97).

   La prise en charge des COM empaquetés fonctionne avec les API COM existantes, mais elle ne fonctionne pas avec les extensions d’application qui s’appuient sur la lecture directe du registre, car l’emplacement des COM empaquetés est de type privé.

+ __Votre application expose des assemblys GAC pour que ces derniers soient utilisés par d’autres processus__. Dans la version actuelle, votre application ne peut pas exposer des assemblys GAC pour une utilisation par des processus issus de fichiers exécutables externes à votre package d’application Windows. Les processus issus du package peuvent enregistrer et utiliser des assemblys GAC normalement, mais ceux-ci ne seront pas visibles en externe. Cela signifie que les scénarios d’interopération, comme OLE, ne fonctionneront pas si ces derniers sont appelés par des processus externes.

+ __Votre application lie des bibliothèques runtimeC d’une manière non prise en charge__. La bibliothèque runtime C/C++ Microsoft fournit des routines de programmation pour le système d’exploitation MicrosoftWindows. Ces routines automatisent de nombreuses tâches de programmation courantes qui ne sont pas fournies par les langages C et C++. Si votre application utilise la bibliothèque runtime C/C++, vous devez vous assurer que la liaison de celle-ci est prise en charge.

    VisualStudio2015 prend en charge à la fois la liaison dynamique (pour que votre code puisse utiliser des fichiers DLL courants) et la liaison statique (pour lier la bibliothèque directement dans votre code) à la version actuelle du CRT. Si possible, nous vous recommandons d’utiliser la liaison dynamique avec VisualStudio2015 pour votre application.

    La prise en charge des versions antérieures de VisualStudio peut varier. Pour plus d’informations, voir le tableau suivant:

    <table>
    <th>Version de VisualStudio</td><th>Liaison dynamique</th><th>Liaison statique</th></th>
    <tr><td>2005 (VC8)</td><td>Non pris en charge</td><td>Pris en charge</td>
    <tr><td>2008 (VC9)</td><td>Non pris en charge</td><td>Pris en charge</td>
    <tr><td>2010 (VC10)</td><td>Prise en charge</td><td>Pris en charge</td>
    <tr><td>2012 (VC11)</td><td>Prise en charge</td><td>Non pris en charge</td>
    <tr><td>2013 (VC12)</td><td>Pris en charge</td><td>Non pris en charge</td>
    <tr><td>2015 (VC14)</td><td>Pris en charge</td><td>Pris en charge</td>
    </table>

    Remarque: dans tous les cas, vous devez créer une liaison vers la toute dernière version de CRT disponible publiquement.

+ __Votre application installe et charge des assemblys à partir du dossier Windows côte-à-côte__. Par exemple, votre application utilise des bibliothèques runtimeC VC8 ou VC9 et les lie dynamiquement à partir du dossier Windows côte-à-côte, ce qui signifie que votre code utilise les fichiers DLL courants à partir d’un dossier partagé. Ceci n’est pas pris en charge. Vous devez les lier de manière statique en créant dans votre code un lien direct vers les fichiers de bibliothèque redistribuables.

+ __Votre application utilise une dépendance dans le dossier System32/SysWOW64__. Pour que ces DLL fonctionnent, vous devez les inclure dans la partie du système de fichiers virtuel de votre package d’application Windows. Ainsi, l’application se comporte comme si les fichiersDLL ont été installés dans le dossier **System32**/**SysWOW64**. À la racine du package, créez un dossier appelé **VFS**. Dans ce dossier, créez les dossiers **SystemX64** et **SystemX86**. Ensuite, placez la version 32 bits de votre fichierDLL dans le dossier **SystemX86** et la version 64bits dans le dossier **SystemX64**.

+ __Votre application utilise le package d’infrastructure VCLibs11__. Les bibliothèques VCLibs11 peuvent être installées directement à partir du Windows Store si elles sont définies comme une dépendance dans le package d’application Windows. Pour ce faire, apportez la modification suivante dans le manifeste de votre package d’application: sous le nœud `<Dependencies>`, ajoutez:  
`<PackageDependency Name="Microsoft.VCLibs.110.00.UWPDesktop" MinVersion="11.0.24217.0" Publisher="CN=Microsoft Corporation, O=Microsoft Corporation, L=Redmond, S=Washington, C=US" />`  
Pendant l’installation à partir du Windows Store, la version appropriée (x86 ou x64) de l’infrastructure VCLibs11 est installée avant l’application.  
Les dépendances ne sont pas installées si l’application est installée en chargement indépendant. Pour installer les dépendances manuellement sur votre ordinateur, vous devez télécharger et installer les [packages d’infrastructure VC11.0 pour Desktop Bridge](https://www.microsoft.com/download/details.aspx?id=53340&WT.mc_id=DX_MVP4025064). Pour plus d’informations sur ces scénarios, consultez l’article [Utilisation de Runtime Visual C++ dans le projet Centennial](https://blogs.msdn.microsoft.com/vcblog/2016/07/07/using-visual-c-runtime-in-centennial-project/).

+ __Votre application contient une liste de raccourcis personnalisée__. Il existe plusieurs problèmes et mises en garde à connaître concernant l’utilisation des listes de raccourcis.

    - __L’architecture de votre application ne correspond pas au système d’exploitation.__  Pour l’instant, les listes de raccourcis ne fonctionnent pas correctement si l’architecture de l’application ne correspond à celle du système d’exploitation (par exemple, exécution d’une applicationx86 sur Windowsx64). À ce stade, la recompilation de votre application vers l’architecture correspondante constitue l’unique solution de contournement existante.

    - __Votre application crée des entrées de liste de raccourcis et appelle [ICustomDestinationList::SetAppID](https://msdn.microsoft.com/library/windows/desktop/dd378403(v=vs.85).aspx) ou [SetCurrentProcessExplicitAppUserModelID](https://msdn.microsoft.com/library/windows/desktop/dd378422(v=vs.85).aspx)__. Ne définissez pas votre AppID dans le code par programmation. Sinon, vos entrées de liste de raccourcis ne s’afficheront pas. Si vous devez définir un ID personnalisé pour votre application, faites-le en utilisant le fichier manifeste. Reportez-vous à [Créer un package d'application manuellement (Pont du bureau)](desktop-to-uwp-manual-conversion.md) pour obtenir des instructions. L’AppID de votre application est définie dans la section *YOUR_PRAID_HERE*.

    - __Votre application ajoute un lien shell de liste de raccourcis qui fait référence à un exécutable dans votre package__. Vous ne pouvez pas lancer des exécutables dans votre package directement à partir d’une liste de raccourcis (sauf pour le chemin d’accès absolu du propre fichier .exe d’une application). Au lieu de cela, inscrivez un alias d’exécution d’application (qui permet à votre application de bureau empaquetée d’être démarrée à l’aide d’un mot clé comme si elle était définie dans la variable PATH) et définissez le chemin cible du lien sur l’alias. Pour plus d’informations sur l’utilisation de l’extension appExecutionAlias, voir [Intégrer votre application avec Windows10 (Pont du bureau)](desktop-to-uwp-extensions.md). Notez que, pour mapper les ressources du lien dans la liste de raccourcis au fichier .exe d’origine, vous devez définir les ressources telles que l’icône avec [**SetIconLocation**](https://msdn.microsoft.com/library/windows/desktop/bb761047(v=vs.85).aspx) et définir le nom complet avec PKEY_Title, comme vous le feriez pour d’autres entrées personnalisées.

    - __Votre application ajoute des entrées de liste de raccourcis qui font référence aux ressources dans le package de l’application par des chemins absolus__. Le chemin d’installation d’une application peut changer lors d’une mise à jour de ses packages qui modifie l’emplacement des ressources (icônes, documents, exécutables, etc.). Si des entrées de liste de raccourcis font référence à ces ressources par des chemins absolus, l’application doit actualiser régulièrement sa liste de raccourcis (par exemple, au démarrage) pour garantir la résolution correcte des chemins. Vous pouvez aussi utiliser les API UWP [**Windows.UI.StartScreen.JumpList**](https://msdn.microsoft.com/library/windows/apps/windows.ui.startscreen.jumplist.aspx), qui vous permettent de faire référence à des ressources chaîne et image à l’aide du schéma d’URI ms-resource relatif au package (également avec prise en charge linguistique, DPI et du contraste élevé).

+ __Votre application démarre un utilitaire chargé d’accomplir certaines tâches__. Évitez de démarrer des utilitaires de commande tels que PowerShell et Cmd.exe. En fait, si les utilisateurs installent votre application sur un système fonctionnant sous Windows10S, elle ne sera pas du tout en mesure de les démarrer. Cela peut bloquer la soumission de votre application au Windows Store, car toutes les applications soumises au Windows Store doivent être compatibles avec Windows10 S.

Le démarrage d’un utilitaire constitue souvent un moyen pratique de chercher des informations dans le système d’exploitation, le registre ou les fonctionnalités d’accès au système. Toutefois, vous pouvez tout aussi bien utiliser les API UWP pour accomplir ces types de tâches. Ces API sont plus performantes, car elles n’ont besoin d’aucun fichier exécutable séparé pour s’exécuter, mais, plus important encore, elles empêchent l’application de sortir du package. La conception de l’application reste cohérente avec l’isolement, la confiance et la sécurité qui accompagnent une application Pont du bureau, et votre application se comportera comme prévu sur les systèmes exécutant Windows10 S.

+ __Votre application héberge des compléments, des modules ou des extensions__.   Dans de nombreux cas, les extensions de type COM continueront probablement à fonctionner tant qu’elles n’auront pas été empaquetées et elles s’installeront en toute confiance. La raison est que ces programmes d’installation peuvent utiliser leurs capacités de confiance totale pour modifier le registre et placer les fichiers d’extension partout où votre application hôte s’attend à les trouver.

   Toutefois, si ces extensions sont empaquetées, puis installées comme un package d’application Windows, elles ne fonctionneront pas car les packages (l’application hôte et l’extension) seront isolés les uns des autres. Pour en savoir davantage sur la méthode utilisée par le Pont du bureau pour isoler les applications du système, voir [Fonctionnement détaillé du Pont du bureau](desktop-to-uwp-behind-the-scenes.md).

 Toutes les applications et les extensions installées par des utilisateurs sur un système exécutant Windows10 S doivent être installées en tant que packages d’application Windows. Par conséquent, si vous envisagez de créer des packages de vos extensions ou si vous prévoyez d’encourager vos collaborateurs à les empaqueter, déterminez la méthode à employer pour faciliter la communication entre le package d’application hôte et tous les packages d’extension. L’utilisation d’un [service d’application](../launch-resume/app-services.md) peut être un bon moyen pour accomplir cette opération.

+ __Votre application génère du code__. Votre application peut générer du code qu’elle consomme en mémoire, toutefois, vous devez éviter d’écrire le code généré sur le disque car le processus de certification d’application Windows ne peut pas valider ce code avant la soumission de l’application. En outre, les applications qui écrivent du code sur le disque ne fonctionnent pas correctement sur les systèmes exécutant Windows10 S. Cela peut bloquer la soumission de votre application au Windows Store, car toutes les applications soumises au Windows Store doivent être compatibles avec Windows10 S.

+ __Votre application utilise l’API MAPI__. Actuellement, l’[API MAPI Outlook](https://msdn.microsoft.com/library/office/cc765775.aspx(d=robot)) n’est pas prise en charge dans les applications Pont du bureau.

## <a name="next-steps"></a>Étapes suivantes

**Créer un package d’application Windows pour votre application de bureau**

Voir [Créer un package d’application Windows](desktop-to-uwp-root.md#convert).

**Trouver des réponses aux questions spécifiques**

Notre équipe contrôle ces [balises StackOverflow](http://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge).

**Envoyer vos commentaires concernant cet article**

Utilisez la section remarques ci-dessous.
