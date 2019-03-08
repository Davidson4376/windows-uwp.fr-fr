---
title: Xbox Live de la limitation du débit de granularité fine
description: Découvrez comment Xbox Live affinée limitation du débit fonctionne et comment empêcher votre titre ne soient pas taux limité.
ms.assetid: ceca4784-9fe3-47c2-94c3-eb582ddf47d6
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, jeux, uwp, windows 10, xbox, la limitation, la limitation de la fréquence
ms.localizationpriority: medium
ms.openlocfilehash: b79a4f0a873ffaf5dd824a0c9832f61aa4a7d77f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57628684"
---
# <a name="xbox-live-fine-grained-rate-limiting"></a>Xbox Live de la limitation du débit de granularité fine

## <a name="introduction"></a>Introduction

Cet article fournit une vue d’ensemble de Xbox Live de la limitation du débit de granularité fine. En plus de résumant les limitation du débit est, que ce livre envisage également de vous aider à déterminer si vous n’êtes pas en cours limité, et si vous êtes, les outils et les ressources sont à votre disposition.

## <a name="terminology-guide"></a>Guide de la terminologie

| Terme         | Définition                                                  |
|--------------|-----------------------------------------------------------------------------
| FGRL         | Affinées limitation du débit                                                  
| XSAPI        | Xbox Services Application Program Interface                                 
| CU           | Mise à jour de contenu                                                              
| Rafale        | Représente un volume de demandes reçues sur une courte période de temps          
| Prendre en charge      | Représente un volume élevé d’appels reçus en permanence sur une période de temps
| Utilisateur + titre | Représente l’association d’un utilisateur et une fonction comme une seule entité                    
| XLTA         | Outil Analyseur de Trace Xbox Live, utilisé pour déterminer si votre titre est en cours de restriction

## <a name="xbox-live-fine-grained-rate-limiting"></a>Xbox Live de limitation du débit de granularité Fine

**Affiner le plus précis de la limitation du débit** a été mis en service deviendront pour promouvoir **juste l’utilisation** de ressources partagées Xbox Live parmi différents titres. Cette solution est comme la plupart des systèmes limites qui ont un service en conservant un décompte du nombre de demandes qu'une entité a effectuées dans une période donnée.

Les entités qui atteignent la limite spécifiée de services sont déplacées vers un état de rejet où toutes les demandes entrantes à partir de l’entité seront activés immédiatement. Les entités ne sont en mesure de quitter cet état lorsque la période de temps donnée arrive à expiration à l’origine le nombre associé entités à réinitialiser.

Bien plus précis limitation du débit utilise le même mécanisme core mentionné ci-dessus toutefois au lieu d’une entité de suivi FGRL effectue le suivi de la combinaison de **utilisateur et le titre** et compare le nombre associé à **deux** différents **limites** comme s’opposer à un. Limites de double FGRL sont appliquées sur chaque service, ce qui signifie que le nombre de demandes pour GameClips n’affecte pas le nombre de demandes de présence. Les sections suivantes prendront plus de détails sur l’utilisateur et titre jumelage limitant double et l’objet de réponse HTTP 429 limitation.

## <a name="fair-usage"></a>Utilisation juste

Xbox Live est convaincu que chaque utilisateur doit avoir la même qualité ne rencontrer aucune question quel jeu/de l’application qu’il est lu. FGRL est conçu pour résoudre le scénario suivant :

Le développeur A vient de sortir un titre qui suit tous les Xbox live de meilleures pratiques en garantissant une utilisation optimale des services, tandis que le développeur B vient également de sortir un titre toutefois celle-ci comporte un bogue inconnu. Ce bogue provoque le titre et chaque utilisateur d’envoyer du courrier indésirable de présence, ce qui entraîne le service va sous une charge importante. Le service ralentit et finit par s’arrête avec rupture de l’expérience des utilisateurs de développement d’un même s’il s’agissait bogue de développeur B qui a provoqué le problème. Si FGRL était implémenté le service aurait été en mesure d’arrêter de recevoir des demandes à partir de l’absence dans le comporte lui permettant de titre pour servir de titre de développeur de sa tranche de répartition de charge du secteur de la ressource.

