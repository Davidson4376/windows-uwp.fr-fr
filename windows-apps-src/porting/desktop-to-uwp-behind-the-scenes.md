---
author: normesta
Description: "Cet article présente de manière approfondie le fonctionnement interne de Pont du bureau."
title: "Fonctionnement détaillé de Pont du bureau"
ms.author: normesta
ms.date: 05/25/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.assetid: a399fae9-122c-46c4-a1dc-a1a241e5547a
ms.openlocfilehash: 050499baaf383fc135d833ae1e4733c95f2b5fa1
ms.sourcegitcommit: 7540962003b38811e6336451bb03d46538b35671
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/26/2017
---
# <a name="behind-the-scenes-of-the-desktop-bridge"></a>Fonctionnement détaillé de Pont du bureau

Cet article présente de manière approfondie le fonctionnement interne de Pont du bureau.

L’objectif principal de Pont du bureau est de séparer autant que possible l’état de l’application de l’état du système tout en conservant la compatibilité avec les autres applications. Pour ce faire, le pont place l’application dans un package de plateforme Windows universelle (UWP), puis détecte et redirige certaines modifications qu’il apporte au système de fichiers et au Registre lors de l’exécution.

Les packages que vous créez pour vos applications de bureau sont des applications totalement approuvées, uniquement de bureau, qui ne sont pas virtualisées ni mises en mode Bac à sable. Ainsi, ils peuvent interagir avec d’autres applications comme le font les applications de bureau classiques.

## <a name="installation"></a>Installation

Les packages d’application sont installés sous *C:\Program Files\WindowsApps\nom_package*, avec le fichier exécutable intitulé *app_name.exe*. Chaque dossier du package comporte un manifeste (nommé AppxManifest.xml) qui contient un espace de noms XML spécial pour les applications empaquetées. Ce fichier manifeste contient un élément ```<EntryPoint>``` qui fait référence à l’application totalement approuvée. Lorsque cette application est lancée, elle ne s’exécute pas à l’intérieur d’un conteneur d’applications, mais comme un utilisateur le ferait normalement.

Après le déploiement, les fichiers du package sont marqués en lecture seule et fortement verrouillés par le système d’exploitation. Windows empêche le lancement des applications si ces fichiers sont falsifiés.

## <a name="file-system"></a>Système de fichiers

Afin de contenir l’état de l’application, le pont tente de capturer les modifications apportées par l’application à AppData. Toutes les écritures effectuées dans le dossier AppData de l’utilisateur (par exemple, *C:\Users\user_name\AppData*), y compris la création, la suppression et la mise à jour, sont copiées au moment de l’écriture dans un emplacement privé par utilisateur, par application. Cela crée l’illusion que l’application empaquetée modifie vraiment AppData alors qu’elle modifie en fait une copie privée. En redirigeant les écritures de cette façon, le système peut suivre toutes les modifications de fichier effectuées par l’application. Ainsi le système peut nettoyer ces fichiers lorsque l’application est désinstallée, réduisant la «détérioration» du système et fournissant une meilleure expérience de suppression d’application à l’utilisateur.

En plus d’effectuer la redirection vers AppData, le pont fusionne dynamiquement les dossiers connus de Windows (System32, Program Files (x86), etc.) avec les répertoires correspondants dans le package de l’application. Chaque package contient un dossier nommé «VFS» à sa racine. Toutes les lectures de répertoires ou de fichiers dans le répertoire VFS sont fusionnées à l’exécution avec leurs équivalents natifs respectifs. Par exemple, une application peut contenir *C:\Program Files\WindowsApps\nom_package\VFS\SystemX86\vc10.dll* dans son package d’application, tandis que le fichier semble être installé dans *C:\Windows\System32\vc10.dll*.  Cela permet de préserver la compatibilité avec les applications de bureau qui s’attendent à trouver les fichiers dans d’autres emplacements que les packages eux-mêmes.

Les écritures dans les fichiers/dossiers du package d’application ne sont pas autorisées. Les écritures dans les fichiers et dossiers qui ne font pas partie du package sont ignorées par le pont et sont autorisées dans la mesure où l’utilisateur a une autorisation.

### <a name="common-operations"></a>Opérations courantes

Le petit tableau de référence suivant présente les opérations courantes du système de fichiers et la manière dont le pont les gère.

Opération | Résultat | Exemple
:--- | :--- | :---
Lire ou énumérer un fichier ou un dossier Windows connu | Fusion dynamique de *C:\Program Files\nom_package\VFS\dossier_connu* avec l’équivalent du système local. | La lecture de *C:\Windows\System32* renvoie le contenu de *C:\Windows\System32* ainsi que le contenu de *C:\Program Files\WindowsApps\nom_package\VFS\SystemX86*.
Écrire sous AppData | Copie à l’écriture dans un emplacement par utilisateur, par application. | AppData est généralement *C:\Users\nom_utilisateur\AppData*.  
Écrire à l’intérieur du package | Non autorisé. Le package est en lecture seule. | Les écritures sous *C:\Program Files\WindowsApps\nom_package* ne sont pas autorisées.
Écrit en dehors du package | Ignoré par le pont. Autorisé si l’utilisateur a des autorisations. | Une écriture dans *C:\Windows\System32\foo.dll* est autorisée si le package ne contient pas *C:\Program Files\WindowsApps\nom_package\VFS\SystemX86\foo.dll* et si l’utilisateur a des autorisations.

