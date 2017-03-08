---
author: mtoepke
title: "Guide de développement de jeux Windows 10"
description: "Guide complet sur les ressources et les informations nécessaires au développement de jeux de plateforme Windows universelle (UWP)."
ms.assetid: 6061F498-96A8-44EF-9711-68AE5A1218C9
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, jeux, développement de jeux"
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: a8b28295a21d020d41f7416dd039a9a4003fb9b2
ms.lasthandoff: 02/07/2017

---

# <a name="windows-10-game-development-guide"></a>Guide de développement de jeux Windows 10


Bienvenue dans le guide de développement de jeux Windows 10 !

Ce guide regroupe les ressources et les informations dont vous avez besoin pour développer un jeu UWP.

## <a name="introduction-to-game-development-for-the-universal-windows-platform-uwp"></a>Présentation du développement de jeux pour la plateforme Windows universelle (UWP)


Lorsque vous créez un jeu Windows 10, vous pouvez toucher des millions de joueurs dans le monde sur leur téléphone, leur PC et leur Xbox One. Avec Xbox sur Windows, Xbox Live, des jeux multijoueurs multiplateforme, une incroyable communauté de joueurs, ainsi que des nouvelles fonctionnalités puissantes comme les jeux de plateforme Windows universelle (UWP) et DirectX 12, les jeux Windows 10 enchantent les joueurs de tous les âges et de tous les genres. La nouvelle plateforme Windows universelle (UWP) garantit la compatibilité de votre jeu sur les appareils Windows 10 avec une API commune pour le téléphone, le PC et Xbox One, et équipés des outils et des options permettant d’adapter votre jeu à chaque performance de l’appareil.

Ce guide fournit une collection complète des informations et des ressources qui vous aideront lors du développement de votre jeu. Les sections sont organisées en fonction des étapes de développement du jeu. Vous savez donc où rechercher les informations lorsque vous en avez besoin.

