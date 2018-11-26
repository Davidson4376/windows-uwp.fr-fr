---
ms.assetid: 9899F6A0-7EDD-4988-A76E-79D7C0C58126
title: Composants UWP et optimisation de l’interopérabilité
description: Créez des applications de plateforme Windows universelle (UWP) qui utilisent des composants UWP et l’interopérabilité entre les types natifs et managés, tout en évitant les problèmes de performances liés à l’interopérabilité.
ms.date: 02/08/2017
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 37bcf2ff6eee6c272339fdc997ee7bbb046f85e9
ms.sourcegitcommit: 681c70f964210ab49ac5d06357ae96505bb78741
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/26/2018
ms.locfileid: "7699552"
---
# <a name="uwp-components-and-optimizing-interop"></a>Composants UWP et optimisation de l’interopérabilité


Créez des applications de plateforme Windows universelle (UWP) qui utilisent des composants UWP et l’interopérabilité entre les types natifs et managés, tout en évitant les problèmes de performances liés à l’interopérabilité.

## <a name="best-practices-for-interoperability-with-uwp-components"></a>Meilleures pratiques en matière d’interopérabilité avec les composants UWP

Si vous n’y prêtez pas attention, l’utilisation de composants UWP peut avoir une incidence considérable sur les performances de votre application. Cette section explique comment obtenir des performances optimales quand votre application utilise des composants UWP.

### <a name="introduction"></a>Introduction

L’interopérabilité peut avoir un impact important sur les performances et il peut arriver que vous y ayez recours sans vous en apercevoir. L’UWP traite automatiquement une grande part de l’interopérabilité avec ses composants pour que vous soyez plus productif et réutilisiez tout code écrit dans un autre langage. Nous vous recommandons de tirer parti de ce que l’UWP fait à votre place, mais sachez que cela peut avoir un impact sur les performances. Cette section indique ce que vous pouvez faire pour atténuer l’impact de l’interopérabilité sur les performances de votre application.

l’UWP comporte une bibliothèque de types accessible à partir de tout langage permettant d’écrire une application UWP. Vous pouvez utiliser les types UWP dans C# ou Microsoft Visual Basic de la même manière que des objets .NET. Vous n’avez pas besoin de configurer la plateforme de sorte qu’elle invoque des appels de méthode pour accéder aux composants UWP. Cela rend l’écriture des applications beaucoup moins complexe mais sachez que l’interopérabilité peut être plus importante que prévu. Si un composant UWP est écrit dans un langage autre que C# ou Visual Basic, vous dépassez les limites de l’interopérabilité lorsque vous utilisez ce composant. Cela a un impact sur les performances d’une application.

Quand vous développez une application UWP en C# ou Visual Basic, les deux types d’API les plus courants que vous utilisez sont les API UWP et les API .NET pour les applications UWP. En règle générale, les types qui sont définis dans UWP se trouvent dans des espaces de noms commençant par «Windows.», alors que les espaces de noms des types .NET commencent par «System.». Il existe cependant quelques exceptions à cette règle. Les types dans .NET pour les applications UWP ne nécessitent pas d’interopérabilité quand ils sont utilisés. Si vous constatez de mauvaises performances pour des actions qui utilisent UWP, vous pouvez peut-être utiliser .NET pour les applications UWP à la place afin d’obtenir de meilleures performances.

**Remarque**  la plupart des composants UWP fournis avec Windows 10 est implémentée en C++, vous dépassez les limites de l’interopérabilité lorsque vous les utilisez dans c# ou Visual Basic. Comme toujours, observez le comportement de votre application pour déterminer si l’utilisation de composants UWP nuit à ses performances avant de modifier le code.

Dans cette rubrique, l’expression « composants UWP » désigne les composants écrits dans un langage autre que C# ou Visual Basic.

 

Chaque fois que vous accédez à une propriété ou appelez une méthode sur un composant UWP, cela induit un coût d’interopérabilité. En fait, créer un composant UWP est plus coûteux en ressources que de créer un objet .NET. Cela est dû au fait que l’UWP doit exécuter du code qui est converti du langage de votre application à celui du composant. En outre, si vous transmettez des données au composant, ces données doivent être converties du type géré au type non géré.

### <a name="using-uwp-components-efficiently"></a>Utilisation efficace des composants UWP

Si vous souhaitez obtenir de meilleures performances, vous pouvez faire en sorte que votre code utilise les composants UWP le plus efficacement possible. Cette section donne des conseils pour améliorer les performances lorsque vous utilisez les composants UWP.

