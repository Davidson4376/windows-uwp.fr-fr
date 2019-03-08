---
title: Unity pour UWP avec serveur principal IL2CPP
description: Ajouter le support de Xbox Live à Unity pour UWP avec le serveur principal de script IL2CPP pour ID@Xbox et gérés des partenaires
ms.assetid: 790a49ad-eff4-4916-8578-968ca8483211
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, jeux, uwp, windows 10, xbox un, Unity
ms.localizationpriority: medium
ms.openlocfilehash: ace950dec6a57a9550ea7b3fbe6c52b53855e2e0
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57622914"
---
# <a name="add-xbox-live-support-to-unity-for-uwp-with-il2cpp-scripting-backend-for-idxbox-and-managed-partners"></a>Ajouter le support de Xbox Live à Unity pour UWP avec le serveur principal de script IL2CPP pour ID@Xbox et gérés des partenaires

## <a name="overview"></a>Vue d’ensemble

Prise en charge de Windows Runtime pour IL2CPP dans Unity

Avec la version de Unity 5.6f3 le moteur a inclus une nouvelle fonctionnalité qui permet aux développeurs d’utiliser des composants Windows Runtime (WinRT) directement dans le script en les incluant dans le projet de jeu directement. Jusqu'à ce que les 5.6 développeurs nécessité un plug-in, ou une dll pour prendre en charge des fonctionnalités de plate-forme (y compris Xbox Live SDK) à partir du script de jeu dans UWP. Cette nouvelle couche de projection supprime la nécessité de plug-in et introduit un flux de travail simplifiée et nouvelle prise en charge uniquement avec les jeux que choisissez le serveur principal de script IL2CPP.

Pour plus d’informations sur la prise en main, consultez la documentation Unity : https://docs.unity3d.com/Manual/IL2CPP-WindowsRuntimeSupport.html

## <a name="steps"></a>Étapes

**(1) installer Unity**

Installer Unity 5.6 ou version ultérieure et assurez-vous d’avoir le **back-end de script Windows Store Il2CPP** sélectionnée pendant l’installation

**(2) installez Visual Studio Tools pour Unity version 3.1 et ci-dessus pour IntelliSense prend en charge lors de l’utilisation de Winmd** pour Visual Studio 2015, cela peut être, consultez https://marketplace.visualstudio.com/items?itemName=SebastienLebreton.VisualStudio2015ToolsforUnity.  Pour Visual Studio 2017, le composant peut être ajouté à l’intérieur du programme d’installation de Visual Studio 2017.

**(3) Ouvrez un projet Unity nouveau ou existant**

**(4) commutateur la plateforme pour la plateforme Windows universelle dans le menu Paramètres de Build Unity**

**(5) activer IL2CPP de script principal dans les paramètres du lecteur Unity et défini la compatibilité d’API sur .NET 4.6**

![](../images/unity/unity-il2cpp-1.png)

**(6) importation la dernière version du package Xbox Live WinRT Unity asset** cela, consultez https://github.com/Microsoft/xbox-live-api/releases

**(7) ajouter et attacher un nouveau C\# script à un objet de Unity.**

Par exemple, cliquez sur un objet Unity tels que « Main Camera », puis cliquez sur « Ajouter un composant » \| « Nouveau Script » \| C\# Script \| et nommez-le « XboxLiveScript ». N’importe quel objet de jeu fera l’affaire.

**(8) Ouvrez le script dans Visual Studio (avec VSTU 3.1 + installé)**

Vous remarquerez deux projets, ouvrez votre script de jeu XboxLiveTest.cs dans le projet « Lecteur » généré par VSTU

![](../images/unity/unity-il2cpp-2.png)

Ceci est un projet spécifique est généré pour UWP et inclut des références pour les fichiers winmd que vous avez placés dans vos éléments multimédias.
Elle définit également la « #if ENABLE_WINMD_SUPPORT » définir pour vous pour IntelliSense et la coloration syntaxique fonctionne correctement.

**9) ajoutez le code de Xbox Live suivant au fichier source XboxLiveTest.cs**

```csharp

using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using System;
public class XboxLiveTest : MonoBehaviour
{
#if ENABLE_WINMD_SUPPORT
    Microsoft.Xbox.Services.System.XboxLiveUser m_user = new   Microsoft.Xbox.Services.System.XboxLiveUser();

    Microsoft.Xbox.Services.XboxLiveContext m_xboxLiveContext = null;
    Windows.UI.Core.CoreDispatcher UIDispatcher = null;
#endif
    string debugText = "";
    // Use this for initialization
    void Start()
    {
#if ENABLE_WINMD_SUPPORT
        Windows.ApplicationModel.Core.CoreApplicationView mainView = Windows.ApplicationModel.Core.CoreApplication.MainView;
        Windows.UI.Core.CoreWindow cw = mainView.CoreWindow;
        UIDispatcher = cw.Dispatcher;
        SignIn();
#endif
    }
    // Update is called once per frame
    void Update()
    {
    }
    void OnGUI()
    {
        GUI.Label(new UnityEngine.Rect(10, 10, 300, 50), debugText);
    }
#if ENABLE_WINMD_SUPPORT
    async void SignIn()
    {
        Microsoft.Xbox.Services.System.SignInResult result = await m_user.SignInAsync(UIDispatcher);
        if (result.Status == Microsoft.Xbox.Services.System.SignInStatus.Success)
        {
            m_xboxLiveContext = new Microsoft.Xbox.Services.XboxLiveContext(m_user);
            debugText += "\n User signed in: " + m_xboxLiveContext.User.Gamertag;
        }

    }
#endif
}

```

