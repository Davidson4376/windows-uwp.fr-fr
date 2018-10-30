---
author: jwmsft
description: Cette rubrique fournit des explications sur les mappages d’espaces de noms XML/XAML (xmlns) tels qu’ils existent dans l’élément racine de la plupart des fichiers XAML. Elle explique également comment produire des mappages similaires pour les assemblys et les types personnalisés.
title: Espaces de noms XAML et mappage d’espaces de noms
ms.assetid: A19DFF78-E692-47AE-8221-AB5EA9470E8B
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: a1aebe3d9aac460d444a5dffcd63142300c022b7
ms.sourcegitcommit: 753e0a7160a88830d9908b446ef0907cc71c64e7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/30/2018
ms.locfileid: "5761031"
---
# <a name="xaml-namespaces-and-namespace-mapping"></a>Espaces de noms XAML et mappage d’espaces de noms


Cette rubrique fournit des explications sur les mappages d’espaces de noms XML/XAML (**xmlns**) tels qu’ils existent dans l’élément racine de la plupart des fichiers XAML. Elle explique également comment produire des mappages similaires pour les assemblys et les types personnalisés.

## <a name="how-xaml-namespaces-relate-to-code-definition-and-type-libraries"></a>Relation entre les espaces de noms XAML, la définition de code et les bibliothèques de types

De manière générale comme pour son application à la programmation d’applications Windows Runtime, le langage XAML sert à déclarer des objets, les propriétés de ces objets et des relations objet-propriété exprimées sous forme de hiérarchies. Les objets que vous déclarez en XAML sont secondés par des bibliothèques de types ou d’autres représentations qui sont définies par d’autres langages et techniques de programmation. Il peut s’agir entre autres de :

-   l’ensemble intégré d’objets pour Windows Runtime. Ensemble défini d’objets dont l’accès à partir du code XAML utilise un mappage de type interne et une logique d’activation ;
-   bibliothèques distribuées fournies par Microsoft ou par des tierces parties ;
-   bibliothèques qui représentent la définition d’un contrôle tiers incorporé par votre application et redistribué par votre package ;
-   votre propre bibliothèque, incluse dans votre projet, et qui contient tout ou partie de vos définitions de code utilisateur.

Les informations sur le type de stockage sont associées à des définitions d’espaces de noms XAML particulières. Les infrastructures XAML telles que Windows Runtime peuvent agréger plusieurs assemblys et plusieurs espaces de noms de code à mapper à un même espace de noms XAML. Cela autorise un concept de vocabulaire XAML qui couvre une technologie ou infrastructure de programmation plus étendue. Un vocabulaire XAML peut être très étendu: par exemple, la majeure partie du code XAML documenté pour les applications Windows Runtime de cette référence constitue un seul vocabulaire XAML. Un vocabulaire XAML est également extensible: vous pouvez l’étendre en ajoutant des types aux définitions de code de stockage, en veillant à inclure les types dans les espaces de noms de code déjà utilisés en tant que sources d’espaces de noms mappées pour le vocabulaire XAML.

Un processeur XAML peut rechercher des types et des membres des assemblys de stockage associés à cet espace de noms XAML quand il crée une représentation d’objet au moment de l’exécution. C’est pourquoi le langage XAML est utile comme manière de formaliser et d’échanger des définitions de comportement de construction d’objet et pourquoi il est utilisé comme technique de définition d’interface utilisateur pour une application UWP.

## <a name="xaml-namespaces-in-typical-xaml-markup-usage"></a>Espaces de noms XAML dans une utilisation de balisage XAML par défaut

