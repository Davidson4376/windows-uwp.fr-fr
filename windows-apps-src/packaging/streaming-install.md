---
author: laurenhughes
ms.assetid: df4d227c-21f9-4f99-9e95-3305b149d9c5
title: Installation en continu d’une application UWP
description: Le mode d’installation en continu des applications de la plateforme Windows universelle (UWP) vous permet de spécifier les parties de votre application que vous souhaitez voir téléchargées en premier par le MicrosoftStore. Lorsque les fichiers essentiels de l’application sont téléchargés en priorité, l’utilisateur peut lancer l’application et interagir avec elle pendant que le reste du téléchargement s’achève en arrière-plan.
ms.author: lahugh
ms.date: 04/05/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: installer des application uwp diffusion en continu Windows 10, uwp, installer, de diffusion en continu
ms.localizationpriority: medium
ms.openlocfilehash: 087226cad4bcf7ea0294d8878564c345d6cfb9d0
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/13/2018
ms.locfileid: "305072"
---
# <a name="uwp-app-streaming-install"></a>Installation en continu d’une application UWP
Le mode d’installation en continu des applications de la plateforme Windows universelle (UWP) vous permet de spécifier les parties de votre application que vous souhaitez voir téléchargées en premier par le MicrosoftStore. Lorsque les fichiers essentiels de l’application sont téléchargés en priorité, l’utilisateur peut lancer l’application et interagir avec elle pendant que le reste du téléchargement s’achève en arrière-plan. 

Pour utiliser UWP application diffusion en continu installer, vous devrez diviser des fichiers de votre application en sections. Pour ce faire, vous allez créer un mappage de groupe de contenu, qui est un fichier XML qui est fourni avec votre application, vous permettant de définir la priorité téléchargement et l’ordre. Consultez la rubrique liée ci-dessous pour plus d’informations.

Pour des instructions complètes sur l’ajout de UWP application diffusion en continu installer à votre application UWP, consultez cette [série de blogs](https://blogs.msdn.microsoft.com/appinstaller/2017/03/15/uwp-streaming-app-installation/).

| Rubrique | Description | 
|-------|-------------|
| [Créer et convertir un mappage de groupe de contenu source](create-cgm.md) | Pour préparer votre application de plateforme Windows universelle (UWP) pour l’installation en continu d’une application UWP, vous devez créer un mappage de groupe de contenu. Cet article vous guidera dans les spécificités de la création et de la conversion d’un mappage de groupe de contenu tout en vous fournissant des conseils et astuces tout au long du processus. |