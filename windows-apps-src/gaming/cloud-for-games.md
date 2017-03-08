---
author: joannaleecy
title: Utilisation des services de cloud computing pour les jeux UWP
description: "Apprenez-en davantage sur l’implémentation de services de cloud computing en tant que serveur principal pour vos jeux UWP."
ms.assetid: 1a7088e0-0d7b-11e6-8e05-0002a5d5c51b
ms.author: joanlee
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "Windows 10, uwp, jeux, services cloud"
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 72f357cbf8e370512f9230978de546aa3d54c660
ms.lasthandoff: 02/07/2017

---
#  <a name="using-cloud-services-for-uwp-games"></a>Utilisation des services de cloud computing pour les jeux UWP

La plateforme Windows universelle (UWP) dans Windows 10 propose un ensemble d’API qui peuvent être utilisées sur les appareils Microsoft pour le développement de jeux. Lorsque vous développez des jeux sur différentes plateformes et différents appareils, vous pouvez utiliser un serveur principal dans le cloud pour accroître leur portée en fonction de la demande.

##  <a name="what-is-cloud-computing"></a>Qu’est-ce que le cloud computing ?

Le cloud computing utilise des ressources informatiques et des applications à la demande sur Internet pour stocker et traiter les données de vos appareils. Le terme _cloud_ est une métaphore faisant référence à la disponibilité des vastes ressources qu’il contient (ressources non locales). Vous accédez à celles-ci à partir d’emplacements non spécifiques.
Le principe de cloud computing offre une nouvelle façon de consommer les ressources et les logiciels. Les utilisateurs n’ont plus besoin de payer à l’avance pour l’intégralité du produit ou des ressources. Au lieu de cela, ils peuvent consommer la plateforme, les logiciels et les ressources en tant que service. Les fournisseurs de services de cloud computing facturent souvent leurs clients d’après des plans d’utilisation ou de services.

