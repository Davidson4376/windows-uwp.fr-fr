---
ms.assetid: E0189423-1DF3-4052-AB2E-846EA18254C4
description: Cette rubrique décrit les effets conçus en vue d’être utilisés pour les scénarios de capture vidéo. Cela inclut l’effet de stabilisation vidéo.
title: Effets de capture vidéo
---

# Effets de capture vidéo

\[ Mise à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

Cette rubrique décrit les effets conçus en vue d’être utilisés pour les scénarios de capture vidéo. Cela inclut l’effet de stabilisation vidéo.

**Remarque**  
Cet article repose sur les concepts et sur le code décrits dans [Capturer des photos et des vidéos à l’aide de MediaCapture](capture-photos-and-video-with-mediacapture.md), qui détaille les étapes d’implémentation de capture photo et vidéo de base. Il est recommandé de vous familiariser avec le modèle de capture multimédia de base dans cet article avant de passer à des scénarios de capture plus avancés. Le code de cet article part du principe que votre application possède déjà une instance de MediaCapture initialisée correctement.

## Effet de stabilisation vidéo

L’effet de stabilisation vidéo manipule les cadres d’un flux vidéo pour réduire les secousses provoquées lorsque vous tenez l’appareil de capture dans votre main. Étant donné que cette technique décale les pixels vers la droite, vers la gauche, vers le haut ou vers le bas, et que l’effet ne peut pas déterminer quel est le contenu à l’extérieur du cadre vidéo, la vidéo stabilisée est légèrement rognée par rapport à la vidéo d’origine. Une fonction utilitaire est fournie pour vous permettre d’ajuster vos paramètres d’encodage vidéo pour gérer au mieux le rognage généré par l’effet.

Sur les appareils qui la prennent en charge, la stabilisation d’image optique (OIS, Optical Image Stabilization) stabilise la vidéo en manipulant mécaniquement l’appareil de capture et, par conséquent, il n’est pas nécessaire de rogner les bords des images vidéo. Pour plus d’informations, voir [Contrôles de l’appareil de capture pour la vidéo](capture-device-controls-for-video-capture.md).

### Configurer votre application pour utiliser la stabilisation vidéo

Outre les espaces de noms nécessaires pour la capture multimédia de base, l’effet de stabilisation vidéo requiert les espaces de noms suivants.

[!code-cs[VideoStabilizationEffectUsing](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetVideoStabilizationEffectUsing)]

Déclarez une variable membre pour stocker l’objet [**VideoStabilizationEffect**](https://msdn.microsoft.com/library/windows/apps/dn926760). Dans le cadre de l’implémentation de l’effet, vous allez modifier les propriétés d’encodage que vous utilisez pour coder la vidéo capturée. Déclarez deux variables pour stocker une copie de sauvegarde des données initiales et les propriétés d’encodage de sortie afin de pouvoir les restaurer ultérieurement lorsque l’effet sera désactivé. Enfin, déclarez une variable membre de type [**MediaEncodingProfile**](https://msdn.microsoft.com/library/windows/apps/hh701026), car cet objet est accessible à partir de différents emplacements au sein de votre code.

[!code-cs[DeclareVideoStabilizationEffect](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetDeclareVideoStabilizationEffect)]

Dans l’implémentation de capture vidéo de base décrite dans l’article [Capturer des photos et vidéos à l’aide de MediaCapture](capture-photos-and-video-with-mediacapture.md), l’objet de profil d’encodage multimédia est affecté à une variable locale, car celle-ci n’est utilisée nulle part ailleurs dans le code. Pour ce scénario, vous devez affecter l’objet à une variable membre afin de pouvoir y accéder ultérieurement.

[!code-cs[EncodingProfileMember](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetEncodingProfileMember)]

### Initialiser l’effet de stabilisation vidéo

Une fois l’objet **MediaCapture** initialisé, créez une instance de l’objet [**VideoStabilizationEffectDefinition**](https://msdn.microsoft.com/library/windows/apps/dn926762). Appelez [**MediaCapture.AddVideoEffectAsync**](https://msdn.microsoft.com/library/windows/apps/dn878035) pour ajouter l’effet au pipeline vidéo et récupérer une instance de la classe [**VideoStabilizationEffect**](https://msdn.microsoft.com/library/windows/apps/dn926760). Spécifiez [**MediaStreamType.VideoRecord**](https://msdn.microsoft.com/library/windows/apps/br226640) pour indiquer que l’effet doit être appliqué au flux d’enregistrement vidéo.

Inscrivez un gestionnaire d’événements pour l’événement [**EnabledChanged**](https://msdn.microsoft.com/library/windows/apps/dn948982) et appelez la méthode d’assistance **SetUpVideoStabilizationRecommendationAsync**. Ces deux thèmes seront décrits plus loin dans cet article. Enfin, définissez la propriété [**Enabled**](https://msdn.microsoft.com/library/windows/apps/dn926775) de l’effet sur la valeur True pour activer l’effet.

[!code-cs[CreateVideoStabilizationEffect](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCreateVideoStabilizationEffect)]

### Utiliser les propriétés d’encodage recommandées

Comme indiqué précédemment dans cet article, la technique utilisée par l’effet de stabilisation vidéo provoque un rognage léger de la vidéo stabilisée par rapport à la vidéo source. Définissez la fonction d’assistance suivante dans votre code afin d’ajuster les propriétés d’encodage vidéo pour mieux gérer cette limitation provoquée par l’effet. Cette étape n’est pas requise pour utiliser l’effet de stabilisation vidéo, mais si vous ne l’effectuez pas, la vidéo sera légèrement améliorée et par conséquent, la fidélité visuelle ne sera plus d’aussi bonne qualité.

Appelez [**GetRecommendedStreamConfiguration**](https://msdn.microsoft.com/library/windows/apps/dn948983) dans l’instance d’effet de stabilisation vidéo, en passant l’objet [**VideoDeviceController**](https://msdn.microsoft.com/library/windows/apps/br226825), qui informe l’effet sur les propriétés actuelles d’encodage de flux d’entrée, et votre **MediaEncodingProfile**, qui informe l’effet sur les propriétés actuelles d’encodage de sortie. Cette méthode renvoie un objet [**VideoStreamConfiguration**](https://msdn.microsoft.com/library/windows/apps/dn926727) contenant de nouvelles recommandations de propriétés d’encodage de flux d’entrée et de sortie.

Lorsque l’appareil le permet, les propriétés d’encodage d’entrée recommandées ont une résolution plus élevée que les paramètres initiaux fournis, de manière à ce que la perte de résolution soit minimale une fois l’effet de rognage appliqué.

Appelez [**VideoDeviceController.SetMediaStreamPropertiesAsync**](https://msdn.microsoft.com/library/windows/apps/hh700895) pour définir les nouvelles propriétés d’encodage. Avant de définir les nouvelles propriétés, utilisez la variable membre pour stocker les propriétés d’encodage initiales afin de pouvoir modifier de nouveau les paramètres lorsque l’effet sera désactivé.

Si l’effet de stabilisation vidéo doit rogner la vidéo de sortie, les propriétés d’encodage de sortie recommandées auront la taille de la vidéo rognée. Cela signifie que la résolution de sortie correspondra à la taille de la vidéo rognée. Si vous n’utilisez pas les propriétés de sortie recommandées, la vidéo est redimensionnée pour correspondre à la taille initiale de sortie, ce qui se traduit par une perte de fidélité visuelle.

Définissez la propriété [**Video**](https://msdn.microsoft.com/library/windows/apps/hh701124) sur l’objet **MediaEncodingProfile**. Avant de définir les nouvelles propriétés, utilisez la variable membre pour stocker les propriétés d’encodage initiales afin de pouvoir modifier de nouveau les paramètres lorsque l’effet sera désactivé.

[!code-cs[SetUpVideoStabilizationRecommendationAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetSetUpVideoStabilizationRecommendationAsync)]

### Gérer la désactivation de l’effet de stabilisation vidéo

Le système peut désactiver automatiquement l’effet de stabilisation vidéo si le débit de pixels est trop élevé ou s’il détecte que l’effet s’exécute lentement. Dans ce cas, l’événement EnabledChanged est déclenché. L’instance **VideoStabilizationEffect** du paramètre *sender* indique le nouvel état de l’effet, activé ou désactivé. [
            **VideoStabilizationEffectEnabledChangedEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn948979)a une valeur [**VideoStabilizationEffectEnabledChangedReason**](https://msdn.microsoft.com/library/windows/apps/dn948981) indiquant pourquoi l’effet a été activé ou désactivé. Cet événement est également déclenché si vous activez ou désactivez l’effet par programme, auquel cas la raison sera **Programmatic**.

En règle générale, cet événement vous permet d’ajuster l’interface utilisateur de votre application pour indiquer l’état actuel de stabilisation vidéo.

[!code-cs[VideoStabilizationEnabledChanged](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetVideoStabilizationEnabledChanged)]

### Nettoyer l’effet de stabilisation vidéo

Pour nettoyer l’effet de stabilisation vidéo, appelez [**ClearEffectsAsync**](https://msdn.microsoft.com/library/windows/apps/br226592) pour effacer tous les effets du pipeline vidéo. Si les variables membres contenant les propriétés d’encodage initiales ne sont pas null, utilisez-les pour restaurer les propriétés d’encodage. Pour finir, supprimez le gestionnaire d’événements **EnabledChanged** et définissez l’effet sur la valeur null.

[!code-cs[CleanUpVisualStabilizationEffect](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCleanUpVisualStabilizationEffect)]

## Rubriques connexes

* [Capturer des photos et des vidéos à l’aide de MediaCapture](capture-photos-and-video-with-mediacapture.md)
 

 




<!--HONumber=Mar16_HO1-->
