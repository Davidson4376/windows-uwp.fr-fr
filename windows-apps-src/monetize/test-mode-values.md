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
ms.openlocfilehash: ec5b1c1723a7f58b20234d703fc786647268a3b3
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="test-mode-values"></a>Valeurs du mode test

Quand vous utilisez une classe [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) ou [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) pour afficher des publicités dans votre application, vous devez spécifier un ID d’application et un ID d’unité publicitaire. Lors du développement de votre application, utilisez les valeurs de test d’ID d’application et d’ID d’unité publicitaire de cet article pour voir comment votre application restitue les publicités au cours du test.

Si vous essayez d’utiliser des valeurs de test dans votre application après l’avoir publiée, votre application dynamique ne recevra pas de publicités. Pour recevoir des publicités dans votre application publiée, vous devez mettre à jour votre code pour utiliser un ID de l’application et un ID d’unité publicitaire fournis par le tableau de bord du Centre de développement Windows. Pour plus d’informations, voir [Configurer des unités publicitaires dans votre application](set-up-ad-units-in-your-app.md).
 
Voici les valeurs de test à utiliser pour les bannières et les spots publicitaires.

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
    <th align="left">AppId</th>
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
    <th align="left">AppId</th>
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


> **Important**&nbsp;&nbsp;La taille d’une publicité dynamique est définie par les propriétés **Width** et **Height** du **AdControl**. Pour obtenir de meilleurs résultats, vérifiez que les propriétés **Width** et **Height** de votre code font partie des [tailles de bannières publicitaires prises en charge](supported-ad-sizes-for-banner-ads.md). Les propriétés **Width** et **Height** ne changent pas en fonction de la taille d’une publicité dynamique.


 

 
