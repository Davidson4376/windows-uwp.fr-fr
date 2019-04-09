---
Description: MakePri.exe est un outil de ligne de commande que vous pouvez utiliser pour créer et vider des fichiers PRI. Il est intégré en tant qu’élément de MSBuild dans Microsoft Visual Studio, mais il peut aussi être utilisé pour la création de packages manuellement ou à l’aide d’un système de génération personnalisé.
title: Compiler des ressources manuellement avec MakePri.exe
template: detail.hbs
ms.date: 10/23/2017
ms.topic: article
keywords: windows 10, uwp, ressources, image, MRT, qualificateur
ms.localizationpriority: medium
ms.openlocfilehash: c6674fc38d41e3a18709dcb81edc95d164f9f86c
ms.sourcegitcommit: 46890e7f3c1287648631c5e318795f377764dbd9
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/21/2019
ms.locfileid: "58320592"
---
# <a name="compile-resources-manually-with-makepriexe"></a>Compiler des ressources manuellement avec MakePri.exe

MakePri.exe est un outil de ligne de commande que vous pouvez utiliser pour créer et vider des fichiers PRI. Il est intégré en tant qu’élément de MSBuild dans Microsoft Visual Studio, mais il peut aussi être utilisé pour la création de packages manuellement ou à l’aide d’un système de génération personnalisé.

> [!NOTE]
> MakePri.exe est installé lorsque vous activez le **Kit de développement logiciel Windows pour les applications UWP managées** option lors de l’installation du Kit de développement logiciel Windows. Il est installé dans le chemin d’accès `%WindowsSdkDir%bin\<WindowsTargetPlatformVersion>\x64\makepri.exe` (ainsi que dans les dossiers nommés pour les autres architectures). Par exemple, `C:\Program Files (x86)\Windows Kits\10\bin\10.0.17713.0\x64\makepri.exe`.

La limite de taille d’un fichier PRI est 64 kilo-octets.

## <a name="in-this-section"></a>Dans cette section
|Rubrique|Description|
|-|-|
| [Options de ligne de commande MakePri.exe](makepri-exe-command-options.md) | MakePri.exe inclut le jeu de commandes `createconfig`, `dump`, `new`, `resourcepack` et `versioned`. Cette rubrique détaille les options de ligne de commande utilisées avec ces commandes. |
| [Fichier de configuration MakePri.exe](makepri-exe-configuration.md) | Cette rubrique décrit le schéma du fichier de configuration XML de MakePri.exe. |
| [Indexeurs de spécifiques au format MakePri.exe](makepri-exe-format-specific-indexers.md) | Cette rubrique décrit les indexeurs spécifiques au format utilisés par l’outil MakePri.exe pour générer son index de ressources. |

## <a name="makepriexe-command-line-options"></a>Options de ligne de commande de MakePri.exe

MakePri.exe inclut le jeu de commandes `createconfig`, `dump`, `new`, `resourcepack` et `versioned`. Pour plus d’informations sur leur utilisation, voir [Options de ligne de commande de MakePri.exe](makepri-exe-command-options.md).

## <a name="makepriexe-configuration"></a>Configuration de MakePri.exe

Le ficher de configuration XML PRI détermine les ressources qui sont indexées et la façon dont l’indexation est effectuée. Le schéma du fichier XML de configuration est décrit dans [Configuration de MakePri.exe](makepri-exe-configuration.md).

## <a name="format-specific-indexers"></a>Indexeurs spécifiques au format

MakePri.exe est généralement utilisé avec les options `new`, `versioned` et `resourcepack`. Dans ces cas, il indexe les fichiers sources pour générer un index des ressources. MakePri.exe utilise plusieurs indexeurs individuels pour lire les fichiers de ressources sources ou les conteneurs pour les ressources. L’indexeur de base est l’indexeur de dossier. Il indexe le contenu d’un dossier pour des ressources telles que des images `.jpg` ou `.png`. Pour plus d’informations, voir [Indexeurs spécifiques au format de MakePri.exe](makepri-exe-format-specific-indexers.md).

## <a name="makepriexe-warnings-and-error-messages"></a>Avertissements et messages d’erreur de MakePri.exe

### <a name="resources-found-for-languages-languages-but-no-resources-found-for-default-languages-languages-change-the-default-language-or-qualify-resources-with-the-default-language"></a>Ressource a été trouvée pour l’ou les langues '< langue >', mais aucune ressource trouvée pour les langues par défaut : '< langue >'. Modifier la langue par défaut ou de qualifier des ressources avec la langue par défaut.

