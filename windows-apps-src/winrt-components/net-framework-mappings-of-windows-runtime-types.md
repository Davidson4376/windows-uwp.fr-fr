---
title: Mappages .NET des types de Windows Runtime
description: Le tableau suivant répertorie les mappages que .NET effectue entre les types de plateforme Windows universelle (UWP) et les types .NET.
ms.assetid: 5317D771-808D-4B97-8063-63492B23292F
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: c035db58fc6aa484f9d47a9af61176a2b05d55ee
ms.sourcegitcommit: d38e2f31c47434cd6dbbf8fe8d01c20b98fabf02
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70393658"
---
# <a name="net-mappings-of-windows-runtime-types"></a>Mappages .NET des types de Windows Runtime

Le tableau suivant répertorie les mappages que .NET effectue entre les types de plateforme Windows universelle (UWP) et les types .NET. Dans une application Windows universelle écrite avec du code managé, Visual Studio IntelliSense affiche le type .NET au lieu du type UWP. Par exemple, si une méthode Windows Runtime accepte un paramètre de type chaîne&lt;&gt;IVector, IntelliSense affiche un paramètre de type IList&lt;chaîne&gt;. De même, dans un composant Windows Runtime écrit avec du code managé, vous utilisez le type .NET dans les signatures de membre. Lorsque l' [outil d’exportation de métadonnées Windows Runtime (Winmdexp. exe)](/dotnet/framework/tools/winmdexp-exe-windows-runtime-metadata-export-tool) génère votre composant Windows Runtime, le type .net est traduit dans le type UWP correspondant.

La plupart des types qui ont le même nom d’espace de noms et le même nom de type dans UWP et .NET sont des structures (ou des types associés à des structures, telles que des énumérations). Dans UWP, les structures n’ont pas de membres autres que des champs et nécessitent des types d’assistance, que .NET masque. Les versions .NET de ces structures ont des propriétés et des méthodes qui fournissent les fonctionnalités des types d’assistance masqués.

## <a name="uwp-types-that-map-to-net-types-with-the-same-name-and-namespace"></a>Types UWP mappés à des types .NET avec les mêmes nom et espace de noms

### <a name="in-net-assembly-systemobjectmodeldll"></a>Dans .NET assembly System. ObjectModel. dll

| Espace de noms | Type |
|-|-|
| Windows.UI.Xaml.Input | ICommand |

### <a name="in-net-assembly-systemruntimewindowsruntimedll"></a>Dans .NET assembly System. Runtime. WindowsRuntime. dll

| Espace de noms | Type |
|-|-|
| Windows.Foundation | Point |
| Windows.Foundation | Rect |
| Windows.Foundation | Size |
| Windows.UI | Color |

### <a name="in-net-assembly-systemruntimewindowsruntimeuixamldll"></a>Dans .NET assembly System. Runtime. WindowsRuntime. UI. Xaml. dll

| Espace de noms | Type |
|-|-|
| Windows.UI.Xaml | CornerRadius |
| Windows.UI.Xaml | Duration |
| Windows.UI.Xaml | DurationType |
| Windows.UI.Xaml | GridLength |
| Windows.UI.Xaml | GridUnitType |
| Windows.UI.Xaml | Thickness |
| Windows.UI.Xaml.Controls.Primitives | GeneratorPosition |
| Windows.UI.Xaml.Media | Matrix |
| Windows.UI.Xaml.Media.Animation | KeyTime |
| Windows.UI.Xaml.Media.Animation | RepeatBehavior |
| Windows.UI.Xaml.Media.Animation | RepeatBehaviorType |
| Windows.UI.Xaml.Media.Media3D | Matrix3D |

## <a name="uwp-types-that-map-to-net-types-with-a-different-name-andor-namespace"></a>Types UWP mappés à des types .NET avec un nom et/ou un espace de noms différents

### <a name="in-net-assembly-systemobjectmodeldll"></a>Dans .NET assembly System. ObjectModel. dll

