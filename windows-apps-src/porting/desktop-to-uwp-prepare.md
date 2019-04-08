---
Description: Cet article répertorie les éléments que vous devez savoir avant d’empaqueter votre application de bureau. Le travail à fournir pour préparer votre application au processus de création de package peut être minime.
Search.Product: eADQiWindows 10XVcnh
title: Préparer empaqueter une application de bureau (Desktop Bridge)
ms.date: 05/18/20188
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 71a57ca2-ca00-471d-8ad9-52f285f3022e
ms.localizationpriority: medium
ms.openlocfilehash: 49238ba1b72cf3daf46fb076432a478e9f381bc6
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57600894"
---
# <a name="prepare-to-package-a-desktop-application"></a>Préparer l’empaquetage d’une application de bureau

Cet article rappelle les points à connaître avant de créer un package d'application de bureau. Vous ne disposez pas grand-chose à faire pour préparer votre application pour le processus d’empaquetage, mais si les éléments ci-dessous s’applique à votre application, vous devez résoudre avant la mise en package. N’oubliez pas que le Microsoft Store se charge pour vous de la prise en charge du Gestionnaire de licences et des mises à jour automatiques, par conséquent, vous pouvez supprimer les fonctionnalités relatives à ces tâches de votre code de base.

>[!IMPORTANT]
>La possibilité de créer un package d’application Windows pour votre application de bureau (également appelé le pont du bureau) a été introduite dans Windows 10, version 1607, et il peut être utilisé uniquement dans les projets qui ciblent la mise à jour anniversaire Windows 10 (10.0 ; Build 14393) ou une version ultérieure dans Visual Studio.

+ __Votre application nécessite une version de .NET antérieure à 4.6.2__. Vous devez vous assurer que votre application s’exécute sur .NET 4.6.2. Vous ne pouvez pas solliciter ou redistribuer les versions antérieures à la version 4.6.2. Il s’agit de la version de .NET fournie dans la mise à jour anniversaire de Windows 10. Vérification de que votre application fonctionne sur cette version garantit que votre application continuera à être compatible avec les futures mises à jour de Windows 10.  Si votre application cible le .NET Framework 4.0 ou version ultérieur, il est supposé s’exécuter sur .NET 4.6.2, mais vous devez toujours la tester.

+ __Toujours votre application s’exécute avec des privilèges de sécurité avec élévation de privilèges__. Votre application doit fonctionner en tant qu’utilisateur interactif. Les utilisateurs qui installent votre application à partir du Microsoft Store ne peuvent pas être des administrateurs système, ce qui votre application s’exécute avec élévation de privilèges signifie qu’il ne s’exécute pas correctement pour les utilisateurs standard. Les applications qui requièrent une élévation pour n’importe quelle partie de leurs fonctionnalités ne sont pas acceptées dans le Microsoft Store.