Cet avertissement s’affiche lorsque MakePri.exe ou MSBuild découvre des fichiers ou des ressources de type chaîne pour une ressource nommée donnée qui semblent être marquée avec les qualificateurs de langage, mais aucun candidat n’est trouvée pour une langue par défaut. La méthode d’utilisation des qualificateurs dans les noms de fichiers et de dossiers est décrite dans [Personnaliser vos ressources pour la langue, l’échelle et d’autres qualificateurs](tailor-resources-lang-scale-contrast.md). Un fichier ou un dossier peut contenir un nom de langue, mais aucune ressource qualifiée pour la langue par défaut exacte n’est détectée. Par exemple, si un projet utilise « en-US » comme langue par défaut et qu’il inclut un fichier nommé « de/logo.png », mais aucun fichier marqué avec la langue par défaut « en-US », cet avertissement s’affiche. Pour supprimer cet avertissement, les fichiers ou les ressources de chaîne doivent être qualifiés avec la langue par défaut, ou la langue par défaut doit être modifiée. Pour modifier la langue par défaut, dans votre solution ouverte dans Visual Studio, ouvrez `Package.appxmanifest`. Sous l’onglet Application, vérifiez que la langue par défaut est correctement définie (par exemple, « en » ou « en-US »).

### <a name="no-default-or-neutral-resource-given-for-resource-identifier-the-application-may-throw-an-exception-for-certain-user-configurations-when-retrieving-the-resources"></a>Aucun par défaut ou la ressource neutre donnée pour '<resource identifier>'. L’application peut lever une exception pour certaines configurations de l’utilisateur lors de la récupération des ressources.

Cet avertissement s’affiche lorsque MakePri.exe ou MSBuild découvre des fichiers ou des ressources qui semblent être marquée avec les qualificateurs de langage pour lequel les ressources ne sont pas claires. Il existe des qualificateurs, mais il n’est pas certain qu’une ressource potentielle spécifique puisse être renvoyée pour cet identificateur de ressource lors de l’exécution. Cet avertissement s’affiche s’il n’est pas possible de trouver une ressource potentielle pour une langue ou un lieu de résidence spécifique, ou un autre qualificateur qui est une ressource par défaut ou qui correspondra toujours au contexte d’un utilisateur. Lors de l’exécution, pour des configurations utilisateur particulières telles que les préférences de langue ou le lieu de résidence d’un utilisateur (**Paramètres** > **Heure et langue** > **Région et langue**), les API utilisées pour récupérer la ressource peuvent lever une exception inattendue. Pour supprimer cet avertissement, des ressources par défaut doivent être fournies, par exemple, une ressource dans la langue par défaut ou le lieu de résidence global du projet (homeregion-001).

## <a name="using-makepriexe-in-a-build-system"></a>Utilisation de MakePri.exe dans un système de génération

Les systèmes de génération doivent utiliser la commande `new`, `versioned` ou `resourcepack` de MakePri.exe, selon le type de projet créé. Les systèmes de génération qui créent un nouveau fichier PRI doivent utiliser la commande `new`. Les systèmes de génération qui doivent garantir la compatibilité des décalages internes par le biais d’itérations peuvent utiliser la commande `versioned`. Les systèmes de génération qui doivent créer un fichier PRI contenant d’autres variantes de ressources, avec une validation pour garantir qu’aucune ressource n’est ajoutée pour cette variante, doivent utiliser la commande `resourcepack`.

Les systèmes de génération qui requièrent le contrôle explicite des fichiers sources qui sont indexés peuvent utiliser l’indexeur ResFiles au lieu d’indexer un dossier. Les systèmes de génération peuvent également utiliser plusieurs passes d’index avec différents [indexeurs spécifiques au format](makepri-exe-format-specific-indexers.md) pour générer un fichier PRI unique.

En outre, les systèmes de génération peuvent utiliser l’indexeur spécifique au format PRI pour ajouter des fichiers PRI prédéfinis dans le PRI pour le package à partir d’autres composants, notamment des bibliothèques de classes, des assemblys, des kits de développement logiciel (SDK) et des DLL.

Lorsque les fichiers PRI sont générés pour d’autres composants, bibliothèques de classes, assemblys, DLL et kits de développement logiciel (SDK), la configuration **initialPath** doit être utilisée pour s’assurer que les ressources du composant ont leurs propres sous-mappages de ressources et que ces derniers ne sont pas en conflit avec l’application dans laquelle elles sont incluses.

## <a name="related-topics"></a>Rubriques connexes
* [Options de ligne de commande MakePri.exe](makepri-exe-command-options.md)
* [Configuration de MakePri.exe](makepri-exe-configuration.md)
* [Indexeurs de spécifiques au format MakePri.exe](makepri-exe-format-specific-indexers.md)
* [Adapter vos ressources de langue, mise à l’échelle et autres qualificateurs](tailor-resources-lang-scale-contrast.md)
