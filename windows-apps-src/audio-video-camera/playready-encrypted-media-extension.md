---
author: drewbatgit
ms.assetid: 79C284CA-C53A-4C24-807E-6D4CE1A29BFA
description: Cette section explique comment modifier votre application web de PlayReady pour prendre en charge les modifications apportées à partir de la version précédente de Windows8.1 vers la version de Windows 10.
title: Extension EME (Encrypted Media Extension) PlayReady
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: b73464ea10aa835b82df17605e983ebdfb9cd890
ms.sourcegitcommit: ed0304b8a214c03b8aab74b8ef12c9f82b8e3c5f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/19/2018
ms.locfileid: "7285958"
---
# <a name="playready-encrypted-media-extension"></a>Extension EME (Encrypted Media Extension) PlayReady



Cette section explique comment modifier votre application web de PlayReady pour prendre en charge les modifications apportées à partir de la version précédente de Windows8.1 vers la version de Windows 10.

L’utilisation d’éléments multimédias PlayReady dans Internet Explorer permet aux développeurs de créer des applications web capables de fournir du contenu PlayReady à l’utilisateur tout en appliquant les règles d’accès définies par le fournisseur de contenu. Cette section explique comment ajouter des éléments multimédias PlayReady à vos applications web existantes en utilisant uniquement HTML5 et JavaScript.

## <a name="whats-new-in-playready-encrypted-media-extension"></a>Nouveautés de l’extension EME (Encrypted Media Extension) PlayReady

Cette section fournit une liste des modifications apportées à la PlayReady Extension (Encrypted Media) pour activer la protection de contenu PlayReady sur Windows 10.

La liste suivante décrit les nouvelles fonctionnalités et les modifications apportées à PlayReady Encrypted Media Extension pour Windows 10:

-   Ajout de la gestion des droits numériques (DRM) en fonction du matériel.

    La prise en charge de la protection du contenu en fonction du matériel permet la lecture sécurisée de contenu en haute définition (HD) et ultra haute définition (UHD) sur plusieurs plateformes d’appareils. Le matériel de clé (y compris les clés privées, les clés de contenu et tout autre matériel de clé utilisé pour dériver ou déverrouiller ces clés) et les échantillons vidéo compressés et non compressés déchiffrés sont protégés en tirant parti de la sécurité matérielle.

