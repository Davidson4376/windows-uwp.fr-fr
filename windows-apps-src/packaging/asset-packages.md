---
author: laurenhughes
title: Introduction aux packages d'actifs
description: Les packages d'actifs désignent un type de package qui agit en tant qu'emplacement centralisé pour les fichiers communs d'une application. Ainsi, la nécessité de dupliquer les fichiers au travers de ses packages d'architecture est efficacement éliminée.
ms.author: lahugh
ms.date: 09/30/2018
ms.topic: article
keywords: windows10, création de packages, disposition de package, package d'actifs
ms.localizationpriority: medium
ms.openlocfilehash: 98980e67d24eb96aa55af7fefe10b5e4c2cdfa67
ms.sourcegitcommit: 753e0a7160a88830d9908b446ef0907cc71c64e7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/30/2018
ms.locfileid: "5768642"
---
# <a name="introduction-to-asset-packages"></a>Introduction aux packages d'actifs

> [!IMPORTANT]
> Si vous avez l'intention de soumettre votre application au Store, vous devez contacter le [Support technique pour les développeurs Windows](https://developer.microsoft.com/windows/support) et obtenir l’autorisation d’utiliser des packages d'actifs.

Les packages d'actifs désignent un type de package qui agit en tant qu'emplacement centralisé pour les fichiers communs d'une application. Ainsi, la nécessité de dupliquer les fichiers au travers de ses packages d'architecture est efficacement éliminée. Le package d'actifs est similaire au package de ressources dans la mesure où ils sont tous deux conçus pour présenter un contenu statique nécessaire à l'exécution de votre application. Néanmoins, ils diffèrent dans la mesure où les packages d'actif sont toujours téléchargés, quelle que soit l'architecture système, la langue ou l'échelle d'affichage de l'utilisateur.

![Diagramme d'offre groupée de package d'actifs](images/primary-bundle.png)

Dans la mesure où les packages d'actifs contiennent tous les fichiers agnostiques d'architecture, de langue et d'échelle, la rentabilisation des packages d'actifs engendre une réduction de la taille de l'application générale mise en package (puisque ces fichiers ne sont plus dupliqués). Ainsi, il est plus facile de gérer l'utilisation de l'espace disque de développement local pour les grandes applications et de gérer les packages de votre application de manière générale. 

### <a name="how-do-asset-packages-affect-publishing"></a>Comment la publication est-elle affectée par les packages d'actifs?
L'avantage le plus évident de ces packages d'actifs concerne la réduction de taille des applications mises en package. Une taille réduite de package d'application permet d'accélérer le processus de publication de l'application. En effet, le processus Store reçoit moins de fichiers. Toutefois, il ne s'agit pas de l'avantage le plus important.

Lorsqu'un package d'actifs est créé, vous pouvez spécifier si le package doit être autorisé à une exécution. Dans la mesure où les packages d'actifs doivent contenir uniquement les fichiers agnostiques d'architecture, ils ne contiennent généralement aucun fichier .dll ou .exe. De ce fait, en général, les packages d'actifs ne nécessitent pas d'être exécutés. Cette distinction est importante lors du processus de publication. Tous les packages exécutables doivent être scannés pour vérifier qu'ils ne contiennent aucun programme malveillant. Pour les packages plus gros, ce processus prend plus longtemps. Néanmoins, si un package est désigné comme non exécutable, l'installation de l'application garantira que les fichiers qu'il contient ne peuvent pas être exécutés. Cette garantie élimine le besoin d'une numérisation complète du package et réduira grandement le délai de vérification des programme malveillant lors de la publication de l'application (ainsi que des mises à jour). Ainsi, le processus de publication est grandement accéléré pour les applications employant des packages d'actifs. Notez que [les packages d’application ensemble plat](flat-bundles.md) doit également être utilisé pour obtenir cet avantage publication car c’est ce qui permet à la banque traiter chaque fichier de package .appx ou .msix en parallèle. 


### <a name="should-i-use-asset-packages"></a>Devrais-je utiliser des packages d'actifs?
La mise à jour de la structure de fichier de votre application pour tirer partie de l'utilisation des packages d'actifs peut engendrer des avantages concrets: une taille de package réduite et des itérations de développement plus fines. Si vos packages d'architecture contiennent tous une quantité importante de fichiers en commun ou si a majeure partie de votre application se compose de fichiers non exécutés, il est fortement conseillé que vous consacriez un délai supplémentaire à la conversion pour l'utilisation de packages d'actifs.

Néanmoins, il est accepté que les packages d'actifs ne constituent pas un moyen de parvenir à l'option de contenu d'application. Les fichiers de package d'actifs ne sont pas facultatifs, ils seront **toujours** téléchargés, quelle que soit l’architecture, langue et l'échelle de l’appareil cible, et tout autre contenu facultatif que votre application doit prendre en charge doit être implémenté à l'aide des [packages facultatifs](optional-packages.md). 


### <a name="how-to-create-an-asset-package"></a>Comment créer un package d'actifs
Le moyen le plus simple de créer des packages d'actifs consiste à utiliser la disposition de mise en package. Toutefois, les packages d'actifs peuvent également être créé manuellement à l'aide de MakeAppx.exe. Pour spécifier les fichiers à inclure dans le package d'actifs vous devez créer un «fichier de mappage». Dans cet exemple, l'unique fichier du package d'actifs est le fichier «Video.mp4», mais c'est ici que tous fichiers du package d'actifs doivent être répertoriés. Notez que le spécificateur **ResourceDimensions** dans **ResourceMetadata** est omis pour les packages d'actifs (par rapport à un fichier de mappage destiné aux packages de ressources).

```example 
[ResourceMetadata]
"ResourceId"        "Videos"

[Files]
"Video.mp4"         "Video.mp4"
```

Utilisez cette commande pour créer le package d'actifs à l'aide de MakeAppx.exe: 

```syntax 
MakeAppx.exe pack /r /m AppxManifest.xml /f MappingFile.txt /p Videos.appx

...

MakeAppx.exe pack /r /m AppxManifest.xml /f MappingFile.txt /p Videos.msix

```
Il faut noter ici que tous les fichiers référencés dans AppxManifest (les fichiers de logo) ne peuvent pas être déplacés vers des packages d'actifs. En effet, ces fichiers doivent être dupliqués parmi les packages d'architecture. Les packages d'actifs ne doivent contenir aucun fichier resources.pri. MRT ne peut pas être utilisé pour accéder aux fichiers du package d'actifs. Pour en savoir plus sur l’accès aux fichiers de package d'actifs et sur la raison pour laquelle les packages d'actifs nécessitent que votre application soit installé sur un lecteur NTFS, consultez [Développement avec des packages d'actifs et mise en dossier du package](Package-Folding.md).

Pour vérifier si un package d'actifs est autorisé à s'exécuter ou non, vous pouvez utiliser **[uap6:AllowExecution](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap6-allowexecution)** dans l'élément **Propriétés** d'AppxManifest. Vous devez également ajouter **uap6** à l'élément de niveau supérieur **Package** pour obtenir l'élément suivant: 

```XML
<Package IgnorableNamespaces="uap uap6" 
xmlns:uap6="http://schemas.microsoft.com/appx/manifest/uap/windows10/6" 
xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10" 
xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10">
```

 Si elle n'est pas spécifiée, la valeur par défaut de **AllowExecution** est **true**. Définissez la valeur **false** pour les packages d'actifs dépourvus de fichiers exécutables pour une publication plus rapide.  



