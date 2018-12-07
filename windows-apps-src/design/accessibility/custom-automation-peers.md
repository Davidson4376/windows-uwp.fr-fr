---
Description: Describes the concept of automation peers for Microsoft UI Automation, and how you can provide automation support for your own custom UI class.
ms.assetid: AA8DA53B-FE6E-40AC-9F0A-CB09637C87B4
title: Homologues d’automatisation personnalisés
label: Custom automation peers
template: detail.hbs
ms.date: 07/13/2018
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 18d3affe5f142c56314d132ba488d87c6f285723
ms.sourcegitcommit: a3dc929858415b933943bba5aa7487ffa721899f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/07/2018
ms.locfileid: "8782075"
---
# <a name="custom-automation-peers"></a>Homologues d’automatisation personnalisés  

Décrit le concept des homologues pour UIAutomation, et la manière dont vous pouvez fournir l’automatisation pour votre classe d’interface utilisateur.

UI Automation procure une infrastructure utilisable par les clients Automation pour examiner ou utiliser les interfaces utilisateur de diverses infrastructures et plateformes d’interface utilisateur. Si vous écrivez une application UWP, les classes que vous utilisez déjà pour votre interface utilisateur fournissent la prise en charge UI Automation. Vous pouvez dériver de classes non verrouillées existantes pour définir un nouveau genre de contrôle d’interface utilisateur ou de classe de prise en charge. Au cours de ce processus, il se peut que votre classe ajoute un comportement qui doit offrir une prise en charge d’accessibilité non gérée par la prise en charge UI Automation par défaut. Dans ce cas, vous devez étendre la prise en charge UI Automation existante en dérivant de la classe [**AutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR209185) utilisée par l’implémentation de base, en ajoutant toute prise en charge nécessaire à votre implémentation d’homologue et en signalant à l’infrastructure de contrôle UWP qu’elle doit créer votre nouvel homologue.

