---
author: Jwmsft
Description: Modèle comportant une zone de contenu et une zone de commande destiné aux applications à affichage unique ou aux expériences modales, telles que les éditeurs/visionneuses de photos, les visionneuses de documents, les applications cartographiques, les applications de dessin ou les autres applications qui utilisent un affichage à défilement libre.
title: Recommandations concernant le modèle de Canvas actif
ms.assetid: 4D768472-64D6-406C-9E87-F750F6B981A0
label: TBD
template: detail.hbs
---
#Modèle de Canvas actif

Un Canvas actif est un modèle avec une zone de contenu et une zone de commande. Il est destiné aux applications à affichage unique ou aux expériences modales, telles que les éditeurs/visionneuses de photos, les visionneuses de documents, les applications cartographiques, les applications de dessin ou les autres applications qui utilisent un affichage à défilement libre. Pour entreprendre des actions, un Canvas actif peut être associé à une barre de commandes ou simplement à des boutons, selon le nombre et le type d’action dont vous avez besoin.

## Exemples

Cette conception d’application de retouche photo présente un modèle de Canvas actif, avec un exemple mobile à gauche et un exemple de bureau à droite. La surface de retouche d’image est un Canvas et la barre de commandes située en bas de l’écran contient toutes les actions contextuelles pour l’application.

![Exemple d’éditeur de photos utilisant un modèle de Canvas actif](images/uap-photo-pc-phone-700.png)

Cette conception d’une application de carte métro utilise un Canvas actif doté d’une bande d’interface utilisateur simple dans la partie supérieure, comprenant seulement deux actions et une zone de recherche. Les actions contextuelles sont affichées dans le menu contextuel, comme illustré sur l’image de droite.

![Exemple d’application cartographique utilisant un modèle de Canvas actif](images/uap-subway-pc-phone-700.png)


## Implémentation de ce modèle

Un modèle de Canvas actif comporte une zone de contenu et une zone de commande.

**Zone de contenu.**  La zone de contenu est généralement un Canvas à défilement libre. Plusieurs zones de contenu peuvent exister dans une application.

**Zone de commande.**  Si vous placez un grand nombre de commandes, une barre de commandes, qui répond en fonction de la taille d’écran peut s’avérer utile. Si vous ne placez pas un nombre important de commandes et que vous ne cherchez pas nécessairement à obtenir une interface utilisateur réactive, les boutons compacts fonctionnent parfaitement bien.



## Articles connexes

-   [**Barre de l’application et barre de commandes**](app-bars.md)


<!--HONumber=May16_HO2-->


