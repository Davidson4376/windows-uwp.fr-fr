---
title: Guide de développement de jeux Windows10
description: Guide complet sur les ressources et les informations nécessaires au développement de jeux de plateforme Windows universelle (UWP).
ms.assetid: 6061F498-96A8-44EF-9711-68AE5A1218C9
ms.date: 04/16/2018
ms.topic: article
keywords: windows10, uwp, jeux, développement de jeux
ms.localizationpriority: medium
ms.openlocfilehash: 58044fba24450c397ee58b1034429f2af8d23ed6
ms.sourcegitcommit: b11f305dbf7649c4b68550b666487c77ea30d98f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/27/2018
ms.locfileid: "7829772"
---
# <a name="windows-10-game-development-guide"></a>Guide de développement de jeux Windows10


Bienvenue dans le guide de développement de jeux Windows10!

Ce guide regroupe les ressources et les informations dont vous avez besoin pour développer un jeu UWP. Une version anglaise (États-Unis) de ce guide est disponible au format [PDF](http://download.microsoft.com/download/9/C/9/9C9D344F-611F-412E-BB01-259E5C76B17F/Windev_Game_Dev_Guide_Oct_2017.pdf).

## <a name="introduction-to-game-development-for-the-universal-windows-platform-uwp"></a>Présentation du développement de jeux pour la plateforme Windows universelle (UWP)


Lorsque vous créez un jeu Windows10, vous pouvez toucher des millions de joueurs dans le monde sur leur téléphone, leur PC et leur Xbox One. Avec Xbox sur Windows, Xbox Live, des jeux multijoueurs multiplateforme, une incroyable communauté de joueurs, ainsi que des nouvelles fonctionnalités puissantes comme les jeux de plateforme Windows universelle (UWP) et DirectX 12, les jeux Windows10 enchantent les joueurs de tous les âges et de tous les genres. La nouvelle plateforme Windows universelle (UWP) garantit la compatibilité de votre jeu sur les appareils Windows10 avec une API commune pour le téléphone, le PC et Xbox One, et équipés des outils et des options permettant d’adapter votre jeu à chaque performance de l’appareil.

Ce guide fournit une collection complète des informations et des ressources qui vous aideront lors du développement de votre jeu. Les sections sont organisées en fonction des étapes de développement du jeu. Vous savez donc où rechercher les informations lorsque vous en avez besoin.

Si vous débutez dans le développement de jeux sur Windows ou Xbox, le guide de [Prise en main](getting-started.md) constitue un bon point de départ. La section [Ressources de développement de jeux](#game-development-resources) fournit également une vue d’ensemble de la documentation, des programmes et des autres ressources qui sont utiles lors de la création d’un jeu. Si vous souhaitez plutôt commencer par examiner du code UWP, consultez la rubrique [Exemples de jeu ](#game-samples).

Ce guide sera mis à jour lorsque des ressources et des documents relatifs au développement de jeux Windows10 seront disponibles.

## <a name="game-development-resources"></a>Ressources de développement de jeux

De la documentation aux programmes de développement, en passant par les forums, les blogs et les exemples, de nombreuses ressources sont disponibles pour vous aider à développer des jeux. Voici un résumé des ressources à connaître lorsque vous commencez à développer votre jeu Windows10.

> [!Note]
> Certaines fonctionnalités sont gérées par le biais de différents programmes. Comme ce guide couvre une large gamme de ressources, vous pouvez donc constater que certaines ressources ne sont pas accessibles selon le programme que vous utilisez ou votre rôle de développement. Les exemples sont les liens developer.xboxlive.com, forums.xboxlive.com, xdi.xboxlive.com ou réseau GDN (Game Developer Network). Pour plus d’informations sur le partenariat avec Microsoft, voir [Programmes pour développeurs](#developer-programs).


### <a name="game-development-documentation"></a>Documentation sur le développement de jeux

Tout au long de ce guide, vous trouverez des liens ciblés vers la documentation appropriée, organisés par tâche, technologie et étape du développement du jeu. Pour vous donner une vue d’ensemble de ce qui est disponible, voici les principaux portails de documentation destinés au développement de jeux Windows10.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Portail principal du Centre de développement Windows</td>
        <td><a href="https://dev.windows.com">Centre de développement Windows</a></td>
    </tr>
    <tr>
        <td>Développement des applications Windows</td>
        <td><a href="https://dev.windows.com/develop">Développement d’applications Windows</a></td>
    </tr>
    <tr>
        <td>Développement d’une application de plateforme universelle Windows</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/mt244352">Guides de procédure pour les applications Windows10</a></td>
    </tr>
    <tr>
        <td>Guides de procédure pour les jeux UWP</td>
        <td><a href="index.md">Jeux et DirectX</a> </td>
    </tr>
    <tr>
        <td>Présentations et informations de référence sur DirectX</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/ee663274">Jeux et graphiques DirectX</a></td>
    </tr>
    <tr>
        <td>Azure pour les jeux</td>
        <td><a href="https://azure.microsoft.com/solutions/gaming/">Utiliser Azure pour développer et faire évoluer vos jeux</a></td>
    </tr>
    <tr>
        <td>PlayFab</td>
        <td><a href="https://api.playfab.com/">Solution principale complète pour les jeux en direct</a></td>
    </tr>
    <tr>
        <td>UWP sur XboxOne</td>
        <td><a href="https://msdn.microsoft.com/windows/uwp/xbox-apps/index">Création d’applications UWP sur XboxOne</a></td>
    </tr>
    <tr>
        <td>UWP sur HoloLens</td>
        <td><a href="https://developer.microsoft.com/windows/mixed-reality/development_overview">Création d’applications UWP sur HoloLens</a></td>
    </tr>
    <tr>
        <td>Documentation Xbox Live</td>
        <td><a href="../xbox-live/index.md">Guide du développeur Xbox Live</a></td>
    </tr>
    <tr>
        <td>Documentation de développement Xbox One (XGD)</td>
        <td><a href="https://developer.microsoft.com/games/xbox/partner/development-home">Développement de Xbox One</a></td>
    </tr>
    <tr>
        <td>Livres blancs de développement Xbox One (XGD)</td>
        <td><a href="https://developer.microsoft.com/games/xbox/partner/development-education-whitepapers">Livres blancs</a></td>
    </tr>
    <tr>
        <td>Documentation Mixer Interactive</td>
        <td><a href="https://dev.mixer.com/reference/interactive/index.html">Ajouter de l’interactivité à votre jeu</a></td>
    </tr>        
</table>

### <a name="partner-center"></a>Espace partenaires

[L’inscription d’un compte de développeur dans l’espace partenaires](https://developer.microsoft.com/store/register) est la première étape de la publication du jeu de Windows. Un compte de développeur vous permet de réserver le nom de votre jeu et de soumettre des jeux gratuits ou payants au Microsoft Store pour tous les appareils Windows. Utilisez votre compte de développeur pour gérer votre jeu et les produits intégrés au jeu, obtenir des analyses détaillées et activer des services qui créent des expériences exceptionnelles pour vos joueurs dans le monde entier. 

Microsoft propose également plusieurs programmes pour développeurs pour vous aider à développer et à publier des jeux Windows. Nous vous recommandons de voir si l’un est adaptée à vos besoins avant de vous inscrire pour un compte espace partenaires. Pour plus d’informations, accédez à [Programmes pour développeurs](#developer-programs)


### <a name="developer-programs"></a>Programmes pour développeurs

Microsoft propose plusieurs programmes pour développeurs pour vous aider à développer et à publier des jeux Windows. Vous devez rejoindre un programme de développement pour développer des jeux pour Xbox One et intégrer des fonctionnalités Xbox Live dans votre jeu. Pour publier un jeu dans le Microsoft Store, vous devez également créer un compte de développeur dans [L’espace partenaires](https://partner.microsoft.com/dashboard) .

#### <a name="xbox-live-creators-program"></a>Programme Créateurs Xbox Live

Avec le programme Créateurs Xbox Live, tout le monde peut intégrer Xbox Live dans ses jeux et les publier sur XboxOne et Windows10. Il existe un processus de certification simplifié où aucune approbation du concept n’est requise en dehors des [Politiques du MicrosoftStore](https://msdn.microsoft.com/library/windows/apps/dn764944.aspx) standard.

Vous pouvez déployer, concevoir et publier votre jeu dans le Programme Créateurs sans kit de développement dédié, à l’aide des supports de vente au détail uniquement. Pour commencer, téléchargez l’[application Dev Mode Activation](https://docs.microsoft.com/windows/uwp/xbox-apps/devkit-activation) sur votre console Xbox One.

Si vous souhaitez accéder à d’autres fonctionnalités Xbox Live, bénéficier d’une aide en termes de marketing et de développement et avoir la chance de figurer dans le Windows Store Xbox One principal, soumettez une demande de participation au programme [ID@Xbox](http://www.xbox.com/Developers/id).

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Programme Créateurs XboxLive</td>
        <td><a href="https://developer.microsoft.com/games/xbox/xboxlive/creator">En savoir plus sur le programme Xbox Live Creators</a></td>
    </tr>
</table>

#### <a name="idxbox"></a>ID@Xbox

Le programme ID@Xbox permet aux développeurs de jeux qualifiés de publier eux-mêmes sur Windows et Xbox One. Si vous voulez développer des jeux pour Xbox One ou ajouter des fonctionnalités Xbox Live telles que les scores des joueurs, les succès et les classements à votre jeu Windows10, inscrivez-vous sur ID@Xbox. Devenez développeur ID@Xbox afin d’obtenir les outils et l’aide dont vous avez besoin pour laisser libre cours à votre créativité et optimiser vos chances de succès. Nous vous recommandons d’appliquer à ID@Xbox avant de vous inscrire pour un compte de développeur dans l’espace partenaires.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Programme pour développeurs ID@Xbox</td>
        <td><a href="http://go.microsoft.com/fwlink/p/?LinkID=526271">Programme de développement indépendant pour Xbox One</a></td>
    </tr>
    <tr>
        <td>Site grand public ID@Xbox</td>
        <td><a href="http://www.idatxbox.com/">ID@Xbox</a></td>
    </tr>
</table>

#### <a name="xbox-tools-and-middleware"></a>Outils et intergiciels (middleware) Xbox

Les outils Xbox et le programme intergiciel cèdent sous licence des kits de développement Xbox aux développeurs professionnels d’outils de jeux et d’intergiciels. Les développeurs acceptés dans le programme peuvent partager et distribuer leurs technologies XDK Xbox à d’autres développeurs Xbox sous licence.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Contacter le programme d’outils et d’intergiciels</td>
        <td><xboxtlsm@microsoft.com></td>
    </tr>
</table>


### <a name="game-samples"></a>Exemples de jeux

De nombreux exemples de jeu et d’application Windows10 sont disponibles pour vous aider à comprendre les fonctionnalités de jeux de Windows10 et à démarrer rapidement le développement de jeux. D’autres exemples sont développés et publiés régulièrement. En conséquence, n’oubliez pas de consulter de temps en temps les portails des exemples pour en voir les nouveautés. Vous pouvez également [consulter](https://help.github.com/articles/watching-repositories/) les référentiels GitHub pour être averti des modifications et des ajouts.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Exemples d’applications de plateforme universelle Windows</td>
        <td><a href="https://github.com/Microsoft/Windows-universal-samples">Windows-universal-samples</a></td>
    </tr>
    <tr>
        <td>Exemples de graphiques Direct3D12</td>
        <td><a href="https://github.com/Microsoft/DirectX-Graphics-Samples">DirectX-Graphics-Samples</a></td>
    </tr>
    <tr>
        <td>Exemples de graphiques Direct3D11</td>
        <td><a href="https://github.com/walbourn/directx-sdk-samples">directx-sdk-samples</a></td>
    </tr>
    <tr>
        <td>Exemple de jeu à la première personne Direct3D11</td>
        <td><a href="tutorial--create-your-first-uwp-directx-game.md">Créer un jeu UWP simple avec DirectX</a></td>
    </tr>
    <tr>
        <td>Exemple d’effets d’image personnalisés de Direct2D</td>
        <td><a href="http://go.microsoft.com/fwlink/p/?LinkId=620531">D2DCustomEffects</a></td>
    </tr>
    <tr>
        <td>Exemple de maillage dégradé Direct2D</td>
        <td><a href="http://go.microsoft.com/fwlink/p/?LinkId=620532">D2DGradientMesh</a></td>
    </tr>
    <tr>
        <td>Exemple d’ajustement de photo Direct2D</td>
        <td><a href="http://go.microsoft.com/fwlink/p/?LinkId=620533">D2DPhotoAdjustment</a></td>
    </tr>
    <tr>
        <td>Exemples publics Xbox Advanced Technology Group</td>
        <td><a href="https://github.com/Microsoft/Xbox-ATG-Samples">Xbox-ATG-Samples</a></td>
    </tr>
    <tr>
        <td>Exemples Xbox Live</td>
        <td><a href="https://github.com/Microsoft/xbox-live-samples">xbox-live-samples</a></td>
    </tr>
    <tr>
        <td>Exemples de jeu Xbox One (XGD)</td>
        <td><a href="https://developer.microsoft.com/games/xbox/partner/development-education-samples">Exemples</a></td>
    </tr>
    <tr>
        <td>Exemples de jeu Windows (MSDN Code Gallery)</td>
        <td><a href="https://code.msdn.microsoft.com/windowsapps/site/search?f%5B0%5D.Type=SearchText&f%5B0%5D.Value=game&f%5B1%5D.Type=Contributors&f%5B1%5D.Value=Microsoft&f%5B1%5D.Text=Microsoft">Exemples de jeu du Microsoft Store</a></td>
    </tr>
    <tr>
        <td>Exemple de jeu en JavaScript 2D</td>
        <td><a href="../get-started/get-started-tutorial-game-js2d.md">Créer un jeu UWP en JavaScript</a></td>
    </tr>
    <tr>
        <td>Exemple de jeu en JavaScript 3D</td>
        <td><a href="../get-started/get-started-tutorial-game-js3d.md">Création d’un jeu JavaScript 3D à l’aide de three.js</a></td>
    </tr>
    <tr>
        <td>Exemple de jeu UWP dans MonoGame2D</td>
        <td><a href="../get-started/get-started-tutorial-game-mg2d.md">Créer un jeu UWP dans MonoGame2D</a></td>
    </tr>      
</table>


### <a name="developer-forums"></a>Forums de développement

Les forums de développement sont un lieu d’échange intéressant pour poser des questions sur le développement des jeux, y répondre et pour contacter la communauté des développeurs de jeux. Les forums peuvent également être des ressources fantastiques pour trouver des réponses à des problèmes difficiles que les développeurs ont rencontrés et résolus dans le passé.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Publication forums de développeurs d’applications et jeux</td>
        <td><a href="https://social.msdn.microsoft.com/Forums/en-us/home?category=windowsapps">La publication et des publicités dans l’App</a></td>
    </tr>
    <tr>
        <td>Forum des développeurs d’applicationsUWP</td>
        <td><a href="https://social.msdn.microsoft.com/Forums/en-us/home?forum=wpdevelop">Développement d’applications de la plateforme Windows universelles</a></td>
    </tr>
    <tr>
        <td>Forums de développeurs d’applications de bureau</td>
        <td><a href="https://social.msdn.microsoft.com/Forums/en-us/home?category=windowsdesktopdev">Forum dédié aux applications de bureau Windows</a></td>
    </tr>
    <tr>
        <td>Jeux du Microsoft StoreDirectX (billets archivés)</td>
        <td><a href="https://social.msdn.microsoft.com/Forums/vstudio/home?forum=wingameswithdirectx">Création de jeux du Microsoft Store en DirectX (archivés)</a></td>
    </tr>
    <tr>
        <td>Forums de développeurs partenaires gérés Windows10</td>
        <td><a href="http://aka.ms/win10devforums">Forum des développeurs Xbox : Windows10</a></td>
    </tr>
    <tr>
        <td>Forums DirectX</td>
        <td><a href="http://forums.directxtech.com/index.php">Forum DirectX 12</a></td>
    </tr>
    <tr>
        <td>Forums plateforme Azure</td>
        <td><a href="https://social.msdn.microsoft.com/Forums/en-us/home?category=windowsazureplatform">Forum Azure</a></td>
    </tr>
    <tr>
        <td>Forum Xbox Live</td>
        <td><a href="https://social.msdn.microsoft.com/Forums/en-US/home?forum=xboxlivedev">Forum de développement Xbox Live</a></td>
    </tr>
    <tr>
        <td>Forums PlayFab</td>
        <td><a href="https://community.playfab.com/index.html">Forums PlayFab</a></td>
    </tr>
</table>


### <a name="developer-blogs"></a>Blogs de développement

Les blogs de développement sont également une excellente ressource pour obtenir les dernières informations sur le développement de jeux. Vous trouverez des billets sur les nouvelles fonctionnalités, les détails de l’implémentation, les recommandations, l’arrière-plan de l’architecture, etc.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Blog de création d’applications pour Windows</td>
        <td><a href="http://blogs.windows.com/buildingapps/">Création d’applications pour Windows</a></td>
    </tr>
    <tr>
        <td>Windows10 (billets de blog)</td>
        <td><a href="http://blogs.windows.com/blog/tag/windows-10/">Publications dans Windows10</a></td>
    </tr>
    <tr>
        <td>Blog de l’équipe d’ingénierie de Visual Studio</td>
        <td><a href="http://blogs.msdn.com/b/visualstudio/">Le blog de Visual Studio</a></td>
    </tr>
    <tr>
        <td>Blogs des outils de développement de Visual Studio</td>
        <td><a href="http://blogs.msdn.com/b/developer-tools/">Blogs des outils de développement</a></td>
    </tr>
    <tr>
        <td>Blog des outils de développement de Somasegar</td>
        <td><a href="http://blogs.msdn.com/b/somasegar/">Blog de Somasegar</a></td>
    </tr>
    <tr>
        <td>Blog DirectX pour les développeurs</td>
        <td><a href="http://blogs.msdn.com/b/directx">Blog DirectX pour les développeurs</a></td>
    </tr>
    <tr>
        <td>Présentation de DirectX 12 (billet de blog)</td>
        <td><a href="http://blogs.msdn.com/b/directx/archive/2014/03/20/directx-12.aspx">DirectX 12</a></td>
    </tr>
    <tr>
        <td>Blog de l’équipe Visual C++</td>
        <td><a href="http://blogs.msdn.com/b/vcblog/">Blog de l’équipe Visual C++</a></td>
    </tr>
    <tr>
        <td>Blog de l’équipe PIX</td>
        <td><a href="https://blogs.msdn.microsoft.com/pix/">Réglage des performances et débogage pour les jeux DirectX12 sur Windows et Xbox</a></td>
    </tr>
    <tr>
        <td>Blog de l’équipe de déploiement des applications Windows universelles</td>
        <td><a href="https://blogs.msdn.microsoft.com/appinstaller/">Générer et déployer le blog de l’équipe des applications UWP</a></td>
    </tr>
</table>
 

## <a name="concept-and-planning"></a>Concept et planification


Lors de l’étape de concept et de planification, vous décidez de l’apparence de votre jeu, et vous choisissez les technologies et les outils que vous allez utiliser pour lui donner vie.

### <a name="overview-of-game-development-technologies"></a>Vue d’ensemble des technologies de développement de jeux

Lorsque vous commencez à développer un jeu pour la plateforme UWP, plusieurs options sont à votre disposition pour les graphismes, les entrées, l’audio, le réseau, les utilitaires et les bibliothèques.

Si vous avez déjà choisi toutes les technologies que vous utiliserez dans votre jeu, félicitations ! Si tel n’est pas le cas, le guide [Technologies de jeu des applications pour la plateforme Windows universelle (UWP)](game-development-platform-guide.md) est un excellent aperçu de la plupart des technologies disponibles. Sa lecture est vivement conseillée pour vous aider à comprendre les options et leur articulation.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Recensement des technologies de jeu UWP</td>
        <td><a href="game-development-platform-guide.md">Technologies de jeu des applications UWP</a></td>
    </tr>
</table>
 

Ces trois vidéos du GDC 2015 constituent une bonne vue d’ensemble du développement de jeux Windows10 et de l’expérience de jeu Windows10.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Vue d’ensemble du développement de jeux Windows10 (vidéo)</td>
        <td><a href="http://channel9.msdn.com/Events/GDC/GDC-2015/Developing-Games-for-Windows-10">Développement de jeux pour Windows10</a></td>
    </tr>
    <tr>
        <td>Expérience de jeu Windows10 (vidéo)</td>
        <td><a href="http://channel9.msdn.com/Events/GDC/GDC-2015/Gaming-Consumer-Experience-on-Windows-10">Expérience de consommateur de jeux sur Windows10</a></td>
    </tr>
    <tr>
        <td>Les jeux à travers l’écosystème Microsoft (vidéo)</td>
        <td><a href="http://channel9.msdn.com/Events/GDC/GDC-2015/The-Future-of-Gaming-Across-the-Microsoft-Ecosystem">L’avenir des jeux à travers l’écosystème Microsoft</a></td>
    </tr>
</table>

### <a name="game-planning"></a>Planification de jeux

Voici quelques concepts et questions d’ordre général à prendre en compte lors de la planification de votre jeu.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Rendre votre jeu accessible</td>
        <td><a href="https://msdn.microsoft.com/windows/uwp/gaming/accessibility-for-games">Accessibilité des jeux</a></td>
    </tr>
    <tr>
        <td>Générer des jeux à l’aide du cloud</td>
        <td><a href="https://msdn.microsoft.com/windows/uwp/gaming/cloud-for-games">Cloud pour les jeux</a></td>
    </tr>
    <tr>
        <td>Monétiser votre jeu</td>
        <td><a href="https://msdn.microsoft.com/windows/uwp/gaming/monetization-for-games">Monétisation pour les jeux</a></td>
    </tr>
</table>



### <a name="choosing-your-graphics-technology-and-programming-language"></a>Choix de la technologie graphique et du langage de programmation

Plusieurs langages de programmation et technologies graphiques peuvent être utilisés dans les jeux Windows10. Votre choix dépend du type de jeu développé, de l’expérience et des préférences de votre studio de développement, et des conditions requises spécifiques pour les fonctionnalités de votre jeu. Allez-vous utiliserC#, C++ ou JavaScript? DirectX, XAML ou HTML5 ?

#### <a name="directx"></a>DirectX

Microsoft DirectX représente le choix à faire pour obtenir des graphismes et des éléments multimédias2D et 3D haute performances.

DirectX 12 est plus rapide et plus efficace que n’importe quelle version précédente. Direct3D 12 permet des scènes plus riches, un plus grand nombre d’objets et des effets plus complexes. Il tire également mieux parti du matériel graphique moderne sur PC Windows10 et Xbox One.

Si vous souhaitez utiliser le pipeline graphique familier de Direct3D 11, vous bénéficierez cependant des nouvelles fonctionnalités de rendu et d’optimisation ajoutées à Direct3D 11.3. Et si vous êtes un développeur éprouvé d’API de bureau pour Windows utilisant Win32, vous avez toujours cette option dans Windows10.

Les fonctionnalités complètes et la solide intégration à la plateforme de DirectX fournissent la puissance et les performances nécessaires aux jeux les plus exigeants.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Développement DirectX pour UWP</td>
        <td><a href="directx-programming.md">Programmation DirectX</a></td>
    </tr>
    <tr>
        <td>Didacticiel: comment créer un jeu UWP DirectX</td>
        <td><a href="tutorial--create-your-first-uwp-directx-game.md">Créer un jeu UWP simple avec DirectX</a></td>
    </tr>
    <tr>
        <td>Présentations et informations de référence sur DirectX</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/ee663274">Jeux et graphismes DirectX</a></td>
    </tr>
    <tr>
        <td>Direct3D 12 : Guide de programmation et informations de référence</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/dn903821">Graphismes Direct3D12</a></td>
    </tr>
    <tr>
        <td>Vidéos de développement Graphics et DirectX12 (YouTube)</td>
        <td><a href="https://www.youtube.com/channel/UCiaX2B8XiXR70jaN7NK-FpA">MicrosoftDirectX12 et GraphicsEducation</a></td>
    </tr>
</table>
 

#### <a name="xaml"></a>XAML

XAML est un langage d’interface utilisateur déclaratif convivial doté de fonctionnalités pratiques comme les animations, les tables de montage séquentiel, la liaison de données, le formatSVG (Scalable Vector Graphics), le redimensionnement dynamique et les graphes de scène. XAML fonctionne parfaitement pour l’interface utilisateur, les menus, les sprites et les graphiques2D des jeux. Pour simplifier la disposition de l’interface utilisateur, le langageXAML est compatible avec les outils de conception et de développement que sont Expression Blend et Microsoft Visual Studio. XAML est couramment utilisé avec C#, mais C++ convient également si c’est votre langage préféré ou si les exigences d’unité centrale de votre jeu sont élevées.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Vue d’ensemble de la plateforme XAML</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/mt228259">Plateforme XAML</a></td>
    </tr>
    <tr>
        <td>Interface utilisateur et contrôles XAML</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/mt228348">Contrôles, dispositions et texte</a></td>
    </tr>
</table>
 

#### <a name="html-5"></a>HTML 5

Le langageHTML (HyperText Markup Language) est un langage de balisage d’interface utilisateur couramment utilisé pour les pages web, les applications et les clients enrichis. Les jeux Windows peuvent utiliser le langageHTML5 comme couche présentation complète avec les fonctionnalités habituelles du HTML, l’accès à la plateforme Universal Windows Platform (UWP) et la prise en charge de fonctionnalités web modernes comme AppCache, les traitements web, le canevas, le glisser-déplacer, la programmation asynchrone et le formatSVG. En arrière-plan, le renduHTML tire parti de la puissance de l’accélération matérielle de DirectX. Vous bénéficiez donc toujours de l’avantage des performances de DirectX sans écrire de code supplémentaire. HTML5 convient bien si vous maîtrisez le développement web, le portage d’un jeu web ou si vous souhaitez utiliser des couches de langage et de graphiques dont l’approche est plus simple que les autres choix. Le langage HTML5 est utilisé avec JavaScript, mais il peut être également appelé dans les composants créés en C# ou C++/CX.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Informations sur HTML5 et DOM</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/br212882.aspx">Informations de référence HTML et DOM</a></td>
    </tr>
    <tr>
        <td>Recommandation du W3C sur HTML5</td>
        <td><a href="http://go.microsoft.com/fwlink/p/?linkid=221374">HTML5</a></td>
    </tr>
</table>
 

#### <a name="combining-presentation-technologies"></a>Combinaison des technologies de présentation

L’infrastructure DXGI (DirectX Graphics Infrastructure) de Microsoft fournit interopérabilité et compatibilité entre plusieurs technologies graphiques. Pour des graphismes haute performance, vous pouvez allier XAML et DirectX, en utilisant XAML pour les menus et les autres éléments simples de l’interface utilisateur, et DirectX pour le rendu des scènes 2D et 3D complexes DXGI assure également la compatibilité entre Direct2D, Direct3D, DirectWrite, DirectCompute et Microsoft Media Foundation.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Guide de programmation et informations de référence sur DXGI</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/hh404534">DXGI</a></td>
    </tr>
    <tr>
        <td>Combinaison de DirectX et XAML</td>
        <td><a href="directx-and-xaml-interop.md">Technologie interop DirectX et XAML</a></td>
    </tr>
</table>
 

#### <a name="c"></a>C++

C++/CX est un langage haute performance à faible traitement, qui fournit une puissante combinaison de vitesse, compatibilité et accès aux plateformes. C++/CX facilite l’utilisation de l’ensemble des fonctionnalités de jeux remarquables de Windows10, notamment DirectX et Xbox Live. Vous pouvez également réutiliser le code et les bibliothèquesC++ existants. C++/CX crée un code rapide et natif qui n’implique pas la surcharge de nettoyage de la mémoire. Votre jeu peut donc bénéficier de performances exceptionnelles et d’une faible consommation d’énergie, ce qui conduit à une autonomie de la batterie plus longue. Utilisez C++/CX avec DirectX ou XAML, ou bien créez un jeu utilisant une combinaison des deux.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Présentations et informations de référence sur C++/CX</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/hh699871.aspx">Informations de référence en matière de langage Visual C++ (C++/CX)</a></td>
    </tr>
    <tr>
        <td>Visual C++: Guide de programmation et informations de référence</td>
        <td><a href="https://docs.microsoft.com/cpp/visual-cpp-in-visual-studio">Visual C++ dans Visual Studio 2017</a></td>
    </tr>
</table>
 

#### <a name="c"></a>C#

C# (prononcez «C sharp») est un langage moderne et innovant, qui est simple, puissant, de type sécurisé et orienté objet. C# permet un développement rapide tout en conservant la familiarité et l’expressivité des langages du styleC. Même s’il est facile à utiliser, C# possède de nombreuses fonctionnalités de langage avancées comme le polymorphisme, les délégués, les expressions lambda, les fermetures, la méthode Iterator, la covariance et les expressionsLINQ (Language-Integrated Query). C# convient parfaitement si vous ciblezXAML, souhaitez commencer à développer rapidement votre jeu ou bénéficiez déjà d’une expérience enC#. C# est utilisé essentiellement avec XAML. Si vous voulez utiliser DirectX, choisissez plutôt C++ ou écrivez une partie de votre jeu en tant que composant C++ qui interagit avec DirectX. Pensez également à [Win2D](https://github.com/Microsoft/Win2D), une bibliothèque de graphismes Direct2D en mode immédiat pour C# et C++.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>C# : Guide de programmation et informations de référence</td>
        <td><a href="https://msdn.microsoft.com/library/kx37x362.aspx">Informations de référence sur le langage C#</a></td>
    </tr>
</table>
 

#### <a name="javascript"></a>JavaScript

JavaScript est un langage de script dynamique largement utilisé pour les applications web modernes et les applications clientes enrichies.

Les applications Windows app en JavaScript peuvent accéder aux puissantes fonctionnalités de la plateforme Universal Windows Platform (UWP) d’une façon simple et intuitive, comme les méthodes et les propriétés des classes JavaScript orientées objet. JavaScript convient bien à votre jeu si vous venez d’un environnement de développement web, si vous maîtrisez déjà ce langage ou si vous souhaitez utiliser les bibliothèquesHTML5, CSS, WinJS ou JavaScript. Si vous ciblez DirectX ou XAML, préférez C# ou C++/CX.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Informations de référence sur JavaScript et Windows Runtime</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/jj613794">Informations de référence JavaScript</a></td>
    </tr>
</table>


#### <a name="use-windows-runtime-components-to-combine-languages"></a>Combinaison des langages à l’aide des composants Windows Runtime

Grâce à la plateforme UWP, vous combinez facilement des composants écrits dans différents langages. Créez des composants Windows Runtime en C++, C# ou Visual Basic, puis appelez-les à partir de JavaScript, C#, C++ ou Visual Basic. C’est là une méthode remarquable pour programmer des parties de votre jeu dans le langage de votre choix. Les composants vous permettent également d’utiliser les bibliothèques externes qui ne sont disponibles que dans un langage particulier ainsi que le code hérité que vous avez déjà écrit.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Création de composants Windows Runtime</td>
        <td><a href="https://docs.microsoft.com/windows/uwp/winrt-components/creating-windows-runtime-components-in-cpp">Création de composants Windows Runtime</a></td>
    </tr>
</table>


### <a name="which-version-of-directx-should-your-game-use"></a>Quelle version de DirectX utiliser dans votre jeu ?

Si vous choisissez DirectX pour votre jeu, vous devez déterminer la version à utiliser: Microsoft Direct3D12 ou Direct3D11 Microsoft.

DirectX 12 est plus rapide et plus efficace que n’importe quelle version précédente. Direct3D 12 permet des scènes plus riches, un plus grand nombre d’objets et des effets plus complexes. Il tire également mieux parti du matériel graphique moderne sur PC Windows10 et Xbox One. Étant donné que Direct3D12 fonctionne à un niveau très faible, il donne aux équipes de développement de graphiques expertes, ou aux équipes de développement de DirectX11 expérimentées, les moyens de maximiser l’optimisation des graphiques.

Direct3D11.3 est une API graphique de niveau faible, qui utilise le modèle de programmation Direct3D familier, et prend plus facilement en charge le processus complexe de rendu GPU. Elle est également prise en charge dans Windows10 et XboxOne. Si vous disposez d’un moteur existant écrit en Direct3D11 et que vous n’êtes pas encore prêt à effectuer la transition vers Direct3D12, vous pouvez utiliser Direct3D11 sur 12 pour obtenir certaines améliorations des performances. Les versions 11.3 et ultérieures contiennent également les nouvelles fonctionnalités de rendu et d’optimisation présentes dans Direct3D12.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Si vous choisissez Direct3D12 ou Direct3D11</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/dn899228">Qu’est Direct3D12?</a></td>
    </tr>
    <tr>
        <td>Vue d’ensemble de Direct3D11</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/ff476080">Graphismes Direct3D 11</a></td>
    </tr>
    <tr>
        <td>Vue d’ensemble de Direct3D 11 sur 12</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/dn913195">Direct3D 11 sur 12</a></td>
    </tr>
</table>


### <a name="bridges-game-engines-and-middleware"></a>Ponts, moteurs de jeu et intergiciels

En fonction des besoins de votre jeu, l’utilisation de ponts, de moteurs de jeu ou d’intergiciels peut économiser du temps et des ressources de développement et de test. Voici une vue d’ensemble et les ressources des ponts, moteurs de jeu et intergiciels.

#### <a name="universal-windows-platform-bridges"></a>Ponts de plateforme Windows universelle

Les ponts de plateforme Windows universelle sont des technologies qui amènent votre application ou votre jeu existant à la plateforme UWP. Ils sont un excellent moyen de démarrer rapidement le développement des jeux UWP.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Ponts UWP</td>
        <td><a href="https://dev.windows.com/bridges/">Importer votre code dans Windows</a></td>
    </tr>
    <tr>
        <td>Pont Windows pour iOS</td>
        <td><a href="https://dev.windows.com/bridges/ios">Porter vos applications iOS vers Windows</a></td>
    </tr>
    <tr>
        <td>Pont Windows pour les applications de bureau (.NET et Win32)</td>
        <td><a href="https://developer.microsoft.com/windows/bridges/desktop">Convertir votre application de bureau en application UWP</a></td>
    </tr>
</table>

#### <a name="playfab"></a>PlayFab

PlayFab, qui fait désormais partie de la famille Microsoft, est une plateforme principale complète pour les jeux en direct et offre aux studios indépendants un moyen puissant pour se lancer. Stimulez les revenus, l’engagement et la durée de rétention, tout en réduisant les coûts, grâce aux services de jeu, l’analyse en temps réel et LiveOps.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>PlayFab</td>
        <td><a href="https://playfab.com/">Vue d’ensemble des outils et des services</a></td>
    </tr>
    <tr>
        <td>Mise en route</td>
        <td><a href="https://api.playfab.com/docs/general-getting-started">Guide de prise en main général</a></td>
    </tr>
    <tr>
        <td>Série de didacticiels vidéo</td>
        <td><a href="https://www.youtube.com/watch?v=fGNpiqVi5xU&list=PLHCfyL7JpoPbLpA_oh_T5PKrfzPgCpPT5">Série de vidéos de démonstration sur les systèmes de base de PlayFab</a></td>
    </tr>
    <tr>
        <td>Recettes</td>
        <td><a href="https://api.playfab.com/docs/tutorials/recipes-index">Exemples de modèle de conception et de mécanismes de jeux populaires</a></td>
    </tr>
    <tr>
        <td>Plateformes</td>
        <td><a href="https://api.playfab.com/platforms">Documentation spécifique pour des plateformes et des moteurs de jeu</a></td>
    </tr>
    <tr>
        <td>Référentiel GitHub</td>
        <td><a href="https://github.com/PlayFab">Accédez à des scripts et des kits de développement logiciel pour de nombreuses plateformes dont Android, iOS, Windows, Unity, Unreal.</a></td>
    </tr>
    <tr>
        <td>Documentation API</td>
        <td><a href="https://api.playfab.com/documentation/">Accès au service PlayFab directement via les API Web REST-like</a></td>
    </tr>
    <tr>
        <td>Forums</td>
        <td><a href="https://community.playfab.com/index.html">Forums PlayFab</a></td>
    </tr>
</table>
 

#### <a name="unity"></a>Unity

Unity offre une plateforme pour créer des jeux et des applications magnifiques, intuitives et conviviales en 2D, 3D et VR. Il vous permet d’exploiter au mieux votre créativité et fournit votre contenu sur pratiquement n’importe quel support ou appareil.

Unity prend en charge le développement de Direct3D 12 à partir d’Unity 5.4.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Moteur de jeu Unity</td>
        <td><a href="http://unity3d.com/">Unity: Moteur de jeu</a></td>
    </tr>
    <tr>
        <td>Obtenir Unity</td>
        <td><a href="http://unity3d.com/get-unity">Obtenir Unity</a></td>
    </tr>
    <tr>
        <td>Documentation Unity pour Windows</td>
        <td><a href="http://docs.unity3d.com/Manual/Windows.html">Manuel Unity/Windows</a></td>
    </tr>
    <tr>
        <td>Ajouter LiveOps à l’aide de PlayFab</td>
        <td><a href="https://api.playfab.com/docs/getting-started/unity-getting-started">Prise en main: passer votre premier appel d’API PlayFab depuis votre jeu Unity</a></td>
    </tr>
    <tr>
        <td>Ajout d’interactivité à votre jeu à l’aide de Mixer Interactive</td>
        <td><a href="https://github.com/mixer/interactive-unity-plugin/wiki/Getting-started">Guide Prise en main</a></td>
    </tr>
    <tr>
        <td>SDK Mixer pour Unity</td>
        <td><a href="https://www.assetstore.unity3d.com/en/#!/content/88585">Plug-in Mixer Unity</a></td>
    </tr>
    <tr>
        <td>Documentation de référence du SDK Mixer pour Unity</td>
        <td><a href="https://dev.mixer.com/reference/interactive/csharp/index.html">Documentation de référence de l’API pour le plug-in Mixer Unity</a></td>
    </tr>
    <tr>
        <td>Publier votre jeu Unity dans le Microsoft Store</td>
        <td><a href="https://unity3d.com/partners/microsoft/porting-guides">Guides de portage</a></td>
    </tr>
    <tr>
        <td>Dépannage: références d’assembly manquantes associées aux API .NET</td>
        <td><a href="https://docs.microsoft.com/windows/uwp/gaming/missing-dot-net-apis-in-unity-and-uwp">API .NET manquantes dans Unity et UWP</a></td>
    </tr>
    <tr>
        <td>Publier votre jeu Unity en tant qu’application de plateforme universelle Windows (vidéo)</td>
        <td><a href="https://channel9.msdn.com/Blogs/One-Dev-Minute/How-to-publish-your-Unity-game-as-a-UWP-app">Comment publier votre jeu Unity en tant qu’application UWP</a></td>
    </tr>
    <tr>
        <td>Utiliser Unity pour créer des jeux et applications Windows (vidéo)</td>
        <td><a href="https://channel9.msdn.com/Blogs/One-Dev-Minute/Making-games-and-apps-with-Unity">Création de jeux et applications Windows avec Unity</a></td>
    </tr>
    <tr>
        <td>Développement de jeux Unity à l’aide de Visual Studio (série de vidéos)</td>
        <td><a href="http://go.microsoft.com/fwlink/?LinkId=722359">Utilisation d’Unity avec Visual Studio 2015</a></td>
    </tr>
</table>
 

#### <a name="havok"></a>Havok

Avec la suite modulaire d’outils et de technologies de Havok, les créateurs de jeux peuvent atteindre de nouveaux niveaux d’interactivité et d’immersion. Havok permet de fournir des données physiques réalistes et d’effectuer des simulations interactives, ainsi que de remarquables animations. La version2015.1 et les versions ultérieures prennent officiellement en charge UWP dans VisualStudio2015 sur x86, 64bits et ARM.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Site web Havok</td>
        <td><a href="http://www.havok.com/">Havok</a></td>
    </tr>
    <tr>
        <td>Suite d’outils Havok</td>
        <td><a href="http://www.havok.com/products/">Vue d’ensemble des produits Havok</a></td>
    </tr>
    <tr>
        <td>Forums de support Havok</td>
        <td><a href="http://support.havok.com">Havok</a></td>
    </tr>
</table>
 

#### <a name="monogame"></a>MonoGame

MonoGame est une infrastructure de développement open source inter-plateforme initialement basée sur Microsoft XNA Framework4.0. Monogame prend actuellement en charge Windows, WindowsPhone, Xbox, ainsi que Linux, MacOS, iOS, Android et certaines autres plateformes.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>MonoGame</td>
        <td><a href="http://www.monogame.net">Site web de MonoGame</a></td>
    </tr>
    <tr>
        <td>Documentation MonoGame</td>
        <td><a href="http://www.monogame.net/documentation/">Documentation MonoGame (version la plus récente)</a></td>
    </tr>
    <tr>
        <td>Téléchargements MonoGame</td>
        <td><a href="http://www.monogame.net/downloads/">Téléchargez des versions, des builds de développement et du code source</a> sur le site web de MonoGame, ou <a href="https://www.nuget.org/profiles/MonoGame">obtenez la version la plus récente via NuGet</a>.
    </tr>
    <tr>
        <td>Exemple de jeu UWP dans MonoGame2D</td>
        <td><a href="../get-started/get-started-tutorial-game-mg2d.md">Créer un jeu UWP dans MonoGame2D</a></td>
    </tr>    
</table>


#### <a name="cocos2d"></a>Cocos2d

Cocos2d-x est une suite d’outils et de moteurs de jeu open source multiplateforme qui prend en charge la création de jeux UWP. Depuis la version 3, des fonctionnalités 3D sont également ajoutées.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Cocos2d-x</td>
        <td><a href="http://www.cocos2d-x.org/">Qu’est-ce que Cocos2d-x?</a></td>
    </tr>
    <tr>
        <td>Guide du programmeur Cocos2d-x</td>
        <td><a href="http://www.cocos2d-x.org/programmersguide/">Guide du programmeur Cocos2d-x</a></td>
    </tr>
    <tr>
        <td>Cocos2d-x sur Windows10 (billet de blog)</td>
        <td><a href="https://blogs.windows.com/buildingapps/2015/06/15/running-cocos2d-x-on-windows-10/">Exécution de Cocos2d-x sur Windows10</a></td>
    </tr>
    <tr>
        <td>Ajouter LiveOps à l’aide de PlayFab</td>
        <td><a href="https://api.playfab.com/docs/getting-started/cocos2d-x-getting-started-guide">Prise en main: passer votre premier appel d’API PlayFab depuis votre jeu Cocos2d</a></td>
    </tr>
</table>


#### <a name="unreal-engine"></a>Unreal Engine

Unreal Engine 4 est une suite complète d’outils de développement de jeux destinée à tous les types de jeu et de développement. Destiné aux jeux pour consoles et PC très exigeants, Unreal Engine est utilisé par les développeurs de jeux du monde entier.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Vue d’ensemble d’Unreal Engine</td>
        <td><a href="https://www.unrealengine.com/what-is-unreal-engine-4">Unreal Engine4</a></td>
    </tr>
    <tr>
        <td>Ajouter LiveOps à l’aide de PlayFab - C++</td>
        <td><a href="https://api.playfab.com/docs/getting-started/unreal-cpp-getting-started">Prise en main: passer votre premier appel d’API PlayFab depuis votre jeu Unreal</a></td>
    </tr>
    <tr>
        <td>Ajouter LiveOps à l’aide de PlayFab - Plans</td>
        <td><a href="https://api.playfab.com/docs/getting-started/unreal-blueprints-getting-started">Prise en main: passer votre premier appel d’API PlayFab depuis votre jeu Unreal</a></td>
    </tr>
</table>

#### <a name="babylonjs"></a>BabylonJS

BabylonJS est une infrastructure JavaScript complète destinée à créer des jeux en 3D avec HTML5, WebGL, WebVR et Web Audio.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>BabylonJS</td>
        <td><a href="http://www.babylonjs.com/">BabylonJS</a></td>
    </tr>
    <tr>
        <td>WebGL 3D avec HTML5 et BabylonJS (série de vidéos)</td>
        <td><a href="https://channel9.msdn.com/Series/Introduction-to-WebGL-3D-with-HTML5-and-Babylonjs/01">Découverte de 3D WebGL et BabylonJS</a></td>
    </tr>
    <tr>
        <td>Création d’un jeu WebGL multiplateforme avec BabylonJS</td>
        <td><a href="https://www.smashingmagazine.com/2016/07/babylon-js-building-sponza-a-cross-platform-webgl-game/">Utiliser BabylonJS pour développer un jeu multiplateforme</a></td>
    </tr>    
</table>

### <a name="porting-your-game"></a>Portage du jeu

Si vous disposez d’un jeu, nombre de ressources et de guides disponibles vous permettent de l’importer rapidement dans la plateforme UWP. Pour vous lancer rapidement dans le portage, vous pouvez également penser à utiliser un [pont de plateforme Windows universelle (UWP)](#universal-windows-platform-bridges).

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Portage d’une application Windows8 vers une application de plateforme Windows universelle</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/mt238322">Passer de Windows Runtime 8.x à UWP</a></td>
    </tr>
    <tr>
        <td>Portage d’une application Windows8 vers une application de plateforme Windows universelle (vidéo)</td>
        <td><a href="https://channel9.msdn.com/Series/A-Developers-Guide-to-Windows-10/21">Portage d’applications Windows8.1 vers Windows10</a></td>
    </tr>
    <tr>
        <td>Portage d’une application iOS vers une application de plateforme Windows universelle (vidéo)</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/mt238320">Migrer d’iOS vers UWP</a></td>
    </tr>
    <tr>
        <td>Portage d’une application Silverlight vers une application de plateforme Windows universelle</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/mt238323">Passer de Silverlight pour Windows Phone à UWP</a></td>
    </tr>
    <tr>
        <td>Portage d’une application Silverlight ou XAML vers une application de plateforme Windows universelle (vidéo)</td>
        <td><a href="https://channel9.msdn.com/Events/Build/2015/3-741">Portage d’une application XAML ou Silverlight vers Windows10</a></td>
    </tr>
    <tr>
        <td>Portage d’une application Xbox vers une application de plateforme Windows universelle</td>
        <td><a href="https://developer.xboxlive.com/en-us/platform/development/education/Documents/Porting%20from%20Xbox%20One%20to%20Windows%2010.aspx">Portage de Xbox One vers Windows10 UWP</a></td>
    </tr>
    <tr>
        <td>Portage de DirectX 9 vers DirectX 11</td>
        <td><a href="porting-your-directx-9-game-to-windows-store.md">Porter de DirectX 9 vers la plateforme Windows universelle (UWP)</a></td>
    </tr>
    <tr>
        <td>Portage de Direct3D 11 vers Direct3D 12</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/mt431709">Portage de Direct3D 11 vers Direct3D 12</a></td>
    </tr>
    <tr>
        <td>Portage d’OpenGL ES vers Direct3D 11</td>
        <td><a href="port-from-opengl-es-2-0-to-directx-11-1.md">Passer d’OpenGL ES 2.0 à Direct3D 11</a></td>
    </tr>
    <tr>
        <td>Passer d’OpenGL ES à Direct3D 11 en utilisant ANGLE</td>
        <td><a href="http://go.microsoft.com/fwlink/p/?linkid=618387">ANGLE</a></td>
    </tr>
    <tr>
        <td>Équivalents des API Windows classiques dans UWP</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/hh464945">Alternatives aux API Windows dans les applications de plateforme Windows universelle (UWP)</a></td>
    </tr>
</table>


## <a name="prototype-and-design"></a>Prototype et conception


Maintenant que vous avez choisi le type de jeu à créer et les outils et la technologie graphique que vous allez utiliser pour ce faire, vous êtes prêt à passer à sa conception et à la création de son prototype. Comme le cœur de votre jeu est une application deplateforme Windows universelle, c’est par là que vous allez commencer.

### <a name="introduction-to-the-universal-windows-platform-uwp"></a>Présentation de la plateforme Windows universelle (UWP)

Windows10 introduit la plateforme Windows universelle (UWP), qui fournit une plateforme des API courantes des appareils Windows10. UWP évolue et développe le modèle Windows Runtime pour le perfectionner et le transformer en noyau cohérent et unifié. Les jeux qui ciblent la plateforme UWP peuvent appeler les API WinRT qui sont communes à tous les appareils. Comme la plateforme UWP fournit des couches API système garantie, vous pouvez choisir de créer un package d’application unique qui s’installera sur les appareils Windows10. Et si vous le souhaitez, votre jeu peut toujours appeler les API (y compris des API Windows classiques de Win32 et .NET) propres aux appareils sur lesquels votre jeu s’exécute.

Les guides indiqués ci-dessous sont excellents. Ils décrivent en détail les applications de plateforme Windows universelle, et il est vivement recommandé de les lire pour mieux comprendre ce qu’est cette plateforme.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Présentation des applications de plateforme Windows universelle</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/dn726767">Qu’est-ce qu’une application de plateforme universelle Windows?</a></td>
    </tr>
    <tr>
        <td>Vue d’ensemble de la plateforme UWP</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/dn894631">Guide des applications UWP</a></td>
    </tr>
</table>
 

### <a name="getting-started-with-uwp-development"></a>Prise en main du développement UWP

La préparation au développement d’une application Windows universelle est rapide et facile. Les guides suivants vous décrivent le processus étape par étape.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Prise en main du développement UWP</td>
        <td><a href="https://dev.windows.com/getstarted">Prise en main des applications Windows</a></td>
    </tr>
    <tr>
        <td>Préparation au développement UWP</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/dn726766">Préparation</a></td>
    </tr>
</table>

Si vous ne connaissez pas du tout la programmation UWP et que vous envisagez d’utiliser XAML dans votre jeu (voir [Choix de la technologie graphique et du langage de programmation](#choosing-your-graphics-technology-and-programming-language)), la série de vidéos [Développement sur Windows10 pour les néophytes](https://channel9.msdn.com/Series/Windows-10-development-for-absolute-beginners) est idéale pour commencer.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Guide des débutants pour le développement pour Windows10 avec le langage XAML (série de vidéos)</td>
        <td><a href="https://channel9.msdn.com/Series/Windows-10-development-for-absolute-beginners">Développement sur Windows10 pour les néophytes</a></td>
    </tr>
    <tr>
        <td>Annonce de la série sur Windows10 pour néophytes utilisant XAML (billet de blog)</td>
        <td><a href="http://blogs.windows.com/buildingapps/2015/09/30/windows-10-development-for-absolute-beginners/">Développement sur Windows10 pour les néophytes</a></td>
    </tr>
</table>

### <a name="uwp-development-concepts"></a>Concepts de développement UWP

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Vue d’ensemble du développement d’une application de plateforme universelle Windows</td>
        <td><a href="https://dev.windows.com/develop">Développement d’applications Windows</a></td>
    </tr>
    <tr>
        <td>Vue d’ensemble de la programmation réseau dans UWP</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/mt280378">Mise en réseau et services web</a></td>
    </tr>
    <tr>
        <td>Utilisation de Windows.Web.HTTP et Windows.Networking.Sockets dans les jeux</td>
        <td><a href="work-with-networking-in-your-directx-game.md">Mise en réseau pour les jeux</a></td>
    </tr>
    <tr>
        <td>Concepts de programmation asynchrone dans UWP</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/mt187335">Programmation asynchrone</a></td>
    </tr>
</table>

### <a name="windows-desktop-apisto-uwp"></a>Windows Desktop APIsto UWP

Voici quelques liens pour vous aider à faire migrer votre jeu de bureau Windows vers UWP.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Utilisez le code C++ existant pour le développement de jeux UWP</td>
        <td><a href="https://docs.microsoft.com/cpp/porting/how-to-use-existing-cpp-code-in-a-universal-windows-platform-app">Procédure: utiliser le code C++ existant dans une application UWP</a></td>
    </tr>
    <tr>
        <td>API UWP pour les API Win32 et COM</td>
        <td><a href="https://docs.microsoft.com/uwp/win32-and-com/win32-and-com-for-uwp-apps">API Win32 et COM pour les applications UWP</a></td>
    </tr>
    <tr>
        <td>Fonctions CRT non prises en charge dans UWP</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/jj606124.aspx">Fonctions CRT non prises en charge dans les applications de la plateforme Windows universelle</a></td>
    </tr>
    <tr>
        <td>Alternatives aux API Windows</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/mt592894.aspx">Alternatives aux API Windows dans les applications de plateforme Windows universelle (UWP)</a></td>
    </tr>
</table>
 

### <a name="process-lifetime-management"></a>Gestion de la durée de vie des processus

La gestion de la durée de vie des processus, ou cycle de vie des applications, décrit les différents états d’activation que peut traverser une application de plateforme Windows universelle. Votre jeu peut être activé, suspendu, rétabli ou arrêté, et il peut transiter par ces états de plusieurs manières.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Gestion des transitions du cycle de vie des applications</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/mt243287">Cycle de vie de l’application</a></td>
    </tr>
    <tr>
        <td>Utilisation de Microsoft Visual Studio pour déclencher des transitions d’application</td>
        <td><a href="https://msdn.microsoft.com/library/hh974425.aspx">Comment déclencher des événements de suspension, de reprise et en arrière-plan pour des applications UWP dans Visual Studio</a></td>
    </tr>
</table>
 

### <a name="designing-game-ux"></a>Conception de l’expérience utilisateur de jeux

Une conception inspirée est à la source d’un jeu réussi.

Les jeux partagent certains éléments d’interface utilisateur et des principes de conception communs avec les applications, mais ils ont souvent une apparence et un objectif de conception uniques pour leur expérience utilisateur. Les jeux rencontreront le succès si les aspects suivants sont bien pensés: quand votre jeu doit-il utiliser une expérience utilisateur testée et quand doit-il varier et innover? La technologie de présentation que vous choisissez pour votre jeu (DirectX, XAML, HTML5 ou une combinaison de celles-ci) peut influencer les détails d’implémentation, mais les principes de conception que vous appliquez ne reposent pas sur ce choix.

Distincte de la conception de l’expérience utilisateur, la conception d’un jeu, par exemple la conception du niveau, le rythme et bien d’autres aspects sont une forme d’art en soi. Elle est de votre ressort, votre équipe et vous, et elle n’est pas traitée dans ce guide de développement.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Informations de base et recommandations sur la conception UWP</td>
        <td><a href="https://dev.windows.com/design">Conception des applications UWP</a></td>
    </tr>
    <tr>
        <td>Conception des états de cycle de vie d’application</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/dn611862">Recommandations en matière d’expérience utilisateur pour le lancement, la suspension et la reprise</a></td>
    </tr>
    <tr>
        <td>Concevez votre application UWP pour les écrans de télévision et la console Xbox One</td>
        <td><a href="https://docs.microsoft.com/windows/uwp/design/devices/designing-for-tv">Conception pour Xbox et téléviseur</a></td>
    </tr>
    <tr>
        <td>Ciblage de plusieurs facteurs de forme d’appareil (vidéo)</td>
        <td><a href="http://channel9.msdn.com/Events/GDC/GDC-2015/Designing-Games-for-a-Windows-Core-World">Conception de jeux pour Windows Core</a></td>
    </tr>   
</table>
 

#### <a name="color-guideline-and-palette"></a>Recommandations de couleur et palette

Le respect de recommandations de couleur cohérentes dans votre jeu lui apporte esthétisme, simplifie la navigation et permet d’informer le joueur sur la fonctionnalité du menu et de l’affichage à tête haute. L’application de couleurs cohérentes aux éléments du jeu comme les avertissements, dommages, XP et scores peut permettre d’obtenir une interface utilisateur plus claire et de réduire l’emploi de libellés explicites.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Guide des couleurs</td>
        <td><a href="https://assets.windowsphone.com/499cd2be-64ed-4b05-a4f5-cd0c9ad3f6a3/101_BestPractices_Color_InvariantCulture_Default.zip">Meilleures pratiques : Couleur</a></td>
    </tr>
</table>
 

#### <a name="typography"></a>Typographie

L’utilisation appropriée de la typographie améliore de nombreux aspects de votre jeu, notamment la disposition de l’interface utilisateur, la navigation, la lisibilité, l’ambiance, la marque et l’immersion du joueur.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Guide de la typographie</td>
        <td><a href="http://go.microsoft.com/fwlink/?LinkId=535007">Meilleures pratiques : Typographie</a></td>
    </tr>
</table>
 

#### <a name="ui-map"></a>Carte d’interface utilisateur

Une carte d’interface utilisateur est une disposition de la navigation et des menus du jeu présentée sous la forme d’un organigramme. La carte d’interface utilisateur permet à toutes les parties concernées de comprendre l’interface et les chemins de navigation du jeu, et permet d’exposer les blocages et arrêts dès la phase de développement.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Guide sur la carte d’interface utilisateur</td>
        <td><a href="http://go.microsoft.com/fwlink/?LinkId=535008">Meilleures pratiques: Carte d’interface utilisateur</a></td>
    </tr>
</table>

### <a name="game-audio"></a>Son du jeu

Guides et références pour l’implémentation de son dans les jeux à l’aide de XAudio2, XAPO et Windows Sonic. XAudio2 est une API audio de bas niveau qui fournit les bases en matière de traitement du signal et de mixage pour le développement de moteurs audio hautes performances. L’API XAPO permet la création d’objets de traitement audio multiplateforme (XAPO) pour une utilisation dans XAudio2 sur Windows et Xbox. La prise en charge audio Windows Sonic permet d’ajouter la prise en charge de Dolby Atmos for Home Theater, Dolby Atmos for Headphones, et de Windows HRTF à votre jeu ou application multimédia de diffusion en continu.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>API XAudio2</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/hh405049.aspx">Guide de programmation et informations de référence sur les API pour XAudio2</a></td>
    </tr>
    <tr>
        <td>Créer des objets de traitement audio multiplateforme</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/ee415735.aspx">Vue d’ensemble de l’authentificationXAPO</a></td>
    </tr>
    <tr>
        <td>Présentation des concepts audio</td>
        <td><a href="working-with-audio-in-your-directx-game.md">Audio pour les jeux</a></td>
    </tr>
    <tr>
        <td>Vue d’ensemble de Windows Sonic</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/mt807491.aspx">Son spatial</a></td>
    </tr>
    <tr>
        <td>Exemples de sons spatiaux Windows Sonic</td>
        <td><a href="https://github.com/Microsoft/Xbox-ATG-Samples/tree/master/UWPSamples/Audio">Exemples audio Xbox Advanced Technology Group</a></td>
    </tr>
    <tr>
        <td>Découvrez comment intégrer Windows Sonic dans vos jeux (vidéo)</td>
        <td><a href="https://channel9.msdn.com/Events/GDC/GDC-2017/GDC2017-002">Présentation Spatial Audio Capabilities for Xbox andWindows</a></td>
    </tr>
</table>

### <a name="directx-development"></a>Développement DirectX

Guides et références pour le développement de jeux DirectX.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Développement DirectX pour UWP</td>
        <td><a href="directx-programming.md">Programmation DirectX</a></td>
    </tr>
    <tr>
        <td>Didacticiel: comment créer un jeu UWP DirectX</td>
        <td><a href="tutorial--create-your-first-uwp-directx-game.md">Créer un jeu UWP simple avec DirectX</a></td>
    </tr>
    <tr>
        <td>Interaction de DirectX avec le modèle d’application UWP</td>
        <td><a href="about-the-uwp-user-interface-and-directx.md">Objet application et DirectX</a></td>
    </tr>
    <tr>
        <td>Vidéos de développement Graphics et DirectX12 (YouTube)</td>
        <td><a href="https://www.youtube.com/channel/UCiaX2B8XiXR70jaN7NK-FpA">MicrosoftDirectX12 et GraphicsEducation</a></td>
    </tr>
    <tr>
        <td>Présentations et informations de référence sur DirectX</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/ee663274">Jeux et graphismes DirectX</a></td>
    </tr>
    <tr>
        <td>Direct3D 12 : Guide de programmation et informations de référence</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/dn903821">Graphismes Direct3D 12</a></td>
    </tr>
    <tr>
        <td>Notions fondamentales sur DirectX 12 (vidéo)</td>
        <td><a href="http://channel9.msdn.com/Events/GDC/GDC-2015/Better-Power-Better-Performance-Your-Game-on-DirectX12">Une meilleure alimentation, de meilleures performances: Votre jeu sur DirectX12</a></td>
    </tr>
</table>

#### <a name="learning-direct3d-12"></a>Prise en main de Direct3D12

Découvrez ce qui a changé dans Direct3D12 et comment commencer à programmer à l’aide de Direct3D12. 

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Configurer l’environnement de programmation</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/dn899120.aspx">Configuration de l’environnement de programmation Direct3D12</a></td>
    </tr>
    <tr>
        <td>Comment créer un composant de base</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/dn859356.aspx">Création d’un composant Direct3D12 de base</a></td>
    </tr>
    <tr>
        <td>Modifications apportées dans Direct3D12</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/dn899194.aspx">Modifications importantes lors de la migration de Direct3D11 à Direct3D12</a></td>
    </tr>
    <tr>
        <td>Comment effectuer le portage de Direct3D11 vers Direct3D12</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/mt431709.aspx">Portage de Direct3D11 vers Direct3D12</a></td>
    </tr>
    <tr>
        <td>Concepts de liaison de ressource (descripteur de recouvrement, tableau de descripteur, tas de descripteur et signature racine) </td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/dn899206.aspx">Liaison de ressource dans Direct3D12</a></td>
    </tr>
    <tr>
        <td>Gestion de la mémoire</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/dn899198.aspx">Gestion de la mémoire dans Direct3D12</a></td>
    </tr>
</table>
 

#### <a name="directx-tool-kit-and-libraries"></a>Kit de ressources et bibliothèques DirectX

Le kit de ressources DirectX, la bibliothèque de traitement des textures DirectX, la bibliothèque de traitement des géométries DirectXMesh, la bibliothèque UVAtlas et la bibliothèque DirectXMath fournissent des fonctionnalités de texture, maillage, sprite etc., ainsi que des classes d’assistance pour le développement avec DirectX. Ces bibliothèques peuvent vous faire gagner du temps et de l’énergie lors du développement.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Obtenir le kit de ressources DirectX pour DirectX11</td>
        <td><a href="http://go.microsoft.com/fwlink/?LinkId=248929">DirectXTK</a></td>
    </tr>
    <tr>
        <td>Obtenir le kit de ressources DirectX pour DirectX12</td>
        <td><a href="http://go.microsoft.com/fwlink/?LinkID=615561">DirectXTK12</a></td>
    </tr>
    <tr>
        <td>Obtenir la bibliothèque de traitement des textures DirectX</td>
        <td><a href="http://go.microsoft.com/fwlink/?LinkId=248926">DirectXTex</a></td>
    </tr>
    <tr>
        <td>Obtenir la bibliothèque de traitement des géométries DirectXMesh</td>
        <td><a href="http://go.microsoft.com/fwlink/?LinkID=324981">DirectXMesh</a></td>
    </tr>
    <tr>
        <td>Obtenir UVAtlas pour la création et la compression d’atlas de textures isochart</td>
        <td><a href="http://go.microsoft.com/fwlink/?LinkID=512686">UVAtlas</a></td>
    </tr>
    <tr>
        <td>Obtenir la bibliothèque DirectXMath</td>
        <td><a href="http://go.microsoft.com/fwlink/?LinkID=615560">DirectXMath</a></td>
    </tr>
    <tr>
        <td>Prise en charge de Direct3D12 dans DirectXTK (billet de blog)</td>
        <td><a href="https://github.com/Microsoft/DirectXTK/issues/2">Prise en charge de DirectX12</a></td>
    </tr>
</table>

#### <a name="directx-resources-from-partners"></a>Ressources DirectX provenant de partenaires

Voici des documentations supplémentaires sur DirectX, créées par des partenaires externes.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Nvidia: DX12 Do’s and Don’ts (billet de blog en anglais) </td>
        <td><a href="https://developer.nvidia.com/dx12-dos-and-donts-updated">DirectX12 sur des GPU Nvidia</a></td>
    </tr>
    <tr>
        <td>Intel: Efficient rendering with DirectX12 (en anglais)</td>
        <td><a href="https://software.intel.com/sites/default/files/managed/4a/38/Efficient-Rendering-with-DirectX-12-on-Intel-Graphics.pdf">Rendu de DirectX12 sur graphiques Intel</a></td>
    </tr>
    <tr>
        <td>Intel: Multi adapter support in DirectX12 (en anglais)</td>
        <td><a href="https://software.intel.com/articles/multi-adapter-support-in-directx-12">Implémentation d’une application explicite comportant plusieurs adaptateurs à l’aide de DirectX12</a></td>
    </tr>
    <tr>
        <td>Intel: DirectX12 tutorial (en anglais)</td>
        <td><a href="https://software.intel.com/articles/tutorial-migrating-your-apps-to-directx-12-part-1">Livre blanc collaboratif, élaboré par Intel, SuzhouSnail et Microsoft</a></td>
    </tr>
</table>


## <a name="production"></a>Production


À présent, votre studio est totalement engagé dans le cycle de production, des tâches étant distribuées à tous les membres de votre équipe. Vous peaufinez, refactorisez et étendez le prototype pour en faire un jeu complet.

### <a name="notifications-and-live-tiles"></a>Notifications et vignettes dynamiques

Une vignette est la représentation de votre jeu dans le menu Démarrer. Les vignettes et les notifications peuvent susciter l’intérêt des joueurs même s’ils n’utilisent pas votre jeu.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Développement de vignettes et de badges</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/mt185606">Vignettes, badges et notifications</a></td>
    </tr>
    <tr>
        <td>Exemple illustrant les vignettes dynamiques et les notifications</td>
        <td><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Notifications">Exemples de notification</a></td>
    </tr>
    <tr>
        <td>Modèles de vignette adaptative (billet de blog)</td>
        <td><a href="http://blogs.msdn.com/b/tiles_and_toasts/archive/2015/06/30/adaptive-tile-templates-schema-and-documentation.aspx">Modèles de vignette adaptative : Schéma et documentation</a></td>
    </tr>
    <tr>
        <td>Conception de vignettes et de badges</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/hh465403">Recommandations en matière de vignettes et de badges</a></td>
    </tr>
    <tr>
        <td>Application Windows10 pour le développement interactif des modèles de vignette dynamique</td>
        <td><a href="https://www.microsoft.com/store/apps/9nblggh5xsl1">Notifications Visualizer</a></td>
    </tr>
    <tr>
        <td>Extension UWP Tile Generator pour Visual Studio</td>
        <td><a href="https://visualstudiogallery.msdn.microsoft.com/09611e90-f3e8-44b7-9c83-18dba8275bb2">Outil permettant de créer toutes les vignettes requises à l’aide d’une image unique</a></td>
    </tr>
    <tr>
        <td>Extension UWP Tile Generator pour Visual Studio (billet de blog)</td>
        <td><a href="https://blogs.windows.com/buildingapps/2016/02/15/uwp-tile-generator-extension-for-visual-studio/">Conseils sur l’utilisation de l’outil UWP Tile Generator</a></td>
    </tr>
</table>
 

### <a name="enable-in-app-product-add-on-purchases"></a>Activer les achats de produits in-app (module complémentaire)

Une extension (produit in-app) est un article supplémentaire que les joueurs peuvent acheter dans le jeu. Les modules complémentaires peuvent être niveaux de jeu, les éléments ou tout autre chose susceptibles de plaire à vos joueurs. Utilisés à bon escient, les modules complémentaires peuvent fournir des revenus tout en améliorant l’expérience de jeu. Vous définissez et publiez des modules complémentaires de votre jeu par le biais de l’espace partenaires et activez les achats dans l’application dans le code de votre jeu.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Modules complémentaires durables</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/mt219684">Activer les achats de produits in-app</a></td>
    </tr>
    <tr>
        <td>Modules complémentaires consommables</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/mt219683">Activer les achats de produits consommables in-app</a></td>
    </tr>
    <tr>
        <td>Soumission et des détails sur les extensions</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/mt148551">Soumissions de modules complémentaires</a></td>
    </tr>
    <tr>
        <td>Surveiller les ventes d’extensions et des données démographiques pour votre jeu</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/mt148538">Rapport sur les acquisitions d’extensions</a></td>
    </tr>
</table>
 

### <a name="debugging-performance-optimization-and-monitoring"></a>Débogage, optimisation et surveillance des performances

Pour optimiser les performances, tirez parti du mode jeu de Windows10 pour proposer à vos joueurs la meilleure expérience de jeu possible en exploitant au mieux la capacité de leur matériel actuel.

Le KitWindowsPerformanceToolkit est composé d’outils d’analyse des performances qui génèrent des profils de performances détaillés des applications et des systèmes d’exploitationWindows. Il s’avère particulièrement précieux pour surveiller l’utilisation de la mémoire et améliorer les performances des jeux. Le KitWindowsPerformanceToolkit est inclus dans le Kit de développement logicielWindows10 et dans WindowsADK. Ce kit d’outils comprend deuxoutils indépendants: l’enregistreur de performance Windows et Windows Performance Analyzer. ProcDump, qui fait partie de [Windows Sysinternals](https://technet.microsoft.com/sysinternals/default), est un utilitaire de ligne de commande qui surveille les pics d’utilisation d’UC et génère des fichiers de vidage lors des plantages de jeux. 

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Test de performances de votre code.</td>
        <td><a href="https://www.visualstudio.com/team-services/cloud-load-testing/">Test de charge basée sur le cloud</a></td>
    </tr>
    <tr>
        <td>Obtenir le type de console Xbox à l’aide des informations sur l’appareil de jeu</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/mt825235">Informations sur l’appareil de jeu</a></td>
    </tr>
    <tr>
        <td>Améliorer les performances en bénéficiant d’un accès exclusif ou prioritaire aux ressources matérielles à l’aide des APi du mode jeu</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/mt808808">Mode jeu</a></td>
    </tr>
    <tr>
        <td>Obtenir le KitWindowsPerformanceToolkit à partir de Windows10 SDK</td>
        <td><a href="https://developer.microsoft.com/windows/downloads/windows-10-sdk">Kit de développement logiciel (SDK) Windows10</a></td>
    </tr>
    <tr>
        <td>Obtenir le KitWindowsPerformanceToolkit à partir de WindowsADK.</td>
        <td><a href="https://msdn.microsoft.com/windows/hardware/dn913721.aspx">WindowsADK</a></td>
    </tr>
    <tr>
        <td>Résoudre les problèmes de réactivité de l’interface utilisateur à l’aide de Windows Performance Analyzer (vidéo).</td>
        <td><a href="https://channel9.msdn.com/Shows/Defrag-Tools/Defrag-Tools-156-Critical-Path-Analysis-with-Windows-Performance-Analyzer">Analyse du chemin critique avec WPA</a></td>
    </tr>
    <tr>
        <td>Diagnostiquer l’utilisation et les fuites de mémoire à l’aide de Enregistreur de performance Windows (vidéo)</td>
        <td><a href="https://channel9.msdn.com/Shows/Defrag-Tools/Defrag-Tools-154-Memory-Footprint-and-Leaks">Encombrement et fuites de mémoire</a></td>
    </tr>
    <tr>
        <td>Obtenir ProcDump</td>
        <td><a href="https://technet.microsoft.com/sysinternals/dd996900">ProcDump</a></td>
    </tr>
    <tr>
        <td>Apprenez à utiliser ProcDump (vidéo)</td>
        <td><a href="https://channel9.msdn.com/Shows/Defrag-Tools/Defrag-Tools-131-Windows-10-SDK">Configurer ProcDump pour créer des fichiers de vidage</a></td>
    </tr>
</table>

### <a name="advanced-directx-techniques-and-concepts"></a>Techniques et concepts DirectX avancés

Certaines parties du développement DirectX peuvent être complexes et nuancées. Lorsque vous atteignez le stade de la production où vous devez examiner les détails de votre moteur DirectX ou déboguer des problèmes complexes de performance, les ressources et les informations présentées dans cette section sont susceptibles de vous aider.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>PIX sur Windows</td>
        <td><a href="https://blogs.msdn.microsoft.com/pix/2017/01/17/introducing-pix-on-windows-beta/">Réglage des performances et outil de débogage pour DirectX12 sur Windows</a></td>
    </tr>
    <tr>
        <td>Outils de débogage et la validation pour le développement de D3D12 (vidéo)</td>
        <td><a href="https://channel9.msdn.com/Events/GDC/GDC-2017/GDC2017-003">D3D12 Performance Tuning and Debugging with PIX and GPUValidation</a></td>
    </tr>
    <tr>
        <td>Optimisation des graphismes et des performances (vidéo)</td>
        <td><a href="http://channel9.msdn.com/Events/GDC/GDC-2015/Advanced-DirectX12-Graphics-and-Performance">Graphismes et performances améliorés avec DirectX 12</a></td>
    </tr>
    <tr>
        <td>Débogage graphique DirectX (vidéo)</td>
        <td><a href="http://channel9.msdn.com/Events/GDC/GDC-2015/Solve-the-Tough-Graphics-Problems-with-your-Game-Using-DirectX-Tools">Résolution des problèmes graphiques épineux liés à votre jeu à l’aide des outils DirectX</a></td>
    </tr>
    <tr>
        <td>Outils VisualStudio2015 pour le débogage de DirectX12 (vidéo)</td>
        <td><a href="https://channel9.msdn.com/Series/ConnectOn-Demand/212">Outils DirectX pour Windows10 dans VisualStudio2015</a></td>
    </tr>
    <tr>
        <td>Guide de programmation pour Direct3D12</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/dn903821">Guide de programmation de Direct3D 12</a></td>
    </tr>
    <tr>
        <td>Combinaison de DirectX et XAML</td>
        <td><a href="directx-and-xaml-interop.md">Interopérabilité DirectX et XAML</a></td>
    </tr>
</table>

### <a name="high-dynamic-range-hdr-content-development"></a>Développement de contenu avec plage dynamique étendue (HDR)

Générer le contenu du jeu qui utilise toutes les fonctionnalités de couleurs de HDR.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Présentation de HDR et des concepts de couleur (vidéo)</td>
        <td><a href="https://channel9.msdn.com/Events/Build/2017/P4061">Éclairage HDR et couleurs avancées dans DirectX</a></td>
    </tr>
    <tr>
        <td>Découvrez comment rendre le contenu HDR et détecter si l’affichage actuel le prend en charge</td>
        <td><a href="https://github.com/Microsoft/DirectX-Graphics-Samples/tree/master/Samples/UWP/D3D12HDR">Exemple HDR</a></td>
    </tr>
    <tr>
        <td>Créer et configurer une couleur avancée à l’aide de DirectX</td>
        <td><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/D2DAdvancedColorImages">Exemple de rendu d’image couleurs avancées Direct2D</a></td>
    </tr>   
</table>


### <a name="globalization-and-localization"></a>Internationalisation et localisation

Développez des jeux prêts à l’emploi pour la plateforme Windows et découvrez les fonctionnalités internationales intégrées aux meilleurs produits Microsoft.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Préparation de votre jeu pour le marché international</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt186453.aspx">Recommandations en matière de développement pour un public international</a></td>
    </tr>
    <tr>
        <td>Combler le fossé entre les langues, les cultures et la technologie</td>
        <td><a href="http://www.microsoft.com/Language/Default.aspx">Ressources en ligne pour les conventions linguistiques et la terminologie Microsoft standard</a></td>
    </tr>
</table>

### <a name="security"></a>Sécurité

Créez un environnement dans lequel les joueurs peuvent se mesurer tout en restant fair-play. Un jeu inscrit dans TruePlay s’exécute au sein d’un processus protégé, ce qui réduit la probabilité d’une classe d’attaques courantes. Le système de surveillance de jeu permet également d’identifier les scénarios de triche courants. 

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Outils pour lutter contre la tricherie au sein de jeux pour PC</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/mt808781">TruePlay</a></td>
    </tr>
</table>

## <a name="submitting-and-publishing-your-game"></a>Envoi et publication du jeu

Les informations et guides suivants contribuent à rendre le processus de soumission et de publication aussi aisé que possible.

### <a name="publishing"></a>Publication

Vous utiliserez [L’espace partenaires](https://partner.microsoft.com/dashboard) pour publier et de gérer vos packages de jeu.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Publication d’application de l’espace partenaires</td>
        <td><a href="https://dev.windows.com/publish">Publier des applications Windows</a></td>
    </tr>
    <tr>
        <td>L’espace partenaires publication avancée (GDN)</td>
        <td><a href="https://developer.xboxlive.com/en-us/windows/documentation/Pages/home.aspx">Guide de publication avancée de l’espace partenaires</a></td>
    </tr>
    <tr>
        <td>Utiliser Azure Active Directory (AAD) pour ajouter des utilisateurs à votre compte espace partenaires</td>
        <td><a href="https://docs.microsoft.com/windows/uwp/publish/manage-account-users">Gérer des utilisateurs de compte</a></td>
    </tr>   
    <tr>
        <td>Évaluation de votre jeu (billet de blog)</td>
        <td><a href="https://blogs.windows.com/buildingapps/2016/01/06/now-available-single-age-rating-system-to-simplify-app-submissions/">Flux de travail unique pour affecter les évaluations de l’âge à l’aide du système IARC</a></td>
    </tr>
</table>

#### <a name="packaging-and-uploading"></a>Création du package et chargement

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Apprenez à utiliser l’installation en continu et les packages facultatifs (vidéos)</td>
        <td><a href="https://channel9.msdn.com/Events/Build/2017/B8093">Distribution d’applications UWP Nextgen: création componentizedapps extensible, capable de flux de données,</a></td>
    </tr>
    <tr>
        <td>Diviser et regrouper du contenu pour activer l’installation en continu</td>
        <td><a href="../packaging/streaming-install.md">Installation en continu d’une application UWP</a></td>
    </tr>
    <tr>
        <td>Créer des packages facultatifs comme du contenu de jeu DLC</td>
        <td><a href="../packaging/optional-packages.md">Packages facultatifs et création d’ensembles connexes</a></td>
    </tr>
    <tr>
        <td>Créer un package de votre jeu UWP</td>
        <td><a href="../packaging/index.md">Création de packages d’application</a></td>
    </tr>
    <tr>
        <td>Créer un package pour votre jeu UWP DirectX</td>
        <td><a href="package-your-windows-store-directx-game.md">Créer un package pour votre jeu UWP DirectX</a></td>
    </tr>
    <tr>
        <td>Empaquetage de votre jeu en tant que développeur tiers (billet de blog)</td>
        <td><a href="https://blogs.windows.com/buildingapps/2015/12/15/building-an-app-for-a-3rd-party-how-to-package-their-store-app/">Créer des packages téléchargeables sans accès au compte Windows Store de l’éditeur</a></td>
    </tr>
    <tr>
        <td>Création de packages d’application et d’ensembles de packages d’application à l’aide de MakeAppx</td>
        <td><a href="https://docs.microsoft.com/windows/uwp/packaging/create-app-package-with-makeappx-tool">Créer des packages à l’aide de l’outil de création de packages d’application MakeAppx.exe</a></td>
    </tr>
    <tr>
        <td>Signature numérique des fichiers à l’aide de SignTool</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/aa387764">Signer les fichiers et vérifier les signatures dans les fichiers à l’aide de SignTool</a></td>
    </tr>    
    <tr>
        <td>Chargement et contrôle de version de votre jeu</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/mt148542">Charger des packages d’application</a></td>
    </tr>
</table>


### <a name="policies-and-certification"></a>Stratégies et certifications

Ne laissez pas les problèmes de certification retarder la publication de votre jeu. Voici des stratégies et des problèmes courants de certification à connaître.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Contrat du développeur d’application du Microsoft Store</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/hh694058">Contrat du développeur d’application</a></td>
    </tr>
    <tr>
        <td>Stratégies pour la publication d’applications dans le Microsoft Store</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/dn764944">Politiques du MicrosoftStore</a></td>
    </tr>
    <tr>
        <td>Comment faire pour éviter certains problèmes de certification d’application courants</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/jj657968">Éviter les échecs de certification courants</a></td>
    </tr>
</table>
 

### <a name="store-manifest-storemanifestxml"></a>Manifeste de magasin (StoreManifest.xml)

Le manifeste de magasin (StoreManifest.xml) est un fichier de configuration facultatif qui peut être inclus dans votre package d’application. Il fournit des fonctionnalités supplémentaires qui ne font pas partie du fichier AppxManifest.xml. Par exemple, vous pouvez utiliser le manifeste de magasin pour bloquer l’installation de votre jeu si un appareil cible ne possède pas le niveau de fonctionnalité DirectX minimal spécifié ou la mémoire système minimale spécifiée.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Schéma du manifeste de magasin</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/mt617335">Schéma StoreManifest (Windows10)</a></td>
    </tr>
</table>
 

## <a name="game-lifecycle-management"></a>Gestion du cycle de vie des jeux


Vous n’avez pas terminé une fois que vous avez développé et fourni votre jeu. Si vous en avez fini avec le développement de la première version, le circuit de votre jeu sur le marché commence à peine quant à lui. Vous allez surveiller son utilisation et les rapports d’erreur, répondre aux commentaires des utilisateurs, et publier des mises à jour pour votre jeu.

### <a name="partner-center-analytics-and-promotion"></a>Promotion et analytique de l’espace partenaires

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Analytique de l’espace partenaires</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/mt148522">Analyser les performances de l’application</a></td>
    </tr>
    <tr>
        <td>Découvrez comment vos clients interagissent avec les fonctionnalités Xbox dans votre jeu.</td>
        <td><a href="../publish/xbox-analytics-report.md">Rapport d’analyse Xbox</a></td>
    </tr>
    <tr>
        <td>Réponse aux avis des clients</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/mt148546">Répondre aux avis des clients</a></td>
    </tr>
    <tr>
        <td>Méthodes pour promouvoir votre jeu</td>
        <td><a href="https://dev.windows.com/store-promotion">Promouvoir vos applications</a></td>
    </tr>
</table>
 

### <a name="visual-studio-application-insights"></a>Visual Studio Application Insights

Visual Studio Application Insights fournit des analyses de performance, de télémétrie et d’utilisation pour votre jeu publié. Application Insights vous permet de détecter et de résoudre les problèmes après publication de votre jeu, de surveiller et d’améliorer en continu son utilisation et de comprendre comment les joueurs ne cessent d’interagir avec votre jeu. Application Insights fonctionne si vous ajoutez un Kit de développement logiciel (SDK) dans votre application, qui envoie les données de télémétrie au [portail Azure](http://portal.azure.com/).

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Analyses de performance et d’utilisation d’application</td>
        <td><a href="https://azure.microsoft.com/documentation/articles/app-insights-get-started/">Visual Studio Application Insights</a></td>
    </tr>
    <tr>
        <td>Activer Application Insights dans les applications Windows</td>
        <td><a href="https://azure.microsoft.com/documentation/articles/app-insights-windows-get-started/">Application Insights pour les applications Windows Phone et les applications du Windows Store</a></td>
    </tr>
</table>


### <a name="third-party-solutions-for-analytics-and-promotion"></a>Solutions tierces pour l’analyse et la promotion

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Comprendre le comportement du lecteur à l’aide de GameAnalytics</td>
        <td><a href="http://www.gameanalytics.com/">GameAnalytics</a></td>
    </tr>
    <tr>
        <td>Connecter votre jeu UWP à Google Analytics</td>
        <td><a href="https://github.com/dotnet/windows-sdk-for-google-analytics">Obtenir SDK Windows pour Google Analytics</a></td>
    </tr>
    <tr>
        <td>Découvrez comment utiliser le SDK Windows pour Google Analytics (vidéo)</td>
        <td><a href="https://channel9.msdn.com/Events/Windows/Windows-Developer-Day-Creators-Update/Getting-started-with-the-Windows-SDK-for-Google-Analytics">Prise en main de SDK Windows pour Google Analytics</a></td>
    </tr>    
    <tr>
        <td>Utilisez les publicités sur l’installation d’applications Facebook pour promouvoir votre jeu auprès des utilisateurs de Facebook</td>
        <td><a href="https://github.com/Microsoft/winsdkfb">Obtenir SDK Windows pour Facebook</a></td>
    </tr>
    <tr>
        <td>Découvrez comment utiliser les publicités sur l’installation d’applications Facebook (vidéo)</td>
        <td><a href="https://channel9.msdn.com/Events/Windows/Windows-Developer-Day-Creators-Update/Getting-started-with-Facebook-App-Install-Ads">Prise en main de SDK Windows pour Facebook</a></td>
    </tr>
    <tr>
        <td>Utiliser Vungle pour ajouter des publicités vidéo dans vos jeux</td>
        <td><a href="https://v.vungle.com/sdk">Obtenir le SDK Windows pour Vungle</a></td>
    </tr>
</table>
 

### <a name="creating-and-managing-content-updates"></a>Création et gestion des mises à jour de contenu

Pour mettre à jour votre jeu publié, soumettez un nouveau package d’application avec un numéro de version supérieur. Le package est automatiquement mis à la disposition des clients en tant que mise à jour dès qu’il a passé les étapes de soumission et de certification.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Mise à jour et contrôle de version de votre jeu</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/mt188602">Numérotation des versions de packages</a></td>
    </tr>
    <tr>
        <td>Recommandations en matière de gestion des packages de jeu</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/mt188602">Aide sur la gestion des packages d’application</a></td>
    </tr>
</table>


## <a name="adding-xbox-live-to-your-game"></a>Ajout de Xbox Live à votre jeu

Xbox Live est le premier réseau de jeux qui connecte des millions de joueurs dans le monde entier. Les développeurs peuvent accéder à des fonctionnalités Xbox Live capables d’augmenter le public de leur jeu de façon drastique. Il s’agit notamment de la présence Xbox Live, des classements, des sauvegardes dans le cloud, des hubs de jeux, des clubs, des tchats de groupe, des jeux DVR et plus encore.

> [!Note]
> Si vous souhaitez développer des titres Xbox Live, plusieurs options s’offrent à vous. Pour en savoir plus sur les divers programmes, voir [vue d’ensemble du programme pour les développeurs](../xbox-live/developer-program-overview.md).

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Vue d’ensemble de Xbox Live</td>
        <td><a href="../xbox-live/index.md">Guide du développeur Xbox Live</a></td>
    </tr>
    <tr>
        <td>Comprendre quelles fonctionnalités sont disponibles en fonction du programme</td>
        <td><a href="../xbox-live/developer-program-overview.md#feature-table">Vue d’ensemble du programme pour les développeurs: tableau des fonctionnalités</a></td>
    </tr>
    <tr>
        <td>Liens vers des ressources utiles pour développer des jeux Xbox Live</td>
        <td><a href="../xbox-live/xbox-live-resources.md">Ressources Xbox Live</a></td>
    </tr>
    <tr>
        <td>Découvrir comment obtenir des informations à partir des services Xbox Live</td>
        <td><a href="../xbox-live/introduction-to-xbox-live-apis.md">Présentation des API Xbox Live</a></td>
    </tr>
</table>


### <a name="for-developers-in-the-xbox-live-creators-program"></a>Pour les développeurs du programme Créateurs Xbox Live

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Vue d’ensemble</td>
        <td><a href="../xbox-live/get-started-with-creators/get-started-with-xbox-live-creators.md">Prise en main du programme Créateurs Xbox Live</a></td>
    </tr>
    <tr>
        <td>Ajouter Xbox Live à votre jeu</td>
        <td><a href="../xbox-live/get-started-with-creators/creators-step-by-step-guide.md">Guide détaillé pour intégrer le programme Créateurs Xbox Live</a></td>
    </tr>
    <tr>
        <td>Ajouter Xbox Live à votre jeu UWP créé à l’aide d’Unity</td>
        <td><a href="../xbox-live/get-started-with-creators/develop-creators-title-with-unity.md">Commencer à développer un titre du programme Créateurs Xbox Live avec le moteur de jeu Unity</a></td>
    </tr>
    <tr>
        <td>Configurer votre bac à sable de développement</td>
        <td><a href="../xbox-live/get-started-with-creators/xbox-live-sandboxes-creators.md">Présentation des bacs à sable Xbox Live</a></td>
    </tr>
    <tr>
        <td>Configurer des comptes de test</td>
        <td><a href="../xbox-live/get-started-with-creators/authorize-xbox-live-accounts.md">Autoriser les comptes Xbox Live dans votre environnement de test</a></td>
    </tr>
    <tr>
        <td>Exemples pour le Programme Créateurs Xbox Live</td>
        <td><a href="https://github.com/Microsoft/xbox-live-samples/tree/master/Samples/CreatorsSDK">Exemples de code pour les développeurs du programme Créateurs</a></td>
    </tr>
    <tr>
        <td>Découvrez comment intégrer des expériences Xbox Live multiplateforme dans les jeux UWP (vidéo)</td>
        <td><a href="https://channel9.msdn.com/Events/GDC/GDC-2017/GDC2017-005">Programme Créateurs Xbox Live</a></td>
    </tr>  
</table>

### <a name="for-managed-partners-and-developers-in-the-idxbox-program"></a>Pour les partenaires et les développeurs gérés dans le programme ID@Xbox

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Vue d’ensemble</td>
        <td><a href="../xbox-live/get-started-with-partner/get-started-with-xbox-live-partner.md">Prise en main de Xbox Live en tant que partenaire géré ou développeur d’ID</a></td>
    </tr>
    <tr>
        <td>Ajouter Xbox Live à votre jeu</td>
        <td><a href="../xbox-live/get-started-with-partner/partners-step-by-step-guide.md">Guide détaillé pour l’intégration de Xbox Live pour les partenaires gérés et les membres d’ID</a></td>
    </tr>
    <tr>
        <td>Ajouter Xbox Live à votre jeu UWP créé à l’aide d’Unity</td>
        <td><a href="../xbox-live/get-started-with-partner/partner-unity-uwp-il2cpp.md">Ajouter la prise en charge Xbox Live à Unity pour UWP avec le serveur principal de script IL2CPP pour les partenaires gérés et ID</a></td>
    </tr>
    <tr>
        <td>Configurer votre bac à sable de développement</td>
        <td><a href="../xbox-live/get-started-with-partner/advanced-xbox-live-sandboxes.md">Bacs à sable Xbox Live avancés</a></td>
    </tr>
    <tr>
        <td>Configuration requise pour les jeux utilisant Xbox Live (GDN)</td>
        <td><a href="http://go.microsoft.com/fwlink/?LinkId=533217">Configuration Xbox requise pour Xbox Live sur Windows10</a></td>
    </tr>
    <tr>
        <td>Exemples</td>
        <td><a href="https://github.com/Microsoft/xbox-live-samples/tree/master/Samples/ID%40XboxSDK">Exemples de code pour les développeurs ID@Xbox</a></td>
    </tr>  
    <tr>
        <td>Vue d’ensemble du développement de jeux Xbox Live (vidéo)</td>
        <td><a href="http://channel9.msdn.com/Events/GDC/GDC-2015/Developing-with-Xbox-Live-for-Windows-10">Développement avec Xbox Live pour Windows10</a></td>
    </tr>
    <tr>
        <td>Matchmaking multiplateforme (vidéo)</td>
        <td><a href="http://channel9.msdn.com/Events/GDC/GDC-2015/Xbox-Live-Multiplayer-Introducing-services-for-cross-platform-matchmaking-and-gameplay">Xbox Live en multijoueur : Présentation des services de matchmaking et de jeu multiplateforme</a></td>
    </tr>
    <tr>
        <td>Jeu multiplateforme dans Fable Legends (vidéo)</td>
        <td><a href="http://channel9.msdn.com/Events/GDC/GDC-2015/Fable-Legends-Cross-device-Gameplay-with-Xbox-Live">Fable Legends : Jeu multiplateforme avec Xbox Live</a></td>
    </tr>
    <tr>
        <td>Xbox Live : Statistiques et succès (vidéo)</td>
        <td><a href="http://channel9.msdn.com/Events/GDC/GDC-2015/Best-Practices-for-Leveraging-Cloud-Based-User-Stats-and-Achievements-in-Xbox-Live">Meilleures pratiques pour tirer parti des statistiques et des succès des utilisateurs basés sur le cloud dans Xbox Live</a></td>
    </tr>
</table>


## <a name="additional-resources"></a>Ressources supplémentaires

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Vidéos de développement de jeux</td>
        <td><a href="https://docs.microsoft.com/windows/uwp/gaming/game-development-videos">Vidéos issues de conférences majeures telles que le GDC et //build</a></td>
    </tr>
    <tr>
        <td>Développement de jeux indépendants (vidéo)</td>
        <td><a href="http://channel9.msdn.com/Events/GDC/GDC-2015/New-Opportunities-for-Independent-Developers">De nouvelles opportunités pour les développeurs indépendants</a></td>
    </tr>
    <tr>
        <td>Considérations pour les appareils mobiles multicœurs (vidéo)</td>
        <td><a href="http://channel9.msdn.com/Events/GDC/GDC-2015/Sustained-gaming-performance-in-multi-core-mobile-devices">Performances de jeu soutenues sur les appareils mobiles multicœurs</a></td>
    </tr>
    <tr>
        <td>Développement de jeux de bureau Windows10 (vidéo)</td>
        <td><a href="http://channel9.msdn.com/Events/GDC/GDC-2015/PC-Games-for-Windows-10">Jeux pour PC Windows10</a></td>
    </tr>
</table>



 

 

 
