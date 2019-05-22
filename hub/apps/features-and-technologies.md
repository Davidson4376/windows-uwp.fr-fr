---
Description: Cette section vous permet de comprendre comment certaines fonctionnalités clées de Windows sont prises en charge dans les plateformes d’applications différents et comment commencer à utiliser les fonctionnalités dans votre code.
title: Fonctionnalités et technologies
ms.topic: article
ms.date: 05/08/2019
ms.openlocfilehash: ff91d8c01e6832e645cc857b638851e1833fc3f9
ms.sourcegitcommit: f0f933d5cf0be734373a7b03e338e65000cc3d80
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65984979"
---
# <a name="features-and-technologies-for-windows-apps"></a>Fonctionnalités et technologies pour les applications Windows

Quel type d’application que vous soyez bâtiment ou l’appareil que vous ciblez, Windows prend en charge de nombreuses fonctionnalités qui sont des blocs de construction clés pour les scénarios d’application importantes. Certaines de ces fonctionnalités sont exposées à la Universal Windows Platform (UWP), Win32 (API Windows) et d’autres plateformes d’applications de différentes façons. Les articles suivants vous aider à comprendre comment certaines fonctionnalités de Windows sont prises en charge dans les plateformes d’applications différents et comment commencer à utiliser les fonctionnalités dans votre code.

Cet article fournit une liste personnalisée d’articles pour en savoir plus sur la façon dont vous pouvez accéder à des fonctionnalités importantes de Windows et les technologies dans la plateforme Windows universelle, Win32 (API Windows), WPF et les plateformes d’applications Windows Forms. Pour obtenir des informations complètes sur les fonctionnalités de développement de chaque plateforme, consultez les ressources suivantes :

