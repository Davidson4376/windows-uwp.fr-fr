---
author: PatrickFarley
title: "Communiquer avec un service d’application distant"
description: "Échanges des messages avec un service d’application exécuté sur un appareil distant à l’aide de projet «Rome»."
translationtype: Human Translation
ms.sourcegitcommit: c90304b7ca3f7185fca9146aa2303b09cba5ab9a
ms.openlocfilehash: bff77a63d0f88907410c74d4dce19fb422c1bd3f

---

# Communiquer avec un service d’application distant

En plus de lancer une application à l’aide d’un URI, vous pouvez exécuter des *services d’application* et communiquer avec eux sur des appareils distants. Un appareil fonctionnant sous Windows peut servir d’appareil source, d’appareil cible ou des deux. Ce qui vous donne un nombre quasiment illimité de modes d’interaction avec les appareils connectés, sans avoir besoin d’amener une application au premier plan.

## Configurer le service d’application sur l’appareil cible
Pour exécuter un service d’application sur un appareil distant, un fournisseur de ce service d’application doit être installé sur l’appareil cible. Ce guide utilise le service d’application Générateur de nombres aléatoires, qui est disponible dans le [référentiel d’exemples d’application Windows universelle](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/AppServices). Pour obtenir des instructions sur la rédaction du code de votre service d’application, consultez [Créer et utiliser un service d’application](how-to-create-and-consume-an-app-service.md).

Que vous utilisiez un service d’application prêt à l’emploi ou créé par vos soins, vous devez lui apporter quelques modifications pour le rendre compatible avec les systèmes distants. Dans VisualStudio, accédez au projet du fournisseur du service d’application et sélectionnez son fichier Package.appxmanifest. Cliquez sur le bouton droit et sélectionnez **Afficher le code** pour afficher le contenu du fichier. Recherchez l’élément **Extension** qui définit le projet comme un service d’application et indique le nom de son projet parent.

``` xml
...
<Extensions>
    <uap:Extension Category="windows.appService" EntryPoint="RandomNumberService.RandomNumberGeneratorTask">
        <uap:AppService Name="com.microsoft.randomnumbergenerator"/>
    </uap:Extension>
</Extensions>
...
```

Modifiez l’espace de noms de l’élément **AppService** en **uap3** et ajoutez l’attribut **SupportsRemoteSystems**:

``` xml
...
<uap3:AppService Name="com.microsoft.randomnumbergenerator" SupportsRemoteSystems="true"/>
...
```

Pour utiliser les éléments dans ce nouvel espace de noms, vous devez ajouter la définition de ce dernier en haut du fichier de manifeste.

``` xml
<?xml version="1.0" encoding="utf-8"?>
<Package
  xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
  xmlns:mp="http://schemas.microsoft.com/appx/2014/phone/manifest"
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3">
  ...
</Package>
```

Créez votre projet de fournisseur de service d’application et déployez-le sur le ou les appareils cibles.

## Cibler le service d’application à partir de l’appareil source
L’appareil *à partir duquel* le service d’application distant doit être appelé a besoin de la fonctionnalité Systèmes distants. Vous pouvez l’ajouter dans l’application qui fournit le service d’application sur l’appareil cible (auquel cas la même application doit être installée sur les deuxappareils) ou dans une autre.

Les instructions **using** suivantes sont nécessaires pour que le code de cette section s’exécute tel quel:

[!code-cs[Main](./code/RemoteAppService/MainPage.xaml.cs#SnippetUsings)]


Vous devez d’abord instancier un objet [**AppServiceConnection**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.AppService.AppServiceConnection), comme si vous vouliez appeler un service d’application localement. Ce processus est décrit plus en détail dans [Créer et utiliser un service d’application](how-to-create-and-consume-an-app-service.md). Dans cet exemple, le service d’application à cibler est le service Générateur de nombres aléatoires.

> [!NOTE]
> On suppose qu’un objet [RemoteSystem](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems.RemoteSystem) a déjà été acquis par un moyen quelconque dans le code qui appelle la méthode suivante. Pour obtenir des instructions sur ce type de configuration, consultez [Lancer une application sur un appareil distant](launch-a-remote-app.md).

[!code-cs[Main](./code/RemoteAppService/MainPage.xaml.cs#SnippetAppService)]

Ensuite, un objet [**RemoteSystemConnectionRequest**](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems.RemoteSystemConnectionRequest) est créé pour l’appareil distant concerné. Il est utilisé pour ouvrir la connexion **AppServiceConnection** à cet appareil. Notez que dans l’exemple ci-dessous, le traitement et le signalement des erreurs sont grandement simplifiés par souci de concision.

[!code-cs[Main](./code/RemoteAppService/MainPage.xaml.cs#SnippetRemoteConnection)]

À ce stade, vous devez avoir une connexion ouverte à un service d’application sur un ordinateur distant.

## Échanger des messages avec le service sur la connexion à distance

Maintenant, vous pouvez échanger des messages avec le service sous la forme d’objets [**ValueSet**](https://msdn.microsoft.com/library/windows/apps/windows.foundation.collections.valueset). Pour plus d’informations, consultez [Créer et utiliser un service d’application](how-to-create-and-consume-an-app-service.md). Le service Générateur de nombres aléatoires prend deuxentiers avec les clés `"minvalue"` et `"maxvalue"` comme entrées, sélectionne de manière aléatoire un entier entre ces deuxvaleurs et le renvoie au processus appelant avec la clé `"Result"`.

[!code-cs[Main](./code/RemoteAppService/MainPage.xaml.cs#SnippetSendMessage)]

Maintenant que vous êtes connecté à un service d’application sur un appareil cible, exécutez une opération sur cet appareil et recevez les données sur votre appareil source.

## Rubriques connexes

[Applications et appareils connectés (projet «Rome»)](connected-apps-and-devices.md)  
[Lancer une application sur un appareil distant](launch-a-remote-app.md)  
[Créer et utiliser un service d’application](how-to-create-and-consume-an-app-service.md)  
[Référence sur l’API Systèmes distants](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems)  
L’[exemple Systèmes distants](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/RemoteSystems ) montre comment détecter un système distant, lancer une application sur un système distant et utiliser des services d’application pour échanger des messages avec des applications qui s’exécutent sur deuxsystèmes.



<!--HONumber=Aug16_HO3-->


