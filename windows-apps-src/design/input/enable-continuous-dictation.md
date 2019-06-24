---
Description: Découvrez comment capturer et reconnaître une entrée vocale dictée en continu et sur une longue durée.
title: Activer la dictée continue
ms.assetid: 383B3E23-1678-4FBB-B36E-6DE2DA9CA9DC
label: Continuous dictation
template: detail.hbs
keywords: voix, vocal, reconnaissance vocale, langage naturel, dictée, saisie, interaction utilisateur
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 646f4ad98e6c914c2318a164629d31ce7b67dab4
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/21/2019
ms.locfileid: "67317307"
---
# <a name="continuous-dictation"></a>Dictée continue

Découvrez comment capturer et reconnaître une entrée vocale dictée en continu et sur une longue durée.

> **API importantes** : [**SpeechContinuousRecognitionSession**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechContinuousRecognitionSession), [**ContinuousRecognitionSession**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.continuousrecognitionsession)

Dans [Reconnaissance vocale](speech-recognition.md), vous avez appris à capturer et à reconnaître des saisies vocales de durée relativement courte à l’aide des méthodes [**RecognizeAsync**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.recognizeasync) ou [**RecognizeWithUIAsync**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.recognizewithuiasync) d’un objet [**SpeechRecognizer**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognizer), par exemple, lorsque l’utilisateur compose un SMS ou pose une question.

Pour les sessions de reconnaissance vocale en continu plus longues, tel qu’un texte dicté ou un e-mail, utilisez la propriété [**ContinuousRecognitionSession**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.continuousrecognitionsession) d’un [**SpeechRecognizer**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognizer) afin d’obtenir un objet [**SpeechContinuousRecognitionSession**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechContinuousRecognitionSession).

