---
ms.assetid: DD8FFA8C-DFF0-41E3-8F7A-345C5A248FC2
description: Cette rubrique explique comment ajouter du contenu multimédia PlayReady protégé à votre application de plateforme Windows universelle (UWP).
title: Gestion des droits numériques PlayReady
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 40df746d89757b481a47cefeaeda3f8a34091014
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57636844"
---
# <a name="playready-drm"></a>Gestion des droits numériques PlayReady



Cette rubrique explique comment ajouter du contenu multimédia PlayReady protégé à votre application de plateforme Windows universelle (UWP).

La gestion des droits numériques par PlayReady permet aux développeurs de créer des applications pour UWP capables de fournir du contenu PlayReady à l’utilisateur tout en appliquant les règles d’accès définies par le fournisseur de contenu. Cette section décrit les modifications apportées à Microsoft PlayReady DRM pour Windows 10 et comment modifier votre application PlayReady UWP pour prendre en charge les modifications apportées à partir de la version précédente de Windows 8.1 vers la version de Windows 10.
 
| Rubrique                                                                     | Description                                                                                                                                                                                                                                                                             |
|---------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Matériel DRM](hardware-drm.md)                                           | Cette rubrique explique comment ajouter la gestion des droits numériques en fonction du matériel par PlayReady à votre application UWP.                                                                                                                                                                 |
| [Adaptive streaming avec PlayReady](adaptive-streaming-with-playready.md) | Cet article décrit comment ajouter le streaming adaptatif de contenu multimédia avec la protection de contenu Microsoft PlayReady à une application UWP. Cette fonctionnalité prend actuellement en charge la lecture de contenu vidéo en streaming HTTP (HLS) et de contenu en streaming dynamique sur HTTP (DASH). |

## <a name="whats-new-in-playready-drm"></a>Nouveautés de la gestion des droits numériques par PlayReady

La liste suivante décrit les nouvelles fonctionnalités et les modifications apportées à PlayReady DRM pour Windows 10.

-   Ajout de la gestion des droits numériques en fonction du matériel.

    La prise en charge de la protection du contenu en fonction du matériel permet la lecture sécurisée de contenu en haute définition (HD) et ultra haute définition (UHD) sur plusieurs plateformes d’appareils. Le matériel de clé (y compris les clés privées, les clés de contenu et tout autre matériel de clé utilisé pour dériver ou déverrouiller ces clés) et les échantillons vidéo compressés et non compressés déchiffrés sont protégés en tirant parti de la sécurité matérielle. Quand la gestion des droits numériques en fonction du matériel est utilisée, aucun des deux activateurs inconnus (lecture sur une sortie inconnue/lecture sur une sortie inconnue avec conversion descendante) n’a de pertinence, car le pipeline HWDRM connaît toujours la sortie utilisée. Pour plus d’informations, voir [Gestion des droits numériques en fonction du matériel](hardware-drm.md).

