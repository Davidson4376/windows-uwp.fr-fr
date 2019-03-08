---
title: Étendre votre application avec des services, des extensions et des packages
description: Décrit comment créer une tâche en arrière-plan qui s’exécute lorsque la mise à jour de votre application de store Universal Windows Platform (UWP).
ms.date: 05/07/2018
ms.topic: article
keywords: Windows 10, uwp, étendre, agencer, service d’application, package, extension
ms.localizationpriority: medium
ms.openlocfilehash: 47ab6491d09775bf86f0f484fc96d85bd07f53a4
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57590674"
---
# <a name="extend-your-app-with-services-extensions-and-packages"></a>Étendre votre application avec des services, des extensions et des packages

Il existe de nombreuses technologies dans Windows 10 pour l’extension et organiser en composants fournisseurs de votre application. Ce tableau devrait vous aider à déterminer quelle technologie, vous devez utiliser selon les besoins. Il est suivi par une brève description des scénarios et des technologies.

| Scénario                           | Package de ressources   | Package d'actifs      | Package facultatif   | Ensemble plat        | Extension d’application      | Service d’application        | Installation en continu  |
|------------------------------------|:------------------:|:------------------:|:------------------:|:------------------:|:------------------:|:------------------:|:------------------:|
| Plug-ins de code de tiers            |                    |                    |                    |                    | :heavy_check_mark: |                    |                    |
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

**Les plug-ins tiers**  

Code que vous pouvez télécharger dans le Store et exécuter depuis votre application. Les extensions du navigateur Microsoft Edge en sont un exemple.

**Plug-ins de code in-process**  

Code qui s’exécute in-process avec votre application. Le langage C++ est le seul pris en charge. Peut également intégrer du contenu. Étant donné que le code s’exécute in-process, on suppose un niveau supérieur de confiance. Vous pouvez choisir de ne pas exposer ce type d’extensibilité à un tiers.

**Ressources de l’expérience utilisateur (chaîne/images)**  

Les ressources d’interface utilisateur telles que les chaînes localisées, les images et tout autre contenu de l’interface utilisateur que vous souhaitez factoriser en fonction des paramètres régionaux ou pour toute autre raison.

**Sur le contenu à la demande**  

Contenu que vous voulez télécharger ultérieurement. Il peut par exemple s’agir d’achats dans l’application qui vous permettent de télécharger de nouveaux niveaux, de nouvelles apparences ou de nouvelles fonctionnalités.

**Licence distincte et acquisition**  

La possibilité d’acquérir le contenu et ses licences indépendamment de l’application.

**Acquisition de dans l’application**  

Indique si la programmation prend en charge l’acquisition de contenu depuis l’application.

**Optimiser le temps d’installation**

Fournit une fonctionnalité permettant de réduire le temps nécessaire à l’acquisition de l’application dans le Store ainsi qu’à son exécution.

**Réduction de l’encombrement du disque** réduit la taille d’une application en incluant uniquement les applications ou ressources nécessaires.

**Optimiser le packaging** optimise le processus de packaging d’application pour les applications à grande échelle ou complexes.

**Réduction de la durée de publication** réduit le temps nécessaire pour publier votre application dans le Store, un partage local ou un serveur web.

## <a name="technology-descriptions-the-columns-in-the-table-above"></a>Descriptions des technologies (colonnes du tableau ci-dessus)

**Package de ressources**

Les packages de ressource sont des packages composés uniquement de ressources. Ils permettent à votre application de s’adapter à plusieurs tailles d’écran et plusieurs langues du système. Le package de ressources cible la langue de l’utilisateur, l’échelle du système et les fonctionnalités DirectX, permettant à l’application de s’adapter à de nombreux scénarios d’utilisation. Même si un package d’application peut contenir plusieurs ressources, le système d’exploitation télécharge uniquement les ressources pertinentes pour chaque appareil de l’utilisateur, ce qui permet d’économiser la bande passante et l’espace disque.

**Package de ressource** packages d’actif sont une source commune et centralisée du fichier exécutable non exécutables des fichiers ou pour une utilisation par votre application. Il s’agit généralement de fichiers non processeur ou spécifiques au langage. Par exemple, cela peut inclure une collection d’images dans un package d'actifs et des vidéos dans un autre package d'actifs, qui sont utilisés par l’application. Si votre application prend en charge plusieurs architectures et plusieurs langues, ces ressources peuvent être incluses dans le package d’architecture ou le package de ressources, mais cela signifie également que les ressources doit se répéter plusieurs fois entre les différents packages de l’architecture, en tenant l’espace disque. Si des packages d'actifs sont utilisés, ils doivent être inclus uniquement dans le package d’application général une seule fois. Voir [Introduction aux packages d'actifs](../packaging/asset-packages.md) pour en savoir plus.

**Package facultatif**

