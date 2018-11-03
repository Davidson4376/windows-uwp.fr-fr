---
author: mtoepke
title: Programmation asynchrone (DirectX et C++)
description: Cette rubrique traite des divers points à prendre en considération lorsque vous utilisez la programmation asynchrone et les threads avec DirectX.
ms.assetid: 17613cd3-1d9d-8d2f-1b8d-9f8d31faaa6b
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
keywords: Windows10, uwp, jeux, programmation asynchrone, directx
ms.localizationpriority: medium
ms.openlocfilehash: 9f395136ff7f331f58b9c8e0ac1ee9efea296c02
ms.sourcegitcommit: 144f5f127fc4fbd852f2f6780ef26054192d68fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/03/2018
ms.locfileid: "5996128"
---
# <a name="asynchronous-programming-directx-and-c"></a>Programmation asynchrone (DirectX et C++)



Cette rubrique traite des divers points à prendre en considération lorsque vous utilisez la programmation asynchrone et les threads avec DirectX.

## <a name="async-programming-and-directx"></a>Programmation asynchrone et DirectX


Que vous soyez débutant ou expert dans l’utilisation de DirectX, mettez l’ensemble de votre pipeline de traitement graphique dans un seul thread. Dans n’importe quelle scène donnée d’un jeu, il existe des ressources usuelles telles que les bitmaps, les nuanceurs et d’autres éléments qui nécessitent un accès exclusif. Vous devez synchroniser l’accès à ces ressources sur les threads parallèles. Le rendu est un processus difficile à paralléliser sur plusieurs threads.

Toutefois, si votre jeu est suffisamment complexe ou si vous cherchez à obtenir de meilleures performances, vous pouvez utiliser la programmation asynchrone pour paralléliser certains composants non spécifiques à votre pipeline de rendu. Le matériel moderne comporte des UC basées sur le multicœur ou l’hyperthreading. Par conséquent, votre application doit pouvoir tirer parti de ces fonctionnalités ! Pour ce faire, utilisez la programmation asynchrone pour certains des composants de votre jeu qui n’ont pas besoin d’accès direct au contexte de périphérique Direct3D, notamment :

-   E/S de fichiers
-   physique
-   IA
-   réseau
-   audio
-   contrôles
-   Composants d’interface utilisateur en XAML

