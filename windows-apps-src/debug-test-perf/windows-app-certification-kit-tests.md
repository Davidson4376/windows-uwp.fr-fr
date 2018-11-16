---
author: PatrickFarley
ms.assetid: 1526FF4B-9E68-458A-B002-0A5F3A9A81FD
title: Tests du Kit de certification des applications Windows
description: Le Kit de Certification d’application Windows contient un certain nombre de tests qui peuvent aider à garantir que votre application est prête à être publiée sur le Microsoft Store.
ms.author: pafarley
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp, certification des applications
ms.localizationpriority: medium
ms.openlocfilehash: 65afbaa4440a5bce43ca6d48126e6cc2b8316466
ms.sourcegitcommit: e38b334edb82bf2b1474ba686990f4299b8f59c7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/15/2018
ms.locfileid: "6853704"
---
# <a name="windows-app-certification-kit-tests"></a>Tests du Kit de certification des applications Windows


Le [Kit de Certification des applications Windows](windows-app-certification-kit.md) contient un certain nombre de tests qui vous assurer que votre application est prête à être publiée dans le Microsoft Store. Les tests sont répertoriées ci-dessous avec leurs critères, plus d’informations et actions dans le cas d’échec suggérées.

## <a name="deployment-and-launch-tests"></a>Tests de déploiement et de lancement

Surveille l’application au cours des tests de certification afin d’enregistrer quand elle cesse de répondre ou se bloque.

### <a name="background"></a>Contexte

Les applications qui cessent de répondre ou qui se bloquent peuvent conduire à la perte de données ou une expérience médiocre du point de vue de l’utilisateur.

Nous attendons des applications qu’elles soient totalement fonctionnelles sans recourir aux modes de compatibilité Windows, aux messages AppHelp ou à d’autres correctifs de compatibilité.

Les applications ne doivent pas énumérer des DLL à télécharger dans la clé de Registre HKEY\-LOCAL\-MACHINE\\Software\\Microsoft\\Windows NT\\CurrentVersion\\Windows\\AppInit\-DLLs.

### <a name="test-details"></a>Détails du test

Nous testons la résilience et la stabilité de l’application tout au long des tests de certification.

