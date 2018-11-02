---
author: joannaleecy
title: Utilisation des services de cloud computing pour les jeux UWP
description: Apprenez-en davantage sur l’implémentation de services de cloud computing en tant que serveur principal pour vos jeux UWP.
ms.assetid: 1a7088e0-0d7b-11e6-8e05-0002a5d5c51b
ms.author: joanlee
ms.date: 03/27/2018
ms.topic: article
keywords: Windows10, uwp, jeux, services cloud
ms.localizationpriority: medium
ms.openlocfilehash: 5d15d3e6b6beb773a8d606db7a5d8a17544270be
ms.sourcegitcommit: 144f5f127fc4fbd852f2f6780ef26054192d68fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/02/2018
ms.locfileid: "5975572"
---
#  <a name="using-cloud-services-for-uwp-games"></a>Utilisation des services de cloud computing pour les jeux UWP

La plateforme Windows universelle (UWP) dans Windows10 propose un ensemble d’API qui peuvent être utilisées sur les appareils Microsoft pour le développement de jeux. Lorsque vous développez des jeux sur différentes plateformes et différents appareils, vous pouvez utiliser un serveur principal dans le cloud pour accroître leur portée en fonction de la demande.

Si vous recherchez une solution de serveur principal cloud complète pour votre jeu, consultez [Serveur principal de jeu et logiciels en tant que service](#software-as-a-service-for-game-backend).

##  <a name="what-is-cloud-computing"></a>Qu’est-ce que le cloud computing?

Le cloud computing utilise des ressources informatiques et des applications à la demande sur Internet pour stocker et traiter les données de vos appareils. Le terme _cloud_ est une métaphore faisant référence à la disponibilité des vastes ressources qu’il contient (ressources non locales). Vous accédez à celles-ci à partir d’emplacements non spécifiques.
Le principe de cloud computing offre une nouvelle façon de consommer les ressources et les logiciels. Les utilisateurs n’ont plus besoin de payer à l’avance pour l’intégralité du produit ou des ressources. Au lieu de cela, ils peuvent consommer la plateforme, les logiciels et les ressources en tant que service. Les fournisseurs de services de cloud computing facturent souvent leurs clients d’après des plans d’utilisation ou de services.

##  <a name="why-use-cloud-services"></a>Pourquoi utiliser des services de cloud computing?

L’un des avantages à utiliser des services de cloud computing pour les jeux réside dans le fait que vous n’avez pas besoin d’investir dans des serveurs physiques ni de les payer au comptant. Vous avez uniquement besoin de payer a posteriori en fonction du plan d’utilisation ou de services mis en place. C’est un moyen de gérer les risques induits par le développement d’un nouveau titre de jeu. 

Un autre avantage réside dans le fait que votre jeu peut s’appuyer sur un large éventail de ressources de cloud computing pour gagner en évolutivité (gestion efficace des hausses soudaines du nombre de joueurs simultanés, des calculs de jeu en temps réel ou des exigences liées aux données). Cela préserve la stabilité des performances de votre jeu, à tout moment. En outre, les ressources de cloud computing sont accessibles depuis tout type d’appareil s’exécutant sur tout type de plateforme, n’importe où dans le monde. Cela signifie que vous êtes en mesure de proposer votre jeu aux utilisateurs du monde entier.

Il est important de proposer aux joueurs une expérience de jeu incroyable. Étant donné que les serveurs de jeu qui s’exécutent dans le cloud ne dépendent pas de mises à jour côté client, ils offrent à votre jeu un environnement mieux contrôlé et plus sécurisé dans l’ensemble.   Vous pouvez également parvenir à une cohérence de jeu dans le cloud en ne faisant jamais confiance au client et en adoptant une logique de jeu côté serveur. Rien ne vous empêche non plus de configurer les connexions d’un service à l’autre pour parvenir à une expérience de jeu mieux intégrée. Par exemple, vous pouvez lier les achats dans le jeu à différents modes de paiement, créer une passerelle entre différents réseaux de jeux et partager les mises à jour dans le jeu sur les portails de réseaux sociaux les plus courants comme Facebook et Twitter. 

Vous pouvez également utiliser des serveurs de cloud computing dédiés pour créer un vaste univers de jeu permanent, construire une communauté de joueurs, collecter et analyser les données des joueurs dans le temps afin d’améliorer l’expérience de jeu et optimiser le modèle de conception de monétisation de votre jeu.

Par ailleurs, les jeux qui nécessitent des fonctionnalités de gestion des données de jeu et beaucoup de ressources (par exemple, les jeux sociaux dotés de mécanismes multijoueurs asynchrones) peuvent être implémentés à l’aide de services de cloud computing.

##  <a name="how-game-companies-use-the-cloud-technology"></a>Comment les entreprises spécialisées dans le développement de jeux vidéo utilisent la technologie de cloud computing

Découvrez comment des développeurs ont implémenté des solutions de cloud computing dans leurs jeux.

<table>
    <colgroup>
    <col width="10%" />
    <col width="30%" />
    <col width="30%" />
    <col width="30%" />
    </colgroup>
    <tr class="header" align="left">
        <th>Développeur</th>
        <th>Description</th>
        <th>Principaux scénarios de jeu</th>
        <th>En savoir plus</th>
    </tr>
    <tr>
        <td><a href="https://www.tencent.com">Jeux Tencent</a></td>
        <td><b>Tencent Games</b> a développé une solution innovante à l'aide d'Azure Service Fabric qui permet de distribuer des jeux PC traditionnels sous forme de service. Sa solution de jeu cloud utilise un modèle «client léger + cloud riche» exécutant des charges de travail en tant que microservices dans le système principal.</td>
        <td>
            <ul>
                <li>Les jeux PC traditionnels sont fournis aux utilisateurs sous forme de jeux cloud dans le monde entier <li>Processus de distribution de jeux optimisé <li>Fonctionnalités de jeu isolées sous forme de microservices pour diminuer la complexité, réduire la répétition des charges de travail en raison des dépendances et donner la possibilité de mettre à niveau les nouvelles fonctionnalités de manière indépendante <li>Petits téléchargements de package d’installation pour lire les contenus de jeu les plus récents (taille du package réduite pour passer de Go à des Mo) <li>Réduction des coûts de maintenance </ul>
        </td>
        <td>
            <ul>
                <li><a href="https://customers.microsoft.com/story/tencent-telecommunications-azure-service-fabric-windows-server-en">Tencent Games et Microsoft ont conçu la solution de jeu cloud</a>
                <li><a href="https://channel9.msdn.com/Shows/Cloud+Cover/Episode-228-Building-Games-with-Service-Fabric#time=38m33s">Création de jeux à l'aide de Service Fabric: informations détaillées sur l’implémentation de Tencent (vidéo)</a>
            </ul>
        </td>
    </tr>
    <tr>
        <td><a href="https://www.halowaypoint.com/">343 Industries</a></td>
        <td><b>Halo5: Guardians</b> a mis en œuvre <a href="https://www.halowaypoint.com/spartan-companies">Halo: Spartan Companies</a> et en a fait sa plateforme de jeu sur les réseaux sociaux en s’appuyant sur Azure Cosmos DB (via l'API DocumentDB), une solution choisie pour sa rapidité et sa flexibilité en raison de ses fonctionnalités d’indexation automatique.</td>
        <td>
            <ul>
                <li>Couche Données évolutive pour gérer la création de groupes et l’expérience de jeu en mode multijoueur <li>Intégration des jeux et des réseaux sociaux <li>Demandes de données en temps réel par le biais de plusieurs attributs <li>Synchronisation des scores et statistiques de jeu </ul>
        </td>
        <td>
            <ul>
                <li><a href="https://azure.microsoft.com/blog/how-halo-5-guardians-implemented-social-gameplay-using-azure-documentdb/">Implémentation d’une expérience de jeu sur les réseaux sociaux avec Azure Cosmos DB (via l'API DocumentDB)</a>
            </ul>
        </td>
    </tr>
    <tr>
        <td><a href="http://web.ageofascent.com/">Illyriad Games</a></td>
        <td>Illyriad Games a créé <b>Age of Ascent</b>, un jeu3D épique dans l’espace de type MMO (en ligne massivement multijoueur) auquel il est possible de jouer sur les appareils équipés de navigateurs modernes. Par conséquent, ce jeu est utilisable sans plug-in sur des PC, ordinateurs portables, téléphones portables et autres appareils mobiles. Le jeu utilise ASP.NET Core, HTML5, WebGL et Azure.</td>
        <td>
            <ul>
                <li>Jeu multiplateforme sur navigateur <li>Grand monde ouvert, persistant et unique <li>Gestion d’importants calculs de jeu en temps réel <li>Évolutivité en fonction du nombre de joueurs </ul>
        </td>
        <td>
            <ul>
                <li><a href="https://channel9.msdn.com/Shows/Cloud+Cover/Episode-228-Building-Games-with-Service-Fabric#time=06m52s">Création de jeux avec Service Fabric: jeu MMO d’Age of Ascent (vidéo)</a>
                <li><a href="https://channel9.msdn.com/Events/Build/2016/KEY02#time=57m20s">Gestion des composants de jeu en tant que microservices avec Azure Service Fabric (vidéo)</a> 
                <li><a href="https://channel9.msdn.com/Shows/Azure-Friday/Age-of-Ascent-from-Illyriad-Powered-by-Azure-Service-Fabric-and-ASPNET">Entretien avec les développeurs d’Age of Ascent (vidéo)</a>
            </ul>
        </td>
    </tr>
    <tr>
        <td><a href="http://www.nextgames.com/">Next Games</a></td>
        <td>Next Games est le créateur du jeu vidéo <b>The Walking Dead: No Man’s Land</b> d’après une série originale d’AMC. Le jeu The Walking Dead utilisait Azure comme serveur principal. Le week-end de son ouverture, 1000000de téléchargements ont eu lieu. Dès la première semaine, il était numéro1 des applications gratuites de l’AppStore sur iPhone et iPad aux États-Unis, numéro1 des applications gratuites dans 12pays et numéro1 des jeux gratuits dans 13pays.
        </td>
        <td>
            <ul>
                <li>Système multiplateforme <li>Mode multijoueur <li>Performances élastiques à grande échelle <li>Protection du joueur contre les fraudes <li>Distribution dynamique de contenu </ul>
        </td>
        <td>
            <ul>
                <li><a href="https://azure.microsoft.com/blog/how-we-built-it-next-games-global-online-gaming-platform-on-azure/">Comment nous l’avons réalisé: plateforme de jeu en ligne mondiale de Next Games sur Azure (blog avec vidéo)</a>
                <li><a href="https://azure.microsoft.com/blog/the-walking-dead-no-mans-land-game-soars-to-1-with-azure-documentdb/">The Walking Dead utilise Azure Cosmos DB (via l'API DocumentDB) pour un cycle de développement plus rapide et une expérience de jeu plus conviviale</a>
            </ul>
        </td>
    </tr>
    <tr>
        <td><a href="http://www.crimecoast.com/">Pixels Squad</a></td>
        <td>Pixels Squad a développé <b>Crime Coast</b> à l’aide du moteur de jeu Unity et d’Azure. <b>Crime Coast</b> est un jeu social de stratégie disponible sur les plateformes Android, iOS et Windows. Ont été utilisés dans ce jeu le stockage d’objets Blob Azure, un cache Azure Redis géré, un ensemble d’ordinateurs virtuels IIS à charge équilibrée et un hub de notification Microsoft. Découvrez comment cette société a géré la mise à l’échelle et l’augmentation du nombre de joueurs avec 5000joueurs simultanés.
        </td>
        <td>
            <ul>
                <li>Système multiplateforme <li>Jeu en ligne en mode multijoueur <li>Système évolutif en fonction du nombre de joueurs </ul>
        </td>
        <td>
            <ul>
                <li><a href="https://channel9.msdn.com/Blogs/The-Game-Blog/BizSpark-Interview-with-Pixel-Squad-How-the-used-Azure-Cloud-Services-to-make-an-MMO-with-a-3-man-te">Comment le jeu MMO Crime Coast a utilisé Azure Cloud Services</a>
            </ul>
        </td>
    </tr> 
</table>

    
### <a name="other-links"></a>Autres liens

* [Hitman et Azure: créent des fonctions de jeu telles que la cible insaisissable qui ne sont possibles qu'en utilisant le cloud](https://channel9.msdn.com/Series/Hitman)
* [Azure, l’arme secrète de Hitcents, Game Troopers et InnoSpark](http://news.microsoft.com/features/game-developers-use-microsoft-azure-as-secret-sauce-for-scale-and-growth-2/)
* [Les start-up du jeu du programme Bizspark utilisent Azure](https://blogs.technet.microsoft.com/bizspark_featured_startups/2015/09/25/azure-open-for-gaming-startups/)


## <a name="how-to-design-your-cloud-backend"></a>Comment concevoir votre serveur principal dans le cloud

Alors que les fabricants et les concepteurs de jeu discutent des fonctions et fonctionnalités nécessaires, nous vous recommandons de vous pencher sur la façon dont vous voulez concevoir votre infrastructure de jeu. À partir du moment où vous voulez développer des jeux pour différents appareils et les principales plateformes, pensez à Azure comme serveur principal pour votre jeu.

### <a name="understanding-iaas-paas-or-saas"></a>Comprendre IaaS, PaaS ou SaaS

Tout d’abord, vous devez prendre en considération le niveau de service le mieux adapté à votre jeu. Une bonne connaissance des trois services suivants et de leurs différences peut vous aider à déterminer l’approche à adopter pour créer votre serveur principal.

* [Infrastructure en tant que service (IaaS)](https://azure.microsoft.com/overview/what-is-iaas/)

    L’infrastructure en tant que service (IaaS) est une infrastructure informatique instantanée, mise en service et gérée via Internet. Imaginez pouvoir utiliser de nombreuses machines immédiatement disponibles pour rapidement augmenter ou réduire votre offre, en fonction de la demande. L’infrastructure en tant que service vous permet d’éviter d’acheter vos propres serveurs physiques et toute autre infrastructure de centre de données, ainsi qu’elle vous libère de la complexité de tels systèmes.

* [Plateforme en tant que service (PaaS)](https://azure.microsoft.com/overview/what-is-paas/)

    La plateforme en tant que service (PaaS) est comme l’infrastructure en tant que service, à ceci près qu’elle intègre également une gestion de l’infrastructure comme les serveurs, le stockage et la mise en réseau. Aussi, non seulement vous n’êtes pas obligé d’acheter des serveurs physiques et une infrastructure de base de données, mais vous n’êtes pas obligé non plus d’acheter ni de gérer des licences logicielles, une infrastructure applicative sous-jacente, un middleware, des outils de développement ou autres ressources.

* [Software as a service (SaaS)](https://azure.microsoft.com/overview/what-is-saas/)

    SaaS (Software as a Service) permet aux utilisateurs de se connecter à des applications basées sur le cloud et de les utiliser sur Internet. Il fournit une solution logicielle complète que vous achetez à un fournisseur de services cloud sur le modèle du paiement à l'utilisation.  Les exemples courants sont les outils de messagerie, de calendrier et de bureautique (tels que MicrosoftOffice365). Vous louez l’utilisation d’une application pour votre organisation et vos utilisateurs s’y connectent via Internet, généralement à l'aide d'un navigateur web. Toute l’infrastructure sous-jacente, les intergiciels (middleware), les logiciels d’application et les données d’application se trouvent dans le centre de données du fournisseur de services. Le fournisseur de services gère les matériels et les logiciels, et avec le contrat de service approprié, garantit également la disponibilité et la sécurité du jeu et de vos données. SaaS permet à votre organisation de rendre rapidement une application opérationnelle avec une avance de coût minimale.


### <a name="design-your-game-infrastructure-using-azure"></a>Conception de votre infrastructure de jeu avec MicrosoftAzure

Voici quelques-unes des façons dont vous pouvez utiliser les offres de services de cloud computing Azure pour un jeu. Azure fonctionne avec Windows, Linux et les technologies Open Source que vous connaissez comme Ruby, Python, Java et PHP. Pour plus d’informations, voir [Azure pour les jeux](https://azure.microsoft.com/solutions/gaming/).

| Configuration requise                 | Scénarios d’activité                            | Offre de produit                      | Fonctionnalités de produit                                    |
|-----------------------------------|-----------------------------------------------|---------------------------------------|----------------------------------------------------|
| Hébergement de votre domaine dans le cloud     | Répondre efficacement aux requêtes DNS            | [Azure DNS](https://azure.microsoft.com/services/dns/) | Hébergement de votre domaine avec des performances et une disponibilité élevées  |
| Connexion et vérification d’identité      | Le joueur se connecte et son identité est authentifiée  | [Azure Active Directory](https://azure.microsoft.com/services/active-directory/) | Authentification unique à toute application web sur site et dans le cloud avec une authentification multifacteur            | 
| Jeu utilisant le modèle Infrastructure en tant que service (IaaS)      | Le jeu est hébergé sur des ordinateurs virtuels dans le cloud       | [Ordinateurs virtuels Azure](https://azure.microsoft.com/services/virtual-machines/) | Mise à l’échelle de 1 à plusieurs milliers d’instances d’ordinateur virtuel en tant que serveurs de jeu avec mise en réseau virtuelle intégrée et équilibrage de la charge; cohérence hybride avec les systèmes sur site           |
| Jeux web ou sur appareil mobile utilisant le modèle Plateforme en tant que service (PaaS)            | Le jeu est hébergé sur une plateforme gérée                | [Azure App Service](https://azure.microsoft.com/services/app-service/) | Plateforme en tant que service pour les jeux sur site web ou sur appareil mobile (soit des ordinateurs virtuels Azure avec middleware/outils de développement/aide à la décision/gestion de base de données)   |
| Jeu cloud hautement disponible et évolutif multiniveaux avec plus de contrôle du système d’exploitation (PaaS)        | Le jeu est hébergé sur une plateforme gérée                | [Azure Cloud Service](https://azure.microsoft.com/services/app-service/) | PaaS conçu pour prendre en charge des applications au fonctionnement évolutif, fiable et peu coûteux   |
| Équilibrage de charge entre les régions pour améliorer les performances et la disponibilité | Achemine les requêtes de jeu entrantes. Peut agir en tant que premier niveau de l’équilibrage de charge.       | [Azure Traffic Manager](https://azure.microsoft.com/en-us/services/traffic-manager/) | Offre de nombreuses options de basculement automatique et permet de distribuer le trafic équitablement ou avec des valeurs pondérées. Peut en toute transparence combiner des systèmes locaux et cloud. |
| Stockage dans le cloud des données de jeu       | Les dernières données de jeu sont stockées dans le cloud et envoyées aux appareils des clients | [Stockage d’objets Blob Azure](https://azure.microsoft.com/services/storage/blobs/)| Aucune restriction sur les types de fichiers qui peuvent être stockés; stockage d’objets pour de grandes quantités de données non structurées comme les images, les fichiers audio, vidéo et plus encore  |
| Tables de stockage de données temporaires| Les transactions de jeu (changements dans les états de jeu) sont stockées temporairement dans les tables | [Stockage de tables Azure](https://azure.microsoft.com/services/storage/tables/)| Les données de jeu peuvent être stockées dans un schéma flexible en fonction des besoins du jeu |
| Mise en file d’attente des transactions/requêtes de jeu| Les transactions de jeu sont traitées sous la forme d’une file d’attente | [Azure Queue Storage](https://azure.microsoft.com/services/storage/queues/)| Les files d’attente absorbent les pics de trafic inattendus et peuvent empêcher les serveurs d’être submergés par un flux soudain de demandes en cours de jeu   |
| Base de données de jeu relationnelle évolutive| Stockage structuré de données relationnelles de type transactions dans le jeu vers base de données | [Base de données SQL Azure](https://azure.microsoft.com/services/sql-database/)| Base de données SQL en tant que service ([comparaison avec SQL sur un ordinateur virtuel](https://azure.microsoft.com/documentation/articles/data-management-azure-sql-database-and-sql-server-iaas/))  |
| Base de données de jeu évolutive et distribuée à faible latence| Rapidité des lectures, écritures et requêtes de jeu et des données du joueur avec flexibilité du schéma | [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/)| Base de données de documents NoSQL à faible latence, gérée en tant que service   |
| Utilisation de votre propre centre de données avec les services Azure | Le jeu est récupéré depuis votre propre centre de données et envoyé sur les appareils du client | [Azure Stack](https://azure.microsoft.com/overview/azure-stack/) | Permet à votre organisation de transmettre des services WindowsAzure depuis votre propre centre de données pour plus d’efficacité  |
| Transfert de blocs de données volumineux| Les fichiers volumineux comme les images, les fichiers audio et les vidéos du jeu peuvent être envoyés aux utilisateurs à partir de l’emplacement POP CDN (réseau de diffusion de contenu) le plus proche avec AzureCDN    | [Azure Content Delivery Network](https://azure.microsoft.com/services/cdn/) | Basé sur une topologie réseau moderne avec nœuds centralisés de grande taille, AzureCDN gère les pics soudains de trafic et les charges élevées pour augmenter considérablement la vitesse et la disponibilité, ce qui conduit à une amélioration significative de l’expérience utilisateur  |
| Faible latence               | Effectuer une mise en cache pour créer des jeux rapides et évolutifs, avec plus de contrôle et de garantie d’isolation des données; peut également être utilisé pour améliorer la fonctionnalité de matchmaking | [Azure Redis Cache](https://azure.microsoft.com/services/cache/) | Débit élevé et accès aux données cohérent et à faible latence pour alimenter rapidement les applications Azure évolutives  |
| Évolutivité élevée, faible latence | Gère les fluctuations du nombre d’utilisateurs du jeu avec des lectures et des écritures à faible latence | [Azure Service Fabric](https://azure.microsoft.com/services/service-fabric/) | Possibilité d’alimenter les scénarios les plus complexes à faible latence avec volumes importants de données et d’assurer une mise à l’échelle fiable pour gérer un plus grand nombre d’utilisateurs à la fois Service Fabric vous permet de développer des jeux sans avoir à créer un magasin ou un cache séparé, tel que cela est demandé pour les applications sans état |
| Capacité à collecter des millions d’événements par seconde depuis les appareils                         | Consigner des millions d’événements par seconde depuis les appareils | [Concentrateurs d’événements Azure](https://azure.microsoft.com/services/event-hubs/) | Ingestion de télémétrie à l’échelle du cloud depuis les jeux, les sites web, les applications et les appareils  |
| Traitement des données de jeu en temps réel  | Effectuer une analyse en temps réel des données des joueurs pour améliorer l’expérience de jeu| [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) | Traitement du flux en temps réel dans le cloud  |
| Développement d’une expérience de jeu prédictive         | Créer une expérience de jeu dynamique et personnalisée en fonction des données du joueur  | [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) | Service cloud entièrement géré qui vous permet de facilement développer, déployer et partager des solutions d’analyse prédictive  |
| Collecte et analyse des données de jeu| Traitement parallèle massif des données à partir de bases de données relationnelles et non relationnelles | [Azure Data Warehouse](https://azure.microsoft.com/services/sql-data-warehouse/)| Entrepôt de données élastique en tant que service doté de fonctionnalités adaptées aux besoins des entreprises   |
| Inciter les utilisateurs à accroître leur utilisation et leur fidélisation| Envoyer des notifications Push ciblées à n'importe quelle plateforme à partir de n'importe quel système principal pour susciter l’intérêt et encourager des actions de jeu spécifiques | [Azure Notification Hubs](https://azure.microsoft.com/services/notification-hubs/)| Diffusion rapide en mode Push pour toucher des millions d’appareils mobiles sur toutes les plateformes principales &mdash; iOS, Android, Windows, Kindle, Baidu. Votre jeu peut être hébergé sur n’importe quel serveur principal &mdash; cloud ou local.|
| Diffuser du contenu multimédia en continu à votre public local et dans le monde entier tout en le protégeant| Permet de regarder des bandes annonces de jeu et des clips cinématographiques de qualité professionnelle à partir de tous les appareils| [Azure Media Services](https://azure.microsoft.com/services/media-services/)| Diffusion en continu de vidéo à la demande et en direct avec fonctionnalités de Content Delivery Network intégrées. Utiliser un lecteur pour tous vos besoins de lecture, avec chiffrement et protection du contenu inclus.| 
| Développer, distribuer et effectuer un bêta-test de vos applications mobiles | Tester et distribuer votre application mobile. Gestion des performances des applications et de l'expérience utilisateur. | [HockeyApp](https://azure.microsoft.com/services/hockeyapp/)| Intègre des rapports d’incidents et des métriques utilisateurs avec une plateforme de distribution d’applications et de commentaires des utilisateurs. Prend en charge les applications Android, Cordova, iOS, OS X, Unity, Windows et Xamarin. Vous pouvez également envisager [Visual Studio Mobile Center](https://www.visualstudio.com/vs/mobile-center/) &mdash; contrôle des missions pour les applications qui combinent analytique riche, rapports d'incidents, notifications Push, distribution d’applications et bien plus encore. |
| Création de campagnes marketing pour accroître l’utilisation et la fidélisation  | Envoyer des notifications push à certains joueurs pour susciter l’intérêt et encourager des actions de jeu spécifiques en fonction de l’analyse des données | [Mobile engagement](https://azure.microsoft.com/services/mobile-engagement/): sera retiré en mars2018 et n’est actuellement disponible que pour les clients existants |  Augmenter le temps de jeu et la fidélisation des utilisateurs sur toutes les plateformes principales (iOS, Android, Windows, WindowsPhone) |


##  <a name="startup-and-developer-resources"></a>Ressources pour start-up et développeurs

* [Microsoft for Startups](https://startups.microsoft.com)

    Microsoft for Startups offre des techniques et des produits, ainsi que des avantages pour la mise sur le marché, afin d’accélérer la croissance des start-up. Parmi ces avantages on trouve notamment l’accès gratuit à un compte Azure. Vous disposez de 200$ de crédit pour explorer des services pendant 30jours, d’un accès gratuit à des services populaires pendant 12mois et d’un accès gratuit illimité à plus de 25services. Pour plus d’informations, consultez la rubrique [Donnez vie aux idées de votre startup avec un compte gratuit Azure](https://azure.microsoft.com/free/startups/).
    
* [Programmes pour développeurs](e2e.md#developer-programs)

    Microsoft propose plusieurs programmes pour développeurs tels que [ID@Xbox](http://www.xbox.com/Developers/id) et le [Programme Créateurs Xbox Live](https://developer.microsoft.com/games/xbox/xboxlive/creator) pour vous aider à développer et à publier des jeux.

## <a name="learning-resources"></a>Ressources de formation

* //build 2016: [Codelabs &mdash; Utilisation de Microsoft Azure App Service et du serveur principal Microsoft SQL Azure pour enregistrer le score du jeu dans Unity](https://github.com/Microsoft-Build-2016/CodeLabs-GameDev-6-Azure)
* build 2017: [offrir des expériences de jeu à l’aide de Microsoft Azure: enseignements tirés de titres tels que Halo, Hitman et WalkingDead (vidéo)](https://channel9.msdn.com/Events/Build/2017/P4062)
* Ensemble réutilisable de blocs de construction, de projets, de services et des meilleures pratiques, conçu pour prendre en charge les charges de travail de jeu courantes utilisant Azure sur GitHub: [blocs de construction pour jeux sur Azure](https://github.com/MicrosoftDX/nether)
* [Services de jeu sur Azure (vidéos)](https://channel9.msdn.com/Series/Gaming-Services-on-Azure)

## <a name="tools-and-other-useful-links"></a>Outils et autres liens utiles

* [Forums MSDN &mdash; plateforme Azure](https://social.msdn.microsoft.com/Forums/azure/home?category=windowsazureplatform)
* [Outil de test de charge basée sur le cloud](https://www.visualstudio.com/team-services/cloud-load-testing/)
* [SDK et outils en ligne de commande](https://azure.microsoft.com/downloads/)
    
## <a name="software-as-a-service-for-game-backend"></a>Serveur principal de jeu et logiciels en tant que service

[Playfab](https://playfab.com/) alimente actuellement plus de 1200jeux en direct avec 80millions de joueurs actifs tous les mois. Il s’agit d’une plateforme principale complète qui inclut la pile complète LiveOps avec le contrôle en temps réel. 

Vous pouvez intégrer cette solution dans votre appareil mobile, votre PC ou les jeux de console à l’aide des kits de développement logiciel. Des kits de développement logiciel sont disponibles pour tous les moteurs et les plateformes de jeu populaires, y compris Android, iOS, Unreal, Unity et Windows. Pour commencer, consultez la rubrique [Documentation](https://api.playfab.com/).

Elle offre des services de jeu tels que l’authentification, la gestion des données du joueur, l’analyse multijoueur et en temps réel pour aider votre jeu à développer sa base d’utilisateurs. Tirez parti de l’utilisation du pipeline de données en temps réel et de LiveOps pour motiver vos utilisateurs avec des éléments de jeu personnalisés, des événements et des promotions. Vous avez également la possibilité d’effectuer des tests A/B, de générer des rapports, d’envoyer des notifications push et bien plus encore. 

Nous cherchons constamment à innover et à ajouter de nouvelles fonctionnalités. Pour plus d’informations, consultez la rubrique [Features](https://playfab.com/features/) et pour obtenir un tarif, consultez la rubrique [Simple pricing that scales with you](https://playfab.com/pricing/).

## <a name="related-links"></a>Liens connexes

* [Guide de développement de jeux Windows10](https://msdn.microsoft.com/windows/uwp/gaming/e2e)
* [Azure pour les jeux](https://azure.microsoft.com/solutions/gaming/)
* [Playfab](https://playfab.com/)
* [Microsoft for Startups](https://startups.microsoft.com)
* [ID@Xbox](http://www.xbox.com/Developers/id)


 

 
