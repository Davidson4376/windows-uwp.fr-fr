---
author: mcleanbyron
ms.assetid: 2ed21281-f996-402d-a968-d1320a4691df
description: "Utilisez les valeurs de test d’ID d’application et d’ID d’unité publicitaire pour voir comment votre application restitue les publicités au cours du test."
title: Valeurs du mode test
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, publicités, publicité, test"
ms.openlocfilehash: 0c3e713d9a2bb7c10bda0d9517f5cb882d5e2e57
ms.sourcegitcommit: 6b772d2a224f8a9c557dc517c6ec0592545e9a43
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/02/2017
---
# <a name="test-mode-values"></a>Valeurs du mode test

Lorsque vous utilisez une classe [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx), [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) ou [NativeAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.nativead.aspx) pour afficher des publicités dans votre application, vous devez spécifier un ID d’application et un ID d’unité publicitaire dans les propriétés **AdUnitId** et **ApplicationId**. Lors du développement de votre application, utilisez les valeurs de test d’ID d’application et d’ID d’unité publicitaire de cet article pour voir comment votre application restitue les publicités au cours du test.

Si vous essayez d’utiliser des valeurs de test dans votre application après l’avoir publiée, votre application dynamique ne recevra pas de publicités. Pour recevoir des publicités dans votre application publiée, vous devez mettre à jour votre code pour utiliser un ID de l’application et un ID d’unité publicitaire fournis par le tableau de bord du Centre de développement Windows. Pour plus d’informations, voir [Configurer des unités publicitaires dans votre application](set-up-ad-units-in-your-app.md).
 
Voici les valeurs de test à utiliser pour les différents types de publicité.

* Pour les spots:

    <table>
    <colgroup>
    <col width="33%" />
    <col width="33%" />
    <col width="33%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th align="left">SE cible</th>
    <th align="left">AdUnitId</th>
    <th align="left">ApplicationId</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td align="left"><p>UWP (Windows 10)</p></td>
    <td align="left"><p>test</p></td>
    <td align="left"><p>d25517cb-12d4-4699-8bdc-52040c712cab</p></td>
    </tr>
    <tr class="odd">
    <td align="left"><p>Windows 8.x et Windows Phone 8.x</p></td>
    <td align="left"><p>11389925</p></td>
    <td align="left"><p>d25517cb-12d4-4699-8bdc-52040c712cab</p></td>
    </tr>
    </tbody>
    </table>

     
* Pour les bannières publicitaires:

    <table>
    <colgroup>
    <col width="33%" />
    <col width="33%" />
    <col width="33%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th align="left">SE cible</th>
    <th align="left">AdUnitId</th>
    <th align="left">ApplicationId</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td align="left"><p>UWP (Windows 10)</p></td>
    <td align="left"><p>test</p></td>
    <td align="left"><p>3f83fe91-d6be-434d-a0ae-7351c5a997f1</p></td>
    </tr>
    <tr class="even">
    <td align="left"><p>Windows 8.x et Windows Phone 8.x</p></td>
    <td align="left"><p>10865270</p></td>
    <td align="left"><p>3f83fe91-d6be-434d-a0ae-7351c5a997f1</p></td>
    </tr>
    </tbody>
    </table>

* Pour les publicités natives:

    <table>
    <col width="33%" />
    <col width="33%" />
    <col width="33%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th align="left">SE cible</th>
    <th align="left">AdUnitId</th>
    <th align="left">ApplicationId</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td align="left"><p>UWP (Windows 10)</p></td>
    <td align="left"><p>test</p></td>
    <td align="left"><p>d25517cb-12d4-4699-8bdc-52040c712cab</p></td>
    </tbody>
    </table>

> [!IMPORTANT]
> Pour un **AdControl**, la taille d’une publicité dynamique est définie par les propriétés **Width** (largeur) et **Height** (hauteur). Pour obtenir de meilleurs résultats, vérifiez que les propriétés **Width** et **Height** de votre code font partie des [tailles de bannières publicitaires prises en charge](supported-ad-sizes-for-banner-ads.md). Les propriétés **Width** et **Height** ne changent pas en fonction de la taille d’une publicité dynamique.


 

 