| Type/espace de noms UWP | Type/espace de noms .NET |
|-|-|
| INotifyCollectionChanged (Windows.UI.Xaml.Interop) | INotifyCollectionChanged (System.Collections.Specialized) | 
| NotifyCollectionChangedEventHandler (Windows.UI.Xaml.Interop) | NotifyCollectionChangedEventHandler (System.Collections.Specialized) | 
| NotifyCollectionChangedEventArgs (Windows.UI.Xaml.Interop) | NotifyCollectionChangedEventArgs (System.Collections.Specialized) | 
| NotifyCollectionChangedAction (Windows.UI.Xaml.Interop) | NotifyCollectionChangedAction (System.Collections.Specialized) | 
| INotifyPropertyChanged (Windows.UI.Xaml.Data) | INotifyPropertyChanged (System.ComponentModel) | 
| PropertyChangedEventHandler (Windows.UI.Xaml.Data) | PropertyChangedEventHandler (System.ComponentModel) | 
| PropertyChangedEventArgs (Windows.UI.Xaml.Data) | PropertyChangedEventArgs (System.ComponentModel) | 

### <a name="in-net-assembly-systemruntimedll"></a>Dans .NET assembly System. Runtime. dll

| Type/espace de noms UWP | Type/espace de noms .NET |
|-|-|
| AttributeUsageAttribute (Windows.Foundation.Metadata) | AttributeUsageAttribute (System) |
| AttributeTargets (Windows.Foundation.Metadata) | AttributeTargets (System) |
| DateTime (Windows.Foundation) | DateTimeOffset (System) |
| EventHandler&lt;T&gt; (Windows.Foundation) | EventHandler&lt;T&gt; (System) |
| HResult (Windows.Foundation) | Exception (System) |
| IReference&lt;T&gt; (Windows.Foundation) | Nullable&lt;T&gt; (System) |
| TimeSpan (Windows.Foundation) | TimeSpan (System) |
| Uri (Windows.Foundation) | Uri (System) |
| IClosable (Windows.Foundation) | IDisposable (System) |
| IIterable&lt;T&gt; (Windows.Foundation.Collections) | IEnumerable&lt;T&gt; (System.Collections.Generic) |
| IVector&lt;T&gt; (Windows.Foundation.Collections) | IList&lt;T&gt; (System.Collections.Generic) |
| IVectorView&lt;T&gt; (Windows.Foundation.Collections) | IReadOnlyList&lt;T&gt; (System.Collections.Generic) |
| IMap&lt;K,V&gt; (Windows.Foundation.Collections) | IDictionary&lt;TKey,TValue&gt; (System.Collections.Generic) |
| IMapView&lt;K,V&gt; (Windows.Foundation.Collections) | IReadOnlyDictionary&lt;TKey,TValue&gt; (System.Collections.Generic) |
| IKeyValuePair&lt;K,V&gt; (Windows.Foundation.Collections) | KeyValuePair&lt;TKey,TValue&gt; (System.Collections.Generic) |
| IBindableIterable (Windows.UI.Xaml.Interop) | IEnumerable (System.Collections) |
| IBindableVector (Windows.UI.Xaml.Interop) | IList (System.Collections) |
| TypeName (Windows.UI.Xaml.Interop) | Type (System) |

### <a name="in-net-assembly-systemruntimeinteropserviceswindowsruntimedll"></a>Dans .NET assembly System. Runtime. InteropServices. WindowsRuntime. dll

| Type/espace de noms UWP | Type/espace de noms .NET |
|-|-|
| EventRegistrationToken (Windows.Foundation) | EventRegistrationToken (System.Runtime.InteropServices.WindowsRuntime) |

## <a name="related-topics"></a>Rubriques connexes

* [Windows Runtime composants avec C# et Visual Basic](creating-windows-runtime-components-in-csharp-and-visual-basic.md)