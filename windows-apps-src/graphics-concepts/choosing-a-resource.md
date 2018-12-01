---
title: Choix d’une ressource
description: Une ressource est une collection de données utilisées par le pipeline 3D.
ms.assetid: 6BAD6287-2930-42F8-BF51-69A379D1D2C3
keywords:
- Choix d’une ressource
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: ccc99395dba2f2d1894db81fb48abb59f9a8ba4f
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/30/2018
ms.locfileid: "8335619"
---
# <a name="choosing-a-resource"></a>Choix d’une ressource


Une ressource est une collection de données utilisées par le pipeline 3D. La création des ressources et la définition de leur comportement constituent la première étape de programmation de votre application. Ce guide aborde les rubriques de base permettant de choisir les ressources dont a besoin votre application.

## <a name="span-ididentifybindingspanspan-ididentifybindingspanspan-ididentifybindingspanidentify-pipeline-stages-that-need-resources"></a><span id="Identify_Binding"></span><span id="identify_binding"></span><span id="IDENTIFY_BINDING"></span>Identifier les étapes du pipeline qui ont besoin de ressources


La première étape consiste à choisir la ou les étapes du [pipeline graphique](graphics-pipeline.md) qui utilisent une ressource. Il s'agit autrement dit d'identifier chaque étape qui lit des données à partir d’une ressource, ainsi que les étapes qui écriront les données dans une ressource. Connaître les étapes du pipeline où les ressources seront utilisées permet de savoir quelles API seront appelées pour établir une liaison entre la ressource et l’étape.

Le tableau suivant répertorie les types de ressources pouvant être liées à chaque étape du pipeline. Il indique si la ressource peut être liée en tant qu’entrée ou sortie.

| Étape du pipeline  | Entrée/Sortie | Ressource               | Type de ressource                           |
|-----------------|--------|------------------------|-----------------------------------------|
| Assembleur d’entrée | Entrée     | Tampon de vertex          | Mémoire tampon                                  |
| Assembleur d’entrée | Entrée     | Tampon d’index           | Mémoire tampon                                  |
| Étapes du nuanceur   | Entrée     | Shader-ResourceView    | Mémoire tampon, Texture1D, Texture2D, Texture3D |
| Étapes du nuanceur   | Entrée     | Mémoire tampon constante du nuanceur | Mémoire tampon                                  |
| Sortie de flux   | Sortie    | Mémoire tampon                 | Mémoire tampon                                  |
| Fusion de sortie   | Sortie    | Affichage de la cible de rendu     | Mémoire tampon, Texture1D, Texture2D, Texture3D |
| Fusion de sortie   | Sortie    | Affichage du gabarit de profondeur     | Texture1D, Texture2D                    |

 

## <a name="span-ididentifyusagespanspan-ididentifyusagespanspan-ididentifyusagespanidentify-how-each-resource-will-be-used"></a><span id="Identify_Usage"></span><span id="identify_usage"></span><span id="IDENTIFY_USAGE"></span>Identifier l’utilisation de chaque ressource


Une fois que vous avez choisi les étapes du pipeline que votre application utilisera (et par conséquent, les ressources requises pour chaque étape), vous devez déterminer comment chaque ressource sera utilisée, autrement dit, spécifier si une ressource sera accessible au processeur graphique ou à l'UC.

Le matériel sur lequel s'exécute votre application comporte un minimum un processeur graphique et une unité centrale. Pour sélectionner une valeur d’utilisation, utilisez les options suivantes pour déterminer le type de processeur qui devra lire ou écrire des données dans la ressource.

| Utilisation de la ressource | Mise à jour par                    | Fréquence de mise à jour |
|----------------|--------------------------------------|---------------------|
| Valeur par défaut        | Processeur graphique                                  | rarement        |
| Dynamique        | UC                                  | fréquemment          |
| Transfert        | Processeur graphique                                  | Non applicable                 |
| Immuable      | UC (uniquement au moment de la création de ressources) | Non applicable                 |

 