**(10) Vérifiez que vous avez fonctionnalité « InternetClient » sélectionnée dans les paramètres de publication trouvés dans les paramètres du lecteur**

![](../images/unity/unity-il2cpp-3.png)

**11) Générez le projet dans Unity.**

1.  Accédez au fichier \| paramètres de Build, cliquez sur **plateforme Windows universelle** et assurez-vous que vous cliquez sur **plateforme de commutation**

2.  Cliquez sur « Ajouter Open coulisses » pour ajouter la scène actuelle à la build

3.  Dans la zone de liste déroulante de kit de développement logiciel, choisissez « 10 universelle »

4.  Dans la zone de liste déroulante type de build UWP, choisir « D3D », mais « XAML » fonctionne également si vous préférez.

5.  Cliquez sur « Build » pour Unity générer le projet UWP de Visual Studio qui encapsule votre jeu Unity dans une application UWP. Lorsque vous êtes invité pour un emplacement, créez un dossier pour éviter toute confusion, car un grand nombre de nouveaux fichiers sera créé. Il est recommandé de vous appelez le dossier « Build », puis sélectionnez ce dossier

**12) ajouter la configuration de Xbox Live à votre projet**

Ajoutez le fichier xboxservices.config :

![](../images/unity/unity-il2cpp-4.png)

Suivez la page de documentation intitulée [Ajout Xbox Live à un projet UWP nouveau ou existant](get-started-with-visual-studio-and-uwp.md)

> [!NOTE]
> Toutes les valeurs à l’intérieur de xboxservices.config respectent la casse.

**13) permet de compiler et exécuter l’application UWP à partir de Visual Studio**

Cela lance l’application comme une application UWP normale et permettre les appels de Xbox Live à fonctionner comme ils ont besoin d’un conteneur d’application UWP à la fonction.

**14) reconstruction si vous apportez des modifications à n’importe quel élément Unity**  
Si vous modifiez quelque chose dans Unity, vous devez reconstruire le projet UWP.

Notez que Unity remplacera votre fichier pfx lorsque vous recompilez ce qui provoquera le Xbox Live connectez-vous à échouer, vous devez la mettre à jour à l’intérieur du projet Unity pour éviter ce problème.

Pour ce faire, accédez au fichier \| paramètres de Build, cliquez sur « Paramètres de Build » sur le **plateforme Windows universelle** player et cliquez sur le bouton PFX pour remplacer le fichier PFX de fichiers avec celle que vous avez obtenu ci-dessus. Vous pouvez également supprimer le fichier PFX à chaque fois que vous regénérez le projet à partir de Unity.

## <a name="troubleshooting-common-issues"></a>Dépannage des problèmes courants

**1)** si Unity a qu’un script associé ne peut pas être chargé, puis vérifiez que vous avez fait l’étape 3 pour faire glisser du WinMD vers le panneau de ressources du projet Unity

**2)** si l’application se bloque immédiatement au démarrage ou lorsque vous tentez d’exécuter cette ligne de code :

    Microsoft.Xbox.Services.System.XboxLiveUser m_user = new Microsoft.Xbox.Services.System.XboxLiveUser();

Assurez-vous de qu'avoir ajouté un fichier de texte xboxservices.config au projet et dans ses propriétés, jeu « Build Action » à « Contenu » et ensemble de « Répertoire de sortie » à « Toujours copier ».
Vérifiez également qu’il contient JSON mise en forme correcte avec le n ° titre sous forme décimale, telles que :

```json
{
    "TitleId" : 928643728,
    "PrimaryServiceConfigId" : "3ebd0100-ace5-4aa4-ab9c-5b733759fa90"
}
```

**3)** si l’application démarre, mais ne parvient pas à se connecter puis vérifiez les éléments suivants :

(a) votre machine est défini sur le votre sandbox du développeur.  Pour ce faire, utilisez le script SwitchSandbox.cmd dans le dossier \Tools du SDK Xbox Live.

(b) vous vous connectez avec un compte Xbox Live qui a accès à la sandbox du développeur.  Comptes de vente au détail normal Xbox Live n’ont pas accès.  Vous pouvez utiliser XDP ou partenaires pour créer des comptes de test.

(c) votre package.appxmanfiest dans votre application UWP est défini sur l’identité appropriée.  Vous pouvez modifier ce champ manuellement, mais la plus simple pour résoudre ce problème consiste à un clic droit sur le projet dans Visual Studio et choisissez « Store » \| « Associer des applications avec Store ».

(d) le fichier .pfx stock fourni par Unity n’auront pas l’identité appropriée afin que soit supprimer le disque et supprimez la ligne dans le fichier .csproj qui y fait référence, ou de droite, cliquez sur le projet dans Visual Studio et choisissez « Store » \| « associer App avec le Store « qui place un fichier .pfx approprié.  N’oubliez pas ensuite pour revenir à Unity, cliquez sur « Paramètres de Build » sur le **plateforme Windows universelle** player et cliquez sur le bouton PFX pour remplacer le fichier .pfx avec la table que vous avez obtenu à partir de l’action de « Associer des applications avec Store » de Visual Studio.
