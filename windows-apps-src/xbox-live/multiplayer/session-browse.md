---
title: Navigation dans la session multijoueurs
description: Découvrez comment implémenter la navigation dans la session multijoueur à l’aide de Xbox Live multijoueurs.
ms.assetid: b4b3ed67-9e2c-4c14-9b27-083b8bccb3ce
ms.date: 10/16/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 579c71ef9266fb9a1ee4ef0538d1beffec0bb4ea
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57660674"
---
# <a name="multiplayer-session-browse"></a>Navigation dans la session multijoueurs

Session multijoueur Parcourir est une nouvelle fonctionnalité introduite en novembre 2016 qui permet un titre à la requête pour obtenir la liste de sessions de jeux multijoueurs ouvertes qui répondent aux critères spécifiés.

## <a name="what-is-session-browse"></a>Quelle est la navigation dans la session ?

Dans un scénario de parcourir la session, un joueur dans un jeu est en mesure de récupérer la liste des sessions de jeu joignables. Chaque entrée de session dans cette liste contient des métadonnées supplémentaires relatives au jeu, un lecteur peut utiliser pour les aider à sélectionner la session à rejoindre.  Ils peuvent également filtrer la liste des sessions en fonction des métadonnées. Une fois que le joueur voit une session de jeu qui suscite la convoitise leur, ils peuvent rejoindre la session.

Un lecteur peut également créer une nouvelle session de jeu et utiliser la navigation dans la session pour recruter de lecteurs supplémentaires au lieu d’utiliser matchmaking.

Navigation dans la session diffère des scénarios de matchmaking traditionnel car le lecteur sélectionne automatiquement session jeu pouvoir joindre, tandis que dans matchmaking, le joueur clique généralement sur un bouton « Rechercher un jeu » qui tente de placer automatiquement le lecteur dans un session de jeu appropriée. Tandis que la navigation dans la session est un processus manuel et plus lent qui ne peut pas toujours sélectionner le jeu objectivement meilleur, il offre un meilleur contrôle du lecteur et peut être perçu comme le jeu comme subjective mieux choisir.

Il est courant d’inclure les deux scénarios de matchmaking et de session dans les jeux. Matchmaking est généralement utilisé pour lu couramment des modes de jeu, tandis que la navigation dans la session est utilisée pour les jeux personnalisés.

**Exemple :** John peut vous intéresser multijoueur bataille héros arena style, mais souhaite jouer à un jeu où tous les acteurs de sélectionnent leur héros de manière aléatoire. Il peut récupérer la liste des sessions de jeu ouvertes et de trouver celles qui incluent « héros aléatoire » dans leur description, ou si l’interface utilisateur jeu le permet, il peut sélectionner le mode de jeu « héros aléatoire » et récupérer uniquement les sessions qui sont marquées pour indiquer qu’ils sont « RandomHero » gam es.

Lorsqu’il détecte un jeu qui il aime, il rejoint le jeu. Quand suffisamment personnes ont rejoint la session, l’hôte de la session de jeu peut démarrer le jeu.

### <a name="roles"></a>Rôles

Un jeu dans la navigation dans la session souhaiterez recruter les joueurs pour des rôles spécifiques. Par exemple, un lecteur souhaiterez peut-être créer une session de jeu qui spécifie que la session contient des classes d’assaut pas plus de 5, mais qu’il doit contenir au moins 2 rôles guérisseur et le rôle de réservoir d’au moins 1.

Lorsqu’un autre lecteur s’appliquent à la session, ils peuvent sélectionner préalablement leur rôle, et le service n’autorise pas à rejoindre la session si aucun emplacement est ouvert pour le rôle qu’ils ont sélectionnées.

Un autre exemple serait si un acteur souhaite réserver 2 emplacements pour ses amis à joindre, le jeu peut spécifier un rôle de « friends », et uniquement les lecteurs qui sont des amis avec l’hôte de session peuvent remplir les 2 emplacements dédiés pour le rôle « friends ».

Pour plus d’informations sur les rôles, consultez [rôles multijoueurs](multiplayer-roles.md).



## <a name="how-does-session-browse-work"></a>Comment fonctionne la navigation dans la session ?

Navigation dans la session fonctionne principalement sur l’utilisation de handles de recherche. Un handle de recherche est un paquet de données qui contient une référence à la session, ainsi que des métadonnées supplémentaires relatives à la session, à savoir les attributs de recherche.

Lorsqu’un titre crée une nouvelle session de jeu qui est éligible pour la session de parcourir, il crée un handle de recherche pour la session. Le handle de recherche est stocké dans le mode multijoueur Service annuaire (MPSD), qui gère les descripteurs de recherche pour le titre.

