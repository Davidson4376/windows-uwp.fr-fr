---
author: andrewleader
Description: Discover the different options desktop Win32 apps have for sending toast notifications
title: Notifications toast à partir d'applications de bureau
label: Toast notifications from desktop apps
template: detail.hbs
ms.author: mijacobs
ms.date: 05/01/2018
ms.topic: article
keywords: windows10, uwp, win32, bureau, notifications toast, pont du bureau, options pour l’envoi de notifications toast, serveur com, activateur com, com, com faux, aucune com, sans com, envoyer toast
ms.localizationpriority: medium
ms.openlocfilehash: 2ee44d990ef29fa8281a14c8ad03b6c6ff16a4cf
ms.sourcegitcommit: 753e0a7160a88830d9908b446ef0907cc71c64e7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/29/2018
ms.locfileid: "5748737"
---
# <a name="toast-notifications-from-desktop-apps"></a>Notifications toast à partir d'applications de bureau

Les applications de bureau (Pont du bureau et Win32 classique) peuvent envoyer des notifications toast interactives, tout comme les applications de plateforme Windows universelle (UWP). Toutefois, il existe quelques options différentes pour les applications de bureau en raison des différences des schémas d’activation.

Dans cet article, nous répertorions les options à votre disposition pour envoyer une notification toast sous Windows10. Chaque option prend entièrement en charge...

* la persistance dans le centre de notifications;
* la possibilité d'être activable depuis le menu contextuel et l’intérieur du centre de maintenance;
* la possibilité d'être activable pendant que votre fichier EXE n’est pas en cours d’exécution.

## <a name="all-options"></a>Toutes les options

Le tableau ci-dessous illustre les options de prise en charge des toasts au sein de votre application de bureau, ainsi que les fonctionnalités prises en charge correspondantes. Vous pouvez utiliser ce tableau pour sélectionner la meilleure option pour votre scénario.<br/><br/>

| Option | Éléments visuels | Actions | Entrées | S'active dans le processus |
| -- | -- | -- | -- | -- |
| [Activateur COM](#preferred-option---com-activator) | ✔️ | ✔️ | ✔️ | ✔️ |
| [Pas de COM/Stub CLSID](#alternative-option---no-com--stub-clsid) | ✔️ | ✔️ | ❌ | ❌ |


## <a name="preferred-option---com-activator"></a>Option par défaut - activateur de COM

Il s’agit de l’option par défaut qui fonctionne pour le Pont du bureau et Win32 classique et qui prend en charge toutes les fonctionnalités de notification. N’ayez pas peur de «l'activateur COM». Nous avons une bibliothèque [pour les applications C#](send-local-toast-desktop.md) et [C++](send-local-toast-desktop-cpp-wrl.md) qui rend cela très simple, même si vous n’avez jamais écrit de serveur COM.<br/><br/>

| Éléments visuels | Actions | Entrées | S'active dans le processus |
| -- | -- | -- | -- |
| ✔️ | ✔️ | ✔️ | ✔️ |

Avec l’option d'activateur COM, vous pouvez utiliser les modèles de notification et les types d’activation suivants dans votre application.<br/><br/>

| Type de modèle et d’activation | Pont du bureau | Win32classique |
| -- | -- | -- |
| ToastGeneric au premier plan | ✔️ | ✔️ |
| ToastGeneric à l'arrière-plan | ✔️ | ✔️ |
| Protocole ToastGeneric | ✔️ | ✔️ |
| Modèles hérités | ✔️ | ❌ |

> [!NOTE]
> Si vous ajoutez l’activateur COM à votre application de Pont du bureau existante, les activations de notification Premier plan/Arrière-plan et Héritées activeront maintenant votre activateur COM au lieu de votre ligne de commande.

Pour savoir comment utiliser cette option, consultez [Envoyer une notification toast locale à partir d'applications de bureau en C#](send-local-toast-desktop.md) ou [Envoyer une notification toast locale à partir d'applications de bureau en C++ WRL](send-local-toast-desktop-cpp-wrl.md).


## <a name="alternative-option---no-com--stub-clsid"></a>Autre option - Pas de COM/Stub CLSID

Il s’agit d’une autre option si vous ne pouvez pas mettre en place d'activateur COM. Toutefois, vous devrez sacrifier quelques fonctionnalités, telles que la prise en charge des entrées (zones de texte dans les toasts) et l’activation dans le processus.<br/><br/>

| Éléments visuels | Actions | Entrées | S'active dans le processus |
| -- | -- | -- | -- |
| ✔️ | ✔️ | ❌ | ❌ |

Avec cette option, si vous prenez en charge Win32 classique, vous êtes beaucoup plus limité dans les modèles de notification et les types d’activation que vous pouvez utiliser, comme illustré ci-dessous.<br/><br/>

| Type de modèle et d’activation | Pont du bureau | Win32classique |
| -- | -- | -- |
| ToastGeneric au premier plan | ✔️ | ❌ |
| ToastGeneric à l'arrière-plan | ✔️ | ❌ |
| Protocole ToastGeneric | ✔️ | ✔️ |
| Modèles hérités | ✔️ | ❌ |

Nous publierons à l’avenir des documents montrant comment utiliser cette option. Essentiellement, pour les applications de Pont du bureau, il suffit d'envoyer les notifications toast comme le ferait une application UWP. Lorsque l’utilisateur clique sur votre notification toast, votre application est lancée par ligne de commande avec les arguments de lancement que vous avez spécifié dans la notification toast.

Pour les applications Win32classique, configurez l’AUMID afin de pouvoir envoyer des toasts et spécifiez également un CLSID sur votre raccourci. Il peut s’agir de n’importe quel GUID. N’ajoutez pas le serveur/l'activateur COM. Vous ajoutez un «stub» COM CLSID, ce qui obligera le centre de notifications à conserver la notification. Notez que vous ne pouvez utiliser que des toasts d’activation de protocole, puisque le stub CLSID bloque l’activation de toutes les autres activations toast. Par conséquent, vous devez mettre à jour votre application afin qu'elle prenne en charge l’activation de protocole et que le protocole de toasts active votre propre application.


## <a name="resources"></a>Ressources

* [Envoyer une notification toast locale depuis des applications de bureau en C#](send-local-toast-desktop.md)
* [Envoyer une notification toast locale depuis des applications de bureau en C++ WRL](send-local-toast-desktop-cpp-wrl.md)
* [Documentation sur le contenu des toasts](adaptive-interactive-toasts.md)