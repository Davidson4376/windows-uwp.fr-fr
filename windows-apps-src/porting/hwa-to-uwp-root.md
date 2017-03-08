---
author: seksenov
title: "Applications web hébergées - Convertir votre application web en application de plateforme Windows universelle (UWP) et accéder aux fonctionnalités Windows 10 natives"
description: "Découvrez les ressources permettant de convertir votre application web en une application de plateforme Windows universelle (UWP) pour le Windows Store."
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "applications web hébergées, convertir un site web en application Windows, applications web sur le Windows Store, applications Chrome pour Windows"
translationtype: Human Translation
ms.sourcegitcommit: 2e230e95be01f0b14fa6346be9fa836c66a812cf
ms.openlocfilehash: c9239f3a3c14bf9da99e60cfef8154eefb4305b4
ms.lasthandoff: 01/20/2017

---

# <a name="hosted-web-apps---access-windows-10-features-from-your-web-app"></a>Applications web hébergées - Accéder aux fonctionnalités Windows 10 à partir de votre application web

Votre application web hébergée peut avoir un accès complet à la plateforme Windows universelle (UWP), notamment l’appel des API Windows Runtime directement à partir du script hébergé sur un serveur, tirant parti de l’intégration de Cortana et utilisant un fournisseur d’authentification en ligne. Les applications hybrides sont également prises en charge pour que vous puissiez inclure du code local à appeler à partir du script hébergé et gérer la navigation entre les pages locales et distantes dans l’application.

## <a name="get-started"></a>Prise en main

Que vous utilisiez un PC ou un Mac, vous pouvez créer une application web hébergée en quelques minutes seulement. Gratuit et complet, [Visual Studio Community 2015](https://www.visualstudio.com/vs/community/) vous assure la meilleure prise en main, en particulier sur un appareil Windows. Si vous n’avez pas accès à Visual Studio, vous avez le choix entre d’autres options. Si vous êtes familiarisé avec les utilitaires d’interface de ligne de commande, consultez [ManifoldJS](http://manifoldjs.com/). Vous pouvez utiliser [App Studio](http://appstudio.windows.com/), un outil de création gratuit et en ligne ne nécessitant aucun codage qui vous permet de développer rapidement des applications Windows 10.

- [Instructions pas à pas pour convertir votre application web en application de plateforme Windows universelle (UWP) avec Windows](hwa-create-windows.md)

- [Instructions pas à pas pour convertir votre application web en application de plateforme Windows universelle (UWP) avec un Mac](hwa-create-mac.md)

## <a name="enhance-your-app"></a>Améliorer votre application

- Mettez votre application en valeur avec des [fonctionnalités Windows Runtime natives](hwa-access-features.md) que vous pouvez appeler directement à partir de JavaScript.

- Préservez la sécurité de votre application en définissant des règles d’URI de contenu d’application (ACUR) grâce à notre modèle de stratégie de sécurité de contenu (CSP, Content Security Policy).

- Exécutez votre application avec la puissance de la voix en intégrant les commandes Cortana.

- Accordez un accès par programme aux ressources utilisateur et fonctionnalités d’appareil en déclarant les fonctionnalités de l’application.

- Créez un flux de connexion simple et sans souci pour vos utilisateurs en vérifiant leur identité avec OpenID et OAuth.

- Ne choisissez plus entre une application web empaquetée ou hébergée. Vous pouvez avoir un peu des deux en créant une application hybride.

## <a name="convert-your-existing-chrome-app"></a>Convertir votre application Chrome existante

Nous avons simplifié le processus visant à [convertir votre application hébergée Chrome existante](hwa-chrome-conversion.md) en application web hébergée Windows. [ManifoldJS](http://manifoldjs.com/) accepte désormais les manifestes Chrome comme type d’entrée. En outre, nous avons développé un [outil d’interface de ligne de commande](https://github.com/MicrosoftEdge/hwa-cli) qui génère un package `.appx` à partir de vos fichiers `.zip` ou `.crx` existants.

## <a name="demos"></a>Démonstrations

- [Application Contoso Travel](http://contosotravel.azurewebsites.net/)