Lorsqu’un titre doit récupérer la liste des sessions, le titre peut envoyer une requête de recherche à MPSD, qui retourne une liste des descripteurs de recherche qui répondent aux critères de recherche. Le titre peut puis utiliser la liste des sessions pour afficher une liste de jeux joignables à l’acteur.

Lorsqu’une session est plein, ou sinon ne peut pas être joint, un titre pouvez supprimer le handle de recherche MPSD afin que la session n’affiche plus dans les requêtes de navigation de session.

>[!NOTE]
> Handles de recherche sont destinées à utiliser lors de l’affichage d’une liste de sessions à présenter à un utilisateur. À l’aide des poignées de recherche pour matchmaking d’arrière-plan n’est pas valide et envisagez plutôt d’utiliser [SmartMatch](multiplayer-manager/play-multiplayer-with-matchmaking.md)

## <a name="set-up-a-session-for-session-browse"></a>Configurer une session de navigation dans la session

Pour pouvoir utiliser des handles de recherche pour une session, la session doit avoir les éléments suivants ensemble de fonctionnalités sur true :

* `searchable`
* `userAuthorizationStyle`

>[!NOTE]
> Le `userAuthorizationStyle` fonctionnalité est uniquement requis pour les jeux UWP, mais nous recommandons de les mettre en œuvre pour tous les Xbox Live jeux, y compris ceux XDK, car elle garantit la portabilité futures.

>[!NOTE]
> Définition de la `userAuthorizationStyle` valeurs par défaut de la fonctionnalité le `readRestriction` et `joinRestriction` de la session à `local` au lieu de `none`. Cela signifie que les titres doivent utiliser des handles de recherche ou transférer les poignées pour rejoindre une session de jeu.

Vous pouvez définir ces fonctionnalités dans le modèle de session lorsque vous configurez vos services Xbox Live.

L’option for browse de session, vous devez uniquement créer des handles de recherche sur les sessions qui seront utilisées pour le jeu réel, pas pour les sessions de salle d’attente.

## <a name="what-does-it-mean-to-be-an-owner-of-a-session"></a>Que signifie être le propriétaire d’une session ?

Alors que la session de jeu de nombreux types, tels que SmartMatch ou un amis uniquement de jeux, ne requièrent pas un propriétaire, pour les sessions de navigation de session vous pouvez souhaiter avoir un propriétaire. 

Une session gérée par propriétaire présente certains avantages pour le propriétaire. Les propriétaires peuvent supprimer d’autres membres de la session ou modifier l’état de la propriété d’autres membres.

Pour pouvoir utiliser les propriétaires d’une session, la session doit avoir les éléments suivants ensemble de fonctionnalités sur true :

* `hasOwners`

Si un propriétaire d’une session possède un membre de Xbox Live bloqué, ce membre ne peut pas joindre la session.

Lorsque vous utilisez [rôles multijoueurs](multiplayer-roles.md), vous pouvez la définir, seuls les propriétaires peuvent attribuer des rôles aux utilisateurs.

Si tous les propriétaires de quitter une session, le service prend des mesures sur la session en fonction du `ownershipPolicy.migration` stratégie est définie pour la session. Si la stratégie est « plus ancienne », puis le lecteur qui a été le plus longtemps dans la session est défini comme relation le nouveau propriétaire. Si la stratégie est « endsession » (la valeur par défaut si non spécifié), le service termine la session et supprime tous les joueurs restants de la session.


## <a name="search-handles"></a>Handles de recherche

Un handle de recherche est stocké dans MSPD comme une structure JSON. En plus de contenir une référence à la session, les handles de recherche contiennent également des métadonnées supplémentaires pour les recherches, connu en tant qu’attributs de la recherche.

Une session peut avoir uniquement un handle de recherche créé pour lui à tout moment.

Pour créer un handle de recherche pour une session dans à l’aide des API Xbox Live, vous créez tout d’abord un `multiplayer::multiplayer_search_handle_request` de l’objet et passez ensuite cet objet à le `multiplayer::multiplayer_service::set_search_handle()` (méthode).

### <a name="search-attributes"></a>Attributs de recherche

Attributs de recherche est constitué des composants suivants :

`tags` -Les balises sont des descripteurs de chaîne qui leur permet de classer une session de jeu, similaire à un mot-dièse. Balises doivent commencer par une lettre, ne peut pas contenir d’espaces et doivent être inférieure à 100 caractères.
Exemples de balise : "ProRankOnly", "norocketlaunchers", "cityMaps".