Pour commencer, la section [Ressources de développement de jeux](#game-development-resources) indique une vue d’ensemble de la documentation, des programmes et des autres ressources qui sont utiles lors de la création d’un jeu.

Ce guide sera mis à jour lorsque des ressources et des documents relatifs au développement de jeux Windows 10 seront disponibles.

## <a name="game-development-resources"></a>Ressources de développement de jeux

De la documentation aux programmes de développement, en passant par les forums, les blogs et les exemples, de nombreuses ressources sont disponibles pour vous aider à développer des jeux. Voici un résumé des ressources à connaître lorsque vous commencez à développer votre jeu Windows 10.

> 
**Remarque**   Le développement de Xbox One et certaines fonctionnalités de jeux Windows 10 (les services Xbox Live, par exemple) sont gérés par le biais de programmes tels qu’ID@Xbox et Microsoft Studios. Comme ce guide couvre une large gamme de ressources, vous pouvez donc constater que certaines ressources ne sont pas accessibles selon le programme que vous utilisez ou votre rôle de développement. Les exemples sont les liens developer.xboxlive.com, forums.xboxlive.com, xdi.xboxlive.com ou réseau GDN (Game Developer Network). Pour plus d’informations sur le partenariat avec Microsoft, voir [Programmes pour développeurs](#developer-programs).

### <a name="game-development-documentation"></a>Documentation sur le développement de jeux

Tout au long de ce guide, vous trouverez des liens ciblés vers la documentation appropriée, organisés par tâche, technologie et étape du développement du jeu. Pour vous donner une vue d’ensemble de ce qui est disponible, voici les principaux portails de documentation destinés au développement de jeux Windows 10.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Portail principal du Centre de développement Windows</td>
        <td>[Centre de développement Windows](https://dev.windows.com)</td>
    </tr>
    <tr>
        <td>Développement des applications Windows</td>
        <td>[Développement d’applications Windows](https://dev.windows.com/develop)</td>
    </tr>
    <tr>
        <td>Développement d’une application de plateforme universelle Windows</td>
        <td>[Guides de procédure pour les applications Windows 10](https://msdn.microsoft.com/library/windows/apps/mt244352)</td>
    </tr>
    <tr>
        <td>Guides de procédure pour les jeux UWP</td>
        <td>[Jeux et DirectX](index.md) </td>
    </tr>
    <tr>
        <td>Présentations et informations de référence sur DirectX</td>
        <td>[Jeux et graphiques DirectX](https://msdn.microsoft.com/library/windows/desktop/ee663274)</td>
    </tr>
    <tr>
        <td>Azure pour les jeux</td>
        <td>[Utiliser Azure pour développer et faire évoluer vos jeux](https://azure.microsoft.com/solutions/gaming/)</td>
    </tr>
    <tr>
        <td>UWP sur Xbox One</td>
        <td>[Création d’applications UWP sur Xbox One](https://msdn.microsoft.com/windows/uwp/xbox-apps/index)</td>
    </tr>
    <tr>
        <td>Documentation Xbox Live</td>
        <td>[Kit de développement logiciel (SDK) Xbox Live](http://aka.ms/xsapi2)</td>
    </tr>
    <tr>
        <td>Documentation pour développeurs Xbox One (GDN)</td>
        <td>[Documentation du XDK Xbox One](https://developer.xboxlive.com/en-us/platform/development/documentation/Pages/home.aspx)</td>
    </tr>
    <tr>
        <td>Livres blancs pour développeurs Xbox One (GDN)</td>
        <td>[Livres blancs](https://developer.xboxlive.com/en-us/platform/development/education/Pages/WhitePapers.aspx)</td>
    </tr>     
</table>

### <a name="developer-programs"></a>Programmes pour développeurs

Microsoft propose plusieurs programmes pour développeurs pour vous aider à développer et à publier des jeux Windows. Pour publier un jeu dans le Windows Store, vous devez créer un compte de développeur dans le Centre de développement Windows. Les autres programmes peuvent être intéressants selon les besoins de votre jeu et de votre studio, et peuvent créer des opportunités comme le développement Xbox One et l’intégration Xbox Live.

#### <a name="windows-dev-center"></a>Centre de développement Windows

L’inscription d’un compte de développeur dans le Centre de développement Windows est la première étape de la publication du jeu Windows. Un compte de développeur vous permet de réserver le nom de votre jeu et de soumettre des jeux gratuits ou payants au Windows Store pour tous les appareils Windows. Utilisez votre compte de développeur pour gérer votre jeu et les produits intégrés au jeu, obtenir des analyses détaillées et activer des services qui créent des expériences exceptionnelles pour vos joueurs dans le monde entier.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Inscrire un compte de développeur</td>
        <td>[Êtes-vous prêt à vous inscrire ?](https://msdn.microsoft.com/library/windows/apps/bg124287)</td>
    </tr> 
</table>

#### <a name="idxbox"></a>ID@Xbox

Le programme ID@Xbox permet aux développeurs de jeux qualifiés de publier eux-mêmes sur Windows et Xbox One. Si vous voulez développer des jeux pour Xbox One ou ajouter des fonctionnalités Xbox Live telles que les scores des joueurs, les succès et les classements à votre jeu Windows 10, inscrivez-vous sur ID@Xbox. Devenez développeur ID@Xbox pour obtenir les outils et l’aide dont vous avez besoin pour laisser libre cours à votre créativité et optimiser votre succès. Avant de faire une demande sur ID@Xbox, inscrivez un compte de développeur dans le Centre de développement Windows.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Programme de développement ID@Xbox</td>
        <td>[Programme de développement indépendant pour Xbox One](http://go.microsoft.com/fwlink/p/?LinkID=526271)</td>
    </tr>
    <tr>
        <td>Site destiné aux utilisateurs d’ID@Xbox</td>
        <td>[ID@Xbox](http://www.idatxbox.com/)</td>
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

De nombreux exemples de jeu et d’application Windows 10 sont disponibles pour vous aider à comprendre les fonctionnalités de jeux de Windows 10 et à démarrer rapidement le développement de jeux. D’autres exemples sont développés et publiés régulièrement. En conséquence, n’oubliez pas de consulter de temps en temps les portails des exemples pour en voir les nouveautés. Vous pouvez également [consulter](https://help.github.com/articles/watching-repositories/) les référentiels GitHub pour être averti des modifications et des ajouts.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Exemples d’applications de plateforme universelle Windows</td>
        <td>[Windows-universal-samples](https://github.com/Microsoft/Windows-universal-samples)</td>
    </tr>
    <tr>
        <td>Exemples publics Xbox Advanced Technology Group</td>
        <td>[Xbox-ATG-Samples](https://github.com/Microsoft/Xbox-ATG-Samples)</td>
    </tr>
    <tr>
        <td>Exemples de graphiques Direct3D 12</td>
        <td>[DirectX-Graphics-Samples](https://github.com/Microsoft/DirectX-Graphics-Samples)</td>
    </tr>
    <tr>
        <td>Exemples de graphiques Direct3D 11</td>
        <td>[directx-sdk-samples](https://github.com/walbourn/directx-sdk-samples)</td>
    </tr>
    <tr>
        <td>Exemple de jeu à la première personne Direct3D 11</td>
        <td>[Créer un jeu UWP simple avec DirectX](tutorial--create-your-first-metro-style-directx-game.md)</td>
    </tr>
    <tr>
        <td>Exemple d’effets d’image personnalisés de Direct2D</td>
        <td>[D2DCustomEffects](http://go.microsoft.com/fwlink/p/?LinkId=620531)</td>
    </tr>
    <tr>
        <td>Exemple de maillage dégradé Direct2D</td>
        <td>[D2DGradientMesh](http://go.microsoft.com/fwlink/p/?LinkId=620532)</td>
    </tr>
    <tr>
        <td>Exemple d’ajustement de photo Direct2D</td>
        <td>[D2DPhotoAdjustment](http://go.microsoft.com/fwlink/p/?LinkId=620533)</td>
    </tr>
    <tr>
        <td>Exemples de jeu Xbox One (GDN)</td>
        <td>[Exemples](https://developer.xboxlive.com/en-us/platform/development/education/Pages/Samples.aspx)</td>
    </tr>
    <tr>
        <td>Exemples de jeu Windows 8 (MSDN Code Gallery)</td>
        <td>[Exemples de jeu du Windows Store](https://code.msdn.microsoft.com/windowsapps/site/search?f%5B0%5D.Type=SearchText&f%5B0%5D.Value=game&f%5B1%5D.Type=Contributors&f%5B1%5D.Value=Microsoft&f%5B1%5D.Text=Microsoft)</td>
    </tr>
    <tr>
        <td>Exemple de jeu en JavaScript et HTML5</td>
        <td>[Exemple de jeu tactile en JavaScript et HTML5](https://code.msdn.microsoft.com/windowsapps/JavaScript-and-HTML5-touch-d96f6031)</td>
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
        <td>Forums des développeurs d’applications Windows</td>
        <td>[Forums sur les applications et le Windows Store](https://social.msdn.microsoft.com/Forums/home?category=windowsapps)</td>
    </tr>
    <tr>
        <td>Forum des développeurs d’applications UWP</td>
        <td>[Développement d’applications de la plateforme Windows universelles](https://social.msdn.microsoft.com/Forums/home?forum=wpdevelop)</td>
    </tr>

    <tr>
        <td>Forums de développeurs d’applications de bureau</td>
        <td>[Forum dédié aux applications de bureau Windows](https://social.msdn.microsoft.com/Forums/home?category=windowsdesktopdev)</td>
    </tr>
    <tr>
        <td>Jeux du Windows Store DirectX (billets archivés)</td>
        <td>[Création de jeux du Windows Store en DirectX (archivés)](https://social.msdn.microsoft.com/Forums/vstudio/home?forum=wingameswithdirectx)</td>
    </tr>
    <tr>
        <td>Forums de développeurs partenaires gérés Windows 10</td>
        <td>[Forum des développeurs Xbox : Windows 10](http://aka.ms/win10devforums)</td>
    </tr>
    <tr>
        <td>Forums du programme d’accès en avant-première à DirectX</td>
        <td>[Forum de DirectX 12](http://directx12forum.azurewebsites.net/index.php)</td>
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
        <td>[Création d’applications pour Windows](http://blogs.windows.com/buildingapps/)</td>
    </tr>
    <tr>
        <td>Windows 10 (billets de blog)</td>
        <td>[Publications dans Windows 10](http://blogs.windows.com/blog/tag/windows-10/)</td>
    </tr>
    <tr>
        <td>Blog de l’équipe d’ingénierie de Visual Studio</td>
        <td>[Le blog de Visual Studio](http://blogs.msdn.com/b/visualstudio/)</td>
    </tr>
    <tr>
        <td>Blogs des outils de développement de Visual Studio</td>
        <td>[Blogs des outils de développement](http://blogs.msdn.com/b/developer-tools/)</td>
    </tr>
    <tr>
        <td>Blog des outils de développement de Somasegar</td>
        <td>[Blog de Somasegar](http://blogs.msdn.com/b/somasegar/)</td>
    </tr>
    <tr>
        <td>Blog DirectX pour les développeurs</td>
        <td>[Blog DirectX pour les développeurs](http://blogs.msdn.com/b/directx)</td>
    </tr>
    <tr>
        <td>Présentation de DirectX 12 (billet de blog)</td>
        <td>[DirectX 12](http://blogs.msdn.com/b/directx/archive/2014/03/20/directx-12.aspx)</td>
    </tr>
    <tr>
        <td>Blog de l’équipe Visual C++</td>
        <td>[Blog de l’équipe Visual C++](http://blogs.msdn.com/b/vcblog/)</td>
    </tr>
    <tr>
        <td>Blog ID@Xbox pour les développeurs</td>
        <td>[Blog ID@Xbox pour les développeurs](http://www.idatxbox.com/category/developer-blog/)</td>
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
        <td>[Technologies de jeu des applications UWP](game-development-platform-guide.md)</td>
    </tr>
</table>
 

Ces trois vidéos du GDC 2015 constituent une bonne vue d’ensemble du développement de jeux Windows 10 et de l’expérience de jeu Windows 10.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Vue d’ensemble du développement de jeux Windows 10 (vidéo)</td>
        <td>[Développement de jeux pour Windows 10](http://channel9.msdn.com/Events/GDC/GDC-2015/Developing-Games-for-Windows-10)</td>
    </tr>
    <tr>
        <td>Expérience de jeu Windows 10 (vidéo)</td>
        <td>[Expérience de consommateur de jeux sur Windows 10](http://channel9.msdn.com/Events/GDC/GDC-2015/Gaming-Consumer-Experience-on-Windows-10)</td>
    </tr>
    <tr>
        <td>Les jeux à travers l’écosystème Microsoft (vidéo)</td>
        <td>[L’avenir des jeux à travers l’écosystème Microsoft](http://channel9.msdn.com/Events/GDC/GDC-2015/The-Future-of-Gaming-Across-the-Microsoft-Ecosystem)</td>
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
        <td>[Accessibilité des jeux](https://msdn.microsoft.com/windows/uwp/gaming/accessibility-for-games)</td>
    </tr>
    <tr>
        <td>Générer des jeux à l’aide du cloud</td>
        <td>[Cloud pour les jeux](https://msdn.microsoft.com/windows/uwp/gaming/cloud-for-games)</td>
    </tr>
    <tr>
        <td>Monétiser votre jeu</td>
        <td>[Monétisation pour les jeux](https://msdn.microsoft.com/windows/uwp/gaming/monetization-for-games)</td>
    </tr>
</table>



### <a name="choosing-your-graphics-technology-and-programming-language"></a>Choix de la technologie graphique et du langage de programmation

Plusieurs langages de programmation et technologies graphiques peuvent être utilisés dans les jeux Windows 10. Votre choix dépend du type de jeu développé, de l’expérience et des préférences de votre studio de développement, et des conditions requises spécifiques pour les fonctionnalités de votre jeu. Allez-vous utiliser C#, C++ ou JavaScript ? DirectX, XAML ou HTML5 ?

#### <a name="directx"></a>DirectX

Microsoft DirectX représente le choix à faire pour obtenir des graphismes et des éléments multimédias 2D et 3D haute performances. 

Direct3D 12, nouvelle fonctionnalité de Windows 10, apporte la puissance d’une API du type console, et sa rapidité et son efficacité sont sans commune mesure. Votre jeu peut pleinement utiliser le matériel vidéo moderne et présenter plus d’objets, des scènes enrichies et des effets améliorés. Direct3D 12 fournit des graphismes optimisés sur les PC Windows 10 et sur Xbox One. Si vous souhaitez utiliser le pipeline graphique familier de Direct3D 11, vous bénéficierez cependant des nouvelles fonctionnalités de rendu et d’optimisation ajoutées à Direct3D 11.3. Et si vous êtes un développeur éprouvé d’API de bureau pour Windows utilisant Win32, vous avez toujours cette option dans Windows 10.

Les fonctionnalités complètes et la solide intégration à la plateforme de DirectX fournissent la puissance et les performances nécessaires aux jeux les plus exigeants.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Guides de procédure pour jeux DirectX</td>
        <td>[Jeux et DirectX](index.md)</td>
    </tr>
    <tr>
        <td>Présentations et informations de référence sur DirectX</td>
        <td>[Jeux et graphismes DirectX](https://msdn.microsoft.com/library/windows/desktop/ee663274)</td>
    </tr>
    <tr>
        <td>Direct3D 12 : Guide de programmation et informations de référence</td>
        <td>[Graphismes Direct3D 12](https://msdn.microsoft.com/library/windows/desktop/dn903821)</td>
    </tr>
    <tr>
        <td>Vidéos de développement Graphics et DirectX 12 (YouTube)</td>
        <td>[Microsoft DirectX 12 et Graphics Education](https://www.youtube.com/channel/UCiaX2B8XiXR70jaN7NK-FpA)</td>
    </tr>
</table>
 

#### <a name="xaml"></a>XAML

XAML est un langage d’interface utilisateur déclaratif convivial doté de fonctionnalités pratiques comme les animations, les tables de montage séquentiel, la liaison de données, le format SVG (Scalable Vector Graphics), le redimensionnement dynamique et les graphes de scène. XAML fonctionne parfaitement pour l’interface utilisateur, les menus, les sprites et les graphiques 2D des jeux. Pour simplifier la disposition de l’interface utilisateur, le langage XAML est compatible avec les outils de conception et de développement que sont Expression Blend et Microsoft Visual Studio. XAML est couramment utilisé avec C#, mais C++ convient également si c’est votre langage préféré ou si les exigences d’unité centrale de votre jeu sont élevées.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Vue d’ensemble de la plateforme XAML</td>
        <td>[Plateforme XAML](https://msdn.microsoft.com/library/windows/apps/mt228259)</td>
    </tr>
    <tr>
        <td>Interface utilisateur et contrôles XAML</td>
        <td>[Contrôles, dispositions et texte](https://msdn.microsoft.com/library/windows/apps/mt228348)</td>
    </tr>
</table>
 

#### <a name="html-5"></a>HTML 5

Le langage HTML (HyperText Markup Language) est un langage de balisage d’interface utilisateur couramment utilisé pour les pages web, les applications et les clients enrichis. Les jeux Windows peuvent utiliser le langage HTML5 comme couche présentation complète avec les fonctionnalités habituelles du HTML, l’accès à la plateforme Universal Windows Platform (UWP) et la prise en charge de fonctionnalités web modernes comme AppCache, les traitements web, le canevas, le glisser-déplacer, la programmation asynchrone et le format SVG. En arrière-plan, le rendu HTML tire parti de la puissance de l’accélération matérielle de DirectX. Vous bénéficiez donc toujours de l’avantage des performances de DirectX sans écrire de code supplémentaire. HTML5 convient bien si vous maîtrisez le développement web, le portage d’un jeu web ou si vous souhaitez utiliser des couches de langage et de graphiques dont l’approche est plus simple que les autres choix. Le langage HTML5 est utilisé avec JavaScript, mais il peut être également appelé dans les composants créés en C# ou C++/CX.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Informations sur HTML5 et DOM</td>
        <td>[Informations de référence HTML et DOM](https://msdn.microsoft.com/library/windows/apps/br212882.aspx)</td>
    </tr>
    <tr>
        <td>Recommandation du W3C sur HTML5</td>
        <td>[HTML5](http://go.microsoft.com/fwlink/p/?linkid=221374)</td>
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
        <td>[DXGI](https://msdn.microsoft.com/library/windows/desktop/hh404534)</td>
    </tr>
    <tr>
        <td>Combinaison de DirectX et XAML</td>
        <td>[Technologie interop DirectX et XAML](directx-and-xaml-interop.md)</td>
    </tr>
</table>
 

#### <a name="c"></a>C++

C++/CX est un langage haute performance à faible traitement, qui fournit une puissante combinaison de vitesse, compatibilité et accès aux plateformes. C++/CX facilite l’utilisation de l’ensemble des fonctionnalités de jeux remarquables de Windows 10, notamment DirectX et Xbox Live. Vous pouvez également réutiliser le code et les bibliothèques C++ existants. C++/CX crée un code rapide et natif qui n’implique pas la surcharge de nettoyage de la mémoire. Votre jeu peut donc bénéficier de performances exceptionnelles et d’une faible consommation d’énergie, ce qui conduit à une autonomie de la batterie plus longue. Utilisez C++/CX avec DirectX ou XAML, ou bien créez un jeu utilisant une combinaison des deux.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Présentations et informations de référence sur C++/CX</td>
        <td>[Informations de référence en matière de langage Visual C++ (C++/CX)](https://msdn.microsoft.com/library/windows/apps/hh699871.aspx)</td>
    </tr>
    <tr>
        <td>Visual C++ : Guide de programmation et informations de référence</td>
        <td>[Visual C++ dans Visual Studio 2015](https://msdn.microsoft.com/library/60k1461a.aspx)</td>
    </tr>
</table>
 

#### <a name="c"></a>C#

C# (prononcez « C sharp ») est un langage moderne et innovant, qui est simple, puissant, de type sécurisé et orienté objet. C# permet un développement rapide tout en conservant la familiarité et l’expressivité des langages du style C. Même s’il est facile à utiliser, C# possède de nombreuses fonctionnalités de langage avancées comme le polymorphisme, les délégués, les expressions lambda, les fermetures, la méthode Iterator, la covariance et les expressions LINQ (Language-Integrated Query). C# convient parfaitement si vous ciblez XAML, souhaitez commencer à développer rapidement votre jeu ou bénéficiez déjà d’une expérience en C#. C# est utilisé essentiellement avec XAML. Si vous voulez utiliser DirectX, choisissez plutôt C++ ou écrivez une partie de votre jeu en tant que composant C++ qui interagit avec DirectX. Pensez également à [Win2D](https://github.com/Microsoft/Win2D), une bibliothèque de graphismes Direct2D en mode immédiat pour C# et C++.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>C# : Guide de programmation et informations de référence</td>
        <td>[Informations de référence sur le langage C#](https://msdn.microsoft.com/library/kx37x362.aspx)</td>
    </tr>
</table>
 

#### <a name="javascript"></a>JavaScript

JavaScript est un langage de script dynamique largement utilisé pour les applications web modernes et les applications clientes enrichies.

Les applications Windows app en JavaScript peuvent accéder aux puissantes fonctionnalités de la plateforme Universal Windows Platform (UWP) d’une façon simple et intuitive, comme les méthodes et les propriétés des classes JavaScript orientées objet. JavaScript convient bien à votre jeu si vous venez d’un environnement de développement web, si vous maîtrisez déjà ce langage ou si vous souhaitez utiliser les bibliothèques HTML5, CSS, WinJS ou JavaScript. Si vous ciblez DirectX ou XAML, préférez C# ou C++/CX.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Informations de référence sur JavaScript et Windows Runtime</td>
        <td>[Informations de référence JavaScript](https://msdn.microsoft.com/library/windows/apps/jj613794)</td>
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
        <td>[Création de composants Windows Runtime](https://msdn.microsoft.com/library/windows/apps/hh441572.aspx)</td>
    </tr>
</table>


### <a name="which-version-of-directx-should-your-game-use"></a>Quelle version de DirectX utiliser dans votre jeu ?

Si vous choisissez DirectX pour votre jeu, vous devez déterminer la version à utiliser : Microsoft Direct3D 12 ou Microsoft Direct3D 11.

Direct3D 12, nouvelle fonctionnalité de Windows 10, apporte la puissance d’une API du type console, et sa rapidité et son efficacité sont sans commune mesure. Votre jeu peut pleinement utiliser le matériel vidéo moderne et présenter plus d’objets, des scènes enrichies et des effets améliorés. Direct3D 12 fournit des graphismes optimisés sur les PC Windows 10 et sur Xbox One. Étant donné que Direct3D 12 fonctionne à un niveau très faible, il donne aux équipes de développement de graphiques expertes, ou aux équipes de développement de DirectX 11 expérimentées, les moyens de maximiser l’optimisation des graphiques.

Direct3D 11.3 est une API graphique de niveau faible, qui utilise le modèle de programmation Direct3D familier, et prend plus facilement en charge le processus complexe de rendu GPU. Elle est également prise en charge dans Windows 10 et Xbox One. Si vous disposez d’un moteur existant écrit en Direct3D 11 et que vous n’êtes pas encore prêt à effectuer la transition vers Direct3D 12, vous pouvez utiliser Direct3D 11 sur 12 pour obtenir certaines améliorations des performances. Les versions 11.3 et ultérieures contiennent également les nouvelles fonctionnalités de rendu et d’optimisation présentes dans Direct3D 12.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Choisir Direct3D 12 ou Direct3D 11</td>
        <td>[Qu’est-ce que Direct3D 12?](https://msdn.microsoft.com/library/windows/desktop/dn899228)</td>
    </tr>
    <tr>
        <td>Vue d’ensemble de Direct3D 11</td>
        <td>[Graphismes Direct3D 11](https://msdn.microsoft.com/library/windows/desktop/ff476080)</td>
    </tr>
    <tr>
        <td>Vue d’ensemble de Direct3D 11 sur 12</td>
        <td>[Direct3D 11 sur 12](https://msdn.microsoft.com/library/windows/desktop/dn913195)</td>
    </tr>
</table>


### <a name="bridges-game-engines-and-middleware"></a>Ponts, moteurs de jeu et intergiciels

En fonction des besoins de votre jeu, l’utilisation de ponts, de moteurs de jeu ou d’intergiciels peut économiser du temps et des ressources de développement et de test. Voici une vue d’ensemble et les ressources des ponts, moteurs de jeu et intergiciels qui vous aideront à déterminer ce qui est fait pour vous.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Ponts et moteurs de jeu pour Windows 10 (billet de blog)</td>
        <td>[Autres moyens d’importer du code dans les applications Windows 10 du Windows Store à croissance rapide](http://blogs.windows.com/buildingapps/2015/09/17/more-ways-to-bring-your-code-to-fast-growing-windows-10-store/)</td>
    </tr>
    <tr>
        <td>Développement de jeux avec des intergiciels (vidéo)</td>
        <td>[Accélération du développement des jeux du Windows Store avec des intergiciels](https://channel9.msdn.com/Events/Build/2013/3-187)</td>
    </tr>
    <tr>
        <td>Visual Studio avec Unity, Unreal et Cocos2d (billet de blog)</td>
        <td>[Visual Studio pour le développement de jeux : Nouveaux partenariats avec Unity, Unreal Engine et Cocos2d](http://blogs.msdn.com/b/somasegar/archive/2015/04/17/visual-studio-for-game-development-new-partnerships-with-unity-unreal-engine-and-cocos2d.aspx)</td>
    </tr>
    <tr>
        <td>Introduction aux intergiciels de jeu (billet de blog)</td>
        <td>[Intergiciels pour le développement des jeux. De quoi s’agit-il ? En ai-je besoin ?](http://blogs.msdn.com/b/wsdevsol/archive/2014/05/02/game-development-middleware-what-is-it-do-i-need-it.aspx)</td>
    </tr>
</table>
 

#### <a name="universal-windows-platform-bridges"></a>Ponts de plateforme Windows universelle

Les ponts de plateforme Windows universelle sont des technologies qui amènent votre application ou votre jeu existant à la plateforme UWP. Ils sont un excellent moyen de démarrer rapidement le développement des jeux UWP.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Ponts UWP</td>
        <td>[Importer votre code dans Windows](https://dev.windows.com/bridges/)</td>
    </tr>
    <tr>
        <td>Pont Windows pour iOS</td>
        <td>[Porter vos applications iOS vers Windows](https://dev.windows.com/bridges/ios)</td>
    </tr>
    <tr>
        <td>Pont Windows pour les applications de bureau (.NET et Win32)</td>
        <td>[Convertir votre application de bureau en application UWP](https://developer.microsoft.com/windows/bridges/desktop)</td>
    </tr>
</table>
 

#### <a name="unity"></a>Unity

Unity 5 est la nouvelle génération de la plateforme de développement maintes fois primée pour la création de jeux 2D et 3D et des expériences interactives. Unity 5 apporte une nouvelle puissance artistique, des fonctionnalités graphiques améliorées et une efficacité renforcée.

D’après le site [Unity roadmap](https://unity3d.com/unity/roadmap), la prise en charge de DirectX 12 sera assurée dans une prochaine version de Unity.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Moteur de jeu Unity</td>
        <td>[Unity : Moteur de jeu](http://unity3d.com/)</td>
    </tr>
    <tr>
        <td>Obtenir Unity 5</td>
        <td>[Obtenir Unity](http://unity3d.com/get-unity)</td>
    </tr>
    <tr>
        <td>Prise en charge des applications Windows universelles dans Unity 5.2 (billet de blog)</td>
        <td>[Applications de la plateforme Windows 10 universelle dans Unity 5.2](http://blogs.unity3d.com/2015/09/09/windows-10-universal-apps-in-unity-5-2/)</td>
    </tr>
    <tr>
        <td>Documentation Unity pour Windows</td>
        <td>[Manuel Unity/Windows](http://docs.unity3d.com/Manual/Windows.html)</td>
    </tr>
    <tr>
        <td>Publier votre jeu Unity dans le Windows Store</td>
        <td>[Guides de portage](https://unity3d.com/partners/microsoft/porting-guides)</td>
    </tr>
    <tr>
        <td>Publier votre jeu Unity en tant qu’application UWP (vidéo)</td>
        <td>[Comment publier votre jeu Unity en tant qu’application UWP](https://channel9.msdn.com/Blogs/One-Dev-Minute/How-to-publish-your-Unity-game-as-a-UWP-app)</td>
    </tr>
    <tr>
        <td>Utiliser Unity pour créer des jeux et applications Windows (vidéo)</td>
        <td>[Création de jeux et applications Windows avec Unity](https://channel9.msdn.com/Blogs/One-Dev-Minute/Making-games-and-apps-with-Unity)</td>
    </tr>
    <tr>
        <td>Développement de jeux Unity à l’aide de Visual Studio (série de vidéos)</td>
        <td>[Utilisation d’Unity avec Visual Studio 2015](http://go.microsoft.com/fwlink/?LinkId=722359)</td>
    </tr>
</table>
 

#### <a name="havok"></a>Havok

Avec la suite modulaire d’outils et de technologies de Havok, les créateurs de jeux peuvent atteindre de nouveaux niveaux d’interactivité et d’immersion. Havok permet de fournir des données physiques réalistes et d’effectuer des simulations interactives, ainsi que de remarquables animations. La version 2015.1 et les versions ultérieures prennent officiellement en charge UWP dans Visual Studio 2015 sur x86, 64 bits et ARM.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Site web Havok</td>
        <td>[Havok](http://www.havok.com/)</td>
    </tr>
    <tr>
        <td>Suite d’outils Havok</td>
        <td>[Vue d’ensemble des produits Havok](http://www.havok.com/products/)</td>
    </tr>
    <tr>
        <td>Forums de support Havok</td>
        <td>[Havok](http://support.havok.com)</td>
    </tr>
</table>
 

#### <a name="monogame"></a>MonoGame

MonoGame est une infrastructure de développement open source inter-plateforme initialement basée sur Microsoft XNA Framework 4.0. Monogame prend actuellement en charge Windows, Windows Phone, Xbox, ainsi que Linux, Mac OS, iOS, Android et certaines autres plateformes.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>MonoGame</td>
        <td>[Site web de MonoGame](http://www.monogame.net)</td>
    </tr>
    <tr>
        <td>Documentation MonoGame</td>
        <td>[Documentation MonoGame (version la plus récente)](http://www.monogame.net/documentation/)</td>
    </tr>
    <tr>
        <td>Téléchargements MonoGame</td>
        <td>[Téléchargez des versions, des builds de développement et du code source](http://www.monogame.net/downloads/) sur le site web de MonoGame, ou [obtenez la version la plus récente via NuGet](https://www.nuget.org/profiles/MonoGame).
    </tr>
</table>


#### <a name="cocos2d"></a>Cocos2d

Cocos2d-X est une suite d’outils et de moteurs de jeu open source multiplateforme qui prend en charge la création de jeux UWP. Depuis la version 3, des fonctionnalités 3D sont également ajoutées.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Cocos2d-x</td>
        <td>[Qu’est-ce que Cocos2d-x ?](http://www.cocos2d-x.org/)</td>
    </tr>
    <tr>
        <td>Guide du programmeur Cocos2d-x</td>
        <td>[Guide du programmeur Cocos2d-x v3.8](http://www.cocos2d-x.org/programmersguide/)</td>
    </tr>
    <tr>
        <td>Cocos2d-x sur Windows 10 (billet de blog)</td>
        <td>[Exécution de Cocos2d-x sur Windows 10](https://blogs.windows.com/buildingapps/2015/06/15/running-cocos2d-x-on-windows-10/)</td>
    </tr>
    <tr>
        <td>Jeux du Windows Store avec Cocos2d-x (vidéo)</td>
        <td>[Créer un jeu avec Cocos2d-x pour les appareils Windows](http://www.microsoftvirtualacademy.com/training-courses/build-a-game-with-cocos2d-x-for-windows-devices)</td>
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
        <td>[Unreal Engine 4](https://www.unrealengine.com/what-is-unreal-engine-4)</td>
    </tr>
</table>

#### <a name="babylonjs"></a>BabylonJS

BabylonJS est une infrastructure JavaScript complète destinée à créer des jeux en 3D avec HTML5, WebGL et Web Audio.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>BabylonJS</td>
        <td>[BabylonJS](http://www.babylonjs.com/)</td>
    </tr>
    <tr>
        <td>WebGL 3D avec HTML5 et BabylonJS (série de vidéos)</td>
        <td>[Découverte de 3D WebGL et BabylonJS](https://channel9.msdn.com/Series/Introduction-to-WebGL-3D-with-HTML5-and-Babylonjs/01)</td>
    </tr>
    <tr>
        <td>Création d’un jeu WebGL multiplateforme avec BabylonJS</td>
        <td>[Utiliser BabylonJS pour développer un jeu multiplateforme](https://www.smashingmagazine.com/2016/07/babylon-js-building-sponza-a-cross-platform-webgl-game/)</td>
    </tr>    
</table>

### <a name="middleware-and-partners"></a>Intergiciels (middleware) et partenaires

De nombreux autres partenaires proposant des intergiciels et des moteurs qui peuvent fournir des solutions en fonction de vos besoins de développement de jeux.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Partenaires du Centre de développement Windows</td>
        <td>[Partenaires du Centre de développement](https://developer.microsoft.com/windows/app-middleware-partners)</td>
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
        <td>Portage d’une application Windows 8 vers une application de plateforme Windows universelle</td>
        <td>[Passer de Windows Runtime 8.x à UWP](https://msdn.microsoft.com/library/windows/apps/mt238322)</td>
    </tr>
    <tr>
        <td>Portage d’une application Windows 8 vers une application de plateforme Windows universelle (vidéo)</td>
        <td>[Portage d’applications Windows 8.1 vers Windows 10](https://channel9.msdn.com/Series/A-Developers-Guide-to-Windows-10/21)</td>
    </tr>
    <tr>
        <td>Portage d’une application iOS vers une application de plateforme Windows universelle (vidéo)</td>
        <td>[Migrer d’iOS vers UWP](https://msdn.microsoft.com/library/windows/apps/mt238320)</td>
    </tr>
    <tr>
        <td>Portage d’une application Silverlight vers une application de plateforme Windows universelle</td>
        <td>[Passer de Silverlight pour Windows Phone à UWP](https://msdn.microsoft.com/library/windows/apps/mt238323)</td>
    </tr>
    <tr>
        <td>Portage d’une application Silverlight ou XAML vers une application de plateforme Windows universelle (vidéo)</td>
        <td>[Portage d’une application XAML ou Silverlight vers Windows 10](https://channel9.msdn.com/Events/Build/2015/3-741)</td>
    </tr>
    <tr>
        <td>Portage d’une application Xbox vers une application de plateforme Windows universelle</td>
        <td>[Portage de Xbox One vers Windows 10 UWP](https://developer.xboxlive.com/en-us/platform/development/education/Documents/Porting%20from%20Xbox%20One%20to%20Windows%2010.aspx)</td>
    </tr>
    <tr>
        <td>Portage de DirectX 9 vers DirectX 11</td>
        <td>[Porter de DirectX 9 vers la plateforme Windows universelle (UWP)](porting-your-directx-9-game-to-windows-store.md)</td>
    </tr>
    <tr>
        <td>Portage de Direct3D 11 vers Direct3D 12</td>
        <td>[Portage de Direct3D 11 vers Direct3D 12](https://msdn.microsoft.com/library/windows/desktop/mt431709)</td>
    </tr>
    <tr>
        <td>Portage d’OpenGL ES vers Direct3D 11</td>
        <td>[Passer d’OpenGL ES 2.0 à Direct3D 11](port-from-opengl-es-2-0-to-directx-11-1.md)</td>
    </tr>
    <tr>
        <td>Passer d’OpenGL ES à Direct3D 11 en utilisant ANGLE</td>
        <td>[ANGLE](http://go.microsoft.com/fwlink/p/?linkid=618387)</td>
    </tr>
    <tr>
        <td>Équivalents des API Windows classiques dans UWP</td>
        <td>[Alternatives aux API Windows dans les applications de plateforme Windows universelle (UWP)](https://msdn.microsoft.com/library/windows/apps/hh464945)</td>
    </tr>
</table>


## <a name="prototype-and-design"></a>Prototype et conception


Maintenant que vous avez choisi le type de jeu à créer et les outils et la technologie graphique que vous allez utiliser pour ce faire, vous êtes prêt à passer à sa conception et à la création de son prototype. Comme le cœur de votre jeu est une application deplateforme Windows universelle, c’est par là que vous allez commencer.

### <a name="introduction-to-the-universal-windows-platform-uwp"></a>Présentation de la plateforme Windows universelle (UWP)

Windows 10 introduit la plateforme Windows universelle (UWP), qui fournit une plateforme des API courantes des appareils Windows 10. UWP évolue et développe le modèle Windows Runtime pour le perfectionner et le transformer en noyau cohérent et unifié. Les jeux qui ciblent la plateforme UWP peuvent appeler les API WinRT qui sont communes à tous les appareils. Comme la plateforme UWP fournit une couche API système garantie, vous pouvez choisir de créer un package d’application unique qui s’installera sur les appareils Windows 10. Et si vous le souhaitez, votre jeu peut toujours appeler les API (y compris des API Windows classiques de Win32 et .NET) propres aux appareils sur lesquels votre jeu s’exécute.

L’objectif de la plateforme UWP est d’avoir :

-   Un système d’exploitation principal
-   Une plateforme d’application
-   Un réseau social de jeux
-   Un magasin
-   Un chemin d’accès d’intégration

Les guides indiqués ci-dessous sont excellents. Ils décrivent en détail les applications de plateforme Windows universelle, et il est vivement recommandé de les lire pour mieux comprendre ce qu’est cette plateforme.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Présentation des applications de plateforme Windows universelle</td>
        <td>[Qu’est-ce qu’une application de plateforme universelle Windows ?](https://msdn.microsoft.com/library/windows/apps/dn726767)</td>
    </tr>
    <tr>
        <td>Vue d’ensemble de la plateforme UWP</td>
        <td>[Guide des applications UWP](https://msdn.microsoft.com/library/windows/apps/dn894631)</td>
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
        <td>[Prise en main des applications Windows](https://dev.windows.com/getstarted)</td>
    </tr>
    <tr>
        <td>Préparation au développement UWP</td>
        <td>[Préparation](https://msdn.microsoft.com/library/windows/apps/dn726766)</td>
    </tr>
</table>

Si vous ne connaissez pas du tout la programmation UWP et que vous envisagez d’utiliser XAML dans votre jeu (voir [Choix de la technologie graphique et du langage de programmation](#choosing-your-graphics-technology-and-programming-language)), la série de vidéos [Développement sur Windows 10 pour les néophytes](https://channel9.msdn.com/Series/Windows-10-development-for-absolute-beginners) est idéale pour commencer.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Guide des débutants pour le développement pour Windows 10 avec le langage XAML (série de vidéos)</td>
        <td>[Développement sur Windows 10 pour les néophytes](https://channel9.msdn.com/Series/Windows-10-development-for-absolute-beginners)</td>
    </tr>
    <tr>
        <td>Annonce de la série sur Windows 10 pour néophytes utilisant XAML (billet de blog)</td>
        <td>[Développement sur Windows 10 pour les néophytes](http://blogs.windows.com/buildingapps/2015/09/30/windows-10-development-for-absolute-beginners/)</td>
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
        <td>[Développement d’applications Windows](https://dev.windows.com/develop)</td>
    </tr>
    <tr>
        <td>Vue d’ensemble de la programmation réseau dans UWP</td>
        <td>[Mise en réseau et services web](https://msdn.microsoft.com/library/windows/apps/mt280378)</td>
    </tr>
    <tr>
        <td>Utilisation de Windows.Web.HTTP et Windows.Networking.Sockets dans les jeux</td>
        <td>[Mise en réseau pour les jeux](work-with-networking-in-your-directx-game.md)</td>
    </tr>
    <tr>
        <td>Concepts de programmation asynchrone dans UWP</td>
        <td>[Programmation asynchrone](https://msdn.microsoft.com/library/windows/apps/mt187335)</td>
    </tr>
</table>

### <a name="windows-desktop-apis-to-uwp"></a>API Windows Desktop vers UWP

Voici quelques liens pour vous aider à faire migrer votre jeu de bureau Windows vers UWP.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>API UWP pour les API Win32 et COM</td>
        <td>[API Win32 et COM pour les applications UWP](https://msdn.microsoft.com/library/windows/apps/mt592904.aspx)</td>
    </tr>
    <tr>
        <td>Fonctions CRT non prises en charge dans UWP</td>
        <td>[Fonctions CRT non prises en charge dans les applications de la plateforme Windows universelle](https://msdn.microsoft.com/library/windows/apps/jj606124.aspx)</td>
    </tr>
    <tr>
        <td>Alternatives aux API Windows</td>
        <td>[Alternatives aux API Windows dans les applications de plateforme Windows universelle (UWP)](https://msdn.microsoft.com/library/windows/apps/mt592894.aspx)</td>
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
        <td>[Cycle de vie de l’application](https://msdn.microsoft.com/library/windows/apps/mt243287)</td>
    </tr>
    <tr>
        <td>Utilisation de Microsoft Visual Studio pour déclencher des transitions d’application</td>
        <td>[Comment déclencher des événements de suspension, de reprise et en arrière-plan pour des applications du Windows Store dans Visual Studio](https://msdn.microsoft.com/library/hh974425.aspx)</td>
    </tr>
</table>
 

### <a name="designing-game-ux"></a>Conception de l’expérience utilisateur de jeux

Une conception inspirée est à la source d’un jeu réussi.

Les jeux partagent certains éléments d’interface utilisateur et des principes de conception communs avec les applications, mais ils ont souvent une apparence et un objectif de conception uniques pour leur expérience utilisateur. Les jeux rencontreront le succès si les aspects suivants sont bien pensés : quand votre jeu doit-il utiliser une expérience utilisateur testée et quand doit-il varier et innover ? La technologie de présentation que vous choisissez pour votre jeu (DirectX, XAML, HTML5 ou une combinaison de celles-ci) peut influencer les détails d’implémentation, mais les principes de conception que vous appliquez ne reposent pas sur ce choix.

Distincte de la conception de l’expérience utilisateur, la conception d’un jeu, par exemple la conception du niveau, le rythme et bien d’autres aspects sont une forme d’art en soi. Elle est de votre ressort, votre équipe et vous, et elle n’est pas traitée dans ce guide de développement.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Informations de base et recommandations sur la conception UWP</td>
        <td>[Conception des applications UWP](https://dev.windows.com/design)</td>
    </tr>
    <tr>
        <td>Conception des états de cycle de vie d’application</td>
        <td>[Recommandations en matière d’expérience utilisateur pour le lancement, la suspension et la reprise](https://msdn.microsoft.com/library/windows/apps/dn611862)</td>
    </tr>
    <tr>
        <td>Ciblage de plusieurs facteurs de forme d’appareil (vidéo)</td>
        <td>[Conception de jeux pour Windows Core](http://channel9.msdn.com/Events/GDC/GDC-2015/Designing-Games-for-a-Windows-Core-World)</td>
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
        <td>[Meilleures pratiques : Couleur](https://assets.windowsphone.com/499cd2be-64ed-4b05-a4f5-cd0c9ad3f6a3/101_BestPractices_Color_InvariantCulture_Default.zip)</td>
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
        <td>[Meilleures pratiques : Typographie](http://go.microsoft.com/fwlink/?LinkId=535007)</td>
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
        <td>[Meilleures pratiques : Carte d’interface utilisateur](http://go.microsoft.com/fwlink/?LinkId=535008)</td>
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
        <td>Développement de jeu DirectX sur la plateforme UWP</td>
        <td>[Jeux et DirectX](index.md)</td>
    </tr>
    <tr>
        <td>Interaction de DirectX avec le modèle d’application UWP</td>
        <td>[Objet application et DirectX](about-the-metro-style-user-interface-and-directx.md)</td>
    </tr>
    <tr>
        <td>Vidéos de développement Graphics et DirectX 12 (YouTube)</td>
        <td>[Microsoft DirectX 12 et Graphics Education](https://www.youtube.com/channel/UCiaX2B8XiXR70jaN7NK-FpA)</td>
    </tr>
    <tr>
        <td>Présentations et informations de référence sur DirectX</td>
        <td>[Jeux et graphismes DirectX](https://msdn.microsoft.com/library/windows/desktop/ee663274)</td>
    </tr>
    <tr>
        <td>Direct3D 12 : Guide de programmation et informations de référence</td>
        <td>[Graphismes Direct3D 12](https://msdn.microsoft.com/library/windows/desktop/dn903821)</td>
    </tr>
    <tr>
        <td>Notions fondamentales sur DirectX 12 (vidéo)</td>
        <td>[Une meilleure alimentation, de meilleures performances : Votre jeu sur DirectX 12](http://channel9.msdn.com/Events/GDC/GDC-2015/Better-Power-Better-Performance-Your-Game-on-DirectX12)</td>
    </tr>
</table>

#### <a name="learning-direct3d-12"></a>Prise en main de Direct3D 12

Découvrez ce qui a changé dans Direct3D 12 et comment commencer à programmer à l’aide de Direct3D 12. 

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Configurer l’environnement de programmation</td>
        <td>[Configuration de l’environnement de programmation Direct3D 12](https://msdn.microsoft.com/library/windows/desktop/dn899120.aspx)</td>
    </tr>
    <tr>
        <td>Comment créer un composant de base</td>
        <td>[Création d’un composant Direct3D 12 de base](https://msdn.microsoft.com/library/windows/desktop/dn859356.aspx)</td>
    </tr>
    <tr>
        <td>Modifications apportées dans Direct3D 12</td>
        <td>[Modifications importantes lors de la migration de Direct3D 11 à Direct3D 12](https://msdn.microsoft.com/library/windows/desktop/dn899194.aspx)</td>
    </tr>
    <tr>
        <td>Comment effectuer le portage de Direct3D 11 vers Direct3D 12</td>
        <td>[Portage de Direct3D 11 vers Direct3D 12](https://msdn.microsoft.com/library/windows/desktop/mt431709.aspx)</td>
    </tr>
    <tr>
        <td>Concepts de liaison de ressource (descripteur de recouvrement, tableau de descripteur, tas de descripteur et signature racine) </td>
        <td>[Liaison de ressource dans Direct3D 12](https://msdn.microsoft.com/library/windows/desktop/dn899206.aspx)</td>
    </tr>
    <tr>
        <td>Gestion de la mémoire</td>
        <td>[Gestion de la mémoire dans Direct3D 12](https://msdn.microsoft.com/library/windows/desktop/dn899198.aspx)</td>
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
        <td>Obtenir le kit de ressources DirectX pour DirectX 11</td>
        <td>[DirectXTK](http://go.microsoft.com/fwlink/?LinkId=248929)</td>
    </tr>
    <tr>
        <td>Obtenir le kit de ressources DirectX pour DirectX 12</td>
        <td>[DirectXTK 12](http://go.microsoft.com/fwlink/?LinkID=615561)</td>
    </tr>
    <tr>
        <td>Obtenir la bibliothèque de traitement des textures DirectX</td>
        <td>[DirectXTex](http://go.microsoft.com/fwlink/?LinkId=248926)</td>
    </tr>
    <tr>
        <td>Obtenir la bibliothèque de traitement des géométries DirectXMesh</td>
        <td>[DirectXMesh](http://go.microsoft.com/fwlink/?LinkID=324981)</td>
    </tr>
    <tr>
        <td>Obtenir UVAtlas pour la création et la compression d’atlas de textures isochart</td>
        <td>[UVAtlas](http://go.microsoft.com/fwlink/?LinkID=512686)</td>
    </tr>
    <tr>
        <td>Obtenir la bibliothèque DirectXMath</td>
        <td>[DirectXMath](http://go.microsoft.com/fwlink/?LinkID=615560)</td>
    </tr>
    <tr>
        <td>Prise en charge de Direct3D 12 dans DirectXTK (billet de blog)</td>
        <td>[Prise en charge de DirectX 12](https://github.com/Microsoft/DirectXTK/issues/2)</td>
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
        <td>[DirectX 12 sur des GPU Nvidia](https://developer.nvidia.com/dx12-dos-and-donts-updated)</td>
    </tr>
    <tr>
        <td>Intel: Efficient rendering with DirectX 12 (en anglais)</td>
        <td>[Rendu de DirectX 12 sur graphiques Intel](https://software.intel.com/sites/default/files/managed/4a/38/Efficient-Rendering-with-DirectX-12-on-Intel-Graphics.pdf)</td>
    </tr>
    <tr>
        <td>Intel: Multi adapter support in DirectX 12 (en anglais)</td>
        <td>[Implémentation d’une application explicite comportant plusieurs adaptateurs à l’aide de DirectX 12](https://software.intel.com/articles/multi-adapter-support-in-directx-12)</td>
    </tr>
    <tr>
        <td>Intel: DirectX 12 tutorial (en anglais)</td>
        <td>[Livre blanc collaboratif, élaboré par Intel, Suzhou Snail et Microsoft](https://software.intel.com/articles/tutorial-migrating-your-apps-to-directx-12-part-1)</td>
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
        <td>[Vignettes, badges et notifications](https://msdn.microsoft.com/library/windows/apps/mt185606)</td>
    </tr>
    <tr>
        <td>Exemple illustrant les vignettes dynamiques et les notifications</td>
        <td>[Exemples de notification](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Notifications)</td>
    </tr>
    <tr>
        <td>Modèles de vignette adaptative (billet de blog)</td>
        <td>[Modèles de vignette adaptative : Schéma et documentation](http://blogs.msdn.com/b/tiles_and_toasts/archive/2015/06/30/adaptive-tile-templates-schema-and-documentation.aspx)</td>
    </tr>
    <tr>
        <td>Conception de vignettes et de badges</td>
        <td>[Recommandations en matière de vignettes et de badges](https://msdn.microsoft.com/library/windows/apps/hh465403)</td>
    </tr>
    <tr>
        <td>Application Windows 10 pour le développement interactif des modèles de vignette dynamique</td>
        <td>[Notifications Visualizer](https://www.microsoft.com/store/apps/9nblggh5xsl1)</td>
    </tr>
    <tr>
        <td>Extension UWP Tile Generator pour Visual Studio</td>
        <td>[Outil permettant de créer toutes les vignettes requises à l’aide d’une image unique](https://visualstudiogallery.msdn.microsoft.com/09611e90-f3e8-44b7-9c83-18dba8275bb2)</td>
    </tr>
    <tr>
        <td>Extension UWP Tile Generator pour Visual Studio (billet de blog)</td>
        <td>[Conseils sur l’utilisation de l’outil UWP Tile Generator](https://blogs.windows.com/buildingapps/2016/02/15/uwp-tile-generator-extension-for-visual-studio/)</td>
    </tr>
</table>
 

### <a name="enable-in-app-product-iap-purchases"></a>Activer les achats de produits dans l’application (in-app)

Un PIA (produit intégré à l’application) est un article supplémentaire que les joueurs peuvent acheter dans le jeu. Il peut s’agir de nouveautés en matière d’extensions, de niveaux de jeu, d’articles ou de tout autre chose susceptibles de plaire à vos joueurs. Utilisés à bon escient, les PIA peuvent fournir des revenus tout en améliorant l’expérience de jeu. Vous pouvez définir et publier des PIA dans votre jeu via le tableau de bord du Centre de développement Windows, et activer les achats dans l’application dans le code de votre jeu.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Produits durables intégrés à l’application</td>
        <td>[Activer les achats de produits in-app](https://msdn.microsoft.com/library/windows/apps/mt219684)</td>
    </tr>
    <tr>
        <td>Produits consommables intégrés à l’application</td>
        <td>[Activer l’achat de produits in-app consommables](https://msdn.microsoft.com/library/windows/apps/mt219683)</td>
    </tr>
    <tr>
        <td>Détails et soumission des produits in-app</td>
        <td>[Soumissions de PIA](https://msdn.microsoft.com/library/windows/apps/mt148551)</td>
    </tr>
    <tr>
        <td>Surveiller les ventes de PIA et les données démographiques correspondantes de votre jeu</td>
        <td>[Rapport sur les acquisitions de produits in-app](https://msdn.microsoft.com/library/windows/apps/mt148538)</td>
    </tr>
</table>
 
### <a name="debugging-and-performance-monitoring-tools"></a>Outils de débogage et de surveillance des performances

Le Kit Windows Performance Toolkit est composé d’outils d’analyse des performances qui génèrent des profils de performances détaillés des applications et des systèmes d’exploitation Windows. Il s’avère particulièrement précieux pour surveiller l’utilisation de la mémoire et améliorer les performances des jeux. Le Kit Windows Performance Toolkit est inclus dans le Kit de développement logiciel Windows 10 et dans Windows ADK. Ce kit d’outils comprend deux outils indépendants : l’enregistreur de performance Windows et Windows Performance Analyzer. Pour générer les fichiers de vidage nécessaires à l’analyse des plantages de jeux, vous bénéficiez également de l’outil ProcDump, qui fait partie de [Windows Sysinternals](https://technet.microsoft.com/sysinternals/default).

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Obtenir le Kit Windows Performance Toolkit à partir de Windows 10 SDK</td>
        <td>[Kit de développement logiciel (SDK) Windows 10](https://developer.microsoft.com/windows/downloads/windows-10-sdk)</td>
    </tr>
    <tr>
        <td>Obtenir le Kit Windows Performance Toolkit à partir de Windows ADK.</td>
        <td>[Windows ADK](https://msdn.microsoft.com/windows/hardware/dn913721.aspx)</td>
    </tr>
    <tr>
        <td>Résoudre les problèmes de réactivité de l’interface utilisateur à l’aide de Windows Performance Analyzer (vidéo).</td>
        <td>[Analyse du chemin critique avec WPA](https://channel9.msdn.com/Shows/Defrag-Tools/Defrag-Tools-156-Critical-Path-Analysis-with-Windows-Performance-Analyzer)</td>
    </tr>
    <tr>
        <td>Diagnostiquer l’utilisation et les fuites de mémoire à l’aide de Enregistreur de performance Windows (vidéo)</td>
        <td>[Encombrement et fuites de mémoire](https://channel9.msdn.com/Shows/Defrag-Tools/Defrag-Tools-154-Memory-Footprint-and-Leaks)</td>
    </tr>
    <tr>
        <td>Obtenir ProcDump</td>
        <td>[ProcDump](https://technet.microsoft.com/sysinternals/dd996900)</td>
    </tr>
    <tr>
        <td>Apprenez à utiliser ProcDump (vidéo)</td>
        <td>[Configurer ProcDump pour créer des fichiers de vidage](https://channel9.msdn.com/Shows/Defrag-Tools/Defrag-Tools-131-Windows-10-SDK)</td>
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
        <td>Optimisation des graphismes et des performances (vidéo)</td>
        <td>[Graphismes et performances améliorés avec DirectX 12](http://channel9.msdn.com/Events/GDC/GDC-2015/Advanced-DirectX12-Graphics-and-Performance)</td>
    </tr>
    <tr>
        <td>Débogage graphique DirectX (vidéo)</td>
        <td>[Résolution des problèmes graphiques épineux liés à votre jeu à l’aide des outils DirectX](http://channel9.msdn.com/Events/GDC/GDC-2015/Solve-the-Tough-Graphics-Problems-with-your-Game-Using-DirectX-Tools)</td>
    </tr>
    <tr>
        <td>Outils Visual Studio 2015 pour le débogage de DirectX 12 (vidéo)</td>
        <td>[Outils DirectX pour Windows 10 dans Visual Studio 2015](https://channel9.msdn.com/Series/ConnectOn-Demand/212)</td>
    </tr>
    <tr>
        <td>Guide de programmation pour Direct3D 12</td>
        <td>[Guide de programmation de Direct3D 12](https://msdn.microsoft.com/library/windows/desktop/dn903821)</td>
    </tr>
    <tr>
        <td>Combinaison de DirectX et XAML</td>
        <td>[Technologie interop DirectX et XAML](directx-and-xaml-interop.md)</td>
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
        <td>[Recommandations en matière de développement pour un public international](https://msdn.microsoft.com/library/windows/apps/xaml/mt186453.aspx)</td>
    </tr>
    <tr>
        <td>Combler le fossé entre les langues, les cultures et la technologie</td>
        <td>[Ressources en ligne pour les conventions linguistiques et la terminologie Microsoft standard](http://www.microsoft.com/Language/Default.aspx)</td>
    </tr>
</table>

## <a name="submitting-and-publishing-your-game"></a>Envoi et publication du jeu

Les informations et guides suivants contribuent à rendre le processus de soumission et de publication aussi aisé que possible.

### <a name="packaging-and-uploading"></a>Création du package et chargement

Vous allez utiliser le nouveau tableau de bord unifié du Centre de développement Windows pour publier et gérer vos packages de jeu.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Publications d’applications du Centre de développement Windows</td>
        <td>[Publier des applications Windows](https://dev.windows.com/publish)</td>
    </tr>
    <tr>
        <td>Publication avancée dans le Centre de développement Windows (GDN)</td>
        <td>[Guide de publication avancée dans le tableau de bord du Centre de développement Windows](https://developer.xboxlive.com/en-us/windows/documentation/Pages/home.aspx)</td>
    </tr>    
    <tr>
        <td>Évaluation de votre jeu (billet de blog)</td>
        <td>[Workflow unique pour affecter les évaluations de l’âge à l’aide du système IARC](https://blogs.windows.com/buildingapps/2016/01/06/now-available-single-age-rating-system-to-simplify-app-submissions/)</td>
    </tr>
    <tr>
        <td>Création d’un package pour votre jeu</td>
        <td>[Créer un package pour votre jeu UWPDirectX](package-your-windows-store-directx-game.md)</td>
    </tr>
    <tr>
        <td>Empaquetage de votre jeu en tant que développeur tiers (billet de blog)</td>
        <td>[Créer des packages téléchargeables sans accès au compte Windows Store de l’éditeur](https://blogs.windows.com/buildingapps/2015/12/15/building-an-app-for-a-3rd-party-how-to-package-their-store-app/)</td>
    </tr>
    <tr>
        <td>Création de packages d’application et d’ensembles de packages d’application à l’aide de MakeAppx</td>
        <td>[Créer des packages à l’aide de l’outil de création de packages d’application MakeAppx.exe](https://msdn.microsoft.com/library/windows/desktop/hh446767)</td>
    </tr>
    <tr>
        <td>Signature numérique des fichiers à l’aide de SignTool</td>
        <td>[Signer les fichiers et vérifier les signatures dans les fichiers à l’aide de SignTool](https://msdn.microsoft.com/library/windows/desktop/aa387764)</td>
    </tr>      
    <tr>
        <td>Chargement et contrôle de version de votre jeu</td>
        <td>[Charger des packages d’application](https://msdn.microsoft.com/library/windows/apps/mt148542)</td>
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
        <td>Contrat du développeur d’application du Windows Store</td>
        <td>[Contrat du développeur d’application](https://msdn.microsoft.com/library/windows/apps/hh694058)</td>
    </tr>
    <tr>
        <td>Stratégies pour la publication d’applications dans le Windows Store</td>
        <td>[Politiques du Windows Store](https://msdn.microsoft.com/library/windows/apps/dn764944)</td>
    </tr>
    <tr>
        <td>Comment faire pour éviter certains problèmes de certification d’application courants</td>
        <td>[Éviter les échecs de certification courants](https://msdn.microsoft.com/library/windows/apps/jj657968)</td>
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
        <td>[Schéma StoreManifest (Windows 10)](https://msdn.microsoft.com/library/windows/apps/mt617335)</td>
    </tr>
</table>
 

## <a name="game-lifecycle-management"></a>Gestion du cycle de vie des jeux


Vous n’avez pas terminé une fois que vous avez développé et fourni votre jeu. Si vous en avez fini avec le développement de la première version, le circuit de votre jeu sur le marché commence à peine quant à lui. Vous allez surveiller son utilisation et les rapports d’erreur, répondre aux commentaires des utilisateurs, et publier des mises à jour pour votre jeu.

### <a name="windows-dev-center-analytics-and-promotion"></a>Promotion et analyses du Centre de développement Windows

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Application Centre de développement</td>
        <td>[Application Centre de développement Windows 10 permettant d’afficher les performances des applications publiées](https://www.microsoft.com/store/apps/dev-center/9nblggh4r5ws)</td>
    </tr>  
    <tr>
        <td>Analyses du Centre de développement Windows</td>
        <td>[Analyses](https://msdn.microsoft.com/library/windows/apps/mt148522)</td>
    </tr>
    <tr>
        <td>Réponse aux avis des clients</td>
        <td>[Répondre aux avis des clients](https://msdn.microsoft.com/library/windows/apps/mt148546)</td>
    </tr>
    <tr>
        <td>Méthodes pour promouvoir votre jeu</td>
        <td>[Promouvoir vos applications](https://dev.windows.com/store-promotion)</td>
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
        <td>[Visual Studio Application Insights](https://azure.microsoft.com/documentation/articles/app-insights-get-started/)</td>
    </tr>
    <tr>
        <td>Activer Application Insights dans les applications Windows</td>
        <td>[Application Insights pour les applications Windows Phone et les applications du Windows Store](https://azure.microsoft.com/documentation/articles/app-insights-windows-get-started/)</td>
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
        <td>[Numérotation des versions de packages](https://msdn.microsoft.com/library/windows/apps/mt188602)</td>
    </tr>
    <tr>
        <td>Recommandations en matière de gestion des packages de jeu</td>
        <td>[Aide sur la gestion des packages d’application](https://msdn.microsoft.com/library/windows/apps/mt188602)</td>
    </tr>
</table>


## <a name="adding-xbox-live-to-your-game"></a>Ajout de Xbox Live à votre jeu


> **Remarque**   Le développement de Xbox Live est géré par le biais de programmes tels qu’ID@Xbox et Microsoft Studios. Ce guide couvre une large gamme de ressources, et vous pouvez constater que certaines ressources ne sont pas accessibles selon le programme auquel vous participez ou votre rôle de développement. Les exemples sont les liens developer.xboxlive.com, forums.xboxlive.com, xdi.xboxlive.com ou réseau GDN (Game Developer Network). Pour plus d’informations sur le partenariat avec Microsoft, voir [Programmes pour développeurs](#developer-programs).

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Télécharger le dernier Kit de développement logiciel (SDK) Xbox Live</td>
        <td>[Kit de développement logiciel (SDK) Xbox Live](http://aka.ms/xsapi2)</td>
    </tr>
    <tr>
        <td>Ajout de Xbox Live à votre application de plateforme Windows universelle</td>
        <td>[Procédure d’ajout du Kit de développement logiciel (SDK) Xbox Live aux applications de plateforme Windows universelle (UWP)](http://aka.ms/xsapi2uwp)</td>
    </tr>
    <tr>
        <td>Configuration requise pour les jeux utilisant Xbox Live</td>
        <td>[Configuration Xbox requise pour Xbox Live sur Windows 10](http://go.microsoft.com/fwlink/?LinkId=533217)</td>
    </tr>
    <tr>
        <td>Vue d’ensemble du développement de jeux Xbox Live (vidéo)</td>
        <td>[Développement avec Xbox Live pour Windows 10](http://channel9.msdn.com/Events/GDC/GDC-2015/Developing-with-Xbox-Live-for-Windows-10)</td>
    </tr>
    <tr>
        <td>Matchmaking multiplateforme (vidéo)</td>
        <td>[Xbox Live en multijoueur : Présentation des services de matchmaking et de jeu multiplateforme](http://channel9.msdn.com/Events/GDC/GDC-2015/Xbox-Live-Multiplayer-Introducing-services-for-cross-platform-matchmaking-and-gameplay)</td>
    </tr>
    <tr>
        <td>Jeu multiplateforme dans Fable Legends (vidéo)</td>
        <td>[Fable Legends : Jeu multiplateforme avec Xbox Live](http://channel9.msdn.com/Events/GDC/GDC-2015/Fable-Legends-Cross-device-Gameplay-with-Xbox-Live)</td>
    </tr>
    <tr>
        <td>Xbox Live : Statistiques et succès (vidéo)</td>
        <td>[Meilleures pratiques pour tirer parti des statistiques et des succès des utilisateurs basés sur le cloud dans Xbox Live](http://channel9.msdn.com/Events/GDC/GDC-2015/Best-Practices-for-Leveraging-Cloud-Based-User-Stats-and-Achievements-in-Xbox-Live)</td>
    </tr>
</table>
 

## <a name="additional-resources"></a>Ressources supplémentaires

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Développement de jeux indépendants (vidéo)</td>
        <td>[De nouvelles opportunités pour les développeurs indépendants](http://channel9.msdn.com/Events/GDC/GDC-2015/New-Opportunities-for-Independent-Developers)</td>
    </tr>
    <tr>
        <td>Considérations pour les appareils mobiles multicœurs (vidéo)</td>
        <td>[Performances de jeu soutenues sur les appareils mobiles multicœurs](http://channel9.msdn.com/Events/GDC/GDC-2015/Sustained-gaming-performance-in-multi-core-mobile-devices)</td>
    </tr>
    <tr>
        <td>Développement de jeux de bureau Windows 10 (vidéo)</td>
        <td>[Jeux pour PC Windows 10](http://channel9.msdn.com/Events/GDC/GDC-2015/PC-Games-for-Windows-10)</td>
    </tr>
</table>



 

 

 

