---
title: Développement d’applications pour Windows as a service
description: Dissociez la publication et le support des applications de versions Windows spécifiques.
ms.date: 02/08/2017
ms.topic: article
keywords: windows10, uwp
ms.assetid: f384ca56-f2b2-4793-b251-f7f5735376bb
ms.localizationpriority: medium
ms.openlocfilehash: e0a0bef4496e9b6aa327a8da404e88bcbd791e70
ms.sourcegitcommit: b5c9c18e70625ab770946b8243f3465ee1013184
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2018
ms.locfileid: "7969697"
---
# <a name="application-development-for-windows-as-a-service"></a>Développement d’applications pour Windows en tant que service

**S’applique à**
-   Windows10
-   Windows 10 Mobile
-   Windows 10 IoT standard 

Dans l’environnement d’aujourd’hui, où les attentes des utilisateurs sont souvent déterminées par des expériences liées à des appareils spécifiques, les cycles de produit complets se mesurent en mois, plutôt qu’en années. En outre, de nouvelles versions doivent être rendues disponibles en permanence, dont le déploiement ne doit avoir qu’un impact minimal sur les utilisateurs. Microsoft a conçu Windows 10 pour répondre à ces exigences en implémentant une nouvelle approche d’innovation, de développement et de remise appelée [Windows en tant que service (WaaS)](https://docs.microsoft.com/windows/deployment/update/waas-overview). La clé pour les cycles de produit sensiblement plus courts tout en conservant des niveaux de haute qualité est une approche innovante de la Communauté, centrée sur à aucun test que Microsoft a implémentée pour Windows 10. Cette communauté d’initiés, appelés « Windows Insiders », compte des millions d’utilisateurs dans le monde entier. Les Windows Insiders qui décident de rejoindre la communauté testent de nombreuses builds tout au long du cycle d’un produit, et leurs commentaires apportent à Microsoft un retour d’expérience via une méthodologie itérative nommée distribution de version d’évaluation.

Les builds distribuées en tant que versions d’évaluation permettent à l’équipe d’ingénieurs de Windows de recueillir des données importantes relatives aux performances des builds dans des conditions d’utilisation réelle. La distribution de version d’évaluation avec la collaboration des Windows Insiders permet également à Microsoft de tester les builds dans des environnements matériels, d’applications et réseau sensiblement plus variés que par le passé, et d’identifier les problèmes beaucoup plus rapidement. Par conséquent, Microsoft estime que la phase de distribution de version d’évaluation axée sur la communauté permettra, plus que jamais, d’accélérer le rythme de la fourniture d’innovations et d’améliorer la qualité des versions publiques.

## <a name="windows-10-release-types-and-cadences"></a>Types et cadences de versions de Windows10

Parallèlement au lancement de builds Insider Preview destinées aux Windows Insiders, Microsoft publie en permanence deux types de versions de Windows10 destinées au grand public :

**Mises à jour** installer les nouvelles fonctions, expériences et fonctionnalités sur les appareils exécutant déjà Windows 10. Dans la mesure où les mises à jour contenant une copie complète de Windows, elles permettent également aux clients d’installer Windows 10 sur des appareils existants exécutant Windows7 ou Windows8.1, ainsi que sur les nouveaux appareils où aucun système d’exploitation n’est installé. Microsoft prévoit de publier des mises à jour par an. 

Les **mises à jour qualité** contiennent les résolutions des problèmes de sécurité et d’autres résolutions de bogues importants. Elles sont fournies pour améliorer chaque fonctionnalité actuellement prise en charge, à une cadence d’une à plusieurs par mois. Microsoft continue à publier des mises à jour qualité le mardi («Update Tuesday» ou «Patch Tuesday»). En outre, Microsoft peut publier des mises à jour qualité supplémentaires pour Windows 10 en dehors des mardis si nécessaire pour répondre aux clients besoins.

Au cours du développement de Windows 10, Microsoft a simplifié le cycle d’ingénierie et de lancement de produits Windows afin de pouvoir fournir les fonctions, expériences et fonctionnalités que les clients souhaitent plus rapidement que jamais. Nous avons également créé de nouvelles façons de fournir et d’installer les mises à jour des fonctionnalités et les mises à jour qualité, qui simplifient les déploiements et la gestion en cours, élargissent la base des employés pouvant être tenus informés des dernières fonctionnalités et expériences de Windows, et réduisent le coût total de possession. Nous avons donc implémenté de nouvelles options de maintenance – appelées en tant que le canal semi-annuel et à long terme canal de maintenance (LTSC) – qui offrent des solutions pragmatiques permettant de maintenir davantage d’appareils plus à jour dans les environnements d’entreprise que ce qui était possible auparavant.

Le tableau suivant décrit les différents canaux de maintenance et leurs principaux attributs.

| Option de maintenance | Disponibilité de nouvelles mises à niveau de fonctionnalités pour installation | Durée de vie de maintenance | Principaux avantages | Éditions prises en charge |
| --- | --- | --- | --- | --- |
| Canal semi-annuel (ciblé) | Immédiatement après la première publication par Microsoft | 18mois | Met les nouvelles fonctionnalités à la disposition des utilisateurs dès que possible. | Famille, Professionnel, éducation, entreprise, Mobile, IoT standard, Windows 10 IoT standard professionnel (IoT standard professionnel) |
| Canal semi-annuel | Environ 4 mois après la première publication par Microsoft | 18 mois à partir de laquelle la première publication | Offre plus de temps pour tester les nouvelles mises à niveau de fonctionnalités avant le déploiement | Professionnel, Éducation, Entreprise, Mobile Entreprise, IoT Standard Professionnel |
| Canal de maintenance à long terme (LTSC) | Immédiatement après la publication par Microsoft | 10 ans | Permet le déploiement à long terme de versions sélectionnées de Windows 10 dans les configurations de changement faible | Entreprise LTSB |

Pour plus d’informations, voir [Options de maintenance de Windows10 pour les mises à jour et les mises à niveau](https://docs.microsoft.com/windows/deployment/update/waas-overview#servicing-channels).

## <a name="supporting-apps-in-windows-as-a-service"></a>Prise en charge des applications dans Windows en tant que service

L’approche traditionnelle de prise en charge des applications consiste à publier une nouvelle version de l’application en réponse à une publication de Windows. Elle part du principe qu’il existe des modifications fondamentales dans le système d’exploitation sous-jacent, qui peuvent potentiellement provoquer une régression de l’application. Ce modèle implique un cycle de développement et de validation dédié qui nécessite que nos partenaires éditeurs de logiciels indépendants s’alignent sur la fréquence de publication de Windows.

Dans le modèle Windows en tant que service, Microsoft s’engage à maintenir la compatibilité du système d’exploitation sous-jacent. En d’autres termes, Microsoft s’efforce, de manière concertée, qu’aucune modification fondamentale ne nuise à l’écosystème d’applications. Dans ce scénario, lorsqu’une nouvelle build Windows est publiée, la plupart des applications (celles qui sont dépourvues de dépendances vis-à-vis du noyau) continuent de fonctionner.

Dans la perspective de cette modification, nous recommandons à nos partenaires éditeurs de logiciels indépendants de dissocier la publication et le support de leur application des builds Windows spécifiques. Une approche de type cycle de vie des applications est plus utile à nos clients mutuels. Cela signifie que lorsqu’une version d’une application est publiée, elle reste prise en charge pendant une période donnée, indépendamment du nombre de builds Windows publiées pendant ce temps. L’éditeur de logiciels indépendant s’engage à assurer la prise en charge de cette version spécifique de l’application tant qu’elle est prise en charge dans le cycle de vie. Pour Windows, Microsoft suit une approche de type cycle de vie similaire, qui peut être référencée [ici](http://go.microsoft.com/fwlink/?LinkID=780549).

Cette approche permet de réduire la charge de la maintenance d’une planification d’application qui s’aligne sur les publications de Windows. Les partenaires éditeurs de logiciels indépendants doivent pouvoir publier librement des fonctionnalités ou des mises à jour à leur propre rythme. Nous pensons que nos partenaires peuvent maintenir leur clientèle à jour avec les dernières mises à jour d’application indépendantes d’une publication de Windows. Par ailleurs, nos clients n’ont pas à rechercher une déclaration de prise en charge explicite à chaque publication d’une build Windows. Voici un exemple de déclaration de prise en charge qui indique dans quelle mesure une application peut être prise en charge dans différentes versions du système d’exploitation:

| Exemple de déclaration de prise en charge du cycle de vie des applications | |
| --- | --- |
| La société de développement logiciel Contoso est propriétaire de l’application populaire Mojave, qui occupe une place importante dans l’entreprise. Contoso publie sa prochaine version majeure Mojave14.0 et déclare une prise en charge standard de troisans à compter de la date de publication. Pendant la prise en charge standard, toutes les mises à jour ainsi que le support sont gratuits pour le produit sous licence. Contoso déclare également deuxannées supplémentaires de support étendu pendant lesquelles les clients peuvent acheter des mises à jour et bénéficier du support pendant une période de grâce. Au-delà de la date de fin du support étendu, le support de cette version du produit n’est plus assuré. Pendant la période de prise en charge standard, Contoso prend en charge Mojave14.0 sur l’ensemble des builds publiées de Windows. Contoso publie également les mises à jour pour Mojave au besoin, et indépendamment des versions de produit Windows. | |

Les sections suivantes contiennent des informations supplémentaires sur les mesures prises par Microsoft pour maintenir la compatibilité du système d’exploitation sous-jacent. Vous y trouverez également des conseils sur les mesures que vous pouvez prendre pour maintenir la compatibilité du système d’exploitation et de l’écosystème des applications. Une section indique également comment tirer parti des builds de distribution de version d’évaluation Windows afin de détecter les régressions d’application avant la publication d’une build Windows. Pour finir, nous expliquons comment utiliser une approche pilotée par l’instrumentation et la télémétrie afin d’augmenter la qualité des builds Windows. Nous recommandons aux éditeurs de logiciels indépendants d’adopter une approche similaire avec leur portefeuille d’applications.

## <a name="key-changes-since-windows7-to-ensure-app-compatibility"></a>Principales modifications depuis Windows7 pour assurer la compatibilité de l’application

Nous comprenons que la compatibilité soit importante pour les développeurs. Les éditeurs de logiciels indépendants et les développeurs veulent s’assurer que leurs applications s’exécuteront comme prévu sur toutes les versions prises en charge du système d’exploitation Windows. L’investissement des consommateurs et des entreprises est essentiel ici: ils souhaitent s’assurer que les applications qu’ils ont payées continueront de fonctionner. Nous savons que la compatibilité est le principal critère motivant les décisions d’achat. Les applications qui sont écrites correctement en respectant les meilleures pratiques entraînent beaucoup moins d’évolution du code lorsqu’une nouvelle version de Windows est publiée et réduisent la fragmentation. Pour ces applications, l’investissement en termes d’ingénierie est réduit pour la maintenance, et le délai de commercialisation est plus court.

À l’époque Windows7, la compatibilité était une approche extrêmement réactive. Dans le package Windows8, nous avons commencé à différemment, en travaillant dans Windows pour vous assurer que la compatibilité conception plutôt que d’un ajout ultérieur. Windows 10 est la version la plus compatible relevant de la conception du système d’exploitation à ce jour. Voici quelques moyens clés qui nous ont permis d’atteindre cet objectif:
-   **Télémétrie applicative**: elle nous permet de comprendre la popularité des applications dans l’écosystème Windows afin de fournir des informations sur les tests de compatibilité.
-   **Partenariats avec des éditeurs de logiciels indépendants**: nous collaborons directement avec des partenaires externes pour leur fournir des données et les aider à résoudre les problèmes rencontrés par nos utilisateurs.
-   **Vérifications de la conception, détection en amont**: nous travaillons en partenariat avec les équipes chargées des fonctionnalités pour réduire le nombre de modifications fondamentales dans Windows. La vérification de la compatibilité est l’une des étapes que doivent effectuer nos équipes chargées des fonctionnalités.
-   **Communication**: contrôle plus étroit des modifications desAPI et communication améliorée.
-   **Distribution de version d’évaluation et système de remontée de l’information**: les WindowsInsiders reçoivent des builds avec version d’évaluation, qui nous aident à améliorer notre capacité à trouver les problèmes de compatibilité avant la publication d’une version finale. Ce processus de commentaires ne révèle pas seulement les bogues, il nous permet également de nous assurer que nous fournissons à nos clients les fonctionnalités qu’ils souhaitent.

## <a name="best-practices-for-app-compatibility"></a>Bonnes pratiques en matière de compatibilité des applications

Nous utilisons des données de diagnostic et d’utilisation pour identifier et résoudre les problèmes, améliorer nos produits et services et proposer à nos utilisateurs des expériences personnalisées. Les données d’utilisation que nous collectons concernent également les applications exécutées sur lesPC dans l’écosystème Windows. En fonction de l’utilisation de nos clients, nous créons une liste pour tester ces applications, appareils et pilotes dans les nouvelles versions du système d’exploitation Windows. Windows 10 a été la version la plus compatible de Windows à jour, avec plus de 90 % compatibilité avec des milliers d’applications populaires. L’équipe de compatibilité de Windows est à l’écoute de nos partenaires éditeurs de logiciels indépendants pour leur fournir des commentaires si des problèmes sont détectés, afin que nous puissions travailler en partenariat sur des solutions. Dans l’idéal, nous voulons que nos clients communs soient en mesure de mettre à jour Windows de façon transparente et sans perte de fonctionnalités dans leur système d’exploitation ou dans les applications dont ils dépendent pour leur productivité ou leurs loisirs.

Les sections suivantes contiennent certaines recommandations de Microsoft pour vous assurer de que vos applications sont compatibles avec Windows 10.

### <a name="windows-version-check"></a>Vérification de la version de Windows

La version du système d’exploitation a été incrémentée avec Windows 10. Cela signifie que le numéro de version interne a été remplacé par10.0. Comme précédemment, nous faisons le maximum pour maintenir la compatibilité entre les applications et l’appareil après modification d’une version du système d’exploitation. Pour la plupart des catégories d’applications (sans dépendances vis-à-vis du noyau), la modification ne sera pas un impact négatif sur fonctionnalités de l’application, et les applications existantes continuent de fonctionner parfaitement sur Windows 10.

La manifestation de cette modification est propre à l’application. En d’autres termes, une application qui vérifie spécifiquement la version du système d’exploitation obtient un numéro de version supérieur, ce qui peut conduire aux situations suivantes:
-   Les programmes d’installation des applications peuvent ne pas être en mesure d’installer les applications, ou les applications peuvent être dans l’impossibilité de démarrer.
-   Les applications peuvent devenir instables ou se bloquer.
-   Les applications peuvent générer des messages d’erreur, tout en continuant de fonctionner correctement.

Certaines applications effectuent une vérification de version et transmettent simplement un avertissement aux utilisateurs. Toutefois, certaines d’entre elles sont liées très étroitement à une vérification de version (dans les pilotes, ou en mode noyau pour éviter la détection). Le cas échéant, les applications échouent si une version incorrecte est détectée. Plutôt qu’une vérification de version, nous vous recommandons l’une des approches suivantes:
-   Si l’application dépend de fonctionnalités d’API spécifiques, assurez-vous de cibler la version d’API correcte.
-   Assurez-vous de détecter la modification via APISet ou une autreAPI publique, et n’utilisez pas la version comme proxy pour certaines fonctionnalités ou correctifs. En cas de modifications fondamentales et si une vérification correcte n’est pas exposée, il s’agit d’un bogue.
-   Veillez à ce que l’application ne vérifie PAS la version de manière curieuse, par exemple via le Registre, les versions de fichiers, les décalages, le mode noyau, les pilotes ou tout autre moyen. Si l’application doit absolument vérifier la version, utilisez lesAPI GetVersion, qui doivent renvoyer le numéro majeur, le numéro mineur et le numéro de build.
-   Si vous utilisez les API [GetVersion](http://go.microsoft.com/fwlink/?LinkID=780555) , n’oubliez pas que le comportement de cette API a changé depuis Windows8.1.

Si vous possédez des applications telles que des applications anti-programme malveillant ou de pare-feu, vous devez utiliser vos canaux de commentaires habituels et le programme WindowsInsider.

### <a name="undocumented-apis"></a>API non documentées

Vos applications ne doivent pas appeler desAPI Windows non documentées ni dépendre d’exportations de fichiers spécifiques ou de clés de Registre. Cela peut aboutir à la rupture de fonctionnalités, la perte de données et à des problèmes de sécurité potentiels. Si des fonctionnalités requises par votre application ne sont pas disponibles, vous pouvez fournir des commentaires via vos canaux de commentaires habituels et via le programme WindowsInsider.

### <a name="develop-universal-windows-platform-uwp-and-centennial-apps"></a>Développer des applications Centennial et de plateforme Windows universelle (UWP)

Nous encourageons tous les éditeurs d’applicationsWin32 indépendants à développer dorénavant des applications de [plateforme Windows universelle (UWP)](http://go.microsoft.com/fwlink/?LinkID=780560) et, plus spécifiquement, des applications [Centennial](http://go.microsoft.com/fwlink/?LinkID=780562). Les avantages sont nombreux à développer ces packages d’applications plutôt qu’à utiliser des programmes d’installationWin32 traditionnels. Les applications UWP sont également prises en charge dans le [Microsoft Store](http://go.microsoft.com/fwlink/?LinkID=780563), il est plus facile de mettre à jour automatiquement, vos utilisateurs vers une version cohérente réduit les coûts de prise en charge.

Si vos types d’applicationWin32 ne fonctionnent pas avec le modèle Centennial, nous vous recommandons vivement d’utiliser le programme d’installation approprié et de vous assurer qu’il a été complètement testé. Un programme d’installation représente la première expérience de votre utilisateur ou de votre client avec votre application; vous devez donc vous assurer de son bon fonctionnement. Il arrive trop souvent qu’il ne fonctionne pas correctement ou qu’il n’ait pas été complètement testé. Le [Kit de certification des applications Windows](http://go.microsoft.com/fwlink/?LinkID=780565) peut vous aider à tester l’installation et la désinstallation de votre applicationWin32. Il peut également vous permettre d’identifier l’utilisation d’API non documentées, ainsi que d’autres problèmes de base liés aux performances, avant vos utilisateurs.

**Bonnes pratiques:**
-   Utilisez des programmes d’installation qui fonctionnent pour les versions 32bits et 64bits de Windows.
-   Concevez vos programmes d’installation pour qu’ils s’exécutent dans plusieurs scénarios (niveau de l’utilisateur ou de l’ordinateur).
-   Conservez tous les composants redistribuables de Windows dans le package d’origine. Si vous réorganisez le package de ces composants, il se peut que le programme d’installation soit rompu.
-   Planifiez le temps de développement de vos programmes d’installation; ils sont souvent négligés en tant que livrables au cours du cycle de développement logiciel.

## <a name="optimized-test-strategies-and-flighting"></a>Stratégies de test optimisées et distribution de version d’évaluation

La distribution de version d’évaluation du système d’exploitation Windows désigne les builds intermédiaires proposées aux WindowsInsiders avant la publication d’une build finale. Plus les Insiders qui évaluent ces builds intermédiaires sont nombreux, plus les commentaires que nous recevons sur la qualité des builds, leur compatibilité, etc. le sont aussi, ce qui contribue à l’amélioration de la qualité des builds finales. Vous pouvez prendre part à ce programme de distribution de version d’évaluation pour vous assurer que vos applications fonctionneront comme prévu dans les builds successives du système d’exploitation. Nous vous invitons également à fournir des commentaires sur le fonctionnement de ces builds avec version d’évaluation, les problèmes que vous avez rencontrés, etc.

Si votre application figure dans le WindowsStore, vous pouvez distribuer une version d’évaluation de votre application par son intermédiaire, ce qui signifie que votre application pourra être installée par nos WindowsInsiders. Les utilisateurs peuvent installer votre application et vous envoyer des commentaires préliminaires sur celle-ci avant que vous ne la publiiez à destination de la population. Les sections suivantes décrivent les étapes permettant de tester vos applications par rapport aux builds avec version d’évaluation de Windows.

### <a name="step-1-become-a-windows-insider-and-participate-in-flighting"></a>Étape1: Devenez un WindowsInsider et prenez part à la distribution de version d’évaluation
Au titre de [WindowsInsider](http://go.microsoft.com/fwlink/p/?LinkId=521639), vous pouvez nous aider à façonner l’avenir de Windows: vos commentaires nous aideront à améliorer les fonctions et fonctionnalités de la plateforme. Il s’agit d’une communauté active dans laquelle vous pouvez communiquer avec d’autres passionnés, rejoindre des forums, échanger des conseils et découvrir les prochains événements destinés uniquement aux Insiders.

Dans la mesure où vous avez accès à afficher un aperçu de versions de Windows 10, Windows 10 Mobile et la dernière version du SDK Windows et l’émulateur, vous avez tous les outils à votre disposition pour développer des applications réussies et Explorer les nouveautés de la plateforme Windows universelle et du Microsoft Store.

C’est là aussi une excellente occasion de créer du matériel remarquable, à l’aide des versions d’évaluation des kits de développement de matériel afin de pouvoir développer des pilotes universels pour Windows. WindowsIoTStandardInsiderPreview est également disponible sur les plateformes de développement IoT prises en charge pour que vous puissiez créer des solutions connectées fantastiques à l’aide de la plateforme Windows universelle.

Avant que vous ne deveniez un WindowsInsider, notez que ce programme s’adresse aux utilisateurs qui:
-   Souhaitent tester des logiciels toujours en développement.
-   Veulent partager des commentaires sur les logiciels et la plateforme.
-   N’ont aucun problème pour installer de nombreuses mises à jour ou une interface utilisateur susceptible de changer considérablement.
-   S’y connaissent vraiment enPC et ne voient aucun inconvénient à résoudre les problèmes, sauvegarder leurs données, formater un disque dur, installer un système d’exploitation depuis le début ou restaurer un ancien système si nécessaire.
-   Savent ce qu’est un fichierISO et comment l’utiliser.
-   N’effectuent pas l’installation de ces logiciels sur leur ordinateur ou leur appareil qu’ils utilisent tous les jours.

### <a name="step-2-test-your-scenarios"></a>Étape2: Testez vos scénarios

Une fois que vous avez procédé à une mise à jour vers une build avec version d’évaluation, vous pouvez commencer à tester l’application et à collecter des commentaires à l’aide des exemples de cas de test suivants. Pour la plupart de ces tests, veillez à utiliser des systèmesx86 etAMD64.
**Test de nouvelle installation:** Sur une nouvelle installation de Windows 10, assurez-vous que votre application est totalement fonctionnelle. Si votre application échoue à ce test et au test de mise à niveau, il est probable que le problème soit imputable à des modifications du système d’exploitation sous-jacent ou à des bogues dans l’application. Si, après un examen approfondi, il en ressort que le problème est dû à des modifications du système d’exploitation sous-jacent, veillez à utiliser le programme WindowsInsider pour fournir des commentaires et collaborer sur des solutions.

**Mise à niveau de Test:** Vérifiez que votre application fonctionne après la mise à niveau à partir d’une version de niveau inférieur de Windows (autrement dit, Windows7 ou Windows8.1) pour Windows 10. Votre application ne doit pas provoquer de restaurations pendant la mise à niveau et doit continuer de fonctionner comme prévu après celle-ci. Ce point est essentiel pour obtenir une mise à niveau parfaite.

**Test de réinstallation:** Assurez-vous que les fonctionnalités de l’application peuvent être restaurées en la réinstallant une fois que vous mettre à niveau le PC vers Windows 10 à partir d’un système d’exploitation de niveau inférieur. Si votre application a échoué au test de mise à niveau, et que vous n’êtes pas en mesure de restreindre la cause de ces problèmes, il est possible qu’une réinstallation puisse restaurer les fonctionnalités perdues. Un passage de test de réinstallation réussi indique que certaines parties de l’application ne peuvent pas avoir été migrées vers Windows 10.

**Test des fonctionnalités du système d’exploitation/de l’appareil**:vérifiez que votre application fonctionne comme prévu si elle s’appuie sur des fonctionnalités spécifiques dans le système d’exploitation. Les zones courantes à tester incluent les zones ci-dessous, souvent par rapport à une sélection de modèles dePC couramment utilisés afin d’assurer une large couverture:
-   Audio
-   Fonctionnalités de périphériqueUSB (clavier, souris, cléUSB, disque dur externe, etc.)
-   Bluetooth
-   Graphismes/Affichage (multimoniteur, projection, rotation d’écran, etc.)
-   Écran tactile (orientation, clavier visuel, stylet, mouvements, etc.)
-   Pavé tactile (boutons droit et gauche, appui, défilement, etc.)
-   Stylet (appui simple/double, appui prolongé, gomme, etc.)
-   Impression/Numérisation
-   Capteurs (accéléromètre, fusion, etc.)
-   Appareil photo

### <a name="step-3-provide-feedback"></a>Étape3: Envoyez vos commentaires

Faites-nous savoir comment votre application se comporte par rapport aux builds avec version d’évaluation. Lorsque vous découvrez des problèmes avec votre application au cours des tests, consignez les bogues via le portail du partenaire si vous y avez accès ou en contactant votre représentant Microsoft. Nous vous encourageons à nous transmettre ces informations afin que nous puissions générer une expérience de qualité pour nos utilisateurs.

### <a name="step-4-register-on-ready-for-windows"></a>Étape 4: Inscrivez-vous sur Ready for Windows
Le site web [Ready for Windows](http://go.microsoft.com/fwlink/?LinkID=780580) est un répertoire de logiciels prenant en charge Windows10. Il est destiné aux administrateurs informatiques d’entreprises et d’organisations du monde entier qui envisagent d’utiliser Windows10 pour leurs déploiements. Les administrateurs informatiques peuvent consulter le site pour voir que si les logiciels déployés dans leur entreprise est pris en charge dans Windows 10.

## <a name="related-topics"></a>Rubriques associées
[Options de maintenance de Windows10 pour les mises à jour et les mises à niveau](https://technet.microsoft.com/itpro/windows/manage/introduction-to-windows-10-servicing)
