---
ms.assetid: 066711E0-D5C4-467E-8683-3CC64EDBCC83
title: Appeler des API asynchrones en C# ou Visual Basic
description: La plateforme Windows universelle (UWP) comporte de nombreuses API asynchrones qui permettent à votre application de rester réactive lorsqu’elle exécute des opérations potentiellement longues.
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp, C#, Visual Basic, asynchrone
ms.localizationpriority: medium
ms.openlocfilehash: db0e3064cc64e6ffbafa50d045ced020ad30b460
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66371542"
---
# <a name="call-asynchronous-apis-in-c-or-visual-basic"></a>Appeler des API asynchrones en C# ou Visual Basic



La plateforme Windows universelle (UWP) comporte de nombreuses API asynchrones qui permettent à votre application de rester réactive lorsqu’elle exécute des opérations potentiellement longues. Cette rubrique décrit comment utiliser les méthodes asynchrones de l’UWP, en C# ou Microsoft Visual Basic.

Les API asynchrones évitent à votre application d’attendre la fin d’opérations d’envergure pour poursuivre son exécution. Par exemple, une application qui télécharge des informations à partir d’un site Internet peut passer plusieurs secondes à attendre que les informations arrivent. Si vous utilisez une méthode synchrone pour récupérer les informations, l’application reste bloquée jusqu’à ce que la méthode renvoie un résultat. L’application ne répond alors plus aux interactions utilisateur, ce qui peut constituer une gêne pour l’utilisateur. En fournissant des API asynchrones, l’UWP contribue à ce que votre application reste réactive aux interactions utilisateur pendant les opérations nécessitant un temps d’exécution plus long.

La plupart des API asynchrones UWP n’ayant pas d’équivalents synchrones, vous devez être certain de savoir comment utiliser les API asynchrones avec C# ou Visual Basic dans votre application de plateforme Windows universelle (UWP). Nous allons vous montrer comment appeler des API asynchrones de l’UWP.

## <a name="using-asynchronous-apis"></a>Utilisation d’API asynchrones


Par convention, les noms attribués aux méthodes asynchrones se terminent par « Async ». Vous appelez généralement des API asynchrones en réponse à une action de l’utilisateur, par exemple quand l’utilisateur clique sur un bouton. L’appel d’une méthode asynchrone dans un gestionnaire d’événements est l’une des façons les plus simples d’utiliser des API asynchrones. Prenons comme exemple l’opérateur **await**.

