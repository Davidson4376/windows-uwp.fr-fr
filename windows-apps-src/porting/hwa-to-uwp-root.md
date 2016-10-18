---
title: "Apps web hébergées - Convertir votre appweb en app UWP et accéder aux fonctionnalités Windows10 natives"
description: "Créez une application de plateforme Windows universelle (UWP) à partir de l’URL de votre site web. Accédez aux fonctionnalités d’appareil natives Windows10 à partir du code de votre application web. Avec les ponts Windows Microsoft (anciennement appelés Project Westminster) pour applications web hébergées, vous pouvez rapidement et facilement inclure votre application web dans le Windows Store."
author: seksenov
translationtype: Human Translation
ms.sourcegitcommit: 7fe6e240e4ef221b49f9b103cf30192449ce4502
ms.openlocfilehash: 95d50aa37f349f494f260ea3af97211a085623a9

---

# Applications web hébergées - Accéder aux fonctionnalités Windows10 à partir de votre application web

Votre application web hébergée peut avoir un accès complet à la plateforme Windows universelle (UWP), notamment l’appel des API Windows Runtime directement à partir du script hébergé sur un serveur, tirant parti de l’intégration de Cortana et utilisant un fournisseur d’authentification en ligne. Les applications hybrides sont également prises en charge pour que vous puissiez inclure du code local pour être appelé à partir du script hébergé et gérer la navigation entre les pages locales et distantes dans l’application.

## Prise en main

Que vous utilisiez un PC ou un Mac, vous pouvez créer votre propre application web hébergée en quelques minutes seulement. Gratuit et complet, [Visual Studio Community2015](https://www.visualstudio.com/) vous assure la meilleure prise en main, en particulier depuis un appareil Windows. Si vous n’avez pas accès à Visual Studio, vous avez le choix entre d’autres options. Si vous êtes familiarisé avec les utilitaires d’interface de ligne de commande, consultez [ManifoldJS](http://manifoldjs.com/). Vous pouvez utiliser [App Studio](http://appstudio.windows.com/), un outil de création gratuit et en ligne ne nécessitant aucun codage qui vous permet de développer rapidement des applications Windows 10.

- [Instructions pas à pas pour convertir votre application web en application de plateforme Windows universelle (UWP) avec Windows](hwa-create-windows.md)

- [Instructions pas à pas pour convertir votre application web en application de plateforme Windows universelle (UWP) avec Mac](hwa-create-mac.md)

## Améliorer votre application

- Faites étinceler votre application [en accédant aux fonctionnalités Windows natives](hwa-access-features.md) dans JavaScript à partir de Windows Runtime.

- Préservez la sécurité de votre application en définissant des règles URI de contenu de l’application (ACUR) grâce à notre modèle Content Security Policy (CSP).
- Exécutez votre application avec la puissance de la voix en intégrant les commandes vocales Cortana.

- Accordez un accès par programme aux ressources utilisateur et fonctionnalités d’appareil en déclarant les fonctionnalités de l’application.

- Créez un flux de connexion simple et sans souci pour vos utilisateurs en vérifiant leur identité avec OpenID et OAuth.

- Ne choisissez plus entre une applicationweb empaquetée ou hébergée. Vous pouvez avoir un peu des deux en créant une application hybride.

## Convertir votre application Chrome existante

Nous avons simplifié le processus visant à [convertir votre application hébergée Chrome existante](hwa-chrome-conversion.md) en application web hébergée Windows. [ManifoldJS](http://manifoldjs.com/) accepte désormais les manifestes Chrome comme type d’entrée. En outre, nous avons développé un [outil d’interface de ligne de commande](https://github.com/MicrosoftEdge/hwa-cli) qui génère un package `.appx` à partir de vos fichiers `.zip` ou `.crx` existants.

## Démos

- [Application Contoso Travel](http://contosotravel.azurewebsites.net/)

- [API Windows Runtime: exemples de code JavaScript](http://rjs.azurewebsites.net/)



<!--HONumber=Aug16_HO3-->


