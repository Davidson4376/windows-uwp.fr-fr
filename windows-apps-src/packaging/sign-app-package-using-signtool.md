---
author: laurenhughes
title: Signer un package d'application à l'aide de SignTool
description: Utilisez SignTool pour signer manuellement un package d'application à l'aide d'un certificat.
ms.author: lahugh
ms.date: 09/30/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.assetid: 171f332d-2a54-4c68-8aa0-52975d975fb1
ms.localizationpriority: medium
ms.openlocfilehash: c238855f4f018e8e3142509842221c6b9d97fae3
ms.sourcegitcommit: 933e71a31989f8063b020746fdd16e9da94a44c4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/11/2018
ms.locfileid: "4532032"
---
# <a name="sign-an-app-package-using-signtool"></a>Signer un package d'application à l'aide de SignTool


**SignTool** est un outil de ligne de commande utilisé pour signer numériquement un package d’application ou un ensemble d’applications à l'aide d'un certificat. Le certificat peut être créé par l’utilisateur (à des fins de test) ou émis par une société (à des fins de distribution). Le fait de signer un package d'application permet à l'utilisateur de vérifier que les données de l'application n'ont pas été modifiées après avoir été signées, mais également de confirmer l'identité du signataire, qu'il s'agisse d'un utilisateur ou d'une société qui l'a signé. **SignTool** permet de signer des packages et des ensembles d’applications cryptés ou non.

