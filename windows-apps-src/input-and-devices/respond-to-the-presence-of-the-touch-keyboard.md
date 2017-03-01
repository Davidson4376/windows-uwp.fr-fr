---
author: Karl-Bridge-Microsoft
Description: "Découvrez comment personnaliser l’interface utilisateur de votre application lorsque le clavier tactile est affiché ou masqué."
title: "Répondre à la présence du clavier tactile"
ms.assetid: 70C6130E-23A2-4F9D-88E7-7060062DA988
label: Respond to the presence of the touch keyboard
template: detail.hbs
keywords: "clavier, accessibilité, navigation, focus, texte, saisie, interactions utilisateur"
ms.author: kbridge
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: c08e33b95241ce95c7670e197e67496b774897cb
ms.lasthandoff: 02/07/2017

---

# <a name="respond-to-the-presence-of-the-touch-keyboard"></a>Répondre à la présence du clavier tactile
<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

Découvrez comment personnaliser l’interface utilisateur de votre application lorsque le clavier tactile est affiché ou masqué.

<div class="important-apis" >
<b>API importantes</b><br/>
<ul>
<li>[**AutomationPeer**](https://msdn.microsoft.com/library/windows/apps/br209185)</li>
<li>[**InputPane**](https://msdn.microsoft.com/library/windows/apps/br242255)</li>
</ul>
</div> 



![clavier tactile en mode de disposition classique](images/touchkeyboard-standard.png)

<sup>Clavier tactile en mode de disposition classique</sup>

Le clavier tactile permet l’entrée de texte pour les appareils qui prennent en charge les fonctions tactiles. Les contrôles de saisie de texte de la plateforme Windows universelle (UWP) appellent le clavier tactile par défaut quand un utilisateur appuie sur un champ d’entrée modifiable. Le clavier tactile reste généralement visible pendant que l’utilisateur navigue entre les contrôles d’un formulaire, mais ce comportement peut varier selon les autres types de contrôles que contient le formulaire.

Pour prendre en charge le comportement de clavier tactile correspondant dans un contrôle de saisie de texte personnalisé qui ne dérive pas d’un contrôle de saisie de texte standard, vous devez utiliser la classe [**AutomationPeer**](https://msdn.microsoft.com/library/windows/apps/br209185) pour exposer vos contrôles à Microsoft UI Automation et mettre en œuvre les modèles de contrôles UI Automation appropriés. Voir [Accessibilité du clavier](https://msdn.microsoft.com/library/windows/apps/mt244347) et [Homologues d’automation personnalisés](https://msdn.microsoft.com/library/windows/apps/mt297667).

Une fois cette prise en charge ajoutée à votre contrôle personnalisé, vous pouvez répondre de manière appropriée à la présence du clavier tactile.

**Éléments requis :  **

Cette rubrique s’appuie sur l’article [Interactions avec le clavier](keyboard-interactions.md).

Vous devez posséder des connaissances de base sur les interactions avec le clavier standard, sur la gestion de la saisie au clavier et des événements de clavier et sur UI Automation.

Si vous débutez dans le développement d’applications de plateforme Windows universelle (UWP), consultez les rubriques ci-dessous pour vous familiariser avec les technologies décrites ici.

-   [Créer votre première application](https://msdn.microsoft.com/library/windows/apps/bg124288)
-   Découvrir les événements avec [Vue d’ensemble des événements et des événements routés](https://msdn.microsoft.com/library/windows/apps/mt185584).

**Recommandations en matière d’expérience utilisateur :  **

Pour obtenir de précieux conseils concernant la conception d’une application optimisée pour la saisie au clavier à la fois utile et conviviale, voir [Recommandations en matière de conception de clavier](https://msdn.microsoft.com/library/windows/apps/hh972345).

## <a name="touch-keyboard-and-a-custom-ui"></a>Clavier tactile et interface utilisateur personnalisée


Voici quelques recommandations de base concernant les contrôles de saisie de texte personnalisés.

-   Affichez le clavier tactile tout au long de l’interaction avec votre formulaire.

-   Assurez-vous que vos contrôles personnalisés ont le [**AutomationControlType**](https://msdn.microsoft.com/library/windows/apps/br209182) UI Automation approprié pour que le clavier reste affiché quand un champ d’entrée de texte perd le focus dans le contexte d’une entrée de texte. Par exemple, si vous avez un menu qui est ouvert au milieu d’un scénario d’entrée de texte et que vous souhaitez que le clavier reste affiché, le menu doit avoir le **AutomationControlType** de Menu.

-   Ne manipulez pas les propriétés UI Automation pour contrôler le clavier tactile. D’autres outils d’accessibilité reposent sur la précision des propriétés UI Automation.

-   Assurez-vous que les utilisateurs peuvent toujours voir le champ d’entrée avec lequel ils interagissent.

    Étant donné que le clavier tactile masque une grande partie de l’écran, la plateforme UWP garantit que le champ d’entrée ayant le focus devient visible quand l’utilisateur navigue parmi les contrôles du formulaire, y compris ceux qui ne sont pas affichés actuellement.

    Lorsque vous personnalisez votre interface utilisateur, donnez un comportement similaire à l’affichage du clavier tactile en gérant les événements [**Showing**](https://msdn.microsoft.com/library/windows/apps/br242262) et [**Hiding**](https://msdn.microsoft.com/library/windows/apps/br242260) exposés par l’objet [**InputPane**](https://msdn.microsoft.com/library/windows/apps/br242255).

    ![Formulaire avec et sans clavier tactile apparent](images/touch-keyboard-pan1.png)

    Dans certains cas, il existe des éléments d’interface utilisateur qui doivent rester tout le temps à l’écran. Concevez l’interface utilisateur de sorte que les contrôles de formulaire se trouvent dans une région panoramique et que les éléments d’interface utilisateur importants soient statiques. Par exemple :

    ![formulaire contenant des zones devant toujours rester affichées](images/touch-keyboard-pan2.png)

## <a name="handling-the-showing-and-hiding-events"></a>Gestion des événements d’affichage et de masquage


Voici un exemple d’association de gestionnaires d’événements pour les événements [**showing**](https://msdn.microsoft.com/library/windows/apps/br242262) et [**hiding**](https://msdn.microsoft.com/library/windows/apps/br242260) du clavier tactile.

```CSharp
public class MyApplication
{
    public MyApplication()
    {
        // Grab the input pane for the main application window and attach
        // touch keyboard event handlers.
        Windows.Foundation.Application.InputPane.GetForCurrentView().Showing  
            += new EventHandler(_OnInputPaneShowing);
        Windows.Foundation.Application.InputPane.GetForCurrentView().Hiding 
            += new EventHandler(_OnInputPaneHiding);
    }

    private void _OnInputPaneShowing(object sender, IInputPaneVisibilityEventArgs eventArgs)
    {
        // If the size of this window is going to be too small, the app uses 
        // the Showing event to begin some element removal animations.
        if (eventArgs.OccludedRect.Top < 400)
        {
            _StartElementRemovalAnimations();

            // Don&#39;t use framework scroll- or visibility-related 
            // animations that might conflict with the app&#39;s logic.
            eventArgs.EnsuredFocusedElementInView = true; 
        }
    }

    private void _OnInputPaneHiding(object sender, IInputPaneVisibilityEventArgs eventArgs)
    {
        if (_ResetToDefaultElements())
        {
            eventArgs.EnsuredFocusedElementInView = true; 
        }
    }

    private void _StartElementRemovalAnimations()
    {
        // This function starts the process of removing elements 
        // and starting the animation.
    }

    private void _ResetToDefaultElements()
    {
        // This function resets the window&#39;s elements to their default state.
    }
}
```

## <a name="related-articles"></a>Articles connexes

* [Interactions avec le clavier](keyboard-interactions.md)
* [Accessibilité du clavier](https://msdn.microsoft.com/library/windows/apps/mt244347)
* [Homologues d’automation personnalisés](https://msdn.microsoft.com/library/windows/apps/mt297667)


**Exemples d’archive**
* [Entrée : exemple de clavier tactile](http://go.microsoft.com/fwlink/p/?linkid=246019)
* [Exemple de réponse à l’apparition du Clavier visuel](http://go.microsoft.com/fwlink/p/?linkid=231633)
* [Exemple de modification de texte XAML](http://go.microsoft.com/fwlink/p/?LinkID=251417)
* [Exemple d’accessibilité XAML](http://go.microsoft.com/fwlink/p/?linkid=238570)
 

 





