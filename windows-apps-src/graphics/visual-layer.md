---
ms.assetid: a2751e22-6842-073a-daec-425fb981bafe
title: Couche visuelle
description: L’API Windows.UI.Composition vous donne accès à la couche de composition comprise entre la couche d’infrastructure (XAML) et la couche graphique (DirectX).
---
# Couche visuelle

\[ Mise à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir la [documentation archivée](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

Dans Windows 10, un travail considérable a été effectué pour créer un compositeur unifié et un moteur de rendu pour toutes les applications Windows, qu’elles soient pour ordinateurs de bureau ou pour appareils mobiles. Conséquence de ce travail, l’API WinRT Composition unifiée, appelée Windows.UI.Composition, qui permet d’accéder à de nouveaux objets de composition légers et aux animations et effets du nouveau compositeur.

Windows.UI.Composition est une API déclarative [en mode retenu](https://msdn.microsoft.com/library/windows/desktop/ff684178.aspx) qui peut être appelée à partir de toutes les applications UWP créer des objets, des animations et des effets de composition directement dans une application. L’API est un puissant complément des infrastructures existantes, par exemple XAML, qui fournit aux développeurs d’applications UWP une surface C# familière à ajouter dans leur application. Ces API peuvent être utilisées pour créer des applications reposant sur moins d’infrastructure de style DX.

Un développeur XAML peut « dérouler » jusqu’à la couche de composition en C# pour effectuer un travail personnalisé dans la couche de composition à l’aide de WinRT afin de créer une composition d’objets dans l’application XAML, au lieu d’atteindre la couche graphique et d’utiliser DirectX et C++ pour effectuer des tâches personnalisées de l’interface utilisateur.

![](images/layers-win-ui-composition.png)
## <span id="Composition_Objects_and_The_Compositor"> </span> <span id="composition_objects_and_the_compositor"> </span> <span id="COMPOSITION_OBJECTS_AND_THE_COMPOSITOR"> </span>Objets de composition et compositeur

Les objets de composition sont créés par le [**Compositor**](https://msdn.microsoft.com/library/windows/apps/Dn706789) qui sert de fabrique pour les objets de composition. Le compositeur peut créer des objets [**Visual**](https://msdn.microsoft.com/library/windows/apps/Dn706858), qui autorisent la création d’une structure arborescente d’éléments visuels sur laquelle toutes les autres fonctionnalités et tous les objets de composition de l’API reposent.

L’API permet aux développeurs de définir et de créer un ou plusieurs objets [**Visual**](https://msdn.microsoft.com/library/windows/apps/Dn706858), qui représentent chacun un nœud unique dans une arborescence d’éléments visuels.

Les éléments visuels peuvent être des conteneurs d’autres éléments visuels. Ils peuvent également héberger des éléments visuels de contenu. L’API permet une utilisation aisée en fournissant un ensemble clair d’objets [**Visual**](https://msdn.microsoft.com/library/windows/apps/Dn706858) destiné à des tâches spécifiques qui existent dans une hiérarchie :

-   [
            **Visual**](https://msdn.microsoft.com/library/windows/apps/Dn706858) : objet de base. La plupart des propriétés sont répertoriées ici et héritées par les autres objets visuels.
-   [
            **ContainerVisual**](https://msdn.microsoft.com/library/windows/apps/Dn706810) : dérive de [**Visual**](https://msdn.microsoft.com/library/windows/apps/Dn706858), et ajoute la possibilité d’insérer des éléments visuels enfants.
-   [
            **SpriteVisual**](https://msdn.microsoft.com/library/windows/apps/Mt589433) : dérive de [**ContainerVisual**](https://msdn.microsoft.com/library/windows/apps/Dn706810), et contient des images, des effets et des chaînes de permutation.
-   [
            **Compositor**](https://msdn.microsoft.com/library/windows/apps/Dn706789) : fabrique d’objet qui gère la relation entre une application et le processus de composition du système.

Le compositeur est également une fabrique pour plusieurs autres objets de composition utilisés pour découper ou transformer des éléments visuels de l’arborescence, ainsi qu’un riche ensemble d’animations et d’effets.

## <span id="Effects_System"> </span> <span id="effects_system"> </span> <span id="EFFECTS_SYSTEM"> </span>Système d’effets

Windows.UI.Composition prend en charge les effets en temps réel qui peuvent être animés, personnalisés et chaînés. Par effets, on entend transformation affine 2D, composite arithmétique, fusion, source de couleur, composite, contraste, exposition, nuances de gris, transfert gamma, rotation teinte, inversion, température, saturation, sépia et teinte.

Pour plus d’informations, voir la vue d’ensemble [Effets de composition](composition-effects.md).

## <span id="Animation_System"> </span> <span id="animation_system"> </span> <span id="ANIMATION_SYSTEM"> </span>Système d’animations

Windows.UI.Composition contient un système d’animations non propre à une infrastructure qui vous permet de configurer deux types d’animations : animations par images clés et animations par expressions. Elles permettent de déplacer des objets visuels, d’effectuer une transformation ou une découpe, ou d’animer un effet. L’exécution directe dans le processus de composition garantit fluidité et mise à l’échelle, car vous pouvez exécuter un grand nombre d’animations simultanées uniques.

Pour plus d’informations, voir la vue d’ensemble [Animations de composition](composition-animation.md).

## <span id="XAML_Interoperation"> </span> <span id="xaml_interoperation"> </span> <span id="XAML_INTEROPERATION"> </span>Interopérabilité de XAML

En plus de créer depuis le début une arborescence d’éléments visuels, l’API Composition peut interopérer avec une interface utilisateur XAML existante à l’aide de la classe [**ElementCompositionPreview**](https://msdn.microsoft.com/library/windows/apps/Mt608976) dans [**Windows.UI.Xaml.Hosting**](https://msdn.microsoft.com/library/windows/apps/Hh701908).


**Remarque**  
Cet article s’adresse aux développeurs de Windows 10 qui créent des applications pour la plateforme Windows universelle (UWP). Si vous développez une application pour Windows 8.x ou Windows Phone 8.x, voir la [documentation archivée](http://go.microsoft.com/fwlink/p/?linkid=619132).

 

## <span id="Additional_Resources_"> </span> <span id="additional_resources_"> </span> <span id="ADDITIONAL_RESOURCES_"> </span>Ressources supplémentaires :

-   Article MSDN de Kenny Kerr portant sur cette API : [Graphics and Animation - Windows Composition Turns 10](https://msdn.microsoft.com/magazine/mt590968) (en anglais)
-   Exemples de composition dans [GitHub Composition](https://github.com/Microsoft/composition).
-   [
            **Documentation de référence complète pour l’API**](https://msdn.microsoft.com/library/windows/apps/Dn706878).
-   Problèmes connus : [Problèmes connus](https://social.msdn.microsoft.com/Forums/en-US/home?forum=Win10SDKToolsIssues).

 

 




<!--HONumber=Mar16_HO1-->
