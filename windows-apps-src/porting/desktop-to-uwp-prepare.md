---
Description: This article lists things you need to know before packaging your desktop application. You may not need to do much to get your app ready for the packaging process.
Search.Product: eADQiWindows 10XVcnh
title: Préparer le package d’une application de bureau (pont du bureau)
ms.date: 05/18/20188
ms.topic: article
keywords: windows10, uwp
ms.assetid: 71a57ca2-ca00-471d-8ad9-52f285f3022e
ms.localizationpriority: medium
ms.openlocfilehash: b58ba8122d5db6cb898c5e6258794960cdc1c1f9
ms.sourcegitcommit: b11f305dbf7649c4b68550b666487c77ea30d98f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/27/2018
ms.locfileid: "7832493"
---
# <a name="prepare-to-package-a-desktop-application"></a>Préparer le package d’une application de bureau

Cet article rappelle les points à connaître avant de créer un package d'application de bureau. Vous devrez pas faire grand chose pour préparer votre application pour le processus de création de packages, mais si un des éléments ci-dessous s’applique à votre application, vous devez y remédier au préalable. N’oubliez pas que le MicrosoftStore se charge pour vous de la prise en charge du Gestionnaire de licences et des mises à jour automatiques, par conséquent, vous pouvez supprimer les fonctionnalités relatives à ces tâches de votre code de base.

>[!IMPORTANT]
>La possibilité de créer un package d’application Windows pour votre application de bureau (également appelé le pont du bureau) a été introduite dans Windows 10, version 1607, et peut être utilisé uniquement dans les projets qui ciblent la mise à jour anniversaire Windows 10 (version 10.0; Build 14393) ou une version ultérieure dans Visual Studio.

+ __Votre application nécessite une version de .NET antérieure à la version 4.6.2__. Vous devez vous assurer que votre application s’exécute sur la version 4.6.2 de .NET. Vous ne pouvez pas solliciter ou redistribuer les versions antérieures à la version 4.6.2. Il s’agit de la version de .NET fournie dans la mise à jour anniversaire de Windows10. En vérifiant que votre application fonctionne sur cette version, vous garantissez que votre application être compatible avec les futures mises à jour de Windows 10.  Si votre application cible .NET Framework 4.0 ou version ultérieur, normalement à s’exécuter sur la version 4.6.2 de .NET, mais vous devez quand même la tester.

+ __Toujours votre application s’exécute avec des privilèges de sécurité avec élévation de privilèges__. Votre application doit fonctionner en cours d’exécution en tant qu’utilisateur interactif. Les utilisateurs qui installent votre application à partir du Microsoft Store ne sont ne peut-être pas des administrateurs système nécessitant par conséquent, votre application s’exécute avec élévation de privilèges signifie qu’il ne s’exécutera pas correctement pour les utilisateurs standard. Les applications qui requièrent une élévation pour n’importe quelle partie de leurs fonctionnalités ne sont pas acceptées dans le Microsoft Store.

