---
ms.assetid: A7E0DA1E-535A-459E-9A35-68A4150EE9F5
description: Cette rubrique explique comment ajouter la gestion des droits numériques (DRM) en fonction du matériel par PlayReady à votre application pour plateforme Windows universelle (UWP).
title: Gestion des droits numériques en fonction du matériel
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 5f0a53d0f725c134bbb7adecaa956000a53235b0
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57600904"
---
# <a name="hardware-drm"></a>Gestion des droits numériques en fonction du matériel


Cette rubrique explique comment ajouter la gestion des droits numériques (DRM) en fonction du matériel par PlayReady à votre application pour plateforme Windows universelle (UWP).

> [!NOTE] 
> La gestion des droits numériques en fonction du matériel par PlayReady est compatible avec une multitude d’appareils, y compris les appareils Windows et tiers tels que les téléviseurs, les tablettes et les téléphones. Pour qu’un appareil Windows prenne en charge la gestion des droits numériques en fonction du matériel par PlayReady, il doit exécuter Windows 10 et présente une configuration matérielle compatible.

Les fournisseurs de contenu se tournent de plus en plus vers des protections matérielles pour autoriser la lecture de contenu à valeur élevée complet dans les applications. La prise en charge robuste d’une implémentation matérielle de la base de chiffrement a été ajoutée à PlayReady pour répondre à ce besoin. Cette prise en charge permet la lecture sécurisée de contenu en haute définition (1080p) et ultra haute définition (UHD) sur plusieurs plateformes d’appareils. Le matériel de clé (y compris les clés privées, les clés de contenu et tout autre matériel de clé utilisé pour dériver ou déverrouiller ces clés) et les échantillons vidéo compressés et non compressés déchiffrés sont protégés en tirant parti de la sécurité matérielle.

## <a name="windows-tee-implementation"></a>Implémentation TEE Windows

Cette rubrique fournit une vue d’ensemble de la façon dont Windows 10 implémente l’environnement d’exécution approuvé (TEE).

Les détails de l’implémentation TEE Windows n’entrent pas dans le cadre de ce document. Cependant, une brève description de la différence entre le port TEE du kit de portage standard et le port Windows sera utile. Windows implémente la couche proxy OEM et transfère les appels de fonctions PRITEE en série à un pilote en mode utilisateur dans le sous-système Windows Media Foundation. Ceux-ci seront finalement acheminés vers le pilote TrEE (Trusted Execution Environment) Windows ou le pilote graphique OEM. Les détails de ces deux approches n’entrent pas dans le cadre de ce document. Le diagramme suivant illustre l’interaction générale des composants pour le port Windows. Si vous souhaitez développer une implémentation TEE Windows PlayReady, vous pouvez contacter <WMLA@Microsoft.com>.

![diagramme des composants tee Windows](images/windowsteecomponentdiagram720.jpg)

## <a name="considerations-for-using-hardware-drm"></a>Considérations pour l’utilisation de la gestion des droits numériques en fonction du matériel

