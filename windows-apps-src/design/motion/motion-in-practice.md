---
author: jwmsft
Description: Learn how Fluent motion fundamentals come together in your app.
title: Mouvement en pratique - Animation dans les applications UWP
label: Motion in practice
template: detail.hbs
ms.author: jimwalk
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
pm-contact: stmoy
design-contact: jeffarn
doc-status: Draft
ms.localizationpriority: medium
ms.openlocfilehash: 87a17d3f73887c9b1b5029e2096c5b41c9444c4e
ms.sourcegitcommit: 517c83baffd344d4c705bc644d7c6d2b1a4c7e1a
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/07/2018
ms.locfileid: "1843787"
---
# <a name="bringing-it-together"></a>Synthèse

> [!IMPORTANT]
> Cet article décrit des fonctionnalités qui n’ont pas encore été publiées et sont susceptibles d’être considérablement modifiées d’ici leur commercialisation. Microsoft ne donne aucune garantie, expresse ou implicite, concernant les informations fournies ici.

Le minutage, l'accélération, la direction et la gravité fonctionnent ensemble pour former la base du mouvement Fluent. Chacun doit être pris en compte par rapport aux autres et correctement appliqué dans le contexte de votre application.

Voici 3manières d’appliquer les principes de base du mouvement Fluent dans votre application.

:::row::: :::column::: **Animation implicite**

        Automatic tween and timing between values in a parameter change to achieve very simple Fluent motion using the standardized values.
    :::column-end:::
    :::column:::
        **Built-in animation**

        System components, such as common controls and shared motion, are "Fluent by default". Fundamentals have been applied in a manner consistent with their implied usage.
    :::column-end:::
    :::column:::
        **Custom animation following guidance recommendations**

        There may be times when the system does not yet provide an exact motion solution for your scenario. In those cases, use the baseline fundamental recommendations as a starting point for your experiences.
    :::column-end:::
:::row-end:::

### **<a name="transition-example"></a>Exemple de transition**

![Animation fonctionnelle](images/pageRefresh.gif)

:::row::: :::column::: <b>Direction vers l'avant dehors:</b><br>
        Disparition en fondu: 150m; Accélération: accélérer par défaut

        <b>Direction Forward In:</b><br>
        Slide up 150px: 300ms; Easing: Default Decelerate
    :::column-end:::
    :::column:::
         <b>Direction Backward Out:</b><br>
        Slide down 150px: 150ms; Easing: Default Accelerate
      
        <b>Direction Backward In:</b><br>
        Fade in: 300ms; Easing: Default Decelerate
    :::column-end:::
:::row-end:::

 ### **<a name="object-example"></a>Exemple d'objet**

 ![Mouvement de 300ms](images/control.gif)
 
:::row::: :::column::: <b>Développement de direction:</b><br>
        Croissance: 300ms; Accélération: Standard :::column-end::: :::column::: <b>Contraction de direction:</b><br>
        Croissance: 150ms; Accélération: accélération par défaut :::column-end::: :::row-end:::

## <a name="related-articles"></a>Articles associés

- [Présentation du mouvement](index.md)
- [Minutage et accélération](timing-and-easing.md)
- [Direction et gravité](directionality-and-gravity.md)