+ __Votre application nécessite un pilote en mode noyau ou un service Windows__. Le pont est approprié pour une application, mais il ne prend pas en charge un pilote en mode noyau ou un service Windows devant être exécutés sous un compte système. Au lieu d’un service Windows, utilisez une [tâche en arrière-plan](https://msdn.microsoft.com/windows/uwp/launch-resume/create-and-register-a-background-task).

+ __Les modules de votre application sont chargés in-process sur les processus ne figurant pas dans votre package d’application Windows__. Cela n’est pas autorisé, ce qui signifie que les extensions in-process, par exemple les [extensions d’environnement](https://msdn.microsoft.com/library/windows/desktop/dd758089.aspx), ne sont pas prises en charge. Mais si vous disposez de deux applications dans le même package, vous pouvez créer une communication entre processus entre elles.

+ __Votre application utilise un personnalisé Application utilisateur ID (modèle)__. Si votre processus appelle [SetCurrentProcessExplicitAppUserModelID](https://msdn.microsoft.com/library/windows/desktop/dd378422.aspx) pour définir son propre AUMID, il peut uniquement utiliser l’AUMID généré pour lui par le package d’application application modèle/de l’environnement Windows. Vous ne pouvez pas définir d’AUMID personnalisés.

+ __Votre application modifie la ruche du Registre HKEY_LOCAL_MACHINE (HKLM)__. Toute tentative par votre application pour créer une clé HKLM ou d’ouvrir une pour modification se traduit par un échec d’accès refusé. N’oubliez pas que votre application possède sa propre vue privée virtualisée du Registre, afin que la notion d’un utilisateur - et machine-ruche du Registre (c'est-à-dire que la définition de HKLM) ne s’applique pas. Vous devez trouver un autre moyen d’obtenir ce pour quoi vous utilisiez HKLM, comme l’écriture sur HKEY_CURRENT_USER (HKCU) à la place.

+ __Votre application utilise une sous-clé de Registre ddeexec comme moyen de lancement d’une autre application__. Utilisez plutôt l’un des gestionnaires de verbe DelegateExecute tel que configuré par les différentes extensions activables* dans votre [manifeste du package de l’application](https://msdn.microsoft.com/library/windows/apps/br211474.aspx).

+ __Votre application écrit dans le dossier AppData ou dans le Registre dans le but de partager des données avec une autre application__. Après la conversion, AppData est redirigé vers le magasin de données de l’application locale, qui est un magasin privé pour chaque application UWP.

  Toutes les entrées que votre application écrit dans la ruche du Registre HKEY_LOCAL_MACHINE sont redirigées vers un fichier binaire isolé et toutes les entrées que votre application écrit dans la ruche du Registre HKEY_CURRENT_USER sont placées dans un magasin privé emplacement par utilisateur, par application. Pour plus d’informations sur la redirection de fichiers et du Registre, consultez [Fonctionnement détaillé de Pont du bureau](desktop-to-uwp-behind-the-scenes.md).  

  Utilisez un autre moyen de partager des données entre processus. Pour plus d’informations, voir [Stocker et récupérer des paramètres et autres données d’application](https://msdn.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data).

+ __Votre application écrit dans le répertoire d’installation de votre application__. Par exemple, votre application écrit dans un fichier journal que vous avez placé dans le même répertoire que votre fichier exe. Cela n’est pas pris en charge; vous devez donc trouver un autre emplacement, comme le magasin de données de l’application locale.

+ __Installation de votre application nécessite une interaction utilisateur__. Votre programme d’installation d’application doit être en mesure de s’exécuter en mode silencieux, et il doit installer tous les éléments requis qui ne sont pas sur par défaut sur une nouvelle image de système d’exploitation.

+ __Votre application utilise le répertoire de travail actuel__. Lors de l’exécution, votre application de bureau empaquetée ne dispose pas du même répertoire de travail que celui spécifié précédemment dans votre bureau. Raccourci LNK. Vous devez modifier votre répertoire lors de l’exécution si le répertoire correct est important pour votre application fonctionner correctement.

+ __Votre application requiert UIAccess__. Si votre application spécifie `UIAccess=true` dans l’élément `requestedExecutionLevel` du manifeste de contrôle de compte d’utilisateur, la conversion vers UWP n’est pas prise en charge actuellement. Pour plus d’informations, consultez [Vue d’ensemble de la sécurité UI Automation](https://msdn.microsoft.com/library/ms742884.aspx).

+ __Votre application expose des objets COM__. Les processus et extensions peuvent, au sein du package, enregistrer et utiliser les serveurs COM et OLE, pour les modes in-process et out-of-process (OOP).  Creators Update ajoute la prise en charge des COM empaquetés qui offre la possibilité d’inscrire les serveurs OOP COM et OLE qui sont désormais visibles en dehors du package.  Voir [Prise en charge des documents COM et OLE pour le Pont du bureau](https://blogs.windows.com/buildingapps/2017/04/13/com-server-ole-document-support-desktop-bridge/#bjPyETFgtpZBGrS1.97).

   La prise en charge des COM empaquetés fonctionne avec les API COM existantes, mais elle ne fonctionne pas avec les extensions d’application qui s’appuient sur la lecture directe du registre, car l’emplacement des COM empaquetés est de type privé.

+ __Votre application expose des assemblys GAC pour une utilisation par d’autres processus__. Dans la version actuelle, votre application ne peut pas exposer des assemblys GAC pour une utilisation par des processus issus de fichiers exécutables externes à votre package d’application Windows. Les processus issus du package peuvent enregistrer et utiliser des assemblys GAC normalement, mais ceux-ci ne seront pas visibles en externe. Cela signifie que les scénarios d’interopération, comme OLE, ne fonctionneront pas si ces derniers sont appelés par des processus externes.

+ __Votre application lie des bibliothèques de runtime C (CRT) d’une manière non prise en charge__. La bibliothèque runtime C/C++ Microsoft fournit des routines de programmation pour le système d’exploitation MicrosoftWindows. Ces routines automatisent de nombreuses tâches de programmation courantes qui ne sont pas fournies par les langages C et C++. Si votre application utilise la bibliothèque runtime C/C++, vous devez vous assurer qu’il est lié prise en charge.

    VisualStudio2017 prend en charge à la fois la liaison statique et dynamique (pour que votre code puisse utiliser des fichiers DLL courants) et la liaison statique (pour lier la bibliothèque directement dans votre code) à la version actuelle du CRT. Si possible, nous vous recommandons de la liaison dynamique avec Visual Studio 2017 votre utilisation de l’application.

    La prise en charge des versions antérieures de VisualStudio peut varier. Pour plus d’informations, voir le tableau suivant:

    <table>
    <th>Version de VisualStudio</td><th>Liaison dynamique</th><th>Liaison statique</th></th>
    <tr><td>2005 (VC8)</td><td>Non pris en charge</td><td>Pris en charge</td>
    <tr><td>2008 (VC9)</td><td>Non pris en charge</td><td>Pris en charge</td>
    <tr><td>2010 (VC10)</td><td>Prise en charge</td><td>Pris en charge</td>
    <tr><td>2012 (VC11)</td><td>Pris en charge</td><td>Non pris en charge</td>
    <tr><td>2013 (VC12)</td><td>Pris en charge</td><td>Non prise en charge</td>
    <tr><td>2015 et 2017 (VC14)</td><td>Prise en charge</td><td>Pris en charge</td>
    </table>

    Remarque: dans tous les cas, vous devez créer une liaison vers la toute dernière version de CRT disponible publiquement.

+ __Votre application installe et charge les assemblys du dossier Windows côte-à-côte__. Par exemple, votre application utilise des bibliothèques runtime C VC8 ou VC9 et les lie dynamiquement à partir du dossier côte-à-côte de Windows, ce qui signifie que votre code utilise les fichiers DLL courants à partir d’un dossier partagé. Ceci n’est pas pris en charge. Vous devez les lier de manière statique en créant dans votre code un lien direct vers les fichiers de bibliothèque redistribuables.

+ __Votre application utilise une dépendance dans le dossier System32/SysWOW64__. Pour que ces DLL fonctionnent, vous devez les inclure dans la partie du système de fichiers virtuel de votre package d’application Windows. Cela garantit que l’application se comporte comme si les fichiers DLL ont été installés dans le **System32**/dossier**SysWOW64** . À la racine du package, créez un dossier appelé **VFS**. Dans ce dossier, créez les dossiers **SystemX64** et **SystemX86**. Ensuite, placez la version 32 bits de votre fichierDLL dans le dossier **SystemX86** et la version 64bits dans le dossier **SystemX64**.

+ __Votre application utilise un package d’infrastructure VCLibs__. Les bibliothèques VCLibs peuvent être installées directement à partir du Microsoft Store si elles sont définies comme une dépendance dans le package d’application Windows. Par exemple, si votre application utilise des packages VCLibs 11 apporter les modifications suivantes à votre manifeste de package d’application: sous le `<Dependencies>` nœud, ajoutez:  
`<PackageDependency Name="Microsoft.VCLibs.110.00.UWPDesktop" MinVersion="11.0.24217.0" Publisher="CN=Microsoft Corporation, O=Microsoft Corporation, L=Redmond, S=Washington, C=US" />`  
Pendant l’installation à partir du Microsoft Store, la version appropriée (x86 ou x64) de l’infrastructure VCLibsest installée avant l’application.  
Les dépendances ne seront pas installées si l’application est installée en chargement indépendant. Pour installer les dépendances manuellement sur votre ordinateur, vous devez télécharger et installer les packages d’infrastructure VCLibs approprié le pont du bureau. Pour plus d’informations sur ces scénarios, consultez l’article [Utilisation de Runtime Visual C++ dans un projet Centennial](https://blogs.msdn.microsoft.com/vcblog/2016/07/07/using-visual-c-runtime-in-centennial-project/).

  **Packages d’infrastructure**:

  * [Packages d'infrastructure VC 14.0 pour le pont du bureau](https://www.microsoft.com/download/details.aspx?id=53175)
  * [Packages d'infrastructure VC 12.0 pur le pont du bureau](https://www.microsoft.com/download/details.aspx?id=53176)
  * [Packages d'infrastructure VC 11.0 pur le pont du bureau](https://www.microsoft.com/download/details.aspx?id=53340)


+ __Votre application contient une liste de raccourcis personnalisée__. Il existe plusieurs problèmes et mises en garde à connaître concernant l’utilisation des listes de raccourcis.

    - __L’architecture de votre application ne correspond pas au système d’exploitation.__  Listes de raccourcis actuellement ne fonctionnent pas correctement si l’application et architectures de système d’exploitation ne correspondent pas (par exemple, une x86 application en cours d’exécution sur x64 Windows). À ce stade, il n’existe aucune solution de contournement autre que de recompiler votre application à l’architecture correspondante.

    - __Votre application crée des entrées de liste de raccourcis et appelle [ICustomDestinationList::SetAppID](https://msdn.microsoft.com/library/windows/desktop/dd378403(v=vs.85).aspx) ou [SetCurrentProcessExplicitAppUserModelID](https://msdn.microsoft.com/library/windows/desktop/dd378422(v=vs.85).aspx)__. Ne définissez pas votre AppID dans le code par programmation. Sinon, vos entrées de liste de raccourcis ne s’afficheront pas. Si votre application a besoin d’un Id personnalisé, faites-le en utilisant le fichier manifeste. Pour obtenir des instructions, reportez-vous à [créer manuellement un Package une application de bureau](desktop-to-uwp-manual-conversion.md) . L’AppID de votre application est définie dans la section *YOUR_PRAID_HERE*.

    - __Votre application ajoute un lien shell de liste de raccourcis qui fait référence à un fichier exécutable dans votre package__. Vous ne pouvez pas lancer des exécutables dans votre package directement à partir d’une liste de raccourcis (sauf pour le chemin d’accès absolu du propre fichier .exe d’une application). Au lieu de cela, inscrivez un alias d’exécution d’application (ce qui permet à votre application de bureau empaquetée à l’aide d’un mot clé comme si elle était sur le chemin d’accès) et définissez le chemin cible du lien sur l’alias à la place. Pour plus d’informations sur l’utilisation de l’extension appExecutionAlias, voir [intégrer votre application de bureau Windows 10](desktop-to-uwp-extensions.md). Notez que, pour mapper les ressources du lien dans la liste de raccourcis au fichier .exe d’origine, vous devez définir les ressources telles que l’icône avec [**SetIconLocation**](https://msdn.microsoft.com/library/windows/desktop/bb761047(v=vs.85).aspx) et définir le nom complet avec PKEY_Title, comme vous le feriez pour d’autres entrées personnalisées.

    - __Votre application ajoute des entrées de liste de raccourcis qui font référence aux ressources dans le package de l’application par des chemins absolus__. Le chemin d’installation d’une application peut changer lors de ses packages sont mis à jour, modification de l’emplacement des ressources (icônes, documents, exécutables, etc.). Si des entrées de liste de raccourcis référence à ces ressources par des chemins absolus, l’application doit actualiser régulièrement sa liste de raccourcis (par exemple, sur l’application de lancement) pour garantir la résolvent correcte des chemins d’accès. Vous pouvez aussi utiliser les API UWP [**Windows.UI.StartScreen.JumpList**](https://msdn.microsoft.com/library/windows/apps/windows.ui.startscreen.jumplist.aspx), qui vous permettent de faire référence à des ressources chaîne et image à l’aide du schéma d’URI ms-resource relatif au package (également avec prise en charge linguistique, DPI et du contraste élevé).

+ __Votre application démarre un utilitaire pour effectuer des tâches__. Évitez de démarrer des utilitaires de commande tels que PowerShell et Cmd.exe. En fait, si les utilisateurs installent votre application sur un système qui exécute Windows 10 S, votre application ne sont pas être en mesure de les démarrer. Cela peut bloquer votre application à partir de la soumission au Microsoft Store, car toutes les applications soumises au Microsoft Store doivent être compatibles avec Windows 10 S.

Le démarrage d’un utilitaire constitue souvent un moyen pratique de chercher des informations dans le système d’exploitation, le registre ou les fonctionnalités d’accès au système. Toutefois, vous pouvez tout aussi bien utiliser les API UWP pour accomplir ces types de tâches. Ces API sont plus performantes, car ils n’ont pas besoin d’un fichier exécutable séparé à s’exécuter, mais plus important encore, elles empêchent l’application de sortir du package. Conception de l’application reste cohérente avec l’isolation, approbation et de sécurité qui accompagnent une application que vous avez empaqueté, et votre application se comportera comme prévu sur les systèmes exécutant Windows 10 S.

+ __Votre application hôtes extensions, les plug-ins ou les extensions__.   Dans de nombreux cas, les extensions de type COM continueront probablement à fonctionner tant qu’elles n’auront pas été empaquetées et elles s’installeront en toute confiance. C’est parce que ces programmes d’installation peuvent utiliser leurs capacités de confiance totale pour modifier le Registre et placer les fichiers d’extension partout où votre application hôte s’attend à les trouver.

   Toutefois, si ces extensions sont empaquetées et puis installées en tant que package d’application Windows, ils ne fonctionneront pas, car chaque package (l’application hôte et l’extension) seront isolé les uns des autres. Pour en savoir plus sur la façon dont les applications sont isolées à partir du système, consultez [détaillé de pont du bureau](desktop-to-uwp-behind-the-scenes.md).

 Toutes les applications et les extensions installées par des utilisateurs sur un système exécutant Windows10 S doivent être installées en tant que packages d’application Windows. Par conséquent, si vous envisagez de créer un package vos extensions ou si vous prévoyez d’encourager vos collaborateurs à les empaqueter, déterminer comment vous pouvez faciliter la communication entre le package d’application hôte et tous les packages d’extension. L’utilisation d’un [service d’application](../launch-resume/app-services.md) peut être un bon moyen pour accomplir cette opération.

+ __Votre application génère du code__. Votre application peut générer du code qu’elle consomme en mémoire, mais évitez d’écrire le code généré sur le disque car le processus de Certification des applications Windows ne peut pas valider ce code avant la soumission d’application. En outre, les applications qui écrivent du code sur le disque ne fonctionnent pas correctement sur les systèmes exécutant Windows 10 S. Cela peut bloquer votre application à partir de la soumission au Microsoft Store, car toutes les applications soumises au Microsoft Store doivent être compatibles avec Windows 10 S.

>[!IMPORTANT]
> Une fois que vous avez créé votre package d’application Windows, veuillez tester votre application pour vous assurer qu’elle fonctionne correctement sur les systèmes qui exécutent Windows 10 S. Toutes les applications soumises au Microsoft Store doivent être compatibles avec Windows 10 S. applications qui ne sont pas compatibles seront pas acceptées dans le Windows store. Consultez [Tester votre application pour Windows10 S](desktop-to-uwp-test-windows-s.md).

## <a name="next-steps"></a>Étapes suivantes

**Trouvez des réponses à vos questions**

Des questions? Contactez-nous sur Stack Overflow. Notre équipe contrôle ces [balises](http://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge). Vous pouvez également nous poser vos questions [ici](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D).

**Transmettre des commentaires ou suggérer des fonctionnalités**

Consultez [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial).

**Créer un package d’application Windows pour votre application de bureau**

Consultez [Créer un package d’application Windows](desktop-to-uwp-root.md#convert).
