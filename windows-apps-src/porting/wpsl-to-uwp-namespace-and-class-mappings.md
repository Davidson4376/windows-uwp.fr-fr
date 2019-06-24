---
description: Cette rubrique fournit un mappage complet de Windows Phone Silverlight APIs en leurs équivalents de plateforme universelle Windows (UWP).
title: Windows Phone Silverlight aux mappages d’espace de noms et classe UWP
ms.assetid: 33f06706-4790-48f3-a2e4-ebef9ddb61a4
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 649062c8d1901a7b0f24a69378e13a7725d7c84c
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/21/2019
ms.locfileid: "67322328"
---
# <a name="windowsphone-silverlight-to-uwp-api-mappings"></a>Windows Phone Silverlight aux mappages d’API UWP


Cette rubrique fournit un mappage complet de Windows Phone Silverlight APIs en leurs équivalents de plateforme universelle Windows (UWP). Il ne s’agit généralement pas d’un mappage un-à-un des fonctionnalités : une plateforme peut comporter plus ou moins de fonctionnalités que l’autre dans un espace de noms ou dans une classe.

La table de mappage vous aidera à lorsque vous travaillez dans un projet UWP et vous êtes nouveau à l’aide de code source à partir d’un projet Windows Phone Silverlight. Il existe des différences dans les noms des espaces de noms et des classes (y compris dans les contrôles d’interface utilisateur) entre les deux plateformes. Dans de nombreux cas, pour effectuer un mappage, il suffit de modifier un nom d’espace de noms. Votre code est ensuite compilé. Parfois, un nom de classe ou d’API a changé, ainsi que le nom de l’espace de noms. D’autres fois, le mappage demande un peu plus de travail et, dans de rares cas, il nécessite un changement d’approche.

**L’utilisation de la table :  ** Tout d’abord, recherchez le nom de la classe que vous utilisez. Les classes sont indiquées lorsque le mappage est plus complexe qu’un simple changement de nom de l’espace de noms. Si votre classe n’est pas répertoriée, le mappage correspond simplement à une modification de l’espace de noms. Recherchez le nom de l’espace de noms de votre classe pour trouver son équivalent UWP. Votre classe figurera dans cet espace de noms. Si votre espace de noms n’est pas indiqué, son nom n’a pas changé.

**Remarque**  Windows 10 prend en charge beaucoup plus du .NET Framework qu’une application Windows Phone Store. Par exemple, Windows 10 a plusieurs System.ServiceModel. \* espaces de noms System.Net, System.Net.NetworkInformation et System.Net.Sockets.
En outre, dans une application Windows 10, vous bénéficierez de .NET Native, qui une technologie de compilation ahead of time qui convertit le MSIL dans un code machine exécutable en mode natif. Les applications .NET natives démarrent plus vite, utilisent moins de mémoire et consomment moins de batterie que leurs équivalents MSIL.

