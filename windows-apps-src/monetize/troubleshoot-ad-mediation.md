---
author: mcleanbyron
Description: Voici quelques solutions à plusieurs problèmes de développement courants liés à la médiation publicitaire.
title: Résoudre les problèmes liés à la médiation publicitaire
ms.assetid: 8728DE4F-E050-4217-93D3-588DD3280A3A
---

# Résoudre les problèmes liés à la médiation publicitaire


\[ Mise à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

Voici quelques solutions à plusieurs problèmes de développement courants liés à la médiation publicitaire.

**Vous ne pouvez pas ajouter de contrôle AdMediatorControl à l’aire de conception.**  
Lorsque vous faites glisser le contrôle **AdMediatorControl** vers le concepteur pour la première fois dans un projet UWP, Windows 8.1 ou Windows Phone 8.1 en C# ou Visual Basic avec XAML, Visual Studio ajoute la référence appropriée d’assembly Ad Mediator à votre projet, mais le contrôle n’est, à ce stade, pas encore ajouté au concepteur. Pour ajouter le contrôle, cliquez sur OK dans le message apparaissant dans Visual Studio, attendez quelques secondes que le concepteur s’actualise, puis faites glisser de nouveau le contrôle vers le concepteur.

Si vous ne réussissez toujours pas à ajouter le contrôle dans le concepteur, vérifiez que votre projet cible l’architecture de processeur requise par votre application (par exemple, **x86**), et non pas **Toute CPU**. Le contrôle ne peut pas être ajouté au concepteur si le projet cible **Toute CPU** pour la plateforme de génération.

*
            *L’élément AdMediatorControl indique l’erreur « &lt;*largeur* &gt; x &lt;*hauteur*&gt; non pris en charge » au moment de l’exécution lorsque la fourniture de publicités à partir de Microsoft**Microsoft Advertising ne prend en charge que [certaines tailles de publicités recommandées par l’IAB (Interactive Advertising Bureau)](add-and-use-the-ad-mediator-control.md#supported-ad-sizes-for-microsoft-advertising). Dans certains cas, même si vous définissez la hauteur et la largeur du contrôle ad mediator dans le concepteur ou dans votre code XAML sur l’une de ces tailles de publicités prises en charge, des problèmes d’arrondi et de mise à l’échelle peuvent empêcher l’infrastructure de médiation publicitaire de fournir une publicité. Pour éviter ce problème, définissez les paramètres facultatifs **Largeur**et**Hauteur** de Microsoft Advertising dans votre code sur l’une des tailles de publicités prises en charge.

L’exemple de code suivant explique comment affecter les paramètres facultatifs **Largeur** et **Hauteur** associés à Microsoft Advertising sur 728 x 90.

```CSharp
myAdMediatorControl.AdSdkOptionalParameters[AdSdkNames.MicrosoftAdvertising]["Width"] = 728;
myAdMediatorControl.AdSdkOptionalParameters[AdSdkNames.MicrosoftAdvertising]["Height"] = 90;
```

**La médiation publicitaire n’inclut pas de localisation (latitude/longitude) pour le réseau publicitaire**  
Si vous activez la fonctionnalité de localisation dans votre application, le contrôle Ad Mediator récupère automatiquement les coordonnées de latitude/longitude et les fournit aux réseaux publicitaires qui les prennent en charge.

**Le contrôle publicitaire Smaato ne s’aligne pas correctement**  
Essayez d’utiliser les paramètres facultatifs pour définir les valeurs des contrôles du Kit de développement logiciel (SDK) :

```CSharp
myAdMediatorControl.AdSdkOptionalParameters[AdSdkNames.Smaato]["Margin"] = new Thickness(0, -20, 0, 0);
myAdMediatorControl.AdSdkOptionalParameters[AdSdkNames.Smaato]["Width"] = 50d;
myAdMediatorControl.AdSdkOptionalParameters[AdSdkNames.Smaato]["Height"] = 320d;
```

**Le contrôle publicitaire AdDuplex ne s’affiche pas avec la taille correcte (il s’affiche au format 250x250)**  
La médiation publicitaire ne définit pas de valeur pour la taille ; vous devez la modifier à l’aide du paramètre facultatif **Taille**. Exemple :

```CSharp
myAdMediatorControl.AdSdkOptionalParameters[AdSdkNames.AdDuplex]["Size"] = "160x600";
```

**Vous recevez l’erreur « Un élément recouvre le contrôle publicitaire »**  
AdDuplex affiche toujours une erreur si la publicité est masquée d’une quelconque manière au sein de votre application. [Lisez la solution](http://blog.adduplex.com/2014/01/solving-something-is-covering-ad.mdl) AdDuplex à cette erreur.

**Vous recevez l’erreur « Un conflit s’est produit entre deux fichiers »**  
Vous avez référencé les assemblys Microsoft Advertising ailleurs dans votre application. La médiation publicitaire est conçue pour fonctionner exclusivement dans votre application ; elle ne fonctionnera pas si d’autres références aux assemblys Microsoft Advertising sont utilisées. Supprimez manuellement les références Microsoft Advertising et réinstallez le SDK d’engagement et de monétisation de la Boutique Microsoft pour effacer l’erreur.

**Vous rencontrez un comportement inattendu après avoir modifié la valeur RefreshRate dans le fichier AdMediator.config**

Une fois que vous avez configuré vos réseaux publicitaires en exécutant le composant **Ad Mediator** dans la boîte de dialogue **Ajouter un service connecté** de Visual Studio, les informations de configuration par défaut sont enregistrées dans le fichier AdMediator.config de votre projet. Vous ne devez pas modifier ce fichier directement. Au lieu de cela, vous pouvez modifier ces informations (y compris la fréquence d’actualisation des publicités) lorsque vous [configurez les paramètres de médiation publicitaire pour votre application](submit-your-app-and-configure-ad-mediation.md) après avoir chargé votre package d’application vers le tableau de bord du Centre de développement Windows.

Si vous modifiez la valeur **RefreshRate** dans le fichier AdMediator.config, gardez à l’esprit que cette valeur doit contenir un entier compris entre 30 et 120, qui représente la fréquence d’actualisation en secondes. Si vous définissez cette valeur sur un entier inférieur à 30 ou supérieur à 120, l’infrastructure de médiation publicitaire utilise automatiquement une fréquence d’actualisation de 60 secondes.

## Rubriques connexes

* [Sélectionner et gérer vos réseaux publicitaires](select-and-manage-your-ad-networks.md)
* [Ajouter et utiliser le contrôle de médiation publicitaire](add-and-use-the-ad-mediator-control.md)
* [Tester l’implémentation de votre médiation publicitaire](test-your-ad-mediation-implementation.md)
* [Soumettre votre application et configurer une médiation publicitaire](submit-your-app-and-configure-ad-mediation.md)
 

 


<!--HONumber=May16_HO2-->


