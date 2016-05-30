---
author: mcleanbyron
ms.assetid: 9ca1f880-2ced-46b4-8ea7-aba43d2ff863
description: Découvrez les problèmes connus de la version actuelle des bibliothèques de publicités Microsoft contenues dans le Kit de développement logiciel (SDK) d’engagement et de monétisation de la Boutique Microsoft.
title: Problèmes connus des bibliothèques de publicités Microsoft
---

# Problèmes connus des bibliothèques de publicités Microsoft


\[ Mise à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

Cette rubrique répertorie les problèmes connus liés à la version actuelle des bibliothèques de publicités Microsoft contenues dans le Kit de développement logiciel (SDK) d’engagement et de monétisation de la Boutique Microsoft.

## Visual Studio Tools pour applications Windows universelles requis pour l’installation

Pour installer le [Kit de développement logiciel (SDK) d’engagement et de monétisation de la Boutique Microsoft](http://aka.ms/store-em-sdk) avec Visual Studio 2015, vous devez avoir installé la version 1.1 ou ultérieure de Visual Studio Tools pour applications Windows universelles. Pour plus d’informations, consultez les [notes de publication](http://go.microsoft.com/fwlink/?LinkID=624516) de Visual Studio.

## Projets Silverlight Windows Phone 8.x

Pour obtenir les assemblys de publicités Microsoft pour des projets Silverlight Windows Phone 8.x, installez le [Kit de développement logiciel (SDK) d’engagement et de monétisation de la Boutique Microsoft](http://aka.ms/store-em-sdk), ouvrez votre projet dans Visual Studio, puis accédez à **Projet** > **Ajouter un service connecté** > **Ad Mediator** pour télécharger automatiquement les assemblys. Après quoi, vous pouvez supprimer les références au médiateur publicitaire dans votre projet si vous ne souhaitez pas utiliser la médiation publicitaire. Pour plus d’informations, voir [AdControl dans Silverlight Windows Phone](adcontrol-in-windows-phone-silverlight.md).

## Interface AdControl inconnue en XAML

Le balisage XAML d’un contrôle [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) peut afficher incorrectement une ligne courbe bleue impliquant que l’interface est inconnue. Ce problème se produit uniquement lors d’un ciblage x86, et peut être ignoré.

## Élément lastError de la demande de publicité précédente

S’il reste un élément **lastError** de la demande de publicité précédente, l’événement peut être déclenché deux fois durant le prochain appel de publicité. Si la nouvelle demande de publicité est toujours effectuée et peut générer une publicité valide, ce comportement peut cependant prêter à confusion.

## Spots publicitaires et boutons de navigation sur les téléphones

Sur les téléphones (ou émulateurs) pourvus des boutons logiciels **Précédent**, **Démarrer** et **Rechercher** au lieu des boutons matériels, le compte à rebours minuterie et les boutons de clic publicitaire des spots vidéo publicitaires risquent d’être masqués.

## Les publicités récemment créées ne sont pas fournies à votre application

Si vous avez créé une publicité récemment (moins d’un jour), elle peut ne pas être disponible immédiatement. Si le contenu éditorial de la publicité a été approuvé, cette publicité est fournie à l’application une fois que le serveur de publicités l’a traitée. Elle est alors disponible en stock.

## Aucune publicité n’est affichée dans votre application

Plusieurs raisons peuvent provoquer le non-affichage des publicités, notamment des erreurs réseau. Autres raisons possibles :

* Vous avez sélectionné une unité publicitaire dans le Centre de développement Windows dont la taille est supérieure ou inférieure à la taille du **AdControl** dans le code de votre application.

* Les publicités ne s’affichent pas si vous utilisez une [valeur du mode test](test-mode-values.md) pour votre ID d’unité publicitaire lors de l’exécution d’une application dynamique.

* Si vous avez créé un ID d’unité publicitaire dans la dernière demi-heure, la publicité risque de ne pas s’afficher tant que les serveurs n’ont pas propagé les nouvelles données dans le système. Les ID existants qui affichaient des publicités précédemment doivent en afficher immédiatement.

Si vous pouvez voir des publicités de test dans l’application, c’est que votre code fonctionne et qu’il peut afficher des publicités. Si vous rencontrez des problèmes, contactez le [support produit](https://go.microsoft.com/fwlink/p/?LinkId=331508). Dans cette page, choisissez **Publicité intégrée à l’application**.

Vous pouvez également publier une question sur le [forum](http://go.microsoft.com/fwlink/p/?LinkId=401266).

## Les publicités de test s’affichent dans votre application à la place des publicités dynamiques

Les publicités de test peuvent s’afficher même lorsque vous attendez des publicités dynamiques. Cela peut se produire dans les cas suivants :

* Microsoft Advertising ne peut pas vérifier ni trouver l’ID d’application dynamique utilisé dans la boutique d’applications. Dans ce cas, lorsqu’une unité publicitaire est créée par un utilisateur, son état peut démarrer à dynamique (non-test), mais passer à l’état de test dans les 6 heures qui suivent la première demande de publicité. Il revient à l’état dynamique en cas d’absence de demandes d’applications de test pendant 10 jours.

* Les applications chargées indépendamment ou les applications qui sont exécutées dans l’émulateur n’affichent pas de publicités dynamiques.

Si une unité publicitaire dynamique fournit des publicités de test, l’état de l’unité publicitaire indique **Publicités de test actives et fournies** dans le Centre de développement Windows. Pour le moment, cela ne s’applique pas aux applications téléphoniques.

## Les valeurs de test obsolètes d’ID d’unité publicitaire et d’ID d’application ne fonctionnent plus

Les valeurs de test suivantes pour les applications Silverlight Windows Phone sont obsolètes et ne fonctionnent plus. Si vous disposez d’un projet qui utilise ces valeurs de test, mettez à jour votre projet pour utiliser les valeurs fournies dans [Valeurs du mode test](test-mode-values.md).

| ID de l’application  |  ID d’unité publicitaire    |
|-----------------|----------------|
| test_client     |  Image320_50   |
| test_client     |  Image300_50   |
| test_client     |  TextAd   |
| test_client     |  Image480_80   |

<span id="reference_errors"/>
## Erreurs de référence provoquées par le ciblage de Toute UC dans votre projet

Lorsque vous utilisez les bibliothèques de publicités Microsoft, vous ne pouvez pas cibler **Toute UC** dans votre projet. Si votre projet cible la plateforme **Toute UC**, un message d’avertissement peut s’afficher après que vous avez ajouté une référence semblable à ce qui suit.

![referenceerror\-solutionexplorer](images/13-19629921-023c-42ec-b8f5-bc0b63d5a191.jpg)

Pour supprimer cet avertissement, mettez à jour votre projet pour utiliser une sortie de génération propre à l’architecture (par exemple, **x86**). Utilisez le **Gestionnaire de configurations** pour définir les cibles de plateforme pour déboguer et publier les configurations.

![configurationmanagerwin10](images/13-87074274-c10d-4dbd-9a06-453b7184f8de.png)

Lorsque vous créez vos packages d’application pour les soumettre au Windows Store (comme illustré dans les images suivantes), veillez à inclure les architectures que vous souhaitez cibler. Vous pouvez choisir d’ignorer x64 si vous prévoyez d’exécuter des builds x86 sur le système d’exploitation x64.

![projectstorecreateapppackages](images/13-a99b05a4-8917-4c53-822e-2548fadf828a.png)

![createapppackages](images/13-16280cb1-a838-42b9-9256-eac7f33f5603.png)

## Ordre de plan dans les applications JavaScript/HTML

Les applications HTML/JavaScript ne doivent pas placer d’éléments dans la plage MAX-10 réservée de l’ordre de plan. La seule exception est une superposition d’interruptions, par exemple une notification d’appel entrant pour une application Skype.

<span id="bkmk-ui"/>
## Ne pas utiliser de bordures

La définition des propriétés associées aux bordures, héritées par la classe **AdControl** de sa classe parente entraîne le placement erroné de la publicité.

## Plus d’informations


Pour plus d’informations sur les derniers problèmes connus et pour publier des questions liées aux bibliothèques de publicités Microsoft, visitez le [forum](http://go.microsoft.com/fwlink/p/?LinkId=401266).

## Support


Pour contacter le support produit à propos des problèmes liés aux bibliothèques de publicités Microsoft, visitez la [page de support](https://go.microsoft.com/fwlink/p/?LinkId=331508) et choisissez **Publicité intégrée à l’application**.

 

 


<!--HONumber=May16_HO2-->