| Windows Phone Silverlight | Windows Runtime |
| ------------------------- | --------------- |
| Publicité | |
| Classe **Microsoft.Advertising.Mobile.UI.AdControl** | Classe [AdControl](https://docs.microsoft.com/windows/uwp/monetize/display-ads-using-the-microsoft-advertising-libraries) |
| Alarmes, rappels et agents d’arrière-plan | |
| Classe **Microsoft.Phone.BackgroundAgent** | [**BackgroundTaskBuilder**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder) class |
| Espace de noms **Microsoft.Phone.Scheduler** | [**Windows.ApplicationModel.Background**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background) namespace |
| Classe **Microsoft.Phone.Scheduler.Alarm** | [**BackgroundTaskBuilder** ](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder) et [ **ToastNotificationManager** ](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ToastNotificationManager) classes |
| Classes **Microsoft.Phone.Scheduler.PeriodicTask**, **ScheduledAction**, **ScheduledActionService**, **ScheduledTask** et **ScheduledTaskAgent** | [**BackgroundTaskBuilder**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder) class |
| Classe **Microsoft.Phone.Scheduler.Reminder** | [**BackgroundTaskBuilder** ](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder) et [ **ToastNotificationManager** ](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ToastNotificationManager) classes |
| Classe **Microsoft.Phone.PictureDecoder** | [**BitmapDecoder** ](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.BitmapDecoder) classe |
| Espace de noms **Microsoft.Phone.BackgroundAudio** | [**Windows.Media.Playback** ](https://docs.microsoft.com/uwp/api/Windows.Media.Playback) espace de noms |
| Espace de noms **Microsoft.Phone.BackgroundTransfer** | [**Windows.Networking.BackgroundTransfer**](https://docs.microsoft.com/uwp/api/Windows.Networking.BackgroundTransfer) namespace |
| Environnement et modèle d’application | |
| Classe **System.AppDomain** | Aucun équivalent direct. Voir les classes [**Application**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Application) et [**CoreApplication**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Core.CoreApplication). |
| Classe **System.Environment** | Aucun équivalent direct |
| Classe **System.ComponentModel.Annotations**  | Aucun équivalent direct |
| Classe **System.ComponentModel.BackgroundWorker** | [**ThreadPool**](https://docs.microsoft.com/uwp/api/Windows.System.Threading.ThreadPool) class |
| Classe **System.ComponentModel.DesignerProperties** | [**DesignMode** ](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.DesignMode) classe |
| Classes **System.Threading.Thread** et **System.Threading.ThreadPool** | [**ThreadPool**](https://docs.microsoft.com/uwp/api/Windows.System.Threading.ThreadPool) class |
| (ST = **System.Threading**) <br/> Méthode **ST.Thread.MemoryBarrier** | (ST = **System.Threading**) <br/> Méthode **ST.Interlocked.MemoryBarrier** |
| (ST = **System.Threading**) <br/> Propriété **ST.Thread.ManagedThreadId** | (S = **System**) <br/> Propriété **S.Environment.ManagedThreadId** |
| Classe **System.Threading.Timer** | [**ThreadPoolTimer**](https://docs.microsoft.com/uwp/api/Windows.System.Threading.ThreadPoolTimer) class |
| (SWT = **System.Windows.Threading**) <br/> Classe **SWT.Dispatcher** | [**CoreDispatcher** ](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreDispatcher) classe |
| (SWT = **System.Windows.Threading**) <br/> Classe **SWT.DispatcherTimer** | [**DispatcherTimer** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DispatcherTimer) classe |
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
| Espace de noms **Microsoft.Phone.UserData** | [**Windows.ApplicationModel.Contacts**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Contacts), [**Windows.ApplicationModel.Appointments**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Appointments) namespaces |
| (MPU = **Microsoft.Phone.UserData**) <br/> Classes **MPU.Account**, **ContactAddress**, **ContactCompanyInformation**, **ContactEmailAddress** et **ContactPhoneNumber** | [**Contact** ](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Contacts.Contact) classe |
| (MPU = **Microsoft.Phone.UserData**) <br/> Classe **MPU.Appointments** | [**AppointmentCalendar** ](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Appointments.AppointmentCalendar) classe |
| (MPU = **Microsoft.Phone.UserData**) <br/> Classe **MPU.Contacts** | [**ContactStore**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Contacts.ContactStore) class |
| Contrôles et infrastructure d’interface utilisateur | |
| Classe **ControlTiltEffect.TiltEffect** | Des animations de la bibliothèque d’animations Windows Runtime sont intégrées aux styles par défaut des contrôles courants. Voir [Animation](wpsl-to-uwp-porting-xaml-and-ui.md). |
| Espace de noms **Microsoft.Phone.Controls** | [**Windows.UI.Xaml.Controls**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls) namespace |
| (MPC = **Microsoft.Phone.Controls**) <br/> Classe **MPC.ContextMenu** | [**Menu contextuel** ](https://docs.microsoft.com/uwp/api/Windows.UI.Popups.PopupMenu) classe |
| (MPC = **Microsoft.Phone.Controls**) <br/>Classe **MPC.DatePickerPage** | [**DatePickerFlyout** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.DatePickerFlyout) classe |
| (MPC = **Microsoft.Phone.Controls**) <br/>Classe **MPC.GestureListener** | [**GestureRecognizer** ](https://docs.microsoft.com/uwp/api/Windows.UI.Input.GestureRecognizer) classe |
| (MPC = **Microsoft.Phone.Controls**) <br/>Classe **MPC.LongListSelector** | [**SemanticZoom** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SemanticZoom) classe |
| (MPC = **Microsoft.Phone.Controls**) <br/>Classe **MPC.ObscuredEventArgs** | [**SystemProtection**](https://docs.microsoft.com/uwp/api/Windows.Phone.System.SystemProtection), [**WindowActivatedEventArgs**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.WindowActivatedEventArgs) classes |
| (MPC = **Microsoft.Phone.Controls**) <br/>Classe **MPC.Panorama** | [**Hub** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Hub) classe |
| (MPC = **Microsoft.Phone.Controls**) <br/>Classes **MPC.PhoneApplicationFrame**,<br/>(SWN = **System.Windows.Navigation**) <br/>**SWN.NavigationService** | [**Frame** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Frame) classe |
| (MPC = **Microsoft.Phone.Controls**) <br/>Classe **MPC.PhoneApplicationPage** | [**Page** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Page) classe |
| (MPC = **Microsoft.Phone.Controls**) <br/>Classe **MPC.TiltEffect** | [**PointerDownThemeAnimation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.PointerDownThemeAnimation) class |
| (MPC = **Microsoft.Phone.Controls**) <br/>Classe **MPC.TimePickerPage** | [**TimePickerFlyout** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TimePickerFlyout) classe |
| (MPC = **Microsoft.Phone.Controls**) <br/>Classe **MPC.WebBrowser** | [**WebView** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.WebView) classe |
| (MPC = **Microsoft.Phone.Controls**) <br/>Classe **MPC.WebBrowserExtensions** | Aucun équivalent direct |
| (MPC = **Microsoft.Phone.Controls**) <br/>Classe **MPC.WrapPanel** | Aucun équivalent direct à des fins de disposition générale. [**ItemsWrapGrid** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ItemsWrapGrid) et [ **WrapGrid** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.WrapGrid) peut être utilisé dans le modèle de panneau de configuration des éléments d’un contrôle d’éléments. |
| (MPD = **Microsoft.Phone.Data**) <br/>Espace de noms **MPD.Linq** | Aucun équivalent direct |
| (MPD = **Microsoft.Phone.Data**) <br/>Espace de noms **MPD. Linq.Mapping** | Aucun équivalent direct |
| Espace de noms **Microsoft.Phone.Globalization** | Aucun équivalent direct |
| (MPI = **Microsoft.Phone.Info**) <br/>Classes **MPI.DeviceExtendedProperties** et **DeviceStatus** | [**EasClientDeviceInformation**](https://docs.microsoft.com/uwp/api/Windows.Security.ExchangeActiveSyncProvisioning.EasClientDeviceInformation), [ **MemoryManager** ](https://docs.microsoft.com/uwp/api/Windows.System.MemoryManager) classes. Pour plus d’informations, voir [État de l’appareil](wpsl-to-uwp-input-and-sensors.md). |
| (MPI = **Microsoft.Phone.Info**) <br/>Classe **MPI.MediaCapabilities** | Aucun équivalent direct |
| (MPI = **Microsoft.Phone.Info**) <br/>Classe **MPI.UserExtendedProperties** | [**AdvertisingManager** ](https://docs.microsoft.com/uwp/api/Windows.System.UserProfile.AdvertisingManager) classe |
| Espace de noms **System.Windows** | [**Windows.UI.Xaml**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml) namespace |
| Espace de noms **System.Windows.Automation** | [**Windows.UI.Xaml.Automation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation) namespace |
| Espaces de noms **System.Windows.Controls** et **System.Windows.Input** | [**Windows.UI.Core**](https://docs.microsoft.com/uwp/api/Windows.UI.Core), [**Windows.UI.Input**](https://docs.microsoft.com/uwp/api/Windows.UI.Input), [**Windows.UI.Xaml.Controls**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls) namespaces |
| Classes **System.Windows.Controls.DrawingSurface** et **DrawingSurfaceBackgroundGrid** | [**SwapChainPanel**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SwapChainPanel) class |
| Classe **System.Windows.Controls.RichTextBox** | [**RichEditBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.RichEditBox) class |
| Classe **System.Windows.Controls.WrapPanel** | Aucun équivalent direct à des fins de disposition générale. [**ItemsWrapGrid** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ItemsWrapGrid) et [ **WrapGrid** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.WrapGrid) peut être utilisé dans le modèle de panneau de configuration des éléments d’un contrôle d’éléments. |
| Espace de noms **System.Windows.Controls.Primitives** | [**Windows.UI.Xaml.Controls.Primitives**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives) namespace |
| Espace de noms **System.Windows.Controls.Shapes** | [**Windows.UI.Xaml.Controls.Shapes**](/uwp/api/Windows.UI.Xaml.Shapes) namespace |
| Espace de noms **System.Windows.Data** | [**Windows.UI.Xaml.Data**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data) namespace |
| Espace de noms **System.Windows.Documents** | [**Windows.UI.Xaml.Documents**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Documents) namespace |
| Espace de noms **System.Windows.Ink** | Aucun équivalent direct |
| Espace de noms **System.Windows.Markup** | [**Windows.UI.Xaml.Markup**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Markup) namespace | 
| Espace de noms **System.Windows.Navigation** | [**Windows.UI.Xaml.Navigation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Navigation) namespace |
| Événement **System.Windows.UIElement.Tap**, délégué **EventHandler&lt;GestureEventArgs&gt;** | [**Tapped**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.tapped) event, [**TappedEventHandler**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.tappedeventhandler) delegate |
| Données et services |  |
| Classe **System.Data.Linq.DataContext** | Aucun équivalent direct |
| Classe **System.Data.Linq.Mapping.ColumnAttribute** | Aucun équivalent direct |
| Classe **System.Data.Linq.SqlClient.SqlHelpers** | Aucun équivalent direct |
| Périphériques | |
| Espaces de noms **Microsoft.Devices** et **Microsoft.Devices.Sensors** | [**Windows.Devices.Enumeration**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration), [**Windows.Devices.Enumeration.Pnp**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.Pnp), [**Windows.Devices.Input**](https://docs.microsoft.com/uwp/api/Windows.Devices.Input), [**Windows.Devices.Sensors**](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors) namespaces |
| Classes **Microsoft.Devices.Camera** et **Microsoft.Devices.PhotoCamera** | [**MediaCapture** ](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCapture) classe. Également la classe [**CameraCaptureUI**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.CameraCaptureUI) (Windows uniquement). |
| Classe **Microsoft.Devices.CameraButtons** | [**HardwareButtons** ](https://docs.microsoft.com/uwp/api/Windows.Phone.UI.Input.HardwareButtons) classe |
| Classe **Microsoft.Devices.CameraVideoBrushExtensions** | [**CaptureElement** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CaptureElement) classe |
| Classe **Microsoft.Devices.Environment** | Aucun équivalent direct. Pour contourner ce problème, utilisez la compilation conditionnelle et définissez un symbole personnalisé. Vous pouvez peut-être concevoir une solution de contournement à l’aide de la propriété [IsAttached](https://docs.microsoft.com/dotnet/api/system.diagnostics.debugger.isattached?redirectedfrom=MSDN#System_Diagnostics_Debugger_IsAttached). |
| Classe **Microsoft.Devices.MediaHistory** | Aucun équivalent direct |
| Classe **Microsoft.Devices.VibrateController** | [**VibrationDevice** ](https://docs.microsoft.com/uwp/api/Windows.Phone.Devices.Notification.VibrationDevice) classe |
| Classe **Microsoft.Devices.Radio.FMRadio** | Aucun équivalent direct |
| Classes **Microsoft.Devices.Sensors.Accelerometer** et **Compass** | Dans l’espace de noms [**Windows.Devices.Sensors**](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors) |
| Classe **Microsoft.Devices.Sensors.Gyroscope** | [**Gyromètre** ](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors.Gyrometer) classe |
| Classe **Microsoft.Devices.Sensors.Motion** | [**Inclinomètre** ](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors.Inclinometer) classe |
| Globalisation | |
| Espace de noms **System.Globalization** | [**Windows.Globalization** ](https://docs.microsoft.com/uwp/api/Windows.Globalization) espace de noms |
| (ST = **System.Threading**) <br/> Propriété **ST.Thread.CurrentCulture** | (SG = **System.Globalization**) <br/> Propriété **S.CultureInfo.CurrentCulture** |
| (ST = **System.Threading**) <br/> Propriété **ST.Thread.CurrentUICulture** | (SG = **System.Globalization**) <br/> Propriété **S.CultureInfo.CurrentUICulture** |
| Graphismes et animation | |
| **Microsoft.Xna.Framework. \***  espaces de noms, [bibliothèque de classes XNA Framework](https://go.microsoft.com/fwlink/p/?LinkId=263769), [bibliothèque de classes de Pipeline de contenu](https://go.microsoft.com/fwlink/p/?LinkId=263770) | Aucun équivalent direct. En règle générale, utilisez [Microsoft DirectX](https://docs.microsoft.com/windows/desktop/directx) avec C++. Voir [Développement de jeux](https://docs.microsoft.com/previous-versions/windows/apps/hh452744(v=win.10)) et [Interopérabilité de DirectX et XAML](https://docs.microsoft.com/previous-versions/windows/apps/hh825871(v=win.10)). |
| Classe **Microsoft.Xna.Framework.Audio.Microphone** | [**MediaCapture** ](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCapture) classe |
| Classe **Microsoft.Xna.Framework.Audio.SoundEffect** | [**MediaElement** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MediaElement) classe |
| Espace de noms **Microsoft.Xna.Framework.GamerServices** | (WPS = **Windows.Phone.System**) <br/> [**WPS.UserProfile.GameServices.Core**](https://docs.microsoft.com/uwp/api/Windows.Phone.System.UserProfile.GameServices.Core) namespace |
| Classe **Microsoft.Xna.Framework.GamerServices.Guide** | Aucun équivalent direct |
| Classe **Microsoft.Xna.Framework.Input.GamePad** | [**HardwareButtons** ](https://docs.microsoft.com/uwp/api/Windows.Phone.UI.Input.HardwareButtons) classe |
| Classe **Microsoft.Xna.Framework.Input.Touch.TouchPanel** | [**GestureRecognizer** ](https://docs.microsoft.com/uwp/api/Windows.UI.Input.GestureRecognizer) classe |
| (MXFM = **Microsoft.Xna.Framework.Media**) <br/> Classes **MXFM.MediaLibrary** et **MXFM.PhoneExtensions.MediaLibraryExtensions** | [**KnownFolders** ](https://docs.microsoft.com/uwp/api/Windows.Storage.KnownFolders) classe |
| Classe **Microsoft.Xna.Framework.Media.MediaQueue** | [**SystemMediaTransportControls** ](https://docs.microsoft.com/uwp/api/Windows.Media.SystemMediaTransportControls) classe |
| Classe **Microsoft.Xna.Framework.Media.Playlist** | [**BackgroundMediaPlayer** ](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.BackgroundMediaPlayer) classe |
| Espace de noms **System.Windows.Media** | [**Windows.UI.Xaml.Media**](/uwp/api/Windows.UI.Xaml.Media) namespace |
| Classe **System.Windows.Media.RadialGradientBrush** | Aucun équivalent direct. Voir [Média et graphismes](wpsl-to-uwp-porting-xaml-and-ui.md). |
| Espace de noms **System.Windows.Media.Animation** | [**Windows.UI.Xaml.Media.Animation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation) namespace |
| Espace de noms **System.Windows.Media.Effects** | Aucun équivalent direct |
| Espace de noms **System.Windows.Media.Imaging** | [**Windows.UI.Xaml.Media.Imaging**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging) namespace |
| Espace de noms **System.Windows.Media.Media3D** | [**Windows.UI.Xaml.Media.Media3D**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Media3D) namespace |
| Espace de noms **System.Windows.Shapes** | [**Windows.UI.Xaml.Shapes**](/uwp/api/Windows.UI.Xaml.Shapes) namespace |
| Lanceurs et sélecteurs | |
| Classes **Microsoft.Phone.Tasks.AddressChooserTask**, **EmailAddressChooserTask** et **PhoneNumberChooserTask** | [**ContactPicker** ](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Contacts.ContactPicker) classe |
| Classes **Microsoft.Phone.Tasks.AddWalletItemTask** et **AddWalletItemResult** | [**Windows.ApplicationModel.Wallet**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Wallet) namespace |
| Classes **Microsoft.Phone.Tasks.BingMapsDirectionsTask** et **BingMapsTask** | Aucun équivalent direct |
| Classe **Microsoft.Phone.Tasks.CameraCaptureTask** | [**MediaCapture** ](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCapture) classe. Également la classe [**CameraCaptureUI**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.CameraCaptureUI) (Windows uniquement). |
| **Microsoft.Phone.Tasks.MarketplaceDetailTask** | [**CurrentApp** ](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.CurrentApp) classe ([**RequestAppPurchaseAsync** ](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.requestapppurchaseasync) méthode) |
| Classes **Microsoft.Phone.Tasks.ConnectionSettingsTask**, **MarketplaceHubTask**, **MarketplaceReviewTask**, **MarketplaceSearchTask**, **MediaPlayerLauncher**, **SearchTask**, **SmsComposeTask** et **WebBrowserTask** | [**Lanceur de** ](https://docs.microsoft.com/uwp/api/Windows.System.Launcher) classe |
| Classe **Microsoft.Phone.Tasks.EmailComposeTask** | [**EmailMessage** ](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Email.EmailMessage) classe |
| Classe **Microsoft.Phone.Tasks.GameInviteTask** | Aucun équivalent direct |
| Classes **Microsoft.Phone.Tasks.MapDownloaderTask**, **MapsDirectionsTask**, **MapsTask** et **MapUpdaterTask** | Aucun équivalent direct |
| Classe **Microsoft.Phone.Tasks.PhoneCallTask** | [**PhoneCallManager** ](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Calls.PhoneCallManager) classe |
| Classe **Microsoft.Phone.Tasks.PhotoChooserTask** | [**FileOpenPicker** ](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileOpenPicker) classe |
| Classe **Microsoft.Phone.Tasks.SaveAppointmentTask** | [**AppointmentManager**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Appointments.AppointmentManager) class |
| Classes **Microsoft.Phone.Tasks.SaveContactTask**, **SaveEmailAddressTask** et **SavePhoneNumberTask** | [**StoredContact** ](https://docs.microsoft.com/uwp/api/Windows.Phone.PersonalInformation.StoredContact) classe (Windows Phone uniquement) | 
| Classe **Microsoft.Phone.Tasks.SaveRingtoneTask** | Aucun équivalent direct |
| Classes **Microsoft.Phone.Tasks.ShareLinkTask**, **ShareMediaTask** et **ShareStatusTask** | [**DataPackage** ](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.DataTransfer.DataPackage) classe |
| Location | |
| Espace de noms **System.Device.Location** | [**Windows.Devices.Geolocation** ](https://docs.microsoft.com/uwp/api/Windows.Devices.Geolocation) espace de noms |
| Classe **System.Device.GeoCoordinateWatcher** | [**Geolocator** ](https://docs.microsoft.com/uwp/api/Windows.Devices.Geolocation.Geolocator) classe |
| Cartes | |
| Espaces de noms **Microsoft.Phone.Maps** | [**Windows.Services.Maps**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps) namespace |
| Espace de noms **Microsoft.Phone.Maps.Controls** | [**Windows.UI.Xaml.Controls.Maps**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps) namespace |
| Classe **Microsoft.Phone.Maps.Controls.Map** | [**MapControl** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl) classe |
| Espace de noms **Microsoft.Phone.Maps.Services** | [**Windows.Services.Maps**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps) namespace |
| Classes **Microsoft.Phone.Maps.Services.GeocodeQuery** et **ReverseGeocodeQuery** | [**MapLocationFinder**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinder) class |
| Classe **System.Device.Location.GeoCoordinate** | [**Geopoint** ](https://docs.microsoft.com/uwp/api/Windows.Devices.Geolocation.Geopoint) classe |
| Classe **Microsoft.Phone.Maps.Services.Route** | [**MapRoute** ](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapRoute) classe |
| Classe **Microsoft.Phone.Maps.Services.RouteQuery** | [**MapRouteFinder** ](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapRouteFinder) classe |
| Monétisation | |
| Espace de noms **Microsoft.Phone.Marketplace** | [**Windows.ApplicationModel.Store**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store) namespace |
| Media | |
| Espace de noms **Microsoft.Phone.Media** | [**MediaElement** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MediaElement) classe |
| Mise en réseau | |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> Classe **MPNN.DeviceNetworkInformation** | [**Nom d’hôte**](https://docs.microsoft.com/uwp/api/Windows.Networking.HostName), [ **NetworkInformation** ](https://docs.microsoft.com/uwp/api/Windows.Networking.Connectivity.NetworkInformation) classes |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> Classe **MPNN.NetworkInterface** | [**NetworkInformation** ](https://docs.microsoft.com/uwp/api/Windows.Networking.Connectivity.NetworkInformation) classe |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> Classe **MPNN.NetworkInterfaceInfo** | [**ConnectionProfile** ](https://docs.microsoft.com/uwp/api/Windows.Networking.Connectivity.ConnectionProfile) classe |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> Classe **MPNN.NetworkInterfaceList** | [**NetworkInformation** ](https://docs.microsoft.com/uwp/api/Windows.Networking.Connectivity.NetworkInformation) classe |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> Classe **MPNN.SocketExtensions** | Aucun équivalent direct |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> Classe **MPNN.WebRequestExtensions** | Aucun équivalent direct |
| Espace de noms **Microsoft.Phone.Networking.Voip** | Aucun équivalent direct |
| Classe **System.Net.CookieCollection** | Toujours pris en charge, mais certaines propriétés sont manquantes (par exemple, IsReadOnly) |
| Classe **System.Net.DownloadProgressChangedEventArgs** et classes similaires associées à **System.Net.WebClient** | [**HttpClient** ](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient) classe (ou [System.Net.Http.HttpClient](https://docs.microsoft.com/previous-versions/visualstudio/hh193681(v=vs.118))). Dérive de [System.Net.Http.StreamContent](https://docs.microsoft.com/previous-versions/visualstudio/hh138119(v=vs.118)) pour mesurer la progression. |
| Classes **System.Net.DnsEndPoint** et **IPAddress** | Ces classes sont toujours prises en charge, mais certaines propriétés sont manquantes. Vous pouvez également effectuer le portage vers la classe [**HostName**](https://docs.microsoft.com/uwp/api/Windows.Networking.HostName). |
| Classe **System.Net.HttpUtility** | [**HtmlFormatHelper** ](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.DataTransfer.HtmlFormatHelper) classe |
| Classe **System.Net.HttpWebRequest** | Prise en charge partielle, mais l’alternative prospective recommandée correspond à la classe [**HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient) (ou [System.Net.Http.HttpClient](https://docs.microsoft.com/previous-versions/visualstudio/hh193681(v=vs.118))). Ces API utilisent [System.Net.Http.HttpRequestMessage](https://docs.microsoft.com/previous-versions/visualstudio/hh159020(v=vs.118)) pour représenter une requête HTTP. |
| Classe **System.Net.HttpWebResponse** | Toujours pris en charge, mais utilisez Dispose() au lieu de Close(). Mais l’alternative prospective recommandée correspond à la classe [**HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient) (ou [System.Net.Http.HttpClient](https://docs.microsoft.com/previous-versions/visualstudio/hh193681(v=vs.118))). Ces API utilisent [System.Net.Http.HttpResponseMessage](https://docs.microsoft.com/dotnet/api/system.net.http.httpresponsemessage?redirectedfrom=MSDN) pour représenter une réponse HTTP. |
| (SNN = **System.Net.NetworkInformation**) <br/> Classe **SNN.NetworkChange** | Toujours pris en charge, à l’exception du constructeur. |
| Classe **System.Net.OpenReadCompletedEventArgs** et classes similaires associées à **System.Net.WebClient** | [**HttpClient** ](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient) classe (ou [System.Net.Http.HttpClient](https://docs.microsoft.com/previous-versions/visualstudio/hh193681(v=vs.118))) |
| Classe **System.Net.Sockets.Socket** | Toujours pris en charge, mais utilisez Dispose() au lieu de Close(). Vous pouvez également effectuer le portage vers la classe [**StreamSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.StreamSocket). |
| Classe **System.Net.Sockets.SocketException** | Toujours pris en charge, mais utilisez la propriété SocketErrorCode au lieu d’ErrorCode. |
| Classes **System.Net.Sockets.UdpAnySourceMulticastClient** et **UdpSingleSourceMulticastClient** | [**DatagramSocket** ](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.DatagramSocket) classe |
| Classe **System.Net.UploadProgressChangedEventArgs** et classes similaires associées à **System.Net.WebClient** | [**HttpClient** ](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient) classe (ou [System.Net.Http.HttpClient](https://docs.microsoft.com/previous-versions/visualstudio/hh193681(v=vs.118))) |
| Classe **System.Net.WebClient** | [**HttpClient** ](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient) classe (ou [System.Net.Http.HttpClient](https://docs.microsoft.com/previous-versions/visualstudio/hh193681(v=vs.118))) |
| Classe **System.Net.WebRequest** | Prise en charge partielle (ensemble de propriétés différent), mais l’alternative prospective recommandée correspond à la classe [**HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient) (ou [System.Net.Http.HttpClient](https://docs.microsoft.com/previous-versions/visualstudio/hh193681(v=vs.118))). Ces API utilisent [System.Net.Http.HttpRequestMessage](https://docs.microsoft.com/previous-versions/visualstudio/hh159020(v=vs.118)) pour représenter une requête HTTP. |
| Classe **System.Net.WebResponse** | Toujours pris en charge, mais utilisez Dispose() au lieu de Close(). Mais l’alternative prospective recommandée correspond à la classe [**HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient) (ou [System.Net.Http.HttpClient](https://docs.microsoft.com/previous-versions/visualstudio/hh193681(v=vs.118))). Ces API utilisent [System.Net.Http.HttpResponseMessage](https://docs.microsoft.com/dotnet/api/system.net.http.httpresponsemessage?redirectedfrom=MSDN) pour représenter une réponse HTTP. |
| (SN = **System.Net**) <br/> Classe **SN.WriteStreamClosedEventArgs** | [**HttpClient** ](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient) classe (ou [System.Net.Http.HttpClient](https://docs.microsoft.com/previous-versions/visualstudio/hh193681(v=vs.118))) |
| (SN = **System.Net**) <br/> Classe **SN.WriteStreamClosedEventHandler** | [**HttpClient** ](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient) classe (ou [System.Net.Http.HttpClient](https://docs.microsoft.com/previous-versions/visualstudio/hh193681(v=vs.118))) |
| Classe **System.UriFormatException** | Classe **System.FormatException** |
| Notifications | |
| Espace de noms MPN = **Microsoft.Phone.Notification** | [**Windows.UI.Notifications**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications), [**Windows.Networking.PushNotifications**](https://docs.microsoft.com/uwp/api/Windows.Networking.PushNotifications) namespaces |
| MPN = **Microsoft.Phone.Notification** <br/> Classe **MPN.HttpNotification** | [**TileNotification** ](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.TileNotification) classe |
| MPN = **Microsoft.Phone.Notification** <br/> Classe **MPN.HttpNotificationChannel** | [**PushNotificationChannel** ](https://docs.microsoft.com/uwp/api/Windows.Networking.PushNotifications.PushNotificationChannel) classe |
| Programmation | |
| Espace de noms **System** | [**Windows.Foundation**](https://docs.microsoft.com/uwp/api/Windows.Foundation) namespace |
| Classes **System.Diagnostics.StackFrame** et **StackTrace** | Aucun équivalent direct |
| Espace de noms **System.Diagnostics** | [**Windows.Foundation.Diagnostics**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Diagnostics) namespace |
| Interface **System.ICloneable** | Méthode personnalisée renvoyant le type approprié. |
| Classe **System.Reflection.Emit.ILGenerator** | Aucun équivalent direct |
| Extensions réactives | |
| Espace de noms **Microsoft.Phone.Reactive** | Aucun équivalent direct |
| Réflexion | |
| Classe **System.Type** | Classe **System.Reflection.TypeInfo**. Voir [Réflexion dans .NET Framework pour les applications UWP](https://docs.microsoft.com/dotnet/framework/reflection-and-codedom/reflection-for-windows-store-apps). |
| Ressources | |
| Classe **System.Resources.ResourceManager** | (WA = **Windows.ApplicationModel**)<br/>[**ÉTAT DE WASHINGTON. Resources.Core** ](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Resources.Core) et [ **WA. Ressources** ](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Resources) espaces de noms, [ **ResourceManager** ](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Resources.Core.ResourceManager) classe. Voir [Création et récupération de ressources dans les applications Windows Runtime](https://docs.microsoft.com/previous-versions/windows/apps/hh694557(v=vs.140)). |
| Élément sécurisé | |
| (MPS = **Microsoft.Phone.SecureElement**) <br/> Classes **MPS.SecureElementChannel** et **MPS.SecureElementSession** | [**SmartCardConnection** ](https://docs.microsoft.com/uwp/api/Windows.Devices.SmartCards.SmartCardConnection) classe |
| (MPS = **Microsoft.Phone.SecureElement**) <br/> Classe **MPS.SecureElementReader** | [**SmartCardReader**](https://docs.microsoft.com/uwp/api/Windows.Devices.SmartCards.SmartCardReader) class |
| Sécurité | |
| (SSC = **System.Security.Cryptography**) <br/> Classes **SSC.Aes** et **SSC.RSA** | [**CryptographicEngine** ](https://docs.microsoft.com/uwp/api/Windows.Security.Cryptography.Core.CryptographicEngine) classe |
| (SSC = **System.Security.Cryptography**) <br/> Classes **SSC.HMACSHA256** et **SSC.SHA256** | [**HashAlgorithmProvider** ](https://docs.microsoft.com/uwp/api/Windows.Security.Cryptography.Core.HashAlgorithmProvider) classe |
| (SSC = **System.Security.Cryptography**) <br/> Classe **SSC.ProtectedData** | [**DataProtectionProvider** ](https://docs.microsoft.com/uwp/api/Windows.Security.Cryptography.DataProtection.DataProtectionProvider) classe |
| (SSC = **System.Security.Cryptography**) <br/> Classe **SSC.RandomNumberGenerator** | [**CryptographicBuffer** ](https://docs.microsoft.com/uwp/api/Windows.Security.Cryptography.CryptographicBuffer) classe |
| (SSC = **System.Security.Cryptography**) <br/> Classe **SSC.X509Certificates.X509Certificate** | [**CertificateEnrollmentManager**](https://docs.microsoft.com/uwp/api/Windows.Security.Cryptography.Certificates.CertificateEnrollmentManager) class |
| Shell | |
| (MPSh = **Microsoft.Phone.Shell**) <br/> Classe **MPSh.ApplicationBar** | [**CommandBar** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CommandBar) classe |
| (MPSh = **Microsoft.Phone.Shell**) <br/> Classe **MPSh.ApplicationBarIconButton** | [**AppBarButton** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AppBarButton) classe (lorsqu’il est utilisé à l’intérieur de la [ **PrimaryCommands** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.commandbar.primarycommands) propriété) |
| (MPSh = **Microsoft.Phone.Shell**) <br/> Classe **MPSh.ApplicationBarMenuItem** | [**AppBarButton** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AppBarButton) classe (lorsqu’il est utilisé à l’intérieur de la [ **SecondaryCommands** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.commandbar.secondarycommands) propriété) |
| (MPSh = **Microsoft.Phone.Shell**) <br/> Classes **MPSh.CycleTileData**, **MPSh.FlipTileData**, **MPSh.IconicTileData**, **MPSh.ShellTileData** et **MPSh.StandardTileData** | [**TileTemplateType**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.TileTemplateType) class |
| (MPSh = **Microsoft.Phone.Shell**) <br/> Classe **MPSh.PhoneApplicationService** | [**CoreApplication**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Core.CoreApplication), [**DisplayRequest**](https://docs.microsoft.com/uwp/api/Windows.System.Display.DisplayRequest) classes |
| (MPSh = **Microsoft.Phone.Shell**) <br/> Classe **MPSh.ProgressIndicator** | [**StatusBarProgressIndicator**](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.StatusBarProgressIndicator) class |
| (MPSh = **Microsoft.Phone.Shell**) <br/> Classe **MPSh.ShellTile** | [**SecondaryTile** ](https://docs.microsoft.com/uwp/api/Windows.UI.StartScreen.SecondaryTile) classe |
| (MPSh = **Microsoft.Phone.Shell**) <br/> Classe **MPSh.ShellTileSchedule** | [**TileUpdater**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.TileUpdater) class |
| (MPSh = **Microsoft.Phone.Shell**) <br/> Classe **MPSh.ShellToast** | [**ToastNotificationManager** ](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ToastNotificationManager) classe |
| (MPSh = **Microsoft.Phone.Shell**) <br/> Classe **MPSh.SystemTray** | [**StatusBar** ](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.StatusBar) classe |
| Stockage et E/S | |
| Classes **Microsoft.Phone.Storage.ExternalStorage**, **ExternalStorageDevice**, **ExternalStorageFile** et **ExternalStorageFolder** | [**KnownFolders** ](https://docs.microsoft.com/uwp/api/Windows.Storage.KnownFolders) classe |
| Espace de noms **System.IO** | [**Windows.Storage**](https://docs.microsoft.com/uwp/api/Windows.Storage), [**Windows.Storage.Streams**](https://docs.microsoft.com/uwp/api/Windows.Storage.Streams) namespaces |
| Classe **System.IO.Directory** | [**StorageFolder** ](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFolder) classe |
| Classe **System.IO.File** | [**StorageFile** ](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile) et [ **PathIO** ](https://docs.microsoft.com/uwp/api/Windows.Storage.PathIO) classes
| (SII = **System.IO.IsolatedStorage**) <br/> Classe **SII.IsolatedStorageFile** |[**ApplicationData.LocalFolder** ](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.localfolder) propriété |
| (SII = **System.IO.IsolatedStorage**) <br/> Classe **SII.IsolatedStorageSettings** | [**ApplicationData.LocalSettings** ](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.localsettings) propriété |
| Classe **System.IO.Stream** | Toujours pris en charge, mais utilisez ReadAsync() et WriteAsync() au lieu de BeginRead()/EndRead() et BeginWrite()/EndWrite(). |
| Portefeuille | |
| Espace de noms **Microsoft.Phone.Wallet** | [**Windows.ApplicationModel.Wallet**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Wallet) namespace |
| xml | |
| (SX = **System.Xml**) | Méthode **SX.XmlConvert.ToDateTime** |
| (SX = **System.Xml**) | Méthode **SX.XmlConvert.ToDateTimeOffset** |


Rubrique suivante : [Portage du projet](wpsl-to-uwp-porting-to-a-uwp-project.md).