## <a name="title-and-user-granularity"></a>Granularité de titre et l’utilisateur

Titre et l’utilisateur ont été choisis comme clé pour garantir une utilisation juste des ressources de Xbox Live. Suivi simplement l’utilisateur créerait un scénario dans lequel l’expérience utilisateur serait à la merci de chaque intégration de titres. Par exemple, la plupart des titres utiliser le service de personnes déjà par conséquent, cet exemple, supposons que bien plus précis limitation du débit a été configuré sur le service de personnes autorisant pas plus de 100 demandes en 5 minutes. Si un utilisateur à un jeu qui a effectué 100 demandes dans 1 minute serait dépassée la limite et que l’utilisateur ne serait pas en mesure d’effectuer d’autres demandes au service de personnes ; Imaginez que dans la même période de l’utilisateur puis revient à l’écran d’accueil et clique sur sa liste d’amis : Étant donné que l’utilisateur est déjà ont dépassé la limite qui appel de liste d’amis échouait jusqu'à 5 minutes est écoulé, même si l’écran d’accueil n’a pas chargé de remettre l’utilisateur dans un état limité.

Vous pouvez également limiter basé uniquement sur le titre produirait un résultat tout aussi déloyal. Définition d’une limite par titre ignorerait la popularité des titres et les demandes simplement être tout d’abord entre tout d’abord répondre jusqu'à ce qu’une limite est atteinte.

L’association de l’utilisateur et le titre garantit qu’aucun titre n’utilise davantage de ressources que ce qui est approprié étant donné le nombre d’utilisateurs actifs tout en offrant à chaque utilisateur un secteur cohérent du graphique de ressource.

![](../../images/FGRL.png)

Le diagramme ci-dessus présente une vue de haut niveau de la façon dont la requête est gérée. Tout d’abord, la demande est générée et ensuite reçue par le service souhaité. Lorsqu’il reçoit la demande, le système vérifie combien de fois l’utilisateur et le titre ensemble ont accédé au service. Si la requête sous la limite, puis il sera traité comme d’habitude. Si la demande n’est pas supérieure ou égale à la limite les services qu’il supprime et renvoyer une réponse 429. La réponse indique la durée pendant laquelle jusqu'à ce que la période restaure sur et de gérer les demandes d’utilisateur et le titre.

## <a name="burst-and-sustain-limits"></a>Dépassement et de faire face aux limites

En règle générale, la restriction se compose d’une limite par point de terminaison qui est suivie sur une période donnée. Cette période représente la quantité de temps qu’un nombre de demandes d’entités est suivi. À la fin de la période, le nombre d’entités est réinitialisé à 0 pour commencer à surveiller à nouveau.

Cette approche fonctionne pour la plupart des API de toutefois il n’était pas suffisamment résilient pour les jeux et applications qui appellent Xbox live. La solution ci-dessus suppose que les personnes font appel un manor prévisible stable cohérent. Dans le cas de Xbox Live, selon le service et le titre de demande/application, les modèles d’appel sont considérablement différentes.

En choisissant simplement une limite dans ce cas nécessiterait transiger sur les deux extrémités du spectre de modèle d’appel. La solution Xbox Live utilise deux périodes et les limites. La période de plus petite est appelée la période de croissance tandis que celui de plus de temps supérieure est connu en tant que la période de maintenir. La durée des rafales période pour FGRL est toujours de 15 secondes tandis que la maintenir est toujours de 300 secondes (5 minutes) par conséquent, pendant une période de maintenir de 5 minutes sont 20 burst périodes. Les deux croître et à maintenir les limites sont de suivi en même temps et par conséquent compter les requêtes en même temps. La croissance et la limite de maintenir sont définies sur le service, ce qui signifie que chaque service a son propre nombre de rafale et maintenir. Pour vous aider à comprendre le fonctionnement de ces deux limites ensemble le tableau ci-dessous montre un utilisateur de lire un qui effectue un nombre de demandes de service qui a implémenté FGRL. Dans ce cas, la limite de croissance est de 30 demandes dans les 15 secondes et la limite de maintenir est 100 demandes pendant 5 minutes.