`strings` -Chaînes sont des variables de texte, et les noms de chaîne doivent commencer par une lettre, ne peut pas contenir d’espaces et doivent être inférieure à 100 caractères.

Exemple de métadonnées de chaîne : "Weapons"="knives+pistols+rifles", "MapName"="UrbanCityAssault", "description"="Fun casual game, new people welcome."

`numbers` -Nombres sont des variables numériques, et les nombres noms doivent commencer par une lettre, ne peut pas contenir d’espaces et doivent être inférieure à 100 caractères. Xbox Live API récupérer des valeurs numériques en tant que float, type.

Exemple de métadonnées numéro : « MinLevel » = 25, « MaxRank » = 10.

>**Remarque :** La casse des lettres de balises et valeurs de chaîne est conservée dans le service, mais vous devez utiliser la fonction tolower() lorsque vous interrogez des balises. Cela signifie que les balises et les valeurs de chaîne sont actuellement tous traités comme des minuscules, même s’ils contiennent des caractères en majuscules.

Dans les API Xbox Live, vous pouvez définir les attributs de recherche à l’aide de la `set_tags()`, `set_stringsmetadata()`, et `set_numbers_metadata()` méthodes d’un `multiplayer_search_handle_request` objet.


### <a name="additional-details"></a>Informations supplémentaires

Lorsque vous récupérez un handle de recherche, les résultats incluent également des données utiles supplémentaires sur la session, par exemple comme si la session est fermée, y a-t-il des restrictions de jointure sur la session, etc.

Dans les API Xbox Live, ces détails, ainsi que les attributs de recherche, sont inclus dans le `multiplayer_search_handle_details` qui sont retournés après une requête de recherche.

### <a name="remove-a-search-handle"></a>Supprimer un handle de recherche

Lorsque vous souhaitez supprimer une session de navigation dans la session, par exemple lorsque la session est complète, ou si la session est fermée, vous pouvez supprimer le handle de recherche.

Dans les API Xbox Live, vous pouvez utiliser la `multiplayer_service::clear_search_handle()` méthode pour supprimer un handle de recherche.

### <a name="example-create-a-search-handle-with-metadata"></a>Exemple : Créer un handle de recherche avec des métadonnées

Le code suivant montre comment créer un handle de recherche pour une session en utilisant les API multijoueurs C++ Xbox Live.

```cpp
auto searchHandleReq = multiplayer_search_handle_request(sessionBrowseRef);

searchHandleReq.set_tags(std::vector<string_t> val);
searchHandleReq.set_numbers_metadata(std::unordered_map<string_t, double> metadata);
searchHandleReq.set_strings_metadata(std::unordered_map<string_t, string_t> metadata);

auto result = xboxLiveContext->multiplayer_service().set_search_handle(searchHandleReq)
.then([](xbox_live_result<void> result)
{
  if (result.err())
  {
    // handle error
  }
});
```


## <a name="create-a-search-query-for-sessions"></a>Créer une requête de recherche pour les sessions

Lorsque vous récupérez une liste des descripteurs de recherche, vous pouvez utiliser une requête de recherche pour restreindre les résultats dans les sessions qui répondent aux critères spécifiques.