Un fichier XAML déclare presque toujours un espace de noms XAML par défaut dans son élément racine. L’espace de noms XAML par défaut définit les éléments que vous pouvez déclarer sans les qualifier à l’aide d’un préfixe. Par exemple, si vous déclarez un élément `<Balloon />`, un analyseur XAML s’attend à ce qu’un élément **Balloon** valide existe dans l’espace de noms XAML par défaut. Par contre, si **Balloon** ne se trouve pas dans l’espace de noms XAML par défaut défini, vous devez plutôt qualifier ce nom d’élément avec un préfixe, par exemple `<party:Balloon />`. Le préfixe indique que l’élément existe dans un autre espace de noms XAML que l’espace de noms par défaut. Vous devez mapper un espace de noms XAML au préfixe **party** pour pouvoir utiliser cet élément. Les espaces de noms XAML s’appliquent à l’élément spécifique sur lequel ils sont déclarés et également à tout élément contenu par cet élément dans la structure XAML. Pour cette raison, les espaces de noms XAML sont presque toujours déclarés sur les éléments racines d’un fichier XAML afin de tirer parti de cet héritage.

## <a name="the-default-and-xaml-language-xaml-namespace-declarations"></a>Les déclarations d’espaces de noms XAML par défaut et de langage XAML

Dans l’élément racine de la plupart des fichiers XAML figurent deux déclarations **xmlns**. La première déclaration mappe un espace de noms XAML comme espace de noms par défaut: `xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"`

Il s’agit du même identificateur d’espace de noms XAML que celui utilisé dans plusieurs technologies Microsoft précédentes qui utilisent également le langage XAML comme format de balisage de définition d’interface utilisateur. L’utilisation du même identificateur est délibérée et utile quand vous migrez une interface utilisateur précédemment définie vers une application Windows Runtime en C++, C# ou Visual Basic.

La seconde déclaration mappe un espace de noms XAML distinct pour les éléments de langage définis en XAML, en le mappant (généralement) au préfixe «x:»: `xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"`

Cette valeur **xmlns**, ainsi que le préfixe « x: » auquel elle est mappée, est également identique aux définitions utilisées dans plusieurs technologies Microsoft antérieures qui utilisent le langage XAML.

La relation entre ces déclarations est que XAML est une définition de langage et le Windows Runtime est une implémentation qui utilise XAML comme langage et définit un vocabulaire spécifique où ses types sont référencés en XAML.

Le langage XAML spécifie certains éléments linguistiques, chacun d’eux devant être accessible par le biais d’implémentations de processeur XAML opérant sur l’espace de noms XAML. La convention de mappage «x:» pour l’espace de noms XAML de langage XAML est suivie de modèles de projets, d’un exemple de code et de la documentation relative aux fonctionnalités de langage. L’espace de noms XAML définit plusieurs fonctionnalités couramment utilisées qui sont nécessaires même pour les applications Windows Runtime en C++, C# ou Visual Basic. Par exemple, pour attacher du code-behind à un fichier XAML par le biais d’une classe partielle, vous devez nommer cette classe comme [attribut x:Class](x-class-attribute.md) dans l’élément racine du fichier XAML pertinent. Sinon, tout élément défini dans une page XAML en tant que ressource indexée dans des [références de ressources ResourceDictionary et XAML](https://msdn.microsoft.com/library/windows/apps/mt187273) doit avoir l’attribut [x:Key](x-key-attribute.md) défini sur l’élément objet en question.

<span id="other-XAML-namespaces"/>

## <a name="other-xaml-namespaces"></a>Autres espaces de noms XAML

Outre l’espace de noms par défaut et l’espace de noms XAML de langage XAML «x:», il se peut que vous rencontriez d’autres espaces de noms XAML mappés dans le code XAML par défaut initial de certaines applications générées par Microsoft Visual Studio.

### **<a name="d-httpschemasmicrosoftcomexpressionblend2008"></a>d: (`http://schemas.microsoft.com/expression/blend/2008`)**