| Période de temps (en secondes)  | Nombre de requêtes par période de croissance  | Prendre en charge les demandes par période  | \# des demandes limitées dans l’intervalle de 15 s | Qui limitent ? (burst, prendre en charge, ou les deux)  |
|-------------|--------------|----------------|-----------------------------------------------------|---------------------------|
| 0-15        | 35           | 35             | 5                                                   | Rafale                     |
| 15-30       | 28           | 63             | 0                                                   | Non applicable                       |
| 30-45       | 21           | 84             | 0                                                   | Non applicable                       |
| 45-60       | 36           | 120            | 20                                                  | Les deux                      |
| 60-75       | 24           | 144            | 24                                                  | Prendre en charge                   |
| …           | …            | …              |                                                     | …                         |
| 285-300     | 4            | 148            |                                                     | Prendre en charge                   |

Le tableau indique que dans les 15 premières secondes courses utilisateur l’explosion limiter en effectuant des 35 demandes. Ces 5 demandes supplémentaires sont supprimées et 5 429 réponses sont envoyés. Il est important de noter que ces 5 demande cependant limitée toujours sont comptabilisées dans la limite de maintenir. Une fois que des limites sont dépassé aucune demande n’est transmis comme indiqué lorsque les limites courses à la marque de deuxième 45 et à nouveau lorsque seules 4 sont demandées au deuxième niveau 285 marquent.

## <a name="http-429-response-object"></a>HTTP 429 objet Response

Lorsque le nombre d’utilisateur et le titre associé est supérieure ou égale à une de l’explosion ou limite de prendre en charge le service ne traitera pas la requête et retourne à la place une réponse HTTP 429. Le code HTTP 429 signifie « trop de demandes » sera accompagné d’un en-tête contenant une valeur de « nouvelle tentative après X secondes ». FGRL 429 contient une nouvelle tentative après un en-tête qui spécifie la durée pendant laquelle que les entités appelantes doivent attendre avant de réessayer. Les développeurs qui utilisent XSAPI n’aura pas à vous soucier comme XSAPI honore et gère l’en-tête Retry-After. La réponse réelle contiendra les champs suivants :

| Nom du champ      | Type de valeur | Exemple                | Définition                       |
|-----------------|------------|------------------------|----------------------------------|
| Version         | Entier    | `"version":1`          |                                  |
| currentRequests | Entier    | `"currentRequests":13` | Nombre total de demandes envoyées    |
| maxRequests     | Entier    | `"maxRequests":10`     | Nombre total de demandes autorisées |
| periodInSeconds | Entier    | `“periodInSeconds”:15` | Fenêtre de temps                      |
| Type            | Chaîne     | `“type”:”burst”`       | Type de limite de limitation              |

## <a name="implemented-limits"></a>Limites de mise en œuvre

Les services suivants ont implémenté des limites FGRL, avec mise en œuvre de ces limites en place depuis **mai 2016**. Pour rappel, ces limites seront les mêmes sur tous les bacs à sable et les titres. **N’importe quel titre a été publié via la plateforme de développement Xbox ou partenaires et expédié avant mai 2016 sera considéré comme hérité et par conséquent exempté.**

| **Nom** | **Limite de rafale** (15 secondes par utilisateur et par titre) | **Limite de faire face aux** (300 secondes par utilisateur et par titre) | **Limite de certification** (10 x Sustained, 300 secondes par utilisateur et par titre) |
|----------------------------|---------------------------|----------------------------|----------------------------|
| Lecture des statistiques                 | 100                       | 300                        | 3000                       |
| Profil                    | 10                        | 30                         | 300                        |
| MPSD                       | 30                        | 300                        | 3000                       |
| Présence                   | Lecture 10, écriture 3          | Lecture 100, écriture 30         | 1000 lecture, écriture 300       |
| Social                     | 10                        | 30                         | 300                        |
| Classements               | 30                        | 100                        | 1000                       |
| Succès               | 100                       | 300                        | 3000                       |
| Correspondance active                | 10                        | 100                        | 1000                       |
| Billets de l’utilisateur                 | 100                       | 300                        | 3000                       |
| Écriture de statistiques                | 100                       | 300                        | 3000                       |
| Confidentialité                    | 10                        | 30                         | 300                        |
| Clubs                      | 10                        | 30                         | 300                        |

