---
ms.assetid: DD8FFA8C-DFF0-41E3-8F7A-345C5A248FC2
Cette rubrique explique comment ajouter du contenu multimédia PlayReady protégé à votre application de plateforme Windows universelle (UWP).
Gestion des droits numériques par PlayReady
---

# Gestion des droits numériques par PlayReady

\[ Mise à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]


Cette rubrique explique comment ajouter du contenu multimédia PlayReady protégé à votre application de plateforme Windows universelle (UWP).

La gestion des droits numériques par PlayReady permet aux développeurs de créer des applications pour UWP capables de fournir du contenu PlayReady à l’utilisateur tout en appliquant les règles d’accès définies par le fournisseur de contenu. Cette section décrit les modifications apportées à la gestion des droits numériques par PlayReady pour Windows 10 et explique comment modifier votre application UWP PlayReady pour prendre en charge les modifications apportées entre la version Windows 8.1 précédente et la version Windows 10.
 
| Rubrique                                                                     | Description                                                                                                                                                                                                                                                                             |
|---------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Gestion des droits numériques en fonction du matériel](hardware-drm.md)                                           | Cette rubrique explique comment ajouter la gestion des droits numériques en fonction du matériel par PlayReady à votre application du UWP.                                                                                                                                                                 |
| [Diffusion en continu adaptative avec PlayReady](adaptive-streaming-with-playready.md) | Cet article décrit comment ajouter la diffusion en continu adaptative de contenu multimédia avec la protection de contenu Microsoft PlayReady à une application UWP. Cette fonctionnalité prend actuellement en charge la lecture de contenu vidéo en flux continu HTTP (HLS) et de contenu à diffusion en continu dynamique sur HTTP (DASH). |

## Nouveautés de la gestion des droits numériques par PlayReady

La liste suivante décrit les nouvelles fonctionnalités et les modifications apportées à la gestion des droits numériques par PlayReady pour Windows 10.

-   Ajout de la gestion des droits numériques (DRM) en fonction du matériel.

    La prise en charge de la protection du contenu en fonction du matériel permet la lecture sécurisée de contenu en haute définition (HD) et ultra haute définition (UHD) sur plusieurs plateformes d’appareils. Le matériel de clé (y compris les clés privées, les clés de contenu et tout autre matériel de clé utilisé pour dériver ou déverrouiller ces clés) et les échantillons vidéo compressés et non compressés déchiffrés sont protégés en tirant parti de la sécurité matérielle. Quand la gestion des droits numériques en fonction du matériel est utilisée, aucun des deux activateurs inconnus (lecture sur une sortie inconnue/lecture sur une sortie inconnue avec conversion descendante) n’a de pertinence, car le pipeline HWDRM connaît toujours la sortie utilisée. Pour plus d’informations, voir [Gestion des droits numériques en fonction du matériel](hardware-drm.md).

