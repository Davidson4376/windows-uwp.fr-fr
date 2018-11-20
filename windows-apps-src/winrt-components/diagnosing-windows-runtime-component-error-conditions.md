---
author: msatranjr
title: Diagnostic des conditions d’erreur d’un composant Windows Runtime
description: Cet article fournit des informations supplémentaires sur les restrictions applicables aux composants WindowsRuntime écrits en code managé.
ms.assetid: CD0D0E11-E68A-411D-B92E-E9DECFDC9599
ms.author: misatran
ms.date: 02/08/2017
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 833dd0a6447e9d0bb49c21a18d17bd7b0dc3455d
ms.sourcegitcommit: cbe7cf620622a5e4df7414f9e38dfecec1cfca99
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/20/2018
ms.locfileid: "7421898"
---
# <a name="diagnosing-windows-runtime-component-error-conditions"></a>Diagnostic des conditions d’erreur d’un composant WindowsRuntime




Cet article fournit des informations supplémentaires sur les restrictions applicables aux composants WindowsRuntime écrits en code managé. Il se base sur les informations fournies dans les messages d’erreur à partir de [Winmdexp.exe (outil d’exportation de métadonnées Windows Runtime)](https://msdn.microsoft.com/library/hh925576.aspx) et complète les informations sur les restrictions fournies dans [Création de composants Windows Runtime en C# et Visual Basic](creating-windows-runtime-components-in-csharp-and-visual-basic.md).

Cet article ne couvre pas toutes les erreurs. Les erreurs décrites ici sont regroupées par catégorie générale, et chaque catégorie inclut un tableau des messages d’erreur associés. Recherchez le texte du message (en omettant les valeurs spécifiques des espaces réservés) ou le numéro du message. Si vous ne trouvez pas les informations dont vous avez besoin ici, veuillez nous aider à améliorer la documentation à l’aide du bouton de commentaire à la fin de cet article. Fournissez le message d’erreur. Autrement, vous pouvez déposer un bogue sur le site web Microsoft Connect.

## <a name="error-message-for-implementing-async-interface-provides-incorrect-type"></a>Message d’erreur pour l’implémentation d’interface asynchrone qui fournit un type incorrect


Les composants Windows Runtime managés ne peuvent pas implémenter les interfaces de plateforme Windows universelle (UWP) qui représentent des opérations ou actions asynchrones : ([IAsyncAction](https://msdn.microsoft.com/library/br205781.aspx), [IAsyncActionWithProgress&lt;TProgress&gt;](https://msdn.microsoft.com/library/br205784.aspx), [IAsyncOperation&lt;TResult&gt;](https://msdn.microsoft.com/library/windows/apps/br206598.aspx) ou [IAsyncOperationWithProgress&lt;TResult, TProgress&gt;](https://msdn.microsoft.com/library/windows/apps/br206594.aspx)). À la place, le .NET Framework fournit la classe [AsyncInfo](https://msdn.microsoft.com/library/system.runtime.interopservices.windowsruntime.asyncinfo.aspx) pour générer des opérations asynchrones dans les composants Windows Runtime. Le message d’erreur affiché par Winmdexp.exe quand vous essayez d’implémenter une interface asynchrone de façon incorrecte fait référence à cette classe en utilisant son ancien nom, AsyncInfoFactory. Le .NET Framework n’inclut plus la classe AsyncInfoFactory.

| Numéro d’erreur | Texte du message|       
|--------------|-------------|
| WME1084      | Type de «{0}«implémente l’interface asynchrone Windows Runtime»{1}». Les types Windows Runtime ne peuvent pas implémenter les interfaces asynchrones. Veuillez utiliser la classe System.Runtime.InteropServices.WindowsRuntime.AsyncInfoFactory pour générer des opérations asynchrones afin d’exporter vers Windows Runtime. |

> **Remarque**les messages d’erreur qui font référence à Windows Runtime utilisent une ancienne terminologie. On utilise maintenant l’appellation «plateforme Windows universelle (UWP)». Par exemple, les types Windows Runtime sont désormais appelés types UWP.

 

## <a name="missing-references-to-mscorlibdll-or-systemruntimedll"></a>Références manquantes à mscorlib.dll ou System.Runtime.dll


Ce problème se produit uniquement lorsque vous utilisez Winmdexp.exe à partir de la ligne de commande. Nous vous recommandons d’utiliser l’option /reference pour inclure des références à mscorlib.dll et System.Runtime.dll à partir des assemblys de référence principale du.NET Framework, qui se trouvent dans «%ProgramFiles(x86)%\\Reference Assemblies\\Microsoft\\Framework\\.NETCore\\v4.5" ("%ProgramFiles%\\...» sur un ordinateur 32bits).

| Numéro d’erreur | Texte du message                                                                                                                                     |
|--------------|--------------------------------------------------------------------------------------------------------------------------------------------------|
| WME1009      | Aucune référence n’a été faite à mscorlib.dll. Une référence à ce fichier de métadonnées est requise pour exporter correctement.                               |
| WME1090      | Impossible de déterminer le principal assembly de référence. Assurez-vous que mscorlib.dll et System.Runtime.dll sont référencés à l’aide du commutateur /reference. |

 

## <a name="operator-overloading-is-not-allowed"></a>Surcharge d’opérateur non autorisée


Dans un composant Windows Runtime écrit en code managé, vous ne pouvez pas exposer les opérateurs surchargés sur les types publics.

> **Remarque**dans le message d’erreur, l’opérateur est identifié par son nom de métadonnées, tel qu’op_Addition, op_Multiply, op_ExclusiveOr, op_Implicit (conversion implicite) et ainsi de suite.

 

| Numéro d’erreur | Texte du message                                                                                          |
|--------------|-------------------------------------------------------------------------------------------------------|
| WME1087      | «{0}» est une surcharge d’opérateur. Les types managés ne peuvent pas exposer les surcharges de l’opérateur dans le Windows Runtime. |

 

## <a name="constructors-on-a-class-have-the-same-number-of-parameters"></a>Constructeurs d’une classe ayant le même nombre de paramètres


Dans l’UWP, une classe ne peut avoir qu’un seul constructeur avec un nombre donné de paramètres ; par exemple, vous ne pouvez pas avoir un constructeur ayant un seul paramètre de type **String** et un autre possédant un seul paramètre de type **int** (**Integer** en Visual Basic). La seule solution consiste à utiliser un nombre différent de paramètres pour chaque constructeur.

| Numéro d’erreur | Texte du message                                                                                                                                            |
|--------------|---------------------------------------------------------------------------------------------------------------------------------------------------------|
| WME1099      | Type '{0}«possède plusieurs constructeurs avec»{1}» des arguments. Les types Windows Runtime ne peuvent pas posséder plusieurs constructeurs ayant le même nombre d’arguments. |

 

## <a name="must-specify-a-default-for-overloads-that-have-the-same-number-of-parameters"></a>Spécification obligatoire d’une valeur par défaut pour les surcharges ayant le même nombre de paramètres


Dans l’UWP, les méthodes surchargées peuvent avoir le même nombre de paramètres uniquement si une surcharge est spécifiée en tant que celle par défaut. Voir « Méthodes surchargées » dans [Création de composants Windows Runtime en C# et Visual Basic](creating-windows-runtime-components-in-csharp-and-visual-basic.md).

| Numéro d’erreur | Texte du message                                                                                                                                                                      |
|--------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| WME1059      | Plusieurs {0}-surcharges de paramètre de «{1}. {2}» sont décorées de Windows.Foundation.Metadata.DefaultOverloadAttribute.                                                            |
| WME1085      | Le {0}-surcharges de paramètre de {1}. {2} doit avoir une seule méthode spécifiée en tant que la surcharge par défaut en la décorant avec Windows.Foundation.Metadata.DefaultOverloadAttribute. |

 

## <a name="namespace-errors-and-invalid-names-for-the-output-file"></a>Erreurs d’espace de noms et noms non valides pour le fichier de sortie


Dans la plateforme Windows universelle, tous les types publics dans un fichier de métadonnées Windows (.winmd) doivent se trouver dans un espace de noms qui partage le nom du fichier .winmd, ou dans des sous-espaces de noms du nom de fichier. Par exemple, si votre projet Visual Studio est nommé A.B (autrement dit, votre composant Windows Runtime est A.B.winmd), il peut contenir des classes publiques A.B.Class1 et A.B.C.Class2, mais pas A.Class3 (WME0006) ou D.Class4 (WME1044).

> **Remarque**ces restrictions s’appliquent uniquement aux types publics, non aux types privés utilisés dans votre implémentation.

 

Dans le cas de A.Class3, vous pouvez déplacer Class3 dans un autre espace de noms ou remplacer le nom du composant Windows Runtime par A.winmd. Bien que WME0006 soit un avertissement, vous devez le traiter comme une erreur. Dans l’exemple précédent, le code qui appelle A.B.winmd ne pourra pas localiser A.Class3.

Dans le cas de D.Class4, aucun nom de fichier ne peut contenir à la fois D.Class4 et des classes dans l’espace de noms A.B ; la modification du nom du composant Windows Runtime n’est donc pas possible. Vous pouvez déplacer D.Class4 vers un autre espace de noms ou le placer dans un autre composant Windows Runtime.

Le système de fichiers ne peut pas effectuer la distinction entre majuscules et minuscules; par conséquent, les espaces de noms dont la casse diffère ne sont pas autorisés (WME1067).

Votre composant doit contenir au moins un type **public sealed** (**Public NotInheritable** en Visual Basic). Si ce n’est pas le cas, vous obtiendrez WME1042 ou WME1043, selon si votre composant contient ou non des types privés.

Un type dans un composant Windows Runtime ne peut pas avoir un nom identique à un espace de noms (WME1068).

> **Attention**si vous appelez Winmdexp.exe directement et n’utilisez pas l’option /out pour spécifier un nom pour votre composant Windows Runtime, Winmdexp.exe essaie de générer un nom qui inclut tous les espaces de noms dans le composant. Le fait de renommer les espaces de noms peut modifier le nom de votre composant.

 

| Numéro d’erreur | Texte du message                                                                                                                                                                                                                                                                                                                                             |
|--------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| WME0006      | «{0}» n’est pas un nom de fichier winmd valide pour cet assembly. Tous les types d’un fichier de métadonnées Windows doivent exister dans un sous-espace de noms de l’espace de noms impliqué par le nom de fichier. Les types qui n’existent pas dans un sous-espace de noms ne peuvent pas être localisés au moment de l’exécution. Dans cet assembly, le plus petit espace de noms commun pouvant faire Office de nom de fichier est «{1}». |
| WME1042      | Le module d’entrée doit contenir au moins un type public se trouvant à l’intérieur d’un espace de noms.                                                                                                                                                                                                                                                                   |
| WME1043      | Le module d’entrée doit contenir au moins un type public se trouvant à l’intérieur d’un espace de noms. Les seuls types présents dans des espaces de noms sont privés.                                                                                                                                                                                                               |
| WME1044      | Un type public possède un espace de noms («{1}») qui ne partage aucun préfixe commun avec d’autres espaces de noms («{0}»). Tous les types d’un fichier de métadonnées Windows doivent exister dans un sous-espace de noms de l’espace de noms impliqué par le nom de fichier.                                                                                                                              |
| WME1067      | Les noms de Namespace ne peuvent pas différer uniquement par la casse: «{0}','{1}».                                                                                                                                                                                                                                                                                                |
| WME1068      | Type de «{0}«ne peut pas d’avoir le même nom que l’espace de noms»{1}».                                                                                                                                                                                                                                                                                                 |

 

## <a name="exporting-types-that-arent-valid-universal-windows-platform-types"></a>Exportation des types qui ne sont pas des types de plateforme Windows universelle valides


L’interface publique de votre composant doit exposer uniquement les types UWP. Toutefois, le .NET Framework fournit des mappages pour un certain nombre de types couramment utilisés qui sont légèrement différents dans le .NET Framework et l’UWP. Cela permet au développeur de .NET Framework de travailler avec des types familiers au lieu d’en apprendre de nouveaux. Vous pouvez utiliser ces types .NET Framework mappés dans l’interface publique de votre composant. Voir « Déclaration des types dans les composants Windows Runtime » et « Passage de types Windows Runtime au code managé » dans [Création de composants Windows Runtime en C# et Visual Basic](creating-windows-runtime-components-in-csharp-and-visual-basic.md) et [Mappages .NET Framework des types Windows Runtime](net-framework-mappings-of-windows-runtime-types.md).

Bon nombre de ces mappages sont des interfaces. Par exemple, [IList&lt;T&gt;](https://msdn.microsoft.com/library/5y536ey6.aspx) mappe vers l’interface UWP [IVector&lt;T&gt;](https://msdn.microsoft.com/library/windows/apps/br206631.aspx). Si vous utilisez List&lt;string&gt; (`List(Of String)` en Visual Basic) au lieu de IList&lt;string&gt; en tant que type de paramètre, Winmdexp.exe fournit une liste de possibilités qui comprend toutes les interfaces mappées implémentées par List&lt;T&gt;. Si vous utilisez des types génériques imbriqués, tels que List&lt;Dictionary&lt;int, string&gt;&gt; (List(Of Dictionary(Of Integer, String)) en Visual Basic), Winmdexp.exe offre des choix pour chaque niveau d’imbrication. Ces listes peuvent considérablement s’allonger.

En général, le meilleur choix est l’interface qui est la plus proche du type. Par exemple, pour Dictionary&lt;int, string&gt;, le meilleur choix est sans doute IDictionary&lt;int, string&gt;.

> **Important**JavaScript utilise l’interface qui s’affiche en premier dans la liste des interfaces implémentées par un type managé. Par exemple, si vous retournez Dictionary&lt;int, string&gt; au code JavaScript, il apparaît comme IDictionary&lt;int, string&gt;, quelle que soit l’interface que vous spécifiez comme type de retour. Cela signifie que si la première interface n’inclut pas un membre qui apparaît sur les interfaces ultérieures, ce membre n’est pas visible pour JavaScript.

> **Attention**Évitez d’utiliser les interfaces non génériques [IList](https://msdn.microsoft.com/library/system.collections.ilist.aspx) et [IEnumerable](https://msdn.microsoft.com/library/system.collections.ienumerable.aspx) si votre composant doit être utilisé par JavaScript. Ces interfaces mappent vers [IBindableVector](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.interop.ibindablevector.aspx) et [IBindableIterator](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.interop.ibindableiterator.aspx), respectivement. Elles prennent en charge la liaison pour les contrôles XAML et sont invisibles dans JavaScript. JavaScript émet l’erreur d’exécution « La fonction “X” a une signature non valide et ne peut pas être appelée ».

 

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Numéro d’erreur</th>
<th align="left">Texte du message</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">WME1033</td>
<td align="left">Méthode «{0}«possède le paramètre»{1}«de type»{2}». «{2}» n’est pas un type de paramètre Windows Runtime valide.</td>
</tr>
<tr class="even">
<td align="left">WME1038</td>
<td align="left">Méthode «{0}«a un paramètre de type»{1}» dans sa signature. Bien que ce type ne soit pas un type Windows Runtime valide, il implémente les interfaces qui sont des types Windows Runtime valides. Modifiez la signature de méthode pour utiliser l’un des types suivants à la place: «{2}».</td>
</tr>
<tr class="odd">
<td align="left">WME1039</td>
<td align="left"><p>Méthode «{0}«a un paramètre de type»{1}» dans sa signature. Bien que ce type générique ne soit pas un type Windows Runtime valide, ce type ou ses paramètres génériques implémentent les interfaces qui sont des types Windows Runtime valides. {2}</p>
> **Remarque**pour {2}, Winmdexp.exe ajoute une liste d’alternatives, par exemple «le type ' System.Collections.Generic.List&lt;T&gt;» dans la signature de méthode sur l’une des options suivantes types:» System.Collections.Generic.IList&lt;T&gt;, System.Collections.Generic.IReadOnlyList&lt;T&gt;, System.Collections.Generic.IEnumerable&lt;T&gt;«.»
</td>
</tr>
<tr class="even">
<td align="left">WME1040</td>
<td align="left">Méthode «{0}«a un paramètre de type»{1}» dans sa signature. Au lieu d’utiliser un type de tâche managé, utilisez Windows.Foundation.IAsyncAction, Windows.Foundation.IAsyncOperation ou l’une des autres interfaces asynchrones Windows Runtime. Le modèle .NET standard await s’applique également à ces interfaces. Pour plus d’informations sur la conversion d’objets de tâches managées en interfaces asynchrones Windows Runtime, voir System.Runtime.InteropServices.WindowsRuntime.AsyncInfo.</td>
</tr>
</tbody>
</table>

 

## <a name="structures-that-contain-fields-of-disallowed-types"></a>Structures contenant des champs de types non autorisés


Dans l’UWP, une structure peut contenir uniquement des champs, et seules les structures peuvent contenir des champs. Ces champs doivent être publics. Les types de champ valides incluent des énumérations, des structures et des types primitifs.

| Numéro d’erreur | Texte du message                                                                                                                                                                                                                                                            |
|--------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| WME1060      | Structure de «{0}«possède le champ»{1}«de type»{2}». «{2}» n’est pas un type de champ Windows Runtime valide. Les champs d’une structure Windows Runtime doivent uniquement avoir la valeur UInt8, Int16, UInt16, Int32, UInt32, Int64, UInt64, Single, Double, Boolean, String, Enum, ou bien correspondre à une structure. |

 

## <a name="restrictions-on-arrays-in-member-signatures"></a>Restrictions sur les tableaux dans les signatures de membre


Dans l’UWP, les tableaux dans les signatures de membre doivent être unidimensionnels avec une limite inférieure de0 (zéro). Les types de tableaux imbriqués tels que `myArray[][]` (`myArray()()` en Visual Basic) ne sont pas autorisés.

> **Remarque**cette restriction ne s’applique pas aux tableaux que vous utilisez en interne dans votre implémentation.

 

| Numéro d’erreur | Texte du message                                                                                                                                                     |
|--------------|--------------------|
| WME1034      | Méthode «{0}«a un tableau de type»{1}» avec une limite inférieure de zéro dans sa signature. Les tableaux dans les signatures de méthode Windows Runtime doivent avoir une limite inférieure de zéro. |
| WME1035      | Méthode «{0}«a un tableau multidimensionnel de type»{1}» dans sa signature. Les tableaux dans les signatures de méthode Windows Runtime doivent être unidimensionnels.                  |
| WME1036      | Méthode '{0}«possède un tableau imbriqué de type»{1}» dans sa signature. Les tableaux dans les signatures de méthode Windows Runtime ne peuvent pas être imbriqués.                                    |

 

## <a name="array-parameters-must-specify-whether-array-contents-are-readable-or-writable"></a>Les paramètres de tableau doivent spécifier si le contenu du tableau est accessible en lecture ou en écriture


Dans l’UWP, les paramètres doivent être en lecture seule ou en écriture seule. Les paramètres ne peuvent pas être marqués **ref** (**ByRef** sans l’attribut [OutAttribute](https://msdn.microsoft.com/library/system.runtime.interopservices.outattribute.aspx) en Visual Basic). Cela s’applique au contenu des tableaux; par conséquent, les paramètres de tableau doivent indiquer si le contenu du tableau est en lecture seule ou en écriture seule. La direction est claire pour les paramètres **out** (paramètre **ByRef** avec l’attribut OutAttribute en Visual Basic), mais les paramètres de tableau passés par valeur (ByVal en Visual Basic) doivent être marqués. Voir [Transmission de tableaux à un composant Windows Runtime](passing-arrays-to-a-windows-runtime-component.md).

| Numéro d’erreur | Texte du message         |
|--------------|----------------------|
| WME1101      | Méthode «{0}«possède le paramètre»{1}» qui est un tableau, et qui comporte à la fois {2} et {3}. Dans le Windows Runtime, les paramètres du tableau de contenu doivent être accessibles en lecture ou en écriture. Supprimez l’un des attributs de «{1}».                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| WME1102      | Méthode «{0}«a un paramètre de sortie»{1}» qui est un tableau, mais qui a {2}. Dans le Windows Runtime, le contenu des tableaux de sortie est accessible en écriture. Supprimez l’attribut de «{1}».                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| WME1103      | Méthode «{0}«possède le paramètre»{1}» qui est un tableau, et qui comporte un attribut System.Runtime.InteropServices.InAttribute ou System.Runtime.InteropServices.OutAttribute. Dans le Windows Runtime, paramètres de tableau doivent avoir {2} ou {3}. Supprimez les attributs suivants ou remplacez-les par l’attribut de Windows Runtime approprié si nécessaire.                                                                                                                                                                                                                                                                                                                                                                                          |
| WME1104      | Méthode «{0}«possède le paramètre»{1}» qui n’est pas un tableau, et qui a un {2} ou un {3}. Windows Runtime ne prend pas en charge le marquage des paramètres avec {2} ou {3}.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| WME1105      | Méthode «{0}«possède le paramètre»{1}» avec l’attribut System.Runtime.InteropServices.InAttribute ou System.Runtime.InteropServices.OutAttribute. Windows Runtime ne prend pas en charge le marquage des paramètres avec l’attribut System.Runtime.InteropServices.InAttribute ou System.Runtime.InteropServices.OutAttribute. Supprimez System.Runtime.InteropServices.InAttribute et remplacez System.Runtime.InteropServices.OutAttribute par le modificateur « out ». Méthode «{0}«possède le paramètre»{1}» avec l’attribut System.Runtime.InteropServices.InAttribute ou System.Runtime.InteropServices.OutAttribute. Windows Runtime ne prend en charge que le marquage des paramètres ByRef par System.Runtime.InteropServices.OutAttribute, et ne prend en charge aucune autre utilisation de ces attributs. |
| WME1106      | Méthode «{0}«possède le paramètre»{1}» qui est un tableau. Dans le Windows Runtime, le contenu des paramètres du tableau doit être accessible en lecture ou en écriture. Appliquez {2} ou {3} à «{1}».                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |


## <a name="member-with-a-parameter-named-value"></a>Membre avec un paramètre nommé « value »


Dans l’UWP, les valeurs de retour sont considérées comme des paramètres de sortie, et les noms de ces paramètres doivent être uniques. Par défaut, Winmdexp.exe donne à la valeur de retour le nom « value ». Si votre méthode possède un paramètre nommé « value », vous obtiendrez l’erreur WME1092. Il existe deux façons de corriger cette situation :

-   Donnez à votre paramètre un nom différent de « value » (dans les accesseurs de propriété, un nom différent de « returnValue »).
-   Utilisez l’attribut ReturnValueNameAttribute pour modifier le nom de la valeur de retour, comme indiqué ci-après:

    > [!div class="tabbedCodeSnippets"]
    > ```cs
    > using System.Runtime.InteropServices;
    > using System.Runtime.InteropServices.WindowsRuntime;
    >
    > [return: ReturnValueName("average")]
    > public int GetAverage(out int lowValue, out int highValue)
    > ```
    > ```vb
    > Imports System.Runtime.InteropServices
    > Imports System.Runtime.InteropServices.WindowsRuntime
    >
    > Public Function GetAverage(<Out> ByRef lowValue As Integer, _
    > <Out> ByRef highValue As Integer) As <ReturnValueName("average")> String
    > ```

> **Remarque**si vous modifiez le nom de la valeur de retour, et le nouveau nom est en conflit avec le nom d’un autre paramètre, vous obtenez l’erreur WME1091.

Le code JavaScript peut accéder aux paramètres de sortie d’une méthode par nom, notamment la valeur de retour. Pour obtenir un exemple, voir l’attribut [ReturnValueNameAttribute](https://msdn.microsoft.com/library/windows/apps/system.runtime.interopservices.windowsruntime.returnvaluenameattribute.aspx).

| Numéro d’erreur | Texte du message |
|--------------|--------------|
| WME1091 | La méthode ' \{0}» a la valeur de retour nommée «\{1}» qui est identique à un nom de paramètre. Les paramètres de méthode Windows Runtime et la valeur de retour doivent avoir des noms uniques. |
| WME1092 | La méthode ' \{0}' possède un paramètre nommé «\{1}» qui est identique à la valeur par défaut de renvoyer le nom de la valeur. Fournissez un autre nom pour le paramètre ou utilisez System.Runtime.InteropServices.WindowsRuntime.ReturnValueNameAttribute pour spécifier explicitement le nom de la valeur de retour. |

**Remarque**le nom par défaut est «returnValue» pour les accesseurs de propriété et «value» pour toutes les autres méthodes.


## <a name="related-topics"></a>Rubriques connexes

* [Création de composants Windows Runtime en C# et Visual Basic](creating-windows-runtime-components-in-csharp-and-visual-basic.md)
* [Winmdexp.exe (outil d’exportation de métadonnées Windows Runtime)](https://msdn.microsoft.com/library/hh925576.aspx)