Supposons que votre application affiche la liste des titres de billets de blog publiés sur un site donné. L’application comporte un bouton ([**Button**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Button)) sur lequel l’utilisateur doit cliquer pour obtenir la liste des titres. Les titres s’affichent dans un bloc de texte ([**TextBlock**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock)). Lorsque l’utilisateur clique sur le bouton, il est important que l’application reste réactive pendant le téléchargement des informations provenant du site web du blog. Pour maintenir cette réactivité, l’UWP fournit la méthode asynchrone [**SyndicationClient.RetrieveFeedAsync**](https://docs.microsoft.com/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync) qui télécharge le flux.

L’exemple suivant obtient les titres des billets d’un blog en appelant la méthode asynchrone, [**SyndicationClient.RetrieveFeedAsync**](https://docs.microsoft.com/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync) et en attendant le résultat.

> [!div class="tabbedCodeSnippets" data-resources="OutlookServices.Calendar"]
[!code-csharp[Main](./AsyncSnippets/csharp/MainPage.xaml.cs#SnippetDownloadRSS)]
[!code-vb[Main](./AsyncSnippets/vbnet/MainPage.xaml.vb#SnippetDownloadRSS)]

Plusieurs points importants sont à signaler dans cet exemple. En premier lieu, notez que la ligne `SyndicationFeed feed = await client.RetrieveFeedAsync(feedUri)` utilise l’opérateur **await** avec un appel à la méthode asynchrone [**RetrieveFeedAsync**](https://docs.microsoft.com/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync). Vous pouvez voir l’opérateur **await** comme un moyen d’indiquer au compilateur que vous appelez une méthode asynchrone, ce qui oblige le compilateur à effectuer des opérations supplémentaires à votre place. En second lieu, notez que la déclaration du gestionnaire d’événements inclut le mot clé **async**. Vous devez inclure ce mot clé dans la déclaration de chaque méthode dans laquelle vous utilisez l’opérateur **await**.

Dans cette rubrique, nous n’allons pas expliquer en détail la manière dont le compilateur utilise l’opérateur **await**. Nous allons plutôt nous attacher à comprendre comment votre application peut rester asynchrone et réactive. Observons ce qui se produit avec du code synchrone. Par exemple, imaginons qu’il y ait une méthode synchrone appelée `SyndicationClient.RetrieveFeed`. (Aucune méthode n’est, mais imaginez qu’il existe). Si votre application inclut la ligne `SyndicationFeed feed = client.RetrieveFeed(feedUri)`, au lieu de `SyndicationFeed feed = await client.RetrieveFeedAsync(feedUri)`, l’exécution de l’application s’arrête jusqu'à ce que la valeur de retour de `RetrieveFeed` est disponible. Pendant que l’application attend la fin de la méthode, elle ne peut plus répondre à d’autres événements (un deuxième événement [**Click**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click), par exemple). En clair, votre application resterait bloquée jusqu’au renvoi du résultat de `RetrieveFeed`.

En revanche, si vous appelez `client.RetrieveFeedAsync`, la méthode initie la récupération et renvoie immédiatement le résultat. Lorsque vous utilisez l’opérateur **await** avec [**RetrieveFeedAsync**](https://docs.microsoft.com/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync), l’application ferme temporairement le gestionnaire d’événements. Elle peut ensuite gérer d’autres événements en même temps que la méthode **RetrieveFeedAsync** s’exécute de manière asynchrone. Ainsi, l’application reste réactive aux interactions utilisateur. Lorsque la méthode **RetrieveFeedAsync** est terminée et que [**SyndicationFeed**](https://docs.microsoft.com/uwp/api/Windows.Web.Syndication.SyndicationFeed) est disponible, l’application rouvre le gestionnaire d’événements là où elle l’avait fermé, après `SyndicationFeed feed = await client.RetrieveFeedAsync(feedUri)`, et finit l’exécution de la méthode.

Vous pouvez constater que le code obtenu avec l’opérateur **await** se présente à peu près de la même façon que le code obtenu avec la méthode fictive `RetrieveFeed`. Il est possible d’écrire du code asynchrone en C# ou Visual Basic sans utiliser l’opérateur **await**, mais le code obtenu a tendance à mettre en évidence les mécanismes de l’exécution de code asynchrone. Cela rend l’écriture, la compréhension et la gestion du code asynchrone difficiles. En utilisant l’opérateur **await**, vous bénéficiez des avantages d’une application asynchrone, sans avoir l’inconvénient de voir votre code devenir plus complexe.

## <a name="return-types-and-results-of-asynchronous-apis"></a>Types de retour et résultats des API asynchrones


Si vous avez suivi le lien vers [**RetrieveFeedAsync**](https://docs.microsoft.com/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync), vous avez peut-être constaté que le type de retour de **RetrieveFeedAsync** n’est pas [**SyndicationFeed**](https://docs.microsoft.com/uwp/api/Windows.Web.Syndication.SyndicationFeed), À la place, le type de retour est `IAsyncOperationWithProgress<SyndicationFeed, RetrievalProgress>`. Du point de vue de la syntaxe brute, une API asynchrone retourne un objet qui contient le résultat. Même s’il est courant, et parfois utile, de traiter une méthode asynchrone comme un élément « awaitable », l’opérateur **await** s’applique à la valeur renvoyée de la méthode, et pas à la méthode proprement dite. Lorsque vous appliquez l’opérateur **await**, vous obtenez le résultat de l’appel à **GetResult** sur l’objet renvoyé par la méthode. Dans l’exemple, **SyndicationFeed** est le résultat de **RetrieveFeedAsync.GetResult()** .

Lorsque vous utilisez une méthode asynchrone, vous pouvez examiner la signature pour voir ce que vous obtiendrez après avoir attendu la valeur renvoyée par la méthode. Toutes les API asynchrones de l’UWP renvoient l’un des types suivants :

-   [**IAsyncOperation&lt;TResult&gt;** ](https://docs.microsoft.com/uwp/api/Windows.Foundation.IAsyncOperation_TResult_)
-   [**IAsyncOperationWithProgress&lt;TResult, TProgress&gt;** ](https://docs.microsoft.com/uwp/api/Windows.Foundation.IAsyncOperationWithProgress_TResult_TProgress_)
-   [**IAsyncAction**](https://docs.microsoft.com/uwp/api/windows.foundation.iasyncaction)
-   [**IAsyncActionWithProgress&lt;TProgress&gt;** ](https://docs.microsoft.com/uwp/api/Windows.Foundation.IAsyncActionWithProgress_TProgress_)

Une méthode asynchrone renvoie un résultat qui est du même type que le paramètre de type `      TResult`. Les types sans `TResult` ne renvoient aucun résultat. Vous pouvez considérer que le résultat est de type **void**. En Visual Basic, une procédure [Sub](https://docs.microsoft.com/dotnet/articles/visual-basic/programming-guide/language-features/procedures/sub-procedures) est équivalente à une méthode ayant un résultat de type **void**.

Le tableau ci-dessous donne quelques exemples de méthodes asynchrones, en indiquant leur type de retour et leur type de résultat respectifs.

| Méthode asynchrone                                                                           | Type de retour                                                                                                                                        | Type de résultat                                       |
|-----------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------|
| [**SyndicationClient.RetrieveFeedAsync**](https://docs.microsoft.com/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync)     | [**IAsyncOperationWithProgress&lt;SyndicationFeed, RetrievalProgress&gt;** ](https://docs.microsoft.com/uwp/api/Windows.Foundation.IAsyncOperationWithProgress_TResult_TProgress_)                                 | [**SyndicationFeed**](https://docs.microsoft.com/uwp/api/Windows.Web.Syndication.SyndicationFeed) |
| [**FileOpenPicker.PickSingleFileAsync**](https://docs.microsoft.com/uwp/api/windows.storage.pickers.fileopenpicker.picksinglefileasync) | [**IAsyncOperation&lt;StorageFile&gt;** ](https://docs.microsoft.com/uwp/api/Windows.Foundation.IAsyncOperation_TResult_)                                                                                | [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile)          |
| [**XmlDocument.SaveToFileAsync**](https://docs.microsoft.com/uwp/api/windows.data.xml.dom.xmldocument.savetofileasync)                 | [**IAsyncAction**](https://docs.microsoft.com/uwp/api/windows.foundation.iasyncaction)                                                                                                           | **void**                                          |
| [**InkStrokeContainer.LoadAsync**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkstrokecontainer.loadasync)               | [**IAsyncActionWithProgress&lt;UInt64&gt;** ](https://docs.microsoft.com/uwp/api/Windows.Foundation.IAsyncActionWithProgress_TProgress_)                                                                   | **void**                                          |
| [**DataReader.LoadAsync**](https://docs.microsoft.com/uwp/api/windows.storage.streams.datareader.loadasync)                            | [**DataReaderLoadOperation**](https://docs.microsoft.com/uwp/api/Windows.Storage.Streams.DataReaderLoadOperation), personnalisé génère la classe qui implémente **IAsyncOperation&lt;UInt32&gt;** | [**UInt32**](https://docs.microsoft.com/uwp/api/Windows.Foundation.IAsyncOperation_TResult_)                     |

 

Les méthodes asynchrones définies dans [ **.NET for UWP apps**](https://dotnet.microsoft.com/apps/desktop) ont le type de retour [**Task**](https://docs.microsoft.com/dotnet/api/system.threading.tasks.task?redirectedfrom=MSDN) ou [**Task&lt;TResult&gt;** ](https://docs.microsoft.com/dotnet/api/system.threading.tasks.task-1?redirectedfrom=MSDN). Les méthodes qui renvoient **Task** sont similaires aux méthodes asynchrones dans l’UWP qui renvoient [**IAsyncAction**](https://docs.microsoft.com/uwp/api/windows.foundation.iasyncaction). Dans chaque cas, le résultat de la méthode asynchrone est de type **void**. Le type de retour **Task&lt;TResult&gt;** est similaire à [**IAsyncOperation&lt;TResult&gt;** ](https://docs.microsoft.com/uwp/api/Windows.Foundation.IAsyncOperation_TResult_) dans la mesure où le résultat de la méthode asynchrone lors de l’exécution de la tâche est du même type que le paramètre de type `TResult`. Pour plus d’informations sur l’utilisation de **.NET for UWP apps** et des tâches, voir [Présentation de .NET pour les applications Windows Runtime](https://docs.microsoft.com/previous-versions/windows/apps/br230302(v=vs.140)).

## <a name="handling-errors"></a>Gestion des erreurs


Quand vous utilisez l’opérateur **await** pour récupérer les résultats d’une méthode asynchrone, vous pouvez utiliser un bloc **try/catch** pour gérer les erreurs qui se produisent dans des méthodes asynchrones, exactement comme vous le faites pour les méthodes synchrones. L’exemple précédent encapsule la méthode **RetrieveFeedAsync** et l’opération **await** dans un bloc **try/catch** pour gérer les erreurs quand une exception est levée.

Quand des méthodes asynchrones appellent d’autres méthodes asynchrones, toute méthode asynchrone qui entraîne une exception est propagée aux méthodes externes. Cela signifie que vous pouvez placer un bloc **try/catch** dans la méthode la plus externe pour détecter les erreurs relatives aux méthodes asynchrones imbriquées. À nouveau, cette opération est similaire à la façon dont vous détectez les exceptions pour les méthodes synchrones. Toutefois, vous ne pouvez pas utiliser **await** dans le bloc **catch**.

**Conseil**  en commençant par C# dans Microsoft Visual Studio 2005, vous pouvez utiliser **await** dans le **catch** bloc.

## <a name="summary-and-next-steps"></a>Récapitulatif et étapes suivantes

Le modèle qui a été utilisé dans cette rubrique pour appeler une méthode asynchrone est le modèle le plus simple permettant d’appeler des API asynchrones dans un gestionnaire d’événements. Vous pouvez également utiliser ce modèle pour appeler une méthode asynchrone dans une méthode remplacée qui renvoie **void**, ou **Sub** en Visual Basic.

En ce qui concerne les méthodes asynchrones fournies dans l’UWP, il est important de garder à l’esprit les points suivants :

-   Par convention, les noms attribués aux méthodes asynchrones se terminent par « Async ».
-   Toute méthode qui utilise l’opérateur **await** doit avoir sa déclaration marquée avec le mot clé**async**.
-   Lorsqu’une application trouve l’opérateur **await**, elle demeure réactive aux interactions utilisateur pendant la durée d’exécution de la méthode asynchrone.
-   L’attente de la valeur renvoyée par une méthode asynchrone renvoie un objet contenant le résultat. Dans la plupart des cas, le résultat contenu dans la valeur renvoyée correspond à des informations utiles, pas à la valeur renvoyée proprement dite. Vous pouvez déterminer le type de la valeur contenue dans le résultat en observant le type de retour de la méthode asynchrone.
-   Les API asynchrones et les modèles **async** s’avèrent souvent utiles pour améliorer la réactivité de votre application.

L’exemple de cette rubrique produit un résultat similaire au texte ci-dessous.

``` syntax
Windows Experience Blog
PC Snapshot: Sony VAIO Y, 8/9/2011 10:26:56 AM -07:00
Tech Tuesday Live Twitter #Chat: Too Much Tech #win7tech, 8/8/2011 12:48:26 PM -07:00
Windows 7 themes: what’s new and what’s popular!, 8/4/2011 11:56:28 AM -07:00
PC Snapshot: Toshiba Satellite A665 3D, 8/2/2011 8:59:15 AM -07:00
Time for new school supplies? Find back-to-school deals on Windows 7 PCs and Office 2010, 8/1/2011 2:14:40 PM -07:00
Best PCs for blogging (or working) on the go, 8/1/2011 10:08:14 AM -07:00
Tech Tuesday – Blogging Tips and Tricks–#win7tech, 8/1/2011 9:35:54 AM -07:00
PC Snapshot: Lenovo IdeaPad U460, 7/29/2011 9:23:05 AM -07:00
GIVEAWAY: Survive BlogHer with a Sony VAIO SA and a Samsung Focus, 7/28/2011 7:27:14 AM -07:00
3 Ways to Stay Cool This Summer, 7/26/2011 4:58:23 PM -07:00
Getting RAW support in Photo Gallery & Windows 7 (…and a contest!), 7/26/2011 10:40:51 AM -07:00
Tech Tuesdays Live Twitter Chats: Photography Tips, Tricks and Essentials, 7/25/2011 12:33:06 PM -07:00
3 Tips to Go Green With Your PC, 7/22/2011 9:19:43 AM -07:00
How to: Buy a Green PC, 7/22/2011 9:13:22 AM -07:00
Windows 7 themes: the distinctive artwork of Cheng Ling, 7/20/2011 9:53:07 AM -07:00
```