> [!IMPORTANT] 
> Si vous avez utilisé Visual Studio pour développer votre application, nous vous recommandons d’utiliser l’Assistant Visual Studio pour créer et signer votre package d’application. Pour plus d’informations, voir [Créer un package d’application UWP avec Visual Studio](https://msdn.microsoft.com/windows/uwp/packaging/packaging-uwp-apps).

Pour en savoir plus sur la signature de code et les certificats en général, voir [Introduction à la signature de code](https://msdn.microsoft.com/library/windows/desktop/aa380259.aspx#introduction_to_code_signing).

## <a name="prerequisites"></a>Prérequis
- **Une application empaquetée**  
    Pour en savoir plus sur la création manuelle d’un package d’application, voir [Créer un package d’application avec l’outil MakeAppx.exe](https://msdn.microsoft.com/windows/uwp/packaging/create-app-package-with-makeappx-tool). 

- **Un certificat de signature valide**  
    Pour plus d’informations sur la création ou l’importation d’un certificat de signature valide, voir [Créer ou importer un certificat pour la signature d'un package](https://msdn.microsoft.com/windows/uwp/packaging/create-certificate-package-signing).

- **SignTool.exe**  
    Selon votre chemin d’installation du Kit de développement logiciel (SDK), c’est là où **SignTool** se trouve sur votre PC Windows10:
    - x86: C:\Program Files (x86)\Windows Kits\10\bin\x86\SignTool.exe
    - x64: C:\Program Files (x86)\Windows Kits\10\bin\x64\SignTool.exe

## <a name="using-signtool"></a>À l’aide de SignTool

**SignTool** peut être utilisé pour signer les fichiers, vérifier les signatures ou les horodatages, supprimer des signatures etc. Dans le cadre de la signature d’un package d’application, nous allons nous concentrer sur la commande **signer**. Pour plus d’informations sur **SignTool**, voir la page de référence [SignTool](https://msdn.microsoft.com/library/windows/desktop/aa387764.aspx). 

### <a name="determine-the-hash-algorithm"></a>Déterminer l’algorithme de hachage
Lorsque vous utilisez **SignTool** pour signer votre package d’application ou un ensemble d’applications, l’algorithme de hachage utilisé dans **SignTool** doit être le même que celui que vous avez utilisé pour empaqueter votre application. Par exemple, si vous avez utilisé **MakeAppx.exe** pour créer votre package d’application avec les paramètres par défaut, vous devez spécifier SHA256lorsque vous utilisez **SignTool** puisqu’il s’agit de l’algorithme par défaut utilisé par **MakeAppx.exe**.

Pour connaître l'algorithme de hachage utilisé lors de l’empaquetage de votre application, extrayez les contenus du package d’application et examinez le fichier AppxBlockMap.xml. Pour découvrir comment décompresser/extraire un package d’application, voir [Extraire les fichiers d’un package ou d’un ensemble d’applications](https://msdn.microsoft.com/windows/uwp/packaging/create-app-package-with-makeappx-tool#extract-files-from-a-package-or-bundle). La méthode de hachage se trouve dans l’élément BlockMap au format format suivant:
```
<BlockMap xmlns="http://schemas.microsoft.com/appx/2010/blockmap" 
HashMethod="http://www.w3.org/2001/04/xmlenc#sha256">
```

Ce tableau présente chaque valeur HashMethod et son algorithme de hachage correspondant:


| Valeur HashMethod                              | Algorithme de hachage |
|-----------------------------------------------|----------------|
| http://www.w3.org/2001/04/xmlenc#sha256       | SHA256         |
| http://www.w3.org/2001/04/xmldsig-more#sha384 | SHA384         |
| http://www.w3.org/2001/04/xmlenc#sha512       | SHA512         |

> [!NOTE]
> Comme l'algorithme par défaut de **SignTool** est SHA1 (non disponible dans **MakeAppx.exe**), vous devez toujours spécifier un algorithme de hachage lors de l’utilisation de **SignTool**.

### <a name="sign-the-app-package"></a>Signer le package d’application

Une fois que toutes les conditions préalables sont réunies et que vous avez déterminé quel algorithme de hachage a été utilisé pour empaqueter votre application, vous êtes prêt à procéder à la signature. 

La syntaxe de ligne de commande générale pour la signature de package **SignTool** est la suivante:
```
SignTool sign [options] <filename(s)>
```

Le certificat utilisé pour signer votre application doit être un fichier .pfx ou être installé dans un magasin de certificats.

Pour signer votre package d’application avec un certificat issu d'un fichier .pfx, utilisez la syntaxe suivante:
```
SignTool sign /fd <Hash Algorithm> /a /f <Path to Certificate>.pfx /p <Your Password> <File path>.appx
```
```
SignTool sign /fd <Hash Algorithm> /a /f <Path to Certificate>.pfx /p <Your Password> <File path>.msix
```
Notez que l'option `/a` permet à **SignTool** de choisir automatiquement le meilleur certificat.

Si votre certificat n’est pas un fichier .pfx, utilisez la syntaxe suivante:
```
SignTool sign /fd <Hash Algorithm> /n <Name of Certificate> <File Path>.appx
```
```
SignTool sign /fd <Hash Algorithm> /n <Name of Certificate> <File Path>.msix
```

Vous pouvez également spécifier le hachage SHA1 du certificat souhaité au lieu de &lt;Nom du certificat&gt; à l’aide de cette syntaxe:
```
SignTool sign /fd <Hash Algorithm> /sha1 <SHA1 hash> <File Path>.appx
```
```
SignTool sign /fd <Hash Algorithm> /sha1 <SHA1 hash> <File Path>.msix
```

Notez que certains certificats n’utilisent pas de mot de passe. Si votre certificat ne dispose pas d’un mot de passe, omettez "/p &lt;Votre mot de passe&gt;" dans les exemples de commandes.

Une fois que votre package d’application est signé avec un certificat valide, vous êtes prêt à importer votre package sur le Store. Pour obtenir des instructions supplémentaires sur l'importation et la soumission des applications au Store, voir [Soumissions d’application](https://msdn.microsoft.com/windows/uwp/publish/app-submissions).

## <a name="common-errors-and-troubleshooting"></a>Erreurs fréquentes et résolution des problèmes
Les types d'erreurs les plus fréquentes lors de l'utilisation de **SignTool** sont internes et prennent généralement la forme suivante:

```
SignTool Error: An unexpected internal error has occurred.
Error information: "Error: SignerSign() failed." (-2147024885 / 0x8007000B) 
```

Si le code d’erreur commence par 0x8008, comme 0x80080206 (APPX_E_CORRUPT_CONTENT), le package en cours de signature n’est pas valide. Si vous obtenez ce type d’erreur, vous devez régénérer le package et exécuter à nouveau **SignTool**.

**SignTool** dispose d’une option de débogage disponible pour afficher les erreurs de certificat et de filtrage. Pour utiliser la fonctionnalité de débogage, placez l'option `/debug` directement après `sign`, suivie de la commande **SignTool** complète.
```
SignTool sign /debug [options]
``` 

Le type d'erreur le plus courant est 0x8007000B. Pour ce type d’erreur, vous trouverez plus d’informations dans le journal des événements.
 
Pour trouver plus d’informations dans le journal des événements:
- Exécutez Eventvwr.msc
- Ouvrez le journal des événements: Observateur d’événements (Local) -> Journaux des applications et des services -> Microsoft -> Windows -> appxPackagingOM -> Microsoft-Windows-AppxPackaging/Operational
- Recherchez l’événement d’erreur le plus récent

L’erreur interne 0x8007000B correspond généralement à l'une des valeurs suivantes:

| **ID d’événement** | **Exemple de chaîne d'événements** | **Suggestion** |
|--------------|--------------------------|----------------|
| 150          | erreur 0x8007000B: le nom de l’éditeur de manifeste de l’application (CN = Contoso) doit correspondre au nom du sujet du certificat de signature (CN = Contoso, C = US). | Le nom de l’éditeur du manifeste de l'application doit correspondre exactement au nom de sujet de la signature.               |
| 151          | erreur 0x8007000B: la méthode de hachage de la signature qui est spécifiée (SHA512) doit correspondre à la méthode de hachage utilisée dans le mappage de bloc du package d'application (SHA256).     | Le hashAlgorithm spécifié dans le paramètre /fd est incorrect. Exécutez de nouveau **SignTool** à l’aide du hashAlgorithm qui correspond à la carte de bloc du package d'application (utilisée pour créer le package d’application)  |
| 152          | Erreur 0x8007000B: le contenu du package d’application doit permettre de valider son mappage de bloc.                                                           | Le package d’application est endommagé et doit être reconstruit pour générer un nouveau mappage de bloc. Pour en savoir plus sur la création d’un package d’application, voir [Créer un package d’application avec l’outil MakeAppx.exe](https://msdn.microsoft.com/windows/uwp/packaging/create-app-package-with-makeappx-tool) |