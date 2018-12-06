---
title: Créer un fichier de programme d'installation d'application avec VisualStudio
description: Découvrez comment utiliser VisualStudio pour permettre des mises à jour automatiques à l'aide du fichier .appinstaller.
ms.date: 5/2/2018
ms.topic: article
keywords: windows10, uwp, programme d’installation de l’application, AppInstaller, charger une version test
ms.localizationpriority: medium
ms.openlocfilehash: 5c7055748eb8905341d9f90c47e6141c9c9c599e
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/05/2018
ms.locfileid: "8743568"
---
# <a name="create-an-app-installer-file-with-visual-studio"></a>Créer un fichier de programme d'installation d'application avec VisualStudio

En démarrant avec la version 1804 de Windows10 et la mise à jour 15.7 de VisualStudio2017, les applications à chargement de version test peuvent être configurées pour recevoir des mises à jour automatiques à l'aide d'un fichier `.appinstaller`. VisualStudio prend en charge l'autorisation de ces mises à jour.

## <a name="app-installer-file-location"></a>Emplacement du fichier du programme d'installation d'application
Le fichier `.appinstaller` peut être hébergé dans un emplacement partagé tel qu'un point de terminaison HTTP ou un dossier partagé UNC. Il comprend alors le chemin de destination des packages d'application à installer. Les utilisateurs installent l'application à partir de l'emplacement partagez et autorisent des vérifications périodiques de nouvelles mises à jour. 


### <a name="configure-the-project-to-target-the-correct-windows-version"></a>Configurer le projet pour cibler la version appropriée de Windows

Vous pouvez configurer la propriété `TargetPlatformMinVersion` lorsque vous créez le projet ou la modifier ultérieurement à partir des propriétés du projet. 

>[!IMPORTANT]
> Le fichier du programme d'installation d'application est généré uniquement lorsque la `TargetPlatformMinVersion` est la version 1804 ou ultérieure de Windows10.


### <a name="create-packages"></a>Créer des packages

Pour distribuer une application via le chargement indépendant, vous devez créer un package d’application (.appx/.msix) ou un ensemble d’applications (.appxbundle/.msixbundle) et la publier dans un emplacement partagé.

Pour ce faire, utilisez l'assistant **Créer des packages d'application** dans VisualStudio à l'aide des étapes suivantes.

1. Cliquez avec le bouton droit sur le projet et choisissez **Store** -> **Créer des packages d'application**.  

![Menu contextuel avec navigation vers Créer des packages d’application](images/packaging-screen2.jpg)   

L’assistant **Créer des packages d’application** s’affiche.

2. Sélectionnez **Je souhaite créer des packages pour le chargement d'une version test.** et **Activer les mises à jour automatiques**  

![Fenêtre Créer vos packages affichée](images/select-sideloading.png)  

**Activer les mises à jour automatiques** est activé uniquement si la version `TargetPlatformMinVersion` du projet est définie sur la version appropriée de Windows10.

3. La boîte de dialogue **Sélectionner et configurer des packages** vous permet de sélectionner les configurations d'architecture prises en charge. Si vous sélectionnez une offre groupée, un programme d'installateur unique sera généré. En revanche, si vous préférez choisir un package par architecture, vous obtiendrez également un fichier de programme d'installation par architecture.  Si vous ne savez pas quelle(s) architecture(s) choisir ou si vous souhaitez en savoir plus sur les architectures utilisées par divers appareils, consultez [Architectures de package d’application](device-architecture.md).

4. Configurez tous les détails supplémentaires, notamment la numérotation de version ou l'emplacement de sortie de l'application.

![Fenêtre Créer des packages d’application avec la configuration de package affichée](images/packaging-screen5.jpg)  

5. Si vous avez coché **Activer les mises à jour automatiques** à l’étape2, la boîte de dialogue **Configurer les paramètres de mise à jour** s’affiche. Ici, vous pouvez spécifier l'**URL d’installation** et la fréquence des vérifications de mise à jour.

![Fenêtre Configurer les paramètres de mise à jour avec la publication de la configuration d'emplacement affichée](images/sideloading-screen.png)  

6. Une fois votre application correctement mise en package, une boîte de dialogue affiche l'emplacement du dossier de sortie contenant votre package d'application. Le dossier de sortie comprend tous les fichiers nécessaires au chargement d'une version test de l'application, notamment une page HTML à utiliser pour promouvoir votre application.

### <a name="publish-packages"></a>Publier les packages

Afin de mettre l'application à disposition, les fichiers générés doivent être publiés à l'emplacement spécifié:

#### <a name="publish-to-shared-folders-unc"></a>Publier sur des dossiers partagés (UNC)

Si vous souhaitez publier vos packages dans des dossiers partagés sous la convention d'affectation des noms (UNC), configurez le dossier de sortie du package d'application et l'URL d'installation (pour plus d'informations, lisez l'étape6) vers le même chemin. L’assistant génère les fichiers à l’emplacement approprié et les utilisateurs obtiennent à la fois l’application et les futures mises à jour à partir du même chemin.

#### <a name="publish-to-a-web-location-http"></a>Publier sur un emplacement web (HTTP)

La publication sur un emplacement web nécessite la publication du contenu sur un serveur web. Ainsi, il est garanti que l'URL final correspond à l'URL d'installation définie dans l'assistant (pour plus d'informations, lisez l'étape6). De manière générale, soit le protocol FTP, soit le protocol SFTP est utilisé pour charger les fichiers, mais d'autres méthodes de publications existent: MSDeploy, SS ou le stockage Blob, selon votre fournisseur web.

Pour configurer le serveur web, vous devez vérifier les types MIME utilisés en tant que types de fichier en cours d'utilisation. Cet exemple concerne l'élément `web.config`pour le logiciel Internet Information Services (IIS):

```xml
<configuration>
  <system.webServer>
    <staticContent>
      <mimeMap fileExtension=".appx" mimeType="application/vns.ms-appx" />
      <mimeMap fileExtension=".appxbundle" mimeType="application/vns.ms-appx" />
      <mimeMap fileExtension=".appinstaller" mimeType="application/xml" />
    </staticContent>  
  </system.webServer>  
</configuration>
```




















