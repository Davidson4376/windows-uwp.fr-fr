---
Description: L’article suivant décrit toutes les propriétés et tous les éléments au sein de la charge utile XML du contenu de notification toast.
title: Schéma XML du contenu du toast
ms.assetid: AF49EFAC-447E-44C3-93C3-CCBEDCF07D22
label: Toast content XML schema
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 6b9535cd8c2dd82b0c209919080df9a88bb80ccc
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57612774"
---
# <a name="toast-content-xml-schema"></a>Schéma XML du contenu du toast

 

L’article suivant décrit toutes les propriétés et tous les éléments au sein de la charge utile XML du contenu du toast.

Dans les schémas XML suivants, le suffixe « ? » signifie qu’un attribut est facultatif.

## <a name="ltvisualgt-and-ltaudiogt"></a>&lt;visual&gt; et &lt;audio&gt;

```
<toast launch? duration? activationType? scenario? >
  <visual lang? baseUri? addImageQuery? >
    <binding template? lang? baseUri? addImageQuery? >
      <text lang? hint-maxLines? >content</text>
      <image src placement? alt? addImageQuery? hint-crop? />
      <group>
        <subgroup hint-weight? hint-textStacking? >
          <text />
          <image />
        </subgroup>
      </group>
    </binding>
  </visual>
  <audio src? loop? silent? />
</toast>
```

**Attributs dans &lt;toast&gt;**

launch?

-   launch? = chaîne
-   Il s’agit d’un attribut facultatif.
-   Chaîne transmise à l’application lorsqu’elle est activée par la notification toast.
-   Selon la valeur d’activationType, cette valeur peut être reçue par l’application au premier plan, à l’intérieur de la tâche en arrière-plan ou par une autre application lancée par protocole à partir de l’application d’origine.
-   L’application définit le format et le contenu de cette chaîne pour son propre usage.
-   Lorsque l’utilisateur appuie ou clique sur la notification toast pour lancer l’application qui lui est associée, la chaîne de lancement précise le contexte à l’application pour permettre à cette dernière de présenter à l’utilisateur une vue adaptée au contenu de la notification toast plutôt qu’une vue par défaut.
-   Si l’activation découle d’un clic par l’utilisateur sur une action plutôt que dans le corps de la notification toast, le développeur récupère les « arguments » prédéfinis dans cette balise &lt;action&gt; à la place de la chaîne « launch » prédéfinie dans la balise &lt;toast&gt;.

duration?

-   duration? = "short|long"
-   Il s’agit d’un attribut facultatif. La valeur par défaut est « short ».
-   Cet attribut n’est proposé que pour des scénarios spécifiques et pour la base de données de compatibilité des applications (AppCompat). Vous n’en avez plus besoin pour le scénario d’alarme.
-   Nous vous déconseillons d’utiliser cette propriété.

activationType?

-   activationType? = "foreground | background | protocol | system"
-   Il s’agit d’un attribut facultatif.
-   La valeur par défaut est « foreground ».

scenario?

-   scenario? = "default | alarm | reminder | incomingCall"
-   Il s’agit d’un attribut facultatif, dont la valeur par défaut est « default ».
-   Vous n’en avez pas besoin, sauf si votre scénario consiste à présenter une alarme, un rappel ou un appel entrant.
-   Ne l’utilisez pas dans le seul but d’assurer la persistance de votre notification à l’écran.

**Attributs dans &lt;visual&gt;**

lang?