+ __Votre application nécessite un pilote en mode noyau ou un service Windows__. Le pont est approprié pour une application, mais il ne prend pas en charge un pilote en mode noyau ou un service Windows devant être exécutés sous un compte système. Au lieu d’un service Windows, utilisez une [tâche en arrière-plan](https://msdn.microsoft.com/windows/uwp/launch-resume/create-and-register-a-background-task).

+ __Les modules de votre application sont chargés in-process sur les processus ne figurant pas dans votre package d’application Windows__. Cela n’est pas autorisé, ce qui signifie que les extensions in-process, par exemple les [extensions d’environnement](https://msdn.microsoft.com/library/windows/desktop/dd758089.aspx), ne sont pas prises en charge. Mais si vous disposez de deux applications dans le même package, vous pouvez créer une communication entre processus entre elles.

+ __Votre application utilise un ID identifiant (Application utilisateur modèle AUMID) personnalisé__. Si votre processus appelle [SetCurrentProcessExplicitAppUserModelID](https://msdn.microsoft.com/library/windows/desktop/dd378422.aspx) pour définir son propre identifiant AUMID, puis il peut uniquement utiliser l’identifiant AUMID généré pour lui par le package d’application application model/Windows de l’environnement. Vous ne pouvez pas définir d’AUMID personnalisés.

+ __Votre application modifie la ruche de Registre HKEY_LOCAL_MACHINE (HKLM)__. Toute tentative par votre application pour créer une clé HKLM, ou ouvrez-en une pour la modification, entraîne un échec d’accès refusé. N’oubliez pas que votre application a sa propre vue virtuelle privée du Registre, donc la notion d’une ruche de Registre de l’utilisateur et ordinateur (c'est-à-dire HKLM What ' s) ne s’applique pas. Vous devez trouver un autre moyen d’obtenir ce pour quoi vous utilisiez HKLM, comme l’écriture sur HKEY_CURRENT_USER (HKCU) à la place.

+ __Votre application utilise une sous-clé de Registre ddeexec comme un moyen d’une autre application de lancement__. Utilisez plutôt l’un des gestionnaires de verbe DelegateExecute tel que configuré par les différentes extensions activables* dans votre [manifeste du package de l’application](https://msdn.microsoft.com/library/windows/apps/br211474.aspx).

+ __Votre application écrit dans le dossier AppData ou dans le Registre avec l’intention de partager des données avec une autre application__. Après la conversion, AppData est redirigé vers le magasin de données de l’application locale, qui est un magasin privé pour chaque application UWP.

  Toutes les entrées de votre application écrit dans la ruche HKEY_LOCAL_MACHINE du Registre sont redirigées vers un fichier binaire isolé et toutes les entrées que votre application écrit dans la ruche du Registre HKEY_CURRENT_USER sont placées dans un privée par utilisateur, emplacement de par l’application. Pour plus d’informations sur la redirection de fichiers et du Registre, consultez [Fonctionnement détaillé de Pont du bureau](desktop-to-uwp-behind-the-scenes.md).  

  Utilisez un autre moyen de partager des données entre processus. Pour plus d’informations, consultez [Stocker et récupérer des paramètres et autres données d’application](https://msdn.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data).

+ __Votre application écrit dans le répertoire d’installation pour votre application__. Par exemple, votre application écrit dans un fichier journal que vous placez dans le même répertoire que votre fichier exe. Cela n’est pas pris en charge ; vous devez donc trouver un autre emplacement, comme le magasin de données de l’application locale.

+ __Installation de votre application nécessite une interaction utilisateur__. Votre programme d’installation de l’application doit être en mesure d’exécuter en mode silencieux, et il doit installer tous ses composants requis qui ne sont pas sur par défaut sur une image de système d’exploitation propre.

+ __Votre application utilise le répertoire de travail actuel__. Lors de l’exécution, votre application de bureau empaquetée n’obtiendrez pas le même répertoire de travail que vous avez spécifié précédemment dans votre bureau. Raccourci LNK. Vous devez modifier votre répertoire de travail actuel lors de l’exécution si le répertoire approprié est important pour votre application de fonctionner correctement.

+ __Votre application nécessite UIAccess__. Si votre application spécifie `UIAccess=true` dans l’élément `requestedExecutionLevel` du manifeste de contrôle de compte d’utilisateur, la conversion vers UWP n’est pas prise en charge actuellement. Pour plus d’informations, consultez [Vue d’ensemble de la sécurité UI Automation](https://msdn.microsoft.com/library/ms742884.aspx).

+ __Votre application expose les objets COM__. Les processus et extensions peuvent, au sein du package, enregistrer et utiliser les serveurs COM et OLE, pour les modes in-process et out-of-process (OOP).  Creators Update ajoute la prise en charge des COM empaquetés qui offre la possibilité d’inscrire les serveurs OOP COM et OLE qui sont désormais visibles en dehors du package.  Voir [Prise en charge des documents COM et OLE pour le Pont du bureau](https://blogs.windows.com/buildingapps/2017/04/13/com-server-ole-document-support-desktop-bridge/#bjPyETFgtpZBGrS1.97).

   La prise en charge des COM empaquetés fonctionne avec les API COM existantes, mais elle ne fonctionne pas avec les extensions d’application qui s’appuient sur la lecture directe du registre, car l’emplacement des COM empaquetés est de type privé.

+ __Votre application expose des assemblys GAC pour une utilisation par d’autres processus__. Dans la version actuelle, votre application ne peut pas exposer des assemblys GAC pour une utilisation par les processus d’origine à partir de fichiers exécutables externes à votre package d’application Windows. Les processus issus du package peuvent enregistrer et utiliser des assemblys GAC normalement, mais ceux-ci ne seront pas visibles en externe. Cela signifie que les scénarios d’interopération, comme OLE, ne fonctionneront pas si ces derniers sont appelés par des processus externes.

+ __Votre application est lier des bibliothèques C runtime (CRT) de manière non pris en charge__. La bibliothèque runtime C/C++ Microsoft fournit des routines de programmation pour le système d’exploitation Microsoft Windows. Ces routines automatisent de nombreuses tâches de programmation courantes qui ne sont pas fournies par les langages C et C++. Si votre application utilise la bibliothèque runtime C/C++, vous devez vous assurer qu’il est lié de manière pris en charge.

    Visual Studio 2017 prend en charge à la fois la liaison statique et dynamique (pour que votre code puisse utiliser des fichiers DLL courants) et la liaison statique (pour lier la bibliothèque directement dans votre code) à la version actuelle du CRT. Si possible, nous vous recommandons de votre utilisation de l’application dynamique une liaison avec Visual Studio 2017.

    La prise en charge des versions antérieures de Visual Studio peut varier. Pour plus d’informations, voir le tableau suivant :

    <table>
    <th>Version de Visual Studio</td><th>Liaison dynamique</th><th>Liaison statique</th></th>
    <tr><td>2005 (VC 8)</td><td>Non pris en charge</td><td>Prise en charge</td>
    <tr><td>2008 (VC 9)</td><td>Non pris en charge</td><td>Prise en charge</td>
    <tr><td>2010 (VC 10)</td><td>Prise en charge</td><td>Prise en charge</td>
    <tr><td>2012 (VC 11)</td><td>Prise en charge</td><td>Non pris en charge</td>
    <tr><td>2013 (VC 12)</td><td>Prise en charge</td><td>Non pris en charge</td>
    <tr><td>2015 et 2017 (VC 14)</td><td>Prise en charge</td><td>Prise en charge</td>
    </table>

    Remarque: Dans tous les cas, vous devez lier à la dernière version disponible publiquement CRT.

+ __Votre application installe et charge les assemblys à partir du dossier côte à côte de Windows__. Par exemple, votre application utilise les bibliothèques C runtime VC8 ou VC9 et est les liaison dynamique à partir du dossier côte à côte de Windows, ce qui signifie que votre code à l’aide des fichiers DLL communs à partir d’un dossier partagé. Ceci n’est pas pris en charge. Vous devez les lier de manière statique en créant dans votre code un lien direct vers les fichiers de bibliothèque redistribuables.

+ __Votre application utilise une dépendance dans le dossier System32/SysWOW64__. Pour que ces DLL fonctionnent, vous devez les inclure dans la partie du système de fichiers virtuel de votre package d’application Windows. Cela garantit que l’application se comporte comme si les DLL ont été installées dans le **System32**/**SysWOW64** dossier. À la racine du package, créez un dossier appelé **VFS**. Dans ce dossier, créez les dossiers **SystemX64** et **SystemX86**. Ensuite, placez la version 32 bits de votre fichier DLL dans le dossier **SystemX86** et la version 64 bits dans le dossier **SystemX64**.

+ __Votre application utilise un package d’infrastructure VCLibs__. Les bibliothèques VCLibs peuvent être installées directement à partir du Microsoft Store si elles sont définies comme une dépendance dans le package d’application Windows. Par exemple, si votre application utilise les packages Dev11 VCLibs, apportez la modification suivante à votre manifeste de package d’application : Sous le `<Dependencies>` nœud, ajoutez :  
`<PackageDependency Name="Microsoft.VCLibs.110.00.UWPDesktop" MinVersion="11.0.24217.0" Publisher="CN=Microsoft Corporation, O=Microsoft Corporation, L=Redmond, S=Washington, C=US" />`  
Pendant l’installation à partir du Microsoft Store, la version appropriée (x86 ou x64) de l’infrastructure VCLibs est installée avant l’application.  
Les dépendances ne seront pas installés si l’application est installée par le chargement de version test. Pour installer les dépendances manuellement sur votre ordinateur, vous devez télécharger et installer les packages d’infrastructure VCLibs approprié le pont du bureau. Pour plus d’informations sur ces scénarios, consultez l’article [Utilisation de Runtime Visual C++ dans un projet Centennial](https://blogs.msdn.microsoft.com/vcblog/2016/07/07/using-visual-c-runtime-in-centennial-project/).

  **Packages d’infrastructure** :

  * [Packages de framework VC 14.0 pour Desktop Bridge](https://www.microsoft.com/download/details.aspx?id=53175)
  * [Packages de framework VC 12.0 pour Desktop Bridge](https://www.microsoft.com/download/details.aspx?id=53176)
  * [Packages de framework VC 11.0 pour Desktop Bridge](https://www.microsoft.com/download/details.aspx?id=53340)


+ __Votre application contient une liste de raccourcis personnalisés__. Il existe plusieurs problèmes et mises en garde à connaître concernant l’utilisation des listes de raccourcis.

    - __Architecture de votre application ne correspond pas au système d’exploitation.__  Les listes de raccourcis actuellement ne fonctionnent pas correctement si l’application et les architectures de système d’exploitation ne correspondent pas (par exemple, un x86 application s’exécutant sur x64 Windows). À ce stade, il n’existe aucune solution autre que de recompiler votre application à l’architecture correspondante.

    - __Votre application crée des entrées de liste de raccourcis et les appels [ICustomDestinationList::SetAppID](https://msdn.microsoft.com/library/windows/desktop/dd378403(v=vs.85).aspx) ou [SetCurrentProcessExplicitAppUserModelID](https://msdn.microsoft.com/library/windows/desktop/dd378422(v=vs.85).aspx)__. Ne définissez pas votre AppID dans le code par programmation. Sinon, vos entrées de liste de raccourcis ne s’afficheront pas. Si votre application a besoin d’un Id personnalisé, spécifiez-le en utilisant le fichier manifeste. Reportez-vous à [empaqueter une application de bureau manuellement](desktop-to-uwp-manual-conversion.md) pour obtenir des instructions. L’AppID de votre application est définie dans la section *YOUR_PRAID_HERE*.

    - __Votre application ajoute un lien de shell de liste de raccourcis qui fait référence à un fichier exécutable dans votre package__. Vous ne pouvez pas lancer des exécutables dans votre package directement à partir d’une liste de raccourcis (sauf pour le chemin d’accès absolu du propre fichier .exe d’une application). Au lieu de cela, inscrire un alias de l’exécution d’application (ce qui permet à votre application de bureau empaquetée démarrer via un mot clé comme s’il était sur le chemin d’accès) et définissez le chemin d’accès cible de lien à l’alias à la place. Pour plus d’informations sur la façon d’utiliser l’extension appExecutionAlias, consultez [intégrer votre application de bureau Windows 10](desktop-to-uwp-extensions.md). Notez que, pour mapper les ressources du lien dans la liste de raccourcis au fichier .exe d’origine, vous devez définir les ressources telles que l’icône avec [**SetIconLocation**](https://msdn.microsoft.com/library/windows/desktop/bb761047(v=vs.85).aspx) et définir le nom complet avec PKEY_Title, comme vous le feriez pour d’autres entrées personnalisées.

    - __Votre application ajoute des entrées de la liste un saut qui fait référence à des ressources dans le package de l’application par les chemins d’accès absolus__. Le chemin d’installation d’une application peut changer lorsque ses packages sont mis à jour, modification de l’emplacement des ressources (telles que des icônes, documents, fichier exécutable et ainsi de suite). Si les entrées de liste de raccourcis référencent ces actifs en chemins d’accès absolus, l’application doit actualiser périodiquement sa liste de liens (par exemple, sur l’application de lancement) afin de résolvent correctement les chemins d’accès. Vous pouvez aussi utiliser les API UWP [**Windows.UI.StartScreen.JumpList**](https://msdn.microsoft.com/library/windows/apps/windows.ui.startscreen.jumplist.aspx), qui vous permettent de faire référence à des ressources chaîne et image à l’aide du schéma d’URI ms-resource relatif au package (également avec prise en charge linguistique, DPI et du contraste élevé).

+ __Votre application démarre un utilitaire pour effectuer des tâches__. Évitez de démarrer des utilitaires de commande tels que PowerShell et Cmd.exe. En fait, si les utilisateurs installent votre application sur un système qui exécute le système Windows 10 S, votre application ne sont pas être en mesure de les démarrer du tout. Cela peut empêcher votre application d’envoi pour le Microsoft Store, car toutes les applications soumises pour le Microsoft Store doivent être compatibles avec Windows 10 S.

Le démarrage d’un utilitaire constitue souvent un moyen pratique de chercher des informations dans le système d’exploitation, le registre ou les fonctionnalités d’accès au système. Toutefois, vous pouvez tout aussi bien utiliser les API UWP pour accomplir ces types de tâches. Ces API ne sont plus performantes, car ils n’ont pas besoin d’un exécutable séparé à exécuter, mais plus important encore, qu’ils conservent l’application d’atteindre en dehors du package. Conception de l’application reste cohérente avec l’isolation, approbation et de sécurité qui est fourni avec une application que vous avez empaquetés et votre application se comportera comme prévu sur les systèmes exécutant Windows 10 S.

+ __Votre application héberge des compléments, des plug-ins ou des extensions__.   Dans de nombreux cas, les extensions de type COM continueront probablement à fonctionner tant qu’elles n’auront pas été empaquetées et elles s’installeront en toute confiance. C’est parce que ces programmes d’installation peuvent utiliser leurs fonctionnalités de niveau de confiance totale pour modifier le Registre et placer les fichiers d’extension partout où votre application hôte s’attend à les trouver.

   Toutefois, si ces extensions sont empaquetées et puis installées comme un package d’application Windows, ils ne fonctionneront pas car chaque package (l’application hôte et l’extension) seront isolé les uns des autres. Pour en savoir plus sur comment les applications sont isolées du système, consultez [dans les coulisses de Desktop Bridge](desktop-to-uwp-behind-the-scenes.md).

 Toutes les applications et les extensions installées par des utilisateurs sur un système exécutant Windows 10 S doivent être installées en tant que packages d’application Windows. Si vous avez l’intention de vos extensions de package, ou que vous envisagez d’encourager vos collaborateurs de les empaqueter, vous devez donc comment vous pouvez faciliter la communication entre le package d’application hôte et pour les packages d’extension. L’utilisation d’un [service d’application](../launch-resume/app-services.md) peut être un bon moyen pour accomplir cette opération.

+ __Votre application génère du code__. Votre application, vous pouvez générer le code qu’il consomme en mémoire, mais évitez le code d’écriture générée sur le disque, car le processus de Certification des applications Windows ne peut pas valider que le code avant la soumission de l’application. En outre, les applications qui écrivent du code sur le disque ne s’exécute correctement sur les systèmes exécutant Windows 10 S. Cela peut empêcher votre application d’envoi pour le Microsoft Store, car toutes les applications soumises pour le Microsoft Store doivent être compatibles avec Windows 10 S.

>[!IMPORTANT]
> Une fois que vous avez créé votre package d’application Windows, testez votre application pour vous assurer qu’elle fonctionne correctement sur les systèmes qui exécutent Windows 10 S. Toutes les applications soumises pour le Microsoft Store doivent être compatibles avec les applications Windows 10 s qui ne sont pas compatibles n’est pas acceptées dans le magasin. Consultez [Tester votre application pour Windows 10 S](desktop-to-uwp-test-windows-s.md).

## <a name="next-steps"></a>Étapes suivantes

**Trouvez des réponses à vos questions**

Des questions ? Contactez-nous sur Stack Overflow. Notre équipe contrôle ces [balises](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge). Vous pouvez également nous poser vos questions [ici](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D).

**Donner votre avis ou faire des suggestions de fonctionnalité**

Consultez [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial).

**Créer un package d’application Windows pour votre application de bureau**

Voir [Créer un package d’application Windows](desktop-to-uwp-root.md#convert).