> [!NOTE]
> Prise en charge de la dictée varie selon le [appareil](https://docs.microsoft.com/windows/uwp/design/devices/) où votre application est en cours d’exécution. Pour les PC et ordinateurs portables, uniquement en-US est reconnu, tandis que Xbox et téléphones peuvent reconnaître toutes les langues prises en charge par la reconnaissance vocale. Pour plus d’informations, consultez [spécifier la langue du module de reconnaissance vocale](specify-the-speech-recognizer-language.md).

## <a name="set-up"></a>Configuration

Votre application a besoin de plusieurs objets pour gérer une session de dictée continue :

- une instance d’un objet [**SpeechRecognizer**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognizer) ;
- une référence de répartiteur d’interface utilisateur afin de mettre à jour l’interface utilisateur lors de la dictée ;
- un dispositif de suivi des mots ajoutés par l’utilisateur.

Dans cet exemple, nous déclarons une instance [**SpeechRecognizer**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognizer) comme un champ privé de la classe code-behind. Votre application doit stocker une référence à un autre emplacement, si vous voulez que la dictée en continu se poursuive sur d’autres pages XAML.

```CSharp
private SpeechRecognizer speechRecognizer;
```

Lors de la dictée, le module de reconnaissance déclenche des événements à partir d’un thread d’arrière-plan. Dans la mesure où un thread d’arrière-plan ne peut pas mettre à jour directement l’interface utilisateur en XAML, votre application doit utiliser un répartiteur pour mettre à jour l’interface utilisateur en réponse aux événements de reconnaissance.

Nous déclarons ici un champ privé, qui sera initialisé ultérieurement avec le répartiteur d’interface utilisateur.

```CSharp
// Speech events may originate from a thread other than the UI thread.
// Keep track of the UI thread dispatcher so that we can update the
// UI in a thread-safe manner.
private CoreDispatcher dispatcher;
```

Pour suivre ce que dit l’utilisateur, vous devez gérer les événements de reconnaissance déclenchés par le module de reconnaissance vocale. Ces événements fournissent les résultats de reconnaissance des différents blocs d’énoncés de l’utilisateur.

Ici, nous utilisons un objet [**StringBuilder**](https://docs.microsoft.com/dotnet/api/system.text.stringbuilder?redirectedfrom=MSDN) pour contenir tous les résultats de reconnaissance obtenus pendant la session. Les nouveaux résultats sont ajoutés à l’objet **StringBuilder** lors de leur traitement.

```CSharp
private StringBuilder dictatedTextBuilder;
```

## <a name="initialization"></a>Initialisation

Pendant l’initialisation de la reconnaissance vocale en continu, vous devez :

- Si vous mettez à jour l’interface utilisateur de votre application dans les gestionnaires d’événements de reconnaissance continue, récupérez le répartiteur pour le thread d’interface utilisateur.
- Initialisez le module de reconnaissance vocale.
- Compilez la grammaire de dictée intégrée.
    **Remarque**    la reconnaissance vocale nécessite au moins une contrainte pour définir un vocabulaire reconnaissable. Si aucune contrainte n’est spécifiée, une grammaire de dictée prédéfinie est utilisée. Voir [Reconnaissance vocale](speech-recognition.md).
- Configurez les détecteurs d’événements pour les événements de reconnaissance.

Nous initialisons la reconnaissance vocale dans l’événement de page [**OnNavigatedTo**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.page.onnavigatedto).

1. Comme les événements déclenchés par le module de reconnaissance vocale se produisent sur un thread d’arrière-plan, créez une référence pour le répartiteur pour les mises à jour du thread d’interface utilisateur. [**OnNavigatedTo** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.page.onnavigatedto) est toujours appelée sur le thread d’interface utilisateur.
```csharp
this.dispatcher = CoreWindow.GetForCurrentThread().Dispatcher;
```

2.  Nous initialisons ensuite l’instance [**SpeechRecognizer**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognizer).
```csharp
this.speechRecognizer = new SpeechRecognizer();
```

3.  Ensuite, nous ajoutons et nous compilons la grammaire qui définit tous les mots et toutes les expressions pouvant être reconnus par le [**SpeechRecognizer**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognizer).

    Si vous ne spécifiez explicitement de grammaire, la grammaire de dictée continue est utilisée par défaut. En règle générale, la grammaire par défaut est préférable pour la dictée générale.

    Ici, nous appelons [**CompileConstraintsAsync**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.compileconstraintsasync) immédiatement sans ajouter de grammaire.

    
```csharp
SpeechRecognitionCompilationResult result =
      await speechRecognizer.CompileConstraintsAsync();
```

## <a name="handle-recognition-events"></a>Gérer les événements de reconnaissance

Ici, vous pouvez capturer un seul énoncé ou une seule expression de courte durée en appelant [**RecognizeAsync**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.recognizeasync) ou [**RecognizeWithUIAsync**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.recognizewithuiasync). 

Toutefois, pour capturer une session de reconnaissance plus longue, continue, nous indiquons des détecteurs d’événements à exécuter en arrière-plan pendant que l’utilisateur parle et définissons des gestionnaires afin de créer la chaîne de dictée.

Nous pouvons ensuite utiliser la propriété [**ContinuousRecognitionSession**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.continuousrecognitionsession) de notre module de reconnaissance vocale pour obtenir un objet [**SpeechContinuousRecognitionSession**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechContinuousRecognitionSession) qui fournit des méthodes et des événements pour la gestion d’une session de reconnaissance vocale en continu.

Deux événements sont particulièrement essentiels :

- [**ResultGenerated**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechcontinuousrecognitionsession.resultgenerated), qui se produit lorsque le module de reconnaissance a généré des résultats.
- [**Terminé**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechcontinuousrecognitionsession.completed), qui se produit lorsque la session de reconnaissance continue s’est terminée.

L’événement [**ResultGenerated**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechcontinuousrecognitionsession.resultgenerated) est déclenché lorsque l’utilisateur parle. Le module de reconnaissance écoute l’utilisateur en continu et déclenche périodiquement un événement qui transmet un segment d’entrée vocale. Examinez l’entrée vocale à l’aide de la propriété [**Result**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechcontinuousrecognitionresultgeneratedeventargs.result) de l’argument d’événement et prenez les mesures appropriées dans le gestionnaire d’événements, par exemple en ajoutant du texte à un objet StringBuilder.

En tant qu’instance de [**SpeechRecognitionResult**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionResult), la propriété [**Result**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechcontinuousrecognitionresultgeneratedeventargs.result) permet d’indiquer si vous acceptez l’entrée vocale ou non : Une instance [**SpeechRecognitionResult**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionResult) fournit deux propriétés spécifiques :

- [**État** ](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognitionresult.status) indique si la reconnaissance a réussi. La reconnaissance peut échouer pour diverses raisons.
- [**Confiance** ](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognitionresult.confidence) indique la confiance relative que le module de reconnaissance compris les mots corrects.

Voici les étapes de base associées à la prise en charge de la reconnaissance continue :  

1. Ici, nous inscrivons le gestionnaire pour l’événement de reconnaissance en continu [**ResultGenerated**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechcontinuousrecognitionsession.resultgenerated) de l’événement de page [**OnNavigatedTo**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.page.onnavigatedto).
```csharp
speechRecognizer.ContinuousRecognitionSession.ResultGenerated +=
        ContinuousRecognitionSession_ResultGenerated;
```

2.  Nous vérifions ensuite la propriété [**Confidence**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognitionresult.confidence). Si la valeur de confiance est de [**Medium**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionConfidence) ou plus, nous ajoutons du texte au StringBuilder. Nous mettons également à jour l’interface utilisateur lorsque nous collectons des entrées.

    **Remarque**  le [ **ResultGenerated** ](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechcontinuousrecognitionsession.resultgenerated) événement est déclenché sur un thread d’arrière-plan qui ne peut pas mettre à jour l’interface utilisateur directement. Si un gestionnaire doit mettre à jour l’interface utilisateur (comme le \[exemple de reconnaissance vocale et synthèse vocale\] est), vous devez distribuer les mises à jour pour le thread d’interface utilisateur via la [ **RunAsync** ](https://docs.microsoft.com/uwp/api/windows.ui.core.coredispatcher.runasync) du répartiteur (méthode).
```csharp
private async void ContinuousRecognitionSession_ResultGenerated(
      SpeechContinuousRecognitionSession sender,
      SpeechContinuousRecognitionResultGeneratedEventArgs args)
      {

        if (args.Result.Confidence == SpeechRecognitionConfidence.Medium ||
          args.Result.Confidence == SpeechRecognitionConfidence.High)
          {
            dictatedTextBuilder.Append(args.Result.Text + " ");

            await dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
            {
              dictationTextBox.Text = dictatedTextBuilder.ToString();
              btnClearText.IsEnabled = true;
            });
          }
        else
        {
          await dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
            {
              dictationTextBox.Text = dictatedTextBuilder.ToString();
            });
        }
      }
```

3.  Ensuite, nous gérons l’événement [**Completed**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechcontinuousrecognitionsession.completed), qui indique la fin de la dictée continue.

    La session se termine lorsque vous appelez le [**StopAsync**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechcontinuousrecognitionsession.stopasync) ou les méthodes [**CancelAsync**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechcontinuousrecognitionsession.cancelasync) (voir la section suivante). La session peut également se terminer lorsqu’une erreur se produit, ou lorsque l’utilisateur cesse de parler. Vérifiez la propriété [**Status**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognitionresult.status) de l’argument d’événement afin de déterminer pourquoi la session s’est terminée ([**SpeechRecognitionResultStatus**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionResultStatus)).

    Ici, nous inscrivons le gestionnaire pour l’événement de reconnaissance en continu [**Completed**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechcontinuousrecognitionsession.completed) de l’événement de page [**OnNavigatedTo**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.page.onnavigatedto).
```csharp
speechRecognizer.ContinuousRecognitionSession.Completed +=
      ContinuousRecognitionSession_Completed;
```

4.  Le gestionnaire d’événements vérifie la propriété d’état afin de savoir si la reconnaissance a réussi. Il gère également le cas où l’utilisateur cesse de parler. Souvent, un [**TimeoutExceeded**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionResultStatus) est considérée comme réussi lorsque l’utilisateur a fini de parler. Vous devez gérer ce cas dans votre code afin d’offrir une expérience optimale.

    **Remarque**  le [ **ResultGenerated** ](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechcontinuousrecognitionsession.resultgenerated) événement est déclenché sur un thread d’arrière-plan qui ne peut pas mettre à jour l’interface utilisateur directement. Si un gestionnaire doit mettre à jour l’interface utilisateur (comme le \[exemple de reconnaissance vocale et synthèse vocale\] est), vous devez distribuer les mises à jour pour le thread d’interface utilisateur via la [ **RunAsync** ](https://docs.microsoft.com/uwp/api/windows.ui.core.coredispatcher.runasync) du répartiteur (méthode).
```csharp
private async void ContinuousRecognitionSession_Completed(
      SpeechContinuousRecognitionSession sender,
      SpeechContinuousRecognitionCompletedEventArgs args)
      {
        if (args.Status != SpeechRecognitionResultStatus.Success)
        {
          if (args.Status == SpeechRecognitionResultStatus.TimeoutExceeded)
          {
            await dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
            {
              rootPage.NotifyUser(
                "Automatic Time Out of Dictation",
                NotifyType.StatusMessage);

              DictationButtonText.Text = " Continuous Recognition";
              dictationTextBox.Text = dictatedTextBuilder.ToString();
            });
          }
          else
          {
            await dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
            {
              rootPage.NotifyUser(
                "Continuous Recognition Completed: " + args.Status.ToString(),
                NotifyType.StatusMessage);

              DictationButtonText.Text = " Continuous Recognition";
            });
          }
        }
      }
```

## <a name="provide-ongoing-recognition-feedback"></a>Fournir des commentaires en cours de reconnaissance


Lorsque des individus discutent, ces derniers s’appuient généralement sur le contexte pour déterminer le sens de la conversation. De la même manière, le module de reconnaissance vocale a souvent besoin de contexte pour fournir des résultats de reconnaissance très fiables. Par exemple, la différence entre les mots « poids » et « pois » est imperceptible tant qu’un contexte plus précis ne révèle leur sens. Tant que le module de reconnaissance n’est pas certain qu’un ou plusieurs mots ont été reconnus correctement, celui-ci ne déclenche pas l’événement [**ResultGenerated**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechcontinuousrecognitionsession.resultgenerated).

Cela peut entraîner une expérience peu agréable, dans la mesure où l’utilisateur continue de parler sans qu’aucun résultat n’apparaisse, jusqu’à ce que le moteur de reconnaissance soit suffisamment certain pour déclencher l’événement [**ResultGenerated**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechcontinuousrecognitionsession.resultgenerated).

Gérer l’événement [**HypothesisGenerated**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.hypothesisgenerated) pour améliorer ce manque de réactivité apparent. Cet événement est déclenché chaque fois que le moteur de reconnaissance vocale génère un nouveau jeu de correspondances potentielles pour le mot en cours de traitement. L’argument d’événement fournit une propriété [**Hypothesis**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognitionhypothesisgeneratedeventargs.hypothesis) contenant les correspondances en cours. Affichez ces correspondances à mesure que l’utilisateur continue de parler afin de leur montrer que le traitement est toujours actif. Une fois le niveau confiance est suffisamment élevé et que le résultat de la reconnaissance a été déterminé, remplacez les résultats **Hypothesis** temporaires par le dernier [**Result**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechcontinuousrecognitionresultgeneratedeventargs.result) indiqué dans l’événement [**ResultGenerated**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechcontinuousrecognitionsession.resultgenerated).

Ici, nous ajoutons le texte hypothétique et des points de suspension (« ... ») à la valeur actuelle de la sortie [**TextBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox). Le contenu de la zone de texte est mis à jour à mesure que de nouvelles hypothèses sont générées, jusqu’à ce que le résultat final soit obtenu à partir de l’événement [**ResultGenerated**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechcontinuousrecognitionsession.resultgenerated).

```CSharp
private async void SpeechRecognizer_HypothesisGenerated(
  SpeechRecognizer sender,
  SpeechRecognitionHypothesisGeneratedEventArgs args)
  {

    string hypothesis = args.Hypothesis.Text;
    string textboxContent = dictatedTextBuilder.ToString() + " " + hypothesis + " ...";

    await dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
    {
      dictationTextBox.Text = textboxContent;
      btnClearText.IsEnabled = true;
    });
  }
```

## <a name="start-and-stop-recognition"></a>Démarrer et arrêter la reconnaissance


Avant de démarrer une session de reconnaissance, vérifiez la valeur de la propriété [**State**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.state) du module de reconnaissance vocale. Le module de reconnaissance vocale doit être dans un état [**Idle**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognizerState).

Une fois la vérification de l’état du module de reconnaissance vocale terminée, nous commençons la session en appelant la méthode [**StartAsync**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechcontinuousrecognitionsession.startasync) de la propriété [**ContinuousRecognitionSession**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.continuousrecognitionsession) du module de reconnaissance vocale.

```CSharp
if (speechRecognizer.State == SpeechRecognizerState.Idle)
{
  await speechRecognizer.ContinuousRecognitionSession.StartAsync();
}
```

La reconnaissance peut être arrêtée de deux façons :

-   [**StopAsync** ](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechcontinuousrecognitionsession.stopasync) permet tout en attente d’événements de reconnaissance complètes ([**ResultGenerated** ](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechcontinuousrecognitionsession.resultgenerated) continue d’être levée jusqu'à ce que tous les en attente de la reconnaissance des opérations sont terminées).
-   [**CancelAsync** ](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechcontinuousrecognitionsession.cancelasync) termine la session de reconnaissance immédiatement et ignore les résultats en attente.

Une fois la vérification de l’état du module de reconnaissance vocale terminée, nous arrêtons la session en appelant la méthode [**CancelAsync**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechcontinuousrecognitionsession.cancelasync) de la propriété [**ContinuousRecognitionSession**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.continuousrecognitionsession) du module de reconnaissance vocale.

```CSharp
if (speechRecognizer.State != SpeechRecognizerState.Idle)
{
  await speechRecognizer.ContinuousRecognitionSession.CancelAsync();
}
```

> [!NOTE]
> Un événement [**ResultGenerated**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechcontinuousrecognitionsession.resultgenerated) peut se produire après l’appel de [**CancelAsync**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechcontinuousrecognitionsession.cancelasync).  
> En raison du multithreading, un événement [**ResultGenerated**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechcontinuousrecognitionsession.resultgenerated) peut rester dans la pile lorsque [**CancelAsync**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechcontinuousrecognitionsession.cancelasync) est appelé. Dans ce cas, l’événement **ResultGenerated** est quand même déclenché.  
> Si vous définissez tous les champs privés lors de l’annulation de la session de reconnaissance vocale, confirmez toujours leurs valeurs dans le gestionnaire [**ResultGenerated**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechcontinuousrecognitionsession.resultgenerated). Par exemple, ne supposez pas qu’un champ est initialisé dans votre gestionnaire si vous définissez ces champs comme nuls lorsque vous annulez la session.

 

## <a name="related-articles"></a>Articles associés


* [Interactions vocales](speech-interactions.md)

**Exemples**
* [La reconnaissance vocale et exemple de synthèse vocale](https://go.microsoft.com/fwlink/p/?LinkID=619897)
 

 