La syntaxe de requête de recherche est un [OData](https://docs.oasis-open.org/odata/odata/v4.0/errata02/os/complete/part2-url-conventions/odata-v4.0-errata02-os-part2-url-conventions-complete.html#_Toc406398092) syntaxe, de style avec uniquement les opérateurs suivants pris en charge :

 Opérateur | Description
 --- | ---
 eq | Est égal à
 Nou | Non égal à
 gt | Supérieur à
 GE | Supérieur ou égal
 lt | Inférieur à
 le | Inférieur ou égal
 et | AND logique
 ou | logique ou (voir la Remarque ci-dessous)

Vous pouvez également utiliser des expressions lambda et le `tolower` canonique. Aucune autre fonction OData n’est actuellement pris en charge.

Lors de la recherche des balises ou des valeurs de chaîne, vous devez utiliser la fonction 'tolower' dans la requête de recherche, comme le service prend uniquement actuellement en charge la recherche de chaînes en minuscules.

Le service Xbox Live retourne uniquement les 100 premiers résultats qui correspondent à la requête de recherche. Votre jeu doit autoriser les lecteurs à affiner leurs requêtes de recherche si les résultats sont trop large.

>[!NOTE]
>  ORs logiques sont pris en charge dans les requêtes de chaîne de filtre ; OR toutefois qu’un seul est autorisé et il doit être à la racine de votre requête. Vous ne pouvez avoir plusieurs opérateurs OR dans votre requête, ni de créer une requête qui entraînerait ou n’est pas en haut niveau de la plupart de la structure de la requête.

### <a name="search-handle-query-examples"></a>Exemples de requêtes de handle de recherche

Dans un appel restful, « Filtre » est à utiliser pour spécifier une chaîne de langage de filtre OData qui s’exécutent dans votre requête sur tous les descripteurs de recherche.  
Dans les API 2015 multijoueurs, vous pouvez spécifier la chaîne de filtre de recherche dans les *searchFilter* paramètre de la `multiplayer_service.get_search_handles()` (méthode).  

Actuellement, les scénarios de filtre suivants sont pris en charge :

 Filtrer par | Chaîne de filtre de recherche
 --- | ---
 Un seul membre de xuid '1234566' | "session/memberXuids/any(d:d eq '1234566')"
 Un xuid unique propriétaire '1234566' | "session/ownerXuids/any(d:d eq '1234566')"
 Une chaîne égale à 'classe 'b ' forzacarclass' | "tolower(strings/forzacarclass) eq 'classb'"
 Un nombre 'forzaskill' égale à 6 | « numéros/forzaskill eq 6 »
 Un nombre supérieur à 1,5 ' halokdratio' | « gt numéros/halokdratio 1.5 »
 Une balise 'coolpeopleonly' | "tags/any(d:tolower(d) eq 'coolpeopleonly')"
 Sessions qui ne contiennent pas de la balise « cursingallowed » | "tags/any(d:tolower(d) ne 'cursingallowed')"
 Les sessions qui ne contiennent pas un nombre 'rank' qui est égale à 0 | nombres « rang nou 0 »
 Les sessions qui ne contiennent pas d’une chaîne « forzacarclass » qui est égale à « classa » | "tolower(strings/forzacarclass) ne 'classa'"
 Une balise « coolpeopleonly » et un nombre 'halokdratio' égal à 7.5 | « tags/any(d:tolower(d) eq 'coolpeopleonly') eq true et numéros/halokdratio eq 7.5 »
 Un nombre 'halodkratio' supérieur ou égal à 1,5, un nombre 'rank' inférieur à 60 et un nombre 'customnumbervalue' inférieur ou égal à 5 | « numéros/halokdratio ge 1.5 et les numéros/rang lt 60 et numéros/customnumbervalue le 5 »
 Un id de réalisation « 123456 » | « achievementIds/any(d:d eq '123456') »
 Le code de langue « en » | "language eq 'en'"
 Heure planifiée, retourne tous les planifiés fois inférieur ou égal à l’heure spécifiée | « session/scheduledTime le « 2009-06-15T13:45:30.0900000Z' »
 Heure validée, retourne tous les validée fois inférieur à l’heure spécifiée | « session/postedTime lt ' 2009-06-15T13:45:30.0900000Z' »
 État de session d’enregistrement | "session/registrationState eq 'registered'"
 Où le nombre de membres de la session est égal à 5 | "session/membersCount eq 5"
 Où le nombre de cibles de membre de session est supérieur à 1 | « session/targetMembersCount gt 1 »
 Où le nombre maximal de membres de la session est inférieur à 3 | « session/maxMembersCount lt 3 »
 Où la différence entre le nombre de cibles de membre de session et le nombre de membres de la session est inférieur ou égal à 5 | « session/targetMembersCountRemaining le 5 »
 Où la différence entre le nombre maximal de membres de la session et le nombre de membres de la session est supérieure à 2 | « session/maxMembersCountRemaining gt 2 »
 Où la différence entre le nombre de cibles de membre de session et le nombre de membres de la session est inférieure ou égale à 15.</br> Si le rôle n’a pas une cible spécifiée, cette requête filtre par rapport à la différence entre le nombre maximal de membres de la session et le nombre de membres de la session. | « session/doit le 15 »
 Rôle « confirmé » du type rôle « lfg » où le nombre de membres avec ce rôle est égal à 5 | « nombre de sessions/rôles/lfg/confirmé/eq 5 »
 Rôle « confirmé » du type rôle « lfg » où la cible de ce rôle est supérieure à 1.</br> Si le rôle n’a pas une cible spécifiée, la valeur maximale du rôle est utilisée à la place. | « gt/rôles/lfg/confirmé/cible de la session 1 »
 Type de rôle « confirmé » du rôle « lfg » où la différence entre la cible du rôle et le nombre de membres avec ce rôle est inférieure ou égale à 15.</br> Si le rôle n’a pas une cible spécifiée, cette requête filtre par rapport à la différence entre la valeur maximale du rôle et le nombre de membres avec ce rôle. | « / rôles/lfg/confirmé/besoins de votre session le 15 »
 Tous les descripteurs de recherche qui pointent vers une session qui contient un mot clé spécifique | "session/keywords/any(d:tolower(d) eq 'level2')"
 Tous les handles qui pointent vers une session appartenant à un scid particulier de recherche | "session/scid eq '151512315'"
 Tous les descripteurs de recherche qui pointent vers une session qui utilise un nom de modèle particulier | "session/templateName eq 'mytemplate1'"
 Tous les handles qui ont la balise « elite » ou un nombre 'canons' supérieures à 15 et « clan' égal à « violet » de la chaîne de recherche | « tags/any(a:tolower(a) eq « elite ») ou nombre/canons gt 15 et chaîne/clan eq 'violet' »

### <a name="refreshing-search-results"></a>Actualiser les résultats de recherche

 Votre jeu doit éviter d’actualiser automatiquement la liste des sessions, mais plutôt pour fournir l’interface utilisateur qui permet à un lecteur actualiser manuellement la liste (éventuellement après affiner les critères de recherche pour mieux filtrer les résultats).

 Si un joueur tente de rejoindre une session, mais cette session est plein ou fermé, votre jeu doit actualiser les résultats.

 Trop de recherche actualisations peuvent entraîner la limitation du service, donc votre titre doit limiter le taux auquel la requête peut être actualisée.

 Pour réduire le volume d’appels de service, les handles de recherche incluent des propriétés de session personnalisée qui peuvent être utilisées pour stocker et interroger les attributs de session rapide évolution. Ces attributs ne doivent pas être stockées dans les attributs de recherche.

### <a name="example-query-for-search-handles"></a>Exemple : requête pour les descripteurs de recherche

 Le code suivant montre comment interroger pour les descripteurs de recherche. L’API retourne une collection de `multiplayer_search_handle_details` objets qui représentent tous les descripteurs de recherche qui correspondent à la requête.

```cpp
 auto result = multiplayer_service().get_search_handles(scid, template, orderBy, orderAscending, searchFilter)
 .then([](xbox_live_result<std::vector<multiplayer_search_handle_details>> result)
 {
   if (result.err())
   {
      // handle error
   }
   else
   {
      // parse result.payload
   }
 });

 /* Payload element properties

 multiplayer_search_handle_details
 {
   string_t& handle_id();
   multiplayer_session_reference& session_reference();
   std::vector<string_t>& session_owner_xbox_user_ids();
   std::vector<string_t>& tags();
   std::unordered_map<string_t, double>& numbers_metadata();
   std::unordered_map<string_t, string_t>& strings_metadata();
   std::unordered_map<string_t, multiplayer_role_type>& role_types();
 }
 */
```

## <a name="join-a-session-by-using-a-search-handle"></a>Rejoindre une session à l’aide d’un handle de recherche

Une fois que vous avez récupéré un handle de recherche pour une session que vous souhaitez joindre, le titre doit utiliser `MultiplayerService::WriteSessionByHandleAsync()` ou `multiplayer_service::write_session_by_handle()` de s’ajouter à la session.

> [!NOTE]
> Le `WriteSessionAsync()` et `write_session()` méthodes ne peuvent pas être utilisées pour rejoindre une session de navigation de session.

Le code suivant montre comment rejoindre une session après avoir récupéré un handle de recherche.

```cpp
void Sample::BrowseSearchHandles()
{
    auto context = m_liveResources->GetLiveContext();
    context->multiplayer_service().get_search_handles(...)
    .then([this](xbox_live_result<std::vector<multiplayer_search_handle_details>> searchHandles)
    {
        if (searchHandles.err())
        {
            LogErrorFormat( L"BrowseSearchHandles failed: %S\n", searchHandles.err_message().c_str() );
        }
        else
        {
            m_searchHandles = searchHandles.payload();

            // Join the game session

            auto handleId = m_searchHandles.at(0).handle_id();
            auto sessionRef = multiplayer_session_reference(m_searchHandles.at(0).session_reference());
            auto gameSession = std::make_shared<multiplayer_session>(m_liveResources->GetLiveContext()->xbox_live_user_id(), sessionRef);
            gameSession->join(web::json::value::null(), false, false, false);

            context->multiplayer_service().write_session_by_handle(gameSession, multiplayer_session_write_mode::update_existing, handleId)
            .then([this, sessionRef](xbox_live_result<std::shared_ptr<multiplayer_session>> writeResult)
            {
                if (!writeResult.err())
                {
                    // Join the game session via MPM
                    m_multiplayerManager->join_game(sessionRef.session_name(), sessionRef.session_template_name());
                }
            });
        }
    });
}
```
