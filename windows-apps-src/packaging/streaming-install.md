---
ms.assetid: df4d227c-21f9-4f99-9e95-3305b149d9c5
title: Installation en continu d’une application UWP
description: Le mode d’installation en continu des applications de la plateforme Windows universelle (UWP) vous permet de spécifier les parties de votre application que vous souhaitez voir téléchargées en premier par le MicrosoftStore. Lorsque les fichiers essentiels de l’application sont téléchargés en priorité, l’utilisateur peut lancer l’application et interagir avec elle pendant que le reste du téléchargement s’achève en arrière-plan.
ms.date: 04/05/2017
ms.topic: article
keywords: installation d’une application uwp de diffusion en continu Windows 10, uwp, l’installation, en continu
ms.localizationpriority: medium
ms.openlocfilehash: 3fa33410be31b1732a04c51d14dbbd114e1f5e0c
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/03/2018
ms.locfileid: "8466979"
---
# <a name="uwp-app-streaming-install"></a>Installation en continu d’une application UWP
Le mode d’installation en continu des applications de la plateforme Windows universelle (UWP) vous permet de spécifier les parties de votre application que vous souhaitez voir téléchargées en premier par le MicrosoftStore. Lorsque les fichiers essentiels de l’application sont téléchargés en priorité, l’utilisateur peut lancer l’application et interagir avec elle pendant que le reste du téléchargement s’achève en arrière-plan. 

Pour utiliser l’installation de diffusion en continu de l’application UWP vous aurez besoin pour répartir les fichiers de votre application en sections. Pour ce faire, vous allez créer un mappage de groupe de contenu, qui est un fichier XML qui est fourni avec votre application, vous permettant de définir la priorité téléchargement et l’ordre. Consultez la rubrique figurent ci-dessous pour plus d’informations.

Pour obtenir un guide complet sur l’ajout de diffusion en continu installation des applications UWP à votre application UWP, regardez cette [série de blog](https://blogs.msdn.microsoft.com/appinstaller/2017/03/15/uwp-streaming-app-installation/).

| Rubrique | Description | 
|-------|-------------|
| [Créer et convertir un mappage de groupe de contenu source](create-cgm.md) | Pour préparer votre application de plateforme Windows universelle (UWP) pour l’installation en continu d’une application UWP, vous devez créer un mappage de groupe de contenu. Cet article vous guidera dans les spécificités de la création et de la conversion d’un mappage de groupe de contenu tout en vous fournissant des conseils et astuces tout au long du processus. |