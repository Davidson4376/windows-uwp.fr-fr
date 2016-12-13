---
author: laurenhughes
title: "Créer un package d’application avec l’outil MakeAppx.exe"
description: "MakeAppx.exe crée, chiffre, déchiffre et extrait les fichiers des packages d’application et des ensembles d’applications."
translationtype: Human Translation
ms.sourcegitcommit: 28cd2b2a922a20e0b9ffc4d1ca65f6a55e92aa8f
ms.openlocfilehash: c99c76fac9303e174b5d804c2f1b99856be25006

---

# <a name="create-an-app-package-with-the-makeappxexe-tool"></a>Créer un package d’application avec l’outil MakeAppx.exe

\[ Article mis à jour pour les applications UWP sur Windows&nbsp;10. Pour les articles sur Windows&nbsp;8.x, consultez l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132).\]

**MakeAppx.exe** crée des packages d’application et des ensembles de packages d’application. **MakeAppx.exe** extrait également les fichiers d’un package d’application ou d’un ensemble d’applications, et chiffre ou déchiffre les packages d’application et les ensembles d’applications. Cet outil est inclus dans le Kit de développement logiciel (SDK) Windows&nbsp;10 et peut s’utiliser à partir d’une invite de commandes ou d’un fichier de script.

Si vous voulez utiliser l’Assistant de Microsoft Visual Studio pour créer un package d’application, ou si vous souhaitez un guide complet sur la configuration, la création et le test de votre package d’application pour le Windows Store, consultez la section [Création de packages d’application UWP](https://msdn.microsoft.com/windows/uwp/packaging/packaging-uwp-apps).

Notez que **MakeAppx.exe** ne crée pas un fichier .appxupload. Le fichier .appxupload est créé dans le cadre du processus de création de packages Visual&nbsp;Studio et contient deux&nbsp;autres fichiers&nbsp;: .appx et .appxsym. Le fichier .appxsym est un fichier .pdb compressé contenant les symboles publics de votre application utilisés pour [bloquer l’analyse](https://blogs.windows.com/buildingapps/2015/07/13/crash-analysis-in-the-unified-dev-center/) dans le centre de développement Windows. Vous pouvez également envoyer un fichier .appx normal, mais aucune information d’incident analytique ou de débogage ne sera disponible. Pour plus d’informations sur l’envoi de packages au Windows store, consultez [Chargement des packages d’application](https://msdn.microsoft.com/windows/uwp/publish/upload-app-packages). 

Pour créer manuellement un fichier .appxupload&nbsp;:
- Placez les fichiers .appx et .appxsym dans un dossier.
- Compressez le dossier.
- Remplacez l’extension&nbsp;.zip du dossier compressé par .appxupload.

## <a name="using-makeappxexe"></a>Utilisation de MakeAppx.exe

Selon votre chemin d’installation du Kit de développement logiciel (SDK), c’est là où **MakeAppx.exe** se trouve sur votre PC Windows&nbsp;10&nbsp;:
- x86&nbsp;: C:\Program Files (x86)\Windows Kits\10\bin\x86\makeappx.exe
- x64&nbsp;: C:\Program Files (x86)\Windows Kits\10\bin\x64\makeappx.exe

Il n’existe aucune version ARM de cet outil.

### <a name="makeappxexe-syntax-and-options"></a>Options et syntaxe de MakeAppx.exe

Syntaxe générale de **MakeAppx.exe**&nbsp;:

``` Usage
MakeAppx <command> [options]      
```

Le tableau suivant décrit les commandes de **MakeAppx.exe**.

| **Commande**   | **Description**                       |
|---------------|---------------------------------------|
| pack          | Crée un package.                    |
| unpack        | Extrait tous les fichiers du package spécifié dans le répertoire de sortie spécifié. |
| bundle        | Crée un ensemble d’applications.                     |
| unbundle      | Décompresse tous les packages dans un sous-répertoire du chemin de sortie spécifié, correspondant au nom complet de l’ensemble d’application. |
| encrypt       | Crée un package d’application ou un ensemble d’applications chiffré, à partir du package ou de l’ensemble d’applications d’entrée dans le package ou l’ensemble d’applications de sortie. |
| decrypt       | Crée un package d’application ou un ensemble d’applications déchiffré, à partir du package ou de l’ensemble d’applications d’entrée dans le package ou l’ensemble d’applications de sortie. |


Cette liste d’options s’applique à toutes les commandes&nbsp;:

| **Option**    | **Description**                       |
|---------------|---------------------------------------|
| /d            | Spécifie l’entrée, la sortie ou le répertoire de contenu. |
| /l            | Utilisé pour les packages localisés. La valeur par défaut se déclenche sur les packages localisés. Cette option ne désactive que cette validation, pas les autres. |
| /kf           | Chiffre ou déchiffre le package ou l’ensemble d’applications, à l’aide de la clé du fichier de clé spécifié. Cette option n’est pas utilisable avec /kt. |
| /kt           | Chiffre ou déchiffre le package ou l’ensemble d’applications, à l’aide de la clé de test global. Cette option n’est pas utilisable avec /kf. |
| /no           | Empêche le remplacement du fichier de sortie s’il existe. Si vous ne spécifiez pas cette option ou l’option&nbsp;/o, l’utilisateur est invité à indiquer s’il souhaite remplacer le fichier. |
| /nv           | Désactive la validation sémantique. Si vous ne spécifiez pas cette option, l’outil effectue une validation complète du package. |
| /o            | Remplace le fichier de sortie s’il existe. Si vous ne spécifiez pas cette option ou l’option&nbsp;/no, l’utilisateur est invité à indiquer s’il souhaite remplacer le fichier. |
| /p            | Spécifie le package d’application ou l’ensemble d’applications.  |
| /v            | Active la sortie de la journalisation détaillée dans la console. |
| /?            | Affiche l’aide.                   |


La liste suivante contient les arguments possibles&nbsp;:

| **Argument**                          | **Description**                       |
|---------------------------------------|---------------------------------------|
| &lt;Nom du package de sortie&gt;           | Nom du package créé. C’est le nom du fichier suivi de l’extension&nbsp;.appx. |
| &lt;Nom du package de sortie chiffré&gt; | Nom du package chiffré créé. C’est le nom du fichier suivi de l’extension&nbsp;.eappx. |
| &lt;Nom du package d’entrée&gt;            | Nom du package. C’est le nom du fichier suivi de l’extension&nbsp;.appx. |
| &lt;Nom du package d’entrée chiffré&gt;  | Nom du package chiffré. C’est le nom du fichier suivi de l’extension&nbsp;.eappx. |
| &lt;Nom de l’ensemble d’applications de sortie&gt;            | Nom de l’ensemble d’applications créé. C’est le nom du fichier suivi de l’extension&nbsp;.appxbundle. |
| &lt;Nom de l’ensemble d’applications de sortie chiffré&gt;  | Nom de l’ensemble d’applications chiffré créé. C’est le nom du fichier suivi de l’extension&nbsp;.eappxbundle. |
| &lt;Nom de l’ensemble d’applications d’entrée&gt;             | Nom de l’ensemble d’applications. C’est le nom du fichier suivi de l’extension&nbsp;.appxbundle. |
| &lt;Nom de l’ensemble d’applications d’entrée chiffré&gt;   | Nom de l’ensemble d’applications chiffré. C’est le nom du fichier suivi de l’extension&nbsp;.eappxbundle. |
| &lt;Répertoire de contenu&gt;             | Chemin d’accès au contenu du package d’application ou de l’ensemble d’applications. |
| &lt;Fichier de mappage&gt;                  | Nom du fichier qui spécifie la source et la cible du package. |
| &lt;Répertoire de sortie&gt;              | Chemin d’accès au répertoire des packages et ensembles d’applications de sortie. |
| &lt;Fichier de clé&gt;                      | Nom du fichier contenant une clé de chiffrement ou de déchiffrement. |
| &lt;ID d’algorithme&gt;                  | Algorithmes utilisés lors de la création d’un mappage de bloc. Les algorithmes valides incluent&nbsp;: SHA256 (par défaut), SHA384 et SHA512. |


### <a name="create-an-app-package"></a>Créer un package d’application

Un package d’application est un jeu complet des fichiers empaquetés dans un fichier de package&nbsp;.appx. Pour créer un package d’application à l’aide de la commande **pack**, vous devez indiquer un répertoire de contenu ou un fichier de mappage pour l’emplacement du package. Vous pouvez également chiffrer un package lors de sa création. Si vous souhaitez chiffrer le package, vous devez utiliser /ep et indiquer si vous utilisez un fichier de clé (/kf) ou la touche de test global (/kt). Pour plus d’informations sur la création d’un package chiffré, consultez [Chiffrer ou déchiffrer un package ou un ensemble d’applications](#encrypt-or-decrypt-a-package-or-bundle).

Options propres à la commande **pack**&nbsp;:

| **Option**    | **Description**                       |
|---------------|---------------------------------------|
| /f            | Spécifie le fichier de mappage.           |
| /h            | Spécifie l’algorithme de hachage à utiliser lors de la création du mappage de bloc. Elle n’est utilisable qu’avec la commande pack. Les algorithmes valides incluent&nbsp;: SHA256 (par défaut), SHA384 et SHA512. |
| /m            | Spécifie le chemin d’accès au manifeste d’une application d’entrée, qui sera utilisé comme base pour générer le manifeste du package de ressources ou du package d’application de sortie.  Lorsque vous utilisez cette option, vous devez également utiliser /f et inclure une section [ResourceMetadata] dans le fichier de mappage pour spécifier les dimensions de la ressource à inclure dans le manifeste généré.|
| /nc           | Empêche la compression des fichiers du package. Par défaut, les fichiers sont compressés en fonction du type de fichier détecté. |
| /r            | Crée un package de ressources. Cette option doit être utilisée avec /m et implique l’utilisation de l’option /l. |  


Les exemples d’utilisation suivants illustrent certaines syntaxes possibles pour la commande **pack**&nbsp;:

``` syntax 
MakeAppx pack [options] /d <content directory> /p <output package name>
MakeAppx pack [options] /f <mapping file> /p <output package name>
MakeAppx pack [options] /m <app package manifest> /f <mapping file> /p <output package name>
MakeAppx pack [options] /r /m <app package manifest> /f <mapping file> /p <output package name>
MakeAppx pack [options] /d <content directory> /ep <encrypted output package name> /kf <key file>
MakeAppx pack [options] /d <content directory> /ep <encrypted output package name> /kt

```
L’exemple suivant montre des exemples de ligne de commande **pack**&nbsp;:

``` examples
MakeAppx pack /v /h SHA256 /d "C:\My Files" /p MyPackage.appx
MakeAppx pack /v /o /f MyMapping.txt /p MyPackage.appx
MakeAppx pack /m "MyApp\AppxManifest.xml" /f MyMapping.txt /p AppPackage.appx
MakeAppx pack /r /m "MyApp\AppxManifest.xml" /f MyMapping.txt /p ResourcePackage.appx
MakeAppx pack /v /h SHA256 /d "C:\My Files" /ep MyPackage.eappx /kf MyKeyFile.txt
MakeAppx pack /v /h SHA256 /d "C:\My Files" /ep MyPackage.eappx /kt
```

### <a name="create-an-app-bundle"></a>Créer un ensemble d’applications

Un ensemble d’applications est similaire à un package d’application, mais il peut réduire la taille de l’application téléchargée par les utilisateurs. Les ensembles d’applications sont utiles pour les ressources linguistiques, différentes ressources de mise à l’échelle d’images ou des ressources qui s’appliquent à des versions spécifiques de Microsoft DirectX. Comme pour un package d’application chiffré, vous pouvez également chiffrer l’ensemble d’applications lors de sa création. Pour chiffrer l’ensemble d’applications, utilisez l’option /ep et indiquez si vous utilisez un fichier de clé (/kf) ou la clé de test global (/kt). Pour plus d’informations sur la création d’un ensemble d’applications chiffré, consultez [Chiffrer ou déchiffrer un package ou un ensemble d’applications](#encrypt-or-decrypt-a-package-or-bundle).

Options propres à la commande **bundle**&nbsp;:

| **Option**    | **Description**                       |
|---------------|---------------------------------------|
| /bv           | Spécifie le numéro de version de l’ensemble d’applications. Le numéro de version doit comprendre quatre&nbsp;parties séparées par des points, au format&nbsp;: &lt;Majeur&gt;.&lt;Mineur&gt;.&lt;Build&gt;.&lt;Révision&gt;. |
| /f            | Spécifie le fichier de mappage.           |

Notez que si la version de l’ensemble d’applications n’est pas spécifiée ou si la version «&nbsp;0.0.0.0&nbsp;» est spécifiée, l’ensemble d’applications est créé avec la date et l’heure actuelles.

Les exemples d’utilisation suivants illustrent certaines syntaxes possibles pour la commande **bundle**&nbsp;:

``` syntax
MakeAppx bundle [options] /d <content directory> /p <output bundle name>
MakeAppx bundle [options] /f <mapping file> /p <output bundle name>
MakeAppx bundle [options] /d <content directory> /ep <encrypted output bundle name> /kf MyKeyFile.txt
MakeAppx bundle [options] /f <mapping file> /ep <encrypted output bundle name> /kt
```
Le bloc suivant contient des exemples pour la commande **bundle**&nbsp;:

``` examples
MakeAppx bundle /v /d "C:\My Files" /p MyBundle.appxbundle
MakeAppx bundle /v /o /bv 1.0.1.2096 /f MyMapping.txt /p MyBundle.appxbundle
MakeAppx bundle /v /o /bv 1.0.1.2096 /f MyMapping.txt /ep MyBundle.eappxbundle /kf MyKeyFile.txt
MakeAppx bundle /v /o /bv 1.0.1.2096 /f MyMapping.txt /ep MyBundle.eappxbundle /kt
```

### <a name="extract-files-from-a-package-or-bundle"></a>Extraire les fichiers d’un package ou d’un ensemble d’applications

Outre la création de packages et d’ensemble d’applications, **MakeAppx.exe** peut également décompresser ou dissocier des packages existants. Vous devez indiquer le répertoire de contenu qui va accueillir les fichiers extraits. Si vous essayez d’extraire des fichiers d’un package ou d’un ensemble d’applications chiffré, vous pouvez déchiffrer et extraire les fichiers simultanément avec l’option&nbsp;/ep et en spécifiant si le déchiffrement doit s’effectuer avec un fichier de clé (/kf) ou la touche de test global (/kt). Pour plus d’informations sur le déchiffrement d’un package ou d’un ensemble d’applications, consultez [Chiffrer ou déchiffrer un package ou un ensemble d’applications](#encrypt-or-decrypt-a-package-or-bundle).

Options propres aux commandes **unpack** et **unbundle**&nbsp;:

| **Option**    | **Description**                       |
|---------------|---------------------------------------|
| /nd           | N’effectue pas le déchiffrement lors de la décompression du package ou de la dissociation de l’ensemble d’applications. |
| /pfn          | Décompresse tous les fichiers dans un sous-répertoire du chemin de sortie spécifié, correspondant au nom complet du package ou de l’ensemble d’applications. |

Les exemples d’utilisation suivants illustrent certaines syntaxes possibles pour les commandes **unpack** et **unbundle**&nbsp;:

``` syntax
MakeAppx unpack [options] /p <input package name> /d <output directory>
MakeAppx unpack [options] /ep <encrypted input package name> /d <output directory> /kf <key file>
MakeAppx unpack [options] /ep <encrypted input package name> /d <output directory> /kt

MakeAppx unbundle [options] /p <input bundle name> /d <output directory>
MakeAppx unbundle [options] /ep <encrypted input bundle name> /d <output directory> /kf <key file>
MakeAppx unbundle [options] /ep <encrypted input bundle name> /d <output directory> /kt
```

Le bloc suivant contient des exemples d’utilisation des commandes **unpack** et **unbundle**&nbsp;:

``` examples
MakeAppx unpack /v /p MyPackage.appx /d "C:\My Files"
MakeAppx unpack /v /ep MyPackage.eappx /d "C:\My Files" /kf MyKeyFile.txt
MakeAppx unpack /v /ep MyPackage.eappx /d "C:\My Files" /kt

MakeAppx unbundle /v /p MyBundle.appxbundle /d "C:\My Files"
MakeAppx unbundle /v /ep MyBundle.eappxbundle /d "C:\My Files" /kf MyKeyFile.txt
MakeAppx unbundle /v /ep MyBundle.eappxbundle /d "C:\My Files" /kt
```

### <a name="encrypt-or-decrypt-a-package-or-bundle"></a>Chiffrer ou déchiffrer un package ou un ensemble d’applications

L’outil **MakeAppx.exe** peut également chiffrer ou déchiffrer un package ou un ensemble d’applications. Vous devez simplement indiquer le nom du package, le nom du package de sortie, et si le chiffrement ou le déchiffrement doit utiliser un fichier de clé (/kf) ou la touche de test global (/kt).

Le chiffrement et le déchiffrement ne sont pas disponibles dans l’Assistant de création de packages de Visual Studio. 

Options propres aux commandes **encrypt** et **decrypt**&nbsp;:

| **Option**    | **Description**                       |
|---------------|---------------------------------------|
| /ep           | Spécifie un package d’application ou un ensemble d’applications chiffré. |

Les exemples d’utilisation suivants illustrent certaines syntaxes possibles pour les commandes **unbundle** et **decrypt**&nbsp;:

``` syntax
MakeAppx encrypt [options] /p <package name> /ep <output package name> /kf <key file>
MakeAppx encrypt [options] /p <package name> /ep <output package name> /kt

MakeAppx decrypt [options] /ep <package name> /p <output package name> /kf <key file>
MakeAppx decrypt [options] /ep <package name> /p <output package name> /kt
```

Le bloc suivant contient des exemples d’utilisation des commandes **encrypt** et **decrypt**&nbsp;:

``` examples
MakeAppx.exe encrypt /p MyPackage.appx /ep MyEncryptedPackage.eappx /kt
MakeAppx.exe encrypt /p MyPackage.appx /ep MyEncryptedPackage.eappx /kf MyKeyFile.txt

MakeAppx.exe decrypt /p MyPackage.appx /ep MyEncryptedPackage.eappx /kt
MakeAppx.exe decrypt p MyPackage.appx /ep MyEncryptedPackage.eappx /kf MyKeyFile.txt
```

## <a name="key-files"></a>Fichiers de clé

Les fichiers de clé doivent commencer par une ligne contenant la chaîne «&nbsp;[Keys]&nbsp;» suivie des lignes décrivant les clés avec lesquelles chiffrer chaque package. Chaque clé est représentée par une paire de chaînes entre guillemets, séparées par des espaces ou des tabulations. La première chaîne représente l’ID clé de 32&nbsp;octets codés en base 64, et la seconde représente la clé de chiffrement de 32&nbsp;octets codée en base&nbsp;64. Un fichier de clé doit être un fichier texte simple.

Exemple de fichier de clé&nbsp;:

``` Example
[Keys]
"OWVwSzliRGY1VWt1ODk4N1Q4R2Vqc04zMzIzNnlUREU="    "MjNFTlFhZGRGZEY2YnVxMTBocjd6THdOdk9pZkpvelc="
```

## <a name="mapping-files"></a>Fichier de mappage
Les fichiers de mappage doivent commencer par une ligne contenant la chaîne «&nbsp;[Files]&nbsp;» suivie des lignes décrivant les fichiers à ajouter au package. Chaque clé est représentée par une paire de chemins entre guillemets, séparés par des espaces ou des tabulations. Chaque fichier représente sa source (sur le disque) et sa destination (dans le package). Un fichier de mappage doit être un fichier texte simple.

Exemple de fichier de mappage (sans l’option /m)&nbsp;:

``` Example
[Files]
"C:\MyApp\StartPage.html"               "default.html"
"C:\Program Files (x86)\example.txt"    "misc\example.txt"
"\\MyServer\path\icon.png"              "icon.png"
"my app files\readme.txt"               "my app files\readme.txt"
"CustomManifest.xml"                    "AppxManifest.xml"
``` 

Lorsque vous utilisez un fichier de mappage, vous pouvez choisir d’utiliser l’option /m ou non. L’option /m permet à l’utilisateur de spécifier les métadonnées des ressources du fichier de mappage à inclure dans le manifeste généré. Si vous utilisez l’option /m, le fichier de mappage doit contenir une section commençant par la ligne «&nbsp;[ResourceMetadata]&nbsp;», suivie de lignes qui spécifient «&nbsp;ResourceDimensions&nbsp;» et «&nbsp;ResourceId&nbsp;». Un package d’application peut contenir plusieurs «&nbsp;ResourceDimensions&nbsp;», mais il ne peut avoir qu’un seul «&nbsp;ResourceId&nbsp;».

Exemple de fichier de mappage (avec l’option /m)&nbsp;:

``` Example
[ResourceMetadata]
"ResourceDimensions"                    "language-en-us"
"ResourceId"                            "English"

[Files]
"images\en-us\logo.png"                 "en-us\logo.png"
"en-us.pri"                             "resources.pri"
```

## <a name="semantic-validation-performed-by-makeappxexe"></a>Validation sémantique effectuée par MakeAppx.exe

**MakeAppx.exe** effectue une validation sémantique limitée, conçue pour détecter les erreurs de déploiement les plus courantes et garantir la validité du package d’application. Sélectionnez l’option /nv pour ne pas effectuer la validation dans **MakeAppx.exe**. 

Cette validation vérifie que&nbsp;:
- tous les fichiers référencés dans le manifeste du package sont inclus dans le package d’application&nbsp;;
- une application n’a pas deux&nbsp;clés identiques&nbsp;;
- une application n’utilise pas un protocole interdit figurant dans cette liste&nbsp;: SMB, FILE, MS-WWA-WEB, MS-WWA. 

Cette validation sémantique n’est pas complète. Elle ne vise qu’à détecter les erreurs courantes. **MakeAppx.exe** ne garantit pas l’installation correcte des packages qu’il crée.


<!--HONumber=Dec16_HO1-->