Le tableau ci-dessus représente la liste actuelle des services qui ont été sélectionnés pour FGRL. Cette liste n’est pas finale en tant que de nouveaux services et services existants peuvent être ajoutés. Quand un service va être ajouté à la table sera mis à jour et une annonce sera effectuée. Les limites représentées dans la table doivent également pas être considérés comme la finalisation. Quand des services changent et évoluent de façon trop seront les limites Toutefois, vous êtes averti et les exemptions héritées nécessaires seront établies.

En tant que de **avril 2018** titres ne peut pas passer de certification s’ils dépassent la maintenir limiter (limite sur le taux de limitation en vigueur) multiplié par 10.  Par exemple, si la limite soutenue auxquelles FGRL prend effet est définie à 300 appels dans les 300 secondes, tel que spécifié dans le tableau ci-dessus, les titres supérieure ou égale à 3000 appels dans les 300 secondes échouera certification. 

## <a name="service-mapping-and-title-effects-of-rate-limiting"></a>Mappage de service et les effets de titre de la limitation du débit

| **Nom** | **Service Endpoint** | **Impact du jeu de Anticipiated de FGRL**
| --- | :---: | --- 
| Lecture des statistiques | userstats.xboxlive.com | Entrées de primes ou Leaderboards pas mis à jour ou à récupérer.
| Profil | profile.xboxlive.com | Les données du joueur mis à jour ni ne s’affiche correctement.
| MPSD | sessiondirectory.xboxlive.com | Jointures/invitations n’est pas terminée correctement, les sessions pas créé ou mis à jour correctement qui peut entraîner des échecs de titre.
| Présence | presence.xboxlive.com | Présence dans le jeu de joueur ne serait pas précis.
| Social | social.xboxlive.com | A un impact sur toutes les écritures d’amis (par exemple, ajout d’un ami, effectue une personne favoris etc.) et peut avoir un impact sur les lectures friend (par exemple, extraire la liste de mes amis). Les développeurs sont encouragés à appeler le peoplehub pour lecture plutôt que social.xboxlive.com.
| Classements | leaderboards.xboxlive.com | L’expérience utilisateur dans le jeu de tableaux de résultats n'aurait pas remplir/mettre à jour.
| Succès | achievements.xboxlive.com | L’expérience utilisateur dans le jeu de primes déverrouillé ne serait pas mis à jour.
| Correspondance active | momatch.xboxlive.com | Correspondances ne seraient pas correctement configurées.
| Billets de l’utilisateur | userposts.xboxlive.com | Billets de l’utilisateur n’apparaissent pas.
| Écriture de statistiques | statswrite.xboxlive.com | Entrées de primes ou Leaderboards ne pas mis à jour.
| Confidentialité | privacy.xboxlive.com | Blocage de l’accès peuvent entraîner des échecs de confidentialité pour tous les appelants.
| Clubs | Clubhub.xboxlive.com | Le lecteur n’est peut-être pas en mesure de voir leur clubs dans le jeu.

