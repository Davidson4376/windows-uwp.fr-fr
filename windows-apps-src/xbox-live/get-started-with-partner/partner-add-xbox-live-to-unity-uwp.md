---
title: Unity pour UWP avec des scripts de .NET
description: Ajouter le support de Xbox Live à Unity pour UWP avec le backend de script .NET pour ID@Xbox et gérés des partenaires
ms.assetid: 790a49ad-eff4-4916-8578-968ca8483211
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, jeux, uwp, windows 10, xbox un, Unity
ms.localizationpriority: medium
ms.openlocfilehash: 8c4ca9d58f89e215563adcc7985b978641efdf07
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57594084"
---
# <a name="add-xbox-live-support-to-unity-for-uwp-with-net-scripting-backend-for-idxbox-and-managed-partners"></a>Ajouter le support de Xbox Live à Unity pour UWP avec le backend de script .NET pour ID@Xbox et gérés des partenaires

**(1) installer Unity**

Installer Unity 5.3 ou version ultérieure, lors de l’unité processus d’installation, vérifiez le composant « Windows Store .NET script principal ».

![](../images/unity/unity1-install.png)

**(2) Ouvrez un projet Unity nouveau ou existant**

Il peut être un projet 2D ou 3D. Des types fonctionneront avec le Kit de développement Xbox Live.

**(3) Importer la dernière version du package Xbox Live WinRT Unity asset que cela, consultez https://github.com/Microsoft/xbox-live-api/releases**

**(4) ajoutez et attacher un nouveau C\# script à un objet de Unity.**

Par exemple, cliquez sur un objet Unity tels que « Main Camera », puis cliquez sur « Ajouter un composant » \| « Nouveau Script » \| C\# Script \| et nommez-le « XboxLiveScript ». N’importe quel objet de jeu fera l’affaire.

**(5) Générez le projet dans Unity.**

1.  Accédez au fichier \| paramètres de Build, cliquez sur le Windows Store et assurez-vous que vous cliquez sur « Commutateur plateforme »

2.  Cliquez sur « Ajouter Open coulisses » pour ajouter la scène actuelle à la build

3.  Dans la zone de liste déroulante de kit de développement logiciel, choisissez « 10 universelle »

4.  Dans la zone de liste déroulante type de build UWP, choisir « D3D », mais « XAML » fonctionne également si vous préférez.

5.  Cliquez sur le « Unity C\# projets « case à cocher pour générer les projets Assembly-Csharp.dll

6.  Cliquez sur « Build » pour Unity générer le projet UWP de Visual Studio qui encapsule votre jeu Unity dans une application UWP. Lorsque vous êtes invité pour un emplacement, créez un dossier pour éviter toute confusion, car un grand nombre de nouveaux fichiers sera créé. Il est recommandé de vous appelez le dossier « Build », puis sélectionnez ce dossier

![](../images/unity/unity3-buildsettings.png)


**(6) Ouvrez le projet UWP généré dans Visual Studio**

Unity s’ouvre le dossier de sortie de projet dans l’Explorateur.  Ignorer le fichier .sln.  Au lieu de cela, accédez dans votre dossier de Build et ouvrez le .sln généré dans Visual Studio.  

Vous verrez 3 projets dans cette solution.

1.  Assembly-CSharp. Il s’agit où résident vos scripts de Xbox Live

2.  Assembly-Csharp-firstpass. Ce projet peut être ignoré dans notre cas.

3.  Application UWP en fonction du nom de votre projet. Il s’agit d’une application UWP traditionnelle qui héberge le moteur Unity. Voici où vous le définirez certains configuration Xbox Live similaire à une application UWP traditionnelle.


**(7) ajouter de la configuration de Xbox Live à l’application UWP**

Suivez la page de documentation intitulée [Ajout Xbox Live à un projet UWP nouveau ou existant](get-started-with-visual-studio-and-uwp.md)

**(8) ajouter du code de Xbox Live à votre script**