L’espace de noms XAML «d:» est destiné à la prise en charge de concepteur, plus spécifiquement à la prise en charge de concepteur dans les aires de conception XAML de Microsoft Visual Studio. L’espace de noms XAML «d:» permet de disposer d’attributs de concepteur ou au moment du design sur des éléments XAML. Ces attributs de concepteur affectent uniquement les aspects du comportement XAML liés à la conception. Ils sont ignorés quand le même code XAML est chargé par l’analyseur XAML Windows Runtime lors de l’exécution d’une application. En règle générale, les attributs de concepteur sont valides sur tout élément XAML, mais en pratique il n’y a que certains scénarios où il convient d’appliquer soi-même un attribut de concepteur. Une grande partie des attributs de concepteur ont pour but de procurer une meilleure expérience pour l’interaction avec des contextes de données et des sources de données pendant que vous développez du code et des applications XAML qui utilisent la liaison de données.

-   **d:DesignHeight and d:DesignWidth attributes :** ces attributs sont parfois appliqués à la racine d’un fichier XAML créé automatiquement par Visual Studio ou tout autre concepteur XAML. Par exemple, ces attributs sont définis sur la racine [**UserControl**](https://msdn.microsoft.com/library/windows/apps/br227647) du code XAML qui est créé si vous ajoutez un nouvel objet **UserControl** à votre projet d’application. Ils facilitent la conception de la composition du contenu XAML, afin que vous ayez une certaine anticipation des contraintes de disposition qui peuvent exister une fois que le contenu XAML est utilisé pour une instance de contrôle ou une autre partie d’une page d’interface utilisateur plus grande.

   **Remarque**si vous migrez du code XAML à partir de Microsoft Silverlight peut-être ces attributs sur les éléments racines qui représentent la totalité d’une page de l’interface utilisateur. Dans ce cas, vous pouvez supprimer les attributs. D’autres fonctionnalités des concepteurs XAML, telles que le simulateur, sont probablement plus utiles pour la conception de dispositions de pages qui gèrent correctement la mise à l’échelle et les états d’affichage qu’une disposition de page de taille fixe utilisant **d:DesignHeight** et **d:DesignWidth**.

-   **Attribut d:DataContext :** vous pouvez définir cet attribut sur une racine de page ou un contrôle pour remplacer tout [**DataContext**](https://msdn.microsoft.com/library/windows/apps/br208713) explicite ou hérité que cet objet a sans cela.
-   **Attribut d:DesignSource :** spécifie une source de données au moment de la conception pour un [**CollectionViewSource**](https://msdn.microsoft.com/library/windows/apps/br209833), en remplacement de [**Source**](https://msdn.microsoft.com/library/windows/apps/br209835).
-   **Extensions de balisage d:DesignInstance et d:DesignData :** ces extensions de balisage sont utilisées pour fournir les ressources de données au moment de la conception pour **d:DataContext** ou **d:DesignSource**. Nous ne documentons pas de manière exhaustive ici l’utilisation des ressources de données au moment de la conception. Pour plus d’informations, voir [Attributs au moment de la conception](http://go.microsoft.com/fwlink/p/?LinkId=272504). Pour obtenir des exemples d’utilisation, voir [Exemples de données sur l’aire de conception et pour la création d’un prototype](https://msdn.microsoft.com/library/windows/apps/mt517866).

### **<a name="mc-httpschemasopenxmlformatsorgmarkup-compatibility2006"></a>mc: (`http://schemas.openxmlformats.org/markup-compatibility/2006`)**

«mc:» indique et prend en charge un mode de compatibilité de balisage pour la lecture de code XAML. Généralement, le préfixe « d: » est associé à l’attribut **mc:Ignorable**. Cette technique permet aux analyseurs XAML au moment de l’exécution d’ignorer les attributs de conception décrits dans «d:».

### <a name="local-and-common"></a>**local:** et **common:**

«local:» est un préfixe qui est souvent mappé pour vous dans les pages XAML d’un projet d’application UWP basée sur un modèle. Ce préfixe est mappé pour faire référence au même espace de noms qui est créé pour contenir l’[attribut x:Class](x-class-attribute.md) et le code pour tous les fichiers XAML, y compris app.xaml. Tant que vous définissez les classes personnalisées que vous souhaitez utiliser en XAML dans ce même espace de noms, vous pouvez utiliser le préfixe **local:** pour faire référence à vos types personnalisés en XAML. **common:** est un préfixe connexe qui provient d’un projet d’application UWP basée sur un modèle. Ce préfixe fait référence à un espace de noms « Common » imbriqué qui contient des classes utilitaires, notamment des convertisseurs et des commandes. Les définitions figurent dans le dossier Common de l’affichage **Explorateur de solutions**.

### **<a name="vsm"></a>vsm:**

À ne pas utiliser. «vsm:» est un préfixe que l’on rencontre parfois dans les anciens modèles XAML importés à partir d’autres technologies Microsoft. À l’origine, l’espace de noms avait pour but de résoudre un problème lié à un outil d’espace de noms hérité. Supprimez les définitions d’espaces de noms XAML pour « vsm: » dans tout code XAML que vous utilisez pour Windows Runtime et modifiez les utilisations de préfixe pour [**VisualState**](https://msdn.microsoft.com/library/windows/apps/br209007), [**VisualStateGroup**](https://msdn.microsoft.com/library/windows/apps/br209014) et les objets connexes de façon à utiliser plutôt l’espace de noms XAML par défaut. Pour plus d’informations sur la migration XAML, voir [Migration de code Silverlight ou XAML WPF vers une application Windows Runtime](https://msdn.microsoft.com/library/windows/apps/br229571).

## <a name="mapping-custom-types-to-xaml-namespaces-and-prefixes"></a>Mappage de types personnalisés à des préfixes et des espaces de noms XAML

Vous pouvez mapper un espace de noms XAML de manière à pouvoir utiliser du code XAML pour accéder à vos propres types personnalisés. Autrement dit, vous mappez un espace de noms de code tel qu’il existe dans une représentation de code qui définit le type personnalisé et vous lui assignez un espace de noms XAML ainsi qu’un préfixe pour l’utilisation. Les types personnalisés pour XAML peuvent être définis dans un langage Microsoft .NET (C# ou Microsoft Visual Basic) ou en C++. Le mappage s’effectue en définissant un préfixe **xmlns**. Par exemple, `xmlns:myTypes` définit un nouvel espace de noms XAML accessible en préfixant toutes les utilisations avec le jeton `myTypes:`.

Une définition **xmlns** comprend une valeur en plus de l’attribution de nom au préfixe. La valeur est une chaîne placée entre guillemets et précédée d’un signe égal. Une convention XML courante consiste à associer l’espace de noms XML à un URI (Uniform Resource Identifier), afin qu’il existe une convention pour le caractère unique et l’identification. On observe également cette convention pour l’espace de noms XAML par défaut et l’espace de noms XAML de langage XAML, ainsi que pour certains espaces de noms XAML moins courants utilisés par le XAML Windows Runtime. Toutefois, pour un espace de noms XAML qui mappe des types personnalisés, au lieu de spécifier un URI, vous commencez la définition de préfixe avec le jeton «using:». Après le jeton «using:», vous nommez l’espace de noms du code.

Par exemple, pour mapper un préfixe «custom1» qui vous permet de faire référence à un espace de noms «CustomClasses» et d’utiliser des classes de cet espace de noms/assembly comme éléments objet en XAML, votre page XAML doit inclure le mappage suivant sur l’élément racine: `xmlns:custom1="using:CustomClasses"`

Les classes partielles de la même étendue de page n’ont pas besoin d’être mappées. Par exemple, aucun préfixe n’est nécessaire pour faire référence à un gestionnaire d’événements que vous avez défini pour la gestion des événements à partir de la définition d’interface utilisateur XAML de votre page. De plus, une grande partie des pages XAML de démarrage des projets générés par Visual Studio pour une application Windows Runtime en C++, C# ou Visual Basic mappent déjà un préfixe «local:», qui fait référence à l’espace de noms par défaut spécifié pour le projet et à l’espace de noms utilisé par les définitions de classes partielles.

### <a name="clr-language-rules"></a>Règles du langage CLR

Si vous écrivez votre code de stockage dans un langage .NET (C# ou Microsoft Visual Basic), peut-être utilisez-vous des conventions qui incluent un point («.») dans les noms des espaces de noms afin de créer une hiérarchie conceptuelle d’espaces de noms de code. Si votre définition d’espace de noms contient un point, celui-ci doit faire partie de la valeur que vous spécifiez après le jeton «using:».

Si votre fichier code-behind ou votre fichier de définition de code est un fichier C++, il existe certaines conventions qui suivent quand même la forme du CLR (Common Language Runtime), afin qu’il n’y ait aucune différence dans la syntaxe XAML. Si vous déclarez des espaces de noms imbriqués en C++, le séparateur entre les chaînes d’espaces de noms imbriquées successives doit être «.» plutôt que «::» quand vous spécifiez la valeur qui suit le jeton «using:».

N’utilisez pas de types imbriqués (imbrication d’une énumération dans une classe, par exemple) lorsque vous définissez votre code en vue d’une utilisation avec le langage XAML. Les types imbriqués ne peuvent pas être évalués. L’analyseur XAML n’a aucun moyen de distinguer qu’un point fait partie du nom du type imbriqué et non de l’espace de noms.

## <a name="custom-types-and-assemblies"></a>Assemblys et types personnalisés

Le nom de l’assembly qui définit les types de stockage d’un espace de noms XAML n’est pas spécifié dans le mappage. La logique pour laquelle des assemblys sont disponibles est contrôlée au niveau de la définition de l’application et fait partie des principes de base de la sécurité et du déploiement d’application. Déclarez tout assembly que vous souhaitez inclure comme source de définition de code XAML en tant qu’assembly dépendant dans les paramètres du projet. Pour plus d’informations, voir [Création de composants Windows Runtime en C# et Visual Basic](https://msdn.microsoft.com/library/windows/apps/xaml/hh441572.aspx).

Si vous faites référence à des types personnalisés à partir de la définition d’application ou des définitions de page de l’application principale, ces types sont accessibles sans configuration d’assembly dépendant supplémentaire, mais vous devez quand même mapper l’espace de noms de code qui contient ces types. Une convention courante consiste à mapper le préfixe «local» pour l’espace de noms de code par défaut de toute page XAML donnée. Cette convention est souvent incluse dans les modèles de projets de départ pour les projets XAML.

## <a name="attached-properties"></a>Propriétés jointes

Si vous référencez des propriétés jointes, la partie du type de propriétaire du nom de la propriété jointe doit soit se trouver dans l’espace de noms XAML par défaut, soit être préfixée. S’il est rare de préfixer des attributs séparément de leurs éléments, cela peut être nécessaire dans ce cas, en particulier pour une propriété jointe personnalisée. Pour plus d’informations, voir [Propriétés jointes personnalisées](custom-attached-properties.md).

## <a name="related-topics"></a>Rubriques connexes

* [Vue d’ensemble du langage XAML](xaml-overview.md)
* [Guide de la syntaxe XAML](xaml-syntax-guide.md)
* [Création de composants Windows Runtime en C# et Visual Basic](https://msdn.microsoft.com/library/windows/apps/xaml/hh441572.aspx)
* [Modèles de projet en C#, VB et C++ pour les applications Windows Runtime](https://msdn.microsoft.com/library/windows/apps/hh768232)
* [Migration de code Silverlight ou XAML WPF vers une application Windows Runtime](https://msdn.microsoft.com/library/windows/apps/br229571)
 

