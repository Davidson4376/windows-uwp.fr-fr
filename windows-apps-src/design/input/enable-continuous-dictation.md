---
author: Karl-Bridge-Microsoft
Description: Learn how to capture and recognize long-form, continuous dictation speech input.
title: Activer la dictée continue
ms.assetid: 383B3E23-1678-4FBB-B36E-6DE2DA9CA9DC
label: Continuous dictation
template: detail.hbs
keywords: voix, vocal, reconnaissance vocale, langage naturel, dictée, saisie, interaction utilisateur
ms.author: kbridge
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: ea7c0b92c5900e468023dd5b972942a89c2833c3
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/27/2018
ms.locfileid: "5686917"
---
# <a name="continuous-dictation"></a>Dictée continue

Découvrez comment capturer et reconnaître une entrée vocale dictée en continu et sur une longue durée.

> **API importantes**: [**SpeechContinuousRecognitionSession**](https://msdn.microsoft.com/library/windows/apps/dn913896), [**ContinuousRecognitionSession**](https://msdn.microsoft.com/library/windows/apps/dn913913)

Dans [Reconnaissance vocale](speech-recognition.md), vous avez appris à capturer et à reconnaître des saisies vocales de durée relativement courte à l’aide des méthodes [**RecognizeAsync**](https://msdn.microsoft.com/library/windows/apps/dn653244) ou [**RecognizeWithUIAsync**](https://msdn.microsoft.com/library/windows/apps/dn653245) d’un objet [**SpeechRecognizer**](https://msdn.microsoft.com/library/windows/apps/dn653226), par exemple, lorsque l’utilisateur compose un SMS ou pose une question.

Pour les sessions de reconnaissance vocale en continu plus longues, tel qu’un texte dicté ou un e-mail, utilisez la propriété [**ContinuousRecognitionSession**](https://msdn.microsoft.com/library/windows/apps/dn913913) d’un [**SpeechRecognizer**](https://msdn.microsoft.com/library/windows/apps/dn653226) afin d’obtenir un objet [**SpeechContinuousRecognitionSession**](https://msdn.microsoft.com/library/windows/apps/dn913896).

> [!NOTE]
> Prise en charge de la dictée dépend de l' [appareil](https://docs.microsoft.com/windows/uwp/design/devices/) où votre application est en cours d’exécution. Pour les PC et ordinateurs portables, uniquement en-US est reconnue, lorsqu’une Xbox et les téléphones peuvent reconnaître toutes les langues prises en charge par la reconnaissance vocale. Pour plus d’informations, consultez [spécifier la langue de reconnaissance vocale](specify-the-speech-recognizer-language.md).

## <a name="set-up"></a>Configuration

Votre application a besoin de plusieurs objets pour gérer une session de dictée continue :

- une instance d’un objet [**SpeechRecognizer**](https://msdn.microsoft.com/library/windows/apps/dn653226) ;
- une référence de répartiteur d’interface utilisateur afin de mettre à jour l’interface utilisateur lors de la dictée ;
- un dispositif de suivi des mots ajoutés par l’utilisateur.

Dans cet exemple, nous déclarons une instance [**SpeechRecognizer**](https://msdn.microsoft.com/library/windows/apps/dn653226) comme un champ privé de la classe code-behind. Votre application doit stocker une référence à un autre emplacement, si vous voulez que la dictée en continu se poursuive sur d’autres pages XAML.

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

Ici, nous utilisons un objet [**StringBuilder**](https://msdn.microsoft.com/library/system.text.stringbuilder.aspx) pour contenir tous les résultats de reconnaissance obtenus pendant la session. Les nouveaux résultats sont ajoutés à l’objet **StringBuilder** lors de leur traitement.

```CSharp
private StringBuilder dictatedTextBuilder;
```

## <a name="initialization"></a>Initialisation

Pendant l’initialisation de la reconnaissance vocale en continu, vous devez :

- Si vous mettez à jour l’interface utilisateur de votre application dans les gestionnaires d’événements de reconnaissance continue, récupérez le répartiteur pour le thread d’interface utilisateur.
- Initialisez le module de reconnaissance vocale.
- Compilez la grammaire de dictée intégrée.
    **Remarque**  la reconnaissance vocale nécessite au moins une contrainte pour définir un vocabulaire reconnaissable. Si aucune contrainte n’est spécifiée, une grammaire de dictée prédéfinie est utilisée. Voir [Reconnaissance vocale](speech-recognition.md).
- Configurez les détecteurs d’événements pour les événements de reconnaissance.

Nous initialisons la reconnaissance vocale dans l’événement de page [**OnNavigatedTo**](https://msdn.microsoft.com/library/windows/apps/br227508).

1. Comme les événements déclenchés par le module de reconnaissance vocale se produisent sur un thread d’arrière-plan, créez une référence pour le répartiteur pour les mises à jour du thread d’interface utilisateur. [**OnNavigatedTo**](https://msdn.microsoft.com/library/windows/apps/br227508) est toujours appelé sur le thread d’interface utilisateur.
```csharp
this.dispatcher = CoreWindow.GetForCurrentThread().Dispatcher;
```

2.  Nous initialisons ensuite l’instance [**SpeechRecognizer**](https://msdn.microsoft.com/library/windows/apps/dn653226).
```csharp
this.speechRecognizer = new SpeechRecognizer();
```

3.  Ensuite, nous ajoutons et nous compilons la grammaire qui définit tous les mots et toutes les expressions pouvant être reconnus par le [**SpeechRecognizer**](https://msdn.microsoft.com/library/windows/apps/dn653226).

    Si vous ne spécifiez explicitement de grammaire, la grammaire de dictée continue est utilisée par défaut. En règle générale, la grammaire par défaut est préférable pour la dictée générale.

    Ici, nous appelons [**CompileConstraintsAsync**](https://msdn.microsoft.com/library/windows/apps/dn653240) immédiatement sans ajouter de grammaire.

    
```csharp
SpeechRecognitionCompilationResult result =
      await speechRecognizer.CompileConstraintsAsync();
```

## <a name="handle-recognition-events"></a>Gérer les événements de reconnaissance

Ici, vous pouvez capturer un seul énoncé ou une seule expression de courte durée en appelant [**RecognizeAsync**](https://msdn.microsoft.com/library/windows/apps/dn653244) ou [**RecognizeWithUIAsync**](https://msdn.microsoft.com/library/windows/apps/dn653245). 

Toutefois, pour capturer une session de reconnaissance plus longue, continue, nous indiquons des détecteurs d’événements à exécuter en arrière-plan pendant que l’utilisateur parle et définissons des gestionnaires afin de créer la chaîne de dictée.

Nous pouvons ensuite utiliser la propriété [**ContinuousRecognitionSession**](https://msdn.microsoft.com/library/windows/apps/dn913913) de notre module de reconnaissance vocale pour obtenir un objet [**SpeechContinuousRecognitionSession**](https://msdn.microsoft.com/library/windows/apps/dn913896) qui fournit des méthodes et des événements pour la gestion d’une session de reconnaissance vocale en continu.

Deux événements sont particulièrement essentiels :

- [**ResultGenerated**](https://msdn.microsoft.com/library/windows/apps/dn913900), qui se produit lorsque le module de reconnaissance a généré des résultats.
- [**Completed**](https://msdn.microsoft.com/library/windows/apps/dn913899), qui se produit lorsque la session de reconnaissance en continu est terminée.

L’événement [**ResultGenerated**](https://msdn.microsoft.com/library/windows/apps/dn913900) est déclenché lorsque l’utilisateur parle. Le module de reconnaissance écoute l’utilisateur en continu et déclenche périodiquement un événement qui transmet un segment d’entrée vocale. Examinez l’entrée vocale à l’aide de la propriété [**Result**](https://msdn.microsoft.com/library/windows/apps/dn913895) de l’argument d’événement et prenez les mesures appropriées dans le gestionnaire d’événements, par exemple en ajoutant du texte à un objet StringBuilder.

En tant qu’instance de [**SpeechRecognitionResult**](https://msdn.microsoft.com/library/windows/apps/dn631432), la propriété [**Result**](https://msdn.microsoft.com/library/windows/apps/dn913895) permet d’indiquer si vous acceptez l’entrée vocale ou non: Une instance [**SpeechRecognitionResult**](https://msdn.microsoft.com/library/windows/apps/dn631432) fournit deux propriétés spécifiques :

- [**Status**](https://msdn.microsoft.com/library/windows/apps/dn631440) indique si la reconnaissance a réussi. La reconnaissance peut échouer pour diverses raisons.
- [**Confidence**](https://msdn.microsoft.com/library/windows/apps/dn631434) indique que le module de reconnaissance a relativement bien compris les mots énoncés.

Voici les étapes de base associées à la prise en charge de la reconnaissance continue :  

1. Ici, nous inscrivons le gestionnaire pour l’événement de reconnaissance en continu [**ResultGenerated**](https://msdn.microsoft.com/library/windows/apps/dn913900) de l’événement de page [**OnNavigatedTo**](https://msdn.microsoft.com/library/windows/apps/br227508).
```csharp
speechRecognizer.ContinuousRecognitionSession.ResultGenerated +=
        ContinuousRecognitionSession_ResultGenerated;
```

2.  Nous vérifions ensuite la propriété [**Confidence**](https://msdn.microsoft.com/library/windows/apps/dn631434). Si la valeur de confiance est de [**Medium**](https://msdn.microsoft.com/library/windows/apps/dn631409) ou plus, nous ajoutons du texte au StringBuilder. Nous mettons également à jour l’interface utilisateur lorsque nous collectons des entrées.

    **Remarque**l’événement [**ResultGenerated**](https://msdn.microsoft.com/library/windows/apps/dn913900) est déclenché sur un thread en arrière-plan qui ne peut pas mettre à jour l’interface utilisateur directement. Si un gestionnaire doit mettre à jour l’interface utilisateur (comme le fait [exemple vocal et TTS]), vous devez distribuer les mises à jour au thread d’interface utilisateur via la méthode [**RunAsync**](https://msdn.microsoft.com/library/windows/apps/hh750317) du répartiteur.
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

3.  Ensuite, nous gérons l’événement [**Completed**](https://msdn.microsoft.com/library/windows/apps/dn913899), qui indique la fin de la dictée continue.

    La session se termine lorsque vous appelez le [**StopAsync**](https://msdn.microsoft.com/library/windows/apps/dn913908) ou les méthodes [**CancelAsync**](https://msdn.microsoft.com/library/windows/apps/dn913898) (voir la section suivante). La session peut également se terminer lorsqu’une erreur se produit, ou lorsque l’utilisateur cesse de parler. Vérifiez la propriété [**Status**](https://msdn.microsoft.com/library/windows/apps/dn631440) de l’argument d’événement afin de déterminer pourquoi la session s’est terminée ([**SpeechRecognitionResultStatus**](https://msdn.microsoft.com/library/windows/apps/dn631433)).

    Ici, nous inscrivons le gestionnaire pour l’événement de reconnaissance en continu [**Completed**](https://msdn.microsoft.com/library/windows/apps/dn913899) de l’événement de page [**OnNavigatedTo**](https://msdn.microsoft.com/library/windows/apps/br227508).
```csharp
speechRecognizer.ContinuousRecognitionSession.Completed +=
      ContinuousRecognitionSession_Completed;
```

4.  Le gestionnaire d’événements vérifie la propriété d’état afin de savoir si la reconnaissance a réussi. Il gère également le cas où l’utilisateur cesse de parler. Souvent, un [**TimeoutExceeded**](https://msdn.microsoft.com/library/windows/apps/dn631433) est considérée comme réussi lorsque l’utilisateur a fini de parler. Vous devez gérer ce cas dans votre code afin d’offrir une expérience optimale.

    **Remarque**l’événement [**ResultGenerated**](https://msdn.microsoft.com/library/windows/apps/dn913900) est déclenché sur un thread en arrière-plan qui ne peut pas mettre à jour l’interface utilisateur directement. Si un gestionnaire doit mettre à jour l’interface utilisateur (comme le fait [exemple vocal et TTS]), vous devez distribuer les mises à jour au thread d’interface utilisateur via la méthode [**RunAsync**](https://msdn.microsoft.com/library/windows/apps/hh750317) du répartiteur.
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


Lorsque des individus discutent, ces derniers s’appuient généralement sur le contexte pour déterminer le sens de la conversation. De la même manière, le module de reconnaissance vocale a souvent besoin de contexte pour fournir des résultats de reconnaissance très fiables. Par exemple, la différence entre les mots «poids» et «pois» est imperceptible tant qu’un contexte plus précis ne révèle leur sens. Tant que le module de reconnaissance n’est pas certain qu’un ou plusieurs mots ont été reconnus correctement, celui-ci ne déclenche pas l’événement [**ResultGenerated**](https://msdn.microsoft.com/library/windows/apps/dn913900).

Cela peut entraîner une expérience peu agréable, dans la mesure où l’utilisateur continue de parler sans qu’aucun résultat n’apparaisse, jusqu’à ce que le moteur de reconnaissance soit suffisamment certain pour déclencher l’événement [**ResultGenerated**](https://msdn.microsoft.com/library/windows/apps/dn913900).

Gérer l’événement [**HypothesisGenerated**](https://msdn.microsoft.com/library/windows/apps/dn913914) pour améliorer ce manque de réactivité apparent. Cet événement est déclenché chaque fois que le moteur de reconnaissance vocale génère un nouveau jeu de correspondances potentielles pour le mot en cours de traitement. L’argument d’événement fournit une propriété [**Hypothesis**](https://msdn.microsoft.com/library/windows/apps/dn913911) contenant les correspondances en cours. Affichez ces correspondances à mesure que l’utilisateur continue de parler afin de leur montrer que le traitement est toujours actif. Une fois le niveau confiance est suffisamment élevé et que le résultat de la reconnaissance a été déterminé, remplacez les résultats **Hypothesis** temporaires par le dernier [**Result**](https://msdn.microsoft.com/library/windows/apps/dn913895) indiqué dans l’événement [**ResultGenerated**](https://msdn.microsoft.com/library/windows/apps/dn913900).

Ici, nous ajoutons le texte hypothétique et des points de suspension (« ... ») à la valeur actuelle de la sortie [**TextBox**](https://msdn.microsoft.com/library/windows/apps/br209683). Le contenu de la zone de texte est mis à jour à mesure que de nouvelles hypothèses sont générées, jusqu’à ce que le résultat final soit obtenu à partir de l’événement [**ResultGenerated**](https://msdn.microsoft.com/library/windows/apps/dn913900).

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


Avant de démarrer une session de reconnaissance, vérifiez la valeur de la propriété [**State**](https://msdn.microsoft.com/library/windows/apps/dn913915) du module de reconnaissance vocale. Le module de reconnaissance vocale doit être dans un état [**Idle**](https://msdn.microsoft.com/library/windows/apps/dn653227).

Une fois la vérification de l’état du module de reconnaissance vocale terminée, nous commençons la session en appelant la méthode [**StartAsync**](https://msdn.microsoft.com/library/windows/apps/dn913901) de la propriété [**ContinuousRecognitionSession**](https://msdn.microsoft.com/library/windows/apps/dn913913) du module de reconnaissance vocale.

```CSharp
if (speechRecognizer.State == SpeechRecognizerState.Idle)
{
  await speechRecognizer.ContinuousRecognitionSession.StartAsync();
}
```

La reconnaissance peut être arrêtée de deux façons :

-   [**StopAsync**](https://msdn.microsoft.com/library/windows/apps/dn913908) vous permet d’attendre des événements de reconnaissance complets ([**ResultGenerated**](https://msdn.microsoft.com/library/windows/apps/dn913900) continue d’être déclenché jusqu’à ce que toutes les opérations de reconnaissance en attente soit terminées).
-   [**CancelAsync**](https://msdn.microsoft.com/library/windows/apps/dn913898) arrête la session de reconnaissance immédiatement et ignore les résultats en attente.

Une fois la vérification de l’état du module de reconnaissance vocale terminée, nous arrêtons la session en appelant la méthode [**CancelAsync**](https://msdn.microsoft.com/library/windows/apps/dn913898) de la propriété [**ContinuousRecognitionSession**](https://msdn.microsoft.com/library/windows/apps/dn913913) du module de reconnaissance vocale.

```CSharp
if (speechRecognizer.State != SpeechRecognizerState.Idle)
{
  await speechRecognizer.ContinuousRecognitionSession.CancelAsync();
}
```

> [!NOTE]
> Un événement [**ResultGenerated**](https://msdn.microsoft.com/library/windows/apps/dn913900) peut se produire après l’appel de [**CancelAsync**](https://msdn.microsoft.com/library/windows/apps/dn913898).  
> En raison du multithreading, un événement [**ResultGenerated**](https://msdn.microsoft.com/library/windows/apps/dn913900) peut rester dans la pile lorsque [**CancelAsync**](https://msdn.microsoft.com/library/windows/apps/dn913898) est appelé. Dans ce cas, l’événement **ResultGenerated** est quand même déclenché.  
> Si vous définissez tous les champs privés lors de l’annulation de la session de reconnaissance vocale, confirmez toujours leurs valeurs dans le gestionnaire [**ResultGenerated**](https://msdn.microsoft.com/library/windows/apps/dn913900). Par exemple, ne supposez pas qu’un champ est initialisé dans votre gestionnaire si vous définissez ces champs comme nuls lorsque vous annulez la session.

 

## <a name="related-articles"></a>Articles connexes


* [Interactions vocales](speech-interactions.md)

**Exemples**
* [Exemple de reconnaissance vocale et de synthèse vocale](http://go.microsoft.com/fwlink/p/?LinkID=619897)
 

 




