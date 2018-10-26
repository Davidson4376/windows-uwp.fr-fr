---
author: TylerMSFT
title: Étendre votre application avec des services, des extensions et des packages
description: Découvrez comment créer une tâche en arrière-plan qui s’exécute lorsque votre application du Windows Store de la plateforme Windows universelle (UWP) est mise à jour.
ms.author: twhitney
ms.date: 05/7/2018
ms.topic: article
keywords: Windows10, uwp, étendre, agencer, service d’application, package, extension
ms.localizationpriority: medium
ms.openlocfilehash: 624d52ff96fb2537afa3affb2d842aa29e1e6667
ms.sourcegitcommit: b7e3d222e229cdbf04e837fcb94fb7d84a93de09
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/26/2018
ms.locfileid: "5597661"
---
# <a name="extend-your-app-with-services-extensions-and-packages"></a>Étendre votre application avec des services, des extensions et des packages

Windows10 intègre différentes technologies qui vous aideront à étendre et agencer votre application. Ce tableau devrait vous aider à déterminer quelle technologie utiliser pour votre scénario. Il est suivi par une brève description des scénarios et des technologies.

| Scénario                           | Package de ressources   | Package d'actifs      | Package facultatif   | Ensemble plat        | Extension d’application      | Service d’application        | Installation en continu  |
|------------------------------------|:------------------:|:------------------:|:------------------:|:------------------:|:------------------:|:------------------:|:------------------:|
| Plug-ins tiers dédiés au code            |                    |                    |                    |                    | :heavy_check_mark: |                    |                    |
| Plug-ins in-process dédiés au code              |                    |                    | :heavy_check_mark: |                    |                    |                    |                    |
| Ressources d’expérience utilisateur (chaînes/images)         | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |                    | :heavy_check_mark: |                    | :heavy_check_mark: |
| Contenu à la demande <br/> (par exemple, des niveaux supplémentaires) |      |                    | :heavy_check_mark: |                    | :heavy_check_mark: |                    | :heavy_check_mark: |
| Licence et acquisition distinctes |                    |                    | :heavy_check_mark: |                    | :heavy_check_mark: | :heavy_check_mark: |                    |
| Acquisition dans l’application                 |                    |                    | :heavy_check_mark: |                    | :heavy_check_mark: |                    |                    |
| Optimisation de la durée d’installation              | :heavy_check_mark: |                    | :heavy_check_mark: |                    | :heavy_check_mark: |                    | :heavy_check_mark: |
| Réduction de l’encombrement du disque              | :heavy_check_mark: |                    | :heavy_check_mark: |                    |                    |                    |                    |
| Optimisation de la création de packages                 |                    | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |                    |                    |                    |
| Réduction de la durée de publication             | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |                    |                    |                    |

## <a name="scenario-descriptions-the-rows-in-the-table-above"></a>Descriptions des scénarios (lignes du tableau ci-dessus)

**Plug-ins tiers**  

Code que vous pouvez télécharger dans le Store et exécuter depuis votre application. Les extensions du navigateur MicrosoftEdge en sont un exemple.

**Plug-ins in-process dédiés au code**  

Code qui s’exécute in-process avec votre application. Le langage C++ est le seul pris en charge. Peut également intégrer du contenu. Étant donné que le code s’exécute in-process, on suppose un niveau supérieur de confiance. Vous pouvez choisir de ne pas offrir ce type d’extensibilité à un tiers.

**Ressources d’expérience utilisateur (chaînes/images)**  

Les ressources d’interface utilisateur telles que les chaînes localisées, les images et tout autre contenu de l’interface utilisateur que vous souhaitez factoriser en fonction des paramètres régionaux ou pour toute autre raison.

**Contenu à la demande**  

Contenu que vous voulez télécharger ultérieurement. Il peut par exemple s’agir d’achats dans l’application qui vous permettent de télécharger de nouveaux niveaux, de nouvelles apparences ou de nouvelles fonctionnalités.

**Licence et acquisition distinctes**  

La possibilité d’acquérir le contenu et ses licences indépendamment de l’application.

**Acquisition dans l’application**  

Indique si la programmation prend en charge l’acquisition de contenu depuis l’application.

**Optimisation de la durée d’installation**

Fournit une fonctionnalité permettant de réduire le temps nécessaire à l’acquisition de l’application dans le Store ainsi qu’à son exécution.

**Réduction de l’encombrement du disque** réduit la taille d’une application en incluant uniquement les applications ou ressources nécessaires.

**Optimisation de la création de packages** permet d’optimiser le processus de création de packages d’application pour les applications complexes ou à grande échelle.

**Réduction de la durée de publication** réduit le temps nécessaire pour publier votre application dans le Store, un partage local ou un serveur web.

## <a name="technology-descriptions-the-columns-in-the-table-above"></a>Descriptions des technologies (colonnes du tableau ci-dessus)

**Package de ressources**

Les packages de ressource sont des packages composés uniquement de ressources. Ils permettent à votre application de s’adapter à plusieurs tailles d’écran et plusieurs langues du système. Le package de ressources cible la langue de l’utilisateur, l’échelle du système et les fonctionnalités DirectX, permettant à l’application de s’adapter à de nombreux scénarios d’utilisation. Même si un package d’application peut contenir plusieurs ressources, le système d’exploitation télécharge uniquement les ressources pertinentes pour chaque appareil de l’utilisateur, ce qui permet d’économiser la bande passante et l’espace disque.

