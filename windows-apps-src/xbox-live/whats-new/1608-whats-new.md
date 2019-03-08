---
title: Nouveautés pour le Kit de développement Xbox Live - août 2016
description: Nouveautés pour le Kit de développement Xbox Live - août 2016
ms.assetid: fa52e7bd-2c2c-4c25-94ab-761036a7ca79
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 2a498fea1ed0974935a273c9ee72ba2c95d15959
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57627904"
---
# <a name="whats-new-for-the-xbox-live-sdk---august-2016"></a>Nouveautés pour le Kit de développement Xbox Live - août 2016

Veuillez consulter la [What ' s New - juin 2016](1606-whats-new.md) article pour ce qui a été ajouté dans la version de juin 2016.

## <a name="os-and-tool-support"></a>Prise en charge du système d’exploitation et d’outil
Le Xbox Live SDK prend en charge Windows 10 RTM [Version 10.0.10240] et Visual Studio 2015 RTM [Version 14.0.23107.0].

## <a name="documentation"></a>Documentation
- Si vous écrivez une application UWP et implémentez la possibilité d’inviter des utilisateurs à un jeu, il existe des instructions sur la ```.appxmanifest``` les modifications nécessaires dans [configurer votre AppXManifest pour mode multijoueur](../multiplayer/service-configuration/configure-your-appxmanifest-for-multiplayer.md).  Cela a été abordé précédemment sur le [forums](https://forums.xboxlive.com) et [portage du code de xbox live à partir de l’ère à uwp](../using-xbox-live/porting-xbox-live-code-from-xdk-to-uwp.md) article
- Le [Introduction au Gestionnaire de sociale](../social-platform/intro-to-social-manager.md) article a été mis à jour pour refléter les dernières modifications d’API et fournir plus d’informations sur les codes de retour pour certaines des fonctions.

## <a name="unity-samples"></a>Exemples de Unity
Certains nouveaux exemples ont été ajoutées pour les développeurs Unity écriture d’une application UWP.
- Il existe désormais une version Unity de l’exemple de réseaux sociaux, vous pouvez le trouver sous le répertoire d’exemples/sociale/Unity.
- Il existe également un exemple décrivant comment utiliser le stockage connecté.  Consultez les exemples/GameSave/Unity pour l’exemple.
Il existe une version Unity de AchievementsLeaderboard sous AchievementsLeaderboard/Samples/Unity

## <a name="social-manager"></a>Gestionnaire sociale
Outre les mises à jour de documentation mentionnés ci-dessus, il existe certaines nouvelles API ont été ajoutés.  Ceux-ci sont décrits ci-dessous, et vous pouvez voir plus en détail dans social_manager.h

- Ajout nouvelle API qui permet la mise à jour des groupes de réseaux sociaux sans recréation de :

```cpp
    _XSAPIIMP xbox_live_result<void> update_social_user_group(
        _In_ const std::shared_ptr<xbox_social_user_group>& group,
        _In_ const std::vector<string_t>& users
        );
```
- Une mise à jour terminée de groupe social est indiqué par la ```social_user_group_updated``` événement


## <a name="multiplayer"></a>Mode multijoueur
Navigation améliorée de session est désormais disponible et vous pouvez utiliser les nouvelles API multijoueurs à le pour utiliser.

Avec les nouvelles API, vous pouvez filtrer sur les balises, des chaînes et autres données riches permettant aux utilisateurs de trouver plus facilement une session de lecture.

Publierons une documentation plus complète dans les prochains mois, mais brièvement, vous pouvez maintenant associer un handle de recherche « » avec un à l’aide de la Session de MPSD ```set_search_handle``` et ensuite les utilisateurs peuvent rechercher des sessions à l’aide d’un mécanisme de filtrage robust par votre appel de titre ```get_search_handles```

Les nouvelles API sont répertoriées ci-dessous.  Essayez de les extraire et si vous rencontrez des problèmes, publier un thread de prise en charge dans les [forums](https://forums.xboxlive.com) ou contacter votre mère.  Nous devons obtenir des exemples montrant comment utiliser ces API bientôt.

```cpp
_XSAPIIMP pplx::task<xbox_live_result<void>> set_search_handle(
    _In_ multiplayer_search_handle_request searchHandleRequest
    );
```

```cpp
_XSAPIIMP pplx::task<xbox_live_result<std::vector<multiplayer_search_handle_details>>> get_search_handles(
    _In_ const string_t& serviceConfigurationId,
    _In_ const string_t& sessionTemplateName,
    _In_ const string_t& orderBy,
    _In_ bool orderAscending,
    _In_ const string_t& searchQuery
    );
```

```cpp
_XSAPIIMP pplx::task<xbox_live_result<void>> clear_search_handle(_In_ const string_t& handleId);
```

### <a name="xbox-integrated-multiplayer"></a>Mode multijoueur intégré Xbox

Nous avons inclus la documentation pour la Xbox intégré multijoueur (XIM) API.  L’API elle-même sera disponible dans une version ultérieure du SDK Xbox Live, mais la documentation et l’en-tête seront disponibles pour la version préliminaire.

XIM est une interface autonome pour ajouter facilement multijoueur communication mise en réseau et les conversations en temps réel à votre jeu grâce à la puissance des services Xbox Live.

Cette version d’évaluation de la documentation de l’API est partagée ici pour encourager la consultation et commentaires des clients. Nous avons parlé de cette API plus tôt en Xfest 2016, et vous pouvez voir archivées [matériel de présentation sur le site de développeur partenaire géré](https://developer.xboxlive.com/en-us/platform/documentlibrary/events/Pages/Xfest2016.aspx) à partir de la discussion « Clé en main mode multijoueur mise en réseau et de conversation ». Notez que cette documentation de version préliminaire est uniquement pour l’API C++. Équivalents de WinRT pour C# et d’autres langues seront disponibles plus loin dans l’année.

Si vous êtes intéressé par les fonctionnalités de XIM, avez des commentaires ou autres questions sur ce projet, n’hésitez pas à poster sur le [Forum de développement Xbox](https://forums.xboxlive.com/) ou contacter via votre responsable de compte de développeur.

Vous pouvez voir cette nouvelle documentation dans xbox_integrated_multiplayer.chm dans le répertoire Docs du SDK Xbox Live.  Le fichier include est disponible en version préliminaire dans \include\xim\XboxIntegratedMultiplayer.h.  