Cette rubrique fournit une courte liste des éléments à prendre en compte pendant le développement d’applications conçues pour utiliser la gestion des droits numériques en fonction du matériel. Comme expliqué dans [Gestion des droits numériques par PlayReady](playready-client-sdk.md#output-protection), avec la gestion des droits numériques en fonction du matériel par PlayReady pour Windows 10, toutes les protections de sortie sont appliquées à partir de l’implémentation TEE Windows, ce qui a des répercussions sur les comportements de protection de sortie :

-   **Prise en charge pour le niveau de protection de sortie (norme OPL) pour numérique non compressé 270 vidéo :** HWDRM PlayReady pour Windows 10 ne prend pas en charge vers le bas de la résolution et appliquera que HDCP est engagée. Nous recommandons que le contenu haute définition pour la gestion des droits numériques en fonction du matériel présente une norme OPL supérieure à 270 (même si ce n’est pas obligatoire). Par ailleurs, nous recommandons de définir une restriction du type de protection HDCP dans la licence (HDCP version 2.2 sur Windows 10).
-   **Contrairement aux logiciels DRM (SWDRM), les protections de sortie sont appliquées sur tous les moniteurs basées sur l’analyse moins apte à.** Par exemple, si l’utilisateur a deux moniteurs connectés dont un seul prend en charge la protection HDCP, la lecture échoue si la licence nécessite la protection HDCP, même si le contenu est uniquement affiché sur l’écran qui prend en charge la protection HDCP. Avec la gestion des droits numériques en fonction du logiciel, le contenu serait lu tant qu’il est affiché uniquement sur le moniteur qui prend en charge la protection HDCP.
-   **L’utilisation par le client et la sécurisation de la gestion des droits numériques en fonction du matériel ne sont garanties que si les conditions suivantes sont remplies** par les clés et les licences de contenu :
    -   La licence utilisée pour la clé de contenu vidéo doit présenter un niveau de sécurité minimal de 3 000.
    -   L’audio doit être chiffré selon une clé de contenu différente de celle de la vidéo, et la licence utilisée pour l’audio doit afficher un niveau de sécurité minimal de 2 000. Par ailleurs, l’audio peut rester en clair.
    
En outre, vous devez prendre les éléments suivants en considération lorsque vous utilisez la gestion des droits numériques en fonction du matériel :

-   Le processus de média protégé (PMP) n’est pas pris en charge.
-   Le format Windows Media Video (également connu sous le nom de VC-1) n’est pas pris en charge (voir [Contourner la gestion des droits numériques en fonction du matériel](#override-hardware-drm)).
-   Plusieurs unités de traitement graphique (GPU) ne sont pas pris en charge par les licences persistantes.

Pour gérer les licences persistantes sur des ordinateurs dotés de plusieurs GPU, considérez le scénario suivant :

1.  Un client achète un nouvel ordinateur équipé d’une carte graphique intégrée.
2.  Le client utilise une application qui acquiert les licences persistantes lors de l’utilisation de la gestion des droits numériques en fonction du matériel.
3.  La licence persistante est maintenant liée aux clés matérielles de la carte graphique.
4.  Le client installe ensuite une nouvelle carte graphique.
5.  Toutes les licences du magasin de données hachées (HDS) sont liées à la carte vidéo intégrée, mais le client souhaite maintenant lire du contenu protégé à l’aide de la carte graphique qu’il vient d’installer.

Pour empêcher l’échec de la lecture dû au fait que les licences ne peuvent pas être déchiffrées par le matériel, PlayReady utilise un magasin HDS distinct pour chaque carte graphique rencontrée. PlayReady tente alors d’acquérir une licence pour un élément de contenu pour lequel PlayReady doit normalement déjà avoir une licence (autrement dit, en cas de gestion des droits numériques en fonction du logiciel ou dans toute situation n’impliquant pas de changement de matériel, PlayReady n’a pas besoin de réacquérir une licence). Par conséquent, si l’application acquiert une licence persistante lors de l’utilisation de la gestion des droits numériques en fonction du matériel, votre application doit pouvoir gérer le cas où cette licence est effectivement « perdue » si l’utilisateur final installe (ou désinstalle) une carte graphique. Comme ce n’est pas un scénario courant, vous pouvez décider de gérer les appels au support quand le contenu n’est plus lu après un changement de matériel plutôt que de déterminer comment traiter un changement de matériel dans le code client/serveur.

## <a name="override-hardware-drm"></a>Contourner la gestion des droits numériques en fonction du matériel

Cette section explique comment contourner la gestion des droits numériques en fonction du matériel si le contenu à lire ne la prend pas en charge.

Par défaut, la gestion des droits numériques en fonction du matériel est utilisée si le système la prend en charge. Cependant, certains contenus ne sont pas pris en charge par la gestion des droits numériques en fonction du matériel, par exemple le contenu Cocktail, tout contenu qui utilise un codec vidéo autre que H.264 et HEVC, ou encore le contenu HEVC, car certains types de gestion des droits numériques en fonction du matériel prennent en charge le codec HEVC et d’autres non. Par conséquent, si vous voulez lire un élément de contenu et que la gestion des droits numériques en fonction du matériel ne le prend pas en charge sur le système en question, vous voudrez peut-être désactiver la gestion des droits numériques en fonction du matériel.

L’exemple suivant explique comment désactiver la gestion des droits numériques en fonction du matériel. Vous devez uniquement effectuer cette action avant de procéder à la modification. Assurez-vous également que vous n’avez aucun objet PlayReady en mémoire. Sinon, le comportement n’est pas défini.

```js
var applicationData = Windows.Storage.ApplicationData.current;
var localSettings = applicationData.localSettings.createContainer("PlayReady", Windows.Storage.ApplicationDataCreateDisposition.always);
localSettings.values["SoftwareOverride"] = 1;
```

Pour revenir à la gestion des droits numériques en fonction du matériel, définissez la valeur **SoftwareOverride** sur **0**.

Pour chaque lecture multimédia, vous devez définir **MediaProtectionManager** sur :

```js
mediaProtectionManager.properties["Windows.Media.Protection.UseSoftwareProtectionLayer"] = true;
```

La meilleure façon de savoir si vous êtes dans le matériel DRM ou logiciel DRM consiste à examiner à C:\\utilisateurs\\&lt;nom d’utilisateur&gt;\\AppData\\Local\\Packages\\ &lt;nom de l’application&gt;\\LocalCache\\PlayReady\\\*

-   Si vous voyez un fichier mspr.hds, vous êtes en gestion des droits numériques en fonction du logiciel.
-   Si vous avez un autre \*.hds fichier, vous êtes dans le matériel DRM.
-   Vous pouvez aussi supprimer l’intégralité du dossier PlayReady et relancer votre test.

## <a name="detect-the-type-of-hardware-drm"></a>Détecter le type de gestion des droits numériques en fonction du matériel

Cette section explique comment détecter le type de gestion des droits numériques en fonction du matériel qui est pris en charge sur le système.

Vous pouvez utiliser la méthode [**PlayReadyStatics.CheckSupportedHardware**](https://msdn.microsoft.com/library/windows/apps/dn986441) pour déterminer si le système prend en charge une fonctionnalité spécifique de gestion des droits numériques (DRM) en fonction du matériel. Exemple :

```csharp
bool isFeatureSupported = PlayReadyStatics.CheckSupportedHardware(PlayReadyHardwareDRMFeatures.HEVC);
```

L’énumération [**PlayReadyHardwareDRMFeatures**](https://msdn.microsoft.com/library/windows/apps/dn986265) contient la liste valide des valeurs de fonctionnalité de gestion des droits numériques en fonction du matériel pouvant être interrogées. Pour déterminer si la gestion des droits numériques en fonction du matériel est prise en charge, utilisez le membre **HardwareDRM** dans la requête. Pour déterminer si le matériel prend en charge le codec HEVC (High Efficiency Video Coding)/H.265, utilisez le membre **HEVC** dans la requête.

Vous pouvez également utiliser la propriété [**PlayReadyStatics.PlayReadyCertificateSecurityLevel**](https://msdn.microsoft.com/library/windows/apps/windows.media.protection.playready.playreadystatics.playreadycertificatesecuritylevel.aspx) pour obtenir le niveau de sécurité du certificat client afin de déterminer si la gestion des droits numériques en fonction du matériel est prise en charge. À moins que le niveau de sécurité renvoyé pour le certificat soit supérieur ou égal à 3 000, le client n’est pas individualisé ou configuré (auquel cas cette propriété renvoie 0) ou la gestion des droits numériques en fonction du matériel n’est pas utilisée (auquel cas cette propriété renvoie une valeur inférieure à 3 000).

### <a name="detecting-support-for-aes128cbc-hardware-drm"></a>Détection de la prise en charge de la gestion des droits numériques en fonction du matériel AES128CBC
À partir de Windows 10, version 1709, vous pouvez détecter la prise en charge du chiffrement matériel AES128CBC sur un appareil en appelant **[PlayReadyStatics.CheckSupportedHardware](https://msdn.microsoft.com/library/windows/apps/dn986441)** et en spécifiant la valeur d’énumération [**PlayReadyHardwareDRMFeatures.Aes128Cbc**](https://msdn.microsoft.com/library/windows/apps/dn986265). Dans les versions précédentes de Windows 10, la définition de cette valeur entraîne la levée d’une exception. Pour cette raison, vous devez vérifier la présence de la valeur d’énumération en appelant **[ApiInformation.IsApiContractPresent](https://docs.microsoft.com/uwp/api/windows.foundation.metadata.apiinformation.isapicontractpresent)** et en spécifiant la version contractuelle principale 5 avant d’appeler **CheckSupportedHardware**.

```csharp
bool supportsAes128Cbc = ApiInformation.IsApiContractPresent("Windows.Foundation.UniversalApiContract", 5);

if (supportsAes128Cbc)
{
    supportsAes128Cbc = PlayReadyStatics.CheckSupportedHardware(PlayReadyHardwareDRMFeatures.Aes128Cbc);
}
```

## <a name="see-also"></a>Voir également
- [PlayReady DRM](playready-client-sdk.md)