### <a name="packaged-vfs-locations"></a>Emplacements de VFS empaquetés

Le tableau suivant montre où les fichiers intégrés à votre package sont superposés sur le système de l’application. Votre application détectera ces fichiers comme se trouvant dans les emplacements système répertoriés, alors qu’en réalité, ils se trouvent dans les emplacements redirigés à l’intérieur de *C:\Program Files\WindowsApps\nom_package\VFS*. Les emplacements FOLDERID proviennent des constantes [**KNOWNFOLDERID**](https://msdn.microsoft.com/library/windows/desktop/dd378457.aspx).

Emplacement système | Emplacement redirigé (sous [Racine du package] \VFS\) | Valide sur les architectures
 :--- | :--- | :---
FOLDERID_SystemX86 | SystemX86 | x86, amd64
FOLDERID_System | SystemX64 | amd64
FOLDERID_ProgramFilesX86 | ProgramFilesX86 | x86, amd6
FOLDERID_ProgramFilesX64 | ProgramFilesX64 | amd64
FOLDERID_ProgramFilesCommonX86 | ProgramFilesCommonX86 | x86, amd64
FOLDERID_ProgramFilesCommonX64 | ProgramFilesCommonX64 | amd64
FOLDERID_Windows | Windows | x86, amd64
FOLDERID_ProgramData | Common AppData | x86, amd64
FOLDERID_System\catroot | AppVSystem32Catroot | x86, amd64
FOLDERID_System\catroot2 | AppVSystem32Catroot2 | x86, amd64
FOLDERID_System\drivers\etc | AppVSystem32DriversEtc | x86, amd64
FOLDERID_System\driverstore | AppVSystem32Driverstore | x86, amd64
FOLDERID_System\logfiles | AppVSystem32Logfiles | x86, amd64
FOLDERID_System\spool | AppVSystem32Spool | x86, amd64

## <a name="registry"></a>Registre

Le pont gère le Registre de la même manière que le système de fichiers. Les packages d’application contiennent un fichier registry.dat, qui est l’équivalent logique de *HKLM\Software* dans le vrai Registre. À l’exécution, ce Registre virtuel fusionne le contenu de cette ruche dans la ruche du système natif afin de fournir un affichage unique des deux. Par exemple, si registry.dat contient une seule clé «Foo», la lecture de *HKLM\Software* à l’exécution semble également contenir «Foo» (en plus de toutes les clés système natives).

Seules les clés sous *HKLM\Software* font partie du package. Les touches sous *HKCU* ou d’autres parties du Registre n’en font pas partie. Les écritures dans les clés ou les valeurs du package ne sont pas autorisées. Les écritures dans les clés ou les valeurs qui ne font pas partie du package sont ignorées par le pont et sont autorisées dans la mesure où l’utilisateur a une autorisation.

Toutes les écritures sous HKCU sont copiées à l’écriture dans un emplacement privé par utilisateur, par application. Les avantages sont les mêmes que lorsque le pont gère le système de fichiers pour la désinstallation du nettoyage. Traditionnellement, les programmes de désinstallation ne parviennent pas à nettoyer *HKEY_CURRENT_USER* car les données du Registre des utilisateurs déconnectées sont déchargées et inaccessibles.

Toutes les écritures sont conservées pendant la mise à niveau du package et supprimées seulement lorsque l’application est entièrement supprimée.

### <a name="common-operations"></a>Opérations courantes

Le petit tableau de référence suivant présente les opérations courantes du Registre et la manière dont le pont les gère.

Opération | Résultat | Exemple
:--- | :--- | :---
Lire ou énumérer *HKLM\Software* | Fusion dynamique de la ruche du package avec l’équivalent du système local. | Si registry.dat contient une seule clé «Foo», à l’exécution la lecture de *HKLM\Software* affiche le contenu de *HKLM\Software* et de *HKLM\Software\Foo*.
Écritures sous HKCU | Copie à l’écriture dans un emplacement privé par utilisateur, par application. | Identique à AppData pour les fichiers.
Écritures à l’intérieur du package. | Non autorisé. Le package est en lecture seule. | Les écritures sous *HKLM\Software* ne sont pas autorisées si un clé/valeur correspondante existe dans la ruche du package.
Écrit en dehors du package | Ignoré par le pont. Autorisé si l’utilisateur a des autorisations. | Les écritures sous *HKLM\Software* sont autorisées dans la mesure où une clé/valeur correspondante n’existe pas dans la ruche du package et où l’utilisateur a les autorisations d’accès appropriées.

## <a name="uninstallation"></a>Désinstallation

Lorsqu’un package est désinstallé par l’utilisateur, tous les fichiers et dossiers situés dans *C:\Program Files\WindowsApps\nom_package* sont supprimés, ainsi que les écritures redirigées vers AppData ou le Registre capturées par le pont.

## <a name="next-steps"></a>Étapes suivantes

**Trouver des réponses aux questions spécifiques**

Notre équipe contrôle ces [balises StackOverflow](http://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge).

**Envoyer vos commentaires concernant cet article**

Utilisez la section remarques ci-dessous.
