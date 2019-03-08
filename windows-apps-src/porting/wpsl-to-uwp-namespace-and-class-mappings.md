---
description: Cette rubrique fournit un mappage complet de Windows Phone Silverlight APIs en leurs équivalents de plateforme universelle Windows (UWP).
title: Windows Phone Silverlight aux mappages d’espace de noms et classe UWP
ms.assetid: 33f06706-4790-48f3-a2e4-ebef9ddb61a4
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 9acd42f57117fb01eef4ba8f87d35664be21cf32
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57634974"
---
# <a name="windowsphone-silverlight-to-uwp-api-mappings"></a>Windows Phone Silverlight aux mappages d’API UWP


Cette rubrique fournit un mappage complet de Windows Phone Silverlight APIs en leurs équivalents de plateforme universelle Windows (UWP). Il ne s’agit généralement pas d’un mappage un-à-un des fonctionnalités : une plateforme peut comporter plus ou moins de fonctionnalités que l’autre dans un espace de noms ou dans une classe.

La table de mappage vous aidera à lorsque vous travaillez dans un projet UWP et vous êtes nouveau à l’aide de code source à partir d’un projet Windows Phone Silverlight. Il existe des différences dans les noms des espaces de noms et des classes (y compris dans les contrôles d’interface utilisateur) entre les deux plateformes. Dans de nombreux cas, pour effectuer un mappage, il suffit de modifier un nom d’espace de noms. Votre code est ensuite compilé. Parfois, un nom de classe ou d’API a changé, ainsi que le nom de l’espace de noms. D’autres fois, le mappage demande un peu plus de travail, et dans de rares cas, il nécessite un changement d’approche.

**L’utilisation de la table :  ** Tout d’abord, recherchez le nom de la classe que vous utilisez. Les classes sont indiquées lorsque le mappage est plus complexe qu’un simple changement de nom de l’espace de noms. Si votre classe n’est pas répertoriée, le mappage correspond simplement à une modification de l’espace de noms. Recherchez le nom de l’espace de noms de votre classe pour trouver son équivalent UWP. Votre classe figurera dans cet espace de noms. Si votre espace de noms n’est pas indiqué, son nom n’a pas changé.

**Remarque**  Windows 10 prend en charge beaucoup plus du .NET Framework qu’une application Windows Phone Store. Par exemple, Windows 10 a plusieurs System.ServiceModel. \* espaces de noms System.Net, System.Net.NetworkInformation et System.Net.Sockets.
En outre, dans une application Windows 10, vous bénéficierez de .NET Native, qui une technologie de compilation ahead of time qui convertit le MSIL dans un code machine exécutable en mode natif. Les applications .NET Native démarrent plus vite, utilisent moins de mémoire et consomment moins de batterie que leurs équivalents MSIL.

