---
author: stevewhims
description: Cette rubrique fournit un mappage complet des WindowsPhone APIs Silverlight vers leurs équivalents de la plateforme Windows universelle (UWP).
title: WindowsPhone Silverlight aux mappages d’espace de noms et des classes UWP
ms.assetid: 33f06706-4790-48f3-a2e4-ebef9ddb61a4
ms.author: stwhi
ms.date: 02/08/2017
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 54118b41fc1f3036dddba9a0cfb8ecd860c1e233
ms.sourcegitcommit: cbe7cf620622a5e4df7414f9e38dfecec1cfca99
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/21/2018
ms.locfileid: "7442548"
---
# <a name="windowsphone-silverlight-to-uwp-api-mappings"></a>WindowsPhone Silverlight aux mappages d’API UWP


Cette rubrique fournit un mappage complet des WindowsPhone APIs Silverlight vers leurs équivalents de la plateforme Windows universelle (UWP). Il ne s’agit généralement pas d’un mappage un-à-un des fonctionnalités : une plateforme peut comporter plus ou moins de fonctionnalités que l’autre dans un espace de noms ou dans une classe.

La table de mappage vous aidera à lorsque vous travaillez dans un projet UWP et que vous réutilisez le code source à partir d’un projet WindowsPhone Silverlight. Il existe des différences dans les noms des espaces de noms et des classes (y compris dans les contrôles d’interface utilisateur) entre les deux plateformes. Dans de nombreux cas, pour effectuer un mappage, il suffit de modifier un nom d’espace de noms. Votre code est ensuite compilé. Parfois, un nom de classe ou d’API a changé, ainsi que le nom de l’espace de noms. D’autres fois, le mappage demande un peu plus de travail et, dans de rares cas, il nécessite un changement d’approche.

**L’utilisation de la table:** Tout d’abord, recherchez le nom de la classe que vous utilisez. Les classes sont indiquées lorsque le mappage est plus complexe qu’un simple changement de nom de l’espace de noms. Si votre classe n’est pas répertoriée, le mappage correspond simplement à une modification de l’espace de noms. Recherchez le nom de l’espace de noms de votre classe pour trouver son équivalent UWP. Votre classe figurera dans cet espace de noms. Si votre espace de noms n’est pas indiqué, son nom n’a pas changé.

**Remarque**Windows 10 prend en charge beaucoup plus de .NET Framework qu’une application Windows Phone Store. Par exemple, Windows 10 a plusieurs espaces de noms System.ServiceModel.\*, ainsi que System.Net, System.Net.NetworkInformation et System.Net.Sockets.
En outre, dans une application Windows 10, vous pourrez bénéficier du code natif .NET, dont une technologie de compilation d’avant-garde qui convertit le MSIL en code machine exécutable en mode natif. Les applications .NET natives démarrent plus vite, utilisent moins de mémoire et consomment moins de batterie que leurs équivalents MSIL.