-   PlayReady n’est plus un composant d’infrastructure appX, mais un composant intégré au système d’exploitation. L’espace de noms **Microsoft.Media.PlayReadyClient** a été remplacé par [**Windows.Media.Protection.PlayReady**](https://msdn.microsoft.com/library/windows/apps/dn986454).
-   Les en-têtes suivants définissant les codes d’erreur PlayReady font désormais partie du Kit de développement logiciel (SDK) Windows : Windows.Media.Protection.PlayReadyErrors.h et Windows.Media.Protection.PlayReadyResults.h.
-   Acquisition proactive de licences non persistantes.

    Les versions précédentes de la gestion des droits numériques par PlayReady ne prenaient pas en charge l’acquisition proactive de licences non persistantes. Cette fonctionnalité a été ajoutée à cette version. Cela peut réduire le délai d’affichage de la première image. Pour plus d’informations, voir [Acquérir une licence non persistante de manière proactive avant la lecture](#proactively-acquire-a-non-persistent-license-before-playback).

-   Acquisition de plusieurs licences en un message.

    Permet à l’application cliente d’acquérir plusieurs licences non persistantes en un message d’acquisition de licence. Cela peut réduire le délai d’affichage de la première image grâce à l’acquisition de licences pour plusieurs éléments de contenu alors que l’utilisateur est encore en train de parcourir votre bibliothèque de contenu ; cela élimine tout délai d’attente pour l’acquisition de licence lorsque l’utilisateur sélectionne le contenu à lire. En outre, les flux audio et vidéo peuvent être chiffrés avec des clés distinctes à l’aide d’un en-tête de contenu qui inclut plusieurs identificateurs de clé (KID) ; cela permet d’acquérir les licences de l’ensemble des flux dans un fichier de contenu avec une seule demande d’acquisition de licence plutôt que de devoir utiliser une logique personnalisée et plusieurs demandes d’acquisition de licence pour parvenir au même résultat.

-   Ajout de la prise en charge du délai d’expiration en temps réel ou licence à durée limitée (LDL).

    Permet de définir l’expiration en temps réel des licences et de passer en toute fluidité d’une licence arrivant à expiration à une autre licence (valide) en cours de lecture. Associée à l’acquisition de plusieurs licences en un message, cette fonctionnalité permet à une application d’acquérir plusieurs licences LDL de façon asynchrone alors que l’utilisateur est encore en train de parcourir la bibliothèque de contenu et d’acquérir simplement une licence à plus longue durée une fois que l’utilisateur a sélectionné du contenu à lire. La lecture démarrera alors plus rapidement (car une licence est déjà disponible) et, comme l’application aura acquis une licence à plus longue durée au moment où la licence LDL expire, la lecture continuera en toute fluidité et sans interruption jusqu’à la fin du contenu.

-   Ajout de chaînes de licences non persistantes.
-   Ajout de la prise en charge de restrictions temporelles (y compris l’expiration, l’expiration après la première lecture et l’expiration en temps réel) sur les licences non persistantes.
-   Ajout de la prise en charge de la stratégie de protection HDCP de type 1 (version 2.2 sous Windows 10).

    Pour plus d’informations, voir [Points à prendre en considération](#things-to-consider).

-   Miracast est désormais une sortie implicite.
-   Ajout de l’arrêt sécurisé.

    L’arrêt sécurisé permet à un périphérique PlayReady de confirmer de manière sûre à un service de diffusion multimédia que la lecture multimédia a été stoppée sur un contenu donné. Cette fonctionnalité permet à vos services de streaming multimédia de contrôler et de rapporter avec précision les limites d’utilisation sur différents appareils pour un compte donné.

-   Ajout d’une séparation de licence audio et vidéo.

    Les pistes séparées empêchent la vidéo d’être décodée en tant qu’audio, offrant ainsi une protection plus sûre du contenu. Les normes émergentes nécessitent des touches distinctes pour les pistes audio et vidéo.

-   Ajout de MaxResDecode.

    Cette fonctionnalité a été ajoutée pour limiter la lecture de contenu à une résolution maximale et ce, même en cas de possession d’une clé plus compatible (mais pas d’une licence). Cela concerne également les cas où plusieurs tailles de flux sont codées avec une clé unique.

Les nouvelles interfaces, classes et énumérations suivantes ont été ajoutées à la gestion des droits numériques par PlayReady :

-   [**IPlayReadyLicenseAcquisitionServiceRequest** ](https://msdn.microsoft.com/library/windows/apps/dn986077) interface
-   [**IPlayReadyLicenseSession** ](https://msdn.microsoft.com/library/windows/apps/dn986080) interface
-   [**IPlayReadySecureStopServiceRequest** ](https://msdn.microsoft.com/library/windows/apps/dn986090) interface
-   [**PlayReadyLicenseSession** ](https://msdn.microsoft.com/library/windows/apps/dn986309) classe
-   [**PlayReadySecureStopIterable** ](https://msdn.microsoft.com/library/windows/apps/dn986371) classe
-   [**PlayReadySecureStopIterator** ](https://msdn.microsoft.com/library/windows/apps/dn986375) classe
-   [**PlayReadyHardwareDRMFeatures** ](https://msdn.microsoft.com/library/windows/apps/dn986265) énumérateur

Un nouvel exemple a été créé pour montrer comment utiliser les nouvelles fonctionnalités de la gestion des droits numériques par PlayReady. L’exemple peut être téléchargé à partir de [https://go.microsoft.com/fwlink/p/?linkid=331670&clcid=0x409](https://go.microsoft.com/fwlink/p/?linkid=331670).

## <a name="things-to-consider"></a>Points à prendre en considération

-   La gestion des droits numériques par PlayReady prend désormais en charge la protection HDCP de type 1 (prise en charge dans HDCP version 2.1 ou ultérieure). PlayReady exécute une stratégie de restriction de type HDCP dans la licence de l’appareil à appliquer. Sous Windows 10, la protection HDCP 2.2 ou ultérieure est enclenchée. Cette fonctionnalité peut être activée dans votre licence du Kit de développement de serveur PlayReady v3.0 (le serveur contrôle cette stratégie dans la licence à l’aide du GUID de restriction de type HDCP). Pour plus d’informations, voir les [Règles de conformité et de robustesse de PlayReady](https://www.microsoft.com/playready/licensing/compliance/).
-   Le format Windows Media Video (également connu sous le nom de VC-1) n’est pas pris en charge dans la gestion des droits numériques en fonction du matériel (voir [Contourner la gestion des droits numériques en fonction du matériel](hardware-drm.md#override-hardware-drm)).
-   La gestion des droits numériques par PlayReady prend désormais en charge la norme de compression vidéo HEVC (High Efficiency Video Coding)/H.265. Pour prendre en charge la norme HEVC, votre application doit utiliser du contenu CENC (Common Encryption Scheme) version 2, ce qui implique notamment de laisser les en-têtes de tranche en clair. Reportez-vous à la norme ISO/IEC 23001-7 Information technology--technologies de systèmes MPEG--partie 7 : Fichiers de format de fichier multimédia de base du chiffrement commun dans la norme ISO (version des spécifications ISO/IEC 23001-7:2015 ou ultérieure est nécessaire.) pour plus d’informations. Microsoft recommande également d’utiliser CENC version 2 pour tout le contenu HWDRM. En outre, certains types de gestion des droits numériques en fonction du matériel prennent en charge la norme HEVC et d’autres non (voir [Contourner la gestion des droits numériques en fonction du matériel](hardware-drm.md#override-hardware-drm)).
-   Pour tirer parti de certaines nouvelles fonctionnalités de PlayReady 3.0 (SL3000 pour les clients matériels, acquisition de plusieurs licences non persistantes en un message d’acquisition de licence, restrictions temporelles sur les licences non persistantes, etc.), le serveur PlayReady doit être la version commerciale du Kit de développement logiciel serveur Microsoft PlayReady v3.0.2769 ou version ultérieure.
-   Selon la stratégie de protection de sortie spécifiée dans la licence de contenu, la lecture multimédia peut échouer pour les utilisateurs finaux si leur sortie connectée ne prend pas en charge ces exigences. Le tableau suivant répertorie l’ensemble des erreurs courantes qui se produisent en conséquence. Pour plus d’informations, voir les [Règles de conformité et de robustesse de PlayReady](https://www.microsoft.com/playready/licensing/compliance/).

| Erreur                                                   | Valeur      | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|---------------------------------------------------------|------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ERREUR\_GRAPHICS\_OPM\_SORTIE\_EST\_PAS\_PRISE EN CHARGE\_HDCP  | 0xC0262513 | La stratégie de protection de contenu de la licence requiert que le moniteur enclenche la protection HDCP, mais celle-ci n’a pas pu être enclenchée.                                                                                                                                                                                                                                                                                                                                                                                              |
| MF\_E\_STRATÉGIE\_NON PRIS EN CHARGE                              | 0xC00D7159 | La stratégie de protection de contenu de la licence requiert que le moniteur enclenche la protection HDCP de type 1, mais celle-ci n’a pas pu être enclenchée.                                                                                                                                                                                                                                                                                                                                                                                |
| DRM\_E\_TEE\_SORTIE\_PROTECTION\_EXIGENCES\_PAS\_ÉCONOMIE | 0x8004CD22 | Ce code d’erreur survient uniquement lors de l’exécution en gestion des droits numériques en fonction du matériel. La stratégie de protection de contenu de la licence requiert que le moniteur enclenche la protection HDCP ou réduise la résolution effective du contenu, mais la protection HDCP n’a pas pu être enclenchée et la résolution effective du contenu n’a pas pu être réduite, car la gestion des droits numériques ne prend pas en charge la réduction de la résolution du contenu. En gestion des droits numériques en fonction du logiciel, le contenu est lu. Voir [Considérations pour l’utilisation de la gestion des droits numériques en fonction du matériel](hardware-drm.md#considerations-for-using-hardware-drm). |
| ERREUR\_GRAPHICS\_OPM\_PAS\_PRIS EN CHARGE                    | 0xc0262500 | Le pilote graphique ne prend pas en charge la protection de sortie. Par exemple, le moniteur est connecté via VGA ou aucun pilote graphique approprié pour la sortie numérique n’est installé. Dans ce dernier cas, le pilote standard qui est installé est celui de la carte vidéo de base Microsoft et l’installation d’un pilote graphique approprié résoudra le problème.                                                                                                                                                  |

## <a name="output-protection"></a>Protection de sortie

La section suivante décrit le comportement de l’interface utilisateur lors de l’utilisation de la gestion des droits numériques par PlayReady pour Windows 10 avec les stratégies de protection de sortie dans une licence PlayReady.

La gestion des droits numériques par PlayReady prend en charge les niveaux de protection de sortie contenus dans les **spécifications relatives aux droits d’utilisation du contenu multimédia extensible Microsoft PlayReady**. Ce document est disponible dans la documentation fournie avec les produits PlayReady sous licence.

> [!NOTE]
> Les valeurs autorisées pour les niveaux de protection de sortie qui peuvent être définis par un serveur de gestion de licences sont régies par les [règles de conformité PlayReady](https://www.microsoft.com/playready/licensing/compliance/).

La gestion des droits numériques par PlayReady permet de lire du contenu avec des stratégies de protection de sortie uniquement sur les connecteurs de sortie, comme indiqué dans les règles de conformité PlayReady. Pour plus d’informations sur les conditions relatives aux connecteurs de sortie spécifiées dans les règles de conformité PlayReady, reportez-vous aux [conditions définies pour les règles de conformité et de robustesse de PlayReady](https://www.microsoft.com/playready/licensing/compliance/).

Cette section est axée sur les scénarios de protection de sortie avec la gestion des droits numériques par PlayReady pour Windows 10 et la gestion des droits numériques en fonction du matériel par PlayReady pour Windows 10, également disponible sur certains clients Windows. Avec la gestion des droits numériques en fonction du matériel par PlayReady, toutes les protections de sortie sont appliquées à partir de l’implémentation TEE Windows (voir [Gestion des droits numériques en fonction du matériel](hardware-drm.md)). Par conséquent, certains comportements diffèrent de l’utilisation de la gestion des droits numériques en fonction du logiciel par PlayReady :

* Prise en charge pour le niveau de Protection de sortie (norme OPL) de la vidéo numérique non compressé 270 : HWDRM PlayReady pour Windows 10 ne prend pas en charge vers le bas de la résolution et appliquera que HDCP (haute-bandwidth Digital Content Protection) est activée. Il est recommandé que le contenu haute définition pour la gestion des droits numériques en fonction du matériel présente une norme OPL supérieure à 270 (bien que cela ne soit pas obligatoire). En outre, vous devez définir la restriction de type HDCP dans la licence (HDCP version 2.2 ou ultérieure).
* À la différence de la gestion des droits numériques en fonction du logiciel, la gestion des droits numériques en fonction du matériel permet d’appliquer les protections de sortie sur tous les moniteurs en fonction du moniteur le moins puissant. Par exemple, si l’utilisateur a deux moniteurs connectés dont un seul prend en charge la protection HDCP, la lecture échoue si la licence requiert la protection HDCP, même si le contenu est uniquement affiché sur l’écran qui prend en charge la protection HDCP. Avec la gestion des droits numériques en fonction du logiciel, le contenu est lu tant qu’il reste uniquement affiché sur le moniteur prenant en charge la protection HDCP.
* L’utilisation par le client et la sécurisation de la gestion des droits numériques en fonction du matériel n’est garantie qu’à condition que les conditions suivantes soient remplies par les clés et les licences de contenu :
    * La licence utilisée pour la clé de contenu vidéo doit présenter un niveau de sécurité minimal de 3000.
    * L’audio doit être chiffré selon une clé de contenu différente de celle de la vidéo, et la licence utilisée pour l’audio doit afficher un niveau de sécurité minimal de 2000. Par ailleurs, l’audio peut rester en clair.
* Tous les scénarios de gestion des droits numériques en fonction du logiciel nécessitent que le niveau de sécurité minimal de la licence PlayReady pour la clé de contenu audio ou vidéo soit inférieur ou égal à 2000.

### <a name="output-protection-levels"></a>Niveaux de protection de sortie

Le tableau suivant recense les mappages entre les différentes normes OPL dans la licence PlayReady et indique comment la gestion des droits numériques par PlayReady pour Windows 10 les applique.

#### <a name="video"></a>Video

<table>
    <tr>
        <th rowspan="2">OPL</th>
        <th>Vidéo numérique compressée</th>
        <th colspan="2">Vidéo numérique non compressée</th>
        <th>TV analogique</th>
    </tr>
    <tr>
        <th>Indéfini</th>
        <th colspan="2">HDMI, DVI, DisplayPort, MHL</th>
        <th>Composant, Composite</th>
    </tr>
    <tr>
        <th>100</th>
        <td rowspan="6">Non applicable\*</td>
        <td colspan="2">Transmet le contenu</td>
        <td>Transmet le contenu</td>
    </tr>
    <tr>
        <th>150</th>
        <td colspan="2" rowspan="2">Non applicable\*</td>
        <td>Transmet le contenu lorsque CGMS-A CopyNever est enclenché ou si CGMS-A ne peut pas être enclenché</td>
    </tr>
    <tr>
        <th>200</th>
        <td>Transmet le contenu lorsque CGMS-A CopyNever est enclenché</td>
    </tr>
    <tr>
        <th>250</th>
        <td colspan="2">Tente d’enclencher la protection HDCP, mais transmet le contenu quel que soit le résultat</td>
        <td rowspan="5">Non applicable\*</td>
    </tr>
    <tr>
        <th>270</th>
        <td><b>SWDRM</b>: Tente de s’engager HDCP. Si la protection HDCP ne s’enclenche pas, le PC restreint la résolution effective à 520 000 pixels par image et transmet le contenu</td>
        <td><b>HWDRM</b>: Transmet le contenu HDCP. Si la protection HDCP n’est pas enclenchée, la lecture vers les ports HDMI/DVI est bloquée</td>
    </tr>
    <tr>
        <th>300</th>
        <td colspan="2">
            <p>
                **Lorsque la restriction de type HDCP n’est pas définie :** Transmet le contenu HDCP. Si la protection HDCP n’est pas enclenchée, la lecture vers les ports HDMI/DVI est bloquée.
            </p>
            <p>
                **Lorsque la restriction de type HDCP est définie**: Contenu de passes avec HDCP 2.2 et le type de flux de contenu défini sur 1. Si la protection HDCP ne s’enclenche pas ou si le type de flux de contenu ne peut pas être défini sur 1, la lecture vers les ports HDMI/DVI est bloquée.
            </p>
        </td>
    </tr>
    <tr>
        <th>400</th>
        <td rowspan="2">Windows 10 ne transmet jamais le contenu vidéo numérique compressé vers les sorties, quelle que soit la valeur OPL ultérieure. Pour en savoir plus sur le contenu vidéo numérique compressé, reportez-vous aux <a href="https://www.microsoft.com/playready/licensing/compliance/">Règles de conformité pour les produits PlayReady</a>.</td>
        <td colspan="2" rowspan="2">Non applicable\*</td>
    </tr>
    <tr>
        <th>500</th>
    </tr>
</table>
<br/>

\* Toutes les valeurs des niveaux de protection de sortie peuvent être définies par un serveur de licences. Pour en savoir plus, reportez-vous aux [Règles de conformité et de robustesse de PlayReady](https://www.microsoft.com/playready/licensing/compliance/).

#### <a name="audio"></a>Audio

<table>
    <tr>
        <th rowspan="2">OPL</th>
        <th>Audio numérique compressé</th>
        <th>Audio numérique non compressé</th>
        <th>Audio analogique ou USB</th>
    </tr>
    <tr>
        <th>HDMI, DisplayPort, MHL</th>
        <th>HDMI, DisplayPort, MHL</th>
        <th>Indéfini</th>
    </tr>
    <tr>
        <th>100</th>
        <td rowspan="3">Transmet le contenu</td>
        <td>Transmet le contenu</td>
        <td rowspan="5">Transmet le contenu</td>
    </tr>
    <tr>
        <th>150</th>
        <td rowspan="4">NE transmet PAS le contenu</td>
    </tr>
    <tr>
        <th>200</th>
    </tr>
    <tr>
        <th>250</th>
        <td>Transmet le contenu lorsque la protection HDCP est enclenchée sur HDMI, DisplayPort ou MHL, ou lorsque la protection SCMS est enclenchée et définie sur CopyNever</td>
    </tr>
    <tr>
        <th>300</th>
        <td>Transmet le contenu lorsque la protection HDCP est enclenchée sur HDMI, DisplayPort ou MHL</td>
    </tr>
</table>
<br/>

### <a name="miracast"></a>Miracast

La gestion des droits numériques par PlayReady vous permet de lire le contenu sur la sortie Miracast dès que la protection HDCP 2.0 ou ultérieure est enclenchée. Cependant, sous Windows 10, Miracast est considérée comme une sortie *numérique*. Pour plus d’informations sur les scénarios Miracast, reportez-vous aux [règles de conformité PlayReady](https://www.microsoft.com/playready/licensing/compliance/). Le tableau suivant recense les mappages entre les différentes normes OPL dans la licence PlayReady et indique comment la gestion des droits numériques par PlayReady les applique sur les sorties Miracast.

<table>
    <tr>
        <th>OPL</th>
        <th>Audio numérique compressé</th>
        <th>Audio numérique non compressé</th>
        <th>Vidéo numérique compressée</th>
        <th>Vidéo numérique non compressée</th>
    </tr>
    <tr>
        <th>100</th>
        <td rowspan="4">Transmet le contenu lorsque la protection HDCP 2.0 ou plus est enclenchée. Si l’enclenchement échoue, le contenu N’EST PAS transmis</td>
        <td>Transmet le contenu lorsque la protection HDCP 2.0 ou plus est enclenchée. Si l’enclenchement échoue, le contenu N’EST PAS transmis</td>
        <td rowspan="6">Non applicable\*</td>
        <td>Transmet le contenu lorsque la protection HDCP 2.0 ou plus est enclenchée. Si l’enclenchement échoue, le contenu N’EST PAS transmis</td>
    </tr>
    <tr>
        <th>150</th>
        <td rowspan="3">NE transmet PAS le contenu</td>
        <td rowspan="2">Non applicable\*</td>
    </tr>
    <tr>
        <th>200</th>
    </tr>
    <tr>
        <th>250</th>
        <td rowspan="2">Transmet le contenu lorsque la protection HDCP 2.0 ou plus est enclenchée. Si l’enclenchement échoue, le contenu N’EST PAS transmis</td>
    </tr>
    <tr>
        <th>270</th>
        <td colspan="2">Non applicable\*</td>
    </tr>
    <tr>
        <th>300</th>
        <td>Transmet le contenu lorsque la protection HDCP 2.0 ou plus est enclenchée. Si l’enclenchement échoue, le contenu N’EST PAS transmis</td>
        <td>NE transmet PAS le contenu</td>
        <td>
            <p>
                **Lorsque la restriction de type HDCP n’est pas définie :** Transmet le contenu lorsque la protection HDCP 2.0 ou plus est enclenchée. Si l’enclenchement échoue, le contenu N’EST PAS transmis.
            </p>
            <p>
                **Lorsque la restriction de type HDCP est définie :** Contenu de passes avec HDCP 2.2 et le type de flux de contenu défini sur 1. Si la protection HDCP n’est pas enclenchée ou si le type de flux de contenu ne peut pas être défini sur 1, le contenu N’EST PAS transmis
            </p>        
        </td>
    </tr>
    <tr>
        <th>400</th>
        <td rowspan="2" colspan="2">Non applicable\*</td>
        <td rowspan="2">Windows 10 ne transmet jamais le contenu vidéo numérique compressé vers les sorties, quelle que soit la valeur OPL ultérieure. Pour en savoir plus sur le contenu vidéo numérique compressé, reportez-vous aux <a href="https://www.microsoft.com/playready/licensing/compliance/">Règles de conformité pour les produits PlayReady</a>.</td>
        <td rowspan="2">Non applicable\*</td>
    </tr>
    <tr>
        <th>500</th>
    </tr>
</table>
<br/>

\* Toutes les valeurs des niveaux de protection de sortie peuvent être définies par un serveur de licences. Pour en savoir plus, reportez-vous aux [Règles de conformité et de robustesse de PlayReady](https://www.microsoft.com/playready/licensing/compliance/).

### <a name="additional-explicit-output-restrictions"></a>Restrictions de sortie explicite supplémentaires

Le tableau suivant décrit la gestion des droits numériques par PlayReady pour l’implémentation sous Windows 10 des restrictions de protection de sortie vidéo numérique explicite.

<table>
    <tr>
        <th>Scénario</th>
        <th>GUID</th>
        <th>Si…</th>
        <th>Alors…</th>
    </tr>
    <tr>
        <th>Taille de décodage de résolution effective maximale</th>
        <td>9645E831-E01D-4FFF-8342-0A720E3E028F</td>
        <td>La sortie connectée est : une sortie vidéo numérique, Miracast, HDMI, DVI, etc.</td>
        <td>
            <p>
                Transmet le contenu lorsque :  
            </p>
            <ul>
                <li>(a) la largeur de l’image doit être inférieure ou égale à la largeur maximale de l’image en pixels, et la hauteur de l’image doit être inférieure ou égale à la hauteur maximale de l’image en pixels, ou</li>
                <li>(b) la hauteur de l’image doit être inférieure ou égale à la largeur maximale de l’image en pixels, et la largeur de l’image doit être inférieure ou égale à la hauteur maximale de l’image en pixels</li>
            </ul>                   
        </td>
    </tr>
    <tr>
        <th>Restriction de type de protection HDCP</th>
        <td>ABB2C6F1-E663-4625-A945-972D17B231E7</td>
        <td>La sortie connectée est : une sortie vidéo numérique, Miracast, HDMI, DVI, etc.</td>
        <td>Transmet le contenu avec la protection HDCP 2.2 et le type de flux de contenu défini sur 1. Si la protection HDCP 2.2 ne s’enclenche pas ou si le type de flux de contenu ne peut pas être défini sur 1, le contenu N’EST PAS transmis. Un niveau de protection de sortie vidéo numérique non compressée d’une valeur supérieure ou égale à 271 doit également être indiqué</td>
    </tr>
</table>
<br/>

Le tableau suivant décrit la gestion des droits numériques par PlayReady pour l’implémentation sous Windows 10 des restrictions de protection de sortie vidéo analogique explicite.

<table>
    <tr>
        <th>Scénario</th>
        <th>GUID</th>
        <th>Si…</th>
        <th colspan="2">Alors…</th>
    </tr>
    <tr>
        <th>Écran d’ordinateur analogique</th>
        <td>D783A191-E083-4BAF-B2DA-E69F910B3772</td>
        <td>Sortie connecté est : VGA, DVI&ndash;analogique, etc.</td>
        <td><b>SWDRM :</b> PC sera contraindre la résolution effective à epx ses 520 000 clients par trame et passez le contenu</td>
        <td><b>HWDRM :</b> NE transmet PAS le contenu</td>
    </tr>
    <tr>
        <th>Composant analogique</th>
        <td>811C5110-46C8-4C6E-8163-C0482A15D47E</td>
        <td>La sortie connectée est : composant</td>
        <td><b>SWDRM :</b> PC sera contraindre la résolution effective à epx ses 520 000 clients par trame et passez le contenu</td>
        <td><b>HWDRM :</b> NE transmet PAS le contenu</td>
    </tr>
    <tr>
        <th rowspan="2">Sorties TV analogiques</th>
        <td>2098DE8D-7DDD-4BAB-96C6-32EBB6FABEA3</td>
        <td>La norme OPL TV analogique est inférieure à 151</td>
        <td colspan="2">CGMS-A doit être enclenché</td>
    </tr>
    <tr>
        <td>225CD36F-F132-49EF-BA8C-C91EA28E4369</td>
        <td>La norme OPL TV analogique est inférieure à 101 et la licence ne contient pas 2098DE8D-7DDD-4BAB-96C6-32EBB6FABEA3</td>
        <td colspan="2">Une tentative doit être faite pour enclencher CGMS-A, mais le contenu peut être lu, quel que soit le résultat</td>
    </tr>
    <tr>
        <th>Contrôle de gain automatique et bande de couleur</th>
        <td>C3FD11C6-F8B7-4D20-B008-1DB17D61F2DA</td>
        <td>Transmission de contenu avec résolution inférieure ou égale à 520 000 pixels sur sortie TV analogique</td>
        <td colspan="2">Définit le contrôle de gain automatique uniquement pour le composant vidéo et le mode PAL lorsque la résolution est inférieure à 520 000 pixels, et définit le contrôle de gain automatique ainsi que les informations de couleur pour NTSC lorsque la résolution est inférieure à 520 000 pixels, conformément au tableau 3.5.7.3. Dans les règles de conformité</td>
    </tr>
    <tr>
        <th>Sortie numérique uniquement</th>
        <td>760AE755-682A-41E0-B1B3-DCDF836A7306</td>
        <td>La sortie connectée est analogique</td>
        <td colspan="2">Ne transmet pas le contenu</td>
    </tr>
</table>
<br/>

> [!NOTE]
> Si vous utilisez un dongle d’adaptation tel que « Mini DisplayPort to VGA » pour la lecture, Windows 10 considère la sortie comme une sortie vidéo numérique, il ne peut donc pas appliquer les stratégies de vidéo analogique.

Le tableau suivant décrit la gestion des droits numériques par PlayReady pour l’implémentation de Windows 10 qui permet la lecture dans d’autres circonstances.

<table>
    <tr>
        <th>Scénario</th>
        <th>GUID</th>
        <th>Si…</th>
        <th colspan="2">Alors…</th>
    </tr>
    <tr>
        <th>Sortie inconnue</th>
        <td>786627D8-C2A6-44BE-8F88-08AE255B01A7</td>
        <td>Si la sortie ne peut pas être raisonnablement déterminée, ou si la prise en charge d’OPM ne peut pas être établie avec le pilote de graphiques</td>
        <td><b>SWDRM :</b> Transmet le contenu</td>
        <td><b>HWDRM :</b> NE transmet PAS le contenu</td>
    </tr>
    <tr>
        <th>Sortie inconnue avec limitation</th>
        <td>B621D91F-EDCC-4035-8D4B-DC71760D43E9</td>
        <td>Si la sortie ne peut pas être raisonnablement déterminée, ou si la prise en charge d’OPM ne peut pas être établie avec le pilote de graphiques</td>
        <td><b>SWDRM :</b> PC sera contraindre la résolution effective à epx ses 520 000 clients par trame et passez le contenu</td>
        <td><b>HWDRM :</b> NE transmet PAS le contenu</td>
    </tr>
</table>
<br/>

## <a name="prerequisites"></a>Conditions préalables

Avant de commencer la création de votre application pour UWP protégée par PlayReady, le logiciel suivant doit être installé sur votre système :

-   Windows 10.
-   Si vous compilez les exemples de PlayReady DRM pour les applications UWP, vous devez utiliser Microsoft Visual Studio 2015 ou version ultérieure pour compiler les exemples. Vous pouvez toujours utiliser Microsoft Visual Studio 2013 pour compiler les exemples à partir de la DRM PlayReady pour les applications de Store Windows 8.1.

<!--This is no longer available-->
<!--If you are planning to play back MPEG-2/H.262 content on your app, you must also download and install [Windows 8.1 Media Center Pack](https://go.microsoft.com/fwlink/p/?LinkId=626876).-->

## <a name="playready-uwp-app-migration-guide"></a>Guide de migration des applications UWP PlayReady

Cette section inclut des informations sur la façon de migrer vos applications de Store PlayReady Windows 8.x existantes vers Windows 10.

L’espace de noms pour les applications PlayReady UWP sur Windows 10 a été remplacé par **Microsoft.Media.PlayReadyClient** à [ **Windows.Media.Protection.PlayReady**](https://msdn.microsoft.com/library/windows/apps/dn986454). Cela signifie que vous devez rechercher l’ancien espace de noms et le remplacer par le nouvel espace de noms dans votre code. Vous devrez toujours référencer un fichier winmd. Il fait partie de windows.media.winmd sur le système d’exploitation Windows 10. Il se trouve sous windows.winmd et fait partie du kit de développement logiciel (SDK) Windows. Pour la plateforme Windows universelle, il est référencé sous windows.foundation.univeralappcontract.winmd.

Pour lire du contenu haute définition (1080p) et ultra haute définition (UHD) protégé par PlayReady, vous devez implémenter la gestion des droits numériques en fonction du matériel par PlayReady. Pour plus d’informations sur l’implémentation de la gestion des droits numériques en fonction du matériel par PlayReady, voir [Gestion des droits numériques en fonction du matériel](hardware-drm.md).

Certains contenus ne sont pas pris en charge par la gestion des droits numériques en fonction du matériel. Pour plus d’informations sur la désactivation de la gestion des droits numériques en fonction du matériel et l’activation de la gestion des droits numériques en fonction du logiciel, voir [Contourner la gestion des droits numériques en fonction du matériel](hardware-drm.md#override-hardware-drm).

Pour le Gestionnaire de protection multimédia, assurez-vous que votre code comporte les paramètres suivants, si ce n’est pas déjà le cas :

```cs
var mediaProtectionManager = new Windows.Media.Protection.MediaProtectionManager();

mediaProtectionManager.Properties["Windows.Media.Protection.MediaProtectionSystemId"] = 
             '{F4637010-03C3-42CD-B932-B48ADF3A6A54}'
var cpsystems = new Windows.Foundation.Collections.PropertySet();
cpsystems["{F4637010-03C3-42CD-B932-B48ADF3A6A54}"] = 
                "Windows.Media.Protection.PlayReady.PlayReadyWinRTTrustedInput";
mediaProtectionManager.Properties["Windows.Media.Protection.MediaProtectionSystemIdMapping"] = cpsystems;

mediaProtectionManager.Properties["Windows.Media.Protection.MediaProtectionContainerGuid"] = 
                "{9A04F079-9840-4286-AB92-E65BE0885F95}";
```

## <a name="proactively-acquire-a-non-persistent-license-before-playback"></a>Acquérir une licence non persistante de manière proactive avant la lecture

Cette section explique comment acquérir des licences non persistantes de manière proactive avant le début de la lecture.

Dans les versions précédentes de la gestion des droits numériques par PlayReady, les licences non persistantes pouvaient uniquement être acquises de manière réactive, pendant la lecture. Dans cette version, vous pouvez acquérir des licences non persistantes de manière proactive avant le début de la lecture.

1.  Créez une session de lecture de manière proactive afin de stocker la licence non persistante. Exemple :

    ```cs
    var cpsystems = new Windows.Foundation.Collections.PropertySet();       
    cpsystems["{F4637010-03C3-42CD-B932-B48ADF3A6A54}"] = "Windows.Media.Protection.PlayReady.PlayReadyWinRTTrustedInput"; // PlayReady

    var pmpSystemInfo = new Windows.Foundation.Collections.PropertySet();
    pmpSystemInfo["Windows.Media.Protection.MediaProtectionSystemId"] = "{F4637010-03C3-42CD-B932-B48ADF3A6A54}";
    pmpSystemInfo["Windows.Media.Protection.MediaProtectionSystemIdMapping"] = cpsystems;
    var pmpServer = new Windows.Media.Protection.MediaProtectionPMPServer( pmpSystemInfo );
    ```

2.  Liez cette session de lecture à la classe d’acquisition de licence. Exemple :

    ```cs
    var licenseSessionProperties = new Windows.Foundation.Collections.PropertySet();
    licenseSessionProperties["Windows.Media.Protection.MediaProtectionPMPServer"] = pmpServer;
    var licenseSession = new Windows.Media.Protection.PlayReady.PlayReadyLicenseSession( licenseSessionProperties );
    ```

3.  Créez une demande de service de licence. Exemple :

    ```cs
    var laSR = licenseSession.CreateLAServiceRequest();
    ```

4.  Procédez à l’acquisition de licence en utilisant la demande de service créée à l’étape 3. La licence sera stockée dans la session de lecture.
5.  Liez la session de lecture à la source de média pour la lecture. Exemple :

    ```cs
    licenseSession.configureMediaProtectionManager( mediaProtectionManager );
    videoPlayer.msSetMediaProtectionManager( mediaProtectionManager );
    ```
    
## <a name="query-for-protection-capabilities"></a>Requête pour les fonctionnalités de protection
À partir de Windows 10, version 1703, vous pouvez interroger les fonctionnalités de gestion des droits numériques en fonction du matériel, telles que le décodage des codecs, la résolution et les protections de sortie (HDCP). Les requêtes sont effectuées avec la méthode [**IsTypeSupported**](https://docs.microsoft.com/uwp/api/windows.media.protection.protectioncapabilities.istypesupported) qui utilise une chaîne représentant les fonctionnalités pour lesquelles la prise en charge est interrogée et une chaîne spécifiant le système de clés auquel la requête s'applique. Pour obtenir la liste des valeurs de chaîne prises en charge, consultez la page de référence des API pour [**IsTypeSupported**](https://docs.microsoft.com/uwp/api/windows.media.protection.protectioncapabilities.istypesupported). L'exemple de code suivant illustre l'utilisation de cette méthode.  

    ```cs
    using namespace Windows::Media::Protection;

    ProtectionCapabilities^ sr = ref new ProtectionCapabilities();

    ProtectionCapabilityResult result = sr->IsTypeSupported(
    L"video/mp4; codecs=\"avc1.640028\"; features=\"decode-bpp=10,decode-fps=29.97,decode-res-x=1920,decode-res-y=1080\"",
    L"com.microsoft.playready");

    switch (result)
    {
        case ProtectionCapabilityResult::Probably:
        // Queue up UHD HW DRM video
        break;

        case ProtectionCapabilityResult::Maybe:
        // Check again after UI or poll for more info.
        break;

        case ProtectionCapabilityResult::NotSupported:
        // Do not queue up UHD HW DRM video.
        break;
    }
    ```
## <a name="add-secure-stop"></a>Ajouter un arrêt sécurisé

Cette section explique comment ajouter un arrêt sécurisé à votre application pour UWP.

L’arrêt sécurisé permet à un périphérique PlayReady de confirmer de manière sûre à un service de diffusion multimédia que la lecture multimédia a été stoppée sur un contenu donné. Cette fonctionnalité permet à vos services de streaming multimédia de contrôler et de rapporter avec précision les limites d’utilisation sur différents appareils pour un compte donné.

Il existe deux scénarios principaux pour l’envoi d’une demande d’arrêt sécurisé :

-   Lorsque la présentation multimédia s’arrête car la fin du contenu a été atteinte ou lorsque l’utilisateur a arrêté la présentation multimédia en cours de lecture.
-   Lorsque la session précédente se termine inopinément (par exemple, en raison d’un blocage du système ou de l’application). L’application devra contrôler, au moment du démarrage ou de l’arrêt, s’il y a des sessions d’arrêt sécurisé en attente et envoyer les demandes séparément de la lecture de tout autre contenu multimédia.

Pour obtenir un exemple d’implémentation de l’arrêt sécurisé, consultez le fichier securestop.cs dans l’exemple PlayReady disponible à l’adresse [https://go.microsoft.com/fwlink/p/?linkid=331670&clcid=0x409](https://go.microsoft.com/fwlink/p/?linkid=331670).

## <a name="use-playready-drm-on-xbox-one"></a>Utiliser la gestion des droits numériques par PlayReady sur Xbox One

Pour utiliser PlayReady DRM dans une application UWP sur Xbox One, vous devez d’abord inscrire votre [partenaires](https://partner.microsoft.com/dashboard) compte que vous utilisez pour publier l’application pour l’autorisation d’utiliser PlayReady. Vous pouvez le faire de deux manières :

* Demander à votre interlocuteur Microsoft de faire le nécessaire pour obtenir l’autorisation.
* Appliquer pour l’autorisation en envoyant votre nom de compte et d’entreprise de partenaires à [ pronxbox@microsoft.com ](mailto:pronxbox@microsoft.com).

Une fois que vous recevez d’autorisation, vous devrez ajouter un objet `<DeviceCapability>`  supplémentaire au manifeste de l’application. Vous devez l’ajouter manuellement, car aucun paramètre n’est actuellement disponible dans le concepteur de manifeste d’application. Pour le configurer, procédez comme suit :

1. Le projet étant ouvert dans Visual Studio, ouvrez l’**Explorateur de solutions** et cliquez avec le bouton droit sur **Package.appxmanifest**.
2. Sélectionnez **Ouvrir avec…**, choisissez **Éditeur XML (Texte)**, puis cliquez sur **OK**.
3. Entre les balises `<Capabilities>`, ajoutez le `<DeviceCapability>` suivant :

    ```xml
    <DeviceCapability Name="6a7e5907-885c-4bcb-b40a-073c067bd3d5" />
    ```

4. Enregistrez le fichier.

Vous devez tenir compte d’un dernier aspect lors de l’utilisation de PlayReady sur Xbox One : les kits de développement sont limités au contenu SL150 (autrement dit, ils ne peuvent pas lire du contenu SL2000 ou SL3000). Les appareils du commerce peuvent lire du contenu associé à des niveaux de sécurité plus élevés, mais pour tester votre application sur un kit de développement, vous devrez utiliser du contenu SL150. Vous pouvez tester ce contenu de l’une des manières suivantes :

* Utiliser un contenu de test surveillé qui requiert des licences SL150.
* Implémentez la logique afin que seuls certains comptes de test authentifiés soient en mesure d’acquérir des licences SL150 pour certains contenus.

Utilisez l’approche qui convient pour votre produit et pour votre entreprise.


## <a name="see-also"></a>Voir également
- [Lecture de contenu multimédia](media-playback.md)