-   PlayReady n’est plus un composant d’infrastructure appX, mais un composant intégré au système d’exploitation. L’espace de noms **Microsoft.Media.PlayReadyClient** a été remplacé par [**Windows.Media.Protection.PlayReady**](https://msdn.microsoft.com/library/windows/apps/dn986454).
-   Les en-têtes suivants définissant les codes d’erreur PlayReady font désormais partie du Kit de développement logiciel (SDK) Windows : Windows.Media.Protection.PlayReadyErrors.h et Windows.Media.Protection.PlayReadyResults.h.
-   Acquisition proactive de licences non persistantes.

    Les versions précédentes de la gestion des droits numériques par PlayReady ne prenaient pas en charge l’acquisition proactive de licences non persistantes. Cette fonctionnalité a été ajoutée à cette version. Cela peut réduire le délai d’affichage de la première image. Pour plus d’informations, voir [Acquérir une licence non persistante de manière proactive avant la lecture](#proactively_acquire_a_non_persistent_license_before_playback).

-   Acquisition de plusieurs licences en un message.

    Permet à l’application cliente d’acquérir plusieurs licences non persistantes en un message d’acquisition de licence. Cela peut réduire le délai d’affichage de la première image grâce à l’acquisition de licences pour plusieurs éléments de contenu alors que l’utilisateur est encore en train de parcourir votre bibliothèque de contenu ; cela élimine tout délai d’attente pour l’acquisition de licence lorsque l’utilisateur sélectionne le contenu à lire. En outre, les flux audio et vidéo peuvent être chiffrés avec des clés distinctes à l’aide d’un en-tête de contenu qui inclut plusieurs identificateurs de clé (KID) ; cela permet d’acquérir les licences de l’ensemble des flux dans un fichier de contenu avec une seule demande d’acquisition de licence plutôt que de devoir utiliser une logique personnalisée et plusieurs demandes d’acquisition de licence pour parvenir au même résultat.

-   Ajout de la prise en charge du délai d’expiration en temps réel ou licence à durée limitée (LDL).

    Permet de définir l’expiration en temps réel des licences et de passer en toute fluidité d’une licence arrivant à expiration à une autre licence (valide) en cours de lecture. Associée à l’acquisition de plusieurs licences en un message, cette fonctionnalité permet à une application d’acquérir plusieurs licences LDL de façon asynchrone alors que l’utilisateur est encore en train de parcourir la bibliothèque de contenu et d’acquérir simplement une licence à plus longue durée une fois que l’utilisateur a sélectionné du contenu à lire. La lecture démarrera alors plus rapidement (car une licence est déjà disponible) et, comme l’application aura acquis une licence à plus longue durée au moment où la licence LDL expire, la lecture continuera en toute fluidité et sans interruption jusqu’à la fin du contenu.

-   Ajout de chaînes de licences non persistantes.
-   Ajout de la prise en charge de restrictions temporelles (y compris l’expiration, l’expiration après la première lecture et l’expiration en temps réel) sur les licences non persistantes.
-   Ajout de la prise en charge de la stratégie de protection HDCP de type 1 (version 2.2).

    Pour plus d’informations, voir [Points à prendre en considération](#things_to_consider).

-   Miracast est désormais une sortie implicite.
-   Ajout de l’arrêt sécurisé.

    L’arrêt sécurisé permet à un périphérique PlayReady de confirmer de manière sûre à un service de diffusion multimédia que la lecture multimédia a été stoppée sur un contenu donné. Cette fonctionnalité permet à vos services de diffusion multimédia de contrôler et de rapporter avec précision les limites d’utilisation sur différents appareils pour un compte donné.

-   Ajout d’une séparation de licence audio et vidéo.

    Les pistes séparées empêchent la vidéo d’être décodée en tant qu’audio, offrant ainsi une protection plus sûre du contenu. Les normes émergentes nécessitent des touches distinctes pour les pistes audio et vidéo.

-   Ajout de MaxResDecode.

    Cette fonctionnalité a été ajoutée pour limiter la lecture de contenu à une résolution maximale et ce, même en cas de possession d’une clé plus compatible (mais pas d’une licence). Cela concerne également les cas où plusieurs tailles de flux sont codées avec une clé unique.

Les nouvelles interfaces, classes et énumérations suivantes ont été ajoutées à la gestion des droits numériques par PlayReady :

-   Interface [**IPlayReadyLicenseAcquisitionServiceRequest**](https://msdn.microsoft.com/library/windows/apps/dn986077)
-   Interface [**IPlayReadyLicenseSession**](https://msdn.microsoft.com/library/windows/apps/dn986080)
-   Interface [**IPlayReadySecureStopServiceRequest**](https://msdn.microsoft.com/library/windows/apps/dn986090)
-   Classe [**PlayReadyLicenseSession**](https://msdn.microsoft.com/library/windows/apps/dn986309)
-   Classe [**PlayReadySecureStopIterable**](https://msdn.microsoft.com/library/windows/apps/dn986371)
-   Classe [**PlayReadySecureStopIterator**](https://msdn.microsoft.com/library/windows/apps/dn986375)
-   Énumérateur [**PlayReadyHardwareDRMFeatures**](https://msdn.microsoft.com/library/windows/apps/dn986265)

Un nouvel exemple a été créé pour montrer comment utiliser les nouvelles fonctionnalités de la gestion des droits numériques par PlayReady. Cet exemple peut être téléchargé à l’adresse [http://go.microsoft.com/fwlink/p/?linkid=331670&clcid=0x409](http://go.microsoft.com/fwlink/p/?linkid=331670).

## Points à prendre en considération

-   La gestion des droits numériques par PlayReady prend désormais en charge la protection HDCP de type 1 (version 2.2 ou ultérieure). PlayReady exécute la stratégie de la licence de l’appareil à appliquer. Cette fonctionnalité peut être activée dans votre licence de SDK serveur PlayReady v3.0 (le serveur contrôle cette stratégie dans la licence à l’aide du **GUID** de l’activateur de lecture). Pour plus d’informations, voir les [Règles de conformité et de robustesse de PlayReady](http://www.microsoft.com/playready/licensing/compliance/).
-   Le format Windows Media Video (également connu sous le nom de VC-1) n’est pas pris en charge dans la gestion des droits numériques en fonction du matériel (voir [Contourner la gestion des droits numériques en fonction du matériel](hardware-drm.md#override-hardware-drm)).
-   La gestion des droits numériques par PlayReady prend désormais en charge la norme de compression vidéo HEVC (High Efficiency Video Coding)/H.265. Pour prendre en charge la norme HEVC, votre application doit utiliser du contenu CENC (Common Encryption Scheme) version 2, ce qui implique notamment de laisser les en-têtes de tranche en clair. Pour plus d’informations, reportez-vous à la norme ISO/IEC 23001-7 Technologies de l’information -- Technologies des systèmes MPEG -- Partie 7 : Cryptage commun des fichiers au format de fichier de médias de la base ISO (la version des spécifications ISO/IEC 23001-7:2015 ou ultérieure est requise). Microsoft recommande également d’utiliser CENC version 2 pour tout le contenu HWDRM. En outre, certains types de gestion des droits numériques en fonction du matériel prennent en charge la norme HEVC et d’autres non (voir [Contourner la gestion des droits numériques en fonction du matériel](hardware-drm.md#override-hardware-drm)).
-   Pour tirer parti de certaines nouvelles fonctionnalités de PlayReady 3.0 (SL3000 pour les clients matériels, acquisition de plusieurs licences non persistantes en un message d’acquisition de licence, restrictions temporelles sur les licences non persistantes, etc.), le serveur PlayReady doit être la version commerciale du Kit de développement logiciel serveur Microsoft PlayReady v3.0.2769 ou version ultérieure.
-   Selon la stratégie de protection de sortie spécifiée dans la licence de contenu, la lecture multimédia peut échouer pour les utilisateurs finaux si leur sortie connectée ne prend pas en charge ces exigences. Le tableau suivant répertorie l’ensemble des erreurs courantes qui se produisent en conséquence. Pour plus d’informations, voir les [Règles de conformité et de robustesse de PlayReady](http://www.microsoft.com/playready/licensing/compliance/).

| Erreur                                                   | Valeur      | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|---------------------------------------------------------|------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ERROR\_GRAPHICS\_OPM\_OUTPUT\_DOES\_NOT\_SUPPORT\_HDCP  | 0xC0262513 | La stratégie de protection de contenu de la licence requiert que le moniteur enclenche la protection HDCP, mais celle-ci n’a pas pu être enclenchée.                                                                                                                                                                                                                                                                                                                                                                                              |
| MF\_E\_POLICY\_UNSUPPORTED                              | 0xC00D7159 | La stratégie de protection de contenu de la licence requiert que le moniteur enclenche la protection HDCP de type 1, mais celle-ci n’a pas pu être enclenchée.                                                                                                                                                                                                                                                                                                                                                                                |
| DRM\_E\_TEE\_OUTPUT\_PROTECTION\_REQUIREMENTS\_NOT\_MET | 0x8004CD22 | Ce code d’erreur survient uniquement lors de l’exécution en gestion des droits numériques en fonction du matériel. La stratégie de protection de contenu de la licence requiert que le moniteur enclenche la protection HDCP ou réduise la résolution effective du contenu, mais la protection HDCP n’a pas pu être enclenchée et la résolution effective du contenu n’a pas pu être réduite, car la gestion des droits numériques ne prend pas en charge la réduction de la résolution du contenu. En gestion des droits numériques en fonction du logiciel, le contenu est lu. Voir [Considérations pour l’utilisation de la gestion des droits numériques en fonction du matériel](hardware-drm.md#considerations-for-using-hardware-drm). |
| ERROR\_GRAPHICS\_OPM\_NOT\_SUPPORTED                    | 0xc0262500 | Le pilote graphique ne prend pas en charge la protection de sortie. Par exemple, le moniteur est connecté via VGA ou aucun pilote graphique approprié pour la sortie numérique n’est installé. Dans ce dernier cas, le pilote standard qui est installé est celui de la carte vidéo de base Microsoft et l’installation d’un pilote graphique approprié résoudra le problème.                                                                                                                                                  |

## Prérequis

Avant de commencer la création de votre application pour UWP protégée par PlayReady, le logiciel suivant doit être installé sur votre système :

-   Windows 10.
-   Si vous compilez les exemples de la gestion des droits numériques par PlayReady pour les applications pour UWP, vous devez utiliser Microsoft Visual Studio 2015 ou version ultérieure. Vous pouvez toujours utiliser Microsoft Visual Studio 2013 pour compiler les exemples de la gestion des droits numériques par PlayReady pour les applications du Windows Store Windows 8.1.

Si vous envisagez de lire du contenu MPEG-2/H.262 sur votre application, vous devez également télécharger et installer le [Pack Media Center pour Windows 8.1](http://go.microsoft.com/fwlink/p/?LinkId=626876).

## Guide de migration des applications du Windows Store PlayReady

Cette section inclut des informations sur la façon de migrer vos applications du Windows Store Windows 8.x PlayReady existantes vers Windows 10.

L’espace de noms pour les applications pour UWP PlayReady sur Windows 10 est passé de **Microsoft.Media.PlayReadyClient** à [**Windows.Media.Protection.PlayReady**](https://msdn.microsoft.com/library/windows/apps/dn986454). Cela signifie que vous devez rechercher l’ancien espace de noms et le remplacer par le nouvel espace de noms dans votre code. Vous devrez toujours référencer un fichier winmd. Ce fichier fait partie de windows.media.winmd sur le système d’exploitation Windows 10. Il se trouve sous windows.winmd et fait partie du kit de développement logiciel (SDK) Windows. Pour la plateforme Windows universelle, il est référencé sous windows.foundation.univeralappcontract.winmd.

Pour lire du contenu haute définition (1080p) et ultra haute définition (UHD) protégé par PlayReady, vous devez implémenter la gestion des droits numériques en fonction du matériel par PlayReady. Pour plus d’informations sur l’implémentation de la gestion des droits numériques en fonction du matériel par PlayReady, voir [Gestion des droits numériques en fonction du matériel](hardware-drm.md).

Certains contenus ne sont pas pris en charge par la gestion des droits numériques en fonction du matériel. Pour plus d’informations sur la désactivation de la gestion des droits numériques en fonction du matériel et l’activation de la gestion des droits numériques en fonction du logiciel, voir [Contourner la gestion des droits numériques en fonction du matériel](hardware-drm.md#override-hardware-drm).

Pour le Gestionnaire de protection multimédia, assurez-vous que votre code comporte les paramètres suivants, si ce n’est pas déjà le cas :

``` syntax
var mediaProtectionManager = new Windows.Media.Protection.MediaProtectionManager();

mediaProtectionManager.properties["Windows.Media.Protection.MediaProtectionSystemId"] = 
             '{F4637010-03C3-42CD-B932-B48ADF3A6A54}'
var cpsystems = new Windows.Foundation.Collections.PropertySet();
cpsystems["{F4637010-03C3-42CD-B932-B48ADF3A6A54}"] = 
                "Windows.Media.Protection.PlayReady.PlayReadyWinRTTrustedInput";
mediaProtectionManager.properties["Windows.Media.Protection.MediaProtectionSystemIdMapping"] = cpsystems;

mediaProtectionManager.properties["Windows.Media.Protection.MediaProtectionContainerGuid"] = 
                "{9A04F079-9840-4286-AB92-E65BE0885F95}";
```

## Acquérir une licence non persistante de manière proactive avant la lecture

Cette section explique comment acquérir des licences non persistantes de manière proactive avant le début de la lecture.

Dans les versions précédentes de la gestion des droits numériques par PlayReady, les licences non persistantes pouvaient uniquement être acquises de manière réactive, pendant la lecture. Dans cette version, vous pouvez acquérir des licences non persistantes de manière proactive avant le début de la lecture.

1.  Créez une session de lecture de manière proactive afin de stocker la licence non persistante. Par exemple :

    ``` syntax
    var cpsystems = new Windows.Foundation.Collections.PropertySet();       
    cpsystems["{F4637010-03C3-42CD-B932-B48ADF3A6A54}"] = "Windows.Media.Protection.PlayReady.PlayReadyWinRTTrustedInput"; // PlayReady

    var pmpSystemInfo = new Windows.Foundation.Collections.PropertySet();
    pmpSystemInfo["Windows.Media.Protection.MediaProtectionSystemId"] = "{F4637010-03C3-42CD-B932-B48ADF3A6A54}";
    pmpSystemInfo["Windows.Media.Protection.MediaProtectionSystemIdMapping"] = cpsystems;
    var pmpServer = new Windows.Media.Protection.MediaProtectionPMPServer( pmpSystemInfo );
    ```

2.  Liez cette session de lecture à la classe d’acquisition de licence. Par exemple :

    ``` syntax
    var licenseSessionProperties = new Windows.Foundation.Collections.PropertySet();
    licenseSessionProperties["Windows.Media.Protection.MediaProtectionPMPServer"] = pmpServer;
    var licenseSession = new Windows.Media.Protection.PlayReady.PlayReadyLicenseSession( licenseSessionProperties );
    ```

3.  Créez une demande de service de licence. Par exemple :

    ``` syntax
    var laSR = licenseSession.CreateLAServiceRequest();
    ```

4.  Procédez à l’acquisition de licence en utilisant la demande de service créée à l’étape 3. La licence sera stockée dans la session de lecture.
5.  Liez la session de lecture à la source de média pour la lecture. Par exemple :

    ``` syntax
    licenseSession.configureMediaProtectionManager( mediaProtectionManager );
    videoPlayer.msSetMediaProtectionManager( mediaProtectionManager );
    ```
    
## Ajouter un arrêt sécurisé

Cette section explique comment ajouter un arrêt sécurisé à votre application pour UWP.

L’arrêt sécurisé permet à un appareil PlayReady de confirmer de manière sûre à un service de diffusion multimédia que la lecture multimédia a été stoppée sur un contenu donné. Cette fonctionnalité permet à vos services de diffusion multimédia de contrôler et de rapporter avec précision les limites d’utilisation sur différents appareils pour un compte donné.

Il existe deux scénarios principaux pour l’envoi d’une demande d’arrêt sécurisé :

-   Lorsque la présentation multimédia s’arrête car la fin du contenu a été atteinte ou lorsque l’utilisateur a arrêté la présentation multimédia en cours de lecture.
-   Lorsque la session précédente se termine inopinément (par exemple, en raison d’un blocage du système ou de l’application). L’application devra contrôler, au moment du démarrage ou de l’arrêt, s’il y a des sessions d’arrêt sécurisé en attente et envoyer les demandes séparément de la lecture de tout autre contenu multimédia.

Pour un exemple d’implémentation de l’arrêt sécurisé, voir le fichier securestop.cs dans l’exemple PlayReady disponible à l’adresse [http://go.microsoft.com/fwlink/p/?linkid=331670&clcid=0x409](http://go.microsoft.com/fwlink/p/?linkid=331670).

 

 






<!--HONumber=Mar16_HO1-->