Les packages facultatifs sont utilisés pour compléter ou étendre la fonctionnalité initiale d’un package d’application. Il est possible de publier une application et de publier ultérieurement des packages facultatifs, ou de publier l’application et les packages facultatifs simultanément. En étendant votre application via un package facultatif, vous bénéficiez des avantages qu’offrent la distribution et la monétisation de contenu sous forme de package d’application distinct. Les packages facultatifs sont généralement destinés à être développés par le développeur d’origine de l’application, dans la mesure où ils s’exécutent avec l’identité de l’application principale (contrairement aux extensions d’application). En fonction de la façon dont vous définissez votre package facultatif, vous pouvez charger du code, des ressources ou du code et des ressources depuis votre package facultatif vers votre application principale. Si vous avez besoin améliorer votre application avec du contenu qui peut être monétisée, sous licence, et des packages distribués séparément, puis facultatifs peuvent être le bon choix pour vous. Pour en savoir plus sur l’implémentation, voir [Packages facultatifs et création d’ensembles connexes](https://docs.microsoft.com/windows/uwp/packaging/optional-packages).

**Ensemble plat**
Les [Packages d'application d'ensemble plat](../packaging/flat-bundles.md) ressemblent à des ensembles d’applications normaux, sauf qu’au lieu d’inclure tous les packages d’application dans le dossier, l'ensemble plat contient uniquement des *références* à ces packages d'application. Comme l'ensemble plat contient des références aux packages d’application au lieu des fichiers eux-mêmes, il permet de réduire le temps nécessaire pour créer un package d'application et télécharger celle-ci.

**Extension d’application**

Les [extensions d'application](https://docs.microsoft.com/uwp/api/windows.applicationmodel.appextensions) permettent à votre application UWP d’héberger le contenu fourni par d’autres applications UWP. Découvrez, énumérez et accédez à du contenu en lecture seule à partir de ces applications.

Si une application prend en charge les extensions, n’importe quel développeur peut soumettre une extension pour l’application. Par conséquent, l’application hôte doit être robuste lorsqu’elle charge une extension qui n’a pas été préalablement testée pour fonctionner avec. Les extensions doivent être considérées comme non approuvées.

Les applications sont incapables de charger du code depuis les extensions. Si vous avez besoin de l’exécution de code, envisagez les services d’application.

**App Service**

Services d’application Windows une communication application-app en permettant à votre application UWP fournir des services vers une autre application Windows universelle. Les services d’application vous permettent de créer des services sans interface utilisateur, que les applications peuvent appeler sur le même appareil et, à partir de Windows 10 version 1607, sur des appareils distants. Voir [Créer et utiliser un service d’application](https://docs.microsoft.com/windows/uwp/launch-resume/how-to-create-and-consume-an-app-service) pour plus d’informations.

Les services d’application sont des applications UWP qui fournissent des services à d’autres applications UWP. Ils sont analogues aux services web sur un appareil. Un service d’application s’exécute sous forme de tâche en arrière-plan dans l’application hôte et peut fournir son service à d’autres applications. Un service d’application peut par exemple fournir un service de scanneur de code-barres que d’autres applications peuvent utiliser. Il peut également s’agir d’une suite d’applications d’entreprise partageant un service d’application de vérification orthographique, accessible aux autres applications de la suite.

**Installation de diffusion en continu des applications UWP**

L’installation en continu est un moyen d’optimiser la manière dont votre application est fournie aux utilisateurs. Au lieu d’attendre le téléchargement complet de l’application pour pouvoir l’utiliser, les utilisateurs peuvent commencer à profiter de l’application dès qu’une partie requise a été téléchargée. C’est à vous, en tant que développeur, de segmenter votre application en une section requise pour son activation de base et son lancement, et en contenu supplémentaire pour le reste de l’application. Voir [Installation en continu d’une application UWP](https://docs.microsoft.com/windows/uwp/packaging/streaming-install) pour obtenir plus d’informations et des détails sur l’implémentation.

## <a name="see-also"></a>Voir aussi

[Créer et consommer un service d’application](https://docs.microsoft.com/windows/uwp/launch-resume/how-to-create-and-consume-an-app-service)  
[Introduction aux packages d’actif](../packaging/asset-packages.md)  
[Création d’un package avec la mise en page de l’empaquetage](../packaging/packaging-layout.md)  
[Création de jeu associé et de packages facultatifs](https://docs.microsoft.com/windows/uwp/packaging/optional-packages)  
[Développement avec les packages d’actif et le pliage de package](../packaging/package-folding.md)  
[Installation de diffusion en continu l’application UWP](https://docs.microsoft.com/windows/uwp/packaging/streaming-install)  
[Packages d’application bundle plat](../packaging/flat-bundles.md)  
[Espace de noms Windows.ApplicationModel.AppService](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.AppService)  
[Espace de noms Windows.ApplicationModel.Extensions](https://docs.microsoft.com/uwp/api/windows.applicationmodel.appextensions)  
