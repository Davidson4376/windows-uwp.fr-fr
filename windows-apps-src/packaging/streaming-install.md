---
ms.assetid: df4d227c-21f9-4f99-9e95-3305b149d9c5
title: Installation en continu d’une application UWP
description: Le mode d’installation en continu des applications de la plateforme Windows universelle (UWP) vous permet de spécifier les parties de votre application que vous souhaitez voir téléchargées en premier par le Microsoft Store. Lorsque les fichiers essentiels de l’application sont téléchargés en priorité, l’utilisateur peut lancer l’application et interagir avec elle pendant que le reste du téléchargement s’achève en arrière-plan.
ms.date: 04/05/2017
ms.topic: article
keywords: windows 10, uwp, installation en continu, installation en continu d’une application uwp
ms.localizationpriority: medium
ms.openlocfilehash: 3fa33410be31b1732a04c51d14dbbd114e1f5e0c
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57608654"
---
# <a name="uwp-app-streaming-install"></a>Installation en continu d’une application UWP
Le mode d’installation en continu des applications de la plateforme Windows universelle (UWP) vous permet de spécifier les parties de votre application que vous souhaitez voir téléchargées en premier par le Microsoft Store. Lorsque les fichiers essentiels de l’application sont téléchargés en priorité, l’utilisateur peut lancer l’application et interagir avec elle pendant que le reste du téléchargement s’achève en arrière-plan. 

Pour permettre l’installation en continu d’une application UWP, vous devez diviser les fichiers qui la constituent en sections. Pour ce faire, vous devez créer un mappage du groupe de contenu, c’est-à-dire un fichier XML qui sera fourni avec votre application, permettant de définir la priorité et l’ordre de téléchargement. Pour plus d’informations, consultez la rubrique liée ci-dessous.

Vous trouverez un guide complet sur l’ajout de la fonction d’installation en continu à votre application UWP dans cette [série de blog](https://blogs.msdn.microsoft.com/appinstaller/2017/03/15/uwp-streaming-app-installation/).

| Rubrique | Description | 
|-------|-------------|
| [Créer et de convertir un mappage de groupe de contenu source](create-cgm.md) | Pour préparer votre application de plateforme Windows universelle (UWP) pour l’installation en continu d’une application UWP, vous devez créer un mappage de groupe de contenu. Cet article vous guidera dans les spécificités de la création et la conversion d’un mappage de groupe de contenu tout en vous fournissant des conseils et astuces tout au long du processus. |