Votre application peut gérer ces composants sur plusieurs threads simultanés. Les E/S de fichiers, en particulier le chargement des ressources, bénéficient grandement du chargement asynchrone, car votre jeu ou application peut être dans un état interactif pendant que plusieurs (voire plusieurs centaines) de mégaoctets de ressources sont en cours de chargement ou de transmission en continu. La façon la plus simple de créer et gérer ces threads est d’utiliser la [Bibliothèque de modèles parallèles](https://msdn.microsoft.com/library/dd492418.aspx) et le modèle **task**, que vous trouverez dans l’espace de noms **concurrency** défini dans PPLTasks.h. L’utilisation de la [Bibliothèque de modèles parallèles](https://msdn.microsoft.com/library/dd492418.aspx) permet de tirer parti des UC basées sur le multicœur et l’hyperthreading. En outre, elle permet d’améliorer de nombreux aspects, qu’il s’agisse des temps de chargement perçus, des défaillances ou des retards liés aux calculs intensifs de l’UC ou au réseau.

> **Remarque**  application dans une plateforme Windows universelle (UWP), l’interface utilisateur s’exécute entièrement dans un single-threaded apartment (STA). Si vous créez une interface utilisateur pour votre jeu DirectX à l’aide de l’[interopérabilité XAML](directx-and-xaml-interop.md), vous ne pouvez accéder aux contrôles qu’à l’aide du thread unique cloisonné (STA).

 

## <a name="multithreading-with-direct3d-devices"></a>Multithreading avec des périphériques Direct3D


Le multithreading des contextes de périphériques n’est disponible que sur les périphériques graphiques qui prennent en charge le niveau de fonctionnalité 11_0 ou supérieur de Direct3D. Toutefois, vous pouvez être amené à maximiser l’utilisation de la puissance de l’unité de traitement graphique (GPU) sur de nombreuses plateformes, par exemple les plateformes de jeux dédiées. Dans le cas de figure le plus simple, vous pouvez séparer le rendu de la superposition d’un affichage à tête haute de la projection et du rendu de la scène 3D. Par ailleurs, ces deux composants utilisent des pipelines parallèles distincts. Pour créer et gérer les objets de ressources (textures, maillages, nuanceurs et autres ressources), les deux threads doivent utiliser le même [**ID3D11DeviceContext**](https://msdn.microsoft.com/library/windows/desktop/ff476385). Toutefois, ce dernier est à thread unique, ce qui vous oblige à implémenter un mécanisme de synchronisation (par exemple des sections critiques) pour y accéder en toute sécurité. Bien que vous puissiez créer des listes de commandes distinctes pour le contexte de périphériques sur différents threads (à des fins de rendu différé), vous ne pouvez pas lire ces listes de commandes simultanément sur la même instance de **ID3D11DeviceContext**.

Votre application peut également utiliser [**ID3D11Device**](https://msdn.microsoft.com/library/windows/desktop/ff476379), ce qui ne pose pas de problème en matière de multithreading, afin de créer des objets de ressources. Alors, pourquoi ne pas toujours utiliser **ID3D11Device** à la place de [**ID3D11DeviceContext**](https://msdn.microsoft.com/library/windows/desktop/ff476385) ? Actuellement, la prise en charge des pilotes pour le multithreading n’est pas toujours disponible pour certaines interfaces graphiques. Vous pouvez interroger l’appareil et déterminer s’il prend en charge le multithreading. Toutefois, si vous cherchez à atteindre le public le plus large possible, utilisez simplement le **ID3D11DeviceContext** à thread unique pour la gestion des objets de ressources. Néanmoins, lorsque le pilote de périphérique graphique ne prend pas en charge le multithreading ou les listes de commandes, Direct3D 11 tente de gérer l’accès synchronisé au contexte de périphérique de manière interne. Si les listes de commandes ne sont pas prises en charge, il fournit une implémentation logicielle. Par conséquent, vous pouvez écrire du code multithread qui s’exécute sur les plateformes dont les interfaces graphiques n’ont pas de pilotes prenant en charge l’accès au contexte de périphérique à l’aide du multithreading.

Si votre application prend en charge les threads distincts pour le traitement des listes de commandes et l’affichage des trames, vous pouvez maintenir l’unité de traitement graphique (GPU) active et permettre le traitement des listes de commandes tout en affichant les trames en temps voulu, sans saccade ou décalage perceptible. Dans ce cas, vous pouvez utiliser un [**ID3D11DeviceContext**](https://msdn.microsoft.com/library/windows/desktop/ff476385) distinct pour chaque thread. Par ailleurs, vous pouvez partager les ressources (par exemple des textures) en les créant avec l’indicateur D3D11\_RESOURCE\_MISC\_SHARED. Dans ce scénario, [**ID3D11DeviceContext::Flush**](https://msdn.microsoft.com/library/windows/desktop/ff476425) doit être appelé sur le thread de traitement pour terminer l’exécution de la liste de commandes avant d’afficher les résultats du traitement de l’objet de ressource dans le thread d’affichage.

## <a name="deferred-rendering"></a>Rendu différé


Le rendu différé enregistre les commandes graphiques dans une liste de commandes afin qu’elles puissent être lues à un autre moment. Cette fonctionnalité est conçue pour la prise en charge du rendu d’un thread unique pendant l’enregistrement des commandes de rendu des threads supplémentaires. Une fois ces commandes terminées, elles peuvent être exécutées sur le thread qui génère l’objet d’affichage final (tampon de trame, texture ou toute autre sortie graphique).

Créez un contexte différé à l’aide de [**ID3D11Device::CreateDeferredContext**](https://msdn.microsoft.com/library/windows/desktop/ff476505) (au lieu de [**D3D11CreateDevice**](https://msdn.microsoft.com/library/windows/desktop/ff476082) ou [**D3D11CreateDeviceAndSwapChain**](https://msdn.microsoft.com/library/windows/desktop/ff476083), qui permettent de créer un contexte immédiat). Pour plus d’informations, voir [Rendu immédiat et différé](https://msdn.microsoft.com/library/windows/desktop/ff476892).

## <a name="related-topics"></a>Rubriques connexes


* [Introduction au multithreading dans Direct3D11](https://msdn.microsoft.com/library/windows/desktop/ff476891)

 

 




