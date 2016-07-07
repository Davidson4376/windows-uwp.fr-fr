---
author: payzer
title: "Comment désactiver le balayage"
description: 
area: Xbox
ms.sourcegitcommit: 32a875348debac9aec9f5a26bc4e7e0af2a0a5b4
ms.openlocfilehash: abd06e78364ff32cc10d733e33b153b854dbc467

---

# Comment étirer l’IU vers le bord de l’écran   
Par défaut, des bordures sont placées sur les angles de la fenêtre d’affichage de l’application. Elles permettent de tenir compte de la zone adaptée à l’écran de TV. Pour en savoir plus, voir [Conception pour Xbox et télévision](http://go.microsoft.com/fwlink/?LinkID=760736#tv-safe-area).  Nous vous recommandons de désactiver cette fonctionnalité et d’étirer l’IU vers le bord de l’écran. Pour ce faire, ajoutez le code suivant lorsque l’application démarre:
   
`Windows.UI.ViewManagement.ApplicationView.GetForCurrentView().SetDesiredBoundsMode(Windows.UI.ViewManagement.ApplicationViewBoundsMode.UseCoreWindow);`
   
Remarque: les applications C++/DirectX n’ont pas besoin de prendre ce facteur en compte. Le système affiche toujours votre application au niveau du bord de l’écran.



<!--HONumber=Jun16_HO5-->


