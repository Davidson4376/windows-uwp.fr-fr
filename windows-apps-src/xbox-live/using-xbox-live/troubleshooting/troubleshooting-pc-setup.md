---
title: Dépannage de l’installation de Xbox Live sur les PC Windows
description: Découvrez comment résoudre les problèmes de votre environnement de développement Xbox Live sur un PC Windows.
ms.assetid: 9cfebdcd-0c1c-4fc2-9457-e7e434b6374c
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, jeux, uwp, windows 10, xbox une, résoudre les problèmes
ms.localizationpriority: medium
ms.openlocfilehash: c1f055a49fe34be35335e50dc8b1efbfb7b9b922
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57647734"
---
# <a name="troubleshooting-xbox-live-setup-on-windows-pc"></a>Dépannage de l’installation de Xbox Live sur les PC Windows

Sur le PC Windows 10, vous pouvez vous assurer que votre ordinateur est configuré correctement avec ces étapes :

1. Modifier votre ordinateur pour pointer vers le bac à sable XDKS.1 où les exemples sont conçus pour s’exécuter.  Cela en exécutant ce script :

        {*SDK source root*}\Tools\SwitchSandbox.cmd XDKS.1

1. Extrayez le contenu du fichier zip « SourcesAndSamples.zip » ont été trouvés dans le Kit de développement.
1. Ouvrir un exemple de solution :
    1. Pour les API C++ : {*racine source de kit de développement logiciel*} \Samples\Social\UWP\Cpp\Social.Cpp.140.sln
    1. Pour les API WinRT avec C#: {*racine source de kit de développement logiciel*} \Samples\Social\UWP\CSharp\Social.CSharp.140.sln
    1. Pour les API WinRT avec C / c++ / CX : {*racine source de kit de développement logiciel*} \Samples\TitleStorage\UWP\CppCx\TitleStorageUniversal.sln
1. Modifier la plateforme cible de build à « Win32 » ou « x64 ».
1. Cliquez avec le bouton droit sur la solution et reconstruisez tous les éléments.
1. Lancez l’application dans le débogueur.
1. Connectez-vous avec le compte de développement que vous avez créé sur le [portail des développeurs Xbox](https://xdp.xboxlive.com), ou avec un compte de développeur de vente au détail autorisé dans [partenaires](https://partner.microsoft.com/dashboard).
1. Accorder l’autorisation d’accéder à vos informations Xbox Live.
1. Vérifiez que l’application peut récupérer vos informations et vous pouvez voir votre identité.