| Windows Phone Silverlight | Windows Runtime |
| ------------------------- | --------------- |
| Publicité | |
| Classe **Microsoft.Advertising.Mobile.UI.AdControl** | Classe [AdControl](https://msdn.microsoft.com/library/advertising-windows-sdk-api-reference-adcontrol.aspx) |
| Alarmes, rappels et agents d’arrière-plan | |
| Classe **Microsoft.Phone.BackgroundAgent** | [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768) class |
| Espace de noms **Microsoft.Phone.Scheduler** | [**Windows.ApplicationModel.Background**](https://msdn.microsoft.com/library/windows/apps/br224847) namespace |
| Classe **Microsoft.Phone.Scheduler.Alarm** | [**BackgroundTaskBuilder** ](https://msdn.microsoft.com/library/windows/apps/br224768) et [ **ToastNotificationManager** ](https://msdn.microsoft.com/library/windows/apps/br208642) classes |
| Classes **Microsoft.Phone.Scheduler.PeriodicTask**, **ScheduledAction**, **ScheduledActionService**, **ScheduledTask** et **ScheduledTaskAgent** | [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768) class |
| Classe **Microsoft.Phone.Scheduler.Reminder** | [**BackgroundTaskBuilder** ](https://msdn.microsoft.com/library/windows/apps/br224768) et [ **ToastNotificationManager** ](https://msdn.microsoft.com/library/windows/apps/br208642) classes |
| Classe **Microsoft.Phone.PictureDecoder** | [**BitmapDecoder** ](https://msdn.microsoft.com/library/windows/apps/br226176) classe |
| Espace de noms **Microsoft.Phone.BackgroundAudio** | [**Windows.Media.Playback** ](https://msdn.microsoft.com/library/windows/apps/dn640562) espace de noms |
| Espace de noms **Microsoft.Phone.BackgroundTransfer** | [**Windows.Networking.BackgroundTransfer**](https://msdn.microsoft.com/library/windows/apps/br207242) namespace |
| Environnement et modèle d’application | |
| Classe **System.AppDomain** | Aucun équivalent direct. Voir les classes [**Application**](https://msdn.microsoft.com/library/windows/apps/br242324) et [**CoreApplication**](https://msdn.microsoft.com/library/windows/apps/br225016). |
| Classe **System.Environment** | Aucun équivalent direct |
| Classe **System.ComponentModel.Annotations**  | Aucun équivalent direct |
| Classe **System.ComponentModel.BackgroundWorker** | [**ThreadPool**](https://msdn.microsoft.com/library/windows/apps/br229621) class |
| Classe **System.ComponentModel.DesignerProperties** | [**DesignMode** ](https://msdn.microsoft.com/library/windows/apps/br224664) classe |
| Classes **System.Threading.Thread** et **System.Threading.ThreadPool** | [**ThreadPool**](https://msdn.microsoft.com/library/windows/apps/br229621) class |
| (ST = **System.Threading**) <br/> Méthode **ST.Thread.MemoryBarrier** | (ST = **System.Threading**) <br/> Méthode **ST.Interlocked.MemoryBarrier** |
| (ST = **System.Threading**) <br/> Propriété **ST.Thread.ManagedThreadId** | (S = **System**) <br/> Propriété **S.Environment.ManagedThreadId** |
| Classe **System.Threading.Timer** | [**ThreadPoolTimer**](https://msdn.microsoft.com/library/windows/apps/br230587) class |
| (SWT = **System.Windows.Threading**) <br/> Classe **SWT.Dispatcher** | [**CoreDispatcher** ](https://msdn.microsoft.com/library/windows/apps/br208211) classe |
| (SWT = **System.Windows.Threading**) <br/> Classe **SWT.DispatcherTimer** | [**DispatcherTimer** ](https://msdn.microsoft.com/library/windows/apps/br244250) classe |
| Blend pour Visual Studio | |
| (MEDC = **Microsoft.Expression.Drawing.Core**) <br/> Classe **MEDC.GeometryHelper** | Aucun équivalent direct |
| Espace de noms **Microsoft.Expression.Interactivity** | Espace de noms [Microsoft.Xaml.Interactivity](https://go.microsoft.com/fwlink/p/?LinkId=328776) |
| Espace de noms **Microsoft.Expression.Interactivity.Core** | Espace de noms [Microsoft.Xaml.Interactions.Core](https://go.microsoft.com/fwlink/p/?LinkId=328773) |
| (MEIC = **Microsoft.Expression.Interactivity.Core**) <br/> Classe **MEIC.ExtendedVisualStateManager** | Aucun équivalent direct |
| Espace de noms **Microsoft.Expression.Interactivity.Input** | Aucun équivalent direct |
| Espace de noms **Microsoft.Expression.Interactivity.Media** | Espace de noms [Microsoft.Xaml.Interactions.Media](https://go.microsoft.com/fwlink/p/?LinkId=328775) |
| Espace de noms **Microsoft.Expression.Shapes** | Aucun équivalent direct |
| (MI = **Microsoft.Internal**) <br/> Interface **MI.IManagedFrameworkInternalHelper** | Aucun équivalent direct |
| Données de contacts et calendrier | |
| Espace de noms **Microsoft.Phone.UserData** | [**Windows.ApplicationModel.Contacts**](https://msdn.microsoft.com/library/windows/apps/br225002), [**Windows.ApplicationModel.Appointments**](https://msdn.microsoft.com/library/windows/apps/dn263359) namespaces |
| (MPU = **Microsoft.Phone.UserData**) <br/> Classes **MPU.Account**, **ContactAddress**, **ContactCompanyInformation**, **ContactEmailAddress** et **ContactPhoneNumber** | [**Contact** ](https://msdn.microsoft.com/library/windows/apps/br224849) classe |
| (MPU = **Microsoft.Phone.UserData**) <br/> Classe **MPU.Appointments** | [**AppointmentCalendar** ](https://msdn.microsoft.com/library/windows/apps/dn596134) classe |
| (MPU = **Microsoft.Phone.UserData**) <br/> Classe **MPU.Contacts** | [**ContactStore**](https://msdn.microsoft.com/library/windows/apps/dn624859) class |
| Contrôles et infrastructure d’interface utilisateur | |
| Classe **ControlTiltEffect.TiltEffect** | Des animations de la bibliothèque d’animations Windows Runtime sont intégrées aux styles par défaut des contrôles courants. Voir [Animation](wpsl-to-uwp-porting-xaml-and-ui.md). |
| Espace de noms **Microsoft.Phone.Controls** | [**Windows.UI.Xaml.Controls**](https://msdn.microsoft.com/library/windows/apps/br227716) namespace |
| (MPC = **Microsoft.Phone.Controls**) <br/> Classe **MPC.ContextMenu** | [**Menu contextuel** ](https://msdn.microsoft.com/library/windows/apps/br208693) classe |
| (MPC = **Microsoft.Phone.Controls**) <br/>Classe **MPC.DatePickerPage** | [**DatePickerFlyout** ](https://msdn.microsoft.com/library/windows/apps/dn625013) classe |
| (MPC = **Microsoft.Phone.Controls**) <br/>Classe **MPC.GestureListener** | [**GestureRecognizer** ](https://msdn.microsoft.com/library/windows/apps/br241937) classe |
| (MPC = **Microsoft.Phone.Controls**) <br/>Classe **MPC.LongListSelector** | [**SemanticZoom** ](https://msdn.microsoft.com/library/windows/apps/hh702601) classe |
| (MPC = **Microsoft.Phone.Controls**) <br/>Classe **MPC.ObscuredEventArgs** | [**SystemProtection**](https://msdn.microsoft.com/library/windows/apps/jj585394), [**WindowActivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br208377) classes |
| (MPC = **Microsoft.Phone.Controls**) <br/>Classe **MPC.Panorama** | [**Hub** ](https://msdn.microsoft.com/library/windows/apps/dn251843) classe |
| (MPC = **Microsoft.Phone.Controls**) <br/>Classes **MPC.PhoneApplicationFrame**,<br/>(SWN = **System.Windows.Navigation**) <br/>**SWN.NavigationService** | [**Frame** ](https://msdn.microsoft.com/library/windows/apps/br242682) classe |
| (MPC = **Microsoft.Phone.Controls**) <br/>Classe **MPC.PhoneApplicationPage** | [**Page** ](https://msdn.microsoft.com/library/windows/apps/br227503) classe |
| (MPC = **Microsoft.Phone.Controls**) <br/>Classe **MPC.TiltEffect** | [**PointerDownThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/hh969164) class |
| (MPC = **Microsoft.Phone.Controls**) <br/>Classe **MPC.TimePickerPage** | [**TimePickerFlyout** ](https://msdn.microsoft.com/library/windows/apps/dn608313) classe |
| (MPC = **Microsoft.Phone.Controls**) <br/>Classe **MPC.WebBrowser** | [**WebView** ](https://msdn.microsoft.com/library/windows/apps/br227702) classe |
| (MPC = **Microsoft.Phone.Controls**) <br/>Classe **MPC.WebBrowserExtensions** | Aucun équivalent direct |
| (MPC = **Microsoft.Phone.Controls**) <br/>Classe **MPC.WrapPanel** | Aucun équivalent direct à des fins de disposition générale. [**ItemsWrapGrid** ](https://msdn.microsoft.com/library/windows/apps/dn298849) et [ **WrapGrid** ](https://msdn.microsoft.com/library/windows/apps/br227717) peut être utilisé dans le modèle de panneau de configuration des éléments d’un contrôle d’éléments. |
| (MPD = **Microsoft.Phone.Data**) <br/>Espace de noms **MPD.Linq** | Aucun équivalent direct |
| (MPD = **Microsoft.Phone.Data**) <br/>Espace de noms **MPD. Linq.Mapping** | Aucun équivalent direct |
| Espace de noms **Microsoft.Phone.Globalization** | Aucun équivalent direct |
| (MPI = **Microsoft.Phone.Info**) <br/>Classes **MPI.DeviceExtendedProperties** et **DeviceStatus** | [**EasClientDeviceInformation**](https://msdn.microsoft.com/library/windows/apps/hh701390), [ **MemoryManager** ](https://msdn.microsoft.com/library/windows/apps/dn633831) classes. Pour plus d’informations, voir [État de l’appareil](wpsl-to-uwp-input-and-sensors.md). |
| (MPI = **Microsoft.Phone.Info**) <br/>Classe **MPI.MediaCapabilities** | Aucun équivalent direct |
| (MPI = **Microsoft.Phone.Info**) <br/>Classe **MPI.UserExtendedProperties** | [**AdvertisingManager** ](https://msdn.microsoft.com/library/windows/apps/dn363391) classe |
| Espace de noms **System.Windows** | [**Windows.UI.Xaml**](https://msdn.microsoft.com/library/windows/apps/br209045) namespace |
| Espace de noms **System.Windows.Automation** | [**Windows.UI.Xaml.Automation**](https://msdn.microsoft.com/library/windows/apps/br209179) namespace |
| Espaces de noms **System.Windows.Controls** et **System.Windows.Input** | [**Windows.UI.Core**](https://msdn.microsoft.com/library/windows/apps/br208383), [**Windows.UI.Input**](https://msdn.microsoft.com/library/windows/apps/br242084), [**Windows.UI.Xaml.Controls**](https://msdn.microsoft.com/library/windows/apps/br227716) namespaces |
| Classes **System.Windows.Controls.DrawingSurface** et **DrawingSurfaceBackgroundGrid** | [**SwapChainPanel**](https://msdn.microsoft.com/library/windows/apps/dn252834) class |
| Classe **System.Windows.Controls.RichTextBox** | [**RichEditBox**](https://msdn.microsoft.com/library/windows/apps/br227548) class |
| Classe **System.Windows.Controls.WrapPanel** | Aucun équivalent direct à des fins de disposition générale. [**ItemsWrapGrid** ](https://msdn.microsoft.com/library/windows/apps/dn298849) et [ **WrapGrid** ](https://msdn.microsoft.com/library/windows/apps/br227717) peut être utilisé dans le modèle de panneau de configuration des éléments d’un contrôle d’éléments. |
| Espace de noms **System.Windows.Controls.Primitives** | [**Windows.UI.Xaml.Controls.Primitives**](https://msdn.microsoft.com/library/windows/apps/br209818) namespace |
| Espace de noms **System.Windows.Controls.Shapes** | [**Windows.UI.Xaml.Controls.Shapes**](/uwp/api/Windows.UI.Xaml.Shapes) namespace |
| Espace de noms **System.Windows.Data** | [**Windows.UI.Xaml.Data**](https://msdn.microsoft.com/library/windows/apps/br209917) namespace |
| Espace de noms **System.Windows.Documents** | [**Windows.UI.Xaml.Documents**](https://msdn.microsoft.com/library/windows/apps/br209984) namespace |
| Espace de noms **System.Windows.Ink** | Aucun équivalent direct |
| Espace de noms **System.Windows.Markup** | [**Windows.UI.Xaml.Markup**](https://msdn.microsoft.com/library/windows/apps/br228046) namespace | 
| Espace de noms **System.Windows.Navigation** | [**Windows.UI.Xaml.Navigation**](https://msdn.microsoft.com/library/windows/apps/br243300) namespace |
| Événement **System.Windows.UIElement.Tap**, délégué **EventHandler&lt;GestureEventArgs&gt;** | [**Tapped**](https://msdn.microsoft.com/library/windows/apps/br208985) event, [**TappedEventHandler**](https://msdn.microsoft.com/library/windows/apps/br227993) delegate |
| Données et services |  |
| Classe **System.Data.Linq.DataContext** | Aucun équivalent direct |
| Classe **System.Data.Linq.Mapping.ColumnAttribute** | Aucun équivalent direct |
| Classe **System.Data.Linq.SqlClient.SqlHelpers** | Aucun équivalent direct |
| Appareils | |
| Espaces de noms **Microsoft.Devices** et **Microsoft.Devices.Sensors** | [**Windows.Devices.Enumeration**](https://msdn.microsoft.com/library/windows/apps/br225459), [**Windows.Devices.Enumeration.Pnp**](https://msdn.microsoft.com/library/windows/apps/br225517), [**Windows.Devices.Input**](https://msdn.microsoft.com/library/windows/apps/br225648), [**Windows.Devices.Sensors**](https://msdn.microsoft.com/library/windows/apps/br206408) namespaces |
| Classes **Microsoft.Devices.Camera** et **Microsoft.Devices.PhotoCamera** | [**MediaCapture** ](https://msdn.microsoft.com/library/windows/apps/br241124) classe. Également la classe [**CameraCaptureUI**](https://msdn.microsoft.com/library/windows/apps/br241030) (Windows uniquement). |
| Classe **Microsoft.Devices.CameraButtons** | [**HardwareButtons** ](https://msdn.microsoft.com/library/windows/apps/jj207557) classe |
| Classe **Microsoft.Devices.CameraVideoBrushExtensions** | [**CaptureElement** ](https://msdn.microsoft.com/library/windows/apps/br209278) classe |
| Classe **Microsoft.Devices.Environment** | Aucun équivalent direct. Pour contourner ce problème, utilisez la compilation conditionnelle et définissez un symbole personnalisé. Vous pouvez peut-être concevoir une solution de contournement à l’aide de la propriété [IsAttached](https://msdn.microsoft.com/library/e299w87h.aspx). |
| Classe **Microsoft.Devices.MediaHistory** | Aucun équivalent direct |
| Classe **Microsoft.Devices.VibrateController** | [**VibrationDevice** ](https://msdn.microsoft.com/library/windows/apps/jj207230) classe |
| Classe **Microsoft.Devices.Radio.FMRadio** | Aucun équivalent direct |
| Classes **Microsoft.Devices.Sensors.Accelerometer** et **Compass** | Dans l’espace de noms [**Windows.Devices.Sensors**](https://msdn.microsoft.com/library/windows/apps/br206408) |
| Classe **Microsoft.Devices.Sensors.Gyroscope** | [**Gyromètre** ](https://msdn.microsoft.com/library/windows/apps/br225718) classe |
| Classe **Microsoft.Devices.Sensors.Motion** | [**Inclinomètre** ](https://msdn.microsoft.com/library/windows/apps/br225766) classe |
| Globalisation | |
| Espace de noms **System.Globalization** | [**Windows.Globalization** ](https://msdn.microsoft.com/library/windows/apps/br206813) espace de noms |
| (ST = **System.Threading**) <br/> Propriété **ST.Thread.CurrentCulture** | (SG = **System.Globalization**) <br/> Propriété **S.CultureInfo.CurrentCulture** |
| (ST = **System.Threading**) <br/> Propriété **ST.Thread.CurrentUICulture** | (SG = **System.Globalization**) <br/> Propriété **S.CultureInfo.CurrentUICulture** |
| Graphismes et animation | |
| **Microsoft.Xna.Framework. \***  espaces de noms, [bibliothèque de classes XNA Framework](https://go.microsoft.com/fwlink/p/?LinkId=263769), [bibliothèque de classes de Pipeline de contenu](https://go.microsoft.com/fwlink/p/?LinkId=263770) | Aucun équivalent direct. En règle générale, utilisez [Microsoft DirectX](https://msdn.microsoft.com/library/windows/desktop/ee663274) avec C++. Voir [Développement de jeux](https://msdn.microsoft.com/library/windows/apps/hh452744) et [Interopérabilité de DirectX et XAML](https://msdn.microsoft.com/library/windows/apps/hh825871). |
| Classe **Microsoft.Xna.Framework.Audio.Microphone** | [**MediaCapture** ](https://msdn.microsoft.com/library/windows/apps/br241124) classe |
| Classe **Microsoft.Xna.Framework.Audio.SoundEffect** | [**MediaElement** ](https://msdn.microsoft.com/library/windows/apps/br242926) classe |
| Espace de noms **Microsoft.Xna.Framework.GamerServices** | (WPS = **Windows.Phone.System**) <br/> [**WPS.UserProfile.GameServices.Core**](https://msdn.microsoft.com/library/windows/apps/jj207609) namespace |
| Classe **Microsoft.Xna.Framework.GamerServices.Guide** | Aucun équivalent direct |
| Classe **Microsoft.Xna.Framework.Input.GamePad** | [**HardwareButtons** ](https://msdn.microsoft.com/library/windows/apps/jj207557) classe |
| Classe **Microsoft.Xna.Framework.Input.Touch.TouchPanel** | [**GestureRecognizer** ](https://msdn.microsoft.com/library/windows/apps/br241937) classe |
| (MXFM = **Microsoft.Xna.Framework.Media**) <br/> Classes **MXFM.MediaLibrary** et **MXFM.PhoneExtensions.MediaLibraryExtensions** | [**KnownFolders** ](https://msdn.microsoft.com/library/windows/apps/br227151) classe |
| Classe **Microsoft.Xna.Framework.Media.MediaQueue** | [**SystemMediaTransportControls** ](https://msdn.microsoft.com/library/windows/apps/dn278677) classe |
| Classe **Microsoft.Xna.Framework.Media.Playlist** | [**BackgroundMediaPlayer** ](https://msdn.microsoft.com/library/windows/apps/dn652527) classe |
| Espace de noms **System.Windows.Media** | [**Windows.UI.Xaml.Media**](/uwp/api/Windows.UI.Xaml.Media) namespace |
| Classe **System.Windows.Media.RadialGradientBrush** | Aucun équivalent direct. Voir [Média et graphismes](wpsl-to-uwp-porting-xaml-and-ui.md). |
| Espace de noms **System.Windows.Media.Animation** | [**Windows.UI.Xaml.Media.Animation**](https://msdn.microsoft.com/library/windows/apps/br243232) namespace |
| Espace de noms **System.Windows.Media.Effects** | Aucun équivalent direct |
| Espace de noms **System.Windows.Media.Imaging** | [**Windows.UI.Xaml.Media.Imaging**](https://msdn.microsoft.com/library/windows/apps/br243258) namespace |
| Espace de noms **System.Windows.Media.Media3D** | [**Windows.UI.Xaml.Media.Media3D**](https://msdn.microsoft.com/library/windows/apps/br243274) namespace |
| Espace de noms **System.Windows.Shapes** | [**Windows.UI.Xaml.Shapes**](/uwp/api/Windows.UI.Xaml.Shapes) namespace |
| Lanceurs et sélecteurs | |
| Classes **Microsoft.Phone.Tasks.AddressChooserTask**, **EmailAddressChooserTask** et **PhoneNumberChooserTask** | [**ContactPicker** ](https://msdn.microsoft.com/library/windows/apps/br224913) classe |
| Classes **Microsoft.Phone.Tasks.AddWalletItemTask** et **AddWalletItemResult** | [**Windows.ApplicationModel.Wallet**](https://msdn.microsoft.com/library/windows/apps/dn631399) namespace |
| Classes **Microsoft.Phone.Tasks.BingMapsDirectionsTask** et **BingMapsTask** | Aucun équivalent direct |
| Classe **Microsoft.Phone.Tasks.CameraCaptureTask** | [**MediaCapture** ](https://msdn.microsoft.com/library/windows/apps/br241124) classe. Également la classe [**CameraCaptureUI**](https://msdn.microsoft.com/library/windows/apps/br241030) (Windows uniquement). |
| **Microsoft.Phone.Tasks.MarketplaceDetailTask** | [**CurrentApp** ](https://msdn.microsoft.com/library/windows/apps/hh779765) classe ([**RequestAppPurchaseAsync** ](https://msdn.microsoft.com/library/windows/apps/hh967813) méthode) |
| Classes **Microsoft.Phone.Tasks.ConnectionSettingsTask**, **MarketplaceHubTask**, **MarketplaceReviewTask**, **MarketplaceSearchTask**, **MediaPlayerLauncher**, **SearchTask**, **SmsComposeTask** et **WebBrowserTask** | [**Lanceur de** ](https://msdn.microsoft.com/library/windows/apps/br241801) classe |
| Classe **Microsoft.Phone.Tasks.EmailComposeTask** | [**EmailMessage** ](https://msdn.microsoft.com/library/windows/apps/dn631270) classe |
| Classe **Microsoft.Phone.Tasks.GameInviteTask** | Aucun équivalent direct |
| Classes **Microsoft.Phone.Tasks.MapDownloaderTask**, **MapsDirectionsTask**, **MapsTask** et **MapUpdaterTask** | Aucun équivalent direct |
| Classe **Microsoft.Phone.Tasks.PhoneCallTask** | [**PhoneCallManager** ](https://msdn.microsoft.com/library/windows/apps/dn624832) classe |
| Classe **Microsoft.Phone.Tasks.PhotoChooserTask** | [**FileOpenPicker** ](https://msdn.microsoft.com/library/windows/apps/br207847) classe |
| Classe **Microsoft.Phone.Tasks.SaveAppointmentTask** | [**AppointmentManager**](https://msdn.microsoft.com/library/windows/apps/dn297254) class |
| Classes **Microsoft.Phone.Tasks.SaveContactTask**, **SaveEmailAddressTask** et **SavePhoneNumberTask** | [**StoredContact** ](https://msdn.microsoft.com/library/windows/apps/jj207727) classe (Windows Phone uniquement) | 
| Classe **Microsoft.Phone.Tasks.SaveRingtoneTask** | Aucun équivalent direct |
| Classes **Microsoft.Phone.Tasks.ShareLinkTask**, **ShareMediaTask** et **ShareStatusTask** | [**DataPackage** ](https://msdn.microsoft.com/library/windows/apps/br205873) classe |
| Emplacement | |
| Espace de noms **System.Device.Location** | [**Windows.Devices.Geolocation** ](https://msdn.microsoft.com/library/windows/apps/br225603) espace de noms |
| Classe **System.Device.GeoCoordinateWatcher** | [**Geolocator** ](https://msdn.microsoft.com/library/windows/apps/br225534) classe |
| Cartes | |
| Espaces de noms **Microsoft.Phone.Maps** | [**Windows.Services.Maps**](https://msdn.microsoft.com/library/windows/apps/dn636979) namespace |
| Espace de noms **Microsoft.Phone.Maps.Controls** | [**Windows.UI.Xaml.Controls.Maps**](https://msdn.microsoft.com/library/windows/apps/dn610751) namespace |
| Classe **Microsoft.Phone.Maps.Controls.Map** | [**MapControl** ](https://msdn.microsoft.com/library/windows/apps/dn637004) classe |
| Espace de noms **Microsoft.Phone.Maps.Services** | [**Windows.Services.Maps**](https://msdn.microsoft.com/library/windows/apps/dn636979) namespace |
| Classes **Microsoft.Phone.Maps.Services.GeocodeQuery** et **ReverseGeocodeQuery** | [**MapLocationFinder**](https://msdn.microsoft.com/library/windows/apps/dn627550) class |
| Classe **System.Device.Location.GeoCoordinate** | [**Geopoint** ](https://msdn.microsoft.com/library/windows/apps/dn263675) classe |
| Classe **Microsoft.Phone.Maps.Services.Route** | [**MapRoute** ](https://msdn.microsoft.com/library/windows/apps/dn636937) classe |
| Classe **Microsoft.Phone.Maps.Services.RouteQuery** | [**MapRouteFinder** ](https://msdn.microsoft.com/library/windows/apps/dn636938) classe |
| Monétisation | |
| Espace de noms **Microsoft.Phone.Marketplace** | [**Windows.ApplicationModel.Store**](https://msdn.microsoft.com/library/windows/apps/br225197) namespace |
| Support | |
| Espace de noms **Microsoft.Phone.Media** | [**MediaElement** ](https://msdn.microsoft.com/library/windows/apps/br242926) classe |
| Mise en réseau | |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> Classe **MPNN.DeviceNetworkInformation** | [**Nom d’hôte**](https://msdn.microsoft.com/library/windows/apps/br207113), [ **NetworkInformation** ](https://msdn.microsoft.com/library/windows/apps/br207293) classes |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> Classe **MPNN.NetworkInterface** | [**NetworkInformation** ](https://msdn.microsoft.com/library/windows/apps/br207293) classe |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> Classe **MPNN.NetworkInterfaceInfo** | [**ConnectionProfile** ](https://msdn.microsoft.com/library/windows/apps/br207249) classe |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> Classe **MPNN.NetworkInterfaceList** | [**NetworkInformation** ](https://msdn.microsoft.com/library/windows/apps/br207293) classe |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> Classe **MPNN.SocketExtensions** | Aucun équivalent direct |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> Classe **MPNN.WebRequestExtensions** | Aucun équivalent direct |
| Espace de noms **Microsoft.Phone.Networking.Voip** | Aucun équivalent direct |
| Classe **System.Net.CookieCollection** | Toujours pris en charge, mais certaines propriétés sont manquantes (par exemple, IsReadOnly) |
| Classe **System.Net.DownloadProgressChangedEventArgs** et classes similaires associées à **System.Net.WebClient** | [**HttpClient** ](https://msdn.microsoft.com/library/windows/apps/dn298639) classe (ou [System.Net.Http.HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.118).aspx)). Dérive de [System.Net.Http.StreamContent](https://msdn.microsoft.com/library/system.net.http.streamcontent.aspx) pour mesurer la progression. |
| Classes **System.Net.DnsEndPoint** et **IPAddress** | Ces classes sont toujours prises en charge, mais certaines propriétés sont manquantes. Vous pouvez également effectuer le portage vers la classe [**HostName**](https://msdn.microsoft.com/library/windows/apps/br207113). |
| Classe **System.Net.HttpUtility** | [**HtmlFormatHelper** ](https://msdn.microsoft.com/library/windows/apps/hh738437) classe |
| Classe **System.Net.HttpWebRequest** | Prise en charge partielle, mais l’alternative prospective recommandée correspond à la classe [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) (ou [System.Net.Http.HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.118).aspx)). Ces API utilisent [System.Net.Http.HttpRequestMessage](https://msdn.microsoft.com/library/system.net.http.httprequestmessage.aspx) pour représenter une requête HTTP. |
| Classe **System.Net.HttpWebResponse** | Toujours pris en charge, mais utilisez Dispose() au lieu de Close(). Mais l’alternative prospective recommandée correspond à la classe [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) (ou [System.Net.Http.HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.118).aspx)). Ces API utilisent [System.Net.Http.HttpResponseMessage](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.aspx) pour représenter une réponse HTTP. |
| (SNN = **System.Net.NetworkInformation**) <br/> Classe **SNN.NetworkChange** | Toujours pris en charge, à l’exception du constructeur. |
| Classe **System.Net.OpenReadCompletedEventArgs** et classes similaires associées à **System.Net.WebClient** | [**HttpClient** ](https://msdn.microsoft.com/library/windows/apps/dn298639) classe (ou [System.Net.Http.HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient.aspx)) |
| Classe **System.Net.Sockets.Socket** | Toujours pris en charge, mais utilisez Dispose() au lieu de Close(). Vous pouvez également effectuer le portage vers la classe [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882). |
| Classe **System.Net.Sockets.SocketException** | Toujours pris en charge, mais utilisez la propriété SocketErrorCode au lieu d’ErrorCode. |
| Classes **System.Net.Sockets.UdpAnySourceMulticastClient** et **UdpSingleSourceMulticastClient** | [**DatagramSocket** ](https://msdn.microsoft.com/library/windows/apps/br241319) classe |
| Classe **System.Net.UploadProgressChangedEventArgs** et classes similaires associées à **System.Net.WebClient** | [**HttpClient** ](https://msdn.microsoft.com/library/windows/apps/dn298639) classe (ou [System.Net.Http.HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient.aspx)) |
| Classe **System.Net.WebClient** | [**HttpClient** ](https://msdn.microsoft.com/library/windows/apps/dn298639) classe (ou [System.Net.Http.HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient.aspx)) |
| Classe **System.Net.WebRequest** | Prise en charge partielle (ensemble de propriétés différent), mais l’alternative prospective recommandée correspond à la classe [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) (ou [System.Net.Http.HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.118).aspx)). Ces API utilisent [System.Net.Http.HttpRequestMessage](https://msdn.microsoft.com/library/system.net.http.httprequestmessage.aspx) pour représenter une requête HTTP. |
| Classe **System.Net.WebResponse** | Toujours pris en charge, mais utilisez Dispose() au lieu de Close(). Mais l’alternative prospective recommandée correspond à la classe [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) (ou [System.Net.Http.HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.118).aspx)). Ces API utilisent [System.Net.Http.HttpResponseMessage](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.aspx) pour représenter une réponse HTTP. |
| (SN = **System.Net**) <br/> Classe **SN.WriteStreamClosedEventArgs** | [**HttpClient** ](https://msdn.microsoft.com/library/windows/apps/dn298639) classe (ou [System.Net.Http.HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient.aspx)) |
| (SN = **System.Net**) <br/> Classe **SN.WriteStreamClosedEventHandler** | [**HttpClient** ](https://msdn.microsoft.com/library/windows/apps/dn298639) classe (ou [System.Net.Http.HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient.aspx)) |
| Classe **System.UriFormatException** | Classe **System.FormatException** |
| Notifications | |
| Espace de noms MPN = **Microsoft.Phone.Notification** | [**Windows.UI.Notifications**](https://msdn.microsoft.com/library/windows/apps/br208661), [**Windows.Networking.PushNotifications**](https://msdn.microsoft.com/library/windows/apps/br241307) namespaces |
| MPN = **Microsoft.Phone.Notification** <br/> Classe **MPN.HttpNotification** | [**TileNotification** ](https://msdn.microsoft.com/library/windows/apps/br208616) classe |
| MPN = **Microsoft.Phone.Notification** <br/> Classe **MPN.HttpNotificationChannel** | [**PushNotificationChannel** ](https://msdn.microsoft.com/library/windows/apps/br241283) classe |
| Programmation | |
| Espace de noms **System** | [**Windows.Foundation**](https://msdn.microsoft.com/library/windows/apps/br226021) namespace |
| Classes **System.Diagnostics.StackFrame** et **StackTrace** | Aucun équivalent direct |
| Espace de noms **System.Diagnostics** | [**Windows.Foundation.Diagnostics**](https://msdn.microsoft.com/library/windows/apps/br206677) namespace |
| Interface **System.ICloneable** | Méthode personnalisée renvoyant le type approprié. |
| Classe **System.Reflection.Emit.ILGenerator** | Aucun équivalent direct |
| Extensions réactives | |
| Espace de noms **Microsoft.Phone.Reactive** | Aucun équivalent direct |
| Réflexion | |
| Classe **System.Type** | Classe **System.Reflection.TypeInfo**. Voir [Réflexion dans .NET Framework pour les applications UWP](https://msdn.microsoft.com/library/hh535795.aspx). |
| Ressources | |
| Classe **System.Resources.ResourceManager** | (WA = **Windows.ApplicationModel**)<br/>[**ÉTAT DE WASHINGTON. Resources.Core** ](https://msdn.microsoft.com/library/windows/apps/br225039) et [ **WA. Ressources** ](https://msdn.microsoft.com/library/windows/apps/br206022) espaces de noms, [ **ResourceManager** ](https://msdn.microsoft.com/library/windows/apps/br206078) classe. Voir [Création et récupération de ressources dans les applications Windows Runtime](https://msdn.microsoft.com/library/windows/apps/xaml/hh694557.aspx). |
| Élément sécurisé | |
| (MPS = **Microsoft.Phone.SecureElement**) <br/> Classes **MPS.SecureElementChannel** et **MPS.SecureElementSession** | [**SmartCardConnection** ](https://msdn.microsoft.com/library/windows/apps/dn608002) classe |
| (MPS = **Microsoft.Phone.SecureElement**) <br/> Classe **MPS.SecureElementReader** | [**SmartCardReader**](https://msdn.microsoft.com/library/windows/apps/dn263857) class |
| Sécurité | |
| (SSC = **System.Security.Cryptography**) <br/> Classes **SSC.Aes** et **SSC.RSA** | [**CryptographicEngine** ](https://msdn.microsoft.com/library/windows/apps/br241490) classe |
| (SSC = **System.Security.Cryptography**) <br/> Classes **SSC.HMACSHA256** et **SSC.SHA256** | [**HashAlgorithmProvider** ](https://msdn.microsoft.com/library/windows/apps/br241511) classe |
| (SSC = **System.Security.Cryptography**) <br/> Classe **SSC.ProtectedData** | [**DataProtectionProvider** ](https://msdn.microsoft.com/library/windows/apps/br241559) classe |
| (SSC = **System.Security.Cryptography**) <br/> Classe **SSC.RandomNumberGenerator** | [**CryptographicBuffer** ](https://msdn.microsoft.com/library/windows/apps/br227092) classe |
| (SSC = **System.Security.Cryptography**) <br/> Classe **SSC.X509Certificates.X509Certificate** | [**CertificateEnrollmentManager**](https://msdn.microsoft.com/library/windows/apps/br212075) class |
| Shell | |
| (MPSh = **Microsoft.Phone.Shell**) <br/> Classe **MPSh.ApplicationBar** | [**CommandBar** ](https://msdn.microsoft.com/library/windows/apps/dn279427) classe |
| (MPSh = **Microsoft.Phone.Shell**) <br/> Classe **MPSh.ApplicationBarIconButton** | [**AppBarButton** ](https://msdn.microsoft.com/library/windows/apps/dn279244) classe (lorsqu’il est utilisé à l’intérieur de la [ **PrimaryCommands** ](https://msdn.microsoft.com/library/windows/apps/dn279430) propriété) |
| (MPSh = **Microsoft.Phone.Shell**) <br/> Classe **MPSh.ApplicationBarMenuItem** | [**AppBarButton** ](https://msdn.microsoft.com/library/windows/apps/dn279244) classe (lorsqu’il est utilisé à l’intérieur de la [ **SecondaryCommands** ](https://msdn.microsoft.com/library/windows/apps/dn279434) propriété) |
| (MPSh = **Microsoft.Phone.Shell**) <br/> Classes **MPSh.CycleTileData**, **MPSh.FlipTileData**, **MPSh.IconicTileData**, **MPSh.ShellTileData** et **MPSh.StandardTileData** | [**TileTemplateType**](https://msdn.microsoft.com/library/windows/apps/br208621) class |
| (MPSh = **Microsoft.Phone.Shell**) <br/> Classe **MPSh.PhoneApplicationService** | [**CoreApplication**](https://msdn.microsoft.com/library/windows/apps/br225016), [**DisplayRequest**](https://msdn.microsoft.com/library/windows/apps/br241816) classes |
| (MPSh = **Microsoft.Phone.Shell**) <br/> Classe **MPSh.ProgressIndicator** | [**StatusBarProgressIndicator**](https://msdn.microsoft.com/library/windows/apps/dn633865) class |
| (MPSh = **Microsoft.Phone.Shell**) <br/> Classe **MPSh.ShellTile** | [**SecondaryTile** ](https://msdn.microsoft.com/library/windows/apps/br242183) classe |
| (MPSh = **Microsoft.Phone.Shell**) <br/> Classe **MPSh.ShellTileSchedule** | [**TileUpdater**](https://msdn.microsoft.com/library/windows/apps/br208628) class |
| (MPSh = **Microsoft.Phone.Shell**) <br/> Classe **MPSh.ShellToast** | [**ToastNotificationManager** ](https://msdn.microsoft.com/library/windows/apps/br208642) classe |
| (MPSh = **Microsoft.Phone.Shell**) <br/> Classe **MPSh.SystemTray** | [**StatusBar** ](https://msdn.microsoft.com/library/windows/apps/dn633864) classe |
| Stockage et E/S | |
| Classes **Microsoft.Phone.Storage.ExternalStorage**, **ExternalStorageDevice**, **ExternalStorageFile** et **ExternalStorageFolder** | [**KnownFolders** ](https://msdn.microsoft.com/library/windows/apps/br227151) classe |
| Espace de noms **System.IO** | [**Windows.Storage**](https://msdn.microsoft.com/library/windows/apps/br227346), [**Windows.Storage.Streams**](https://msdn.microsoft.com/library/windows/apps/br241791) namespaces |
| Classe **System.IO.Directory** | [**StorageFolder** ](https://msdn.microsoft.com/library/windows/apps/br227230) classe |
| Classe **System.IO.File** | [**StorageFile** ](https://msdn.microsoft.com/library/windows/apps/br227171) et [ **PathIO** ](https://msdn.microsoft.com/library/windows/apps/hh701663) classes
| (SII = **System.IO.IsolatedStorage**) <br/> Classe **SII.IsolatedStorageFile** |[**ApplicationData.LocalFolder** ](https://msdn.microsoft.com/library/windows/apps/br241621) propriété |
| (SII = **System.IO.IsolatedStorage**) <br/> Classe **SII.IsolatedStorageSettings** | [**ApplicationData.LocalSettings** ](https://msdn.microsoft.com/library/windows/apps/windows.storage.applicationdata.localsettings.aspx) propriété |
| Classe **System.IO.Stream** | Toujours pris en charge, mais utilisez ReadAsync() et WriteAsync() au lieu de BeginRead()/EndRead() et BeginWrite()/EndWrite(). |
| Portefeuille | |
| Espace de noms **Microsoft.Phone.Wallet** | [**Windows.ApplicationModel.Wallet**](https://msdn.microsoft.com/library/windows/apps/dn631399) namespace |
| Xml | |
| (SX = **System.Xml**) | Méthode **SX.XmlConvert.ToDateTime** |
| (SX = **System.Xml**) | Méthode **SX.XmlConvert.ToDateTimeOffset** |


Rubrique suivante : [Portage du projet](wpsl-to-uwp-porting-to-a-uwp-project.md).

