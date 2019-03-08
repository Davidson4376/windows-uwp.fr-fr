---
title: Configurer votre AppXManifest pour le mode multijoueur
description: Découvrez comment configurer votre AppXManifest UWP pour activer les invitations de multijoueur Xbox Live.
ms.assetid: 72f179e7-4705-4161-9b8a-4d6a1a05b8f7
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, jeux, uwp, windows 10, xbox une, l’activation de protocole, mode multijoueur
ms.localizationpriority: medium
ms.openlocfilehash: 13b04a86fdc4e4f661dd1c181dda7d9c9e4c1c8a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57646024"
---
# <a name="configure-your-appxmanifest-for-multiplayer"></a>Configurer votre AppXManifest pour le mode multijoueur

Vous devez effectuer certaines mises à jour dans le fichier .appxmanifest dans votre projet Visual Studio si les conditions suivantes sont remplies :
- Vous développez une UWP
- Vous souhaitez implémenter la possibilité pour les joueurs à inviter d’autres utilisateurs à votre titre

Si vous ne le faites pas cette étape, votre titre n’obtiendrez pas de protocole activé lorsqu’un lecteur destinataire accepte une invitation à lire.

## <a name="open-your-packageappxmanifest"></a>Ouvrez votre fichier Package.appxmanifest

Votre fichier Package.appxmanifest se trouve généralement dans le même répertoire que le fichier solution du projet Visual Studio.  Ou vous pouvez le trouver dans l’Explorateur de solutions.

![](../../images/multiplayer/multiplayer_open_appxmanifest.png)

## <a name="add-new-entry"></a>Ajouter une nouvelle entrée

Vous devez ajouter le code suivant à la ```<Extensions>``` élément sous ```<Applications>``` dans votre fichier Package.appxmanifest

```
<Extensions>
  <uap:Extension Category="windows.protocol">
    <uap:Protocol Name="ms-xbl-multiplayer" />
  </uap:Extension>
</Extensions>
```

Par exemple :

![](../../images/multiplayer/multiplayer_appxmanifest_changes.png)

Enregistrez et régénérez votre titre.  Pour savoir comment utiliser le Gestionnaire de mode multijoueur pour implémenter la possibilité d’inviter d’autres joueurs pour votre titre, veuillez consultez [lire le mode multijoueur avec des amis](../multiplayer-manager/play-multiplayer-with-friends.md)