Le mode d’utilisation par défaut doit être utilisé pour une ressource qui ne doit être que rarement mise à jour par l'UC (moins d'une fois par trame). Dans l’idéal, l'UC n'écrit jamais directement dans une ressource associée à une utilisation par défaut, ce afin d’éviter tout risque de dégradation des performances.

L'utilisation dynamique doit être utilisée pour une ressource qui sera assez souvent mise à jour par l'UC (une ou plusieurs fois par trame). Le scénario classique pour une ressource dynamique serait de créer des mémoires tampons de vertex et d’index dynamiques, qui seraient remplies au moment de l’exécution avec des données sur la géométrie visible du point de vue de l’utilisateur pour chaque trame. Ces mémoires tampons permettraient d'afficher uniquement la géométrie visible par l’utilisateur pour cette trame.

L'utilisation de transfert doit être utilisée pour copier des données vers et à partir d’autres ressources. Un scénario classique consisterait à copier des données d'une ressource associée à une utilisation par défaut (à laquelle l'UC ne peut pas accéder) dans une ressource associée à une utilisation de transfert (accessible à l'UC).

Les ressources immuables doivent être utilisées lorsque les données de la ressource ne changent pas.

Une autre approche consiste à considérer la manière dont une application traite une ressource.

| Comment l'application utilise la ressource     | Utilisation de la ressource       |
|---------------------------------------|----------------------|
| Charger une seule fois et ne jamais mettre à jour            | Immuable ou par défaut |
| L'application remplit les ressources à plusieurs reprises | Dynamique              |
| Rendu de texture                     | Valeur par défaut              |
| Accès des données du processeur graphique par l'UC                | Transfert              |

 

Si vous ne savez pas quelle utilisation choisir, commencez par l’utilisation par défaut, qui correspond au scénario le plus courant. Une mémoire tampon constante de nuanceur est un type de ressource qui doit toujours être associé à une utilisation par défaut.

## <a name="span-idresourcetypesandpipelinestagesspanspan-idresourcetypesandpipelinestagesspanspan-idresourcetypesandpipelinestagesspanbinding-resources-to-pipeline-stages"></a><span id="Resource_Types_and_Pipeline_stages"></span><span id="resource_types_and_pipeline_stages"></span><span id="RESOURCE_TYPES_AND_PIPELINE_STAGES"></span>Liaison de ressources aux étapes du pipeline


Une ressource peut être liée à plusieurs étapes du pipeline simultanément tant que les restrictions spécifiées lors de la création de la ressource sont respectées. Ces restrictions sont spécifiées en tant qu'indicateurs d’utilisation, indicateurs de liaison ou indicateurs d'accès UC. Plus spécifiquement, une ressource peut être simultanément liée comme entrée et comme sortie tant que la lecture et l’écriture d'une partie d’une ressource ne peuvent pas se produire en même temps.

Lors de la liaison d'une ressource, considérez la manière dont celle-ci sera accessible au processeur graphique et à l'UC. Les ressources conçues pour un seul objet (qui n'utilisent pas plusieurs indicateurs d'utilisation, de liaison et d'accès UC) produisent généralement de meilleures performances.

Considérez, par exemple, le cas d’une cible de rendu utilisée plusieurs fois comme texture. Il peut être plus rapide d’avoir deux ressources: une cible de rendu et une texture utilisée comme ressource de nuanceur. Chaque ressource utiliserait dans ce cas un seul indicateur de liaison, spécifiant «cible de rendu» ou «ressource de nuanceur». Les données sont copiées de la texture cible de rendu vers la texture de nuanceur.

La technique employée dans cet exemple peut améliorer les performances en isolant l’opération d'écriture de la cible de rendu de l'opération de lecture de la texture de nuanceur. L'idéal est de mettre en œuvre les deux approches et d'évaluer les différences de performances dans votre application.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Ressources](resources.md)

 

 