-   Pour plus de détails sur cet attribut facultatif, voir [cet article concernant le schéma des éléments](https://msdn.microsoft.com/library/windows/apps/br230847).

baseUri?

-   Pour plus de détails sur cet attribut facultatif, voir [cet article concernant le schéma des éléments](https://msdn.microsoft.com/library/windows/apps/br230847).

addImageQuery?

-   Pour plus de détails sur cet attribut facultatif, voir [cet article concernant le schéma des éléments](https://msdn.microsoft.com/library/windows/apps/br230847).

**Attributs dans &lt;liaison&gt;**

template?

-   \[Important\] modèle ? = "ToastGeneric"
-   Si vous avez recours à l’une des nouvelles fonctionnalités des notifications adaptatives et interactives, vérifiez que vous commencez par utiliser le modèle « ToastGeneric » plutôt que le modèle hérité.
-   Même si l’utilisation des modèles hérités avec les nouvelles actions fonctionne encore, il ne s’agit pas du cas d’utilisation prévu, et nous ne pouvons pas garantir que cette approche continuera de fonctionner à l’avenir.

lang?

-   Pour plus de détails sur cet attribut facultatif, voir [cet article concernant le schéma des éléments](https://msdn.microsoft.com/library/windows/apps/br230847).

baseUri?

-   Pour plus de détails sur cet attribut facultatif, voir [cet article concernant le schéma des éléments](https://msdn.microsoft.com/library/windows/apps/br230847).

addImageQuery?

-   Pour plus de détails sur cet attribut facultatif, voir [cet article concernant le schéma des éléments](https://msdn.microsoft.com/library/windows/apps/br230847).

**Attributs dans &lt;texte&gt;**

lang?

-   Pour plus de détails sur cet attribut facultatif, voir [cet article concernant le schéma des éléments](https://msdn.microsoft.com/library/windows/apps/br230847).

**Attributs dans &lt;image&gt;**

src

-   Pour plus de détails sur cet attribut obligatoire, voir [cet article concernant le schéma des éléments](https://msdn.microsoft.com/library/windows/apps/br230844).

placement?

-   placement? = "inline" | "appLogoOverride"
-   Cet attribut est facultatif.
-   Il spécifie l’endroit où cette image s’affichera.
-   L’élément « inline » indique de placer l’image dans le corps de la notification toast, sous le texte ; L’élément « appLogoOverride » indique de remplacer l’icône de l’application (qui s’affiche dans le coin supérieur gauche de la notification toast).
-   Vous ne pouvez définir qu’une seule image par attribut « placement ».

alt?

-   Pour plus de détails sur cet attribut facultatif, voir [cet article concernant le schéma des éléments](https://msdn.microsoft.com/library/windows/apps/br230844).

addImageQuery?

-   Pour plus de détails sur cet attribut facultatif, voir [cet article concernant le schéma des éléments](https://msdn.microsoft.com/library/windows/apps/br230844).

hint-crop?

-   hint-crop? = "none" | "circle"
-   Cet attribut est facultatif.
-   L’élément « none » est la valeur par défaut qui signifie aucun rognage.
-   L’élément « circle » rogne l’image pour lui donner une forme circulaire. Utilisez cette valeur pour les images de profil d’un contact, les images d’une personne, etc.

**Attributs dans &lt;audio&gt;**

src?

-   Pour plus de détails sur cet attribut facultatif, voir [cet article concernant le schéma des éléments](https://msdn.microsoft.com/library/windows/apps/br230842).

loop?

-   Pour plus de détails sur cet attribut facultatif, voir [cet article concernant le schéma des éléments](https://msdn.microsoft.com/library/windows/apps/br230842).

silent?

-   Pour plus de détails sur cet attribut facultatif, voir [cet article concernant le schéma des éléments](https://msdn.microsoft.com/library/windows/apps/br230842).

## <a name="schemas-ltactiongt"></a>Schémas : &lt;action&gt;


Dans les schémas XML suivants, le suffixe « ? » signifie qu’un attribut est facultatif.

```
<toast>
  <visual>
  </visual>
  <audio />
  <actions>
    <input id type title? placeHolderContent? defaultInput? >
      <selection id content />
    </input>
    <action content arguments activationType? imageUri? hint-inputId />
  </actions>
</toast>
```

**Attributs dans &lt;d’entrée&gt;**

id

-   id = chaîne
-   Cet attribut est obligatoire.
-   L’attribut « id » est obligatoire et permet aux développeurs de récupérer les entrées utilisateur une fois l’application activée (au premier plan ou en arrière-plan).

type

-   type = "text | selection"
-   Cet attribut est obligatoire.
-   Il permet de spécifier une entrée de texte ou une entrée effectuée à partir d’une liste de sélections prédéfinies.
-   Sur les appareils mobiles et de bureau, cet attribut sert à indiquer si vous souhaitez créer une entrée de zone de texte ou une entrée de zone de liste.

title?

-   title? = chaîne
-   L’attribut « title » est facultatif et permet aux développeurs de spécifier un titre pour l’entrée à afficher par les interpréteurs de commande lorsqu’il existe une affordance.
-   Sur les appareils mobiles et de bureau, ce titre apparaîtra au-dessus de l’entrée.

placeHolderContent?

-   placeHolderContent? = chaîne
-   L’attribut « placeHolderContent » est facultatif et constitue le texte d’information grisé d’une entrée de type texte. Cet attribut est ignoré lorsque l’entrée ne présente pas le type « text ».

defaultInput?

-   defaultInput? = chaîne
-   L’attribut « defaultInput » est facultatif et permet de fournir une valeur d’entrée par défaut.
-   Si l’entrée présente le type « text », cette valeur sera traitée comme une entrée de chaîne.
-   Si l’entrée présente le type « selection », cette valeur doit correspondre à l’identificateur de l’une des sélections disponibles dans les éléments de cette entrée.

**Attributs dans &lt;sélection&gt;**

id

-   Cet attribut est obligatoire. Il permet d’identifier les sélections de l’utilisateur. L’identificateur est renvoyé à votre application.

content

-   Cet attribut est obligatoire. Il fournit la chaîne à afficher pour cet élément de sélection.

**Attributs dans &lt;action&gt;**

content

-   content = chaîne
-   L’attribut « content » est obligatoire. Il fournit la chaîne de texte affichée sur le bouton.

arguments

-   arguments = chaîne
-   L’attribut « arguments » est obligatoire. Il décrit les données définies par l’application que cette dernière peut récupérer par la suite une fois activée par l’utilisateur exécutant cette action.

activationType?

-   activationType? = "foreground | background | protocol | system"
-   L’attribut « activationType » est facultatif et présente la valeur par défaut « foreground ».
-   Il décrit le type d’activation généré par cette action : au premier plan, en arrière-plan, lancement d’une autre application par le biais d’un lancement par protocole ou appel d’une action système.

imageUri?

-   imageUri? = chaîne
-   L’attribut « imageUri » est facultatif et permet de fournir une icône d’image pour cette action à afficher dans le bouton avec le contenu de texte.

hint-inputId

-   hint-inputId = chaîne
-   L’attribut « hint-inpudId » est obligatoire. Il est spécifiquement destiné au scénario de réponse rapide.
-   Cette valeur doit correspondre à l’identificateur de l’élément d’entrée que vous souhaitez associer.
-   Sur les appareils mobiles et de bureau, cet attribut placera le bouton juste à côté de la zone d’entrée.

## <a name="attributes-for-system-handled-actions"></a>Attributs pour les actions gérées par le système


Le système peut gérer les actions de répétition et de masquage des notifications si vous ne voulez pas que votre application traite la répétition/replanification des notifications sous la forme d’une tâche en arrière-plan. Les actions gérées par le système peuvent être combinées (ou spécifiées individuellement), mais nous vous déconseillons d’implémenter une action de répétition sans une action de masquage.

Liste déroulante de commandes système : SnoozeAndDismiss

```
<toast>
  <visual>
  </visual>
  <actions hint-systemCommands="SnoozeAndDismiss" />
</toast>
```

Actions gérées par le système individuelles

```
<toast>
  <visual>
  </visual>
  <actions>
  <input id="snoozeTime" type="selection" defaultInput="10">
    <selection id="5" content="5 minutes" />
    <selection id="10" content="10 minutes" />
    <selection id="20" content="20 minutes" />
    <selection id="30" content="30 minutes" />
    <selection id="60" content="1 hour" />
  </input>
  <action activationType="system" arguments="snooze" hint-inputId="snoozeTime" content=""/>
  <action activationType="system" arguments="dismiss" content=""/>
  </actions>
</toast>
```

Pour construire des actions de répétition et de masquage individuelles, procédez comme suit :

-   Spécifiez : activationType = "system".
-   Spécifiez : arguments = "snooze" | "dismiss".
-   Spécifiez le contenu :
    -   Si vous souhaitez afficher les chaînes localisées de « snooze » et de « dismiss » sur les actions, spécifiez le contenu comme étant une chaîne vide : &lt;action content = ""/&gt;
    -   Si vous voulez définir une chaîne personnalisée, fournissez simplement sa valeur : &lt;action content="Me le rappeler ultérieurement" /&gt;
-   Spécifiez l’entrée :
    -   Si vous ne voulez pas que l’utilisateur sélectionne un intervalle de répétition, mais souhaitez simplement que votre notification se répète une seule fois pendant un intervalle de temps défini par le système (et cohérent dans l’ensemble du système d’exploitation), ne construisez aucun élément &lt;input&gt;.
    -   Si vous voulez fournir des sélections d’intervalle de répétition :
        -   Spécifiez l’attribut « hint-inputId » dans l’action de répétition.
        -   Faites correspondre l’identificateur de l’entrée avec la valeur de l’attribut « hint-inputId » de l’action de répétition : &lt;input id="snoozeTime"&gt;&lt;/input&gt;&lt;action hint-inputId="snoozeTime"/&gt;
        -   Spécifiez l’identificateur de sélection comme étant un entier non négatif (nonNegativeInteger) qui représente l’intervalle de répétition en minutes : &lt;selection id="240" /&gt; signifie une répétition pendant 4 heures.
        -   Assurez-vous que la valeur de l’attribut « defaultInput » dans &lt;input&gt; correspond à l’un des identificateurs des éléments enfants &lt;selection&gt;.
        -   Fournissez jusqu’à 5 valeurs &lt;selection&gt;.