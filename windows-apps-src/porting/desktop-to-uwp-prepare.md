---
author: awkoren
Description: "Cet article rappelle les points à connaître avant de convertir une application avec Desktop to UWP Bridge. Il est possible que vous n’ayez que peu à faire pour préparer votre application pour le processus de conversion."
Search.Product: eADQiWindows 10XVcnh
title: "Préparer votre application pour Desktop to UWP Bridge"
translationtype: Human Translation
ms.sourcegitcommit: d22d51d52c129534f8766ab76e043a12d140e8b7
ms.openlocfilehash: a93d5ad1c1f429182c8df7d29df85dee70064e2f

---

# <a name="prepare-an-app-for-conversion-with-the-desktop-bridge"></a>Préparer une application pour la conversion avec Desktop Bridge

Cet article rappelle les points à connaître avant de convertir une application avec Desktop to UWP Bridge. Il est possible que vous n’ayez que peu à faire pour préparer votre application pour le processus de conversion. Toutefois, si l’un des éléments ci-dessous s’applique à votre application, vous devez y remédier au préalable. N’oubliez pas que Windows Store gère le Gestionnaire de licences et les mises à jour automatiques pour vous, par conséquent, vous pouvez supprimer ces fonctionnalités de votre code de base.

+ __Votre application utilise une version de .NET antérieure à 4.6.1__. Seul .NET 4.6.1 est pris en charge. Vous devez recibler votre application vers .NET 4.6.1 avant la conversion. 

+ __Votre application s’exécute toujours avec des privilèges élevés de sécurité__. Votre application a besoin de travailler lors de l’exécution en tant qu’utilisateur interactif. Les utilisateurs qui installent votre application depuis le Windows Store ne sont peut-être pas des administrateurs système. Par conséquent, exiger que votre application s’exécute avec élévation de privilèges signifie qu’elle ne s’exécutera pas correctement pour les utilisateurs standard.

