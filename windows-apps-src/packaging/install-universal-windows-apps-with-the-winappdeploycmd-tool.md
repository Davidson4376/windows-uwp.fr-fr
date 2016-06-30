---
author: msatranjr
ms.assetid: 6AA037C0-35ED-4B9C-80A3-5E144D7EE94B
title: "Installer des applications avec l’outil WinAppDeployCmd.exe"
description: "Le déploiement d’applications Windows (WinAppDeployCmd.exe) est un outil de ligne de commande qui permet de déployer une application de plateforme Windows universelle (UWP) à partir d’un ordinateur Windows 10 et vers tout appareil Windows 10 Mobile."
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: d3135724e092495e8f937e6d53bdfbaa9e7cf450

---
# Installer des applications avec l’outil WinAppDeployCmd.exe

\[ Article mis à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

Le déploiement d’applications Windows (WinAppDeployCmd.exe) est un outil de ligne de commande qui permet de déployer une application de plateforme Windows universelle (UWP) à partir d’un ordinateur Windows 10 et vers tout appareil Windows 10 Mobile. Vous pouvez utiliser cet outil pour déployer un package .appx lorsque l’appareil Windows 10 Mobile est connecté via un port USB ou disponible sur le même sous-réseau, sans avoir besoin de Microsoft Visual Studio ni de la solution pour cette application. Cet article décrit comment installer des applications UWP à l’aide de cet outil.

Vous devez simplement installer le kit de développement logiciel (SDK) Windows 10 pour exécuter l’outil WinAppDeployCmd à partir d’une invite de commandes ou d’un fichier de script. Lorsque vous installez une application avec WinAppDeployCmd.exe, celui-ci utilise le fichier .appx pour le chargement indépendant de votre application sur un appareil Windows 10 Mobile. Cette commande n’installe pas le certificat nécessaire pour votre application. Pour exécuter l’application, l’appareil Windows 10 Mobile doit être en mode développeur ou le certificat doit déjà avoir été installé.

Avant de pouvoir déployer votre application pour les appareils Windows 10 Mobile, vous devez d’abord créer un package. Pour plus d’informations, voir \[lien parent ici \].

L’outil **WinAppDeployCmd.exe** se trouve ici sur votre ordinateur Windows 10 : **C:\\Program Files (x86)\\Windows Kits\\10\\bin\\x86\\WinAppDeployCmd.exe** (selon le chemin d’installation du Kit de développement logiciel (SDK) sur votre ordinateur). Tout d’abord, connectez votre appareil Windows 10 Mobile au même sous-réseau ou connectez-le directement à votre ordinateur Windows 10 via un port USB. Utilisez ensuite la syntaxe et les exemples suivants de la commande présentés plus loin dans cet article pour déployer votre package .appx :

## Options et syntaxe WinAppDeployCmd

Voici la syntaxe que vous pouvez utiliser pour **WinAppDeployCmd.exe**

``` syntax
WinAppDeployCmd command -option <argument> ...
    WinAppDeployCmd devices
    WinAppDeployCmd devices <x>
    WinAppDeployCmd install -file <path> -ip <address>
    WinAppDeployCmd install -file <path> -guid <address> -pin <p>
    WinAppDeployCmd install -file <path> -ip <address> -dependency <a> <b> ...
    WinAppDeployCmd install -file <path> -guid <address> -dependency <a> <b> ...
    WinAppDeployCmd uninstall -file <path>
    WinAppDeployCmd uninstall -package <name>
    WinAppDeployCmd update -file <path>
    WinAppDeployCmd list -ip <address>
    WinAppDeployCmd list -guid <address>
```

Vous pouvez installer ou désinstaller une application sur l’appareil cible, ou vous pouvez mettre à jour une application déjà installée. Pour conserver les données ou les paramètres enregistrés par une application déjà installée, utilisez les options **update** plutôt que les options **install**.

Le tableau suivant décrit les commandes pour **WinAppDeployCmd.exe**.

|             |                                                                     |
|-------------|---------------------------------------------------------------------|
| **Commande** | **Description**                                                     |
| devices     | Affiche la liste des périphériques réseau disponibles.                         |
| install     | Installe un package d’application UWP sur l’appareil cible.                     |
| update      | Met à jour une application UWP déjà installée sur l’appareil cible.    |
| list        | Affiche la liste des applications UWP installées sur l’appareil cible spécifié. |
| uninstall   | Désinstalle le package d’application spécifié de l’appareil cible.         |

 

Le tableau suivant décrit les options pour **WinAppDeployCmd.exe**.

|                  |                                                                                                                                                                                                               |
|------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Commande**      | **Description**                                                                                                                                                                                               |
| -h (-help)       | Affiche les commandes, les options et les arguments.                                                                                                                                                                     |
| -ip              | Adresse IP de l’appareil cible.                                                                                                                                                                              |
| -g (-guid)       | Identificateur unique de l’appareil cible.                                                                                                                                                                       |
| -d (-dependency) | (Facultatif) Spécifie le chemin d’accès de dépendance pour chacune des dépendances du package. Si aucun chemin d’accès n’est spécifié, l’outil recherche des dépendances dans le répertoire racine pour le package d’application et les répertoires du kit de développement logiciel (SDK). |
| -f (-file)       | Chemin d’accès de fichier pour le package d’application à installer, mettre à jour ou désinstaller.                                                                                                                                                |
| -p (-package)    | Le nom complet du package pour le package d’application à désinstaller. (Vous pouvez utiliser la commande « list » pour rechercher le nom complet des packages déjà installés sur l’appareil.)                                                   |
| -pin             | Un code confidentiel ,s’il est nécessaire pour établir une connexion avec l’appareil cible. (Vous serez invité à réessayer avec l’option -pin si une authentification est nécessaire.)                                                 |

 

Le tableau suivant décrit les options pour **WinAppDeployCmd.exe**.

|                        |                                                                              |
|------------------------|------------------------------------------------------------------------------|
| **Argument**           | **Description**                                                              |
| &lt;x&gt;              | Délai d’expiration en secondes. (La valeur par défaut est 10)                                          |
| &lt;adresse&gt;        | Adresse IP ou identificateur unique de l’appareil cible.                        |
| &lt;a&gt;&lt;b&gt; ... | Chemin d’accès de dépendance pour chacune des dépendances du package d’application.                    |
| &lt;p&gt;              | Un PIN alphanumérique indiqué dans les paramètres de l’appareil pour établir une connexion. |
| &lt;path&gt;           | Chemin d’accès au système de fichiers.                                                            |
| &lt;name&gt;           | Nom complet du package pour le package d’application à désinstaller.                          |

 
## Exemples de WinAppDeployCmd.exe

Voici quelques exemples illustrant comment déployer à partir de la ligne de commande via la syntaxe pour **WinAppDeployCmd.exe**.

Affiche les appareils qui sont disponibles pour le déploiement. La commande expire dans 3 secondes.

``` syntax
WinAppDeployCmd devices 3
```

Installe l’application à partir du package MyApp.appx qui se trouve dans le répertoire Téléchargements de votre PC pour un appareil Windows 10 Mobile avec l’adresse IP 192.168.0.1 et avec le PIN A1B2C3 nécessaire pour établir une connexion avec l’appareil.

``` syntax
WinAppDeployCmd install -file "Downloads\MyApp.appx" -ip 192.168.0.1 -pin A1B2C3
```

Désinstalle le package spécifié (en fonction de son nom complet) à partir d’un appareil Windows 10 Mobile avec l’adresse IP 192.168.0.1. Vous pouvez utiliser la commande list pour afficher le nom complet de tous les packages installés sur un appareil.

``` syntax
WinAppDeployCmd uninstall -package Company.MyApp_1.0.0.1_x64__qwertyuiop -ip 192.168.0.1
```

Met à jour l’application qui est déjà installée sur l’appareil Windows 10 Mobile avec l’adresse IP 192.168.0.1 à l’aide du package .appx spécifié.

``` syntax
WinAppDeployCmd update -file "Downloads\MyApp.appx" -ip 192.168.0.1
```




<!--HONumber=Jun16_HO4-->