-   Acquisition proactive de licences non persistantes.
-   Acquisition de plusieurs licences en un message.

    Vous pouvez utiliser un objet PlayReady avec plusieurs identificateurs de clé (KeyID) comme dans Windows8.1, ou utiliser des [données de modèle de déchiffrement de contenu (CDMData)](https://go.microsoft.com/fwlink/p/?LinkID=626819) avec plusieurs KeyID.

    > [!NOTE]
    > Dans Windows 10, plusieurs identificateurs de clé sont pris en charge sous &lt;KeyID&gt; dans CDMData.

-   Ajout de la prise en charge du délai d’expiration en temps réel ou licence à durée limitée (LDL).

    Permet de définir l’expiration en temps réel des licences.

-   Ajout de la prise en charge de la stratégie de protection HDCP de type 1 (version 2.2).
-   Miracast est désormais une sortie implicite.
-   Ajout de l’arrêt sécurisé.

    L’arrêt sécurisé permet à un appareil PlayReady de confirmer de manière sûre à un service de diffusion multimédia que la lecture multimédia a été stoppée sur un contenu donné.

-   Ajout d’une séparation de licence audio et vidéo.

    Les pistes séparées empêchent la vidéo d’être décodée en tant qu’audio, offrant ainsi une protection plus sûre du contenu. Les normes émergentes nécessitent des touches distinctes pour les pistes audio et vidéo.

-   Ajout de MaxResDecode.

    Cette fonctionnalité a été ajoutée pour limiter la lecture de contenu à une résolution maximale et ce, même en cas de possession d’une clé plus compatible (mais pas d’une licence). Cela concerne également les cas où plusieurs tailles de flux sont codées avec une clé unique.

## <a name="encrypted-media-extension-support-in-playready"></a>Prise en charge de l’extension EME (Encrypted Media Extension) dans PlayReady

Cette section décrit la version de l’extension EME (Encrypted Media Extension) W3C prise en charge par PlayReady.

PlayReady pour les applications web est actuellement lié à l’[ébauche du document W3C sur l’extension EME (Encrypted Media Extension) datée du 10mai2013](http://www.w3.org/TR/2013/WD-encrypted-media-20130510/). Cette prise en charge sera remplacée par la spécification EME mise à jour dans les prochaines versions de Windows.

## <a name="use-hardware-drm"></a>Utiliser la gestion des droits numériques en fonction du matériel

Cette section explique comment votre application web peut utiliser la gestion des droits numériques en fonction du matériel par PlayReady et comment désactiver la gestion des droits numériques en fonction du matériel si le contenu protégé ne la prend pas en charge.

Pour utiliser la gestion des droits numériques en fonction du matériel par PlayReady, votre application web JavaScript doit utiliser la méthode EME **isTypeSupported** avec un identificateur système de clé `com.microsoft.playready.hardware` pour vérifier la prise en charge de la gestion des droits numériques en fonction du matériel par PlayReady à partir du navigateur.

Il arrive que certains contenus ne soient pas pris en charge par la gestion des droits numériques en fonction du matériel. Le contenu Cocktail n’est jamais pris en charge par la gestion des droits numériques en fonction du matériel. Si vous souhaitez lire du contenu Cocktail, vous devez désactiver la gestion des droits numériques en fonction du matériel. Certains types de gestion des droits numériques en fonction du matériel prennent en charge le contenu HEVC et d’autres non. Si vous voulez lire du contenu HEVC et que la gestion des droits numériques en fonction du matériel ne le prend pas en charge, vous pouvez la désactiver.

> [!NOTE]
> Pour déterminer si le contenu HEVC est pris en charge, après l’instanciation de `com.microsoft.playready`, utilisez la méthode [**PlayReadyStatics.CheckSupportedHardware**](https://msdn.microsoft.com/library/windows/apps/dn986441).

## <a name="add-secure-stop-to-your-web-app"></a>Ajouter un arrêt sécurisé à votre application web

Cette section explique comment ajouter un arrêt sécurisé à votre application web.

L’arrêt sécurisé permet à un appareil PlayReady de confirmer de manière sûre à un service de diffusion multimédia que la lecture multimédia a été stoppée sur un contenu donné. Cette fonctionnalité permet à vos services de streaming multimédia de contrôler et de rapporter avec précision les limites d’utilisation sur différents appareils pour un compte donné.

Il existe deux scénarios principaux pour l’envoi d’une demande d’arrêt sécurisé :

-   Lorsque la présentation multimédia s’arrête car la fin du contenu a été atteinte ou lorsque l’utilisateur a arrêté la présentation multimédia en cours de lecture.
-   Lorsque la session précédente se termine inopinément (par exemple, en raison d’un blocage du système ou de l’application). L’application devra contrôler, au moment du démarrage ou de l’arrêt, s’il y a des sessions d’arrêt sécurisé en attente et envoyer les demandes séparément de la lecture de tout autre contenu multimédia.

Les procédures suivantes expliquent comment configurer l’arrêt sécurisé pour différents scénarios.

Pour configurer l’arrêt sécurisé pour un arrêt normal d’une présentation:

1.  Enregistrez l’événement **onEnded** avant le début de la lecture.
2.  Le gestionnaire d’événements **onEnded** doit appeler`removeAttribute(“src”)` à partir de l’objet de l’élément audio/vidéo pour définir la source sur **NULL**, ce qui déclenchera la suppression de la topologie, la destruction des déchiffreurs et la définition de l’état d’arrêt par Media Foundation.
3.  Vous pouvez démarrer la session CDM d’arrêt sécurisé dans le gestionnaire pour envoyer la demande d’arrêt sécurisé au serveur afin de notifier l’arrêt de la lecture à ce moment, mais vous pouvez également le faire ultérieurement.

Pour configurer l’arrêt sécurisé si l’utilisateur quitte la page ou ferme l’onglet ou le navigateur :

-   Aucune action de l’application n’est requise pour enregistrer l’état d’arrêt ; il sera enregistré pour vous.

Pour configurer l’arrêt sécurisé pour des contrôles de page ou des actions utilisateur personnalisés (par exemple, des boutons de navigation personnalisés ou le démarrage d’une nouvelle présentation avant la fin de la présentation actuelle):

-   Lorsqu’une action utilisateur personnalisée a lieu, l’application doit définir la source sur **NULL**, ce qui déclenchera la suppression de la topologie, la destruction des déchiffreurs et la définition de l’état d’arrêt par Media Foundation.

L’exemple suivant montre comment utiliser l’arrêt sécurisé dans votre application web:

```JavaScript
// JavaScript source code

var g_prkey = null;
var g_keySession = null;
var g_fUseSpecificSecureStopSessionID = false;
var g_encodedMeteringCert = 'Base64 encoded of your metering cert (aka publisher cert)';

// Note: g_encodedLASessionId is the CDM session ID of the proactive or reactive license acquisition 
//       that we want to initiate the secure stop process.
var g_encodedLASessionId = null;

function main()
{
    ...

    g_prkey = new MSMediaKeys("com.microsoft.playready");

    ...

    // add 'onended' event handler to the video element
    // Assume 'myvideo' is the ID of the video element
    var videoElement = document.getElementById("myvideo");
    videoElement.onended = function (e) { 

        //
        // Calling removeAttribute("src") will set the source to null
        // which will trigger the MF to tear down the topology, destroy the
        // decryptor(s) and set the stop state.  This is required in order
        // to set the stop state.
        //
        videoElement.removeAttribute("src");
        videoElement.load();

        onEndOfStream();
    };
}

function onEndOfStream()
{
    ...

    createSecureStopCDMSession();

    ...    
}

function createSecureStopCDMSession()
{
    try{    
        var targetMediaCodec = "video/mp4";
        var customData = "my custom data";

        var encodedSessionId = g_encodedLASessionId;
        if( !g_fUseSpecificSecureStopSessionID )
        {
            // Use "*" (wildcard) as the session ID to include all secure stop sessions
            // TODO: base64 encode "*" and place encoded result to encodedSessionId
        }

        var int8ArrayCDMdata = formatSecureStopCDMData( encodedSessionId, customData,  g_encodedMeteringCert );
        var emptyArrayofInitData = new Uint8Array();

        g_keySession = g_prkey.createSession(targetMediaCodec, emptyArrayofInitData, int8ArrayCDMdata);

        addPlayreadyKeyEventHandler();

    } catch( e )
    {
        // TODO: Handle exception
    }
}

function addPlayreadyKeyEventHandler()
{
    // add 'keymessage' eventhandler   
    g_keySession.addEventListener('mskeymessage', function (event) {

        // TODO: Get the keyMessage from event.message.buffer which contains the secure stop challenge
        //       The keyMessage format for the secure stop is similar to LA as below:
        //
        //            <PlayReadyKeyMessage type="SecureStop" >
        //              <SecureStop version="1.0" >
        //                <Challenge encoding="base64encoded">
        //                    secure stop challenge
        //                </Challenge>
        //                <HttpHeaders>
        //                    <HttpHeader>
        //                      <name>Content-Type</name>
        //                         <value>"content type data"</value>
        //                    </HttpHeader>
        //                    <HttpHeader>
        //                         <name>SOAPAction</name>
        //                         <value>soap action</value>
        //                     </HttpHeader>
        //                    ....
        //                </HttpHeaders>
        //              </SecureStop>
        //            </PlayReadyKeyMessage>
                
        // TODO: send the secure stop challenge to a server that handles the secure stop challenge

        // TODO: Recevie and response and call event.target.Update() to proecess the response
    });
    
    // add 'keyerror' eventhandler
    g_keySession.addEventListener('mskeyerror', function (event) {
        var session = event.target;
        
        ...

        session.close();
    });
    
    // add 'keyadded' eventhandler
    g_keySession.addEventListener('mskeyadded', function (event) {
        
        var session = event.target;

        ...

        session.close();             
    });
}

/**
* desc@ formatSecureStopCDMData
*   generate playready CDMData
*   CDMData is in xml format:
*   <PlayReadyCDMData type="SecureStop">
*     <SecureStop version="1.0">
*       <SessionID>B64 encoded session ID</SessionID>
*       <CustomData>B64 encoded custom data</CustomData>
*       <ServerCert>B64 encoded server cert</ServerCert>
*     </SecureCert>
* </PlayReadyCDMData>        
*/
function formatSecureStopCDMData(encodedSessionId, customData, encodedPublisherCert) 
{
    var encodedCustomData = null;

    // TODO: base64 encode the custom data and place the encoded result to encodedCustomData

    var CDMDataStr = "<PlayReadyCDMData type=\"SecureStop\">" +
                     "<SecureStop version=\"1.0\" >" +
                     "<SessionID>" + encodedSessionId + "</SessionID>" +
                     "<CustomData>" + encodedCustomData + "</CustomData>" +
                     "<ServerCert>" + encodedPublisherCert + "</ServerCert>" +
                     "</SecureStop></PlayReadyCDMData>";
    
    var int8ArrayCDMdata = null

    // TODO: Convert CDMDataStr to Uint8 byte array and palce the converted result to int8ArrayCDMdata

    return int8ArrayCDMdata;
}
```

> [!NOTE]
> La ligne de code `<SessionID>B64 encoded session ID</SessionID>` des données d’arrêt sécurisé dans l’exemple ci-dessus peut être un astérisque (\*), qui constitue un caractère générique pour toutes les sessions d’arrêt sécurisé enregistrées. Autrement dit, la balise **SessionID** peut être une session spécifique ou un caractère générique (\*) pour sélectionner toutes les sessions d’arrêt sécurisé.

## <a name="programming-considerations-for-encrypted-media-extension"></a>Considérations en matière de programmation de l’extension EME (Encrypted Media Extension)

Cette section répertorie les considérations de programmation que vous devez prendre en compte lors de la création de votre application web prenant en charge par PlayReady pour Windows 10.

Les objets **MSMediaKeys** et **MSMediaKeySession** créés par votre application doivent être maintenus actifs jusqu’à la fermeture de votre application. Pour garantir que ces objets restent actifs, vous pouvez les affecter en tant que variables globales (les variables deviennent hors de portée et sont mises à la corbeille si elles sont déclarées comme variables locales à l’intérieur d’une fonction). L’exemple suivant attribue les variables *g\_msMediaKeys* et *g\_mediaKeySession* en tant que variables globales, avant de les attribuer aux objets **MSMediaKeys** et **MSMediaKeySession** dans la fonction.

``` syntax
var g_msMediaKeys;
var g_mediaKeySession;

function foo() {
  ...
  g_msMediaKeys = new MSMediaKeys("com.microsoft.playready");
  ...
  g_mediaKeySession = g_msMediaKeys.createSession("video/mp4", intiData, null);
  g_mediaKeySession.addEventListener(this.KEYMESSAGE_EVENT, function (e) 
  {
    ...
    downloadPlayReadyKey(url, keyMessage, function (data) 
    {
      g_mediaKeySession.update(data);
    });
  });
  g_mediaKeySession.addEventListener(this.KEYADDED_EVENT, function () 
  {
    ...
    g_mediaKeySession.close();
    g_mediaKeySession = null;
  });
}
```

Pour plus d’informations, voir les [exemples d’applications](https://code.msdn.microsoft.com/windowsapps/PlayReady-samples-for-124a3738).

## <a name="see-also"></a>Voir également
- [Gestion des droits numériques PlayReady](playready-client-sdk.md)