**Package d'actifs** Les packages d'actifs sont une source centralisée commune de fichiers exécutables ou non exécutables utilisés par votre application. Ces fichiers sont généralement non-processeur ou spécifiques à une langue. Par exemple, cela peut inclure une collection d’images dans un package d'actifs et des vidéos dans un autre package d'actifs, qui sont utilisés par l’application. Par exemple, cela peut inclure une collection d’images dans un package d'actifs et des vidéos dans un autre package d'actifs. Si votre application prend en charge plusieurs architectures et plusieurs langues, ces actifs peuvent être inclus dans le package d'architecture ou dans le package de ressources, mais cela signifie également que les actifs doivent être dupliqués plusieurs fois entre les différents packages d'architecture, ce qui prend de l’espace disque. Si des packages d'actifs sont utilisés, ils doivent être inclus uniquement dans le package d’application général une seule fois. Voir [Introduction aux packages d'actifs](../packaging/asset-packages.md) pour en savoir plus.

**Package facultatif**

Les packages facultatifs sont utilisés pour compléter ou étendre la fonctionnalité initiale d’un package d’application. Il est possible de publier une application et de publier ultérieurement des packages facultatifs, ou de publier l’application et les packages facultatifs simultanément. En étendant votre application via un package facultatif, vous bénéficiez des avantages qu’offrent la distribution et la monétisation de contenu sous forme de package d’application distinct. Les packages facultatifs sont généralement destinés à être développés par le développeur d’origine de l’application, dans la mesure où ils s’exécutent avec l’identité de l’application principale (contrairement aux extensions d’application). En fonction de la façon dont vous définissez votre package facultatif, vous pouvez charger du code, des ressources ou du code et des ressources depuis votre package facultatif vers votre application principale. Si vous cherchez à améliorer votre application avec du contenu qu’il est possible de monétiser, d’octroyer sous licence et de distribuer séparément, les packages facultatifs peuvent s’avérer le bon choix pour vous. Pour en savoir plus sur l’implémentation, voir [Packages facultatifs et création d’ensembles connexes](https://docs.microsoft.com/windows/uwp/packaging/optional-packages).

**Ensemble plat**
Les [Packages d'application d'ensemble plat](../packaging/flat-bundles.md) ressemblent à des ensembles d’applications normaux, sauf qu’au lieu d’inclure tous les packages d’application dans le dossier, l'ensemble plat contient uniquement des *références* à ces packages d'application. Comme l'ensemble plat contient des références aux packages d’application au lieu des fichiers eux-mêmes, il permet de réduire le temps nécessaire pour créer un package d'application et télécharger celle-ci.

**Extension d’application**

Les [extensions d'application](https://docs.microsoft.com/uwp/api/windows.applicationmodel.appextensions) permettent à votre application UWP d’héberger le contenu fourni par d’autres applications UWP. Découvrez, énumérez et accédez à du contenu en lecture seule à partir de ces applications.

Si une application prend en charge les extensions, n’importe quel développeur peut soumettre une extension pour l’application. Par conséquent, l’application hôte doit être robuste lorsqu’elle charge une extension qui n’a pas été préalablement testée pour fonctionner avec. Les extensions doivent être considérées comme non approuvées.

Les applications sont incapables de charger du code depuis les extensions. Si vous avez besoin de l’exécution de code, envisagez les services d’application.

**Service d’application**

Les services d’application Windows permettent la communication entre applications en autorisant votre application UWP à fournir des services à une autre application Windows universelle. Les services d’application vous permettent de créer des services sans interface utilisateur, que les applications peuvent appeler sur le même appareil et, à partir de Windows10 version1607, sur des appareils distants. Voir [Créer et utiliser un service d’application](https://docs.microsoft.com/windows/uwp/launch-resume/how-to-create-and-consume-an-app-service) pour plus d’informations.

Les services d’application sont des applications UWP qui fournissent des services à d’autres applications UWP. Ils sont semblables aux services web, sur un appareil. Un service d’application s’exécute sous forme de tâche en arrière-plan dans l’application hôte et peut fournir son service à d’autres applications. Un service d’application peut par exemple fournir un service de scanneur de code-barres que d’autres applications peuvent utiliser. Il peut également s’agir d’une suite d’applications d’entreprise partageant un service d’application de vérification orthographique, accessible aux autres applications de la suite.

**Installation en continu d’une application UWP**

L’installation en continu est un moyen d’optimiser la manière dont votre application est fournie aux utilisateurs. Au lieu d’attendre le téléchargement complet de l’application pour pouvoir l’utiliser, les utilisateurs peuvent commencer à profiter de l’application dès qu’une partie requise a été téléchargée. C’est à vous, en tant que développeur, de segmenter votre application en une section requise pour son activation de base et son lancement, et en contenu supplémentaire pour le reste de l’application. Voir [Installation en continu d’une application UWP](https://docs.microsoft.com/windows/uwp/packaging/streaming-install) pour obtenir plus d’informations et des détails sur l’implémentation.

## <a name="see-also"></a>Articles associés

[Créer et utiliser un service d’application](https://docs.microsoft.com/windows/uwp/launch-resume/how-to-create-and-consume-an-app-service)  
[Introduction aux packages d'actifs](../packaging/asset-packages.md)  
[Création de package à l'aide de la disposition de mise en package](../packaging/packaging-layout.md)  
[Packages facultatifs et création d’ensembles connexes](https://docs.microsoft.com/windows/uwp/packaging/optional-packages)  
[Développement de packages d'actifs et mise en dossier de packages](../packaging/package-folding.md)  
[Installation en continu d’une application UWP](https://docs.microsoft.com/windows/uwp/packaging/streaming-install)  
[Packages d'application d'ensemble plat](../packaging/flat-bundles.md)  
[Espace de noms Windows.ApplicationModel.AppService](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.AppService)  
[Espace de noms Windows.ApplicationModel.Extensions](https://docs.microsoft.com/uwp/api/windows.applicationmodel.appextensions)  