##  <a name="why-use-cloud-services"></a>Pourquoi utiliser des services de cloud computing ?

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
    <tr class="header">
        <th>Développeur</th>
        <th>Description</th>
        <th>Principaux scénarios de jeu</th>
        <th>En savoir plus</th>
    </tr>
    <tr>
        <td>[343 Industries](https://www.halowaypoint.com/)</td>
        <td>_Halo 5: Guardians_ a mis en œuvre les [Spartan Companies](https://www.halowaypoint.com/spartan-companies) et en a fait sa plateforme de jeu sur les réseaux sociaux en s’appuyant sur Microsoft Azure DocumentDB, une solution choisie pour sa rapidité et sa flexibilité en raison de ses fonctionnalités d’indexation automatique.</td>
        <td>
            <ul>
                <li>Couche Données évolutive pour gérer la création de groupes et l’expérience de jeu en mode multijoueur <li>Intégration des jeux et des réseaux sociaux <li>Demandes de données en temps réel par le biais de plusieurs attributs <li>Synchronisation des scores et statistiques de jeu </ul>
        </td>
        <td>
            <ul>
                <li>[Implémentation d’une expérience de jeu sur les réseaux sociaux avec Azure DocumentDB](https://azure.microsoft.com/blog/how-halo-5-guardians-implemented-social-gameplay-using-azure-documentdb/)</td>
            </ul>
    </tr>
    <tr>
        <td>[Illyriad Games](http://web.ageofascent.com/)</td>
        <td>Illyriad Games a créé _Age of Ascent_, un jeu 3D épique dans l’espace de type MMO (en ligne massivement multijoueur) auquel il est possible de jouer sur les appareils équipés de navigateurs modernes. Par conséquent, ce jeu est utilisable sans plug-in sur PC, ordinateurs portables, téléphones portables et autres appareils mobiles. Le jeu utilise ASP.NET Core, HTML5, WebGL et Microsoft Azure.</td>
        <td>
            <ul>
                <li>Jeu multiplateforme sur navigateur <li>Grand monde ouvert, persistant et unique <li>Gestion d’importants calculs de jeu en temps réel <li>Évolutivité en fonction du nombre de joueurs </ul>
        </td>
        <td>
            <ul>
                <li>[Gestion des composants de jeu en tant que microservices avec Azure Service Fabric (vidéo)](https://channel9.msdn.com/Events/Build/2016/KEY02#time=57m20s)  
                <li>[Entretien avec les développeurs d’Age of Ascent (vidéo)](https://channel9.msdn.com/Shows/Azure-Friday/Age-of-Ascent-from-Illyriad-Powered-by-Azure-Service-Fabric-and-ASPNET)
            </ul>
        </td>
    </tr>
    <tr>
        <td>[Next Games](http://www.nextgames.com/)</td>
        <td>Next Games est le créateur du jeu vidéo _The Walking Dead: No Man’s Land_ d’après une série originale d’AMC. Le jeu The Walking Dead utilisait Azure comme serveur principal. Le week-end de son ouverture, 1 000 000 de téléchargements ont eu lieu. Dès la première semaine, il était numéro 1 des applications gratuites de l’App Store sur iPhone et iPad aux États-Unis, numéro 1 des applications gratuites dans 12 pays et numéro 1 des jeux gratuits dans 13 pays.
        </td>
        <td>
            <ul>
                <li>Système multiplateforme <li>Mode multijoueur <li>Performances élastiques à grande échelle </ul>
        </td>
        <td>
            <ul>
                <li>[Entretien avec Kalle Hiitola, directeur de Next Games (vidéo)](https://channel9.msdn.com/Blogs/AzureDocumentDB/azure-documentdb-walking-dead)
                <li>[The Walking Dead utilise DocumentDB pour un cycle de développement plus rapide et une expérience de jeu plus conviviale](https://azure.microsoft.com/blog/the-walking-dead-no-mans-land-game-soars-to-1-with-azure-documentdb/)
            </ul>
    </tr>
    </td>
        <td>[Pixels Squad](http://www.crimecoast.com/)</td>
        <td>Pixels Squad a développé _Crime Coast_ à l’aide du moteur de jeu Unity et d’Azure. _Crime Coast_ est un jeu social de stratégie disponible sur les plateformes Android, iOS et Windows. Ont été utilisés dans ce jeu le stockage d’objets Blob Azure, un cache Azure Redis géré, un ensemble d’ordinateurs virtuels IIS à charge équilibrée et un hub de notification Microsoft. Découvrez comment cette société a géré la mise à l’échelle et l’augmentation du nombre de joueurs avec 5 000 joueurs simultanés.
        </td>
        <td>
            <ul>
                <li>Système multiplateforme <li>Jeu en ligne en mode multijoueur <li>Système évolutif en fonction du nombre de joueurs </ul>
        </td>
        <td>
            <ul>
                <li>[Comment le jeu MMO Crime Coast a utilisé Azure Cloud Services](https://channel9.msdn.com/Blogs/The-Game-Blog/BizSpark-Interview-with-Pixel-Squad-How-the-used-Azure-Cloud-Services-to-make-an-MMO-with-a-3-man-te)
            </ul>
        </td>
    </tr> 
</table>

    
### <a name="other-links"></a>Autres liens

* [Azure, l’arme secrète de Hitcents, Game Troopers et InnoSpark](http://news.microsoft.com/features/game-developers-use-microsoft-azure-as-secret-sauce-for-scale-and-growth-2/)
* [Les start-up du jeu du programme Bizspark utilisent Azure](https://blogs.technet.microsoft.com/bizspark_featured_startups/2015/09/25/azure-open-for-gaming-startups/)


## <a name="how-to-design-your-cloud-backend"></a>Comment concevoir votre serveur principal dans le cloud

Alors que les fabricants et les concepteurs de jeu discutent des fonctions et fonctionnalités nécessaires, nous vous recommandons de vous pencher sur la façon dont vous voulez concevoir votre infrastructure de jeu. À partir du moment où vous voulez développer des jeux pour différents appareils et sur différentes plateformes parmi les principales, pensez à Azure comme serveur principal pour votre jeu.

### <a name="step-by-step-learning-guides"></a>Guides de formation étape par étape

* [Build 2016 Codelabs : utilisation de Microsoft Azure App Service et du serveur principal Microsoft SQL Azure pour enregistrer le score du jeu](https://github.com/Microsoft-Build-2016/CodeLabs-GameDev-6-Azure)
* [Conception de la stratégie de mobile engagement de votre jeu](https://azure.microsoft.com/documentation/articles/mobile-engagement-gaming-scenario/)
* [Prise en main d’Azure Mobile Engagement pour le déploiement de l’application Unity pour iOS](https://azure.microsoft.com/documentation/articles/mobile-engagement-unity-ios-get-started/)

### <a name="understanding-iaas-paas-or-saas"></a>Comprendre IaaS, PaaS ou SaaS

Tout d’abord, vous devez prendre en considération le niveau de service le mieux adapté à votre jeu. Une bonne connaissance des trois services suivants et de leurs différences peut vous aider à déterminer l’approche à adopter pour créer votre serveur principal.

* [Infrastructure en tant que service (IaaS)](https://azure.microsoft.com/overview/what-is-iaas/)

    L’infrastructure en tant que service (IaaS) est une infrastructure informatique instantanée, mise en service et gérée via Internet. Imaginez pouvoir utiliser de nombreuses machines immédiatement disponibles pour rapidement augmenter ou réduire votre offre, en fonction de la demande. L’infrastructure en tant que service vous permet d’éviter d’acheter vos propres serveurs physiques et toute autre infrastructure de centre de données, ainsi qu’elle vous libère de la complexité de tels systèmes.

* [Plateforme en tant que service (PaaS)](https://azure.microsoft.com/overview/what-is-paas/)

    La plateforme en tant que service (PaaS) est comme l’infrastructure en tant que service, à ceci près qu’elle intègre également une gestion de l’infrastructure comme les serveurs, le stockage et la mise en réseau. Aussi, non seulement vous n’êtes pas obligé d’acheter des serveurs physiques et une infrastructure de base de données, mais vous n’êtes pas obligé non plus d’acheter ni de gérer des licences logicielles, une infrastructure applicative sous-jacente, un middleware, des outils de développement ou autres ressources.

* Logiciel en tant que service (SaaS)

    Les logiciels en tant que service sont normalement des applications déjà développées automatiquement qui sont hébergées sur une plateforme cloud existante. Ces logiciels sont conçus pour faciliter l’exécution de votre jeu sur le service proposé.


### <a name="design-your-game-infrastructure-using-azure"></a>Conception de votre infrastructure de jeu avec Microsoft Azure

Voici quelques-unes des façons dont vous pouvez utiliser les offres de services de cloud computing Azure pour un jeu. Azure fonctionne avec Windows, Linux et les technologies Open Source que vous connaissez comme Ruby, Python, Java et PHP. Pour plus d’informations, voir [Azure pour les jeux](https://azure.microsoft.com/solutions/gaming/).

| Configuration requise                 | Scénarios d’activité                            | Offre de produit                      | Fonctionnalités de produit                               |
|-----------------------------------|-----------------------------------------------|---------------------------------------|----------------------------------------------------|
| Hébergement de votre domaine dans le cloud     | Répondre efficacement aux requêtes DNS            | [Azure DNS](https://azure.microsoft.com/services/dns/) | Hébergement de votre domaine avec des performances et une disponibilité élevées  |
| Connexion et vérification d’identité      | Le joueur se connecte et son identité est authentifiée  | [Azure Active Directory](https://azure.microsoft.com/services/active-directory/) | Authentification unique à toute application web sur site et dans le cloud avec une authentification multifacteur            |
| Jeu utilisant le modèle Infrastructure en tant que service (IaaS)      | Le jeu est hébergé sur des ordinateurs virtuels dans le cloud       | [Ordinateurs virtuels Azure](https://azure.microsoft.com/services/virtual-machines/) | Mise à l’échelle de 1 à plusieurs milliers d’instances d’ordinateur virtuel en tant que serveurs de jeu avec mise en réseau virtuelle intégrée et équilibrage de la charge ; cohérence hybride avec les systèmes sur site           |
| Jeux web ou sur appareil mobile utilisant le modèle Plateforme en tant que service (PaaS)            | Le jeu est hébergé sur une plateforme gérée                | [Azure App Service](https://azure.microsoft.com/services/app-service/) | Plateforme en tant que service pour les jeux sur site web ou sur appareil mobile (soit des ordinateurs virtuels Azure avec middleware/outils de développement/aide à la décision/gestion de base de données)   |
| Stockage dans le cloud des données de jeu       | Les dernières données de jeu sont stockées dans le cloud et envoyées aux appareils des clients | [Stockage d’objets Blob Azure](https://azure.microsoft.com/services/storage/blobs/)| Aucune restriction sur les types de fichiers qui peuvent être stockés ; stockage d’objets pour de grandes quantités de données non structurées comme les images, les fichiers audio, vidéo et plus encore  |
| Tables de stockage de données temporaires| Les transactions de jeu (changements dans les états de jeu) sont stockées temporairement dans les tables | [Stockage de tables Azure](https://azure.microsoft.com/services/storage/tables/)| Les données de jeu peuvent être stockées dans un schéma flexible en fonction des besoins du jeu |
| Mise en file d’attente des transactions/requêtes de jeu| Les transactions de jeu sont traitées sous la forme d’une file d’attente | [Azure Queue Storage](https://azure.microsoft.com/services/storage/queues/)| Les files d’attente absorbent les pics de trafic inattendus et peuvent empêcher les serveurs d’être submergés par un flux soudain de demandes en cours de jeu   |
| Base de données de jeu relationnelle évolutive| Stockage structuré de données relationnelles de type transactions dans le jeu vers base de données | [Base de données SQL Azure](https://azure.microsoft.com/services/sql-database/)| Base de données SQL en tant que service ([comparaison avec SQL sur un ordinateur virtuel](https://azure.microsoft.com/documentation/articles/data-management-azure-sql-database-and-sql-server-iaas/))  |
| Base de données de jeu évolutive et distribuée à faible latence| Rapidité des lectures, écritures et requêtes de jeu et des données du joueur avec flexibilité du schéma | [Azure DocumentDB](https://azure.microsoft.com/services/documentdb/)| Base de données de documents NoSQL à faible latence, gérée en tant que service   |
| Utilisation de votre propre centre de données avec les services Azure | Le jeu est récupéré depuis votre propre centre de données et envoyé sur les appareils du client | [Azure Stack](https://azure.microsoft.com/overview/azure-stack/) | Permet à votre organisation de transmettre des services Windows Azure depuis votre propre centre de données pour plus d’efficacité  |
| Transfert de blocs de données volumineux| Les fichiers volumineux comme les images, les fichiers audio et les vidéos du jeu peuvent être envoyés aux utilisateurs à partir de l’emplacement POP CDN (réseau de diffusion de contenu) le plus proche avec Azure CDN    | [Azure Content Delivery Network](https://azure.microsoft.com/services/cdn/) | Basé sur une topologie réseau moderne avec nœuds centralisés de grande taille, Azure CDN gère les pics soudains de trafic et les charges élevées pour augmenter considérablement la vitesse et la disponibilité, ce qui conduit à une amélioration significative de l’expérience utilisateur  |
| Faible latence               | Effectuer une mise en cache pour créer des jeux rapides et évolutifs, avec plus de contrôle et de garantie d’isolation des données ; peut également être utilisé pour améliorer la fonctionnalité de matchmaking | [Azure Redis Cache](https://azure.microsoft.com/services/cache/) | Débit élevé et accès aux données cohérent et à faible latence pour alimenter rapidement les applications Azure évolutives  |
| Évolutivité élevée, faible latence | Gère les fluctuations du nombre d’utilisateurs du jeu avec des lectures et des écritures à faible latence | [Azure Service Fabric](https://azure.microsoft.com/services/service-fabric/) | Possibilité d’alimenter les scénarios les plus complexes à faible latence avec volumes importants de données et d’assurer une mise à l’échelle fiable pour gérer un plus grand nombre d’utilisateurs à la fois Service Fabric vous permet de développer des jeux sans avoir à créer un magasin ou un cache séparé, tel que cela est demandé pour les applications sans état |
| Capacité à collecter des millions d’événements par seconde depuis les appareils                         | Consigner des millions d’événements par seconde depuis les appareils | [Concentrateurs d’événement Azure](https://azure.microsoft.com/services/event-hubs/) | Ingestion de télémétrie à l’échelle du cloud depuis les jeux, les sites web, les applications et les appareils  |
| Traitement des données de jeu en temps réel  | Effectuer une analyse en temps réel des données des joueurs pour améliorer l’expérience de jeu| [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) | Traitement du flux en temps réel dans le cloud  |
| Développement d’une expérience de jeu prédictive         | Créer une expérience de jeu dynamique et personnalisée en fonction des données du joueur  | [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) | Service cloud entièrement géré qui vous permet de facilement développer, déployer et partager des solutions d’analyse prédictive  |
| Collecte et analyse des données de jeu| Traitement parallèle massif des données à partir de bases de données relationnelles et non relationnelles | [Azure Data Warehouse](https://azure.microsoft.com/services/sql-data-warehouse/)| Entrepôt de données élastique en tant que service doté de fonctionnalités adaptées aux besoins des entreprises   |
| Création de campagnes marketing pour accroître l’utilisation et la fidélisation  | Envoyer des notifications push à certains joueurs pour susciter l’intérêt et encourager des actions de jeu spécifiques en fonction de l’analyse des données | [Mobile engagement](https://azure.microsoft.com/services/mobile-engagement/) |  Augmenter le temps de jeu et la fidélisation des utilisateurs sur toutes les principales plateformes (iOS, Android, Windows, Windows Phone) |


##  <a name="startup-and-developer-resources"></a>Ressources pour start-up et développeurs

* [Microsoft BizSpark](https://www.microsoft.com/bizspark/)

    Microsoft BizSpark est un programme international qui favorise la réussite des start-up en offrant un accès gratuit aux services de cloud computing Azure, ainsi qu’aux logiciels et au support technique. Les membres BizSpark reçoivent cinq abonnements Visual Studio Enterprise avec MSDN, chacun avec un crédit Azure mensuel de 150 dollars. Cela correspond à un total de 750 dollars/mois que les développeurs peuvent dépenser en services Azure. BizSpark est une offre proposée aux start-up privées en activité depuis moins de 5 ans et qui génèrent un chiffre d’affaires annuel inférieur à 1 million de dollars. Chez Microsoft, nous pensons qu’en aidant les start-up, nous aidons à mettre en place un partenariat productif à long terme.
    
* [ID@Xbox](http://www.xbox.com/Developers/id)

    Vous voulez ajouter à votre jeu Windows 10 des fonctionnalités Xbox Live comme une expérience de jeu multijoueur, un matchmaking multiplateforme, les scores des joueurs, les progrès et les classements ? Inscrivez-vous sur ID@Xbox pour obtenir les outils et le support technique dont vous avez besoin. Vous pourrez ainsi libérer votre créativité et renforcer vos chances de réussir. Avant de faire une demande sur ID@Xbox, inscrivez un compte de développeur dans le [Centre de développement Windows](https://developer.microsoft.com/windows/programs/join).

## <a name="software-as-a-service-for-game-backend"></a>Serveur principal de jeu et logiciels en tant que service

Voici quelques sociétés qui proposent un serveur principal dans le cloud pour les jeux basés sur les principaux fournisseurs de services de cloud computing. L’intérêt est de vous permettre de vous concentrer sur le développement de votre jeu.

* [GameSparks](http://www.gamesparks.com/)

    GameSparks est une plateforme de développement basé sur le cloud pour les développeurs de jeu, qui leur permet de développer leur jeu côté serveur.

* [Photon Engine](https://www.photonengine.com/en/Photon)

    Photon est une plateforme multijoueur pour les jeux et un moteur de mise en réseau indépendant. Photon propose Photon Cloud, un service entièrement géré de logiciels en tant que service (SaaS). Vous pouvez totalement vous concentrer sur votre client d’application pendant l’hébergement ; les opérations serveur et la mise à l’échelle sont toutes prises en charge par Exit Games.

* [Playfab](https://playfab.com/)

    Playfab apporte rapidement et simplement une excellence gestion des jeux en direct et une technologie de serveur principal performante sur appareils mobiles, PC ou consoles de jeux.

## <a name="related-links"></a>Liens connexes

* [Guide de développement de jeux Windows 10](https://msdn.microsoft.com/windows/uwp/gaming/e2e)
* [Azure pour les jeux](https://azure.microsoft.com/solutions/gaming/)
* [Microsoft BizSpark](https://www.microsoft.com/bizspark/)
* [ID@Xbox](http://www.xbox.com/Developers/id)


 

 