Un nombre d’appels important dans une courte période est nécessaire pour que l’impact sur les performances soit visible. Une application bien conçue qui encapsule les appels aux composants UPW à partir de la logique métier et de tout autre code managé ne doit pas induire de coûts excessifs d’interopérabilité. Toutefois, si vos tests indiquent que l’utilisation des composants UWP nuit aux performances de votre application, les conseils présentés dans cette section vous aideront à éviter ce problème.

### <a name="consider-using-net-for-uwp-apps"></a>Envisager d’utiliser .NET pour les applications UWP

Dans certains cas, vous pouvez accomplir une tâche à l’aide de l’UWP ou de .NET pour les applications UWP. Nous vous conseillons de ne pas essayer de mélanger les types .NET et les types UWP. Essayez de n’utiliser qu’un seul type. Par exemple, vous pouvez analyser un flux de données xml à l’aide du type [**Windows.Data.Xml.Dom.XmlDocument**](https://msdn.microsoft.com/library/windows/apps/BR206173) (un type UWP) ou du type [**System.Xml.XmlReader**](https://msdn.microsoft.com/library/windows/apps/xaml/system.xml.xmlreader.aspx) (un type .NET). Utilisez l’API de la même technologie que celle du flux. Par exemple, si vous lisez des données xml à partir d’un type [**MemoryStream**](https://msdn.microsoft.com/library/windows/apps/xaml/system.io.memorystream.aspx), utilisez le type **System.Xml.XmlReader**, car les deux types sont des types .NET. Si vous lisez des données dans un fichier, utilisez le type **Windows.Data.Xml.Dom.XmlDocument**, car les API de fichier et **XmlDocument** sont des composants UWP.

### <a name="copy-window-runtime-objects-to-net-types"></a>Copier les objets Window Runtime dans les types .NET

Quand un composant UWP renvoie un objet UWP, il peut être judicieux de copier l’objet retourné dans un objet .NET. Cela est notamment important lorsque vous utilisez des collections et des flux.

Si vous appelez une API UWP qui renvoie une collection, puis si vous enregistrez cette collection et y accédez de nombreuses fois, il peut être utile de copier la collection dans une collection .NET et d’utiliser ensuite cette collection .NET.

### <a name="cache-the-results-of-calls-to-uwp-components-for-later-use"></a>Mettre en cache les résultats des appels aux composants UWP pour un usage ultérieur

Il est possible que vous obteniez de meilleures performances en enregistrant les valeurs dans des variables locales au lieu d’accéder à un type UWP plusieurs fois. Cela peut s’avérer particulièrement utile si vous utilisez une valeur dans une boucle. Observez le comportement de votre application pour déterminer si l’utilisation de variables locales améliore ses performances. L’utilisation de valeurs mises en cache peut augmenter la vitesse de votre application, car elle consacre moins de temps à l’interopérabilité.

### <a name="combine-calls-to-uwp-components"></a>Combiner les appels aux composants UWP

Essayez d’accomplir des tâches avec le moins d’appels possible aux objets UWP. Par exemple, il est en général préférable de lire un grand volume de données à partir d’un flux que de lire de petites quantités de données les unes après les autres.

Utilisez des API qui permettent de réaliser une tâche avec le moins d’appels possible au lieu d’utiliser des API qui permettent de réaliser moins de travail avec plus d’appels. Par exemple, créez de préférence un objet en appelant des constructeurs qui initialisent plusieurs propriétés au lieu d’appeler le constructeur par défaut et de lui affecter des propriétés une par une.

### <a name="building-a-uwp-components"></a>Création d’un composant UWP

Si vous écrivez un composant UWP pouvant être utilisé par des applications écrites en C++ ou JavaScript, assurez-vous que ce composant produira des performances optimales. Toutes les suggestions pour obtenir des performances optimales dans les applications s’appliquent également aux composants. Observez votre composant afin de savoir quelles API connaissent un trafic élevé et en conséquence, envisagez de fournir des API qui permettent à vos utilisateurs de faire leur travail en quelques appels.

## <a name="keep-your-app-fast-when-you-use-interop-in-managed-code"></a>Préserver la rapidité de votre application en cas d’utilisation de l’interopérabilité en code managé

L’UWP facilite l’interopérabilité entre les codes natif et managé mais, si vous ne faites pas attention, cela peut occasionner des coûts de performance. Nous expliquons ici comment obtenir des performances optimales lorsque vous utilisez l’interopérabilité dans vos applications UWP managées.

Avec l’UWP, les développeurs peuvent choisir le langage dans lequel ils souhaitent écrire leurs applications UWP en XAML, grâce aux projections des API UWP disponibles dans chaque langage. Dans le cas d’une application en C# ou Visual Basic, cette interopérabilité a un coût. En effet, comme les API UWP sont habituellement implémentées en code natif, chaque appel de l’UWP depuis C# ou Visual Basic nécessite que le CLR passe d’un frame de pile managé à un frame de pile natif, et qu’il marshale les paramètres de fonction aux représentations accessibles par le code natif. Ce temps de traitement supplémentaire a un impact négligeable pour la plupart des applications. Toutefois, si vous effectuez beaucoup d’appels (des centaines de milliers à des millions) aux API UWP dans le chemin critique d’une application, ce coût peut devenir considérable. En général, il faut veiller à ce que le temps consacré à la transition entre les langages est faible par rapport à l’exécution du reste de votre code. Ceci est illustré par le schéma suivant.

![Les transitions d’interopérabilité ne doivent pas dominer la durée d’exécution du programme.](images/interop-transitions.png)

Les types répertoriés dans [**.NET for Windows apps**](https://msdn.microsoft.com/library/windows/apps/xaml/br230232.aspx) ne présentent pas ce coût d’interopérabilité lorsqu’ils sont utilisés enC# ou Visual Basic. En règle générale, vous pouvez supposer que les types dans les espaces de noms qui commencent par « Windows. » font partie de l’UWP, et que les types dans les espaces de noms qui commencent par « System. » sont des types .NET. Souvenez-vous qu’une utilisation même simple des types UWP, telle que l’allocation ou l’accès aux propriétés, a un coût en matière d’interopérabilité.

Vous devez effectuer des mesures sur votre application et déterminer si l’interopérabilité requiert une partie significative de la durée d’exécution de votre application avant d’optimiser les coûts liés à l’interopérabilité. Lors de l’analyse de votre application avec Visual Studio, il est très facile d’obtenir une limite supérieure des coûts d’interopérabilité à l’aide de l’affichage **Fonctions**, en examinant la durée inclusive consacrée aux méthodes qui appellent l’UWP.

Si votre application est lente à cause de la surcharge liée à l’interopérabilité, vous pouvez améliorer ses performances en diminuant le nombre d’appels aux API UWP sur les chemins de code réactifs. Par exemple, lorsqu’un moteur de jeu doit effectuer une multitude de calculs physiques en demandant constamment la position et les dimensions de [**UIElements**](https://msdn.microsoft.com/library/windows/apps/BR208911), il peut le faire beaucoup plus rapidement en stockant les informations nécessaires de **UIElements** dans des variables locales. De cette manière, il effectue les calculs sur ces valeurs mises en cache, puis retourne le résultat final à **UIElements** une fois les calculs terminés. Prenons un autre exemple : lorsqu’une collection est souvent sollicitée par du code C# ou Visual Basic, il est plus judicieux d’utiliser une collection de l’espace de noms [**System.Collections**](https://msdn.microsoft.com/library/windows/apps/xaml/system.collections.aspx) qu’une collection de l’espace de noms [**Windows.Foundation.Collections**](https://msdn.microsoft.com/library/windows/apps/BR206657). Vous pourriez également combiner des appels aux composants UWP. Cela est par exemple possible lors de l’utilisation des API [**Windows.Storage.BulkAccess**](https://msdn.microsoft.com/library/windows/apps/BR207676).

### <a name="building-a-uwp-component"></a>Création d’un composant UWP

Si vous écrivez un composant UWP pour des applications écrites en C++ ou JavaScript, assurez-vous que ce composant produira des performances optimales. Votre surface d’API définit votre limite d’interopérabilité ainsi que le degré auquel vos utilisateurs devront prendre en compte les recommandations de cette rubrique. Si vous distribuez vos composants à des tierces parties, ceci revêt une importance toute particulière.

Toutes les suggestions pour obtenir des performances optimales dans les applications s’appliquent également aux composants. Observez votre composant afin de savoir quelles API connaissent un trafic élevé et fournissez des API qui permettent à vos utilisateurs de faire leur travail en quelques appels. Des efforts significatifs ont été consacrés à la conception de l’UWP en vue de permettre aux applications de l’utiliser sans avoir à traverser fréquemment la limite d’interopérabilité.

 