UI Automation permet de bénéficier non seulement d’applications d’accessibilité et de technologies d’assistance telles que les lecteurs d’écran, mais aussi du code (test) d’assurance qualité. Quel que soit le scénario, les clients UI Automation peuvent examiner les éléments d’interface utilisateur et simuler l’interaction utilisateur avec votre application à partir d’un autre code hors de votre application. Pour plus d’informations sur UI Automation sur toutes les plateformes et dans son sens le plus large, voir [Vue d’ensemble d’UI Automation](https://msdn.microsoft.com/library/windows/desktop/Ee684076).

Il existe deux publics distincts d’utilisateurs de l’infrastructure UI Automation.

* Les ***clients* UI Automation** appellent les API UI Automation pour se familiariser avec l’ensemble de l’interface utilisateur actuellement présentée à l’utilisateur. Par exemple, une technologie d’assistance telle qu’un lecteur d’écran fait office de client UI Automation. L’interface utilisateur se présente sous la forme d’une arborescence d’éléments d’automation liés les uns aux autres. Le client UI Automation peut s’intéresser à une seule application à la fois ou à l’arborescence tout entière. Le client UI Automation peut se servir des API UI Automation pour parcourir l’arborescence et pour consulter ou modifier des informations dans les éléments Automation.
* Les ***fournisseurs* UI Automation** apportent des informations à l’arborescence UI Automation en implémentant les API chargées d’exposer les éléments dans l’interface utilisateur qu’ils ont introduites dans le cadre de leur application. Lorsque vous créez un contrôle, vous devez à présent agir en tant que participant au scénario du fournisseur UI Automation. En tant que fournisseur, vous devez vous assurer que tous les clients UI Automation peuvent utiliser l’infrastructure UI Automation pour interagir avec votre contrôle à des fins à la fois d’accessibilité et de test.

En règle générale, l’infrastructure UI Automation est dotée d’API parallèles : une API pour les clients UI Automation et une autre du même nom pour les fournisseurs UI Automation. Cette rubrique décrit, en grande partie, les API dédiées au fournisseur UI Automation, tout particulièrement les classes et interfaces qui permettent l’extensibilité du fournisseur dans cette infrastructure de l’interface utilisateur. Occasionnellement, nous citons les API UI Automation utilisées par les clients UI Automation pour offrir autre un point de vue, ou bien nous proposons une table de recherche qui met en corrélation les API des clients avec celles des fournisseurs. Pour en savoir plus sur le point de vue client, consultez le [Guide de programmeur du client UI Automation](https://msdn.microsoft.com/library/windows/desktop/Ee684021).

> [!NOTE]
> En règle générale, les clients UI Automation n’utilisent pas le code managé et ne sont pas implémentés en tant qu’applications UWP (il s’agit généralement d’applications de bureau). UI Automation est basé sur une norme et non sur une implémentation ni une infrastructure spécifique. De nombreux clients UI Automation existants, y compris des technologies d’assistance comme les lecteurs d’écran, utilisent des interfaces COM (Component Object Model) pour interagir avec UI Automation, le système et les applications exécutées dans les fenêtres enfants. Pour en savoir plus sur les interfaces COM et l’écriture d’un client UI Automation utilisant COM, voir [Notions de base d’UI Automation](https://msdn.microsoft.com/library/windows/desktop/Ee684007).

<span id="Determining_the_existing_state_of_UI_Automation_support_for_your_custom_UI_class"/>
<span id="determining_the_existing_state_of_ui_automation_support_for_your_custom_ui_class"/>
<span id="DETERMINING_THE_EXISTING_STATE_OF_UI_AUTOMATION_SUPPORT_FOR_YOUR_CUSTOM_UI_CLASS"/>

## <a name="determining-the-existing-state-of-ui-automation-support-for-your-custom-ui-class"></a>Détermination de l’état existant de la prise en charge UI Automation pour votre classe d’interface utilisateur personnalisée  
Avant d’essayer de mettre en œuvre un homologue d’automation pour un contrôle personnalisé, vous devez vérifier si la classe de base et son homologue d’automation fournissent déjà l’accessibilité ou la prise en charge d’automation dont vous avez besoin. Dans de nombreux cas, la combinaison des implémentations [**FrameworkElementAutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR242472), des homologues spécifiques et des modèles qu’ils mettent en œuvre peut contribuer à une expérience d’accessibilité de niveau basique mais satisfaisante. Tout dépend de la quantité des modifications que vous avez apportées à l’exposition du modèle objet de votre contrôle par rapport à sa classe de base. Cela varie également selon que vos ajouts à la fonctionnalité de classe de base sont en corrélation avec de nouveaux éléments d’interface utilisateur dans le contrat modèle ou avec l’apparence visuelle du contrôle. Dans certains cas, vos modifications peuvent introduire de nouveaux aspects d’expérience utilisateur qui exigent une prise en charge supplémentaire de l’accessibilité.

Même si la classe homologue de base existante prend en charge l’accessibilité à un niveau basique, il est toujours préférable de définir un homologue pour communiquer des informations **ClassName** précises à UI Automation dans des scénarios de tests automatisés. Cet élément est particulièrement important si vous écrivez un contrôle destiné à un usage tiers.

<span id="Automation_peer_classes__"/>
<span id="automation_peer_classes__"/>
<span id="AUTOMATION_PEER_CLASSES__"/>

## <a name="automation-peer-classes"></a>Classes homologues d’automation  
UWP repose sur des techniques et conventions UI Automation existantes employées par des infrastructures d’interface utilisateur de code managé précédentes, telles que Windows Forms, Windows Presentation Foundation (WPF) et Microsoft Silverlight. Une grande partie des classes de contrôle et de leurs fonctions et rôles ont également pour origine une infrastructure d’interface utilisateur antérieure.

Par convention, les noms des classes homologues commencent par le nom de la classe du contrôle et se terminent par «AutomationPeer». Par exemple, [**ButtonAutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR242458) est la classe homologue pour la classe du contrôle [**Button**](https://msdn.microsoft.com/library/windows/apps/BR209265).

> [!NOTE]
> Pour les besoins de cette rubrique, nous traitons les propriétés relatives à l’accessibilité comme des éléments plus importants lorsque vous implémentez un homologue de contrôle. Toutefois, pour obtenir un concept plus général de la prise en charge UI Automation, vous devez implémenter un homologue selon les recommandations qui figurent dans le [Guide de programmeur du fournisseur UI Automation](https://msdn.microsoft.com/library/windows/desktop/Ee671596) et dans [Notions de base d’UI Automation](https://msdn.microsoft.com/library/windows/desktop/Ee684007). Ces rubriques n’abordent pas les API [**AutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR209185) spécifiques que vous utilisez habituellement pour fournir les informations dans l’infrastructure UWP pour UI Automation, mais elles décrivent les propriétés qui identifient votre classe ou apportent d’autres informations ou bien proposent d’autres interactions.

<span id="Peers__patterns_and_control_types"/>
<span id="peers__patterns_and_control_types"/>
<span id="PEERS__PATTERNS_AND_CONTROL_TYPES"/>

## <a name="peers-patterns-and-control-types"></a>Homologues, modèles et types de contrôle  
Un *modèle de contrôle* est une implémentation d’interface qui expose un aspect particulier des fonctionnalités d’un contrôle à un client UI Automation. Les clients UI Automation utilisent les propriétés et méthodes exposées par le biais d’un modèle de contrôle pour extraire des informations sur les fonctionnalités d’un contrôle ou pour manipuler son comportement au moment de l’exécution.

Les modèles de contrôle offrent un moyen de catégoriser et d’exposer les fonctionnalités d’un contrôle indépendamment du type de contrôle ou de l’apparence du contrôle. Par exemple, un contrôle qui présente une interface sous forme de tableau utilise un modèle de contrôle **Grid** pour exposer le nombre de lignes et de colonnes du tableau et pour permettre à un client UI Automation d’extraire des éléments du tableau. Pour citer d’autres exemples, le client UI Automation peut avoir recours au modèle de contrôle **Invoke** pour les contrôles qu’il est possible d’appeler (des boutons, par exemple) et au modèle de contrôle **Scroll** pour les contrôles munis de barres de défilement, telles que des zones de liste, des affichages Liste ou des zones de liste modifiable. Chaque modèle de contrôle représente un type de fonctionnalité distinct et les modèles de contrôle peuvent être combinés pour décrire le jeu complet de fonctionnalités prises en charge par un contrôle donné.

Les modèles de contrôle sont à l’interface utilisateur ce que les interfaces sont aux objets COM. Dans COM, vous pouvez interroger un objet et lui demander quelles interfaces il prend en charge, puis vous servir de ces interfaces pour accéder aux fonctionnalités. Dans UI Automation, les clients UI Automation peuvent interroger un élément UI Automation sur les modèles de contrôle qu’il prend en charge, puis interagir avec l’élément et son contrôle homologue via des propriétés, des méthodes, des événements et des structures exposés par les modèles de contrôle pris en charge.

L’une des principales fonctions d’un homologue d’automation consiste à indiquer à un client UI Automation les modèles de contrôle que l’élément d’interface utilisateur peut prendre en charge via son homologue. Pour ce faire, les fournisseurs UI Automation implémentent de nouveaux homologues qui modifient le comportement de la méthode [**GetPattern**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getpattern) en remplaçant la méthode [**GetPatternCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getpatterncore). Les clients UI Automation effectuent des appels que le fournisseur UI Automation mappe à la méthode d’appel **GetPattern**. Les clients UI Automation demandent chaque modèle spécifique avec lequel ils veulent interagir. Si l’homologue prend en charge le modèle, il retourne une référence d’objet à lui-même ; sinon, il retourne la valeur **null**. Si la valeur renvoyée n’est pas **null**, le client UI Automation s’attend à pouvoir appeler les API de l’interface de modèle en tant que client, pour pouvoir interagir avec ce modèle de contrôle.

Un *type de contrôle* est un moyen de définir globalement la fonctionnalité d’un contrôle représenté par l’homologue. Ce concept est différent de celui d’un modèle de contrôle. En effet, un modèle signale à UI Automation les informations qu’il peut obtenir ou les actions qu’il peut effectuer via une interface particulière, le type de contrôle se situe un niveau au-dessus. Chaque type de contrôle est associé à des instructions sur ces aspects d’UI Automation :

* Modèles de contrôle UI Automation : un type de contrôle peut prendre en charge plusieurs modèles, chacun d’entre eux représentant une classification d’informations ou une interaction différente. Chaque type de contrôle possède un jeu de modèles de contrôle que le contrôle doit prendre en charge, un jeu facultatif et un jeu que le contrôle ne doit pas prendre en charge.
* Valeurs de propriété UI Automation : chaque type de contrôle possède un jeu de propriétés que le contrôle doit prendre en charge. Ces propriétés sont d’ordre général, comme décrit dans [Vue d’ensemble des propriétés UI Automation](https://msdn.microsoft.com/library/windows/desktop/Ee671594), et non spécifiques au modèle.
* Événements UI Automation : chaque type de contrôle possède un jeu d’événements que le contrôle doit prendre en charge. Là encore, ces événements, décrits dans la section [Vue d’ensemble des propriétés UI Automation](https://msdn.microsoft.com/library/windows/desktop/Ee671221), sont d’ordre général.
* Structure de l’arborescence UI Automation : chaque type de contrôle définit la façon dont le contrôle doit apparaître dans la structure de l’arborescence UI Automation.

Indépendamment de la manière dont les homologues d’automatisation de l’infrastructure sont implémentés, la fonctionnalité de client UI Automation n’est pas liée à UWP. En réalité, les clients UI Automation existants, comme les technologies d’assistance, vont probablement utiliser d’autres modèles de programmation, par exemple COM. Dans ce type de modèle, les clients peuvent effectuer une action **QueryInterface** pour l’interface de modèle de contrôle COM qui implémente le modèle demandé ou l’infrastructure UI Automation générale à des fins d’examen des propriétés, des événements ou de l’arborescence. Pour les modèles, l’infrastructure UI Automation assemble ce code d’interface avec le code UWP exécuté sur le fournisseur UI Automation de l’application et l’homologue approprié.

Lorsque vous implémentez des modèles de contrôle dédiés à une infrastructure de code managé à l’instar d’une applicationUWP du WindowsStore en C\# ou MicrosoftVisualBasic, vous pouvez utiliser des interfaces.NETFramework pour représenter ces modèles plutôt que la représentation d’interfaceCOM. Par exemple, l’interface de modèle UI Automation pour une implémentation de fournisseur Microsoft .NET du modèle **Invoke** est [**IInvokeProvider**](https://msdn.microsoft.com/library/windows/apps/BR242582).

Pour obtenir une liste de modèles de contrôle, d’interfaces de fournisseur et de leurs fonctions, voir [Modèles de contrôle et interfaces](control-patterns-and-interfaces.md). Pour obtenir la liste des types de contrôle, voir [Vue d’ensemble des types de contrôle d’UI Automation](https://msdn.microsoft.com/library/windows/desktop/Ee671197).

<span id="Guidance_for_how_to_implement_control_patterns"/>
<span id="guidance_for_how_to_implement_control_patterns"/>
<span id="GUIDANCE_FOR_HOW_TO_IMPLEMENT_CONTROL_PATTERNS"/>

### <a name="guidance-for-how-to-implement-control-patterns"></a>Instructions relatives à l’implémentation des modèles de contrôle  
Les modèles de contrôle et ce à quoi ils sont destinés font partie d’une définition plus large de l’infrastructureUIAutomation. À ce titre, ils ne s’appliquent pas seulement à la prise en charge de l’accessibilité au niveau d’une applicationUWP. Lorsque vous implémentez un modèle de contrôle, assurez-vous de respecter les instructions documentées sur MSDN, ainsi que dans la spécification UI Automation. Si vous recherchez des instructions, vous pouvez utiliser les rubriques MSDN sans avoir besoin de consulter la spécification, dans la plupart des cas. Les instructions pour chaque modèle sont documentées dans [Implémentation de modèles de contrôle UI Automation](https://msdn.microsoft.com/library/windows/desktop/Ee671292). Notez que chaque rubrique sous cette zone contient une section «Instructions et conventions d’implémentation», ainsi qu’une section «Membres requis». Les instructions font généralement référence à des API spécifiques de l’interface du modèle de contrôle appropriée dans le document de référence [Interfaces des modèles de contrôle pour fournisseurs](https://msdn.microsoft.com/library/windows/desktop/Ee671201). Ces interfaces correspondent aux interfaces natives/COM (et leurs API utilisent une syntaxe de style COM). Cependant, tous les éléments que vous voyez ont un équivalent dans l’espace de noms [**Windows.UI.Xaml.Automation.Provider**](https://msdn.microsoft.com/library/windows/apps/BR209225).

Si vous utilisez les homologues d’automatisation par défaut et étendez leur comportement, la rédaction de ces homologues répond déjà aux instructions d’UI Automation. S’ils prennent en charge les modèles de contrôle, vous pouvez être certain que la prise en charge des modèles est conforme aux instructions fournies dans la section [Implémentation de modèles de contrôle UI Automation](https://msdn.microsoft.com/library/windows/desktop/Ee671292). Si un homologue de contrôle signale qu’il est représentatif d’un type de contrôle défini par UI Automation, les instructions documentées dans la section [Prise en charge des types de contrôle UI Automation](https://msdn.microsoft.com/library/windows/desktop/Ee671633) sont alors respectées par cet homologue.

Néanmoins, vous aurez peut-être besoin d’instructions supplémentaires concernant les modèles ou les types de contrôle afin de respecter les recommandations d’UI Automation lors de votre implémentation d’homologue. Ceci est particulièrement vrai si vous implémentez un modèle ou un type de contrôle qui n’existe pas encore en tant qu’implémentation par défaut dans un contrôle UWP. Par exemple, le modèle pour les annotations n’est implémenté dans aucun contrôle XAML par défaut. Mais il est possible qu’une application utilise intensivement des annotations. Dans ce cas, vous devez mettre en évidence cette fonctionnalité afin qu’elle soit accessible. Dans ce scénario, votre homologue doit implémenter [**IAnnotationProvider**](https://msdn.microsoft.com/library/windows/apps/Hh738493) et probablement se signaler en tant que type de contrôle **Document** avec des propriétés appropriées afin d’indiquer que vos documents prennent en charge les annotations.

Nous vous recommandons d’utiliser les instructions disponibles pour les modèles sous [Implémentation des modèles de contrôle UI Automation](https://msdn.microsoft.com/library/windows/desktop/Ee671292) ou les types de contrôle sous [Prise en charge des types de contrôle UI Automation](https://msdn.microsoft.com/library/windows/desktop/Ee671633) afin d’obtenir des directions et des instructions d’ordre général. Vous pouvez même essayer de cliquer sur certains liens d’API pour obtenir des descriptions et des remarques relatives à la nature de chaque API. Cependant, pour obtenir les détails de la syntaxe nécessaire pour la programmation d’applications UWP, recherchez l’API équivalente dans l’espace de noms [**Windows.UI.Xaml.Automation.Provider**](https://msdn.microsoft.com/library/windows/apps/BR209225) et utilisez ces pages de référence pour obtenir plus d’informations.

<span id="Built-in_automation_peer_classes"/>
<span id="built-in_automation_peer_classes"/>
<span id="BUILT-IN_AUTOMATION_PEER_CLASSES"/>

## <a name="built-in-automation-peer-classes"></a>Classes homologues d’automation intégrées  
En règle générale, les éléments implémentent une classe homologue d’automation s’ils acceptent une activité d’interface utilisateur de l’utilisateur ou s’ils contiennent des informations nécessaires aux utilisateurs de technologies d’assistance représentant l’interface utilisateur interactive ou significative des applications. Les éléments visuels UWP n’ont pas tous des homologues d’automation. [**Button**](https://msdn.microsoft.com/library/windows/apps/BR209265) et [**TextBox**](https://msdn.microsoft.com/library/windows/apps/BR209683) sont des exemples de classes qui mettent en œuvre des homologues d’automatisation. Les classes [**Border**](https://msdn.microsoft.com/library/windows/apps/BR209250) et les classes basées sur [**Panel**](https://msdn.microsoft.com/library/windows/apps/BR227511) (telles que [**Grid**](https://msdn.microsoft.com/library/windows/apps/BR242704) et [**Canvas**](https://msdn.microsoft.com/library/windows/apps/BR209267)) sont des exemples de classes qui ne mettent pas en œuvre d’homologues d’automatisation. Un élément **Panel** n’a aucun homologue, car il fournit un comportement de la disposition qui est uniquement visuel. Il n’existe, pour l’utilisateur, aucun moyen pertinent concernant l’accessibilité d’interagir avec un élément **Panel**. Tous les éléments enfants qu’un élément **Panel** contient sont en revanche signalés aux arborescences UI Automation en tant qu’éléments enfants du prochain parent disponible dans l’arborescence qui dispose d’un homologue ou d’une représentation d’élément.

<span id="UI_Automation_and_UWP_process_boundaries"/>
<span id="ui_automation_and_uwp_process_boundaries"/>
<span id="UI_AUTOMATION_AND_UWP_PROCESS_BOUNDARIES"/>

## <a name="ui-automation-and-uwp-process-boundaries"></a>Limites de processus UIAutomation et UWP  
En règle générale, le code client UIAutomation qui accède à une application UWP s’exécute hors processus. L’infrastructure UI Automation permet de communiquer des informations au-delà des limites du processus. Ce concept est présenté de manière plus détaillée dans la rubrique [Notions de base d’UI Automation](https://msdn.microsoft.com/library/windows/desktop/Ee684007).

<span id="OnCreateAutomationPeer"/>
<span id="oncreateautomationpeer"/>
<span id="ONCREATEAUTOMATIONPEER"/>

## <a name="oncreateautomationpeer"></a>OnCreateAutomationPeer  
Toutes les classes qui dérivent de [**UIElement**](https://msdn.microsoft.com/library/windows/apps/BR208911) contiennent la méthode virtuelle protégée [**OnCreateAutomationPeer**](https://msdn.microsoft.com/ibrary/windows/apps/windows.ui.xaml.uielement.oncreateautomationpeer). La séquence d’initialisation d’objet pour les homologues d’automatisation appelle **OnCreateAutomationPeer** pour se procurer l’objet homologue d’automatisation de chaque contrôle et créer ainsi une arborescence UI Automation à utiliser au moment de l’exécution. Le code UI Automation peut utiliser l’homologue pour obtenir des informations sur les caractéristiques et fonctionnalités d’un contrôle et pour simuler l’utilisation interactive au moyen de ses modèles de contrôle. Un contrôle personnalisé qui prend en charge l’automatisation doit substituer **OnCreateAutomationPeer** et renvoyer une instance d’une classe qui dérive de [**AutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR209185). Par exemple, si un contrôle personnalisé est dérivé de la classe [**ButtonBase**](https://msdn.microsoft.com/library/windows/apps/BR227736), l’objet renvoyé par **OnCreateAutomationPeer** doit dériver de [**ButtonBaseAutomationPeer**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.automation.peers.buttonbaseautomationpeer).

Si vous écrivez une classe de contrôle personnalisé et envisagez de fournir également un nouvel homologue d’automatisation, vous devez remplacer la méthode [**OnCreateAutomationPeer**](https://msdn.microsoft.com/ibrary/windows/apps/windows.ui.xaml.uielement.oncreateautomationpeer) de votre contrôle personnalisé de sorte qu’il retourne une nouvelle instance de votre homologue. Votre classe homologue doit directement ou indirectement dériver de [**AutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR209185).

Par exemple, le code qui suit déclare que le contrôle personnalisé `NumericUpDown` doit utiliser l’homologue `NumericUpDownPeer` pour les besoins de UIAutomation.

```csharp
using Windows.UI.Xaml.Automation.Peers;
...
public class NumericUpDown : RangeBase {
    public NumericUpDown() {
    // other initialization; DefaultStyleKey etc.
    }
    ...
    protected override AutomationPeer OnCreateAutomationPeer()
    {
        return new NumericUpDownAutomationPeer(this);
    }
}
```

```vb
Public Class NumericUpDown
    Inherits RangeBase
    ' other initialization; DefaultStyleKey etc.
       Public Sub New()
       End Sub
       Protected Overrides Function OnCreateAutomationPeer() As AutomationPeer
              Return New NumericUpDownAutomationPeer(Me)
       End Function
End Class
```

```cppwinrt
// NumericUpDown.idl
namespace MyNamespace
{
    runtimeclass NumericUpDown : Windows.UI.Xaml.Controls.Primitives.RangeBase
    {
        NumericUpDown();
        Int32 MyProperty;
    }
}

// NumericUpDown.h
...
struct NumericUpDown : NumericUpDownT<NumericUpDown>
{
    ...
    Windows::UI::Xaml::Automation::Peers::AutomationPeer OnCreateAutomationPeer()
    {
        return winrt::make<MyNamespace::implementation::NumericUpDownAutomationPeer>(*this);
    }
};
```

```cpp
//.h
public ref class NumericUpDown sealed : Windows::UI::Xaml::Controls::Primitives::RangeBase
{
// other initialization not shown
protected:
    virtual AutomationPeer^ OnCreateAutomationPeer() override
    {
         return ref new NumericUpDownAutomationPeer(this);
    }
};
```

> [!NOTE]
> L’implémentation [**OnCreateAutomationPeer**](https://msdn.microsoft.com/ibrary/windows/apps/windows.ui.xaml.uielement.oncreateautomationpeer) doit se limiter à l’initialisation d’une nouvelle instance de votre homologue d’automation personnalisé, en passant le contrôle d’appel propriétaire, et au renvoi de cette instance. Ne tentez pas une logique supplémentaire dans cette méthode. En particulier, toute logique qui peut éventuellement aboutir à la destruction de [**AutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR209185) dans le même appel peut entraîner un comportement inattendu lors de l’exécution.

Dans des implémentations classiques de [**OnCreateAutomationPeer**](https://msdn.microsoft.com/ibrary/windows/apps/windows.ui.xaml.uielement.oncreateautomationpeer), le *owner* est précisé sous la forme **this** ou **Me** puisque la substitution de méthode intervient dans la même étendue que le reste de la définition de classe de contrôle.

La définition de classe homologue réelle peut survenir dans le même fichier de code que le contrôle ou dans un fichier de code distinct. Les définitions d’homologues existent toutes dans l’espace de noms [**Windows.UI.Xaml.Automation.Peers**](https://msdn.microsoft.com/library/windows/apps/BR242563) qui est un espace de noms distinct des contrôles pour lesquels ils fournissent des homologues. Vous pouvez choisir de déclarer vos homologues également dans des espaces de noms distincts, tant que vous référencez les espaces de noms nécessaires pour l’appel de méthode [**OnCreateAutomationPeer**](https://msdn.microsoft.com/ibrary/windows/apps/windows.ui.xaml.uielement.oncreateautomationpeer).

<span id="Choosing_the_correct_peer_base_class"/>
<span id="choosing_the_correct_peer_base_class"/>
<span id="CHOOSING_THE_CORRECT_PEER_BASE_CLASS"/>

### <a name="choosing-the-correct-peer-base-class"></a>Choix de la classe de base d’homologue appropriée  
Assurez-vous que votre [**AutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR209185) est dérivé d’une classe de base qui procure une correspondance parfaite pour la logique homologue existante de la classe de contrôle de laquelle vous dérivez. Dans l’exemple précédent, du fait que `NumericUpDown` est dérivé de [**RangeBase**](https://msdn.microsoft.com/library/windows/apps/BR227863), une classe [**RangeBaseAutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR242506) sur laquelle vous devez baser votre homologue est disponible. En utilisant la classe homologue correspondante la plus proche en parallèle à la manière dont vous dérivez le contrôle lui-même, vous pouvez au moins éviter de remplacer certaines des fonctionnalités [**IRangeValueProvider**](https://msdn.microsoft.com/library/windows/apps/BR242590) puisque la classe homologue de base les a déjà implémentées.

La classe [**Control**](https://msdn.microsoft.com/library/windows/apps/BR209390) de base n’a aucune classe homologue correspondante. Si vous avez besoin de faire correspondre une classe homologue à un contrôle personnalisé dérivé de **Control**, dérivez la classe homologue personnalisée de [**FrameworkElementAutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR242472).

Si vous dérivez directement de [**ContentControl**](https://msdn.microsoft.com/library/windows/apps/BR209365), cette classe ne présente aucun comportement d’homologue d’automatisation par défaut dans la mesure où aucune implémentation [**OnCreateAutomationPeer**](https://msdn.microsoft.com/ibrary/windows/apps/windows.ui.xaml.uielement.oncreateautomationpeer) référençant une classe homologue n’existe. Assurez-vous soit d’implémenter **OnCreateAutomationPeer** pour utiliser votre propre homologue, soit d’utiliser [**FrameworkElementAutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR242472) en guise d’homologue si ce niveau de prise en charge de l’accessibilité convient pour votre contrôle.

> [!NOTE]
> Vous ne dérivez généralement pas de [**AutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR209185), mais plutôt de [**FrameworkElementAutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR242472). Si vous avez dérivé directement de **AutomationPeer**, vous devez dupliquer une grande partie de la prise en charge de l’accessibilité de base qui, autrement, proviendrait de **FrameworkElementAutomationPeer**.

<span id="Initialization_of_a_custom_peer_class"/>
<span id="initialization_of_a_custom_peer_class"/>
<span id="INITIALIZATION_OF_A_CUSTOM_PEER_CLASS"/>

## <a name="initialization-of-a-custom-peer-class"></a>Initialisation d’une classe homologue personnalisée  
L’homologue d’automatisation doit définir un constructeur de type sécurisé qui utilise une instance du contrôle propriétaire pour l’initialisation de base. Dans l’exemple qui suit, l’implémentation transmet la valeur *owner* à la base [**RangeBaseAutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR242506) et, en fin de compte, c’est le [**FrameworkElementAutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR242472) qui utilise *owner* pour définir [**FrameworkElementAutomationPeer.Owner**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.frameworkelementautomationpeer.owner).

```csharp
public NumericUpDownAutomationPeer(NumericUpDown owner): base(owner)
{}
```

```vb
Public Sub New(owner As NumericUpDown)
    MyBase.New(owner)
End Sub
```

```cppwinrt
// NumericUpDownAutomationPeer.idl
import "NumericUpDown.idl";
namespace MyNamespace
{
    runtimeclass NumericUpDownAutomationPeer : Windows.UI.Xaml.Automation.Peers.AutomationPeer
    {
        NumericUpDownAutomationPeer(NumericUpDown owner);
        Int32 MyProperty;
    }
}

// NumericUpDownAutomationPeer.h
...
struct NumericUpDownAutomationPeer : NumericUpDownAutomationPeerT<NumericUpDownAutomationPeer>
{
    ...
    NumericUpDownAutomationPeer(MyNamespace::NumericUpDown const& owner);
};
```

```cpp
//.h
public ref class NumericUpDownAutomationPeer sealed :  Windows::UI::Xaml::Automation::Peers::RangeBaseAutomationPeer
//.cpp
public:    NumericUpDownAutomationPeer(NumericUpDown^ owner);
```

<span id="Core_methods_of_AutomationPeer"/>
<span id="core_methods_of_automationpeer"/>
<span id="CORE_METHODS_OF_AUTOMATIONPEER"/>

## <a name="core-methods-of-automationpeer"></a>Méthodes principales d’AutomationPeer  
Pour des raisons liées à l’infrastructure UWP, les méthodes substituables d’un homologue d’automatisation font partie d’une paire de méthodes: la méthode d’accès publique que le fournisseur UI Automation utilise comme point de transfert pour les clients UI Automation, et la méthode de personnalisation «Core» protégée qu’une classe UWP peut remplacer pour influencer le comportement. La paire de méthodes est liée par défaut pour que l’appel à la méthode d’accès appelle toujours la méthode «Core» parallèle qui a l’implémentation de fournisseur ou, en guise de solution de secours, appelle une implémentation par défaut à partir des classes de base.

Au moment d’implémenter un homologue pour un contrôle personnalisé, remplacez toutes les méthodes «Core» à partir de la classe homologue d’automatisation de base dans laquelle vous souhaitez exposer le comportement qui est unique à votre contrôle personnalisé. Le code UI Automation obtient des informations sur votre contrôle en appelant des méthodes publiques de la classe homologue. Pour fournir des informations sur votre contrôle, remplacez chaque méthode par un nom se terminant par «Core» lorsque votre implémentation et structure de contrôle créent des scénarios d’accessibilité ou d’autres scénarios UI Automation qui diffèrent de ce que la classe homologue d’automatisation de base prend en charge.

Au minimum, quand vous définissez une nouvelle classe homologue, implémentez la méthode [**GetClassNameCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getclassnamecore) comme le décrit l’exemple suivant.

```csharp
protected override string GetClassNameCore()
{
    return "NumericUpDown";
}
```

> [!NOTE]
> Vous pouvez, si vous le souhaitez, stocker les chaînes sous forme de constantes plutôt que directement dans le corps de méthode. Pour [**GetClassNameCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getclassnamecore), vous n’avez pas besoin de localiser cette chaîne. La propriété **LocalizedControlType** est utilisée chaque fois qu’une chaîne localisée est requise par un client UI Automation, et non **ClassName**.

### <span id="GetAutomationControlType"/>
<span id="getautomationcontroltype"/>
<span id="GETAUTOMATIONCONTROLTYPE"/>GetAutomationControlType

Certaines technologies d’assistance utilisent la valeur [**GetAutomationControlType**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getautomationcontroltype) directement lors du signalement des caractéristiques des éléments dans une arborescence UI Automation, comme des informations supplémentaires au-delà de **Name** UI Automation. Si votre contrôle est très différent du contrôle duquel vous dérivez et que vous voulez signaler un type de contrôle différent de celui indiqué par la classe homologue de base utilisée par le contrôle, vous devez implémenter un homologue et remplacer [**GetAutomationControlTypeCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getautomationcontroltypecore) dans votre implémentation d’homologue. Ceci est primordial surtout si vous dérivez d’une classe de base généralisée, telle que [**ItemsControl**](https://msdn.microsoft.com/library/windows/apps/BR242803) ou [**ContentControl**](https://msdn.microsoft.com/library/windows/apps/BR209365), dans laquelle l’homologue de base n’apporte aucune information précise sur le type de contrôle.

Votre implémentation de [**GetAutomationControlTypeCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getautomationcontroltypecore) décrit votre contrôle en renvoyant une valeur [**AutomationControlType**](https://msdn.microsoft.com/library/windows/apps/BR209182). Bien qu’il soit possible de renvoyer **AutomationControlType.Custom**, vous devez renvoyer l’un des types de contrôles plus spécifiques s’il décrit de manière précise les principaux scénarios de votre contrôle. En voici un exemple.

```csharp
protected override AutomationControlType GetAutomationControlTypeCore()
{
    return AutomationControlType.Spinner;
}
```

> [!NOTE]
> Sauf si vous spécifiez [**AutomationControlType.Custom**](https://msdn.microsoft.com/library/windows/apps/BR209182), vous n’avez pas à implémenter [**GetLocalizedControlTypeCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getlocalizedcontroltypecore) pour fournir une valeur de propriété **LocalizedControlType** aux clients. L’infrastructure UI Automation courante propose des chaînes traduites pour toutes les valeurs **AutomationControlType** possibles autres que **AutomationControlType.Custom**.

<span id="GetPattern_and_GetPatternCore"/>
<span id="getpattern_and_getpatterncore"/>
<span id="GETPATTERN_AND_GETPATTERNCORE"/>

### <a name="getpattern-and-getpatterncore"></a>GetPattern et GetPatternCore  
L’implémentation de [**GetPatternCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getpatterncore) d’un homologue renvoie l’objet qui prend en charge le modèle demandé dans le paramètre d’entrée. Plus précisément, un client UI Automation appelle une méthode qui est transmise à la méthode [**GetPattern**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getpattern) du fournisseur et spécifie une valeur d’énumération [**PatternInterface**](https://msdn.microsoft.com/library/windows/apps/BR242496) qui nomme le modèle demandé. Votre substitution de **GetPatternCore** doit renvoyer l’objet qui met en œuvre le modèle spécifié. Cet objet est l’homologue lui-même, car l’homologue doit implémenter l’interface de modèle correspondante chaque fois qu’il signale qu’il prend en charge un modèle. Si votre homologue ne propose aucune implémentation personnalisée pour un modèle mais que vous savez que la base de l’homologue implémente véritablement le modèle, vous pouvez appeler l’implémentation de la méthode **GetPatternCore** du type de base de votre **GetPatternCore**. La méthode **GetPatternCore** d’un homologue doit renvoyer une valeur **null** si un modèle n’est pas pris en charge par l’homologue. En revanche, plutôt que de renvoyer une valeur **null** directement depuis votre implémentation, vous devez généralement vous appuyer sur l’appel à l’implémentation de base pour renvoyer une valeur **null** pour tous les modèles non pris en charge.

Lorsqu’un modèle est pris en charge, l’implémentation de [**GetPatternCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getpatterncore) peut renvoyer une valeur **this** ou **Me**. Le client UI Automation doit alors normalement effectuer un cast de la valeur de retour [**GetPattern**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getpattern) vers l’interface de modèle demandée chaque fois qu’elle n’affiche pas la valeur **null**.

Si une classe homologue hérite d’un autre homologue et si tous les signalements de modèle et de prise en charge nécessaires sont déjà gérés par la classe de base, l’implémentation de [**GetPatternCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getpatterncore) n’est pas utile. Par exemple, si vous mettez en œuvre un contrôle de plage qui dérive de [**RangeBase**](https://msdn.microsoft.com/library/windows/apps/BR227863) et que votre homologue dérive de [**RangeBaseAutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR242506), cet homologue se renvoie lui-même pour [**PatternInterface.RangeValue**](https://msdn.microsoft.com/library/windows/apps/BR242496) et dévoile des implémentations opérationnelles de l’interface [**IRangeValueProvider**](https://msdn.microsoft.com/library/windows/apps/BR242590) qui prend en charge le modèle.

Bien qu’il ne s’agisse pas de code littéral, cet exemple ressemble à peu près à l’implémentation de [**GetPatternCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getpatterncore) déjà présente dans [**RangeBaseAutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR242506).


```csharp
protected override object GetPatternCore(PatternInterface patternInterface)
{
    if (patternInterface == PatternInterface.RangeValue)
    {
        return this;
    }
    return base.GetPattern(patternInterface);
}
```

Si vous mettez en œuvre un homologue et que vous n’avez pas toute la prise en charge dont vous avez besoin par le biais d’une classe homologue de base, ou si vous voulez modifier ou accroître l’ensemble de modèles hérités de la base pris en charge par votre homologue, vous devez substituer [**GetPatternCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getpatterncore) pour permettre aux clients UI Automation d’utiliser les modèles.

Pour obtenir une liste des modèles de fournisseurs disponibles dans l’implémentation UWP de la prise en charge UI Automation, voir [**Windows.UI.Xaml.Automation.Provider**](https://msdn.microsoft.com/library/windows/apps/BR209225). Chacun de ces modèles a une valeur correspondante de l’énumération [**PatternInterface**](https://msdn.microsoft.com/library/windows/apps/BR242496), ce qui permet aux clients UI Automation de demander le modèle dans un appel [**GetPattern**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getpattern).

Un homologue peut signaler qu’il prend en charge plusieurs modèles. Dans ce cas, la substitution doit inclure la logique de chemin d’accès de retour pour chaque valeur [**PatternInterface**](https://msdn.microsoft.com/library/windows/apps/BR242496) prise en charge et renvoyer l’homologue dans chaque cas correspondant. L’appelant doit généralement ne demander qu’une seule interface à la fois et il lui incombe d’effectuer un cast vers l’interface attendue.

Voici un exemple de substitution de [**GetPatternCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getpatterncore) pour un homologue personnalisé. Il indique la prise en charge de deux modèles, [**IRangeValueProvider**](https://msdn.microsoft.com/library/windows/apps/BR242590) et [**IToggleProvider**](https://msdn.microsoft.com/library/windows/apps/BR242653). Il s’agit ici d’un contrôle d’affichage multimédia qui peut s’afficher en plein écran (mode bascule) et qui possède une barre de progression dans laquelle les utilisateurs peuvent sélectionner une position (contrôle de plage). Ce code provient de l’[exemple d’accessibilitéXAML](http://go.microsoft.com/fwlink/p/?linkid=238570).


```csharp
protected override object GetPatternCore(PatternInterface patternInterface)
{
    if (patternInterface == PatternInterface.RangeValue)
    {
        return this;
    }
    else if (patternInterface == PatternInterface.Toggle)
    {
        return this;
    }
    return null;
}
```

<span id="Forwarding_patterns_from_sub-elements"/>
<span id="forwarding_patterns_from_sub-elements"/>
<span id="FORWARDING_PATTERNS_FROM_sub-elementS"/>

### <a name="forwarding-patterns-from-sub-elements"></a>Transfert de modèles à partir de sous-éléments  
Une implémentation de méthode [**GetPatternCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getpatterncore) peut également spécifier un sous-élément ou une partie comme fournisseur de modèle pour son hôte. Cet exemple reproduit la manière dont [**ItemsControl**](https://msdn.microsoft.com/library/windows/apps/BR242803) transfère la gestion des modèles de défilement à l’homologue de son contrôle [**ScrollViewer**](https://msdn.microsoft.com/library/windows/apps/BR209527) interne. Pour spécifier un sous-élément pour la gestion des modèles, ce code obtient l’objet sous-élément, crée un homologue pour le sous-élément à l’aide de la méthode [**FrameworkElementAutomationPeer.CreatePeerForElement**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.frameworkelementautomationpeer.createpeerforelement), puis renvoie le nouvel homologue.


```csharp
protected override object GetPatternCore(PatternInterface patternInterface)
{
    if (patternInterface == PatternInterface.Scroll)
    {
        ItemsControl owner = (ItemsControl) base.Owner;
        UIElement itemsHost = owner.ItemsHost;
        ScrollViewer element = null;
        while (itemsHost != owner)
        {
            itemsHost = VisualTreeHelper.GetParent(itemsHost) as UIElement;
            element = itemsHost as ScrollViewer;
            if (element != null)
            {
                break;
            }
        }
        if (element != null)
        {
            AutomationPeer peer = FrameworkElementAutomationPeer.CreatePeerForElement(element);
            if ((peer != null) && (peer is IScrollProvider))
            {
                return (IScrollProvider) peer;
            }
        }
    }
    return base.GetPatternCore(patternInterface);
}
```

<span id="Other_Core_methods"/>
<span id="other_core_methods"/>
<span id="OTHER_CORE_METHODS"/>

### <a name="other-core-methods"></a>Autres méthodes principales  
Il est possible que votre contrôle doive prendre en charge des équivalents clavier pour les principaux scénarios. Pour plus d’informations sur les raisons d’une telle nécessité, voir [Accessibilité du clavier](keyboard-accessibility.md). L’implémentation de la prise en charge des touches clavier s’inscrit nécessairement dans le cadre du code de contrôle et non du code de l’homologue puisqu’elle est inhérente à la logique d’un contrôle mais votre classe homologue doit remplacer les méthodes [**GetAcceleratorKeyCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getacceleratorkeycore) et [**GetAccessKeyCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getaccesskeycore) pour signaler les touches utilisées aux clients UI Automation. Retenez que les chaînes qui signalent des informations de ce type devront peut-être localisées et doivent par conséquent provenir de ressources, et non de chaînes codées en dur.

Si vous fournissez un homologue pour une classe qui prend en charge une collection, il est préférable de dériver à la fois des classes fonctionnelles et des classes homologues qui offrent déjà ce genre de prise en charge de collection. Si ceci est impossible, les homologues des contrôles qui maintiennent des collections enfants devront peut-être substituer la méthode homologue liée à la collection [**GetChildrenCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getchildrencore) pour signaler correctement les relations parent-enfant à l’arborescence UI Automation.

Mettez en œuvre les méthodes [**IsContentElementCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.iscontentelementcore) et [**IsControlElementCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.iscontrolelementcore) pour indiquer si votre contrôle a du contenu de données ou s’il remplit un rôle interactif dans l’interface utilisateur (ou les deux). Par défaut, les deux méthodes renvoient la valeur **true**. Ces paramètres améliorent la simplicité d’utilisation des technologies d’assistance telles que les lecteurs d’écran, qui peuvent avoir recours à ces méthodes pour filtrer l’arborescence d’automatisation. Si votre méthode [**GetPatternCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getpatterncore) transfère la gestion des modèles vers un homologue de sous-élément, la méthode **IsControlElementCore** de ce dernier peut renvoyer la valeur **false** afin de masquer l’homologue de sous-élément dans l’arborescence d’automatisation.

Certains contrôles peuvent prendre en charge des scénarios d’étiquetage dans lesquels une étiquette de texte apporte des informations sur une partie non textuelle ou un contrôle entretient une relation d’étiquetage connue avec un autre contrôle dans l’interface utilisateur. S’il est possible de fournir un comportement par classe utile, vous pouvez remplacer [**GetLabeledByCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getlabeledbycore) pour valider ce comportement.

[**GetBoundingRectangleCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getboundingrectanglecore) et [**GetClickablePointCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getclickablepointcore) sont utilisés principalement dans des scénarios de tests automatisés. Si vous voulez prendre en charge les tests automatisés pour votre contrôle, vous pouvez remplacer ces méthodes. Cela peut être souhaitable pour les contrôles de type plage, où vous ne pouvez pas suggérer juste un seul point, car l’endroit où l’utilisateur clique dans l’espace de coordonnées a un effet différent sur une plage. Par exemple, l’homologue d’automatisation [**ScrollBar**](https://msdn.microsoft.com/library/windows/apps/BR209745) par défaut remplace **GetClickablePointCore** pour retourner une valeur [**Point**](https://msdn.microsoft.com/library/windows/apps/BR225870) « n’est pas un nombre ».

[**GetLiveSettingCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getlivesettingcore) influence la valeur par défaut du contrôle pour la valeur **LiveSetting** pour UI Automation. Vous pouvez la remplacer si vous voulez que votre contrôle retourne une valeur autre que [**AutomationLiveSetting.Off**](https://msdn.microsoft.com/library/windows/apps/JJ191519). Pour plus d’informations sur ce que **LiveSetting** représente, voir [**AutomationProperties.LiveSetting**](https://msdn.microsoft.com/library/windows/apps/JJ191516).

Vous pouvez remplacer [**GetOrientationCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getorientationcore) si votre contrôle comprend une propriété d’orientation définissable qui peut mapper à [**AutomationOrientation**](https://msdn.microsoft.com/library/windows/apps/BR209184). Les classes [**ScrollBarAutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR242522) et [**SliderAutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR242546) effectuent cette opération.

<span id="Base_implementation_in_FrameworkElementAutomationPeer"/>
<span id="base_implementation_in_frameworkelementautomationpeer"/>
<span id="BASE_IMPLEMENTATION_IN_FRAMEWORKELEMENTAUTOMATIONPEER"/>

### <a name="base-implementation-in-frameworkelementautomationpeer"></a>Implémentation de base dans FrameworkElementAutomationPeer  
L’implémentation de base de [**FrameworkElementAutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR242472) fournit certains informations UI Automation qu’il est possible d’interpréter à partir de diverses propriétés de disposition et de comportement définies au niveau de l’infrastructure.

* [**GetBoundingRectangleCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getboundingrectanglecore) : renvoie une structure [**Rect**](https://msdn.microsoft.com/library/windows/apps/BR225994) fondée sur les caractéristiques de disposition connues. Renvoie une valeur nulle 0 **Rect** si [**IsOffscreen**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.isoffscreen) affiche la valeur **true**.
* [**GetClickablePointCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getclickablepointcore) : renvoie une structure [**Point**](https://msdn.microsoft.com/library/windows/apps/BR225870) fondée sur les caractéristiques de disposition connues, tant qu’une valeur **BoundingRectangle** différente de zéro est disponible.
* [**GetNameCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getnamecore) : un comportement plus étendu peut être résumé ici ; voir [**GetNameCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getnamecore). En bref, cette méthode tente de convertir les chaînes du contenu connu d’une classe [**ContentControl**](https://msdn.microsoft.com/library/windows/apps/BR209365) ou de classes associées dotées d’un contenu. De même, si une valeur est disponible pour [**LabeledBy**](https://msdn.microsoft.com/library/windows/apps/Hh759769), la valeur **Name** de cet élément est utilisée comme élément **Name**.
* [**HasKeyboardFocusCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.haskeyboardfocuscore) : méthode évaluée en fonction des propriétés [**FocusState**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.control.focusstate) et [**IsEnabled**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.control.isenabled) du propriétaire. Les éléments qui ne sont pas des contrôles renvoient toujours la valeur **false**.
* [**IsEnabledCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.isenabledcore) : méthode évaluée en fonction de la propriété [**IsEnabled**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.control.isenabled) du propriétaire s’il s’agit d’une classe [**Control**](https://msdn.microsoft.com/library/windows/apps/BR209390). Les éléments qui ne sont pas des contrôles renvoient toujours la valeur **true**. Cela ne signifie pas que le propriétaire est activé dans le sens de l’interaction classique, mais que l’homologue est activé même si le propriétaire n’a pas de propriété **IsEnabled**.
* [**IsKeyboardFocusableCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.iskeyboardfocusablecore): retourne **true** si le propriétaire est un [**Control**](https://msdn.microsoft.com/library/windows/apps/BR209390); sinon, retourne **false**.
* [**IsOffscreenCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.isoffscreencore): une propriété [**Visibility**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.visibility) de type [**Collapsed**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.visibility) dans l’élément propriétaire ou l’un de ses parents équivaut à une valeur **true** pour [**IsOffscreen**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.isoffscreen). Exception: un objet [**Popup**](https://msdn.microsoft.com/library/windows/apps/BR227842) peut être visible même si les parents de son propriétaire ne le sont pas.
* [**SetFocusCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.setfocuscore) : appelle la méthode [**Focus**](https://msdn.microsoft.com/library/windows/apps/hh702161).
* [**GetParent**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getparent) : appelle la propriété [**FrameworkElement.Parent**](https://msdn.microsoft.com/library/windows/apps/BR208739) à partir du propriétaire et recherche l’homologue adéquat. Comme il ne s’agit pas d’une paire de substitution avec une méthode «Core», vous ne pouvez pas modifier ce comportement.

> [!NOTE]
> Les homologues UWP par défaut implémentent un comportement à l’aide d’un code natif interne chargé d’implémenter UWP, et pas nécessairement en faisant appel à un code UWP réel. Vous ne serez pas en mesure de consulter le code ou la logique de cette implémentation via une technique de réflexionCLR (Common Language Runtime) ou d’autres techniques. Vous ne verrez pas non plus de pages de référence distinctes pour les substitutions de sous-classes du comportement d’homologue de base. Par exemple, il se peut qu’il existe un autre comportement pour la méthode [**GetNameCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getnamecore) d’une classe [**TextBoxAutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR242550) qui ne sera pas décrite dans la page de référence **AutomationPeer.GetNameCore** et il n’y a pas de page de référence pour **TextBoxAutomationPeer.GetNameCore**. Il n’existe même pas de page de référence **TextBoxAutomationPeer.GetNameCore**. Lisez plutôt la rubrique de référence pour la classe homologue la plus immédiate et recherchez des notes d’implémentation dans la section Remarques.

<span id="Peers_and_AutomationProperties"/>
<span id="peers_and_automationproperties"/>
<span id="PEERS_AND_AUTOMATIONPROPERTIES"/>

## <a name="peers-and-automationproperties"></a>Homologues et AutomationProperties  
Votre homologue d’automation doit fournir des valeurs par défaut appropriées pour les informations liées à l’accessibilité de votre contrôle. Notez que tout code d’application qui utilise le contrôle peut remplacer une partie du comportement en insérant des valeurs de propriétés jointes [**AutomationProperties**](https://msdn.microsoft.com/library/windows/apps/BR209081) dans les instances de contrôle. Les appelants peuvent procéder ainsi pour les contrôles par défaut ou pour les contrôles personnalisés. L’exempleXAML suivant crée un bouton doté de deux propriétés UIAutomation personnalisées: `<Button AutomationProperties.Name="Special"      AutomationProperties.HelpText="This is a special button."/>`

Pour plus d’informations sur les propriétés jointes de [**AutomationProperties**](https://msdn.microsoft.com/library/windows/apps/BR209081), voir [Informations d’accessibilité élémentaires](basic-accessibility-information.md).

Certaines des méthodes [**AutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR209185) existent en raison de la manière générale dont les fournisseurs UI Automation doivent transmettre des informations mais ces méthodes ne sont généralement pas implémentées dans les homologues de contrôle. Ceci est lié au fait que les valeurs [**AutomationProperties**](https://msdn.microsoft.com/library/windows/apps/BR209081) appliquées au code de l’application qui utilise les contrôles dans une interface utilisateur spécifique doivent fournir des informations. Par exemple, pour définir la relation d’étiquetage entre les deux contrôles différents au sein de l’interface utilisateur, la plupart des applications appliqueraient une valeur [**AutomationProperties.LabeledBy**](https://msdn.microsoft.com/library/windows/apps/Hh759769). Cependant, la méthode [**LabeledByCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getlabeledbycore) est implémentée dans certains homologues qui représentent des relations de données ou d’éléments dans un contrôle. Il peut s’agir d’un en-tête utilisé pour étiqueter un champ de données, d’éléments étiquetés avec leurs conteneurs ou de scénarios similaires.

<span id="Implementing_patterns"/>
<span id="implementing_patterns"/>
<span id="IMPLEMENTING_PATTERNS"/>

## <a name="implementing-patterns"></a>Implémentation de modèles  
Examinons comment écrire un homologue pour un contrôle chargé de mettre en œuvre un comportement de développement/réduction en procédant à l’implémentation de l’interface de modèle de contrôle pour les actions développer/réduire. L’homologue doit activer l’accessibilité pour le comportement de développement/réduction en se renvoyant lui-même chaque fois que la méthode [**GetPattern**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getpattern) est appelée avec une valeur [**PatternInterface.ExpandCollapse**](https://msdn.microsoft.com/library/windows/apps/BR242496). L’homologue doit alors hériter de l’interface du fournisseur pour ce modèle ([**IExpandCollapseProvider**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.provider.iexpandcollapseprovider)) et proposer des implémentations pour chacun des membres de cette même interface. Dans le cas présent, l’interface dispose de trois membres à substituer : [**Expand**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.provider.iexpandcollapseprovider.expand), [**Collapse**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.provider.iexpandcollapseprovider.collapse) et [**ExpandCollapseState**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.provider.iexpandcollapseprovider.expandcollapsestate).

Il est préférable de planifier l’accessibilité au moment de concevoir l’API de la classe proprement dite. Lorsque vous êtes confronté à un comportement potentiellement requis soit dans le cadre d’interactions standard avec un utilisateur qui travaille dans l’interface utilisateur, soit par le biais d’un modèle de fournisseur d’automation, vous devez préciser une méthode unique que la réponse d’interface utilisateur ou le modèle d’automation peut appeler. Par exemple, si votre contrôle dispose de boutons dotés de gestionnaires d’événements connectés capables de développer ou de réduire le contrôle, ainsi que d’équivalents clavier pour ces mêmes actions, faites en sorte que ces gestionnaires d’événements appellent la même méthode que celle que vous avez appelée à partir des implémentations [**Expand**](https://msdn.microsoft.com/library/windows/apps/BR242570) ou [**Collapse**](https://msdn.microsoft.com/library/windows/apps/BR242569) de l’interface [**IExpandCollapseProvider**](https://msdn.microsoft.com/library/windows/desktop/Ee671242) au sein de l’homologue. Le recours à une méthode logique courante peut également être utile pour s’assurer que les états visuels de votre contrôle sont mis à jour afin d’afficher l’état logique de façon uniforme, quelle que soit la manière dont le comportement a été appelé.

Dans le cadre d’une implémentation classique, les API du fournisseur appellent d’abord [**Owner**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.frameworkelementautomationpeer.owner) pour accéder à l’instance de contrôle au moment de l’exécution. Les méthodes de comportement nécessaires peuvent ensuite être appelées dans cet objet.


```csharp
public class IndexCardAutomationPeer : FrameworkElementAutomationPeer, IExpandCollapseProvider {
    private IndexCard ownerIndexCard;
    public IndexCardAutomationPeer(IndexCard owner) : base(owner)
    {
         ownerIndexCard = owner;
    }
}
```

Le contrôle lui-même qui peut référencer son homologue est une autre implémentation possible. Il s’agit d’un modèle courant si vous déclenchez des événements d’automation à partir du contrôle, car la méthode [**RaiseAutomationEvent**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.raiseautomationevent) est une méthode homologue.

<span id="UI_Automation_events"/>
<span id="ui_automation_events"/>
<span id="UI_AUTOMATION_EVENTS"/>

## <a name="ui-automation-events"></a>Événements UI Automation  

Les événements UIAutomation s’inscrivent dans les catégories suivantes.

| Événement | Description |
|-------|-------------|
| Changement de propriété | Se déclenche quand une propriété dans un élément ou un modèle de contrôle UI Automation change. Par exemple, si un client a besoin de surveiller le contrôle de case à cocher d’une application, il peut s’inscrire pour écouter un événement de changement de propriété dans la propriété [**ToggleState**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.provider.itoggleprovider.togglestate). Lorsque le contrôle de la case à cocher est activé ou désactivé, le fournisseur déclenche l’événement et le client peut agir selon ses besoins. |
| Action d’élément | Se déclenche quand un changement dans l’interface utilisateur résulte d’une activité menée par l’utilisateur ou par programme (par exemple, lorsque vous cliquez sur un bouton ou l’appelez par le biais du modèle **Invoke**). |
| Changement de structure | Se déclenche lorsque la structure de l’arborescence UI Automation change. La structure évolue quand de nouveaux éléments de l’interface utilisateur deviennent visibles, sont masqués ou sont supprimés sur le Bureau. |
| Changement global | Se déclenche lorsque des actions d’intérêt général pour le client sont menées, par exemple lorsque le focus passe d’un élément à un autre ou quand une fenêtre enfant se ferme. Certains événements ne signifient pas nécessairement que l’état de l’interface utilisateur a changé. Par exemple, si l’utilisateur sélectionne un champ de saisie de texte, puis clique sur un bouton pour mettre ce champ à jour, un événement [**TextChanged**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textbox.textchanged) est déclenché même si l’utilisateur n’a pas réellement modifié le texte. Lors du traitement d’un événement, il peut être nécessaire pour une application cliente de vérifier si quelque chose a changé avant d’entreprendre toute action. |

<span id="AutomationEvents_identifiers"/>
<span id="automationevents_identifiers"/>
<span id="AUTOMATIONEVENTS_IDENTIFIERS"/>

### <a name="automationevents-identifiers"></a>Identificateurs AutomationEvents  
Les événements UI Automation sont identifiés par des valeurs [**AutomationEvents**](https://msdn.microsoft.com/library/windows/apps/BR209183). Les valeurs de l’énumération identifient le type d’événement de manière unique.

<span id="Raising_events"/>
<span id="raising_events"/>
<span id="RAISING_EVENTS"/>

### <a name="raising-events"></a>Déclenchement d’événements  
Les clients UI Automation peuvent s’abonner à des événements d’automation. Dans le modèle d’homologue d’automatisation, les homologues des contrôles personnalisés doivent signaler les changements de l’état du contrôle en rapport avec l’accessibilité en appelant la méthode [**RaiseAutomationEvent**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.raiseautomationevent). De même, quand une valeur de propriété UI Automation clé change, les homologues de contrôles personnalisés doivent appeler la méthode [**RaisePropertyChangedEvent**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.raisepropertychangedevent).

L’exemple de code suivant montre comment se procurer l’objet homologue à partir du code de définition de contrôle et comment appeler une méthode pour déclencher un événement à partir de cet homologue. À des fins d’optimisation, le code détermine s’il existe des écouteurs pour ce type d’événement. Le fait de déclencher l’événement et de créer l’objet homologue uniquement lorsqu’il y a des écouteurs permet d’éviter toute surcharge inutile et aide à maintenir la réactivité du contrôle.


```csharp
if (AutomationPeer.ListenerExists(AutomationEvents.PropertyChanged))
{
    NumericUpDownAutomationPeer peer =
        FrameworkElementAutomationPeer.FromElement(nudCtrl) as NumericUpDownAutomationPeer;
    if (peer != null)
    {
        peer.RaisePropertyChangedEvent(
            RangeValuePatternIdentifiers.ValueProperty,
            (double)oldValue,
            (double)newValue);
    }
}
```

<span id="Peer_navigation"/>
<span id="peer_navigation"/>
<span id="PEER_NAVIGATION"/>

## <a name="peer-navigation"></a>Navigation parmi les homologues  
Après avoir trouvé un homologue d’automatisation, le client UI Automation peut naviguer dans la structure d’homologues d’une application en appelant les méthodes [**GetChildren**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getchildren) et [**GetParent**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getparent) de l’objet homologue. La navigation parmi les éléments d’interface utilisateur dans un contrôle est prise en charge par l’implémentation de la méthode [**GetChildrenCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getchildrencore) par l’homologue. Le système UI Automation appelle cette méthode pour créer une arborescence des sous-éléments contenus dans un contrôle, par exemple les éléments de liste d’une zone de liste. La méthode **GetChildrenCore** par défaut dans [**FrameworkElementAutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR242472) traverse l’arborescence visuelle des éléments pour créer l’arborescence d’homologues d’automatisation. Les contrôles personnalisés peuvent substituer cette méthode pour exposer une représentation différente d’éléments enfants aux clients d’automatisation, en renvoyant les homologues d’automatisation d’éléments qui transmettent des informations ou autorisent une interaction utilisateur.

<span id="Native_automation_support_for_text_patterns"/>
<span id="native_automation_support_for_text_patterns"/>
<span id="NATIVE_AUTOMATION_SUPPORT_FOR_TEXT_PATTERNS"/>

## <a name="native-automation-support-for-text-patterns"></a>Prise en charge de l’automation native pour les modèles de texte  
Certains des homologues d’automatisation d’application UWP par défaut offrent une prise en charge des modèles de contrôles pour le modèle de texte ([**PatternInterface.Text**](https://msdn.microsoft.com/library/windows/apps/BR242496)). Toutefois, ils fournissent cette prise en charge par le biais de méthodes natives et les homologues impliqués ne vont pas noter l’interface [**ITextProvider**](https://msdn.microsoft.com/library/windows/apps/BR242627) dans l’héritage (managé). En outre, si un client UI Automation managé ou non managé demande des modèles à l’homologue, il signale la prise en charge du modèle de texte et fournit le comportement pour certaines parties du modèle quand les API clientes sont appelées.

Si vous envisagez de dériver de l’un des contrôles de texte des applications UWP et de créer également un homologue personnalisé qui dérive de l’un des homologues associés au texte, examinez les sections Remarques pour l’homologue pour en savoir plus sur toute prise en charge des modèles au niveau natif. Vous pouvez accéder au comportement de base natif dans votre homologue personnalisé si vous appelez l’implémentation de base à partir des implémentations de l’interface du fournisseur managé, mais il est difficile de modifier les actions de l’implémentation de base, car les interfaces natives sur l’homologue et son contrôle propriétaire ne sont pas exposées. En règle générale, vous devez utiliser les implémentations de base en l’état (appel de la base uniquement) ou remplacer entièrement la fonctionnalité par votre propre code managé et ne pas appeler l’implémentation de base. Ce dernier scénario étant plus abouti, vous devez disposer de bonnes connaissances sur l’infrastructure des services de texte utilisés par votre contrôle pour prendre en charge les exigences d’accessibilité lors de l’utilisation de cette infrastructure.

<span id="AutomationProperties.AccessibilityView"/>
<span id="automationproperties.accessibilityview"/>
<span id="AUTOMATIONPROPERTIES.ACCESSIBILITYVIEW"/>

## <a name="automationpropertiesaccessibilityview"></a>AutomationProperties.AccessibilityView  
En plus de fournir un homologue personnalisé, vous pouvez également ajuster la représentation d’arborescence pour toute instance de contrôle, en définissant la propriété [**AutomationProperties.AccessibilityView**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.automationproperties.accessibilityview) en XAML. Elle n’est pas implémentée dans le cadre d’une classe homologue, mais nous la signalons ici car elle est apparentée à la prise en charge de l’accessibilité soit pour des contrôles personnalisés, soit pour des modèles que vous personnalisez.

Le principal scénario d’utilisation de la propriété **AutomationProperties.AccessibilityView** consiste à omettre délibérément certains contrôles d’un modèle dans les vues UI Automation, car ils ne contribuent pas clairement à l’affichage avec accessibilité de l’ensemble du contrôle. Pour éviter cela, affectez à **AutomationProperties.AccessibilityView** la valeur «Raw».

<span id="Throwing_exceptions_from_automation_peers"/>
<span id="throwing_exceptions_from_automation_peers"/>
<span id="THROWING_EXCEPTIONS_FROM_AUTOMATION_PEERS"/>

## <a name="throwing-exceptions-from-automation-peers"></a>Levée d’exceptions à partir d’homologues d’automation  
Les API que vous implémentez pour la prise en charge de votre homologue d’automation sont autorisées à lever des exceptions. Tout client UI Automation à l’écoute est censé être suffisamment robuste pour continuer après la plupart des exceptions levées. Selon toute vraisemblance, cet écouteur se trouve face à une arborescence Automation comprenant des applications autres que les vôtres. Du point de vue de la conception, il est inacceptable que le client soit rendu complètement hors service si l’une des zones de l’arborescence lève une exception basée sur l’homologue quand le client appelle ses API.

Pour les paramètres qui sont passés à votre homologue, il est acceptable de valider l’entrée, et par exemple de lever [**ArgumentNullException**](https://msdn.microsoft.com/library/windows/apps/system.argumentnullexception) si la valeur **null** a été passée et qu’il ne s’agit pas d’une valeur valide pour votre implémentation. Cependant, si votre homologue effectue des opérations ultérieures, souvenez-vous que les interactions de l’homologue avec le contrôle d’hébergement sont de nature asynchrone. Tout ce que fait un homologue ne bloque pas nécessairement le thread d’interface utilisateur dans le contrôle (ce qui est probablement mieux ainsi). Ainsi, un objet qui était disponible ou qui disposait de certaines propriétés au moment de la création de l’homologue ou lors du premier appel de la méthode d’homologue d’automation peut avoir à présent un état de contrôle différent. Dans ces cas-là, un fournisseur peut lever deux exceptions dédiées :

* Levez l’exception [**ElementNotAvailableException**](https://msdn.microsoft.com/library/system.windows.automation.elementnotavailableexception) si vous êtes dans l’incapacité d’accéder au propriétaire de l’homologue ou à un élément d’homologue lié en fonction des informations d’origine passées à votre API. Par exemple, un homologue peut tenter d’exécuter ses méthodes, mais dans le même temps, suite à la fermeture d’une boîte de dialogue modale par exemple, le propriétaire a été supprimé de l’interface utilisateur. Pour un client non-.NET, cela correspond à [**UIA\_E\_ELEMENTNOTAVAILABLE**](https://msdn.microsoft.com/library/windows/desktop/Ee671218).
* Levez [**ElementNotEnabledException**](https://msdn.microsoft.com/library/system.windows.automation.elementnotenabledexception) si le propriétaire est encore présent, mais que celui-ci est dans un mode tel que [**IsEnabled**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.control.isenabled)`=`**false** qui bloque une partie des modifications par programme spécifiques que votre homologue cherche à accomplir. Pour un client non-.NET, cela correspond à [**UIA\_E\_ELEMENTNOTENABLED**](https://msdn.microsoft.com/library/windows/desktop/Ee671218).

Au-delà de cet aspect, les homologues doivent être relativement conservateurs concernant les exceptions qu’ils lèvent dans le cadre de la prise en charge des homologues. La plupart des clients ne sont pas en mesure de gérer les exceptions des homologues ni de les transformer en choix exploitables proposés aux utilisateurs lors de l’interaction avec le client. Par conséquent, il est parfois plus judicieux d’employer un « no-op » et d’intercepter les exceptions sans les lever une nouvelle fois dans vos implémentations d’homologue que de lever des exceptions chaque fois que l’homologue ne parvient pas à faire quelque chose. Tenez aussi compte du fait que la plupart des clients UI Automation ne sont pas écrits en code managé. La plupart d’entre eux sont écrits en COM et vérifient simplement la présence de **S\_OK** dans un élément **HRESULT** chaque fois qu’ils appellent une méthode d’un client UI Automation qui finit par accéder à votre homologue.

<span id="related_topics"/>

## <a name="related-topics"></a>Rubriques connexes  
* [Accessibilité](accessibility.md)
* [Exemple d’accessibilité XAML](http://go.microsoft.com/fwlink/p/?linkid=238570)
* [**FrameworkElementAutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR242472)
* [**AutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR209185)
* [**OnCreateAutomationPeer**](https://msdn.microsoft.com/ibrary/windows/apps/windows.ui.xaml.uielement.oncreateautomationpeer)
* [Modèles de contrôle et interfaces](control-patterns-and-interfaces.md)