* [Documentation UWP](/windows/uwp/index)
* [Documentation de Win32 (API Windows)](/windows/desktop/index)
* [Documentation de WPF](https://docs.microsoft.com/dotnet/framework/wpf/index)
* [Documentation Windows Forms](https://docs.microsoft.com/dotnet/framework/winforms/index)

## <a name="key-windows-features-and-technologies"></a>Technologies et fonctionnalités de Windows de la clé

Les sections suivantes illustrent plusieurs fonctionnalités de Windows importantes et les technologies qui vous permettent de livrer modernes et créer des expériences incroyables à vos clients.

### <a name="windows-ink"></a>Windows Ink

![Stylet Surface](images/hero-small.png)  

La plateforme Windows Ink, associée à un stylet, permet de créer des notes manuscrites, des dessins et des annotations plus naturellement. La plateforme prend en charge la capture d’entrée du numériseur sous forme de données d’entrée manuscrite, la génération et la gestion de données d’entrée manuscrite, la restitution de ces données sous forme de traits et la conversion de l’encre en texte via la reconnaissance d’écriture manuscrite.

Pour plus d’informations sur les différentes façons d’utiliser Windows Ink dans les applications Windows, consultez [Windows Ink](/windows/uwp/design/input/pen-and-stylus-interactions).

### <a name="speech-interactions"></a>Interactions vocales

![Écran initial de la reconnaissance vocale correspondant à une contrainte basée sur un fichier de grammaire SGRS](images/speech-listening-initial.png)

![Écran final de la reconnaissance vocale correspondant à une contrainte basée sur un fichier de grammaire SGRS](images/speech-listening-complete.png)

Windows offre plusieurs manières d’intégrer la reconnaissance vocale et synthèse vocale (également appelée TTS, ou la synthèse vocale) directement dans l’expérience utilisateur de votre application. Reconnaissance vocale peut être un moyen fiable et plus agréable pour les personnes d’interagir avec votre application, en complément ou le même remplacement, clavier, souris, tactile et mouvements.

Pour plus d’informations sur les différentes façons d’utiliser des interactions de reconnaissance vocale dans les applications Windows, consultez [interactions de reconnaissance vocale](/windows/uwp/design/input/speech-interactions).

### <a name="windows-ai"></a>Windows IA

![Windows IA](images/windows-ai.png)

Nous proposons plusieurs solutions d’intelligence artificielle différents que vous pouvez utiliser pour améliorer vos applications Windows. Avec Windows Machine Learning, vous pouvez intégrer formé modèles d’apprentissage dans vos applications et de les exécuter localement sur l’appareil. Compétences de Vision de Windows vous permet d’utiliser des bibliothèques prédéfinies pour accomplir les tâches de traitement d’images courantes ou créer vos propres solutions personnalisées. DirectML fournit de bas niveau, les API DirectX-style qui vous permettent de tirer pleinement parti du matériel.

Pour plus d’informations sur les différentes façons d’intégrer l’intelligence artificielle dans les applications Windows, consultez [Windows AI](https://docs.microsoft.com/windows/ai/).

## <a name="features-and-technologies-by-platform"></a>Fonctionnalités et technologies de plate-forme

Les sections suivantes fournit des liens utiles pour en savoir plus sur l’intégration avec les principales fonctionnalités de Windows et les technologies de nos plateformes d’application principale : UWP, Win32 (Windows API), WPF et Windows Forms.

### <a name="user-interface-and-accessibility"></a>Accessibilité et l’interface utilisateur

|  UWP  |  Win32 (Windows API) |  WPF  |  Windows Forms  |
|-------|----------------------|-------|-----------------|
| [Concevez](/windows/uwp/design/basics/)<br/><br/>[Disposition](/windows/uwp/design/layout/)<br/><br/>[Contrôles](/windows/uwp/design/controls-and-patterns/)<br/><br/>[entrée](/windows/uwp/design/input/)<br/><br/>[Vignettes](/windows/uwp/design/shell/tiles-and-notifications/creating-tiles)<br/><br/>[Couche visuelle](/windows/uwp/composition/visual-layer)<br/><br/>[Plateforme XAML](/windows/uwp/xaml-platform/)<br/><br/>[Lancement, reprise et tâches en arrière-plan](/windows/uwp/launch-resume/)<br/><br/>[Accessibilité de Windows](/windows/uwp/design/accessibility/accessibility)<br/><br/>  |  [Interface utilisateur de bureau](/windows/desktop/windows-application-ui-development)<br/><br/>[Interpréteur de commandes et l’environnement de bureau](/windows/desktop/user-interface)<br/><br/>[contrôles Windows](/windows/desktop/controls/window-controls)<br/><br/>[Contrôles UWP dans les applications de bureau (îles XAML)](/windows/apps/desktop/modernize/xaml-islands)<br/><br/>[Couche UWP Visual dans les applications de bureau](/windows/apps/desktop/modernize/visual-layer-in-desktop-apps)<br/><br/>[Windows et des messages](/windows/desktop/winmsg/windowing)<br/><br/>[Menus et autres ressources](/windows/desktop/menurc/resources)<br/><br/>[Haute résolution](/windows/desktop/hidpi/high-dpi-desktop-application-development-on-windows)<br/><br/>[Accessibilité](/windows/desktop/accessibility)<br/><br/>  |  [Windows dans WPF](https://docs.microsoft.com/dotnet/framework/wpf/app-development/windows-in-wpf-applications)<br/><br/>[Vue d’ensemble de la navigation](https://docs.microsoft.com/dotnet/framework/wpf/app-development/navigation-overview)<br/><br/>[XAML dans WPF](https://docs.microsoft.com/dotnet/framework/wpf/advanced/xaml-in-wpf)<br/><br/>[Contrôles](https://docs.microsoft.com/dotnet/framework/wpf/controls/)<br/><br/>[Programmation de la couche visuelle](https://docs.microsoft.com/dotnet/framework/wpf/graphics-multimedia/visual-layer-programming)<br/><br/>[entrée](https://docs.microsoft.com/dotnet/framework/wpf/advanced/input-wpf)<br/><br/>[Accessibilité](https://docs.microsoft.com/dotnet/framework/ui-automation/)<br/><br/>  | [Créer un formulaire Windows](https://docs.microsoft.com/dotnet/framework/winforms/creating-a-new-windows-form)<br/><br/>[Contrôles](https://docs.microsoft.com/dotnet/framework/winforms/controls/)<br/><br/>[Boîtes de dialogue](https://docs.microsoft.com/dotnet/framework/winforms/dialog-boxes-in-windows-forms)<br/><br/>[Entrée d’utilisateur](https://docs.microsoft.com/dotnet/framework/winforms/user-input-in-windows-forms)<br/><br/>[Accessibilité des Windows Forms](https://docs.microsoft.com/dotnet/framework/winforms/advanced/windows-forms-accessibility)<br/><br/> |

### <a name="audio-video-and-graphics"></a>Audio, vidéo et graphiques

|  UWP  |  Win32 (Windows API) |  WPF  |  Windows Forms  |
|-------|----------------------|-------|-----------------|
| [Audio, vidéo et appareil photo](/windows/uwp/audio-video-camera/)<br/><br/>[Lecture de contenu multimédia](/windows/uwp/audio-video-camera/media-playback/)<br/><br/>[Couche visuelle](/windows/uwp/composition/visual-layer)<br/><br/>[Plateforme XAML](/windows/uwp/xaml-platform/) |  [Audio et vidéo](/windows/desktop/audio-and-video)<br/><br/>[Graphiques et jeux](/windows/desktop/graphics-and-multimedia)<br/><br/>[DirectX](/windows/desktop/getting-started-with-directx-graphics)<br/><br/>[Direct2D](/windows/desktop/direct2d/direct2d-portal)<br/><br/>[Direct3D](/windows/desktop/direct3d)<br/><br/>[Windows GDI](/windows/desktop/gdi/windows-gdi)<br/><br/>[GDI+](/windows/desktop/gdiplus/-gdiplus-gdi-start)  |  [Graphiques](https://docs.microsoft.com/dotnet/framework/wpf/graphics-multimedia/graphics)<br/><br/>[Mutimedia](https://docs.microsoft.com/dotnet/framework/wpf/graphics-multimedia/multimedia-overview)  |  [Graphiques et dessins](https://docs.microsoft.com/dotnet/framework/winforms/advanced/graphics-and-drawing-in-windows-forms?view=netframework-4.8)<br/><br/>[Classe SoundPlayer](https://docs.microsoft.com/dotnet/framework/winforms/controls/soundplayer-class-overview)  |

### <a name="data-access-and-app-resources"></a>Ressources de données access et application

|  UWP  |  Win32 (Windows API) |  WPF  |  Windows Forms  |
|-------|----------------------|-------|-----------------|
| [Accès aux données](/windows/uwp/data-access/)<br/><br/>[Liaison de données](/windows/uwp/data-binding/)<br/><br/>[Fichiers, dossiers et bibliothèques](/windows/uwp/files/)<br/><br/>[Ressources d’application](/windows/uwp/app-resources/) |  [Accès aux données et stockage](/windows/desktop/data-access-and-storage)<br/><br/>[Systèmes de fichiers locaux](/windows/desktop/fileio/file-systems)<br/><br/>[Vues d’ensemble de ressources](/windows/desktop/menurc/resources-overviews)</li>  |  [Données et modélisation](https://docs.microsoft.com/dotnet/framework/data/)<br/><br/>[Liaison de données](https://docs.microsoft.com/dotnet/framework/wpf/data/data-binding-wpf)<br/><br/>[Ressources dans les applications .NET](https://docs.microsoft.com/dotnet/framework/resources/?view=netframework-4.8)<br/><br/>[Fichiers de ressources, de contenu et de données d’application](https://docs.microsoft.com/dotnet/framework/wpf/app-development/wpf-application-resource-content-and-data-files)  |  [Données et modélisation](https://docs.microsoft.com/dotnet/framework/data/)<br/><br/>[Liaison de données](https://docs.microsoft.com/dotnet/framework/winforms/windows-forms-data-binding)<br/><br/>[Ressources dans les applications .NET](https://docs.microsoft.com/dotnet/framework/resources/?view=netframework-4.8)<br/><br/>[Paramètres d’application](https://docs.microsoft.com/dotnet/framework/winforms/advanced/application-settings-for-windows-forms)  |

### <a name="devices-documents-and-printing"></a>L’impression, des documents et des appareils

|  UWP  |  Win32 (Windows API) |  WPF  |  Windows Forms  |
|-------|----------------------|-------|-----------------|
| [Activer les fonctionnalités d’un appareil](/windows/uwp/devices-sensors/enable-device-capabilities)<br/><br/>[Énumérer les appareils](/windows/uwp/devices-sensors/enumerate-devices)<br/><br/>[Détecteurs](/windows/uwp/devices-sensors/sensors)<br/><br/>[Bluetooth](/windows/uwp/devices-sensors/bluetooth)<br/><br/>[Impression et numérisation](/windows/uwp/devices-sensors/printing-and-scanning)<br/><br/>[NFC](/windows/uwp/devices-sensors/nfc) | [API de capteur](/windows/desktop/sensorsapi/portal)<br/><br/>[Impression](/desktop/printdocs/printdocs-printing)<br/><br/>[API de l’UPnP](/desktop/upnp/universal-plug-and-play-start-page) |  [Gestion du système d’impression et impression](https://docs.microsoft.com/dotnet/framework/wpf/advanced/printing-and-print-system-management)  |  [prise en charge d’impression](https://docs.microsoft.com/dotnet/framework/winforms/advanced/windows-forms-print-support)  |

### <a name="system-network-and-power"></a>Système, réseau et puissance

|  UWP  |  Win32 (Windows API) |  WPF  |  Windows Forms  |
|-------|----------------------|-------|-----------------|
| [Énumérer les appareils](/windows/uwp/devices-sensors/enumerate-devices)<br/><br/>[Obtenir des informations sur la batterie](/windows/uwp/devices-sensors/get-battery-info)<br/><br/>[Threads et programmation asynchrone](/windows/uwp/threading-async/)<br/><br/>[Mise en réseau et services web](/windows/uwp/networking/) | [Services système](/windows/desktop/system-services)<br/><br/>[Gestion de la mémoire](/windows/desktop/memory/memory-management)<br/><br/>[Gestion de l’alimentation](/windows/desktop/power/power-management-portal)<br/><br/>[Processus et threads](/windows/desktop/procthread/processes-and-threads)<br/><br/>[Mise en réseau et Internet](/windows/desktop/networking)<br/><br/>[Informations de système de Windows](/windows/desktop/sysinfo/windows-system-information) |  [Modèle de thread](https://docs.microsoft.com/dotnet/framework/wpf/advanced/threading-model)<br/><br/>[Programmation réseau dans le .NET Framework](https://docs.microsoft.com/dotnet/framework/network-programming/)  |  [Informations système](https://docs.microsoft.com/dotnet/framework/winforms/advanced/system-information-and-windows-forms)<br/><br/>[Gestion de l’alimentation](https://docs.microsoft.com/dotnet/framework/winforms/advanced/power-management-in-windows-forms)<br/><br/>[Programmation réseau dans le .NET Framework](https://docs.microsoft.com/dotnet/framework/network-programming/)<br/><br/>[Mise en réseau dans Windows Forms](https://docs.microsoft.com/dotnet/framework/winforms/advanced/networking-in-windows-forms-applications)  |

### <a name="packaging-and-deployment"></a>Empaquetage et déploiement

|  UWP  |  Win32 (Windows API) |  WPF  |  Windows Forms  |
|-------|----------------------|-------|-----------------|
| [Empaquetage d’applications](/windows/uwp/packaging/)<br/><br/>[MSIX](https://docs.microsoft.com/windows/msix/)<br/><br/>[Schéma de manifeste de package App](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/schema-root) | [Empaqueter des applications de bureau Windows (MSIX)](/windows/msix/desktop/desktop-to-uwp-root)<br/><br/>[Installation de l’application et de maintenance](/windows/desktop/application-installing-and-servicing)<br/><br/>[Windows Installer](/windows/desktop/msi/windows-installer-portal) |  [Empaqueter des applications de bureau Windows (MSIX)](/windows/msix/desktop/desktop-to-uwp-root)<br/><br/>[Déploiement d’applications et du .NET Framework](https://docs.microsoft.com/dotnet/framework/deployment/)<br/><br/>[Déploiement d’une application WPF](https://docs.microsoft.com/dotnet/framework/wpf/app-development/deploying-a-wpf-application-wpf)  |  [Empaqueter des applications de bureau Windows (MSIX)](/windows/msix/desktop/desktop-to-uwp-root)<br/><br/>[Déploiement d’applications et du .NET Framework](https://docs.microsoft.com/dotnet/framework/deployment/)<br/><br/>[Déploiement ClickOnce pour les Windows Forms](https://docs.microsoft.com/dotnet/framework/winforms/clickonce-deployment-for-windows-forms)  |