| WindowsPhone Silverlight | Windows Runtime |
| ------------------------- | --------------- |
| Publicité | |
| Classe **Microsoft.Advertising.Mobile.UI.AdControl** | Classe [AdControl](http://msdn.microsoft.com/library/advertising-windows-sdk-api-reference-adcontrol.aspx) |
| Alarmes, rappels et agents d’arrière-plan | |
| Classe **Microsoft.Phone.BackgroundAgent** | Classe [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768) |
| Espace de noms **Microsoft.Phone.Scheduler** | Espace de noms [**Windows.ApplicationModel.Background**](https://msdn.microsoft.com/library/windows/apps/br224847) |
| Classe **Microsoft.Phone.Scheduler.Alarm** | Classes [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768) et [**ToastNotificationManager**](https://msdn.microsoft.com/library/windows/apps/br208642) |
| Classes **Microsoft.Phone.Scheduler.PeriodicTask**, **ScheduledAction**, **ScheduledActionService**, **ScheduledTask** et **ScheduledTaskAgent** | Classe [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768) |
| Classe **Microsoft.Phone.Scheduler.Reminder** | Classes [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768) et [**ToastNotificationManager**](https://msdn.microsoft.com/library/windows/apps/br208642) |
| Classe **Microsoft.Phone.PictureDecoder** | Classe [**BitmapDecoder**](https://msdn.microsoft.com/library/windows/apps/br226176) |
| Espace de noms **Microsoft.Phone.BackgroundAudio** | Espace de noms [**Windows.Media.Playback**](https://msdn.microsoft.com/library/windows/apps/dn640562) |
| Espace de noms **Microsoft.Phone.BackgroundTransfer** | Espace de noms [**Windows.Networking.BackgroundTransfer**](https://msdn.microsoft.com/library/windows/apps/br207242) |
| Environnement et modèle d’application | |
| Classe **System.AppDomain** | Aucun équivalent direct. Voir les classes [**Application**](https://msdn.microsoft.com/library/windows/apps/br242324) et [**CoreApplication**](https://msdn.microsoft.com/library/windows/apps/br225016). |
| Classe **System.Environment** | Aucun équivalent direct |
| Classe **System.ComponentModel.Annotations**  | Aucun équivalent direct |
| Classe **System.ComponentModel.BackgroundWorker** | Classe [**ThreadPool**](https://msdn.microsoft.com/library/windows/apps/br229621) |
| Classe **System.ComponentModel.DesignerProperties** | Classe [**DesignMode**](https://msdn.microsoft.com/library/windows/apps/br224664) |
| Classes **System.Threading.Thread** et **System.Threading.ThreadPool** | Classe [**ThreadPool**](https://msdn.microsoft.com/library/windows/apps/br229621) |
| (ST = **System.Threading**) <br/> Méthode **ST.Thread.MemoryBarrier** | (ST = **System.Threading**) <br/> Méthode **ST.Interlocked.MemoryBarrier** |
| (ST = **System.Threading**) <br/> Propriété **ST.Thread.ManagedThreadId** | (S = **System**) <br/> Propriété **S.Environment.ManagedThreadId** |
| Classe **System.Threading.Timer** | Classe [**ThreadPoolTimer**](https://msdn.microsoft.com/library/windows/apps/br230587) |
| (SWT = **System.Windows.Threading**) <br/> Classe **SWT.Dispatcher** | Classe [**CoreDispatcher**](https://msdn.microsoft.com/library/windows/apps/br208211) |
| (SWT = **System.Windows.Threading**) <br/> Classe **SWT.DispatcherTimer** | Classe [**DispatcherTimer**](https://msdn.microsoft.com/library/windows/apps/br244250) |
| Blend pour Visual Studio | |
| (MEDC = **Microsoft.Expression.Drawing.Core**) <br/> Classe **MEDC.GeometryHelper** | Aucun équivalent direct |
| Espace de noms **Microsoft.Expression.Interactivity** | Espace de noms [Microsoft.Xaml.Interactivity](http://go.microsoft.com/fwlink/p/?LinkId=328776) |
| Espace de noms **Microsoft.Expression.Interactivity.Core** | Espace de noms [Microsoft.Xaml.Interactions.Core](http://go.microsoft.com/fwlink/p/?LinkId=328773) |
| (MEIC = **Microsoft.Expression.Interactivity.Core**) <br/> Classe **MEIC.ExtendedVisualStateManager** | Aucun équivalent direct |
| Espace de noms **Microsoft.Expression.Interactivity.Input** | Aucun équivalent direct |
| Espace de noms **Microsoft.Expression.Interactivity.Media** | Espace de noms [Microsoft.Xaml.Interactions.Media](http://go.microsoft.com/fwlink/p/?LinkId=328775) |
| Espace de noms **Microsoft.Expression.Shapes** | Aucun équivalent direct |
| (MI = **Microsoft.Internal**) <br/> Interface **MI.IManagedFrameworkInternalHelper** | Aucun équivalent direct |
| Données de contacts et calendrier | |
| Espace de noms **Microsoft.Phone.UserData** | Espaces de noms [**Windows.ApplicationModel.Contacts**](https://msdn.microsoft.com/library/windows/apps/br225002) et [**Windows.ApplicationModel.Appointments**](https://msdn.microsoft.com/library/windows/apps/dn263359) |
| (MPU = **Microsoft.Phone.UserData**) <br/> Classes **MPU.Account**, **ContactAddress**, **ContactCompanyInformation**, **ContactEmailAddress** et **ContactPhoneNumber** | Classe [**Contact**](https://msdn.microsoft.com/library/windows/apps/br224849) |
| (MPU = **Microsoft.Phone.UserData**) <br/> Classe **MPU.Appointments** | Classe [**AppointmentCalendar**](https://msdn.microsoft.com/library/windows/apps/dn596134) |
| (MPU = **Microsoft.Phone.UserData**) <br/> Classe **MPU.Contacts** | Classe [**ContactStore**](https://msdn.microsoft.com/library/windows/apps/dn624859) |
| Contrôles et infrastructure d’interface utilisateur | |
| Classe **ControlTiltEffect.TiltEffect** | Des animations de la bibliothèque d’animations Windows Runtime sont intégrées aux styles par défaut des contrôles courants. Voir [Animation](wpsl-to-uwp-porting-xaml-and-ui.md). |
| Espace de noms **Microsoft.Phone.Controls** | Espace de noms [**Windows.UI.Xaml.Controls**](https://msdn.microsoft.com/library/windows/apps/br227716) |
| (MPC = **Microsoft.Phone.Controls**) <br/> Classe **MPC.ContextMenu** | Classe [**PopupMenu**](https://msdn.microsoft.com/library/windows/apps/br208693) |
| (MPC = **Microsoft.Phone.Controls**) <br/>Classe **MPC.DatePickerPage** | Classe [**DatePickerFlyout**](https://msdn.microsoft.com/library/windows/apps/dn625013) |
| (MPC = **Microsoft.Phone.Controls**) <br/>Classe **MPC.GestureListener** | Classe [**GestureRecognizer**](https://msdn.microsoft.com/library/windows/apps/br241937) |
| (MPC = **Microsoft.Phone.Controls**) <br/>Classe **MPC.LongListSelector** | Classe [**SemanticZoom**](https://msdn.microsoft.com/library/windows/apps/hh702601) |
| (MPC = **Microsoft.Phone.Controls**) <br/>Classe **MPC.ObscuredEventArgs** | Classes [**SystemProtection**](https://msdn.microsoft.com/library/windows/apps/jj585394) et [**WindowActivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br208377) |
| (MPC = **Microsoft.Phone.Controls**) <br/>Classe **MPC.Panorama** | Classe [**Hub**](https://msdn.microsoft.com/library/windows/apps/dn251843) |
| (MPC = **Microsoft.Phone.Controls**) <br/>Classes **MPC.PhoneApplicationFrame**,<br/>(SWN = **System.Windows.Navigation**) <br/>**SWN.NavigationService** | Classe [**Frame**](https://msdn.microsoft.com/library/windows/apps/br242682) |
| (MPC = **Microsoft.Phone.Controls**) <br/>Classe **MPC.PhoneApplicationPage** | Classe [**Page**](https://msdn.microsoft.com/library/windows/apps/br227503) |
| (MPC = **Microsoft.Phone.Controls**) <br/>Classe **MPC.TiltEffect** | Classe [**PointerDownThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/hh969164) |
| (MPC = **Microsoft.Phone.Controls**) <br/>Classe **MPC.TimePickerPage** | Classe [**TimePickerFlyout**](https://msdn.microsoft.com/library/windows/apps/dn608313) |
| (MPC = **Microsoft.Phone.Controls**) <br/>Classe **MPC.WebBrowser** | Classe [**WebView**](https://msdn.microsoft.com/library/windows/apps/br227702) |
| (MPC = **Microsoft.Phone.Controls**) <br/>Classe **MPC.WebBrowserExtensions** | Aucun équivalent direct |
| (MPC = **Microsoft.Phone.Controls**) <br/>Classe **MPC.WrapPanel** | Aucun équivalent direct à des fins de disposition générale. Les classes [**ItemsWrapGrid**](https://msdn.microsoft.com/library/windows/apps/dn298849) et [**WrapGrid**](https://msdn.microsoft.com/library/windows/apps/br227717) peuvent être utilisées dans le modèle de panneau d’éléments d’un contrôle d’éléments. |
| (MPD = **Microsoft.Phone.Data**) <br/>Espace de noms **MPD.Linq** | Aucun équivalent direct |
| (MPD = **Microsoft.Phone.Data**) <br/>Espace de noms **MPD. Linq.Mapping** | Aucun équivalent direct |
| Espace de noms **Microsoft.Phone.Globalization** | Aucun équivalent direct |
| (MPI = **Microsoft.Phone.Info**) <br/>Classes **MPI.DeviceExtendedProperties** et **DeviceStatus** | Classes [**EasClientDeviceInformation**](https://msdn.microsoft.com/library/windows/apps/hh701390) et [**MemoryManager**](https://msdn.microsoft.com/library/windows/apps/dn633831). Pour plus d’informations, voir [État de l’appareil](wpsl-to-uwp-input-and-sensors.md). |
| (MPI = **Microsoft.Phone.Info**) <br/>Classe **MPI.MediaCapabilities** | Aucun équivalent direct |
| (MPI = **Microsoft.Phone.Info**) <br/>Classe **MPI.UserExtendedProperties** | Classe [**AdvertisingManager**](https://msdn.microsoft.com/library/windows/apps/dn363391) |
| Espace de noms **System.Windows** | Espace de noms [**Windows.UI.Xaml**](https://msdn.microsoft.com/library/windows/apps/br209045) |
| Espace de noms **System.Windows.Automation** | Espace de noms [**Windows.UI.Xaml.Automation**](https://msdn.microsoft.com/library/windows/apps/br209179) |
| Espaces de noms **System.Windows.Controls** et **System.Windows.Input** | Espaces de noms [**Windows.UI.Core**](https://msdn.microsoft.com/library/windows/apps/br208383), [**Windows.UI.Input**](https://msdn.microsoft.com/library/windows/apps/br242084) et [**Windows.UI.Xaml.Controls**](https://msdn.microsoft.com/library/windows/apps/br227716) |
| Classes **System.Windows.Controls.DrawingSurface** et **DrawingSurfaceBackgroundGrid** | Classe [**SwapChainPanel**](https://msdn.microsoft.com/library/windows/apps/dn252834) |
| Classe **System.Windows.Controls.RichTextBox** | Classe [**RichEditBox**](https://msdn.microsoft.com/library/windows/apps/br227548) |
| Classe **System.Windows.Controls.WrapPanel** | Aucun équivalent direct à des fins de disposition générale. Les classes [**ItemsWrapGrid**](https://msdn.microsoft.com/library/windows/apps/dn298849) et [**WrapGrid**](https://msdn.microsoft.com/library/windows/apps/br227717) peuvent être utilisées dans le modèle de panneau d’éléments d’un contrôle d’éléments. |
| Espace de noms **System.Windows.Controls.Primitives** | Espace de noms [**Windows.UI.Xaml.Controls.Primitives**](https://msdn.microsoft.com/library/windows/apps/br209818) |
| Espace de noms **System.Windows.Controls.Shapes** | Espace de noms [**Windows.UI.Xaml.Controls.Shapes**](/uwp/api/Windows.UI.Xaml.Shapes) |
| Espace de noms **System.Windows.Data** | Espace de noms [**Windows.UI.Xaml.Data**](https://msdn.microsoft.com/library/windows/apps/br209917) |
| Espace de noms **System.Windows.Documents** | Espace de noms [**Windows.UI.Xaml.Documents**](https://msdn.microsoft.com/library/windows/apps/br209984) |
| Espace de noms **System.Windows.Ink** | Aucun équivalent direct |
| Espace de noms **System.Windows.Markup** | Espace de noms [**Windows.UI.Xaml.Markup**](https://msdn.microsoft.com/library/windows/apps/br228046) | 
| Espace de noms **System.Windows.Navigation** | Espace de noms [**Windows.UI.Xaml.Navigation**](https://msdn.microsoft.com/library/windows/apps/br243300) |
| Événement **System.Windows.UIElement.Tap**, délégué **EventHandler&lt;GestureEventArgs&gt;** | Événement [**Tapped**](https://msdn.microsoft.com/library/windows/apps/br208985), délégué [**TappedEventHandler**](https://msdn.microsoft.com/library/windows/apps/br227993) |
| Données et services |  |
| Classe **System.Data.Linq.DataContext** | Aucun équivalent direct |
| Classe **System.Data.Linq.Mapping.ColumnAttribute** | Aucun équivalent direct |
| Classe **System.Data.Linq.SqlClient.SqlHelpers** | Aucun équivalent direct |
| Périphériques | |
| Espaces de noms **Microsoft.Devices** et **Microsoft.Devices.Sensors** | Espaces de noms [**Windows.Devices.Enumeration**](https://msdn.microsoft.com/library/windows/apps/br225459), [**Windows.Devices.Enumeration.Pnp**](https://msdn.microsoft.com/library/windows/apps/br225517), [**Windows.Devices.Input**](https://msdn.microsoft.com/library/windows/apps/br225648) et [**Windows.Devices.Sensors**](https://msdn.microsoft.com/library/windows/apps/br206408) |
| Classes **Microsoft.Devices.Camera** et **Microsoft.Devices.PhotoCamera** | Classe [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/br241124). Également la classe [**CameraCaptureUI**](https://msdn.microsoft.com/library/windows/apps/br241030) (Windows uniquement). |
| Classe **Microsoft.Devices.CameraButtons** | Classe [**HardwareButtons**](https://msdn.microsoft.com/library/windows/apps/jj207557) |
| Classe **Microsoft.Devices.CameraVideoBrushExtensions** | Classe [**CaptureElement**](https://msdn.microsoft.com/library/windows/apps/br209278) |
| Classe **Microsoft.Devices.Environment** | Aucun équivalent direct. Pour contourner ce problème, utilisez la compilation conditionnelle et définissez un symbole personnalisé. Vous pouvez peut-être concevoir une solution de contournement à l’aide de la propriété [IsAttached](http://msdn.microsoft.com/library/e299w87h.aspx). |
| Classe **Microsoft.Devices.MediaHistory** | Aucun équivalent direct |
| Classe **Microsoft.Devices.VibrateController** | Classe [**VibrationDevice**](https://msdn.microsoft.com/library/windows/apps/jj207230) |
| Classe **Microsoft.Devices.Radio.FMRadio** | Aucun équivalent direct |
| Classes **Microsoft.Devices.Sensors.Accelerometer** et **Compass** | Dans l’espace de noms [**Windows.Devices.Sensors**](https://msdn.microsoft.com/library/windows/apps/br206408) |
| Classe **Microsoft.Devices.Sensors.Gyroscope** | Classe [**Gyrometer**](https://msdn.microsoft.com/library/windows/apps/br225718) |
| Classe **Microsoft.Devices.Sensors.Motion** | Classe [**Inclinometer**](https://msdn.microsoft.com/library/windows/apps/br225766) |
| Globalisation | |
| Espace de noms **System.Globalization** | Espace de noms [**Windows.Globalization**](https://msdn.microsoft.com/library/windows/apps/br206813) |
| (ST = **System.Threading**) <br/> Propriété **ST.Thread.CurrentCulture** | (SG = **System.Globalization**) <br/> Propriété **S.CultureInfo.CurrentCulture** |
| (ST = **System.Threading**) <br/> Propriété **ST.Thread.CurrentUICulture** | (SG = **System.Globalization**) <br/> Propriété **S.CultureInfo.CurrentUICulture** |
| Graphismes et animation | |
| Espaces de noms**Microsoft.Xna.Framework.\***, [Bibliothèque de classes de XNA Framework](http://go.microsoft.com/fwlink/p/?LinkId=263769), [Bibliothèque de classes du contenu à venir](http://go.microsoft.com/fwlink/p/?LinkId=263770) | Aucun équivalent direct. En règle générale, utilisez [Microsoft DirectX](https://msdn.microsoft.com/library/windows/desktop/ee663274) avec C++. Voir [Développement de jeux](https://msdn.microsoft.com/library/windows/apps/hh452744) et [Interopérabilité de DirectX et XAML](https://msdn.microsoft.com/library/windows/apps/hh825871). |
| Classe **Microsoft.Xna.Framework.Audio.Microphone** | Classe [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/br241124) |
| Classe **Microsoft.Xna.Framework.Audio.SoundEffect** | Classe [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926) |
| Espace de noms **Microsoft.Xna.Framework.GamerServices** | (WPS = **Windows.Phone.System**) <br/> Espace de noms [**WPS.UserProfile.GameServices.Core**](https://msdn.microsoft.com/library/windows/apps/jj207609) |
| Classe **Microsoft.Xna.Framework.GamerServices.Guide** | Aucun équivalent direct |
| Classe **Microsoft.Xna.Framework.Input.GamePad** | Classe [**HardwareButtons**](https://msdn.microsoft.com/library/windows/apps/jj207557) |
| Classe **Microsoft.Xna.Framework.Input.Touch.TouchPanel** | Classe [**GestureRecognizer**](https://msdn.microsoft.com/library/windows/apps/br241937) |
| (MXFM = **Microsoft.Xna.Framework.Media**) <br/> Classes **MXFM.MediaLibrary** et **MXFM.PhoneExtensions.MediaLibraryExtensions** | Classe [**KnownFolders**](https://msdn.microsoft.com/library/windows/apps/br227151) |
| Classe **Microsoft.Xna.Framework.Media.MediaQueue** | Classe [**SystemMediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/dn278677) |
| Classe **Microsoft.Xna.Framework.Media.Playlist** | Classe [**BackgroundMediaPlayer**](https://msdn.microsoft.com/library/windows/apps/dn652527) |
| Espace de noms **System.Windows.Media** | Espace de noms [**Windows.UI.Xaml.Media**](/uwp/api/Windows.UI.Xaml.Media) |
| Classe **System.Windows.Media.RadialGradientBrush** | Aucun équivalent direct. Voir [Média et graphismes](wpsl-to-uwp-porting-xaml-and-ui.md). |
| Espace de noms **System.Windows.Media.Animation** | Espace de noms [**Windows.UI.Xaml.Media.Animation**](https://msdn.microsoft.com/library/windows/apps/br243232) |
| Espace de noms **System.Windows.Media.Effects** | Aucun équivalent direct |
| Espace de noms **System.Windows.Media.Imaging** | Espace de noms [**Windows.UI.Xaml.Media.Imaging**](https://msdn.microsoft.com/library/windows/apps/br243258) |
| Espace de noms **System.Windows.Media.Media3D** | Espace de noms [**Windows.UI.Xaml.Media.Media3D**](https://msdn.microsoft.com/library/windows/apps/br243274) |
| Espace de noms **System.Windows.Shapes** | Espace de noms [**Windows.UI.Xaml.Shapes**](/uwp/api/Windows.UI.Xaml.Shapes) |
| Lanceurs et sélecteurs | |
| Classes **Microsoft.Phone.Tasks.AddressChooserTask**, **EmailAddressChooserTask** et **PhoneNumberChooserTask** | Classe [**ContactPicker**](https://msdn.microsoft.com/library/windows/apps/br224913) |
| Classes **Microsoft.Phone.Tasks.AddWalletItemTask** et **AddWalletItemResult** | Espace de noms [**Windows.ApplicationModel.Wallet**](https://msdn.microsoft.com/library/windows/apps/dn631399) |
| Classes **Microsoft.Phone.Tasks.BingMapsDirectionsTask** et **BingMapsTask** | Aucun équivalent direct |
| Classe **Microsoft.Phone.Tasks.CameraCaptureTask** | Classe [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/br241124). Également la classe [**CameraCaptureUI**](https://msdn.microsoft.com/library/windows/apps/br241030) (Windows uniquement). |
| **Microsoft.Phone.Tasks.MarketplaceDetailTask** | Classe [**CurrentApp**](https://msdn.microsoft.com/library/windows/apps/hh779765) (méthode [**RequestAppPurchaseAsync**](https://msdn.microsoft.com/library/windows/apps/hh967813)) |
| Classes **Microsoft.Phone.Tasks.ConnectionSettingsTask**, **MarketplaceHubTask**, **MarketplaceReviewTask**, **MarketplaceSearchTask**, **MediaPlayerLauncher**, **SearchTask**, **SmsComposeTask** et **WebBrowserTask** | Classe [**Launcher**](https://msdn.microsoft.com/library/windows/apps/br241801) |
| Classe **Microsoft.Phone.Tasks.EmailComposeTask** | Classe [**EmailMessage**](https://msdn.microsoft.com/library/windows/apps/dn631270) |
| Classe **Microsoft.Phone.Tasks.GameInviteTask** | Aucun équivalent direct |
| Classes **Microsoft.Phone.Tasks.MapDownloaderTask**, **MapsDirectionsTask**, **MapsTask** et **MapUpdaterTask** | Aucun équivalent direct |
| Classe **Microsoft.Phone.Tasks.PhoneCallTask** | Classe [**PhoneCallManager**](https://msdn.microsoft.com/library/windows/apps/dn624832) |
| Classe **Microsoft.Phone.Tasks.PhotoChooserTask** | Classe [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847) |
| Classe **Microsoft.Phone.Tasks.SaveAppointmentTask** | Classe [**AppointmentManager**](https://msdn.microsoft.com/library/windows/apps/dn297254) |
| Classes **Microsoft.Phone.Tasks.SaveContactTask**, **SaveEmailAddressTask** et **SavePhoneNumberTask** | Classe [**StoredContact**](https://msdn.microsoft.com/library/windows/apps/jj207727) (Windows Phone uniquement) | 
| Classe **Microsoft.Phone.Tasks.SaveRingtoneTask** | Aucun équivalent direct |
| Classes **Microsoft.Phone.Tasks.ShareLinkTask**, **ShareMediaTask** et **ShareStatusTask** | Classe [**DataPackage**](https://msdn.microsoft.com/library/windows/apps/br205873) |
| Emplacement | |
| Espace de noms **System.Device.Location** | Espace de noms [**Windows.Devices.Geolocation**](https://msdn.microsoft.com/library/windows/apps/br225603) |
| Classe **System.Device.GeoCoordinateWatcher** | Classe [**Geolocator**](https://msdn.microsoft.com/library/windows/apps/br225534) |
| Cartes | |
| Espaces de noms **Microsoft.Phone.Maps** | Espace de noms [**Windows.Services.Maps**](https://msdn.microsoft.com/library/windows/apps/dn636979) |
| Espace de noms **Microsoft.Phone.Maps.Controls** | Espace de noms [**Windows.UI.Xaml.Controls.Maps**](https://msdn.microsoft.com/library/windows/apps/dn610751) |
| Classe **Microsoft.Phone.Maps.Controls.Map** | Classe [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004) |
| Espace de noms **Microsoft.Phone.Maps.Services** | Espace de noms [**Windows.Services.Maps**](https://msdn.microsoft.com/library/windows/apps/dn636979) |
| Classes **Microsoft.Phone.Maps.Services.GeocodeQuery** et **ReverseGeocodeQuery** | Classe [**MapLocationFinder**](https://msdn.microsoft.com/library/windows/apps/dn627550) |
| Classe **System.Device.Location.GeoCoordinate** | Classe [**Geopoint**](https://msdn.microsoft.com/library/windows/apps/dn263675) |
| Classe **Microsoft.Phone.Maps.Services.Route** | Classe [**MapRoute**](https://msdn.microsoft.com/library/windows/apps/dn636937) |
| Classe **Microsoft.Phone.Maps.Services.RouteQuery** | Classe [**MapRouteFinder**](https://msdn.microsoft.com/library/windows/apps/dn636938) |
| Monétisation | |
| Espace de noms **Microsoft.Phone.Marketplace** | Espace de noms [**Windows.ApplicationModel.Store**](https://msdn.microsoft.com/library/windows/apps/br225197) |
| Support | |
| Espace de noms **Microsoft.Phone.Media** | Classe [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926) |
| Réseau | |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> Classe **MPNN.DeviceNetworkInformation** | Classes [**Hostname**](https://msdn.microsoft.com/library/windows/apps/br207113) et [**NetworkInformation**](https://msdn.microsoft.com/library/windows/apps/br207293) |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> Classe **MPNN.NetworkInterface** | Classe [**NetworkInformation**](https://msdn.microsoft.com/library/windows/apps/br207293) |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> Classe **MPNN.NetworkInterfaceInfo** | Classe [**ConnectionProfile**](https://msdn.microsoft.com/library/windows/apps/br207249) |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> Classe **MPNN.NetworkInterfaceList** | Classe [**NetworkInformation**](https://msdn.microsoft.com/library/windows/apps/br207293) |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> Classe **MPNN.SocketExtensions** | Aucun équivalent direct |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> Classe **MPNN.WebRequestExtensions** | Aucun équivalent direct |
| Espace de noms **Microsoft.Phone.Networking.Voip** | Aucun équivalent direct |
| Classe **System.Net.CookieCollection** | Toujours pris en charge, mais certaines propriétés sont manquantes (par exemple, IsReadOnly) |
| Classe **System.Net.DownloadProgressChangedEventArgs** et classes similaires associées à **System.Net.WebClient** | Classe [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) (ou [System.Net.Http.HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.118).aspx)). Dérive de [System.Net.Http.StreamContent](https://msdn.microsoft.com/library/system.net.http.streamcontent.aspx) pour mesurer la progression. |
| Classes **System.Net.DnsEndPoint** et **IPAddress** | Ces classes sont toujours prises en charge, mais certaines propriétés sont manquantes. Vous pouvez également effectuer le portage vers la classe [**HostName**](https://msdn.microsoft.com/library/windows/apps/br207113). |
| Classe **System.Net.HttpUtility** | Classe [**HtmlFormatHelper**](https://msdn.microsoft.com/library/windows/apps/hh738437) |
| Classe **System.Net.HttpWebRequest** | Prise en charge partielle, mais l’alternative prospective recommandée correspond à la classe [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) (ou [System.Net.Http.HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.118).aspx)). Ces API utilisent [System.Net.Http.HttpRequestMessage](https://msdn.microsoft.com/library/system.net.http.httprequestmessage.aspx) pour représenter une requête HTTP. |
| Classe **System.Net.HttpWebResponse** | Toujours pris en charge, mais utilisez Dispose() au lieu de Close(). Mais l’alternative prospective recommandée correspond à la classe [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) (ou [System.Net.Http.HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.118).aspx)). Ces API utilisent [System.Net.Http.HttpResponseMessage](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.aspx) pour représenter une réponse HTTP. |
| (SNN = **System.Net.NetworkInformation**) <br/> Classe **SNN.NetworkChange** | Toujours pris en charge, à l’exception du constructeur. |
| Classe **System.Net.OpenReadCompletedEventArgs** et classes similaires associées à **System.Net.WebClient** | Classe [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) (ou [System.Net.Http.HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient.aspx)) |
| Classe **System.Net.Sockets.Socket** | Toujours pris en charge, mais utilisez Dispose() au lieu de Close(). Vous pouvez également effectuer le portage vers la classe [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882). |
| Classe **System.Net.Sockets.SocketException** | Toujours pris en charge, mais utilisez la propriété SocketErrorCode au lieu d’ErrorCode. |
| Classes **System.Net.Sockets.UdpAnySourceMulticastClient** et **UdpSingleSourceMulticastClient** | Classe [**DatagramSocket**](https://msdn.microsoft.com/library/windows/apps/br241319) |
| Classe **System.Net.UploadProgressChangedEventArgs** et classes similaires associées à **System.Net.WebClient** | Classe [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) (ou [System.Net.Http.HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient.aspx)) |
| Classe **System.Net.WebClient** | Classe [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) (ou [System.Net.Http.HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient.aspx)) |
| Classe **System.Net.WebRequest** | Prise en charge partielle (ensemble de propriétés différent), mais l’alternative prospective recommandée correspond à la classe [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) (ou [System.Net.Http.HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.118).aspx)). Ces API utilisent [System.Net.Http.HttpRequestMessage](https://msdn.microsoft.com/library/system.net.http.httprequestmessage.aspx) pour représenter une requête HTTP. |
| Classe **System.Net.WebResponse** | Toujours pris en charge, mais utilisez Dispose() au lieu de Close(). Mais l’alternative prospective recommandée correspond à la classe [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) (ou [System.Net.Http.HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.118).aspx)). Ces API utilisent [System.Net.Http.HttpResponseMessage](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.aspx) pour représenter une réponse HTTP. |
| (SN = **System.Net**) <br/> Classe **SN.WriteStreamClosedEventArgs** | Classe [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) (ou [System.Net.Http.HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient.aspx)) |
| (SN = **System.Net**) <br/> Classe **SN.WriteStreamClosedEventHandler** | Classe [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) (ou [System.Net.Http.HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient.aspx)) |
| Classe **System.UriFormatException** | Classe **System.FormatException** |
| Notifications | |
| Espace de noms MPN = **Microsoft.Phone.Notification** | Espaces de noms [**Windows.UI.Notifications**](https://msdn.microsoft.com/library/windows/apps/br208661) et [**Windows.Networking.PushNotifications**](https://msdn.microsoft.com/library/windows/apps/br241307) |
| MPN = **Microsoft.Phone.Notification** <br/> Classe **MPN.HttpNotification** | Classe [**TileNotification**](https://msdn.microsoft.com/library/windows/apps/br208616) |
| MPN = **Microsoft.Phone.Notification** <br/> Classe **MPN.HttpNotificationChannel** | Classe [**PushNotificationChannel**](https://msdn.microsoft.com/library/windows/apps/br241283) |
| Programmation | |
| Espace de noms **System** | Espace de noms [**Windows.Foundation**](https://msdn.microsoft.com/library/windows/apps/br226021) |
| Classes **System.Diagnostics.StackFrame** et **StackTrace** | Aucun équivalent direct |
| Espace de noms **System.Diagnostics** | Espace de noms [**Windows.Foundation.Diagnostics**](https://msdn.microsoft.com/library/windows/apps/br206677) |
| Interface **System.ICloneable** | Méthode personnalisée renvoyant le type approprié. |
| Classe **System.Reflection.Emit.ILGenerator** | Aucun équivalent direct |
| Extensions réactives | |
| Espace de noms **Microsoft.Phone.Reactive** | Aucun équivalent direct |
| Réflexion | |
| Classe **System.Type** | Classe **System.Reflection.TypeInfo**. Voir [Réflexion dans .NET Framework pour les applications UWP](https://msdn.microsoft.com/library/hh535795.aspx). |
| Ressources | |
| Classe **System.Resources.ResourceManager** | (WA = **Windows.ApplicationModel**)<br/>Espaces de noms [**WA.Resources.Core**](https://msdn.microsoft.com/library/windows/apps/br225039) et [**WA.Resources**](https://msdn.microsoft.com/library/windows/apps/br206022), classe [**ResourceManager**](https://msdn.microsoft.com/library/windows/apps/br206078). Voir [Création et récupération de ressources dans les applications Windows Runtime](https://msdn.microsoft.com/library/windows/apps/xaml/hh694557.aspx). |
| Élément sécurisé | |
| (MPS = **Microsoft.Phone.SecureElement**) <br/> Classes **MPS.SecureElementChannel** et **MPS.SecureElementSession** | Classe [**SmartCardConnection**](https://msdn.microsoft.com/library/windows/apps/dn608002) |
| (MPS = **Microsoft.Phone.SecureElement**) <br/> Classe **MPS.SecureElementReader** | Classe [**SmartCardReader**](https://msdn.microsoft.com/library/windows/apps/dn263857) |
| Sécurité | |
| (SSC = **System.Security.Cryptography**) <br/> Classes **SSC.Aes** et **SSC.RSA** | Classe [**CryptographicEngine**](https://msdn.microsoft.com/library/windows/apps/br241490) |
| (SSC = **System.Security.Cryptography**) <br/> Classes **SSC.HMACSHA256** et **SSC.SHA256** | Classe [**HashAlgorithmProvider**](https://msdn.microsoft.com/library/windows/apps/br241511) |
| (SSC = **System.Security.Cryptography**) <br/> Classe **SSC.ProtectedData** | Classe [**DataProtectionProvider**](https://msdn.microsoft.com/library/windows/apps/br241559) |
| (SSC = **System.Security.Cryptography**) <br/> Classe **SSC.RandomNumberGenerator** | Classe [**CryptographicBuffer**](https://msdn.microsoft.com/library/windows/apps/br227092) |
| (SSC = **System.Security.Cryptography**) <br/> Classe **SSC.X509Certificates.X509Certificate** | Classe [**CertificateEnrollmentManager**](https://msdn.microsoft.com/library/windows/apps/br212075) |
| Interpréteur de commandes | |
| (MPSh = **Microsoft.Phone.Shell**) <br/> Classe **MPSh.ApplicationBar** | Classe [**CommandBar**](https://msdn.microsoft.com/library/windows/apps/dn279427) |
| (MPSh = **Microsoft.Phone.Shell**) <br/> Classe **MPSh.ApplicationBarIconButton** | Classe [**AppBarButton**](https://msdn.microsoft.com/library/windows/apps/dn279244) (lorsqu’elle est utilisée à l’intérieur de la propriété [**PrimaryCommands**](https://msdn.microsoft.com/library/windows/apps/dn279430)) |
| (MPSh = **Microsoft.Phone.Shell**) <br/> Classe **MPSh.ApplicationBarMenuItem** | Classe [**AppBarButton**](https://msdn.microsoft.com/library/windows/apps/dn279244) (lorsqu’elle est utilisée à l’intérieur de la propriété [**SecondaryCommands**](https://msdn.microsoft.com/library/windows/apps/dn279434)) |
| (MPSh = **Microsoft.Phone.Shell**) <br/> Classes **MPSh.CycleTileData**, **MPSh.FlipTileData**, **MPSh.IconicTileData**, **MPSh.ShellTileData** et **MPSh.StandardTileData** | Classe [**TileTemplateType**](https://msdn.microsoft.com/library/windows/apps/br208621) |
| (MPSh = **Microsoft.Phone.Shell**) <br/> Classe **MPSh.PhoneApplicationService** | Classes [**CoreApplication**](https://msdn.microsoft.com/library/windows/apps/br225016) et [**DisplayRequest**](https://msdn.microsoft.com/library/windows/apps/br241816) |
| (MPSh = **Microsoft.Phone.Shell**) <br/> Classe **MPSh.ProgressIndicator** | Classe [**StatusBarProgressIndicator**](https://msdn.microsoft.com/library/windows/apps/dn633865) |
| (MPSh = **Microsoft.Phone.Shell**) <br/> Classe **MPSh.ShellTile** | Classe [**SecondaryTile**](https://msdn.microsoft.com/library/windows/apps/br242183) |
| (MPSh = **Microsoft.Phone.Shell**) <br/> Classe **MPSh.ShellTileSchedule** | Classe [**TileUpdater**](https://msdn.microsoft.com/library/windows/apps/br208628) |
| (MPSh = **Microsoft.Phone.Shell**) <br/> Classe **MPSh.ShellToast** | Classe [**ToastNotificationManager**](https://msdn.microsoft.com/library/windows/apps/br208642) |
| (MPSh = **Microsoft.Phone.Shell**) <br/> Classe **MPSh.SystemTray** | Classe [**StatusBar**](https://msdn.microsoft.com/library/windows/apps/dn633864) |
| Stockage et E/S | |
| Classes **Microsoft.Phone.Storage.ExternalStorage**, **ExternalStorageDevice**, **ExternalStorageFile** et **ExternalStorageFolder** | Classe [**KnownFolders**](https://msdn.microsoft.com/library/windows/apps/br227151) |
| Espace de noms **System.IO** | Espaces de noms [**Windows.Storage**](https://msdn.microsoft.com/library/windows/apps/br227346) et [**Windows.Storage.Streams**](https://msdn.microsoft.com/library/windows/apps/br241791) |
| Classe **System.IO.Directory** | Classe [**StorageFolder**](https://msdn.microsoft.com/library/windows/apps/br227230) |
| Classe **System.IO.File** | Classes [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) et [**PathIO**](https://msdn.microsoft.com/library/windows/apps/hh701663)
| (SII = **System.IO.IsolatedStorage**) <br/> Classe **SII.IsolatedStorageFile** |Propriété [**ApplicationData.LocalFolder**](https://msdn.microsoft.com/library/windows/apps/br241621) |
| (SII = **System.IO.IsolatedStorage**) <br/> Classe **SII.IsolatedStorageSettings** | Propriété [**ApplicationData.LocalSettings**](https://msdn.microsoft.com/library/windows/apps/windows.storage.applicationdata.localsettings.aspx) |
| Classe **System.IO.Stream** | Toujours pris en charge, mais utilisez ReadAsync() et WriteAsync() au lieu de BeginRead()/EndRead() et BeginWrite()/EndWrite(). |
| Portefeuille | |
| Espace de noms **Microsoft.Phone.Wallet** | Espace de noms [**Windows.ApplicationModel.Wallet**](https://msdn.microsoft.com/library/windows/apps/dn631399) |
| Xml | |
| (SX = **System.Xml**) | Méthode **SX.XmlConvert.ToDateTime** |
| (SX = **System.Xml**) | Méthode **SX.XmlConvert.ToDateTimeOffset** |


Rubrique suivante : [Portage du projet](wpsl-to-uwp-porting-to-a-uwp-project.md).

