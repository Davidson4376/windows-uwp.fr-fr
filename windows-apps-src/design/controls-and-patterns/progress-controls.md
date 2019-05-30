---
Description: Un contrôle de progression offre un retour à l’utilisateur lorsqu’une longue opération est en cours.
title: Recommandations en matière de contrôles de progression
ms.assetid: FD53B716-C43D-408D-8B07-522BC1F3DF9D
label: Progress controls
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: kisai
design-contact: jeffarn
dev-contact: mitra
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: a409c4b940ad0e194428981f536823d880e56302
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66364490"
---
# <a name="progress-controls"></a>Contrôles de progression

 

Un contrôle de progression offre un retour à l’utilisateur lorsqu’une longue opération est en cours. Cela peut signifier que l’utilisateur ne peut pas interagir avec l’application lorsque l’indicateur de progression est visible et peut également indiquer le temps d’attente en fonction de l’indicateur utilisé.

> **API importantes** : [Classe ProgressBar](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ProgressBar), [IsIndeterminate propriété](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.progressbar.isindeterminate), [ProgressRing classe](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ProgressRing), [propriété IsActive](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.progressring.isactive)

## <a name="types-of-progress"></a>Types de progression

Deux contrôles permettent d’indiquer à l’utilisateur qu’une opération est en cours d’exécution : ProgressBar ou ProgressRing.

-   L’état *déterminé* du contrôle ProgressBar indique le pourcentage d’achèvement d’une tâche. Ce contrôle doit être utilisé au cours d’une opération dont la durée est connue, mais dont sa progression ne doit pas bloquer l’interaction de l’utilisateur avec l’application.
-   L’état *indéterminé* de ProgressBar indique qu’une opération est en cours, qu’elle ne bloque pas l’interaction de l’utilisateur avec l’application et que son heure de fin est inconnue.
-   Seul le contrôle ProgressRing possède un état *indéterminé* et doit être utilisé quand toute interaction de l’utilisateur est bloquée jusqu’à la fin de l’opération.

De plus, un contrôle de progression est en lecture seule et n’est pas interactif. Ce qui signifie que l’utilisateur ne peut pas appeler ou utiliser ces contrôles directement.

![États ProgressBar](images/ProgressBar_TwoStates.png)

*Haut en bas - ProgressBar indéterminé et un ProgressBar déterminée*

![État ProgressRing](images/ProgressRing_SingleState.png)

*Un ProgressRing indéterminé*

## <a name="examples"></a>Exemples

