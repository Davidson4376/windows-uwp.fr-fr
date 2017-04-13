---
author: Karl-Bridge-Microsoft
Description: "Découvrez comment gérer les problèmes liés à la précision de la reconnaissance vocale qu’entraîne une baisse de qualité des entrées audio."
title: "Gérer les problèmes liés aux entrées audio"
ms.assetid: 3E36C683-C96A-4FEE-AD52-FDB87E0CC299
label: Manage audio input issues
template: detail.hbs
keywords: "voix, vocal, reconnaissance vocale, langage naturel, dictée, saisie, interaction utilisateur"
ms.author: kbridge
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.openlocfilehash: 05986651e6197f189046b5d868b6ee27f1ba411e
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="manage-issues-with-audio-input"></a>Gérer les problèmes liés aux entrées audio
<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

Découvrez comment gérer les problèmes liés à la précision de la reconnaissance vocale qu’entraîne une baisse de qualité des entrées audio.

<div class="important-apis" >
<b>API importantes</b><br/>
<ul>
<li>[**SpeechRecognizer**](https://msdn.microsoft.com/library/windows/apps/dn653226)</li>
<li>[**RecognitionQualityDegrading**](https://msdn.microsoft.com/library/windows/apps/dn653243)</li>
<li>[**SpeechRecognitionAudioProblem**](https://msdn.microsoft.com/library/windows/apps/dn631406)</li>

</ul>
</div>


## <a name="assess-audio-input-quality"></a>Évaluer la qualité de l’entrée audio


Lorsque la reconnaissance vocale est active, utilisez l’événement [**RecognitionQualityDegrading**](https://msdn.microsoft.com/library/windows/apps/dn653243) de votre moteur de reconnaissance vocale pour déterminer si un ou plusieurs problèmes audio peuvent interférer avec la saisie vocale. L’argument d’événement ([**SpeechRecognitionQualityDegradingEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn631430)) fournit la propriété [**Problem**](https://msdn.microsoft.com/library/windows/apps/dn631431), qui décrit les problèmes détectés liés à l’entrée audio.

En effet, la fonction de reconnaissance vocale peut être affectée par un trop grand bruit de fond, un microphone muet ainsi que le volume ou la vitesse du haut-parleur.

Ici, nous configurons un module de reconnaissance vocale et nous commençons à écouter l’événement [**RecognitionQualityDegrading**](https://msdn.microsoft.com/library/windows/apps/dn653243).

```CSharp
private async void WeatherSearch_Click(object sender, RoutedEventArgs e)
{
    // Create an instance of SpeechRecognizer.
    var speechRecognizer = new Windows.Media.SpeechRecognition.SpeechRecognizer();

    // Listen for audio input issues.
    speechRecognizer.RecognitionQualityDegrading += speechRecognizer_RecognitionQualityDegrading;

    // Add a web search grammar to the recognizer.
    var webSearchGrammar = new Windows.Media.SpeechRecognition.SpeechRecognitionTopicConstraint(Windows.Media.SpeechRecognition.SpeechRecognitionScenario.WebSearch, "webSearch");


    speechRecognizer.UIOptions.AudiblePrompt = "Say what you want to search for...";
    speechRecognizer.UIOptions.ExampleText = @"Ex. &#39;weather for London&#39;";
    speechRecognizer.Constraints.Add(webSearchGrammar);

    // Compile the constraint.
    await speechRecognizer.CompileConstraintsAsync();

    // Start recognition.
    Windows.Media.SpeechRecognition.SpeechRecognitionResult speechRecognitionResult = await speechRecognizer.RecognizeWithUIAsync();
    //await speechRecognizer.RecognizeWithUIAsync();

    // Do something with the recognition result.
    var messageDialog = new Windows.UI.Popups.MessageDialog(speechRecognitionResult.Text, "Text spoken");
    await messageDialog.ShowAsync();
}
```

## <a name="manage-the-speech-recognition-experience"></a>Gérer l’expérience de la fonction de reconnaissance vocale


Utilisez la description fournie par la propriété [**Problem**](https://msdn.microsoft.com/library/windows/apps/dn631431) pour améliorer l’expérience de l’utilisateur en matière de reconnaissance vocale.

Ici, nous créons un gestionnaire pour l’événement [**RecognitionQualityDegrading**](https://msdn.microsoft.com/library/windows/apps/dn653243) qui recherche un faible volume. Nous pouvons ensuite utiliser un objet [**SpeechSynthesizer**](https://msdn.microsoft.com/library/windows/apps/dn298152) pour suggérer que l’utilisateur essaye de parler plus fort.

```CSharp
private async void speechRecognizer_RecognitionQualityDegrading(
    Windows.Media.SpeechRecognition.SpeechRecognizer sender,
    Windows.Media.SpeechRecognition.SpeechRecognitionQualityDegradingEventArgs args)
{
    // Create an instance of a speech synthesis engine (voice).
    var speechSynthesizer =
        new Windows.Media.SpeechSynthesis.SpeechSynthesizer();

    // If input speech is too quiet, prompt the user to speak louder.
    if (args.Problem == Windows.Media.SpeechRecognition.SpeechRecognitionAudioProblem.TooQuiet)
    {
        // Generate the audio stream from plain text.
        Windows.Media.SpeechSynthesis.SpeechSynthesisStream stream;
        try
        {
            stream = await speechSynthesizer.SynthesizeTextToStreamAsync("Try speaking louder");
            stream.Seek(0);
        }
        catch (Exception)
        {
            stream = null;
        }

        // Send the stream to the MediaElement declared in XAML.
        await CoreApplication.MainView.CoreWindow.Dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.High, () =>
        {
            this.media.SetSource(stream, stream.ContentType);
        });
    }
}
```

## <a name="related-articles"></a>Articles connexes


* [Interactions vocales](speech-interactions.md)

**Exemples**
* [Exemple de reconnaissance vocale et de synthèse vocale](http://go.microsoft.com/fwlink/p/?LinkID=619897)
 

 