Copiez/collez cet exemple de code de Xbox Live dans le script que vous avez attaché à l’objet de jeu. Ce script s’affiche dans le projet « Assembly-CSharp ». Vous pouvez modifier le code comme vous le souhaitez.

```csharp
#if NETFX_CORE

using UnityEngine;
using System;
using Microsoft.Xbox.Services.System;

public class XboxLiveScript : MonoBehaviour
{
    Microsoft.Xbox.Services.System.XboxLiveUser m_user = new Microsoft.Xbox.Services.System.XboxLiveUser();
    Microsoft.Xbox.Services.XboxLiveContext m_xboxLiveContext = null;
    Windows.UI.Core.CoreDispatcher UIDispatcher = null;
    string debugText = "";

    void Start()
    {
        Windows.ApplicationModel.Core.CoreApplicationView mainView = Windows.ApplicationModel.Core.CoreApplication.MainView;
        Windows.UI.Core.CoreWindow cw = mainView.CoreWindow;

        UIDispatcher = cw.Dispatcher;
        SignIn();
    }

    void Update()
    {
    }

    void OnGUI()
    {
        GUI.Label(new UnityEngine.Rect(10, 10, 300, 50), debugText);
    }

    async void SignIn()
    {
        SignInResult result = await m_user.SignInAsync(UIDispatcher);

        if (result.Status == SignInStatus.Success)
        {
            m_xboxLiveContext = new Microsoft.Xbox.Services.XboxLiveContext(m_user);
            debugText += "\n User signed in: " + m_xboxLiveContext.User.Gamertag;
        }
    }
}

#endif
```

**9) permet de compiler et exécuter l’application UWP à partir de Visual Studio**

Cela lance l’application comme une application UWP normale et permettre les appels de Xbox Live à fonctionner comme ils ont besoin d’un conteneur d’application UWP à la fonction.

**10) reconstruction si vous apportez des modifications à n’importe quel élément Unity**  
Si vous modifiez quelque chose dans Unity, vous devez reconstruire le projet UWP.

Notez que Unity remplacera votre fichier pfx lorsque vous recompilez ce qui provoquera le Xbox Live connectez-vous à échouer, vous devez la mettre à jour à l’intérieur du projet Unity pour éviter ce problème.

Pour ce faire, accédez au fichier \| paramètres de Build, cliquez sur « Paramètres de Build » sur le lecteur Windows Store et cliquez sur le bouton PFX pour remplacer le fichier PFX avec celui que vous avez obtenu ci-dessus. Vous pouvez également supprimer le fichier PFX à chaque fois que vous regénérez le projet à partir de Unity.

## <a name="troubleshooting-common-issues"></a>Dépannage des problèmes courants

**1)** si Unity a qu’un script associé ne peut pas être chargé, puis vérifiez que vous avez fait l’étape 3 pour faire glisser du WinMD vers le panneau de ressources du projet Unity

**2)** si l’application bloque dès lors du démarrage ou lorsque vous tentez d’exécuter cette ligne de code :

    Microsoft.Xbox.Services.System.XboxLiveUser m_user = new Microsoft.Xbox.Services.System.XboxLiveUser();

Assurez-vous de qu'avoir ajouté un fichier de texte xboxservices.config au projet et dans ses propriétés, jeu « Build Action » à « Contenu » et ensemble de « Répertoire de sortie » à « Toujours copier ».

> [!NOTE]
> Toutes les valeurs à l’intérieur de xboxservices.config respectent la casse.

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

(d) le fichier .pfx stock fourni par Unity n’auront pas l’identité appropriée afin que soit supprimer le disque et supprimez la ligne dans le fichier .csproj qui y fait référence, ou de droite, cliquez sur le projet dans Visual Studio et choisissez « Store » \| « associer App avec le Store « qui place un fichier .pfx approprié.  Veillez ensuite à accédez revenir dans Unity, cliquez sur « Paramètres de Build » sur le lecteur Windows Store, puis cliquez sur le bouton PFX pour remplacer le fichier .pfx avec celle que vous avez obtenu à partir de l’action de « Associer des applications avec Store » de Visual Studio.