<table>
<th align="left">Galerie de contrôles XAML<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si vous disposez de l'application <strong style="font-weight: semi-bold">Galerie de contrôles XAML</strong>, cliquez ici pour ouvrir l’application et voir l'objet <a href="xamlcontrolsgallery:/item/ProgressBar">ProgressBar</a> ou <a href="xamlcontrolsgallery:/item/ProgressRing">ProgressRing</a> en action.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenir l’application Galerie de contrôles XAML (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtenir le code source (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="when-to-use-each-control"></a>Quand utiliser chaque contrôle

Il n’est pas toujours évident de savoir quel contrôle ou quel état (déterminé ou indéterminé) utiliser lorsque vous essayez d’indiquer que quelque chose se produit. Parfois, une tâche est suffisamment évidente pour ne pas nécessiter de contrôle de progression, et parfois, même si un contrôle de progression est utilisé, une ligne de texte reste nécessaire afin d’indiquer à l’utilisateur le type d’opération en cours.

### <a name="progressbar"></a>Barre de progression
-   **Le contrôle a une durée définie ou la fin prévisible ?**

    Utilisez un contrôle ProgressBar déterminé et mettez à jour le pourcentage ou la valeur en conséquence.

-   **L’utilisateur peut continuer sans avoir à surveiller la progression de l’opération ?**

    Lorsqu’un contrôle ProgressBar est en cours d’utilisation, l’interaction est non modale, ce qui signifie généralement que l’utilisateur n’est pas bloqué tant que cette opération n’est pas terminée et peut continuer à utiliser les autres fonctionnalités de l’application dans l’intervalle.

-   **Keywords**

    Si vous pensez que votre fonctionnement tourne autour de ces mots-clés ou si vous affichez du texte au cours de la progression qui correspond aux mots-clés suivants, envisagez d’utiliser un contrôle ProgressBar :

    - *Chargement en cours...*
    - *Récupération*
    - *Utilisation...*

### <a name="progressring"></a>ProgressRing

-   **L’opération entraîne l’utilisateur d’attendre pour continuer ?**

    Si une opération nécessite une interaction complète (ou très importante) avec l’application qui implique d’attendre son achèvement, le contrôle ProgressRing est le meilleur choix. Le contrôle ProgressRing est utilisé pour les interactions modales, ce qui signifie que l’utilisateur est bloqué jusqu’à la fin de la progression.

-   **L’application attend que l’utilisateur effectuer une tâche ?**

    Si tel est le cas, utilisez un contrôle ProgressRing, censé indiquer une durée d’attente inconnue pour l’utilisateur.

-   **Keywords**

    Si vous pensez que votre fonctionnement tourne autour de ces mots-clés ou si vous affichez du texte au cours de la progression qui correspond aux mots-clés suivants, envisagez d’utiliser un contrôle ProgressRing :

    - *L’actualisation*
    - *Connexion...*
    - *Connexion en cours...*

### <a name="no-progress-indication-necessary"></a>Aucune indication de progression n’est nécessaire
-   **L’utilisateur n’a besoin de savoir que quelque chose qui se passe ?**

    Par exemple, si l’application exécute un téléchargement en arrière-plan et que l’utilisateur n’en est pas l’initiateur, il n’est pas nécessaire d’informer l’utilisateur.

-   **L’opération n’est une activité en arrière-plan qui ne bloque pas l’activité des utilisateurs et présente un intérêt minimal (mais toujours certaines) à l’utilisateur ?**

    Utilisez du texte lorsque votre application effectue des tâches qui n’ont pas à être visibles tout le temps, mais dont vous souhaitez quand même indiquer le statut.

-   **L’utilisateur seulement se soucie l’achèvement de l’opération ?**

    Il est parfois préférable d’afficher un avis seulement lorsque l’opération est terminée ou de donner un élément visuel montrant que l’opération a été exécutée immédiatement et d’exécuter les finitions en arrière-plan.

## <a name="progress-controls-best-practices"></a>Meilleures pratiques en termes de contrôles de progression

Il est parfois préférable de voir des représentations visuelles indiquant quand et comment utiliser ces différents contrôles de progression :

**ProgressBar - déterminée**

![Exemple de contrôle ProgressBar déterminé.](images/PB_DeterminateExample.png)

Le premier exemple est le contrôle ProgressBar déterminé. Un contrôle ProgressBar est préférable lorsque la durée de l’opération est connue, lors de l’installation, lors du téléchargement, lors de la configuration, etc.

**ProgressBar - indéterminé**

![Exemple de contrôle ProgressBar indéterminé.](images/PB_IndeterminateExample.png)

Utilisez un contrôle ProgressBar indéterminé lorsque la durée de l’opération est inconnue. Les contrôles ProgressBar indéterminés s’appliquent également en cas de remplissage d’une liste virtualisée et la création d’une transition visuelle fluide d’un contrôle ProgressBar indéterminé vers un contrôle ProgressBar déterminé.

-   **Est l’opération dans une collection virtualisée ?**

    Si tel est le cas, ne placez pas d’indicateur de progression dès l’affichage de chaque élément de liste. Au lieu de cela, utilisez un contrôle ProgressBar et placez-le en haut de la collection d’éléments en cours de chargement, afin d’indiquer que les éléments sont en cours de recherche.

**ProgressRing - indéterminé**

![Exemple de contrôle ProgressRing indéterminé](images/PR_IndeterminateExample.png)

Le contrôle ProgressRing indéterminé est utilisé lorsque toute autre interaction de l’utilisateur avec l’application est bloquée, ou lorsque l’application attend la saisie de l’utilisateur pour continuer. L’exemple « Connexion en cours... » ci-dessus est un scénario idéal pour le contrôle ProgressRing. L’utilisateur ne peut pas continuer à utiliser l’application tant que la connexion n’est pas terminée.

## <a name="customizing-a-progress-control"></a>Personnalisation d’un contrôle de progression

Les deux contrôles de progression sont assez simples ; toutefois, certaines fonctionnalités visuelles des contrôles ne sont pas faciles à personnaliser.

**Dimensionnement du ProgressRing**

Le contrôle ProgressRing peut être agrandi à la taille que vous souhaitez, mais ne peut pas être réduit à moins de 20x20epx. Pour redimensionner un contrôle ProgressRing, vous devez définir sa hauteur et sa largeur. Si seule la hauteur ou largeur est définie, le contrôle suppose un dimensionnement minimal (20x20epx). À l’inverse, si la hauteur et la largeur sont définies sur deux tailles différentes, la plus d’entre elles sera prise en considération.
Pour s’assurer que le contrôle ProgressRing est approprié à vos besoins, définissez la hauteur et la largeur sur la même valeur :

```XAML
<ProgressRing Height="100" Width="100"/>
```

Pour rendre votre contrôle ProgressRing visible et l’animer, définissez la propriété IsActive sur True :

```XAML
<ProgressRing IsActive="True" Height="100" Width="100"/>
```

```C#
progressRing.IsActive = true;
```

**Les contrôles de progression de la coloration**

Par défaut, la couleur principale des contrôles de progression est définie sur la couleur d’accentuation du système. Pour modifier cette couleur,, il suffit de modifier la propriété de premier plan sur chaque contrôle.

```XAML
<ProgressRing IsActive="True" Height="100" Width="100" Foreground="Blue"/>
<ProgressBar Width="100" Foreground="Green"/>
```

La modification de la couleur de premier plan du contrôle de progression a pour effet de modifier la couleur des points. La propriété de premier plan du contrôle ProgressBar change la couleur de remplissage de la barre. Pour modifier la partie vide de la barre, il suffit de remplacer la propriété d’arrière-plan.

**Affichage d’un curseur d’attente**

Il est parfois préférable d’afficher brièvement un curseur d’attente lorsque l’application ou l’opération a besoin d’un temps de réflexion, et que vous devez indiquer à l’utilisateur que l’application ou la zone où le curseur d’attente est visible ne doit faire l’objet d’aucune interaction tant que ce dernier est affiché.

```C#
Window.Current.CoreWindow.PointerCursor = new Windows.UI.Core.CoreCursor(Windows.UI.Core.CoreCursorType.Wait, 10);
```

## <a name="get-the-sample-code"></a>Obtenir l’exemple de code

- [Exemples de la Galerie de contrôles XAML](https://github.com/Microsoft/Xaml-Controls-Gallery) - Affichez tous les contrôles XAML dans un format interactif.

## <a name="related-articles"></a>Articles connexes

- [Classe ProgressBar](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ProgressBar)
- [Classe de ProgressRing](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ProgressRing)

**Pour les développeurs (XAML)**
- [Ajout de contrôles de progression](https://docs.microsoft.com/previous-versions/windows/apps/hh780651(v=win.10))
- [Comment créer une barre de progression indéterminée personnalisé pour Windows Phone](https://go.microsoft.com/fwlink/p/?LinkID=392426)