Le Kit de certification des applications Windows appelle la méthode [**IApplicationActivationManager::ActivateApplication**](https://msdn.microsoft.com/library/windows/desktop/Hh706903) pour lancer les applications. Pour que la méthode **ActivateApplication** lance une application, il faut que le contrôle de compte d’utilisateur (UAC) soit activé et que la résolution de l’écran soit d’au moins 1024×768 ou 768×1024. Si l’une de ces conditions n’est pas respectée, votre application échouera à ce test.

### <a name="corrective-actions"></a>Actions correctives

Assurez-vous que le contrôle UAC est activé sur l’ordinateur de test.

Veillez à exécuter le test sur un ordinateur dont l’écran est suffisamment grand.

Si le lancement de votre application échoue et que votre plateforme de test satisfait aux exigences liées à la méthode [**ActivateApplication**](https://msdn.microsoft.com/library/windows/desktop/Hh706903), vous pouvez résoudre le problème en examinant le journal des événements d’activation. Pour rechercher ces entrées dans le journal des événements:

1.  Ouvrez eventvwr.exe et accédez au dossier Journaux des applications et des services\\Microsoft\\Windows\\Immersive-Shell.
2.  Filtrez la vue de manière à afficher les ID d’événement 5900 à 6000.
3.  Dans les entrées du journal, recherchez les informations susceptibles d’expliquer l’échec du lancement de l’application.

Identifiez le fichier posant problème et corrigez-le. Générez et testez de nouveau l’application. Vous pouvez également vérifier si un fichier de vidage a été généré dans le dossier du journal du Kit de certification des applications Windows qui peut être utilisé pour déboguer votre application.

## <a name="platform-version-launch-test"></a>Test de lancement de la version de plateforme

Vérifie que l’application Windows peut s’exécuter sur une version ultérieure du système d’exploitation. À l’origine, ce test s’appliquait uniquement au workflow des applications de bureau, mais il est désormais activé pour les workflows des applications du Windows Store et de la plateforme Windows universelle (UWP).

### <a name="background"></a>Arrière-plan

Informations de version de système d’exploitation a restreint l’utilisation pour le Microsoft Store. Elle a souvent été incorrectement utilisée par les applications pour vérifier la version du système d’exploitation afin de pouvoir fournir aux utilisateurs des fonctionnalités propres à une version de système d’exploitation.

### <a name="test-details"></a>Détails du test

Le Kit de certification des applications Windows utilise le test HighVersionLie pour détecter le mode de vérification de la version du système d’exploitation utilisé par l’application. Si l’application se bloque, elle échoue à ce test.

### <a name="corrective-action"></a>Action corrective

Les applications doivent utiliser les fonctions d’assistance de l’API Version pour effectuer cette vérification. Pour plus d’informations, voir [Version du système d’exploitation](https://msdn.microsoft.com/library/windows/desktop/ms724832).

## <a name="background-tasks-cancellation-handler-validation"></a>Validation du gestionnaire d’annulation de tâches en arrière-plan

Ce test permet de vérifier que l’application dispose d’un gestionnaire d’annulation pour les tâches en arrière-plan déclarées. Il doit exister une fonction dédiée qui sera appelée lorsque la tâche est annulée. Ce test s’applique uniquement aux applications déployées.

### <a name="background"></a>Contexte

Les applications du Windows Store peuvent inscrire un processus qui s’exécute en arrière-plan. Par exemple, une application de messagerie peut de temps à autre effectuer un test ping sur un serveur. Toutefois, si le système d’exploitation a besoin de ces ressources, il annule la tâche en arrière-plan, et les applications doivent gérer correctement cette annulation. Les applications qui ne disposent pas d’un gestionnaire d’annulation peuvent se bloquer ou ne pas se fermer lorsque l’utilisateur essaie de les fermer.

### <a name="test-details"></a>Détails du test

L’application est lancée, suspendue et les tâches de l’application qui ne s’exécutent pas en arrière-plan sont arrêtées. Ensuite, les tâches en arrière-plan associées à cette application sont annulées. L’état de l’application est vérifié, et si l’application est toujours en cours d’exécution, elle échoue à ce test.

### <a name="corrective-action"></a>Action corrective

Ajoutez le gestionnaire d’annulation à votre application. Pour plus d’informations, voir [Définir des tâches en arrière-plan pour les besoins de votre application](https://msdn.microsoft.com/library/windows/apps/Mt299103).

## <a name="app-count"></a>Nombre d’applications

Ce test permet de vérifier qu’un package d’application (APPX, ensemble d’applications) contient une seule application. Il a été modifié dans le kit afin d’en faire un test autonome.

### <a name="background"></a>Contexte

Ce test a été implémenté conformément à la politique du Windows Store.

### <a name="test-details"></a>Détails du test

Pour les applications Windows Phone 8.1, le test vérifie que le nombre total de packages appx de l’ensemble est inférieur à (&lt; ) 512, qu’il n’y a qu’un seul package principal dans l’ensemble et que l’architecture du package principal de l’ensemble est marquée comme ARM ou neutre.

Pour les applications Windows10, le test vérifie que le numéro de révision de la version de l’ensemble est défini sur0.

### <a name="corrective-action"></a>Action corrective

Assurez-vous que le package et que l’ensemble d’applications satisfont aux exigences décrites dans les détails du test ci-dessus.

## <a name="app-manifest-compliance-test"></a>Test de conformité du manifeste d’application

Teste le contenu du manifeste d’application pour vérifier qu’il est correct.

### <a name="background"></a>Contexte

Les applications doivent avoir un manifeste d’application correctement mis en forme.

### <a name="test-details"></a>Détails du test

Examine le manifeste de l’application afin de vérifier que son contenu est correct, comme décrit dans [Exigences relatives aux packages d’applications](https://msdn.microsoft.com/library/windows/apps/Mt148525).

-   **Extensions de fichiers et protocoles**

    Votre application peut déclarer les extensions de fichier auxquelles elle veut s’associer. Une application peut utiliser de façon incorrecte cette fonction et déclarer un grand nombre d’extensions de fichier, alors qu’elle n’en utilisera peut-être même pas la majeure partie, ce qui aboutit à une expérience utilisateur médiocre. Ce test ajoute une vérification pour limiter le nombre d’extensions de fichier auxquelles une application peut s’associer.

-   **Règle de dépendance d’infrastructure**

    Ce test applique la spécification selon laquelle les applications établissent des dépendances appropriées envers la plateforme Windows universelle (UWP). En cas de dépendance inappropriée, ce test échoue.

    En cas d’incompatibilité entre la version du système d’exploitation à laquelle l’application s’applique et les dépendances d’infrastructure établies, le test échoue. Le test échoue également si l’application fait référence à des versions d’évaluation des DLL d’infrastructure.

-   **Vérification de la communication entre processus (IPC)**

    Ce test applique la spécification selon que les applications UWP ne communiquent pas en dehors du conteneur d’application dans les composants de bureau. La communication entre processus ne concerne que les applications chargées latéralement. Les applications qui spécifient l’attribut [**ActivatableClassAttribute**](https://msdn.microsoft.com/library/windows/apps/BR211414) avec «DesktopApplicationPath» comme nom échouent à ce test.

### <a name="corrective-action"></a>Action corrective

Confrontez le manifeste de l’application aux exigences décrites dans [Exigences relatives aux packages d’applications](https://msdn.microsoft.com/library/windows/apps/Mt148525).

## <a name="windows-security-features-test"></a>Test des fonctionnalités de sécurité Windows

### <a name="background"></a>Contexte

La modification des protections de sécurité Windows par défaut peut exposer les clients à des risques accrus.

### <a name="test-details"></a>Détails du test

Teste la sécurité de l’application en exécutant [BinScope Binary Analyzer](#binscope-binary-analyzer-tests).

Les tests BinScope Binary Analyzer examinent les fichiers binaires de l’application afin de vérifier qu’ils utilisent des pratiques de codage et de génération qui rendent l’application moins vulnérable à des attaques ou à leur utilisation comme vecteurs d’attaque.

Les tests BinScope Binary Analyzer vérifient que les fonctionnalités de sécurité suivantes sont correctement utilisées.

-   Tests BinScope Binary Analyzer
-   Signature de code privé

### <a name="binscope-binary-analyzer-tests"></a>Tests BinScope Binary Analyzer

Les tests [BinScope Binary Analyzer](https://www.microsoft.com/en-us/download/details.aspx?id=44995) examinent les fichiers binaires de l’application afin de vérifier qu’ils utilisent des pratiques de codage et de génération qui rendent l’application moins vulnérable à des attaques ou à leur utilisation comme vecteurs d’attaque.

Les tests BinScope Binary Analyzer vérifient que les fonctionnalités de sécurité suivantes sont correctement utilisées :

-   [AllowPartiallyTrustedCallersAttribute](#binscope-1)
-   [Protection de la gestion des exceptions /SafeSEH](#binscope-2)
-   [Prévention de l’exécution des données](#binscope-3)
-   [Randomisation du format d’espace d’adresse](#binscope-4)
-   [Section PE partagée en lecture/écriture](#binscope-5)
-   [AppContainerCheck](#appcontainercheck)
-   [ExecutableImportsCheck](#binscope-7)
-   [WXCheck](#binscope-8)

### <a name="span-idbinscope-1spanallowpartiallytrustedcallersattribute"></a><span id="binscope-1"></span>AllowPartiallyTrustedCallersAttribute

**Message d’erreur du Kit de certification des applications Windows:** Échec du test APTCACheck

L’attribut AllowPartiallyTrustedCallersAttribute (APTCA) autorise l’accès au code entièrement fiable à partir de code partiellement fiable dans des assemblys signés. Lorsque vous appliquez l’attribut APTCA à un assembly, les appelants partiellement fiables peuvent accéder à cet assembly pendant toute la durée de vie de l’assembly, ce qui peut compromettre la sécurité.

**Ce que vous devez faire si votre application échoue à ce test**

N’utilisez pas l’attribut APTCA sur les assemblys portant un nom fort, à moins que votre projet ne l’exige et que vous ayez conscience des risques encourus. Assurez-vous alors que toutes les API sont protégées avec des demandes de sécurité appropriées d’accès au code. L’attribut APTCA est sans effet lorsque l’assembly fait partie d’une application UWP (plateforme Windows universelle).

**Remarques**

Ce test est uniquement réalisé sur le code managé (C#, .NET, etc.).

### <a name="span-idbinscope-2spansafeseh-exception-handling-protection"></a><span id="binscope-2"></span>Protection de la gestion des exceptions /SafeSEH

**Message d’erreur du Kit de certification des applications Windows:** Échec du test SafeSEHCheck

Un gestionnaire d’exceptions est exécuté lorsque l’application rencontre une condition exceptionnelle, telle qu’une erreur de type « division par zéro ». L’adresse du gestionnaire d’exceptions étant stockée sur la pile lors de l’appel d’une fonction, elle peut faire l’objet d’une attaque par saturation de la mémoire tampon si un logiciel malveillant parvient à remplacer la pile.

**Ce que vous devez faire si votre application échoue à ce test**

Activez l’option /SAFESEH dans la commande de l’éditeur de liens lorsque vous générez votre application. Cette option est activée par défaut dans les configurations Release de Visual Studio. Vérifiez que cette option est activée dans les instructions de génération pour tous les modules exécutables dans votre application.

**Remarques**

Le test n’est pas effectué sur les binaires 64bits ni sur les binaires du circuit microprogrammé ARM, ceux-ci ne stockant pas les adresses du gestionnaire d’exceptions sur la pile.

### <a name="span-idbinscope-3spandata-execution-prevention"></a><span id="binscope-3"></span>Prévention de l’exécution des données

**Message d’erreur du Kit de certification des applications Windows:** Échec du test NXCheck

Ce test vérifie qu’une application n’exécute pas du code qui est stocké dans un segment de données.

**Ce que vous devez faire si votre application échoue à ce test**

Activez l’option /NXCOMPAT dans la commande de l’éditeur de liens lorsque vous générez votre application. Cette option est activée par défaut dans les versions de l’éditeur de liens qui prennent en charge la prévention de l’exécution des données (PED).

**Remarques**

Nous vous recommandons de tester vos applications sur une unité centrale compatible avec PED et de corriger toutes les erreurs résultant de cette fonction.

### <a name="span-idbinscope-4spanaddress-space-layout-randomization"></a><span id="binscope-4"></span>Randomisation du format d’espace d’adresse

**Message d’erreur du Kit de certification des applications Windows:** Échec du test DBCheck

La randomisation du format d’espace d’adresse (ASLR) charge des images exécutables à des endroits imprévisibles de la mémoire, ce qui complique la tâche des logiciels malveillants qui s’attendent à ce qu’un programme soit chargé à une adresse virtuelle particulière pour fonctionner de manière prévisible. Votre application et tous les composants qu’elle utilise doivent prendre en charge ASLR.

**Ce que vous devez faire si votre application échoue à ce test**

Activez l’option /DYNAMICBASE dans la commande de l’éditeur de liens lorsque vous générez votre application. Vérifiez que tous les modules utilisés par votre application utilisent également cette option de l’éditeur de liens.

**Remarques**

En règle générale, ASLR n’affecte pas les performances. Toutefois, dans certains scénarios, les systèmes 32 bits bénéficient d’une légère amélioration des performances. Une dégradation des performances peut se produire dans un système fortement encombré dans lequel de nombreuses images sont chargées dans différents emplacements de mémoire.

Ce test est réalisé uniquement sur les applications écrites dans des langages non managés, par exemple en utilisant C ou C++.

### <a name="span-idbinscope-5spanreadwrite-shared-pe-section"></a><span id="binscope-5"></span>Section PE partagée en lecture/écriture

**Message d’erreur du Kit de certification des applications Windows:** Échec du test SharedSectionsCheck.

Les fichiers binaires avec des sections accessibles en écriture qui sont marquées comme étant partagées constituent faille de sécurité. Ne générez pas d’applications avec des sections accessibles en écriture partagées sauf en cas d’absolue nécessité. Utilisez [**CreateFileMapping**](https://msdn.microsoft.com/library/windows/desktop/Aa366537) ou [**MapViewOfFile**](https://msdn.microsoft.com/library/windows/desktop/Aa366761) pour créer un objet mémoire partagée correctement sécurisé.

**Ce que vous devez faire si votre application échoue à ce test**

Supprimez toutes les sections partagées de l’application et créez des objets mémoire partagée en appelant [**CreateFileMapping**](https://msdn.microsoft.com/library/windows/desktop/Aa366537) ou [**MapViewOfFile**](https://msdn.microsoft.com/library/windows/desktop/Aa366761)avec les attributs de sécurité appropriés, puis regénérez votre application.

**Remarques**

Ce test est réalisé uniquement sur les applications écrites dans des langages non managés, par exemple en utilisant C ou C++.

### <a name="appcontainercheck"></a>AppContainerCheck

**Message d’erreur du Kit de certification des applications Windows:** Échec du test AppContainerCheck.

Le test AppContainerCheck vérifie que le bit **appcontainer** est défini dans l’en-tête de fichier exécutable portable (PE) d’un binaire exécutable. Le bit **appcontainer** doit être défini dans tous les fichiers .exe et les DLL non managées des applications pour que ces dernières s’exécutent correctement.

**Que faire si votre application échoue à ce test?**

Si un fichier exécutable natif échoue à ce test, vérifiez que vous avez utilisé le compilateur et l’éditeur de liens les plus récents pour générer le fichier et que vous utilisez l’indicateur */appcontainer* sur l’éditeur de liens.

Si un fichier exécutable managé échoue à ce test, assurez-vous que vous avez utilisé le compilateur dernière et l’éditeur de liens, tels que Microsoft Visual Studio, pour générer l’application UWP.

**Remarques**

Ce test est réalisé sur tous les fichiers.exe et DLL non managées.

### <a name="span-idbinscope-7spanexecutableimportscheck"></a><span id="binscope-7"></span>ExecutableImportsCheck

**Message d’erreur du Kit de certification des applications Windows:** Échec du test ExecutableImportsCheck.

Une image PE (Portable Executable) échoue à ce test si sa table d’importation a été placée dans une section de code exécutable. Cette situation peut se produire si vous avez activé la fusion .rdata pour l’image PE en définissant l’indicateur */merge* de l’éditeur de liens Visual C++ sur */merge:.rdata=.text*.

**Que faire si votre application échoue à ce test?**

Ne fusionnez pas la table d’importation dans une section de code exécutable. Assurez-vous que l’indicateur */merge* de l’éditeur de liens Visual C++ n’est pas défini pour fusionner la section «.rdata» dans une section de code.

**Remarques**

Ce test est réalisé sur l’ensemble du code binaire, à l’exception des assemblys purement managés.

### <a name="span-idbinscope-8spanwxcheck"></a><span id="binscope-8"></span>WXCheck

**Message d’erreur du Kit de certification des applications Windows:** Échec du test WXCheck.

Cette vérification permet de s’assurer qu’un binaire ne comporte pas de pages mappées en tant qu’éléments accessibles en écriture et exécutables. Cela peut se produire si le binaire a une section accessible en écriture et exécutable ou si la valeur de *SectionAlignment* du binaire est inférieure à *PAGE\-SIZE*.

**Que faire si votre application échoue à ce test?**

Assurez-vous que le binaire n’a pas de section accessible en écriture et exécutable, et que la valeur de *SectionAlignment* du binaire est au moins égale à *PAGE\-SIZE*.

**Remarques**

Ce test est effectué sur tous les fichiers .exe, ainsi que sur les DLL natives, non managées.

Un exécutable peut avoir une section accessible en écriture et exécutable s’il est généré quand Modifier &amp; Continuer est activé (/ZI). Si Modifier &amp; Continuer est désactivé, la section non valide n’est pas présente.

*PAGE\-SIZE* est la valeur de *SectionAlignment* par défaut pour les exécutables.

### <a name="private-code-signing"></a>Signature de code privé

Teste l’existence de fichiers binaires de signature de code privé dans le package de l’application.

### <a name="background"></a>Contexte

Les fichiers de signature de code privé doivent demeurer privés car ils peuvent être utilisés à des fins malveillantes s’ils sont compromis.

### <a name="test-details"></a>Détails du test

Teste si le package d’application contient des fichiers portant l’extension .pfx ou .snk qui indiquerait la présence de clés de signature privée.

### <a name="corrective-actions"></a>Actions correctives

Supprimez du package toute clé de signature de code privé (par exemple, les fichiers .pfx et .snk).

## <a name="supported-api-test"></a>Test des API prises en charge

Teste l’application afin de savoir si elle utilise des API non conformes.

### <a name="background"></a>Contexte

Les applications doivent utiliser les API pour les applications UWP (Windows Runtime ou des API Win32 prises en charge) afin d’être certifiées pour le Microsoft Store. Ce test identifie également les cas où un fichier binaire managé devient dépendant d’une fonction en dehors du profil approuvé.

### <a name="test-details"></a>Détails du test

-   Vérifie que chaque fichier binaire dans le package d’application n’est pas dépendant d’une API Win32 qui n’est pas pris en charge pour le développement d’applications UWP en vérifiant la table adresses d’importation du fichier binaire.
-   Vérifie que chaque fichier binaire managé dans le package d’application n’est pas dépendant d’une fonction en dehors du profil approuvé.

### <a name="corrective-actions"></a>Actions correctives

Vérifiez que l’application a été compilée en tant que version de publication et non en tant que version de débogage.

> **Remarque**la version de débogage d’une application échouera ce test même si l’application utilise uniquement des [API pour les applications UWP](https://msdn.microsoft.com/library/windows/apps/xaml/bg124285.aspx).

Passez en revue les messages d’erreur pour identifier l’API utilisée par l’application qui n’est pas une [API pour les applications UWP](https://msdn.microsoft.com/library/windows/apps/xaml/bg124285.aspx).

> **Remarque**les applications C++ générées dans une configuration de débogage échouent à ce test même si la configuration utilise uniquement des API du SDK Windows pour les applications UWP. [Solutions de rechange aux API Windows dans les applications UWP](http://go.microsoft.com/fwlink/p/?LinkID=244022) pour plus d’informations, voir.

## <a name="performance-tests"></a>Tests de performances

L’application doit répondre rapidement à l’interaction utilisateur et aux commandes système pour présenter à l’utilisateur une expérience rapide et fluide.

Les caractéristiques de l’ordinateur sur lequel le test est exécuté peuvent influencer les résultats du test. Les seuils du test de performances pour la certification d’une application sont définis de telle sorte que les ordinateurs à faible consommation d’énergie répondent aux attentes du client en termes de rapidité et de fluidité. Pour déterminer les performances de votre application, nous vous recommandons d’effectuer le test sur un ordinateur à faible consommation d’énergie, tel qu’un ordinateur équipé d’un processeur Intel Atom, d’une résolution d’écran de 1366x768 (ou plus) et d’un disque dur rotatif (par opposition à un disque SSD).

### <a name="bytecode-generation"></a>Génération de bytecode

Dans le cadre d’une optimisation des performances pour accélérer la durée d’exécution JavaScript, les fichiers JavaScript dont l’extension est .js génèrent du bytecode lors du déploiement de l’application. Cela améliore considérablement le temps de démarrage et d’exécution des opérations JavaScript.

### <a name="test-details"></a>Détails du test

Vérifie le déploiement de l’application pour s’assurer que tous les fichiers .js ont été convertis en bytecode.

### <a name="corrective-action"></a>Action corrective

Si ce test échoue, effectuez les actions suivantes pour résoudre le problème :

-   Vérifiez que la journalisation des événements est activée.
-   Vérifiez que tous les fichiers JavaScript sont valides du point de vue syntaxique.
-   Vérifiez que toutes les précédentes versions de l’application ont été désinstallées.
-   Excluez les fichiers identifiés du package d’application.

### <a name="optimized-binding-references"></a>Références de liaisons optimisées

Si vous utilisez des liaisons, WinJS.Binding.optimizeBindingReferences doit avoir la valeur True de manière à optimiser l’utilisation de la mémoire.

### <a name="test-details"></a>Détails du test

Vérifiez la valeur de WinJS.Binding.optimizeBindingReferences.

### <a name="corrective-action"></a>Action corrective

Affectez à WinJS.Binding.optimizeBindingReferences la valeur **true** dans le code JavaScript de l’application.

## <a name="app-manifest-resources-test"></a>Test des ressources du manifeste d’application

### <a name="app-resources-validation"></a>Validation des ressources de l’application

L’application peut ne pas s’installer si les chaînes ou les images déclarées dans le manifeste de votre application sont incorrectes. Si l’application s’installe avec des erreurs, le logo ou d’autres images utilisées par votre application peuvent ne pas s’installer correctement.

### <a name="test-details"></a>Détails du test

Inspecte les ressources définies dans le manifeste de l’application afin de vérifier qu’elles sont présentes et valides.

### <a name="corrective-action"></a>Action corrective

Inspirez-vous du tableau suivant.

<table>
<tr><th>Message d’erreur</th><th>Commentaires</th></tr>
<tr><td>
<p>L’image {image name} définit à la fois les qualificateurs Scale et TargetSize ; vous ne pouvez définir qu’un seul qualificateur à la fois.</p>
</td><td>
<p>Vous pouvez personnaliser les images pour différentes résolutions.</p>
<p>Dans le message réel, {image name} représente le nom de l’image affectée par l’erreur.</p>
<p> Assurez-vous que chaque image définit Scale ou TargetSize comme qualificateur.</p>
</td></tr>
<tr><td>
<p>L’image {image name} ne respecte pas les restrictions imposées pour la taille.</p>
</td><td>
<p>Assurez-vous que toutes les images de l’application adhèrent aux restrictions définissant la taille appropriée.</p>
<p>Dans le message réel, {image name} représente le nom de l’image affectée par l’erreur.</p>
</td></tr>
<tr><td>
<p>L’image {image name} ne se trouve pas dans le package.</p>
</td><td>
<p>Une image requise est manquante.</p>
<p>Dans le message réel, {image name} représente le nom de l’image manquante.</p>
</td></tr>
<tr><td>
<p>L’image {image name} n’est pas un fichier image valide.</p>
</td><td>
<p>Assurez-vous que toutes les images de l’application adhèrent aux restrictions définissant le type de format de fichier approprié.</p>
<p>Dans le message réel, {image name} représente le nom de l’image non valide.</p>
</td></tr>
<tr><td>
<p>L’image « BadgeLogo » a une valeur ABGR {value} à la position (x, y) qui n’est pas valide. Le pixel doit être blanc (##FFFFFF) ou transparent (00######).</p>
</td><td>
<p>Le logo du badge représente une image qui apparaît à côté de la notification de badge afin d’identifier l’application sur l’écran de verrouillage. L’image doit être monochrome (elle ne peut contenir que des pixels blancs ou transparents).</p>
<p>Dans le message réel, {value} représente la valeur de couleur qui n’est pas valide dans l’image.</p>
</td></tr>
<tr><td>
<p>L’image «BadgeLogo» a une valeur ABGR «{value}» non valide pour une image blanche à contraste élevé à la position (x, y). Les pixels doivent être (##2A2A2A) ou plus sombres, ou transparents (00######).</p>
</td><td>
<p>Le logo du badge représente une image qui apparaît à côté de la notification de badge afin d’identifier l’application sur l’écran de verrouillage.   Étant donné que le logo du badge apparaît sur un arrière-plan blanc lors de l’utilisation d’un motif blanc à contraste élevé, il doit être une version sombre du logo de badge normal. Lors de l’utilisation d’un motif blanc à contraste élevé, le logo du badge ne peut contenir que des pixels plus sombres que (##2A2A2A) ou transparents.</p>
<p>Dans le message réel, {value} représente la valeur de couleur qui n’est pas valide dans l’image.</p>
</td></tr>
<tr><td>
<p>L’image doit définir au moins un type Variant sans qualificateur TargetSize. Elle doit définir un qualificateur Scale ou laisser Scale et TargetSize non spécifiés, ce qui donne la valeur par défaut Scale-100.</p>
</td><td>
<p>Pour plus d’informations, voir <a href="https://msdn.microsoft.com/library/windows/apps/xaml/dn958435.aspx">Conception réactive 101 pour les applications UWP</a> et <a href="https://msdn.microsoft.com/library/windows/apps/xaml/hh465241.aspx">Recommandations en matière de ressources d’application</a>.</p>
</td></tr>
<tr><td>
<p>Un fichier «resources.pri» manque dans le package.</p>
</td><td>
<p>Si le manifeste de votre application comporte du contenu localisable, veillez à ce que le package de votre application contienne un fichier resources.pri valide.</p>
</td></tr>
<tr><td>
<p>Le fichier «resources.pri» doit contenir un mappage des ressources avec un nom qui correspond au nom du package «{package full name}».</p>
</td><td>
<p>Vous pouvez obtenir cette erreur si le manifeste a changé et que le nom du mappage de ressources dans resources.pri ne correspond plus au nom du package dans le manifeste.</p>
<p>Dans le message réel, {package full name} représente le nom du package que resources.pri doit contenir.</p>
<p>Pour résoudre ce problème, vous devez régénérer resources.pri; la façon la plus facile de le faire consiste à régénérer le package de l’application.</p>
</td></tr>
<tr><td>
<p>La fusion automatique ne doit pas être activée pour le fichier «resources.pri».</p>
</td><td>
<p>MakePRI.exe prend en charge une option appelée <strong>AutoMerge</strong>. La valeur par défaut de <strong>AutoMerge</strong> est <strong>off</strong>. Lorsque l’option <strong>AutoMerge</strong> est activée, elle fusionne les ressources du module linguistique d’une application en un fichier resources.pri unique au moment de l’exécution. Est déconseillé pour les applications que vous envisagez de distribuer par le biais du Microsoft Store. Le fichier resources.pri d’une application distribuée par le biais du Microsoft Store doivent être à la racine du package de l’application et contenir toutes les références de langage qui prend en charge de l’application.</p>
</td></tr>
<tr><td>
<p>La chaîne «{string}» ne respecte pas la limite maximale de {number}caractères.</p>
</td><td>
<p>Consultez les <a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt148525.aspx">Exigences relatives aux packages d’applications</a>.</p>
<p>Dans le message réel, {string} est remplacé par la chaîne affectée par l’erreur et {number} représente la longueur maximale.</p>
</td></tr>
<tr><td>
<p>La chaîne {string} ne doit pas comporter d’espace de début/fin.</p>
</td><td>
<p>Le schéma des éléments du manifeste de l’application n’autorise pas les espaces de début ou de fin.</p>
<p>Dans le message réel, {string} est remplacé par la chaîne affectée par l’erreur.</p>
<p>Assurez-vous qu’aucune des valeurs localisées des champs du manifeste dans resources.pri ne possède d’espaces de début ou de fin.</p>
</td></tr>
<tr><td>
<p>La chaîne ne doit pas être vide (sa longueur doit être supérieure à zéro).</p>
</td><td>
<p>Pour plus d’informations, voir <a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt148525.aspx">Exigences relatives aux packages d’applications</a>.</p>
</td></tr>
<tr><td>
<p>Il n’y a aucune ressource par défaut spécifiée dans le fichier «resources.pri».</p>
</td><td>
<p>Pour plus d’informations, voir <a href="https://msdn.microsoft.com/library/windows/apps/xaml/hh465241.aspx">Recommandations en matière de ressources de l’application</a>.</p>
<p>Dans la configuration de build par défaut, Visual Studio inclut uniquement les ressources d’image avec qualificateur «Scale-200» dans le package d’application lors de la génération des offres groupées, et place les autres ressources dans le package de ressources. Prenez soin d’inclure les ressources d’image avec qualificateur «Scale-200» ou de configurer votre projet pour qu’il intègre les ressources dont vous disposez.</p>
</td></tr>
<tr><td>
<p>Aucune valeur de ressource n’est spécifiée dans le fichier «resources.pri».</p>
</td><td>
<p>Assurez-vous que des ressources valides sont définies dans resources.pri pour le manifeste de l’application.</p>
</td></tr>
<tr><td>
<p>La taille du fichier image {filename} doit être inférieure à 204800octets.**</p>
</td><td>
<p>Réduisez la taille des images indiquées.</p>
</td></tr>
<tr><td>
<p>Le fichier «{filename}» ne doit pas contenir de section de correspondance inverse.**</p>
</td><td>
<p>Bien que la correspondance inverse soit générée pendant un débogage F5 Visual Studio lors d’un appel de makepri.exe, elle peut être supprimée en exécutant makepri.exe sans le paramètre /m lors de la génération d’un fichier .pri.</p>
</td></tr>
<tr><td colspan="2">
<p>\*\* Indique qu’un test a été ajouté au Kit de certification des applications Windows version3.3 pour Windows8.1 et qu’il n’est applicable que lors de l’utilisation de cette version du Kit ou d’une version ultérieure.</p>
</td></tr>
</table>



 

### <a name="branding-validation"></a>Validation de la personnalisation

Les applications UWP doivent être terminées et pleinement fonctionnelles. Les applications qui utilisent les images par défaut (provenant des exemples ou exemples SDK) offrent une expérience utilisateur médiocre et sont difficilement identifiables dans le catalogue du Windows Store.

### <a name="test-details"></a>Détails du test

Le test réussit si les images utilisées par l’application ne sont pas des images par défaut provenant des exemples du Kit de développement logiciel (SDK) ou de Visual Studio.

### <a name="corrective-actions"></a>Actions correctives

Remplacez les images par défaut par quelque chose de plus singulier et de plus représentatif de votre application.

## <a name="debug-configuration-test"></a>Test de configuration du débogage

Teste l’application afin de vérifier qu’il ne s’agit pas d’une version de débogage.

### <a name="background"></a>Contexte

Pour pouvoir être certifiées pour le Microsoft Store, les applications ne doivent pas être compilées pour le débogage et ne doivent pas référencer les versions de débogage d’un fichier exécutable. En outre, vous devez générer votre code de manière optimisée pour que votre application réussisse ce test.

### <a name="test-details"></a>Détails du test

Testez l’application de manière à vérifier qu’il ne s’agit pas d’une version de débogage et qu’elle n’est pas liée à des infrastructures de débogage.

### <a name="corrective-actions"></a>Actions correctives

-   Générez l’application comme une version commerciale avant de la soumettre au Microsoft Store.
-   Vérifiez que la version correcte du .NET Framework est installée.
-   Assurez-vous que l’application ne crée pas de liens vers des versions de débogage d’une infrastructure et qu’elle est créée avec une version commerciale. Si l’application contient des composants .NET, assurez-vous que vous avez installé la version correcte du .NET Framework.

## <a name="file-encoding-test"></a>Test d’encodage des fichiers

### <a name="utf-8-file-encoding"></a>Codage de fichier UTF-8

### <a name="background"></a>Contexte

Les fichiers HTML, CSS et JavaScript doivent être encodés au format UTF-8 avec une marque d’ordre d’octet (BOM) pour bénéficier de la mise en cache du bytecode et éviter certaines conditions d’erreur d’exécution.

### <a name="test-details"></a>Détails du test

Teste le contenu des packages d’application pour vérifier que l’encodage de fichiers correct est utilisé.

### <a name="corrective-action"></a>Action corrective

Ouvrez le fichier affecté et sélectionnez **Enregistrer sous** dans le menu **Fichier** de Visual Studio. Sélectionnez le contrôle de liste déroulante en regard du bouton **Enregistrer**, puis sélectionnez **Enregistrer avec codage**. Dans la boîte de dialogue **Options d’enregistrement avancées**, choisissez l’option Unicode (UTF-8 avec signature) et cliquez sur **OK**.

## <a name="direct3d-feature-level-test"></a>Test du niveau de fonctionnalité Direct3D

### <a name="direct3d-feature-level-support"></a>Prise en charge du niveau de fonctionnalité Direct3D

Teste les applications Microsoft Direct3D pour s’assurer qu’elles ne se bloquent pas avec les matériels vidéo plus anciens.

### <a name="background"></a>Contexte

Microsoft Store nécessite toutes les applications à l’aide de Direct3D pour un rendu correct ou échouent de manière cartes graphiques de 9\-1 au niveau de fonctionnalité.

Dans la mesure où les utilisateurs peuvent changer de matériel graphique sur leur appareil après l’installation de l’application, si vous choisissez un niveau de fonctionnalité minimal supérieur au niveau 9\-1, votre application doit détecter au démarrage si le matériel actuel répond ou non aux critères minimaux. Dans le cas contraire, l’application doit afficher un message qui détaille les critères exigés pour Direct3D. Par ailleurs, si une application est téléchargée sur un appareil avec lequel elle n’est pas compatible, elle doit détecter cette incompatibilité au démarrage et afficher un message expliquant au client la configuration requise.

### <a name="test-details"></a>Détails du test

Le test est validé si les applications assurent un rendu précis avec le niveau de fonctionnalité 9\-1.

### <a name="corrective-action"></a>Action corrective

Assurez-vous que votre application s’affiche correctement avec le niveau de fonctionnalité Direct3D 9\-1, même si vous vous attendez à ce qu’elle s’exécute à un niveau de fonctionnalité supérieur. Pour plus d’informations, voir [Développement pour différents niveaux de fonctionnalités Direct3D](http://go.microsoft.com/fwlink/p/?LinkID=253575).

### <a name="direct3d-trim-after-suspend"></a>Découpage Direct3D après suspension

> **Remarque**ce test s’applique uniquement aux applications UWP développées pour Windows8.1 et versions ultérieures.

### <a name="background"></a>Contexte

Si l’application n’appelle pas [**Trim**](https://msdn.microsoft.com/library/windows/desktop/Dn280346) sur son périphérique Direct3D, elle ne libère pas la mémoire allouée pour sa précédente tâche3D. Cela augmente le risque que les applications soient arrêtées en raison de la sollicitation de la mémoire système.

### <a name="test-details"></a>Détails du test

Vérifie la conformité des applications avec les spécifications D3D et s’assure que les applications appellent une nouvelle API [**Trim**](https://msdn.microsoft.com/library/windows/desktop/Dn280346) lors de leur rappel de suspension.

### <a name="corrective-action"></a>Action corrective

L’application doit appeler l’API [**Trim**](https://msdn.microsoft.com/library/windows/desktop/Dn280346) sur son interface [**IDXGIDevice3**](https://msdn.microsoft.com/library/windows/desktop/Dn280345) chaque fois qu’elle est sur le point d’être suspendue.

## <a name="app-capabilities-test"></a>Test des fonctionnalités de l’application

### <a name="special-use-capabilities"></a>Fonctionnalités à usage spécial

### <a name="background"></a>Contexte

Les fonctionnalités à usage spécial sont destinées à des scénarios très spécifiques. Seuls les comptes d’entreprise sont autorisés à utiliser ces fonctionnalités.

### <a name="test-details"></a>Détails du test

Le test est validé si l’application déclare une ou plusieurs des fonctionnalités suivantes :

-   EnterpriseAuthentication
-   SharedUserCertificates
-   DocumentsLibrary

Si au moins une de ces fonctionnalités est déclarée, le test affiche un message d’avertissement pour l’utilisateur.

### <a name="corrective-actions"></a>Actions correctives

Envisagez de supprimer la fonctionnalité à usage spécial si votre application n’en a pas besoin. De plus, l’utilisation de ces fonctionnalités est sujette à un examen supplémentaire de la stratégie d’accueil.

## <a name="windows-runtime-metadata-validation"></a>Validation des métadonnées Windows Runtime

### <a name="background"></a>Arrière-plan

S’assure que les composants fournis avec une application sont conformes au système de type UWP.

### <a name="test-details"></a>Détails du test

Vérifie que les fichiers **.winmd** du package sont conformes aux règles UWP.

### <a name="corrective-actions"></a>Actions correctives

-   **Test de l’attribut ExclusiveTo:** s’assure que les classes UWP n’implémentent pas d’interfaces marquées comme étant des interfaces exclusives d’une autre classe.
-   **Test d’emplacement du type:** s’assure que les métadonnées de tous les types UWP se trouvent dans le fichier winmd dont le nom correspondant à l’espace de noms est le plus long du package d’application.
-   **Test de respect de la casse du nom du type:** s’assure que tous les types UWP de votre package d’application ont un nom unique qui ne respecte pas la casse. S’assure également qu’aucun nom de type UWP n’est utilisé comme nom d’espace de noms dans votre package d’application.
-   **Test d’exactitude du nom du type:** s’assure qu’aucun type UWP ne se trouve dans l’espace de noms global ni dans l’espace de noms Windows de niveau supérieur.
-   **Test d’exactitude des métadonnées générales:** s’assure que le compilateur que vous utilisez pour générer vos types est conforme aux dernières spécifications UWP.
-   **Test des propriétés:** s’assure que toutes les propriétés d’une classe UWP disposent d’une méthode Get (les méthodes Set sont facultatives). S’assure que le type de la valeur retournée par la méthode Get correspond au type du paramètre d’entrée de la méthode Set pour toutes les propriétés des types UWP.

## <a name="package-sanity-tests"></a>Tests de validité des packages

### <a name="platform-appropriate-files-test"></a>Test des fichiers appropriés à la plateforme

Les applications qui installent des fichiers mixtes binaires peuvent se bloquer ou ne pas s’exécuter correctement selon l’architecture du processeur de l’utilisateur.

### <a name="background"></a>Contexte

Ce test valide les conflits d’architecture sur les fichiers binaires stockés dans un package d’application. Un package d’application ne doit pas inclure des fichiers binaires qui ne peuvent pas être utilisés sur l’architecture de processeur spécifiée dans le manifeste. Inclure des fichiers binaires non pris encharge peut entraîner le blocage de votre application ou une augmentation inutile de la taille de son package.

### <a name="test-details"></a>Détails du test

S’assure que le nombre de bits figurant dans l’en-tête PE de chaque fichier est approprié en cas de référence croisée avec la déclaration de l’architecture de processeur du package d’application.

### <a name="corrective-action"></a>Action corrective

Suivez les recommandations suivantes pour vous assurer que votre package d’application contient uniquement des fichiers pris en charge par l’architecture spécifiée dans le manifeste d’application:

-   Si l’architecture du processeur cible de votre application a un type de processeur Neutre, le package d’application ne peut pas contenir des fichiers binairesx86, x64 ou ARM, ni de fichiers de types d’images.

-   Si l’architecture du processeur cible de votre application a un type de processeurx86, le package d’application doit uniquement contenir des fichiers binairesx86 ou des fichiers de types d’images. Si le package contient des fichiers binairesx64 ou ARM, ou des fichiers de types d’images, il échouera au test.

-   Si l’architecture du processeur cible de votre application a un type de processeurx64, le package d’application doit contenir des fichiers binairesx64 ou des fichiers de types d’images. Notez que, dans ce cas, le package peut également inclure des fichiersx86, mais l’expérience d’application principale doit utiliser le fichier binairex64.

    Toutefois, si le package contient des fichiers binairesARM ou des fichiers de types d’images, ou s’il contient uniquement des fichiers binairesx86 ou des fichiers de types d’images, il échouera au test.

-   Si l’architecture du processeur cible de votre application a un type de processeurARM, le package d’application doit uniquement contenir des fichiers binairesARM ou des fichiers de types d’images. Si le package contient des fichiers binairesx64 oux86, ou des fichiers de types d’images, il échouera au test.

### <a name="supported-directory-structure-test"></a>Test de la structure de répertoires prise en charge

S’assure que les applications ne créent pas de sous-répertoires plus longs que MAX\-PATH dans le cadre de l’installation.

### <a name="background"></a>Arrière-plan

Les composants du système d’exploitation (notamment Trident, WWAHost, etc.) sont limités en interne à MAX\-PATH pour les chemins d’accès au système de fichiers et ne fonctionnent pas correctement pour les chemins plus longs.

### <a name="test-details"></a>Détails du test

Vérifie qu’aucun chemin d’accès dans le répertoire d’installation de l’application ne dépasse MAX\-PATH.

### <a name="corrective-action"></a>Action corrective

Utilisez une structure de répertoires et/ou un nom de fichier plus court.

## <a name="resource-usage-test"></a>Test d’utilisation des ressources

### <a name="winjs-background-task-test"></a>Test de la tâche en arrière-plan WinJS

Le test de la tâche en arrière-plan WinJS s’assure que les applications JavaScript comportent les instructions close adéquates afin que l’application ne consomme pas inutilement la batterie.

### <a name="background"></a>Arrière-plan

Les applications comportant des tâches en arrière-plan JavaScript doivent appeler Close() en dernière instruction dans leur tâche en arrière-plan. Les applications qui ne respectent pas cette règle risquent d’empêcher le système de retourner au mode de veille connectée et entraîner le déchargement de la batterie.

### <a name="test-details"></a>Détails du test

Si aucun fichier de tâche en arrière-plan n’est spécifié dans le manifeste de l’application, le test réussit. Dans le cas contraire, le test analyse le fichier de tâche en arrière-plan JavaScript qui est spécifié dans le package d’application et recherche une instruction Close(). S’il trouve l’instruction, le test réussit, sinon le test échoue.

### <a name="corrective-action"></a>Action corrective

Mettez à jour le code JavaScript en arrière-plan pour appeler Close() correctement.


## <a name="related-topics"></a>Rubriques connexes

* [Tests d’application Pont du bureau Windows](windows-desktop-bridge-app-tests.md)
* [Politiques du MicrosoftStore](https://msdn.microsoft.com/library/windows/apps/Dn764944)
 