+ __Votre application nécessite un pilote en mode noyau ou un service Windows__. Le pont est approprié pour une application, mais il ne prend pas en charge un pilote en mode noyau ou un service Windows devant être exécutés sous un compte système. Au lieu d’un service Windows, utilisez une [tâche en arrière-plan](https://msdn.microsoft.com/windows/uwp/launch-resume/create-and-register-a-background-task).

+ __Les modules de votre application sont chargés in-process sur les processus ne figurant pas dans votre package AppX__. Cela n’est pas autorisé, ce qui signifie que les extensions in-process, par exemple les [extensions d’environnement](https://msdn.microsoft.com/library/windows/desktop/dd758089.aspx), ne sont pas prises en charge. Mais si vous disposez de deux applications dans le même package, vous pouvez créer une communication entre processus entre elles.

+ __Votre application appelle [SetDllDirectory](https://msdn.microsoft.com/library/windows/desktop/ms686203) ou [AddDllDirectory](https://msdn.microsoft.com/library/windows/desktop/hh310513)__. Ces fonctions ne sont actuellement pas prises en charge pour les applications converties. Nous travaillons sur l’ajout d’une prise en charge dans une version future. Pour contourner le problème, vous pouvez copier tous les fichiers .dll que vous localisiez à l’aide de ces fonctions vers la racine de votre package. 

+ __Votre application utilise un ID de modèle utilisateur de l’application (AUMID) personnalisé__. Si votre processus appelle [SetCurrentProcessExplicitAppUserModelID](https://msdn.microsoft.com/library/windows/desktop/dd378422.aspx) pour définir son propre AUMID, il peut uniquement utiliser l’AUMID généré pour lui par l’environnement de modèle de l’application/du package AppX. Vous ne pouvez pas définir d’AUMID personnalisés.

+ __Votre application modifie la ruche du Registre HKEY_LOCAL_MACHINE (HKLM)__. Toute tentative par votre application de créer une clé HKLM ou d’en ouvrir une pour modification donnera lieu à une erreur d’accès refusé. N’oubliez pas que votre application dispose de sa propre vue privée virtualisée du Registre. La notion de ruche du Registre de l’utilisateur ou de l’ordinateur (la définition de HKLM) ne s’applique pas. Vous devez trouver un autre moyen d’obtenir ce pour quoi vous utilisiez HKLM, comme l’écriture sur HKEY_CURRENT_USER (HKCU) à la place.

+ __Votre application utilise une sous-clé de Registre ddeexec comme moyen de lancement d’une autre application__. Utilisez plutôt l’un des gestionnaires de verbe DelegateExecute tel que configuré par les différentes extensions activables* dans votre [manifeste du package de l’application](https://msdn.microsoft.com/library/windows/apps/br211474.aspx).

+ __Votre application écrit dans le dossier AppData dans le but de partager des données avec une autre application__. Après la conversion, AppData est redirigé vers le magasin de données de l’application locale, qui est un magasin privé pour chaque application UWP. Utilisez un autre moyen de partager des données entre processus. Pour plus d’informations, voir [Stocker et récupérer des paramètres et autres données d’application](https://msdn.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data).

+ __Votre application écrit dans le répertoire d’installation de votre application__. Par exemple, votre application écrit dans un fichier journal que vous avez placé dans le même répertoire que votre fichier exe. Cela n’est pas pris en charge ; vous devez donc trouver un autre emplacement, comme le magasin de données de l’application locale.

+ __L’installation de votre application requiert l’intervention de l’utilisateur__. Le programme d’installation de votre application doit être en mesure de s’exécuter en silence, et il doit installer tous les éléments requis qui ne sont pas disponibles par défaut sur une nouvelle image de système d’exploitation.

+ __Votre application utilise le répertoire de travail actuel__. Lors de l’exécution, votre application convertie ne dispose pas du même répertoire de travail que celui spécifié précédemment dans le raccourci .LNK sur votre bureau. Vous devez modifier votre répertoire de travail actuel lors de l’exécution si le fait de disposer du répertoire correct est important pour le bon fonctionnement de votre application.

+ __Votre application requiert UIAccess__. Si votre application spécifie `UIAccess=true` dans l’élément `requestedExecutionLevel` du manifeste de contrôle de compte d’utilisateur, la conversion vers UWP n’est pas prise en charge actuellement. Pour plus d’informations, consultez [Vue d’ensemble de la sécurité UI Automation](https://msdn.microsoft.com/library/ms742884.aspx).

+ __Votre application expose des objets COM ou des assemblys GAC pour que ces derniers soient utilisés par d’autres processus__. Dans la version actuelle, votre application ne peut pas exposer des objets COM ou des assemblys GAC pour une utilisation par des processus issus de fichiers exécutables externes à votre package AppX. Les processus issus du package peuvent enregistrer et utiliser des objets COM et des assemblys GAC normalement, mais ceux-ci ne seront pas visibles en externe. Cela signifie que les scénarios d’interopération, comme OLE, ne fonctionneront pas si ces derniers sont appelés par des processus externes. 

+ __Votre application lie des bibliothèques runtime C d’une manière non prise en charge__. La bibliothèque runtime C/C++ Microsoft fournit des routines de programmation pour le système d’exploitation Microsoft Windows. Ces routines automatisent de nombreuses tâches de programmation courantes qui ne sont pas fournies par les langages C et C++. Si votre application utilise la bibliothèque runtime C/C++, vous devez vous assurer que la liaison de celle-ci est prise en charge. 
    
    Visual Studio 2015 prend en charge à la fois la liaison dynamique (pour que votre code puisse utiliser des fichiers DLL courants) et la liaison statique (pour lier la bibliothèque directement dans votre code) à la version actuelle du CRT. Si possible, nous vous recommandons d’utiliser la liaison dynamique avec Visual Studio 2015 pour votre application. 

    La prise en charge des versions antérieures de Visual Studio peut varier. Pour plus d’informations, voir le tableau suivant : 

    <table>
    <th>Version de Visual Studio</td><th>Liaison dynamique</th><th>Liaison statique</th></th>
    <tr><td>2005 (VC 8)</td><td>Non pris en charge</td><td>Pris en charge</td>
    <tr><td>2008 (VC 9)</td><td>Non pris en charge</td><td>Pris en charge</td>
    <tr><td>2010 (VC 10)</td><td>Pris en charge</td><td>Pris en charge</td>
    <tr><td>2012 (VC 11)</td><td>Prise en charge</td><td>Non pris en charge</td>
    <tr><td>2013 (VC 12)</td><td>Prise en charge</td><td>Non pris en charge</td>
    <tr><td>2015 (VC 14)</td><td>Pris en charge</td><td>Pris en charge</td>
    </table>
    
    Remarque : dans tous les cas, vous devez créer une liaison vers la toute dernière version de CRT disponible publiquement.

+ __Votre application installe et charge des assemblys à partir du dossier Windows côte-à-côte__. Par exemple, votre application utilise des bibliothèques runtime C VC8 ou VC9 et les lie dynamiquement à partir du dossier Windows côte-à-côte, ce qui signifie que votre code utilise les fichiers DLL courants à partir d’un dossier partagé. Ceci n’est pas pris en charge. Vous devez les lier de manière statique en créant dans votre code un lien direct vers les fichiers de bibliothèque redistribuables.

+ __Votre application utilise une dépendance dans le dossier System32/SysWOW64__. Pour que ces DLL fonctionnent, vous devez les inclure dans la partie du système de fichiers virtuel de votre package AppX. Ainsi, l’application se comporte comme si les fichiers DLL ont été installés dans le dossier **System32**/**SysWOW64**. À la racine du package, créez un dossier appelé **VFS**. Dans ce dossier, créez les dossiers **SystemX64** et **SystemX86**. Ensuite, placez la version 32 bits de votre fichier DLL dans le dossier **SystemX86** et la version 64 bits dans le dossier **SystemX64**.

+ __Votre application utilise le package d’infrastructure VCLibs 11__. Les bibliothèques VCLibs 11 peuvent être installées directement à partir du Windows Store si elles sont définies comme une dépendance dans le package AppX. Pour ce faire, apportez la modification suivante dans le manifeste de votre package d’application : sous le nœud `<Dependencies>`, ajoutez :  
`<PackageDependency Name="Microsoft.VCLibs.110.00.UWPDesktop" MinVersion="11.0.24217.0" Publisher="CN=Microsoft Corporation, O=Microsoft Corporation, L=Redmond, S=Washington, C=US" />`  
Pendant l’installation à partir du Windows Store, la version appropriée (x86 ou x64) de l’infrastructure VCLibs 11 est installée avant l’application.  
Les dépendances ne sont pas installées si l’application est installée en chargement indépendant. Pour installer les dépendances manuellement sur votre ordinateur, vous devez télécharger et installer les [packages d’infrastructure VC 11.0 pour Desktop Bridge](https://www.microsoft.com/download/details.aspx?id=53340&WT.mc_id=DX_MVP4025064). Pour plus d’informations sur ces scénarios, consultez [Utilisation de Visual C++ Runtime dans le projet Centennial](https://blogs.msdn.microsoft.com/vcblog/2016/07/07/using-visual-c-runtime-in-centennial-project/).

+ __Votre application crée des entrées de liste de raccourcis et appelle [ICustomDestinationList::SetAppID](https://msdn.microsoft.com/library/windows/desktop/dd378403(v=vs.85).aspx) ou [SetCurrentProcessExplicitAppUserModelID](https://msdn.microsoft.com/library/windows/desktop/dd378422(v=vs.85).aspx)__. Ne définissez pas votre AppID dans le code par programmation. Sinon, vos entrées de liste de raccourcis ne s’afficheront pas. Si vous devez définir un ID personnalisé pour votre application, faites-le en utilisant le fichier manifeste. Pour obtenir des instructions, consultez [Convertir manuellement votre application en application UWP à l’aide de Desktop Bridge](desktop-to-uwp-manual-conversion.md). L’AppID de votre application est définie dans la section *YOUR_PRAID_HERE*. 

+ __Votre application ajoute un lien shell de liste de raccourcis qui fait référence à un exécutable dans votre package__. Vous ne pouvez pas lancer des exécutables dans votre package directement à partir d’une liste de raccourcis (sauf pour le chemin d’accès absolu du propre fichier .exe d’une application). Au lieu de cela, inscrivez un alias d’exécution d’application (qui permet à une application convertie d’être démarrée à l’aide d’un mot clé comme si elle était définie dans la variable PATH) et définissez le chemin cible du lien sur l’alias. Pour plus d’informations sur l’utilisation de l’extension appExecutionAlias, consultez [Extensions d’application Desktop Bridge](desktop-to-uwp-extensions.md). Notez que, pour mapper les ressources du lien dans la liste de raccourcis au fichier .exe d’origine, vous devez définir les ressources telles que l’icône avec [**SetIconLocation**](https://msdn.microsoft.com/library/windows/desktop/bb761047(v=vs.85).aspx) et définir le nom complet avec PKEY_Title, comme vous le feriez pour d’autres entrées personnalisées. 

+ __Votre application ajoute des entrées de liste de raccourcis qui font référence aux ressources dans le package de l’application par des chemins absolus__. Le chemin d’installation d’une application peut changer lors d’une mise à jour de ses packages qui modifie l’emplacement des ressources (icônes, documents, exécutables, etc.). Si des entrées de liste de raccourcis font référence à ces ressources par des chemins absolus, l’application doit actualiser régulièrement sa liste de raccourcis (par exemple, au démarrage) pour garantir la résolution correcte des chemins. Vous pouvez aussi utiliser les API UWP [**Windows.UI.StartScreen.JumpList**](https://msdn.microsoft.com/library/windows/apps/windows.ui.startscreen.jumplist.aspx), qui vous permettent de faire référence à des ressources chaîne et image à l’aide du schéma d’URI ms-resource relatif au package (également avec prise en charge linguistique, DPI et du contraste élevé). 


<!--HONumber=Dec16_HO1-->


