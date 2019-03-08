---
title: Ajouter la Xbox Live à un projet XDK
description: Découvrez comment ajouter la Xbox Live à un projet de Kit de développement Xbox (XDK) nouvelle ou existante.
ms.assetid: fc6f987c-1a87-4ff5-b063-891591aa6653
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, jeux, uwp, windows 10, xbox une, xdk
ms.localizationpriority: medium
ms.openlocfilehash: f17765b09dcb0b6f5c89d168f2d3d9611a60ffa6
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57649044"
---
# <a name="add-xbox-live-to-a-new-or-existing-xdk-project"></a>Ajouter la Xbox Live à un projet XDK nouveau ou existant

Cette rubrique décrit comment ajouter la Xbox Live à un projet XDK nouveau ou existant.

Le processus est :

- Le programme d’installation de votre environnement de développement Xbox One
- Obtenir votre ID
- Configurer votre console de développement
- Ajouter le n ° titre et SCID à votre fichier binaire


## <a name="setup-up-your-xbox-one-development-environment"></a>Le programme d’installation de votre environnement de développement Xbox One
Tout d’abord, configurer la console en suivant la section « Configuration d’un Xbox un développement environnement » dans la documentation XDK

## <a name="get-your-ids"></a>Obtenir votre ID

Pour permettre aux services Xbox Live, vous devrez obtenir plusieurs ID pour configurer votre kit de développement et votre titre. Il est possible avec le même processus.

Vous allez obtenir votre ID en effectuant le processus de [configuration du service Xbox Live](../xbox-live-service-configuration.md)

## <a name="configure-your-development-console"></a>Configurer votre console de développement

Une fois que vous avez votre ID, suivez le [configurer votre console de développement](configure-your-development-console.md) guide pour configurer votre console de développement.

## <a name="add-the-titleid-and-scid-to-your-binary"></a>Ajouter le n ° titre et SCID à votre fichier binaire
Bien que le bac à sable est configuré sur un niveau de la plateforme pour chaque Kit de développement, le n ° titre et SCID sont liés à un fichier binaire spécifique. Pour ajouter un n ° titre et le SCID à votre fichier binaire, modifiez le Package.appxmanifest pour ce fichier binaire en ajoutant un nouveau nœud dans le <Extensions> nœud comme suit :

```
<Applications>
    ...
    <Application ...>
      ...
      <Extensions>
        <mx:Extension Category="xbox.live">
           <mx:XboxLive TitleId="<your titleID>" PrimaryServiceConfigId="<your SCID>" RequireXboxLive="<boolean indicating Live requirement>" />
        </mx:Extension>
      </Extensions>
   </Application>
</Applications>
```

Pour plus d’informations sur le fichier AppxManifest.xml, reportez-vous aux modèles de projet dans Visual Studio pour le développement d’une Xbox.

Voir schéma de manifeste d’Application pour obtenir une description de l’application de schéma de manifeste.

**L’indicateur RequireXboxLive** si l’indicateur est défini sur true, le titre de RequireXboxLive ne lancera pas, sauf si le niveau de la connexion Windows.Networking.Connectivity retourne en tant que XboxLiveAccess et le titre efface l’authentification avec Xbox Live. Cela garantit que le titre a pris les dernières mises à jour du contenu. Si la connexion est perdue pendant que le titre est en cours d’exécution, le titre est suspendu.

Titres « Internet requise » doivent marquer RequireXboxLive comme true et notez que marquant votre titre de cette façon ne garantit pas que les services requis pour le titre sont en hausse et en cours d’exécution.