**REMARQUE :** Le mappage d’API plus récente est régulièrement mis à jour et peut être trouvé sous [mappage d’API Live Trace analyseur](https://github.com/Microsoft/xbox-live-trace-analyzer/blob/master/Source/XboxLiveTraceAnalyzer.APIMap.csv).

## <a name="faq"></a>Forum Aux Questions

## <a name="how-can-i-determine-i-am-being-throttled-and-what-steps-can-i-take"></a>Comment puis-je déterminer je suis limité et quelles mesures puis-je prendre ?

Reportez-vous à la [Xbox Live meilleures pratiques](best-practices-for-calling-xbox-live.md) document car il contient les étapes pour améliorer votre modèle d’appel, ainsi que pour une explication de la façon dont l’assertion de XSAPI et responsables sociaux XSAPI et mode multijoueur peuvent servir à vous avertir d’et atténuer les problèmes de limitation.

Une autre option consiste à enregistrer une trace des appels Xbox Live et les analyser cette trace à l’aide de la [outil Analyseur de Trace Xbox Live](https://docs.microsoft.com/windows/uwp/xbox-live/tools/analyze-service-calls).  Pour enregistrer une trace, vous pouvez utiliser Fiddler pour enregistrer un. Fichier SAZ, ou utiliser la journalisation de suivi intégré des XSAPI. Pour plus d’informations, comment utiliser activer le suivi dans XSAPI reportez-vous à la page de documentation Xbox Live « Analyser les appels aux Services Live Xbox ». Une fois que vous avez une trace, l’analyseur Xbox Live Trace outil avertira lors de la détection limitée des appels.

Vous pouvez trouver les meilleures pratiques papier sur docs GDNP et Kit de développement logiciel et XDK 1602 et versions ultérieures.

### <a name="can-limits-change"></a>Peuvent modifier les limites ?

L’objectif est que les limites publiées ne changeront pas au fil du temps. Toutefois, si le besoin devait se produire, il est possible que certaines limites a pu être établie à la plus stricte ; Dans ce cas, les titres déjà mis à la vente au détail être apportées exemptés de la limite de mise à jour.

### <a name="are-more-services-going-to-get-limits"></a>Plus de services vont parvenir limites ?

Oui, plus de services et de nouveaux services peuvent et vont être création de limites. Cependant simplement comme la première version FGRL vous êtes averti et vous allez prendre les précautions appropriées.

### <a name="when-will-these-changes-take-effect"></a>Lorsque ces modifications prendra effet ?

Limites de taux ont été appliquées depuis **mai 2016**.  En tant que de **avril 2018**, titres dépassant spécifié subi des limites par x 10 ou plus ne peut pas passer le processus de Certification de la Xbox.

### <a name="what-if-we-cant-adhere-to-the-limits"></a>Que se passe-t-il si nous ne pouvons pas conforme aux limites ?

Veuillez consulter la [Xbox Live meilleures pratiques](best-practices-for-calling-xbox-live.md) et vérifiez que vous suivez ces étapes.  Envisagez également d’utiliser le [manager sociaux](../../social-platform/intro-to-social-manager.md) si vous êtes toujours soumis à restriction avec un des services sociaux.

Si après avoir suivi ces étapes, vous ne parvenez pas à restent inférieures aux limites, veuillez contacter votre responsable de compte développeur.

**REMARQUE : Titres supérieure ou égale à 10 fois la limite de maintenir spécifié ne pourrez pas passer le certificat après avril 2018**.  Par exemple, si la limite soutenue auxquelles FGRL prend effet est définie à 300 appels dans les 300 secondes, tel que spécifié dans le tableau ci-dessus, les titres supérieure ou égale à 3000 appels dans les 300 secondes échouera certification.

### <a name="what-about-my-existing-title"></a>Qu’en est-il de mon titre existant ?

Aucun titre de la vente au détail avant avril 2018 est considérés comme hérité et exemptés.

### <a name="content-updates"></a>Les mises à jour ?

Pour un logiciel hérité ou exempté, mises à jour de contenu sera également exemptés, bien que nous vous encourageons vivement à exploiter les outils et les ressources pour optimiser les aspects de l’intégration de service de votre jeu.

### <a name="can-i-get-an-exemption-for-my-game-until-i-can-make-a-content-update"></a>Trouver une exemption de mon jeu jusqu'à ce que je peux effectuer une mise à jour de contenu ?

Veuillez prendre contact avec votre responsable de compte de développeur.
