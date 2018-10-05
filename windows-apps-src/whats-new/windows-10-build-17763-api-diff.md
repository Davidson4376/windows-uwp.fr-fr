---
author: QuinnRadich
title: Modifications apportées aux API de Windows 10 Build 17763
description: Les développeurs peuvent utiliser la liste suivante pour identifier les espaces de noms nouveaux ou modifiés dans Windows 10, build 17763
keywords: Quelles sont les nouveautés, nouveauté, mises à jour, Windows 10, plus récent, API, octobre 17763,
ms.author: quradic
ms.date: 10/02/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: 178bee54296c355a17a78cb63d17986f51132893
ms.sourcegitcommit: 63cef0a7805f1594984da4d4ff2f76894f12d942
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/05/2018
ms.locfileid: "4388084"
---
# <a name="new-apis-in-windows-10-build-17763"></a>Nouvelles API dans Windows 10, build 17763

Espaces de noms d’API nouveaux et mis à jour ont été mis à disposition pour les développeurs dans Windows 10, build 17763 (également connue sous le 2018 octobre mise à jour ou version 1809). Vous trouverez ci-dessous une liste complète de ressources concernant la documentation publiée pour les espaces de noms ajoutés ou modifiés dans cette version.

Pour plus d’informations sur les API ajoutées dans la version publique précédente, consultez les [Nouvelles API de la mise à jour de Windows 10 avril](windows-10-build-17134-api-diff.md).

## <a name="windowsai"></a>windows.ai

### [<a name="windowsaimachinelearning"></a>Windows.ai.machinelearning](https://docs.microsoft.com/uwp/api/windows.ai.machinelearning)

#### [<a name="ilearningmodelfeaturedescriptor"></a>ilearningmodelfeaturedescriptor](https://docs.microsoft.com/uwp/api/windows.ai.machinelearning.ilearningmodelfeaturedescriptor)

ilearningmodelfeaturedescriptor <br> ilearningmodelfeaturedescriptor.Description <br> ilearningmodelfeaturedescriptor.IsRequired <br> ilearningmodelfeaturedescriptor.Kind <br> ilearningmodelfeaturedescriptor.Name

#### [<a name="ilearningmodelfeaturevalue"></a>ilearningmodelfeaturevalue](https://docs.microsoft.com/uwp/api/windows.ai.machinelearning.ilearningmodelfeaturevalue)

ilearningmodelfeaturevalue <br> ilearningmodelfeaturevalue.Kind

#### [<a name="ilearningmodeloperatorprovider"></a>ilearningmodeloperatorprovider](https://docs.microsoft.com/uwp/api/windows.ai.machinelearning.ilearningmodeloperatorprovider)

ilearningmodeloperatorprovider

#### [<a name="imagefeaturedescriptor"></a>imagefeaturedescriptor](https://docs.microsoft.com/uwp/api/windows.ai.machinelearning.imagefeaturedescriptor)

imagefeaturedescriptor <br> imagefeaturedescriptor.bitmapalphamode <br> imagefeaturedescriptor.bitmappixelformat <br> imagefeaturedescriptor.Description <br> imagefeaturedescriptor.Height <br> imagefeaturedescriptor.IsRequired <br> imagefeaturedescriptor.Kind <br> imagefeaturedescriptor.Name <br> imagefeaturedescriptor.Width

#### [<a name="imagefeaturevalue"></a>imagefeaturevalue](https://docs.microsoft.com/uwp/api/windows.ai.machinelearning.imagefeaturevalue)

imagefeaturevalue <br> imagefeaturevalue.createfromvideoframe <br> imagefeaturevalue.Kind <br> imagefeaturevalue.videoframe

#### [<a name="itensor"></a>itensor](https://docs.microsoft.com/uwp/api/windows.ai.machinelearning.itensor)

itensor <br> itensor.Shape <br> itensor.tensorkind

#### [<a name="learningmodel"></a>learningmodel](https://docs.microsoft.com/uwp/api/windows.ai.machinelearning.learningmodel)

learningmodel <br> learningmodel.Author <br> learningmodel.Close <br> learningmodel.Description <br> learningmodel.Domain <br> learningmodel.inputfeatures <br> learningmodel.loadfromfilepath <br> learningmodel.loadfromfilepath <br> learningmodel.loadfromstoragefileasync <br> learningmodel.loadfromstoragefileasync <br> learningmodel.LoadFromStream <br> learningmodel.LoadFromStream <br> learningmodel.loadfromstreamasync <br> learningmodel.loadfromstreamasync <br> learningmodel.Metadata <br> learningmodel.Name <br> learningmodel.outputfeatures <br> learningmodel.version

#### [<a name="learningmodelbinding"></a>learningmodelbinding](https://docs.microsoft.com/uwp/api/windows.ai.machinelearning.learningmodelbinding)

learningmodelbinding <br> learningmodelbinding.Bind <br> learningmodelbinding.Bind <br> learningmodelbinding.Clear <br> learningmodelbinding.First <br> learningmodelbinding.haskey <br> learningmodelbinding.learningmodelbinding <br> learningmodelbinding.Lookup <br> learningmodelbinding.Size <br> learningmodelbinding.Split

#### [<a name="learningmodeldevice"></a>learningmodeldevice](https://docs.microsoft.com/uwp/api/windows.ai.machinelearning.learningmodeldevice)

learningmodeldevice <br> learningmodeldevice.AdapterId <br> learningmodeldevice.createfromdirect3d11device <br> learningmodeldevice.direct3d11device <br> learningmodeldevice.learningmodeldevice

#### [<a name="learningmodeldevicekind"></a>learningmodeldevicekind](https://docs.microsoft.com/uwp/api/windows.ai.machinelearning.learningmodeldevicekind)

learningmodeldevicekind

#### [<a name="learningmodelevaluationresult"></a>learningmodelevaluationresult](https://docs.microsoft.com/uwp/api/windows.ai.machinelearning.learningmodelevaluationresult)

learningmodelevaluationresult <br> learningmodelevaluationresult.CorrelationId <br> learningmodelevaluationresult.ErrorStatus <br> learningmodelevaluationresult.Outputs <br> learningmodelevaluationresult.Succeeded

#### [<a name="learningmodelfeaturekind"></a>learningmodelfeaturekind](https://docs.microsoft.com/uwp/api/windows.ai.machinelearning.learningmodelfeaturekind)

learningmodelfeaturekind

#### [<a name="learningmodelsession"></a>learningmodelsession](https://docs.microsoft.com/uwp/api/windows.ai.machinelearning.learningmodelsession)

learningmodelsession <br> learningmodelsession.Close <br> learningmodelsession.Device <br> learningmodelsession.Evaluate <br> learningmodelsession.evaluateasync <br> learningmodelsession.evaluatefeatures <br> learningmodelsession.evaluatefeaturesasync <br> learningmodelsession.evaluationproperties <br> learningmodelsession.learningmodelsession <br> learningmodelsession.learningmodelsession <br> learningmodelsession.Model

#### [<a name="mapfeaturedescriptor"></a>mapfeaturedescriptor](https://docs.microsoft.com/uwp/api/windows.ai.machinelearning.mapfeaturedescriptor)

mapfeaturedescriptor <br> mapfeaturedescriptor.Description <br> mapfeaturedescriptor.IsRequired <br> mapfeaturedescriptor.keykind <br> mapfeaturedescriptor.Kind <br> mapfeaturedescriptor.Name <br> mapfeaturedescriptor.valuedescriptor

#### [<a name="sequencefeaturedescriptor"></a>sequencefeaturedescriptor](https://docs.microsoft.com/uwp/api/windows.ai.machinelearning.sequencefeaturedescriptor)

sequencefeaturedescriptor <br> sequencefeaturedescriptor.Description <br> sequencefeaturedescriptor.elementdescriptor <br> sequencefeaturedescriptor.IsRequired <br> sequencefeaturedescriptor.Kind <br> sequencefeaturedescriptor.Name

#### [<a name="tensorboolean"></a>tensorboolean](https://docs.microsoft.com/uwp/api/windows.ai.machinelearning.tensorboolean)

tensorboolean <br> tensorboolean.Create <br> tensorboolean.Create <br> tensorboolean.createfromarray <br> tensorboolean.createfromiterable <br> tensorboolean.getasvectorview <br> tensorboolean.Kind <br> tensorboolean.Shape <br> tensorboolean.tensorkind

#### [<a name="tensordouble"></a>tensordouble](https://docs.microsoft.com/uwp/api/windows.ai.machinelearning.tensordouble)

tensordouble <br> tensordouble.Create <br> tensordouble.Create <br> tensordouble.createfromarray <br> tensordouble.createfromiterable <br> tensordouble.getasvectorview <br> tensordouble.Kind <br> tensordouble.Shape <br> tensordouble.tensorkind

#### [<a name="tensorfeaturedescriptor"></a>tensorfeaturedescriptor](https://docs.microsoft.com/uwp/api/windows.ai.machinelearning.tensorfeaturedescriptor)

tensorfeaturedescriptor <br> tensorfeaturedescriptor.Description <br> tensorfeaturedescriptor.IsRequired <br> tensorfeaturedescriptor.Kind <br> tensorfeaturedescriptor.Name <br> tensorfeaturedescriptor.Shape <br> tensorfeaturedescriptor.tensorkind

#### [<a name="tensorfloat"></a>tensorfloat](https://docs.microsoft.com/uwp/api/windows.ai.machinelearning.tensorfloat)

tensorfloat <br> tensorfloat.Create <br> tensorfloat.Create <br> tensorfloat.createfromarray <br> tensorfloat.createfromiterable <br> tensorfloat.getasvectorview <br> tensorfloat.Kind <br> tensorfloat.Shape <br> tensorfloat.tensorkind

#### [<a name="tensorfloat16bit"></a>tensorfloat16bit](https://docs.microsoft.com/uwp/api/windows.ai.machinelearning.tensorfloat16bit)

tensorfloat16bit <br> tensorfloat16bit.Create <br> tensorfloat16bit.Create <br> tensorfloat16bit.createfromarray <br> tensorfloat16bit.createfromiterable <br> tensorfloat16bit.getasvectorview <br> tensorfloat16bit.Kind <br> tensorfloat16bit.Shape <br> tensorfloat16bit.tensorkind

#### [<a name="tensorint16bit"></a>tensorint16bit](https://docs.microsoft.com/uwp/api/windows.ai.machinelearning.tensorint16bit)

tensorint16bit <br> tensorint16bit.Create <br> tensorint16bit.Create <br> tensorint16bit.createfromarray <br> tensorint16bit.createfromiterable <br> tensorint16bit.getasvectorview <br> tensorint16bit.Kind <br> tensorint16bit.Shape <br> tensorint16bit.tensorkind

#### [<a name="tensorint32bit"></a>tensorint32bit](https://docs.microsoft.com/uwp/api/windows.ai.machinelearning.tensorint32bit)

tensorint32bit <br> tensorint32bit.Create <br> tensorint32bit.Create <br> tensorint32bit.createfromarray <br> tensorint32bit.createfromiterable <br> tensorint32bit.getasvectorview <br> tensorint32bit.Kind <br> tensorint32bit.Shape <br> tensorint32bit.tensorkind

#### [<a name="tensorint64bit"></a>tensorint64bit](https://docs.microsoft.com/uwp/api/windows.ai.machinelearning.tensorint64bit)

tensorint64bit <br> tensorint64bit.Create <br> tensorint64bit.Create <br> tensorint64bit.createfromarray <br> tensorint64bit.createfromiterable <br> tensorint64bit.getasvectorview <br> tensorint64bit.Kind <br> tensorint64bit.Shape <br> tensorint64bit.tensorkind

#### [<a name="tensorint8bit"></a>tensorint8bit](https://docs.microsoft.com/uwp/api/windows.ai.machinelearning.tensorint8bit)

tensorint8bit <br> tensorint8bit.Create <br> tensorint8bit.Create <br> tensorint8bit.createfromarray <br> tensorint8bit.createfromiterable <br> tensorint8bit.getasvectorview <br> tensorint8bit.Kind <br> tensorint8bit.Shape <br> tensorint8bit.tensorkind

#### [<a name="tensorkind"></a>tensorkind](https://docs.microsoft.com/uwp/api/windows.ai.machinelearning.tensorkind)

tensorkind

#### [<a name="tensorstring"></a>tensorstring](https://docs.microsoft.com/uwp/api/windows.ai.machinelearning.tensorstring)

tensorstring <br> tensorstring.Create <br> tensorstring.Create <br> tensorstring.createfromarray <br> tensorstring.createfromiterable <br> tensorstring.getasvectorview <br> tensorstring.Kind <br> tensorstring.Shape <br> tensorstring.tensorkind

#### [<a name="tensoruint16bit"></a>tensoruint16bit](https://docs.microsoft.com/uwp/api/windows.ai.machinelearning.tensoruint16bit)

tensoruint16bit <br> tensoruint16bit.Create <br> tensoruint16bit.Create <br> tensoruint16bit.createfromarray <br> tensoruint16bit.createfromiterable <br> tensoruint16bit.getasvectorview <br> tensoruint16bit.Kind <br> tensoruint16bit.Shape <br> tensoruint16bit.tensorkind

#### [<a name="tensoruint32bit"></a>tensoruint32bit](https://docs.microsoft.com/uwp/api/windows.ai.machinelearning.tensoruint32bit)

tensoruint32bit <br> tensoruint32bit.Create <br> tensoruint32bit.Create <br> tensoruint32bit.createfromarray <br> tensoruint32bit.createfromiterable <br> tensoruint32bit.getasvectorview <br> tensoruint32bit.Kind <br> tensoruint32bit.Shape <br> tensoruint32bit.tensorkind

#### [<a name="tensoruint64bit"></a>tensoruint64bit](https://docs.microsoft.com/uwp/api/windows.ai.machinelearning.tensoruint64bit)

tensoruint64bit <br> tensoruint64bit.Create <br> tensoruint64bit.Create <br> tensoruint64bit.createfromarray <br> tensoruint64bit.createfromiterable <br> tensoruint64bit.getasvectorview <br> tensoruint64bit.Kind <br> tensoruint64bit.Shape <br> tensoruint64bit.tensorkind

#### [<a name="tensoruint8bit"></a>tensoruint8bit](https://docs.microsoft.com/uwp/api/windows.ai.machinelearning.tensoruint8bit)

tensoruint8bit <br> tensoruint8bit.Create <br> tensoruint8bit.Create <br> tensoruint8bit.createfromarray <br> tensoruint8bit.createfromiterable <br> tensoruint8bit.getasvectorview <br> tensoruint8bit.Kind <br> tensoruint8bit.Shape <br> tensoruint8bit.tensorkind

#### [<a name="windows"></a>windows](https://docs.microsoft.com/uwp/api/windows.ai.machinelearning.windows)

Windows.ai.machinelearning

## <a name="windowsapplicationmodel"></a>windows.applicationmodel

### [<a name="windowsapplicationmodelcalls"></a>windows.applicationmodel.calls](https://docs.microsoft.com/uwp/api/windows.applicationmodel.calls)

#### [<a name="voipcallcoordinator"></a>voipcallcoordinator](https://docs.microsoft.com/uwp/api/windows.applicationmodel.calls.voipcallcoordinator)

voipcallcoordinator.reservecallresourcesasync

### [<a name="windowsapplicationmodelchat"></a>Windows.ApplicationModel.Chat](https://docs.microsoft.com/uwp/api/windows.applicationmodel.chat)

#### [<a name="chatcapabilitiesmanager"></a>chatcapabilitiesmanager](https://docs.microsoft.com/uwp/api/windows.applicationmodel.chat.chatcapabilitiesmanager)

chatcapabilitiesmanager.getcachedcapabilitiesasync <br> chatcapabilitiesmanager.getcapabilitiesfromnetworkasync

#### [<a name="rcsmanager"></a>rcsmanager](https://docs.microsoft.com/uwp/api/windows.applicationmodel.chat.rcsmanager)

rcsmanager.transportlistchanged

### [<a name="windowsapplicationmodeldatatransfer"></a>windows.applicationmodel.datatransfer](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer)

#### [<a name="clipboard"></a>Presse-papiers](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.clipboard)

Clipboard.ClearHistory <br> Clipboard.deleteitemfromhistory <br> Clipboard.gethistoryitemsasync <br> Clipboard.historychanged <br> Clipboard.historyenabledchanged <br> Clipboard.ishistoryenabled <br> Clipboard.isroamingenabled <br> Clipboard.roamingenabledchanged <br> Clipboard.setcontentwithoptions <br> Clipboard.sethistoryitemascontent

#### [<a name="clipboardcontentoptions"></a>clipboardcontentoptions](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.clipboardcontentoptions)

clipboardcontentoptions <br> clipboardcontentoptions.clipboardcontentoptions <br> clipboardcontentoptions.historyformats <br> clipboardcontentoptions.isallowedinhistory <br> clipboardcontentoptions.isroamable <br> clipboardcontentoptions.roamingformats

#### [<a name="clipboardhistorychangedeventargs"></a>clipboardhistorychangedeventargs](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.clipboardhistorychangedeventargs)

clipboardhistorychangedeventargs

#### [<a name="clipboardhistoryitem"></a>clipboardhistoryitem](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.clipboardhistoryitem)

clipboardhistoryitem <br> clipboardhistoryitem.Content <br> clipboardhistoryitem.ID <br> clipboardhistoryitem.timestamp

#### [<a name="clipboardhistoryitemsresult"></a>clipboardhistoryitemsresult](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.clipboardhistoryitemsresult)

clipboardhistoryitemsresult <br> clipboardhistoryitemsresult.Items <br> clipboardhistoryitemsresult.Status

#### [<a name="clipboardhistoryitemsresultstatus"></a>clipboardhistoryitemsresultstatus](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.clipboardhistoryitemsresultstatus)

clipboardhistoryitemsresultstatus

#### [<a name="datapackagepropertysetview"></a>datapackagepropertysetview](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.datapackagepropertysetview)

datapackagepropertysetview.isfromroamingclipboard

#### [<a name="sethistoryitemascontentstatus"></a>sethistoryitemascontentstatus](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.sethistoryitemascontentstatus)

sethistoryitemascontentstatus

### [<a name="windowsapplicationmodelstorepreviewinstallcontrol"></a>windows.applicationmodel.store.preview.installcontrol](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.preview.installcontrol)

#### [<a name="appinstallationtoastnotificationmode"></a>appinstallationtoastnotificationmode](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.preview.installcontrol.appinstallationtoastnotificationmode)

appinstallationtoastnotificationmode

#### [<a name="appinstallitem"></a>appinstallitem](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.preview.installcontrol.appinstallitem)

appinstallitem.completedinstalltoastnotificationmode <br> appinstallitem.installinprogresstoastnotificationmode <br> appinstallitem.pintodesktopafterinstall <br> appinstallitem.pintostartafterinstall <br> appinstallitem.pintotaskbarafterinstall

#### [<a name="appinstallmanager"></a>appinstallmanager](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.preview.installcontrol.appinstallmanager)

appinstallmanager.caninstallforallusers

#### [<a name="appinstalloptions"></a>appinstalloptions](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.preview.installcontrol.appinstalloptions)

appinstalloptions.CampaignID <br> appinstalloptions.completedinstalltoastnotificationmode <br> appinstalloptions.extendedcampaignid <br> appinstalloptions.InstallForAllUsers <br> appinstalloptions.installinprogresstoastnotificationmode <br> appinstalloptions.pintodesktopafterinstall <br> appinstalloptions.pintostartafterinstall <br> appinstalloptions.pintotaskbarafterinstall <br> appinstalloptions.stagebutdonotinstall

#### [<a name="appupdateoptions"></a>appupdateoptions](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.preview.installcontrol.appupdateoptions)

appupdateoptions.automaticallydownloadandinstallupdateiffound

### [<a name="windowsapplicationmodelstorepreview"></a>windows.applicationmodel.store.preview](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.preview)

#### [<a name="deliveryoptimizationdownloadmode"></a>deliveryoptimizationdownloadmode](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.preview.deliveryoptimizationdownloadmode)

deliveryoptimizationdownloadmode

#### [<a name="deliveryoptimizationdownloadmodesource"></a>deliveryoptimizationdownloadmodesource](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.preview.deliveryoptimizationdownloadmodesource)

deliveryoptimizationdownloadmodesource

#### [<a name="deliveryoptimizationsettings"></a>deliveryoptimizationsettings](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.preview.deliveryoptimizationsettings)

deliveryoptimizationsettings <br> deliveryoptimizationsettings.downloadmode <br> deliveryoptimizationsettings.downloadmodesource <br> deliveryoptimizationsettings.getcurrentsettings

#### [<a name="storeconfiguration"></a>storeconfiguration](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.preview.storeconfiguration)

storeconfiguration.ispintodesktopsupported <br> storeconfiguration.ispintostartsupported <br> storeconfiguration.ispintotaskbarsupported <br> storeconfiguration.pintodesktop <br> storeconfiguration.pintodesktopforuser

### [<a name="windowsapplicationmodeluseractivities"></a>windows.applicationmodel.useractivities](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities)

#### [<a name="useractivity"></a>useractivity](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities.useractivity)

useractivity.isroamable

### [<a name="windowsapplicationmodel"></a>windows.applicationmodel](https://docs.microsoft.com/uwp/api/windows.applicationmodel)

#### [<a name="appinstallerinfo"></a>appinstallerinfo](https://docs.microsoft.com/uwp/api/windows.applicationmodel.appinstallerinfo)

appinstallerinfo <br> appinstallerinfo.Uri

#### [<a name="limitedaccessfeaturerequestresult"></a>limitedaccessfeaturerequestresult](https://docs.microsoft.com/uwp/api/windows.applicationmodel.limitedaccessfeaturerequestresult)

limitedaccessfeaturerequestresult <br> limitedaccessfeaturerequestresult.estimatedremovaldate <br> limitedaccessfeaturerequestresult.featureId <br> limitedaccessfeaturerequestresult.Status

#### [<a name="limitedaccessfeatures"></a>limitedaccessfeatures](https://docs.microsoft.com/uwp/api/windows.applicationmodel.limitedaccessfeatures)

limitedaccessfeatures <br> limitedaccessfeatures.tryunlockfeature

#### [<a name="limitedaccessfeaturestatus"></a>limitedaccessfeaturestatus](https://docs.microsoft.com/uwp/api/windows.applicationmodel.limitedaccessfeaturestatus)

limitedaccessfeaturestatus

#### [<a name="package"></a>package](https://docs.microsoft.com/uwp/api/windows.applicationmodel.package)

package.checkupdateavailabilityasync <br> package.getappinstallerinfo

#### [<a name="packageupdateavailability"></a>packageupdateavailability](https://docs.microsoft.com/uwp/api/windows.applicationmodel.packageupdateavailability)

packageupdateavailability

#### [<a name="packageupdateavailabilityresult"></a>packageupdateavailabilityresult](https://docs.microsoft.com/uwp/api/windows.applicationmodel.packageupdateavailabilityresult)

packageupdateavailabilityresult <br> packageupdateavailabilityresult.Availability <br> packageupdateavailabilityresult.extendederror

## <a name="windowsdata"></a>Windows.Data

### [<a name="windowsdatatext"></a>Windows.Data.Text](https://docs.microsoft.com/uwp/api/windows.data.text)

#### [<a name="textpredictiongenerator"></a>textpredictiongenerator](https://docs.microsoft.com/uwp/api/windows.data.text.textpredictiongenerator)

textpredictiongenerator.getcandidatesasync <br> textpredictiongenerator.getnextwordcandidatesasync <br> textpredictiongenerator.InputScope

#### [<a name="textpredictionoptions"></a>textpredictionoptions](https://docs.microsoft.com/uwp/api/windows.data.text.textpredictionoptions)

textpredictionoptions

## <a name="windowsdevices"></a>windows.devices

### [<a name="windowsdevicesdisplaycore"></a>Windows.Devices.Display.Core](https://docs.microsoft.com/uwp/api/windows.devices.display.core)

#### [<a name="displayadapter"></a>displayadapter](https://docs.microsoft.com/uwp/api/windows.devices.display.core.displayadapter)

displayadapter <br> displayadapter.deviceinterfacepath <br> displayadapter.fromid <br> displayadapter.ID <br> displayadapter.pcideviceid <br> displayadapter.pcirevision <br> displayadapter.pcisubsystemid <br> displayadapter.pcivendorid <br> displayadapter.Properties <br> displayadapter.SourceCount

#### [<a name="displaybitsperchannel"></a>displaybitsperchannel](https://docs.microsoft.com/uwp/api/windows.devices.display.core.displaybitsperchannel)

displaybitsperchannel

#### [<a name="displaydevice"></a>displaydevice](https://docs.microsoft.com/uwp/api/windows.devices.display.core.displaydevice)

displaydevice <br> displaydevice.createperiodicfence <br> displaydevice.createprimary <br> displaydevice.createscanoutsource <br> displaydevice.createsimplescanout <br> displaydevice.createtaskpool <br> displaydevice.iscapabilitysupported <br> displaydevice.waitforvblank

#### [<a name="displaydevicecapability"></a>displaydevicecapability](https://docs.microsoft.com/uwp/api/windows.devices.display.core.displaydevicecapability)

displaydevicecapability

#### [<a name="displayfence"></a>displayfence](https://docs.microsoft.com/uwp/api/windows.devices.display.core.displayfence)

displayfence

#### [<a name="displaymanager"></a>displaymanager](https://docs.microsoft.com/uwp/api/windows.devices.display.core.displaymanager)

displaymanager <br> displaymanager.Changed <br> displaymanager.Close <br> displaymanager.Create <br> displaymanager.createdisplaydevice <br> displaymanager.Disabled <br> displaymanager.Enabled <br> displaymanager.getcurrentadapters <br> displaymanager.getcurrenttargets <br> displaymanager.pathsfailedorinvalidated <br> displaymanager.releasetarget <br> displaymanager.Start <br> displaymanager.Stop <br> displaymanager.tryacquiretarget <br> displaymanager.tryacquiretargetsandcreateemptystate <br> displaymanager.tryacquiretargetsandcreatesubstate <br> displaymanager.tryacquiretargetsandreadcurrentstate <br> displaymanager.tryreadcurrentstateforalltargets

#### [<a name="displaymanagerchangedeventargs"></a>displaymanagerchangedeventargs](https://docs.microsoft.com/uwp/api/windows.devices.display.core.displaymanagerchangedeventargs)

displaymanagerchangedeventargs <br> displaymanagerchangedeventargs.getdeferral <br> displaymanagerchangedeventargs.Handled

#### [<a name="displaymanagerdisabledeventargs"></a>displaymanagerdisabledeventargs](https://docs.microsoft.com/uwp/api/windows.devices.display.core.displaymanagerdisabledeventargs)

displaymanagerdisabledeventargs <br> displaymanagerdisabledeventargs.getdeferral <br> displaymanagerdisabledeventargs.Handled

#### [<a name="displaymanagerenabledeventargs"></a>displaymanagerenabledeventargs](https://docs.microsoft.com/uwp/api/windows.devices.display.core.displaymanagerenabledeventargs)

displaymanagerenabledeventargs <br> displaymanagerenabledeventargs.getdeferral <br> displaymanagerenabledeventargs.Handled

#### [<a name="displaymanageroptions"></a>displaymanageroptions](https://docs.microsoft.com/uwp/api/windows.devices.display.core.displaymanageroptions)

displaymanageroptions

#### [<a name="displaymanagerpathsfailedorinvalidatedeventargs"></a>displaymanagerpathsfailedorinvalidatedeventargs](https://docs.microsoft.com/uwp/api/windows.devices.display.core.displaymanagerpathsfailedorinvalidatedeventargs)

displaymanagerpathsfailedorinvalidatedeventargs <br> displaymanagerpathsfailedorinvalidatedeventargs.getdeferral <br> displaymanagerpathsfailedorinvalidatedeventargs.Handled

#### [<a name="displaymanagerresult"></a>displaymanagerresult](https://docs.microsoft.com/uwp/api/windows.devices.display.core.displaymanagerresult)

displaymanagerresult

#### [<a name="displaymanagerresultwithstate"></a>displaymanagerresultwithstate](https://docs.microsoft.com/uwp/api/windows.devices.display.core.displaymanagerresultwithstate)

displaymanagerresultwithstate <br> displaymanagerresultwithstate.ErrorCode <br> displaymanagerresultwithstate.extendederrorcode <br> displaymanagerresultwithstate.State

#### [<a name="displaymodeinfo"></a>displaymodeinfo](https://docs.microsoft.com/uwp/api/windows.devices.display.core.displaymodeinfo)

displaymodeinfo <br> displaymodeinfo.getwireformatsupportedbitsperchannel <br> displaymodeinfo.isinterlaced <br> displaymodeinfo.isstereo <br> displaymodeinfo.iswireformatsupported <br> displaymodeinfo.presentationrate <br> displaymodeinfo.Properties <br> displaymodeinfo.sourcepixelformat <br> displaymodeinfo.sourceresolution <br> displaymodeinfo.targetresolution

#### [<a name="displaymodequeryoptions"></a>displaymodequeryoptions](https://docs.microsoft.com/uwp/api/windows.devices.display.core.displaymodequeryoptions)

displaymodequeryoptions

#### [<a name="displaypath"></a>chemin affiché](https://docs.microsoft.com/uwp/api/windows.devices.display.core.displaypath)

chemin affiché <br> displaypath.applypropertiesfrommode <br> displaypath.findmodes <br> displaypath.isinterlaced <br> displaypath.isstereo <br> displaypath.presentationrate <br> displaypath.Properties <br> displaypath.rotation <br> displaypath.Scaling <br> displaypath.sourcepixelformat <br> displaypath.sourceresolution <br> displaypath.Status <br> displaypath.Target <br> displaypath.targetresolution <br> displaypath.View <br> displaypath.wireformat

#### [<a name="displaypathscaling"></a>displaypathscaling](https://docs.microsoft.com/uwp/api/windows.devices.display.core.displaypathscaling)

displaypathscaling

#### [<a name="displaypathstatus"></a>displaypathstatus](https://docs.microsoft.com/uwp/api/windows.devices.display.core.displaypathstatus)

displaypathstatus

#### [<a name="displaypresentationrate"></a>displaypresentationrate](https://docs.microsoft.com/uwp/api/windows.devices.display.core.displaypresentationrate)

displaypresentationrate

#### [<a name="displayprimarydescription"></a>displayprimarydescription](https://docs.microsoft.com/uwp/api/windows.devices.display.core.displayprimarydescription)

displayprimarydescription <br> displayprimarydescription.ColorSpace <br> displayprimarydescription.createwithproperties <br> displayprimarydescription.displayprimarydescription <br> displayprimarydescription.format <br> displayprimarydescription.Height <br> displayprimarydescription.isstereo <br> displayprimarydescription.multisampledescription <br> displayprimarydescription.Properties <br> displayprimarydescription.Width

#### [<a name="displayrotation"></a>DisplayRotation](https://docs.microsoft.com/uwp/api/windows.devices.display.core.displayrotation)

DisplayRotation

#### [<a name="displayscanout"></a>displayscanout](https://docs.microsoft.com/uwp/api/windows.devices.display.core.displayscanout)

displayscanout

#### [<a name="displaysource"></a>displaysource](https://docs.microsoft.com/uwp/api/windows.devices.display.core.displaysource)

displaysource <br> displaysource.AdapterId <br> displaysource.GetMetadata <br> displaysource.SourceID

#### [<a name="displaystate"></a>displayState](https://docs.microsoft.com/uwp/api/windows.devices.display.core.displaystate)

displayState <br> displayState.canconnecttargettoview <br> displayState.Clone <br> displayState.connecttarget <br> displayState.connecttarget <br> displayState.disconnecttarget <br> displayState.getpathfortarget <br> displayState.getviewfortarget <br> displayState.IsReadOnly <br> displayState.isstale <br> displayState.Properties <br> displayState.targets <br> displayState.tryapply <br> displayState.tryfunctionalize <br> displayState.Views

#### [<a name="displaystateapplyoptions"></a>displaystateapplyoptions](https://docs.microsoft.com/uwp/api/windows.devices.display.core.displaystateapplyoptions)

displaystateapplyoptions

#### [<a name="displaystatefunctionalizeoptions"></a>displaystatefunctionalizeoptions](https://docs.microsoft.com/uwp/api/windows.devices.display.core.displaystatefunctionalizeoptions)

displaystatefunctionalizeoptions

#### [<a name="displaystateoperationresult"></a>displaystateoperationresult](https://docs.microsoft.com/uwp/api/windows.devices.display.core.displaystateoperationresult)

displaystateoperationresult <br> displaystateoperationresult.extendederrorcode <br> displaystateoperationresult.Status

#### [<a name="displaystateoperationstatus"></a>displaystateoperationstatus](https://docs.microsoft.com/uwp/api/windows.devices.display.core.displaystateoperationstatus)

displaystateoperationstatus

#### [<a name="displaysurface"></a>displaysurface](https://docs.microsoft.com/uwp/api/windows.devices.display.core.displaysurface)

displaysurface

#### [<a name="displaytarget"></a>displaytarget](https://docs.microsoft.com/uwp/api/windows.devices.display.core.displaytarget)

displaytarget <br> displaytarget.adapter <br> displaytarget.adapterrelativeid <br> displaytarget.deviceinterfacepath <br> displaytarget.IsConnected <br> displaytarget.IsEqual <br> displaytarget.issame <br> displaytarget.isstale <br> displaytarget.isvirtualmodeenabled <br> displaytarget.isvirtualtopologyenabled <br> displaytarget.monitorpersistence <br> displaytarget.Properties <br> displaytarget.stablemonitorid <br> displaytarget.trygetmonitor <br> displaytarget.usagekind

#### [<a name="displaytargetpersistence"></a>displaytargetpersistence](https://docs.microsoft.com/uwp/api/windows.devices.display.core.displaytargetpersistence)

displaytargetpersistence

#### [<a name="displaytask"></a>displaytask](https://docs.microsoft.com/uwp/api/windows.devices.display.core.displaytask)

displaytask <br> displaytask.setscanout <br> displaytask.setwait

#### [<a name="displaytaskpool"></a>displaytaskpool](https://docs.microsoft.com/uwp/api/windows.devices.display.core.displaytaskpool)

displaytaskpool <br> displaytaskpool.CreateTask <br> displaytaskpool.ExecuteTask

#### [<a name="displaytasksignalkind"></a>displaytasksignalkind](https://docs.microsoft.com/uwp/api/windows.devices.display.core.displaytasksignalkind)

displaytasksignalkind

#### [<a name="displayview"></a>displayview](https://docs.microsoft.com/uwp/api/windows.devices.display.core.displayview)

displayview <br> displayview.contentresolution <br> displayview.Paths <br> displayview.Properties <br> displayview.setprimarypath

#### [<a name="displaywireformat"></a>displaywireformat](https://docs.microsoft.com/uwp/api/windows.devices.display.core.displaywireformat)

displaywireformat <br> displaywireformat.bitsperchannel <br> displaywireformat.ColorSpace <br> displaywireformat.createwithproperties <br> displaywireformat.displaywireformat <br> displaywireformat.eotf <br> displaywireformat.hdrmetadata <br> displaywireformat.pixelencoding <br> displaywireformat.Properties

#### [<a name="displaywireformatcolorspace"></a>displaywireformatcolorspace](https://docs.microsoft.com/uwp/api/windows.devices.display.core.displaywireformatcolorspace)

displaywireformatcolorspace

#### [<a name="displaywireformateotf"></a>displaywireformateotf](https://docs.microsoft.com/uwp/api/windows.devices.display.core.displaywireformateotf)

displaywireformateotf

#### [<a name="displaywireformathdrmetadata"></a>displaywireformathdrmetadata](https://docs.microsoft.com/uwp/api/windows.devices.display.core.displaywireformathdrmetadata)

displaywireformathdrmetadata

#### [<a name="displaywireformatpixelencoding"></a>displaywireformatpixelencoding](https://docs.microsoft.com/uwp/api/windows.devices.display.core.displaywireformatpixelencoding)

displaywireformatpixelencoding

#### [<a name="windows"></a>windows](https://docs.microsoft.com/uwp/api/windows.devices.display.core.windows)

Windows.Devices.Display.Core

### [<a name="windowsdevicesenumeration"></a>Windows.Devices.Enumeration](https://docs.microsoft.com/uwp/api/windows.devices.enumeration)

#### [<a name="deviceinformationpairing"></a>deviceinformationpairing](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformationpairing)

deviceinformationpairing.tryregisterforallinboundpairingrequestswithprotectionlevel

### [<a name="windowsdeviceslightseffects"></a>Windows.Devices.Lights.Effects](https://docs.microsoft.com/uwp/api/windows.devices.lights.effects)

#### [<a name="ilamparrayeffect"></a>ilamparrayeffect](https://docs.microsoft.com/uwp/api/windows.devices.lights.effects.ilamparrayeffect)

ilamparrayeffect <br> ilamparrayeffect.ZIndex

#### [<a name="lamparraybitmapeffect"></a>lamparraybitmapeffect](https://docs.microsoft.com/uwp/api/windows.devices.lights.effects.lamparraybitmapeffect)

lamparraybitmapeffect <br> lamparraybitmapeffect.bitmaprequested <br> lamparraybitmapeffect.Duration <br> lamparraybitmapeffect.lamparraybitmapeffect <br> lamparraybitmapeffect.startdelay <br> lamparraybitmapeffect.suggestedbitmapsize <br> lamparraybitmapeffect.UpdateInterval <br> lamparraybitmapeffect.ZIndex

#### [<a name="lamparraybitmaprequestedeventargs"></a>lamparraybitmaprequestedeventargs](https://docs.microsoft.com/uwp/api/windows.devices.lights.effects.lamparraybitmaprequestedeventargs)

lamparraybitmaprequestedeventargs <br> lamparraybitmaprequestedeventargs.sincestarted <br> lamparraybitmaprequestedeventargs.updatebitmap

#### [<a name="lamparrayblinkeffect"></a>lamparrayblinkeffect](https://docs.microsoft.com/uwp/api/windows.devices.lights.effects.lamparrayblinkeffect)

lamparrayblinkeffect <br> lamparrayblinkeffect.attackduration <br> lamparrayblinkeffect.Color <br> lamparrayblinkeffect.decayduration <br> lamparrayblinkeffect.lamparrayblinkeffect <br> lamparrayblinkeffect.occurrences <br> lamparrayblinkeffect.repetitiondelay <br> lamparrayblinkeffect.repetitionmode <br> lamparrayblinkeffect.startdelay <br> lamparrayblinkeffect.sustainduration <br> lamparrayblinkeffect.ZIndex

#### [<a name="lamparraycolorrampeffect"></a>lamparraycolorrampeffect](https://docs.microsoft.com/uwp/api/windows.devices.lights.effects.lamparraycolorrampeffect)

lamparraycolorrampeffect <br> lamparraycolorrampeffect.Color <br> lamparraycolorrampeffect.completionbehavior <br> lamparraycolorrampeffect.lamparraycolorrampeffect <br> lamparraycolorrampeffect.rampduration <br> lamparraycolorrampeffect.startdelay <br> lamparraycolorrampeffect.ZIndex

#### [<a name="lamparraycustomeffect"></a>lamparraycustomeffect](https://docs.microsoft.com/uwp/api/windows.devices.lights.effects.lamparraycustomeffect)

lamparraycustomeffect <br> lamparraycustomeffect.Duration <br> lamparraycustomeffect.lamparraycustomeffect <br> lamparraycustomeffect.UpdateInterval <br> lamparraycustomeffect.updaterequested <br> lamparraycustomeffect.ZIndex

#### [<a name="lamparrayeffectcompletionbehavior"></a>lamparrayeffectcompletionbehavior](https://docs.microsoft.com/uwp/api/windows.devices.lights.effects.lamparrayeffectcompletionbehavior)

lamparrayeffectcompletionbehavior

#### [<a name="lamparrayeffectplaylist"></a>lamparrayeffectplaylist](https://docs.microsoft.com/uwp/api/windows.devices.lights.effects.lamparrayeffectplaylist)

lamparrayeffectplaylist <br> lamparrayeffectplaylist.Append <br> lamparrayeffectplaylist.effectstartmode <br> lamparrayeffectplaylist.First <br> lamparrayeffectplaylist.GetAt <br> lamparrayeffectplaylist.getmany <br> lamparrayeffectplaylist.IndexOf <br> lamparrayeffectplaylist.lamparrayeffectplaylist <br> lamparrayeffectplaylist.occurrences <br> lamparrayeffectplaylist.overridezindex <br> lamparrayeffectplaylist.pause <br> lamparrayeffectplaylist.pauseall <br> lamparrayeffectplaylist.repetitionmode <br> lamparrayeffectplaylist.Size <br> lamparrayeffectplaylist.Start <br> lamparrayeffectplaylist.startall <br> lamparrayeffectplaylist.Stop <br> lamparrayeffectplaylist.stopAll

#### [<a name="lamparrayeffectstartmode"></a>lamparrayeffectstartmode](https://docs.microsoft.com/uwp/api/windows.devices.lights.effects.lamparrayeffectstartmode)

lamparrayeffectstartmode

#### [<a name="lamparrayrepetitionmode"></a>lamparrayrepetitionmode](https://docs.microsoft.com/uwp/api/windows.devices.lights.effects.lamparrayrepetitionmode)

lamparrayrepetitionmode

#### [<a name="lamparraysolideffect"></a>lamparraysolideffect](https://docs.microsoft.com/uwp/api/windows.devices.lights.effects.lamparraysolideffect)

lamparraysolideffect <br> lamparraysolideffect.Color <br> lamparraysolideffect.completionbehavior <br> lamparraysolideffect.Duration <br> lamparraysolideffect.lamparraysolideffect <br> lamparraysolideffect.startdelay <br> lamparraysolideffect.ZIndex

#### [<a name="lamparrayupdaterequestedeventargs"></a>lamparrayupdaterequestedeventargs](https://docs.microsoft.com/uwp/api/windows.devices.lights.effects.lamparrayupdaterequestedeventargs)

lamparrayupdaterequestedeventargs <br> lamparrayupdaterequestedeventargs.SetColor <br> lamparrayupdaterequestedeventargs.setcolorforindex <br> lamparrayupdaterequestedeventargs.setcolorsforindices <br> lamparrayupdaterequestedeventargs.setsinglecolorforindices <br> lamparrayupdaterequestedeventargs.sincestarted

#### [<a name="windows"></a>windows](https://docs.microsoft.com/uwp/api/windows.devices.lights.effects.windows)

Windows.Devices.Lights.Effects

### [<a name="windowsdeviceslights"></a>Windows.Devices.Lights](https://docs.microsoft.com/uwp/api/windows.devices.lights)

#### [<a name="lamparray"></a>lamparray](https://docs.microsoft.com/uwp/api/windows.devices.lights.lamparray)

lamparray <br> lamparray.BoundingBox <br> lamparray.brightnesslevel <br> lamparray.DeviceID <br> lamparray.fromidasync <br> lamparray.getdeviceselector <br> lamparray.getindicesforkey <br> lamparray.getindicesforpurposes <br> lamparray.getlampinfo <br> lamparray.hardwareproductid <br> lamparray.hardwarevendorid <br> lamparray.HardwareVersion <br> lamparray.IsConnected <br> lamparray.IsEnabled <br> lamparray.lamparraykind <br> lamparray.lampcount <br> lamparray.minupdateinterval <br> lamparray.requestmessageasync <br> lamparray.sendmessageasync <br> lamparray.SetColor <br> lamparray.setcolorforindex <br> lamparray.setcolorsforindices <br> lamparray.setcolorsforkey <br> lamparray.setcolorsforkeys <br> lamparray.setcolorsforpurposes <br> lamparray.setsinglecolorforindices <br> lamparray.supportsvirtualkeys

#### [<a name="lamparraykind"></a>lamparraykind](https://docs.microsoft.com/uwp/api/windows.devices.lights.lamparraykind)

lamparraykind

#### [<a name="lampinfo"></a>lampinfo](https://docs.microsoft.com/uwp/api/windows.devices.lights.lampinfo)

lampinfo <br> lampinfo.bluelevelcount <br> lampinfo.fixedcolor <br> lampinfo.gainlevelcount <br> lampinfo.getnearestsupportedcolor <br> lampinfo.greenlevelcount <br> lampinfo.index <br> lampinfo.position <br> lampinfo.purposes <br> lampinfo.redlevelcount <br> lampinfo.updatelatency

#### [<a name="lamppurposes"></a>lamppurposes](https://docs.microsoft.com/uwp/api/windows.devices.lights.lamppurposes)

lamppurposes

### [<a name="windowsdevicespointofserviceprovider"></a>windows.devices.pointofservice.provider](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.provider)

#### [<a name="barcodescannerdisablescannerrequest"></a>barcodescannerdisablescannerrequest](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.provider.barcodescannerdisablescannerrequest)

barcodescannerdisablescannerrequest.reportfailedasync <br> barcodescannerdisablescannerrequest.reportfailedasync

#### [<a name="barcodescannerenablescannerrequest"></a>barcodescannerenablescannerrequest](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.provider.barcodescannerenablescannerrequest)

barcodescannerenablescannerrequest.reportfailedasync <br> barcodescannerenablescannerrequest.reportfailedasync

#### [<a name="barcodescannerframereader"></a>barcodescannerframereader](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.provider.barcodescannerframereader)

barcodescannerframereader <br> barcodescannerframereader.Close <br> barcodescannerframereader.Connection <br> barcodescannerframereader.framearrived <br> barcodescannerframereader.startasync <br> barcodescannerframereader.stopasync <br> barcodescannerframereader.tryacquirelatestframeasync

#### [<a name="barcodescannerframereaderframearrivedeventargs"></a>barcodescannerframereaderframearrivedeventargs](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.provider.barcodescannerframereaderframearrivedeventargs)

barcodescannerframereaderframearrivedeventargs <br> barcodescannerframereaderframearrivedeventargs.getdeferral

#### [<a name="barcodescannergetsymbologyattributesrequest"></a>barcodescannergetsymbologyattributesrequest](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.provider.barcodescannergetsymbologyattributesrequest)

barcodescannergetsymbologyattributesrequest.reportfailedasync <br> barcodescannergetsymbologyattributesrequest.reportfailedasync

#### [<a name="barcodescannerhidevideopreviewrequest"></a>barcodescannerhidevideopreviewrequest](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.provider.barcodescannerhidevideopreviewrequest)

barcodescannerhidevideopreviewrequest.reportfailedasync <br> barcodescannerhidevideopreviewrequest.reportfailedasync

#### [<a name="barcodescannerproviderconnection"></a>barcodescannerproviderconnection](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.provider.barcodescannerproviderconnection)

barcodescannerproviderconnection.createframereaderasync <br> barcodescannerproviderconnection.createframereaderasync <br> barcodescannerproviderconnection.createframereaderasync

#### [<a name="barcodescannersetactivesymbologiesrequest"></a>barcodescannersetactivesymbologiesrequest](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.provider.barcodescannersetactivesymbologiesrequest)

barcodescannersetactivesymbologiesrequest.reportfailedasync <br> barcodescannersetactivesymbologiesrequest.reportfailedasync

#### [<a name="barcodescannersetsymbologyattributesrequest"></a>barcodescannersetsymbologyattributesrequest](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.provider.barcodescannersetsymbologyattributesrequest)

barcodescannersetsymbologyattributesrequest.reportfailedasync <br> barcodescannersetsymbologyattributesrequest.reportfailedasync

#### [<a name="barcodescannerstartsoftwaretriggerrequest"></a>barcodescannerstartsoftwaretriggerrequest](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.provider.barcodescannerstartsoftwaretriggerrequest)

barcodescannerstartsoftwaretriggerrequest.reportfailedasync <br> barcodescannerstartsoftwaretriggerrequest.reportfailedasync

#### [<a name="barcodescannerstopsoftwaretriggerrequest"></a>barcodescannerstopsoftwaretriggerrequest](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.provider.barcodescannerstopsoftwaretriggerrequest)

barcodescannerstopsoftwaretriggerrequest.reportfailedasync <br> barcodescannerstopsoftwaretriggerrequest.reportfailedasync

#### [<a name="barcodescannervideoframe"></a>barcodescannervideoframe](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.provider.barcodescannervideoframe)

barcodescannervideoframe <br> barcodescannervideoframe.Close <br> barcodescannervideoframe.format <br> barcodescannervideoframe.Height <br> barcodescannervideoframe.pixeldata <br> barcodescannervideoframe.Width

### [<a name="windowsdevicespointofservice"></a>windows.devices.pointofservice](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice)

#### [<a name="barcodescannercapabilities"></a>barcodescannercapabilities](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannercapabilities)

barcodescannercapabilities.isvideopreviewsupported

#### [<a name="claimedbarcodescanner"></a>claimedbarcodescanner](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner)

claimedbarcodescanner.Closed

#### [<a name="claimedbarcodescannerclosedeventargs"></a>claimedbarcodescannerclosedeventargs](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescannerclosedeventargs)

claimedbarcodescannerclosedeventargs

#### [<a name="claimedcashdrawer"></a>claimedcashdrawer](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedcashdrawer)

claimedcashdrawer.Closed

#### [<a name="claimedcashdrawerclosedeventargs"></a>claimedcashdrawerclosedeventargs](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedcashdrawerclosedeventargs)

claimedcashdrawerclosedeventargs

#### [<a name="claimedlinedisplay"></a>claimedlinedisplay](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedlinedisplay)

claimedlinedisplay.Closed

#### [<a name="claimedlinedisplayclosedeventargs"></a>claimedlinedisplayclosedeventargs](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedlinedisplayclosedeventargs)

claimedlinedisplayclosedeventargs

#### [<a name="claimedmagneticstripereader"></a>claimedmagneticstripereader](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader)

claimedmagneticstripereader.Closed

#### [<a name="claimedmagneticstripereaderclosedeventargs"></a>claimedmagneticstripereaderclosedeventargs](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedmagneticstripereaderclosedeventargs)

claimedmagneticstripereaderclosedeventargs

#### [<a name="claimedposprinter"></a>claimedposprinter](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedposprinter)

claimedposprinter.Closed

#### [<a name="claimedposprinterclosedeventargs"></a>claimedposprinterclosedeventargs](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedposprinterclosedeventargs)

claimedposprinterclosedeventargs

### [<a name="windowsdevicessensors"></a>windows.devices.sensors](https://docs.microsoft.com/uwp/api/windows.devices.sensors)

#### [<a name="hingeanglereading"></a>hingeanglereading](https://docs.microsoft.com/uwp/api/windows.devices.sensors.hingeanglereading)

hingeanglereading <br> hingeanglereading.angleindegrees <br> hingeanglereading.Properties <br> hingeanglereading.timestamp

#### [<a name="hingeanglesensor"></a>hingeanglesensor](https://docs.microsoft.com/uwp/api/windows.devices.sensors.hingeanglesensor)

hingeanglesensor <br> hingeanglesensor.DeviceID <br> hingeanglesensor.fromidasync <br> hingeanglesensor.getcurrentreadingasync <br> hingeanglesensor.getdefaultasync <br> hingeanglesensor.getdeviceselector <br> hingeanglesensor.getrelatedtoadjacentpanelsasync <br> hingeanglesensor.minreportthresholdindegrees <br> hingeanglesensor.readingchanged <br> hingeanglesensor.reportthresholdindegrees

#### [<a name="hingeanglesensorreadingchangedeventargs"></a>hingeanglesensorreadingchangedeventargs](https://docs.microsoft.com/uwp/api/windows.devices.sensors.hingeanglesensorreadingchangedeventargs)

hingeanglesensorreadingchangedeventargs <br> hingeanglesensorreadingchangedeventargs.Reading

#### [<a name="simpleorientationsensor"></a>simpleorientationsensor](https://docs.microsoft.com/uwp/api/windows.devices.sensors.simpleorientationsensor)

simpleorientationsensor.fromidasync <br> simpleorientationsensor.getdeviceselector

### [<a name="windowsdevicessmartcards"></a>windows.devices.smartcards](https://docs.microsoft.com/uwp/api/windows.devices.smartcards)

#### [<a name="knownsmartcardappletids"></a>knownsmartcardappletids](https://docs.microsoft.com/uwp/api/windows.devices.smartcards.knownsmartcardappletids)

knownsmartcardappletids <br> knownsmartcardappletids.paymentsystemenvironment <br> knownsmartcardappletids.proximitypaymentsystemenvironment

#### [<a name="smartcardappletidgroup"></a>smartcardappletidgroup](https://docs.microsoft.com/uwp/api/windows.devices.smartcards.smartcardappletidgroup)

smartcardappletidgroup.Description <br> smartcardappletidgroup.logo <br> smartcardappletidgroup.Properties <br> smartcardappletidgroup.secureuserauthenticationrequired

#### [<a name="smartcardappletidgroupregistration"></a>smartcardappletidgroupregistration](https://docs.microsoft.com/uwp/api/windows.devices.smartcards.smartcardappletidgroupregistration)

smartcardappletidgroupregistration.setpropertiesasync <br> smartcardappletidgroupregistration.smartcardreaderid

## <a name="windowsfoundation"></a>windows.foundation

### [<a name="windowsfoundation"></a>windows.foundation](https://docs.microsoft.com/uwp/api/windows.foundation)

#### [<a name="guidhelper"></a>guidhelper](https://docs.microsoft.com/uwp/api/windows.foundation.guidhelper)

guidhelper <br> guidhelper.createnewguid <br> guidhelper.Empty <br> guidhelper.Equals

## <a name="windowsglobalization"></a>windows.globalization

### [<a name="windowsglobalization"></a>windows.globalization](https://docs.microsoft.com/uwp/api/windows.globalization)

#### [<a name="currencyidentifiers"></a>currencyidentifiers](https://docs.microsoft.com/uwp/api/windows.globalization.currencyidentifiers)

currencyidentifiers.MRU <br> currencyidentifiers.SSP <br> currencyidentifiers.STN <br> currencyidentifiers.VES

## <a name="windowsgraphics"></a>windows.graphics

### [<a name="windowsgraphicscapture"></a>windows.graphics.capture](https://docs.microsoft.com/uwp/api/windows.graphics.capture)

#### [<a name="direct3d11captureframepool"></a>direct3d11captureframepool](https://docs.microsoft.com/uwp/api/windows.graphics.capture.direct3d11captureframepool)

direct3d11captureframepool.createfreethreaded

#### [<a name="graphicscaptureitem"></a>graphicscaptureitem](https://docs.microsoft.com/uwp/api/windows.graphics.capture.graphicscaptureitem)

graphicscaptureitem.createfromvisual

### [<a name="windowsgraphicsdisplaycore"></a>windows.graphics.display.core](https://docs.microsoft.com/uwp/api/windows.graphics.display.core)

#### [<a name="hdmidisplaymode"></a>hdmidisplaymode](https://docs.microsoft.com/uwp/api/windows.graphics.display.core.hdmidisplaymode)

hdmidisplaymode.isdolbyvisionlowlatencysupported

### [<a name="windowsgraphicsholographic"></a>windows.graphics.holographic](https://docs.microsoft.com/uwp/api/windows.graphics.holographic)

#### [<a name="holographiccamera"></a>holographiccamera](https://docs.microsoft.com/uwp/api/windows.graphics.holographic.holographiccamera)

holographiccamera.ishardwarecontentprotectionenabled <br> holographiccamera.ishardwarecontentprotectionsupported

#### [<a name="holographicquadlayerupdateparameters"></a>holographicquadlayerupdateparameters](https://docs.microsoft.com/uwp/api/windows.graphics.holographic.holographicquadlayerupdateparameters)

holographicquadlayerupdateparameters.acquirebuffertoupdatecontentwithhardwareprotection <br> holographicquadlayerupdateparameters.canacquirewithhardwareprotection

### [<a name="windowsgraphicsimaging"></a>Windows.Graphics.Imaging](https://docs.microsoft.com/uwp/api/windows.graphics.imaging)

#### [<a name="bitmapdecoder"></a>BitmapDecoder](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmapdecoder)

BitmapDecoder.heifdecoderid <br> BitmapDecoder.webpdecoderid

#### [<a name="bitmapencoder"></a>BitmapEncoder](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmapencoder)

BitmapEncoder.heifencoderid

## <a name="windowsmanagement"></a>windows.management

### [<a name="windowsmanagementdeployment"></a>windows.management.deployment](https://docs.microsoft.com/uwp/api/windows.management.deployment)

#### [<a name="packagemanager"></a>packagemanager](https://docs.microsoft.com/uwp/api/windows.management.deployment.packagemanager)

PackageManager.deprovisionpackageforallusersasync

## <a name="windowsmedia"></a>windows.media

### [<a name="windowsmediaaudio"></a>windows.media.audio](https://docs.microsoft.com/uwp/api/windows.media.audio)

#### [<a name="createaudiodeviceinputnoderesult"></a>createaudiodeviceinputnoderesult](https://docs.microsoft.com/uwp/api/windows.media.audio.createaudiodeviceinputnoderesult)

createaudiodeviceinputnoderesult.extendederror

#### [<a name="createaudiodeviceoutputnoderesult"></a>createaudiodeviceoutputnoderesult](https://docs.microsoft.com/uwp/api/windows.media.audio.createaudiodeviceoutputnoderesult)

createaudiodeviceoutputnoderesult.extendederror

#### [<a name="createaudiofileinputnoderesult"></a>createaudiofileinputnoderesult](https://docs.microsoft.com/uwp/api/windows.media.audio.createaudiofileinputnoderesult)

createaudiofileinputnoderesult.extendederror

#### [<a name="createaudiofileoutputnoderesult"></a>createaudiofileoutputnoderesult](https://docs.microsoft.com/uwp/api/windows.media.audio.createaudiofileoutputnoderesult)

createaudiofileoutputnoderesult.extendederror

#### [<a name="createaudiographresult"></a>createaudiographresult](https://docs.microsoft.com/uwp/api/windows.media.audio.createaudiographresult)

createaudiographresult.extendederror

#### [<a name="createmediasourceaudioinputnoderesult"></a>createmediasourceaudioinputnoderesult](https://docs.microsoft.com/uwp/api/windows.media.audio.createmediasourceaudioinputnoderesult)

createmediasourceaudioinputnoderesult.extendederror

#### [<a name="mixedrealityspatialaudioformatpolicy"></a>mixedrealityspatialaudioformatpolicy](https://docs.microsoft.com/uwp/api/windows.media.audio.mixedrealityspatialaudioformatpolicy)

mixedrealityspatialaudioformatpolicy

#### [<a name="setdefaultspatialaudioformatresult"></a>setdefaultspatialaudioformatresult](https://docs.microsoft.com/uwp/api/windows.media.audio.setdefaultspatialaudioformatresult)

setdefaultspatialaudioformatresult <br> setdefaultspatialaudioformatresult.Status

#### [<a name="setdefaultspatialaudioformatstatus"></a>setdefaultspatialaudioformatstatus](https://docs.microsoft.com/uwp/api/windows.media.audio.setdefaultspatialaudioformatstatus)

setdefaultspatialaudioformatstatus

#### [<a name="spatialaudiodeviceconfiguration"></a>spatialaudiodeviceconfiguration](https://docs.microsoft.com/uwp/api/windows.media.audio.spatialaudiodeviceconfiguration)

spatialaudiodeviceconfiguration <br> spatialaudiodeviceconfiguration.activespatialaudioformat <br> spatialaudiodeviceconfiguration.configurationchanged <br> spatialaudiodeviceconfiguration.defaultspatialaudioformat <br> spatialaudiodeviceconfiguration.DeviceID <br> spatialaudiodeviceconfiguration.getfordeviceid <br> spatialaudiodeviceconfiguration.isspatialaudioformatsupported <br> spatialaudiodeviceconfiguration.isspatialaudiosupported <br> spatialaudiodeviceconfiguration.setdefaultspatialaudioformatasync

#### [<a name="spatialaudioformatconfiguration"></a>spatialaudioformatconfiguration](https://docs.microsoft.com/uwp/api/windows.media.audio.spatialaudioformatconfiguration)

spatialaudioformatconfiguration <br> spatialaudioformatconfiguration.getdefault <br> spatialaudioformatconfiguration.mixedrealityexclusivemodepolicy <br> spatialaudioformatconfiguration.reportconfigurationchangedasync <br> spatialaudioformatconfiguration.reportlicensechangedasync

#### [<a name="spatialaudioformatsubtype"></a>spatialaudioformatsubtype](https://docs.microsoft.com/uwp/api/windows.media.audio.spatialaudioformatsubtype)

spatialaudioformatsubtype <br> spatialaudioformatsubtype.dolbyatmosforheadphones <br> spatialaudioformatsubtype.dolbyatmosforhometheater <br> spatialaudioformatsubtype.dolbyatmosforspeakers <br> spatialaudioformatsubtype.dtsheadphonex <br> spatialaudioformatsubtype.dtsxultra <br> spatialaudioformatsubtype.windowssonic

### [<a name="windowsmediacontrol"></a>Windows.Media.Control](https://docs.microsoft.com/uwp/api/windows.media.control)

#### [<a name="currentsessionchangedeventargs"></a>currentsessionchangedeventargs](https://docs.microsoft.com/uwp/api/windows.media.control.currentsessionchangedeventargs)

currentsessionchangedeventargs

#### [<a name="globalsystemmediatransportcontrolssession"></a>globalsystemmediatransportcontrolssession](https://docs.microsoft.com/uwp/api/windows.media.control.globalsystemmediatransportcontrolssession)

globalsystemmediatransportcontrolssession <br> globalsystemmediatransportcontrolssession.getplaybackinfo <br> globalsystemmediatransportcontrolssession.gettimelineproperties <br> globalsystemmediatransportcontrolssession.mediapropertieschanged <br> globalsystemmediatransportcontrolssession.playbackinfochanged <br> globalsystemmediatransportcontrolssession.sourceappusermodelid <br> globalsystemmediatransportcontrolssession.timelinepropertieschanged <br> globalsystemmediatransportcontrolssession.trychangeautorepeatmodeasync <br> globalsystemmediatransportcontrolssession.trychangechanneldownasync <br> globalsystemmediatransportcontrolssession.trychangechannelupasync <br> globalsystemmediatransportcontrolssession.trychangeplaybackpositionasync <br> globalsystemmediatransportcontrolssession.trychangeplaybackrateasync <br> globalsystemmediatransportcontrolssession.trychangeshuffleactiveasync <br> globalsystemmediatransportcontrolssession.tryfastforwardasync <br> globalsystemmediatransportcontrolssession.trygetmediapropertiesasync <br> globalsystemmediatransportcontrolssession.trypauseasync <br> globalsystemmediatransportcontrolssession.tryplayasync <br> globalsystemmediatransportcontrolssession.tryrecordasync <br> globalsystemmediatransportcontrolssession.tryrewindasync <br> globalsystemmediatransportcontrolssession.tryskipnextasync <br> globalsystemmediatransportcontrolssession.tryskippreviousasync <br> globalsystemmediatransportcontrolssession.trystopasync <br> globalsystemmediatransportcontrolssession.trytoggleplaypauseasync

#### [<a name="globalsystemmediatransportcontrolssessionmanager"></a>globalsystemmediatransportcontrolssessionmanager](https://docs.microsoft.com/uwp/api/windows.media.control.globalsystemmediatransportcontrolssessionmanager)

globalsystemmediatransportcontrolssessionmanager <br> globalsystemmediatransportcontrolssessionmanager.currentsessionchanged <br> globalsystemmediatransportcontrolssessionmanager.getcurrentsession <br> globalsystemmediatransportcontrolssessionmanager.getsessions <br> globalsystemmediatransportcontrolssessionmanager.requestasync <br> globalsystemmediatransportcontrolssessionmanager.sessionschanged

#### [<a name="globalsystemmediatransportcontrolssessionmediaproperties"></a>globalsystemmediatransportcontrolssessionmediaproperties](https://docs.microsoft.com/uwp/api/windows.media.control.globalsystemmediatransportcontrolssessionmediaproperties)

globalsystemmediatransportcontrolssessionmediaproperties <br> globalsystemmediatransportcontrolssessionmediaproperties.albumartist <br> globalsystemmediatransportcontrolssessionmediaproperties.albumtitle <br> globalsystemmediatransportcontrolssessionmediaproperties.albumtrackcount <br> globalsystemmediatransportcontrolssessionmediaproperties.Artist <br> globalsystemmediatransportcontrolssessionmediaproperties.genres <br> globalsystemmediatransportcontrolssessionmediaproperties.playbacktype <br> globalsystemmediatransportcontrolssessionmediaproperties.Subtitle <br> globalsystemmediatransportcontrolssessionmediaproperties.Thumbnail <br> globalsystemmediatransportcontrolssessionmediaproperties.Title <br> globalsystemmediatransportcontrolssessionmediaproperties.tracknumber

#### [<a name="globalsystemmediatransportcontrolssessionplaybackcontrols"></a>globalsystemmediatransportcontrolssessionplaybackcontrols](https://docs.microsoft.com/uwp/api/windows.media.control.globalsystemmediatransportcontrolssessionplaybackcontrols)

globalsystemmediatransportcontrolssessionplaybackcontrols <br> globalsystemmediatransportcontrolssessionplaybackcontrols.ischanneldownenabled <br> globalsystemmediatransportcontrolssessionplaybackcontrols.ischannelupenabled <br> globalsystemmediatransportcontrolssessionplaybackcontrols.isfastforwardenabled <br> globalsystemmediatransportcontrolssessionplaybackcontrols.isnextenabled <br> globalsystemmediatransportcontrolssessionplaybackcontrols.ispauseenabled <br> globalsystemmediatransportcontrolssessionplaybackcontrols.isplaybackpositionenabled <br> globalsystemmediatransportcontrolssessionplaybackcontrols.isplaybackrateenabled <br> globalsystemmediatransportcontrolssessionplaybackcontrols.isplayenabled <br> globalsystemmediatransportcontrolssessionplaybackcontrols.isplaypausetoggleenabled <br> globalsystemmediatransportcontrolssessionplaybackcontrols.ispreviousenabled <br> globalsystemmediatransportcontrolssessionplaybackcontrols.isrecordenabled <br> globalsystemmediatransportcontrolssessionplaybackcontrols.isrepeatenabled <br> globalsystemmediatransportcontrolssessionplaybackcontrols.isrewindenabled <br> globalsystemmediatransportcontrolssessionplaybackcontrols.isshuffleenabled <br> globalsystemmediatransportcontrolssessionplaybackcontrols.isstopenabled

#### [<a name="globalsystemmediatransportcontrolssessionplaybackinfo"></a>globalsystemmediatransportcontrolssessionplaybackinfo](https://docs.microsoft.com/uwp/api/windows.media.control.globalsystemmediatransportcontrolssessionplaybackinfo)

globalsystemmediatransportcontrolssessionplaybackinfo <br> globalsystemmediatransportcontrolssessionplaybackinfo.autorepeatmode <br> globalsystemmediatransportcontrolssessionplaybackinfo.Controls <br> globalsystemmediatransportcontrolssessionplaybackinfo.isshuffleactive <br> globalsystemmediatransportcontrolssessionplaybackinfo.playbackrate <br> globalsystemmediatransportcontrolssessionplaybackinfo.playbackstatus <br> globalsystemmediatransportcontrolssessionplaybackinfo.playbacktype

#### [<a name="globalsystemmediatransportcontrolssessionplaybackstatus"></a>globalsystemmediatransportcontrolssessionplaybackstatus](https://docs.microsoft.com/uwp/api/windows.media.control.globalsystemmediatransportcontrolssessionplaybackstatus)

globalsystemmediatransportcontrolssessionplaybackstatus

#### [<a name="globalsystemmediatransportcontrolssessiontimelineproperties"></a>globalsystemmediatransportcontrolssessiontimelineproperties](https://docs.microsoft.com/uwp/api/windows.media.control.globalsystemmediatransportcontrolssessiontimelineproperties)

globalsystemmediatransportcontrolssessiontimelineproperties <br> globalsystemmediatransportcontrolssessiontimelineproperties.EndTime <br> globalsystemmediatransportcontrolssessiontimelineproperties.LastUpdatedTime <br> globalsystemmediatransportcontrolssessiontimelineproperties.maxseektime <br> globalsystemmediatransportcontrolssessiontimelineproperties.minseektime <br> globalsystemmediatransportcontrolssessiontimelineproperties.position <br> globalsystemmediatransportcontrolssessiontimelineproperties.StartTime

#### [<a name="mediapropertieschangedeventargs"></a>mediapropertieschangedeventargs](https://docs.microsoft.com/uwp/api/windows.media.control.mediapropertieschangedeventargs)

mediapropertieschangedeventargs

#### [<a name="playbackinfochangedeventargs"></a>playbackinfochangedeventargs](https://docs.microsoft.com/uwp/api/windows.media.control.playbackinfochangedeventargs)

playbackinfochangedeventargs

#### [<a name="sessionschangedeventargs"></a>sessionschangedeventargs](https://docs.microsoft.com/uwp/api/windows.media.control.sessionschangedeventargs)

sessionschangedeventargs

#### [<a name="timelinepropertieschangedeventargs"></a>timelinepropertieschangedeventargs](https://docs.microsoft.com/uwp/api/windows.media.control.timelinepropertieschangedeventargs)

timelinepropertieschangedeventargs

#### [<a name="windows"></a>windows](https://docs.microsoft.com/uwp/api/windows.media.control.windows)

Windows.Media.Control

### [<a name="windowsmediacore"></a>windows.media.core](https://docs.microsoft.com/uwp/api/windows.media.core)

#### [<a name="mediastreamsample"></a>MediaStreamSample](https://docs.microsoft.com/uwp/api/windows.media.core.mediastreamsample)

MediaStreamSample.createfromdirect3d11surface <br> MediaStreamSample.direct3d11surface

### [<a name="windowsmediadevicescore"></a>Windows.Media.Devices.Core](https://docs.microsoft.com/uwp/api/windows.media.devices.core)

#### [<a name="cameraintrinsics"></a>cameraintrinsics](https://docs.microsoft.com/uwp/api/windows.media.devices.core.cameraintrinsics)

cameraintrinsics.cameraintrinsics

### [<a name="windowsmediaimport"></a>Windows.Media.Import](https://docs.microsoft.com/uwp/api/windows.media.import)

#### [<a name="photoimportitem"></a>photoimportitem](https://docs.microsoft.com/uwp/api/windows.media.import.photoimportitem)

photoimportitem.Path

### [<a name="windowsmediamediaproperties"></a>windows.media.mediaproperties](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties)

#### [<a name="imageencodingproperties"></a>imageencodingproperties](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.imageencodingproperties)

imageencodingproperties.createheif

#### [<a name="mediaencodingsubtypes"></a>mediaencodingsubtypes](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingsubtypes)

mediaencodingsubtypes.heif

### [<a name="windowsmediaprotectionplayready"></a>windows.media.protection.playready](https://docs.microsoft.com/uwp/api/windows.media.protection.playready)

#### [<a name="playreadystatics"></a>playreadystatics](https://docs.microsoft.com/uwp/api/windows.media.protection.playready.playreadystatics)

playreadystatics.hardwaredrmdisabledattime <br> playreadystatics.hardwaredrmdisableduntiltime <br> playreadystatics.resethardwaredrmdisabled

## <a name="windowsnetworking"></a>windows.networking

### [<a name="windowsnetworkingconnectivity"></a>windows.networking.connectivity](https://docs.microsoft.com/uwp/api/windows.networking.connectivity)

#### [<a name="connectionprofile"></a>connectionprofile](https://docs.microsoft.com/uwp/api/windows.networking.connectivity.connectionprofile)

connectionprofile.CanDelete <br> connectionprofile.trydeleteasync

#### [<a name="connectionprofiledeletestatus"></a>connectionprofiledeletestatus](https://docs.microsoft.com/uwp/api/windows.networking.connectivity.connectionprofiledeletestatus)

connectionprofiledeletestatus

## <a name="windowsperception"></a>windows.perception

### [<a name="windowsperceptionspatialpreview"></a>Windows.perception.spatial.Preview](https://docs.microsoft.com/uwp/api/windows.perception.spatial.preview)

#### [<a name="spatialgraphinteroppreview"></a>spatialgraphinteroppreview](https://docs.microsoft.com/uwp/api/windows.perception.spatial.preview.spatialgraphinteroppreview)

spatialgraphinteroppreview <br> spatialgraphinteroppreview.createcoordinatesystemfornode <br> spatialgraphinteroppreview.createcoordinatesystemfornode <br> spatialgraphinteroppreview.createcoordinatesystemfornode <br> spatialgraphinteroppreview.createlocatorfornode

#### [<a name="windows"></a>windows](https://docs.microsoft.com/uwp/api/windows.perception.spatial.preview.windows)

Windows.perception.spatial.Preview

### [<a name="windowsperceptionspatial"></a>windows.perception.spatial](https://docs.microsoft.com/uwp/api/windows.perception.spatial)

#### [<a name="spatialanchorexporter"></a>spatialanchorexporter](https://docs.microsoft.com/uwp/api/windows.perception.spatial.spatialanchorexporter)

spatialanchorexporter <br> spatialanchorexporter.getanchorexportsufficiencyasync <br> spatialanchorexporter.getdefault <br> spatialanchorexporter.requestaccessasync <br> spatialanchorexporter.tryexportanchorasync

#### [<a name="spatialanchorexportpurpose"></a>spatialanchorexportpurpose](https://docs.microsoft.com/uwp/api/windows.perception.spatial.spatialanchorexportpurpose)

spatialanchorexportpurpose

#### [<a name="spatialanchorexportsufficiency"></a>spatialanchorexportsufficiency](https://docs.microsoft.com/uwp/api/windows.perception.spatial.spatialanchorexportsufficiency)

spatialanchorexportsufficiency <br> spatialanchorexportsufficiency.isminimallysufficient <br> spatialanchorexportsufficiency.recommendedsufficiencylevel <br> spatialanchorexportsufficiency.sufficiencylevel

#### [<a name="spatiallocation"></a>spatiallocation](https://docs.microsoft.com/uwp/api/windows.perception.spatial.spatiallocation)

spatiallocation.absoluteangularaccelerationaxisangle <br> spatiallocation.absoluteangularvelocityaxisangle

### [<a name="windowsperception"></a>windows.perception](https://docs.microsoft.com/uwp/api/windows.perception)

#### [<a name="perceptiontimestamp"></a>perceptiontimestamp](https://docs.microsoft.com/uwp/api/windows.perception.perceptiontimestamp)

perceptiontimestamp.systemrelativetargettime

#### [<a name="perceptiontimestamphelper"></a>perceptiontimestamphelper](https://docs.microsoft.com/uwp/api/windows.perception.perceptiontimestamphelper)

perceptiontimestamphelper.fromsystemrelativetargettime

## <a name="windowsservices"></a>windows.services

### [<a name="windowsservicescortana"></a>windows.services.cortana](https://docs.microsoft.com/uwp/api/windows.services.cortana)

#### [<a name="cortanaactionableinsights"></a>cortanaactionableinsights](https://docs.microsoft.com/uwp/api/windows.services.cortana.cortanaactionableinsights)

cortanaactionableinsights <br> cortanaactionableinsights.getdefault <br> cortanaactionableinsights.getforuser <br> cortanaactionableinsights.isavailableasync <br> cortanaactionableinsights.showinsightsasync <br> cortanaactionableinsights.showinsightsasync <br> cortanaactionableinsights.showinsightsforimageasync <br> cortanaactionableinsights.showinsightsforimageasync <br> cortanaactionableinsights.showinsightsfortextasync <br> cortanaactionableinsights.showinsightsfortextasync <br> cortanaactionableinsights.User

#### [<a name="cortanaactionableinsightsoptions"></a>cortanaactionableinsightsoptions](https://docs.microsoft.com/uwp/api/windows.services.cortana.cortanaactionableinsightsoptions)

cortanaactionableinsightsoptions <br> cortanaactionableinsightsoptions.contentsourceweblink <br> cortanaactionableinsightsoptions.cortanaactionableinsightsoptions <br> cortanaactionableinsightsoptions.surroundingtext

### [<a name="windowsservicesstore"></a>windows.services.store](https://docs.microsoft.com/uwp/api/windows.services.store)

#### [<a name="storeapplicense"></a>storeapplicense](https://docs.microsoft.com/uwp/api/windows.services.store.storeapplicense)

storeapplicense.isdisclicense

#### [<a name="storecontext"></a>storecontext](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext)

storecontext.requestrateandreviewappasync <br> storecontext.setinstallorderforassociatedstorequeueitemsasync

#### [<a name="storequeueitem"></a>storequeueitem](https://docs.microsoft.com/uwp/api/windows.services.store.storequeueitem)

storequeueitem.cancelinstallasync <br> storequeueitem.pauseinstallasync <br> storequeueitem.resumeinstallasync

#### [<a name="storerateandreviewresult"></a>storerateandreviewresult](https://docs.microsoft.com/uwp/api/windows.services.store.storerateandreviewresult)

storerateandreviewresult <br> storerateandreviewresult.extendederror <br> storerateandreviewresult.extendedjsondata <br> storerateandreviewresult.Status <br> storerateandreviewresult.wasupdated

#### [<a name="storerateandreviewstatus"></a>storerateandreviewstatus](https://docs.microsoft.com/uwp/api/windows.services.store.storerateandreviewstatus)

storerateandreviewstatus

## <a name="windowsstorage"></a>windows.storage

### [<a name="windowsstorageprovider"></a>windows.storage.provider](https://docs.microsoft.com/uwp/api/windows.storage.provider)

#### [<a name="storageprovidersyncrootinfo"></a>storageprovidersyncrootinfo](https://docs.microsoft.com/uwp/api/windows.storage.provider.storageprovidersyncrootinfo)

storageprovidersyncrootinfo.ProviderID

## <a name="windowssystem"></a>windows.system

### [<a name="windowssystemimplementationholographic"></a>Windows.System.Implementation.Holographic](https://docs.microsoft.com/uwp/api/windows.system.implementation.holographic)

#### [<a name="sysholographicdeploymentprogress"></a>sysholographicdeploymentprogress](https://docs.microsoft.com/uwp/api/windows.system.implementation.holographic.sysholographicdeploymentprogress)

sysholographicdeploymentprogress

#### [<a name="sysholographicdeploymentresult"></a>sysholographicdeploymentresult](https://docs.microsoft.com/uwp/api/windows.system.implementation.holographic.sysholographicdeploymentresult)

sysholographicdeploymentresult <br> sysholographicdeploymentresult.deploymentstate <br> sysholographicdeploymentresult.extendederror

#### [<a name="sysholographicdeploymentstate"></a>sysholographicdeploymentstate](https://docs.microsoft.com/uwp/api/windows.system.implementation.holographic.sysholographicdeploymentstate)

sysholographicdeploymentstate

#### [<a name="sysholographicdisplay"></a>sysholographicdisplay](https://docs.microsoft.com/uwp/api/windows.system.implementation.holographic.sysholographicdisplay)

sysholographicdisplay <br> sysholographicdisplay.DeviceID <br> sysholographicdisplay.Display <br> sysholographicdisplay.experiencemode <br> sysholographicdisplay.leftviewportparameters <br> sysholographicdisplay.outputadapterid <br> sysholographicdisplay.rightviewportparameters

#### [<a name="sysholographicdisplayexperiencemode"></a>sysholographicdisplayexperiencemode](https://docs.microsoft.com/uwp/api/windows.system.implementation.holographic.sysholographicdisplayexperiencemode)

sysholographicdisplayexperiencemode

#### [<a name="sysholographicdisplaywatcher"></a>sysholographicdisplaywatcher](https://docs.microsoft.com/uwp/api/windows.system.implementation.holographic.sysholographicdisplaywatcher)

sysholographicdisplaywatcher <br> sysholographicdisplaywatcher.Added <br> sysholographicdisplaywatcher.enumerationcompleted <br> sysholographicdisplaywatcher.Removed <br> sysholographicdisplaywatcher.Start <br> sysholographicdisplaywatcher.Status <br> sysholographicdisplaywatcher.Stop <br> sysholographicdisplaywatcher.Stopped <br> sysholographicdisplaywatcher.sysholographicdisplaywatcher

#### [<a name="sysholographicdisplaywatcherstatus"></a>sysholographicdisplaywatcherstatus](https://docs.microsoft.com/uwp/api/windows.system.implementation.holographic.sysholographicdisplaywatcherstatus)

sysholographicdisplaywatcherstatus

#### [<a name="sysholographicpreviewmediasource"></a>sysholographicpreviewmediasource](https://docs.microsoft.com/uwp/api/windows.system.implementation.holographic.sysholographicpreviewmediasource)

sysholographicpreviewmediasource <br> sysholographicpreviewmediasource.Create

#### [<a name="sysholographicwindowingenvironment"></a>sysholographicwindowingenvironment](https://docs.microsoft.com/uwp/api/windows.system.implementation.holographic.sysholographicwindowingenvironment)

sysholographicwindowingenvironment <br> sysholographicwindowingenvironment.deployasync <br> sysholographicwindowingenvironment.getdefault <br> sysholographicwindowingenvironment.getdeploymentstateasync <br> sysholographicwindowingenvironment.isdevicesetupcomplete <br> sysholographicwindowingenvironment.islearningexperiencecomplete <br> sysholographicwindowingenvironment.ispreviewactive <br> sysholographicwindowingenvironment.ispreviewactivechanged <br> sysholographicwindowingenvironment.isprotectedcontentpresent <br> sysholographicwindowingenvironment.isprotectedcontentpresentchanged <br> sysholographicwindowingenvironment.isspeechpersonalizationsupported <br> sysholographicwindowingenvironment.setisspeechpersonalizationenabledasync <br> sysholographicwindowingenvironment.startasync <br> sysholographicwindowingenvironment.Status <br> sysholographicwindowingenvironment.StatusChanged <br> sysholographicwindowingenvironment.stopasync

#### [<a name="sysholographicwindowingenvironmentcomponentkind"></a>sysholographicwindowingenvironmentcomponentkind](https://docs.microsoft.com/uwp/api/windows.system.implementation.holographic.sysholographicwindowingenvironmentcomponentkind)

sysholographicwindowingenvironmentcomponentkind

#### [<a name="sysholographicwindowingenvironmentcomponentstate"></a>sysholographicwindowingenvironmentcomponentstate](https://docs.microsoft.com/uwp/api/windows.system.implementation.holographic.sysholographicwindowingenvironmentcomponentstate)

sysholographicwindowingenvironmentcomponentstate

#### [<a name="sysholographicwindowingenvironmentcomponentstatus"></a>sysholographicwindowingenvironmentcomponentstatus](https://docs.microsoft.com/uwp/api/windows.system.implementation.holographic.sysholographicwindowingenvironmentcomponentstatus)

sysholographicwindowingenvironmentcomponentstatus

#### [<a name="sysholographicwindowingenvironmentstate"></a>sysholographicwindowingenvironmentstate](https://docs.microsoft.com/uwp/api/windows.system.implementation.holographic.sysholographicwindowingenvironmentstate)

sysholographicwindowingenvironmentstate

#### [<a name="sysholographicwindowingenvironmentstatus"></a>sysholographicwindowingenvironmentstatus](https://docs.microsoft.com/uwp/api/windows.system.implementation.holographic.sysholographicwindowingenvironmentstatus)

sysholographicwindowingenvironmentstatus <br> sysholographicwindowingenvironmentstatus.componentstatuses <br> sysholographicwindowingenvironmentstatus.State

#### [<a name="sysspatialinputdevice"></a>sysspatialinputdevice](https://docs.microsoft.com/uwp/api/windows.system.implementation.holographic.sysspatialinputdevice)

sysspatialinputdevice <br> sysspatialinputdevice.handedness <br> sysspatialinputdevice.haspositionaltracking <br> sysspatialinputdevice.trygetbatteryreport

#### [<a name="sysspatialinputdevicewatcher"></a>sysspatialinputdevicewatcher](https://docs.microsoft.com/uwp/api/windows.system.implementation.holographic.sysspatialinputdevicewatcher)

sysspatialinputdevicewatcher <br> sysspatialinputdevicewatcher.Added <br> sysspatialinputdevicewatcher.enumerationcompleted <br> sysspatialinputdevicewatcher.Removed <br> sysspatialinputdevicewatcher.Start <br> sysspatialinputdevicewatcher.Status <br> sysspatialinputdevicewatcher.Stop <br> sysspatialinputdevicewatcher.Stopped <br> sysspatialinputdevicewatcher.sysspatialinputdevicewatcher <br> sysspatialinputdevicewatcher.Updated

#### [<a name="sysspatialinputdevicewatcherstatus"></a>sysspatialinputdevicewatcherstatus](https://docs.microsoft.com/uwp/api/windows.system.implementation.holographic.sysspatialinputdevicewatcherstatus)

sysspatialinputdevicewatcherstatus

#### [<a name="sysspatiallocator"></a>sysspatiallocator](https://docs.microsoft.com/uwp/api/windows.system.implementation.holographic.sysspatiallocator)

sysspatiallocator <br> sysspatiallocator.getfloorlocator

#### [<a name="sysspatialstageboundarydisposition"></a>sysspatialstageboundarydisposition](https://docs.microsoft.com/uwp/api/windows.system.implementation.holographic.sysspatialstageboundarydisposition)

sysspatialstageboundarydisposition

#### [<a name="sysspatialstagemanager"></a>sysspatialstagemanager](https://docs.microsoft.com/uwp/api/windows.system.implementation.holographic.sysspatialstagemanager)

sysspatialstagemanager <br> sysspatialstagemanager.doesanystagehaveboundariesasync <br> sysspatialstagemanager.getboundarydisposition <br> sysspatialstagemanager.setandsavenewstageasync <br> sysspatialstagemanager.setboundaryenabled <br> sysspatialstagemanager.sysspatialstagemanager <br> sysspatialstagemanager.updatestageanchorasync

#### [<a name="windows"></a>windows](https://docs.microsoft.com/uwp/api/windows.system.implementation.holographic.windows)

Windows.System.Implementation.Holographic

### [<a name="windowssystempreview"></a>Windows.System.Preview](https://docs.microsoft.com/uwp/api/windows.system.preview)

#### [<a name="hingestate"></a>hingestate](https://docs.microsoft.com/uwp/api/windows.system.preview.hingestate)

hingestate

#### [<a name="twopanelhingeddeviceposturepreview"></a>twopanelhingeddeviceposturepreview](https://docs.microsoft.com/uwp/api/windows.system.preview.twopanelhingeddeviceposturepreview)

twopanelhingeddeviceposturepreview <br> twopanelhingeddeviceposturepreview.getcurrentpostureasync <br> twopanelhingeddeviceposturepreview.getdefaultasync <br> twopanelhingeddeviceposturepreview.posturechanged

#### [<a name="twopanelhingeddeviceposturepreviewreading"></a>twopanelhingeddeviceposturepreviewreading](https://docs.microsoft.com/uwp/api/windows.system.preview.twopanelhingeddeviceposturepreviewreading)

twopanelhingeddeviceposturepreviewreading <br> twopanelhingeddeviceposturepreviewreading.hingestate <br> twopanelhingeddeviceposturepreviewreading.panel1id <br> twopanelhingeddeviceposturepreviewreading.panel1orientation <br> twopanelhingeddeviceposturepreviewreading.panel2id <br> twopanelhingeddeviceposturepreviewreading.panel2orientation <br> twopanelhingeddeviceposturepreviewreading.timestamp

#### [<a name="twopanelhingeddeviceposturepreviewreadingchangedeventargs"></a>twopanelhingeddeviceposturepreviewreadingchangedeventargs](https://docs.microsoft.com/uwp/api/windows.system.preview.twopanelhingeddeviceposturepreviewreadingchangedeventargs)

twopanelhingeddeviceposturepreviewreadingchangedeventargs <br> twopanelhingeddeviceposturepreviewreadingchangedeventargs.Reading

#### [<a name="windows"></a>windows](https://docs.microsoft.com/uwp/api/windows.system.preview.windows)

Windows.System.Preview

### [<a name="windowssystemprofilesystemmanufacturers"></a>windows.system.profile.systemmanufacturers](https://docs.microsoft.com/uwp/api/windows.system.profile.systemmanufacturers)

#### [<a name="systemsupportdeviceinfo"></a>systemsupportdeviceinfo](https://docs.microsoft.com/uwp/api/windows.system.profile.systemmanufacturers.systemsupportdeviceinfo)

systemsupportdeviceinfo <br> systemsupportdeviceinfo.FriendlyName <br> systemsupportdeviceinfo.OperatingSystem <br> systemsupportdeviceinfo.systemfirmwareversion <br> systemsupportdeviceinfo.systemhardwareversion <br> systemsupportdeviceinfo.systemmanufacturer <br> systemsupportdeviceinfo.systemproductname <br> systemsupportdeviceinfo.systemsku

#### [<a name="systemsupportinfo"></a>systemsupportinfo](https://docs.microsoft.com/uwp/api/windows.system.profile.systemmanufacturers.systemsupportinfo)

systemsupportinfo.localdeviceinfo

### [<a name="windowssystemprofile"></a>windows.system.profile](https://docs.microsoft.com/uwp/api/windows.system.profile)

#### [<a name="systemoutofboxexperiencestate"></a>systemoutofboxexperiencestate](https://docs.microsoft.com/uwp/api/windows.system.profile.systemoutofboxexperiencestate)

systemoutofboxexperiencestate

#### [<a name="systemsetupinfo"></a>systemsetupinfo](https://docs.microsoft.com/uwp/api/windows.system.profile.systemsetupinfo)

systemsetupinfo <br> systemsetupinfo.outofboxexperiencestate <br> systemsetupinfo.outofboxexperiencestatechanged

#### [<a name="windowsintegritypolicy"></a>windowsintegritypolicy](https://docs.microsoft.com/uwp/api/windows.system.profile.windowsintegritypolicy)

windowsintegritypolicy <br> windowsintegritypolicy.candisable <br> windowsintegritypolicy.isdisablesupported <br> windowsintegritypolicy.IsEnabled <br> windowsintegritypolicy.isenabledfortrial <br> windowsintegritypolicy.policychanged

### [<a name="windowssystemremotesystems"></a>windows.system.remotesystems](https://docs.microsoft.com/uwp/api/windows.system.remotesystems)

#### [<a name="remotesystem"></a>remotesystem](https://docs.microsoft.com/uwp/api/windows.system.remotesystems.remotesystem)

remotesystem.Apps

#### [<a name="remotesystemapp"></a>remotesystemapp](https://docs.microsoft.com/uwp/api/windows.system.remotesystems.remotesystemapp)

remotesystemapp <br> remotesystemapp.Attributes <br> remotesystemapp.DisplayName <br> remotesystemapp.ID <br> remotesystemapp.isavailablebyproximity <br> remotesystemapp.isavailablebyspatialproximity

#### [<a name="remotesystemappregistration"></a>remotesystemappregistration](https://docs.microsoft.com/uwp/api/windows.system.remotesystems.remotesystemappregistration)

remotesystemappregistration <br> remotesystemappregistration.Attributes <br> remotesystemappregistration.getdefault <br> remotesystemappregistration.getforuser <br> remotesystemappregistration.saveasync <br> remotesystemappregistration.User

#### [<a name="remotesystemconnectioninfo"></a>remotesystemconnectioninfo](https://docs.microsoft.com/uwp/api/windows.system.remotesystems.remotesystemconnectioninfo)

remotesystemconnectioninfo <br> remotesystemconnectioninfo.isproximal <br> remotesystemconnectioninfo.trycreatefromappserviceconnection

#### [<a name="remotesystemconnectionrequest"></a>remotesystemconnectionrequest](https://docs.microsoft.com/uwp/api/windows.system.remotesystems.remotesystemconnectionrequest)

remotesystemconnectionrequest.createforapp <br> remotesystemconnectionrequest.remotesystemapp

#### [<a name="remotesystemwebaccountfilter"></a>remotesystemwebaccountfilter](https://docs.microsoft.com/uwp/api/windows.system.remotesystems.remotesystemwebaccountfilter)

remotesystemwebaccountfilter <br> remotesystemwebaccountfilter.Account <br> remotesystemwebaccountfilter.remotesystemwebaccountfilter

### [<a name="windowssystemupdate"></a>Windows.System.Update](https://docs.microsoft.com/uwp/api/windows.system.update)

#### [<a name="systemupdateattentionrequiredreason"></a>systemupdateattentionrequiredreason](https://docs.microsoft.com/uwp/api/windows.system.update.systemupdateattentionrequiredreason)

systemupdateattentionrequiredreason

#### [<a name="systemupdateitem"></a>systemupdateitem](https://docs.microsoft.com/uwp/api/windows.system.update.systemupdateitem)

systemupdateitem <br> systemupdateitem.Description <br> systemupdateitem.DownloadProgress <br> systemupdateitem.extendederror <br> systemupdateitem.ID <br> systemupdateitem.installprogress <br> systemupdateitem.Revision <br> systemupdateitem.State <br> systemupdateitem.Title

#### [<a name="systemupdateitemstate"></a>systemupdateitemstate](https://docs.microsoft.com/uwp/api/windows.system.update.systemupdateitemstate)

systemupdateitemstate

#### [<a name="systemupdatelasterrorinfo"></a>systemupdatelasterrorinfo](https://docs.microsoft.com/uwp/api/windows.system.update.systemupdatelasterrorinfo)

systemupdatelasterrorinfo <br> systemupdatelasterrorinfo.extendederror <br> systemupdatelasterrorinfo.IsInteractive <br> systemupdatelasterrorinfo.State

#### [<a name="systemupdatemanager"></a>systemupdatemanager](https://docs.microsoft.com/uwp/api/windows.system.update.systemupdatemanager)

systemupdatemanager <br> systemupdatemanager.attentionrequiredreason <br> systemupdatemanager.blockautomaticrebootasync <br> systemupdatemanager.DownloadProgress <br> systemupdatemanager.extendederror <br> systemupdatemanager.getautomaticrebootblockids <br> systemupdatemanager.getflightring <br> systemupdatemanager.getupdateitems <br> systemupdatemanager.installprogress <br> systemupdatemanager.IsSupported <br> systemupdatemanager.lasterrorinfo <br> systemupdatemanager.lastupdatechecktime <br> systemupdatemanager.lastupdateinstalltime <br> systemupdatemanager.reboottocompleteinstall <br> systemupdatemanager.setflightring <br> systemupdatemanager.startcancelupdates <br> systemupdatemanager.startinstall <br> systemupdatemanager.State <br> systemupdatemanager.StateChanged <br> systemupdatemanager.trysetuseractivehours <br> systemupdatemanager.unblockautomaticrebootasync <br> systemupdatemanager.useractivehoursend <br> systemupdatemanager.useractivehoursmax <br> systemupdatemanager.useractivehoursstart

#### [<a name="systemupdatemanagerstate"></a>systemupdatemanagerstate](https://docs.microsoft.com/uwp/api/windows.system.update.systemupdatemanagerstate)

systemupdatemanagerstate

#### [<a name="systemupdatestartinstallaction"></a>systemupdatestartinstallaction](https://docs.microsoft.com/uwp/api/windows.system.update.systemupdatestartinstallaction)

systemupdatestartinstallaction

#### [<a name="windows"></a>windows](https://docs.microsoft.com/uwp/api/windows.system.update.windows)

Windows.System.Update

### [<a name="windowssystemuserprofile"></a>windows.system.userprofile](https://docs.microsoft.com/uwp/api/windows.system.userprofile)

#### [<a name="assignedaccesssettings"></a>assignedaccesssettings](https://docs.microsoft.com/uwp/api/windows.system.userprofile.assignedaccesssettings)

assignedaccesssettings <br> assignedaccesssettings.getdefault <br> assignedaccesssettings.getforuser <br> assignedaccesssettings.IsEnabled <br> assignedaccesssettings.issingleappkioskmode <br> assignedaccesssettings.User

### [<a name="windowssystem"></a>windows.system](https://docs.microsoft.com/uwp/api/windows.system)

#### [<a name="appurihandlerhost"></a>appurihandlerhost](https://docs.microsoft.com/uwp/api/windows.system.appurihandlerhost)

appurihandlerhost <br> appurihandlerhost.appurihandlerhost <br> appurihandlerhost.appurihandlerhost <br> appurihandlerhost.Name

#### [<a name="appurihandlerregistration"></a>appurihandlerregistration](https://docs.microsoft.com/uwp/api/windows.system.appurihandlerregistration)

appurihandlerregistration <br> appurihandlerregistration.getappaddedhostsasync <br> appurihandlerregistration.Name <br> appurihandlerregistration.setappaddedhostsasync <br> appurihandlerregistration.User

#### [<a name="appurihandlerregistrationmanager"></a>appurihandlerregistrationmanager](https://docs.microsoft.com/uwp/api/windows.system.appurihandlerregistrationmanager)

appurihandlerregistrationmanager <br> appurihandlerregistrationmanager.getdefault <br> appurihandlerregistrationmanager.getforuser <br> appurihandlerregistrationmanager.trygetregistration <br> appurihandlerregistrationmanager.User

#### [<a name="launcher"></a>Lanceur](https://docs.microsoft.com/uwp/api/windows.system.launcher)

Launcher.launchfolderpathasync <br> Launcher.launchfolderpathasync <br> Launcher.launchfolderpathforuserasync <br> Launcher.launchfolderpathforuserasync

## <a name="windowsui"></a>windows.ui

### [<a name="windowsuiaccessibility"></a>Windows.UI.Accessibility](https://docs.microsoft.com/uwp/api/windows.ui.accessibility)

#### [<a name="screenreaderpositionchangedeventargs"></a>screenreaderpositionchangedeventargs](https://docs.microsoft.com/uwp/api/windows.ui.accessibility.screenreaderpositionchangedeventargs)

screenreaderpositionchangedeventargs <br> screenreaderpositionchangedeventargs.isreadingtext <br> screenreaderpositionchangedeventargs.screenpositioninrawpixels

#### [<a name="screenreaderservice"></a>screenreaderservice](https://docs.microsoft.com/uwp/api/windows.ui.accessibility.screenreaderservice)

screenreaderservice <br> screenreaderservice.currentscreenreaderposition <br> screenreaderservice.screenreaderpositionchanged <br> screenreaderservice.screenreaderservice

#### [<a name="windows"></a>windows](https://docs.microsoft.com/uwp/api/windows.ui.accessibility.windows)

Windows.UI.Accessibility

### [<a name="windowsuicompositioninteractions"></a>windows.ui.composition.interactions](https://docs.microsoft.com/uwp/api/windows.ui.composition.interactions)

#### [<a name="interactionsourceconfiguration"></a>interactionsourceconfiguration](https://docs.microsoft.com/uwp/api/windows.ui.composition.interactions.interactionsourceconfiguration)

interactionsourceconfiguration <br> interactionsourceconfiguration.positionxsourcemode <br> interactionsourceconfiguration.positionysourcemode <br> interactionsourceconfiguration.scalesourcemode

#### [<a name="interactionsourceredirectionmode"></a>interactionsourceredirectionmode](https://docs.microsoft.com/uwp/api/windows.ui.composition.interactions.interactionsourceredirectionmode)

interactionsourceredirectionmode

#### [<a name="interactiontracker"></a>interactiontracker](https://docs.microsoft.com/uwp/api/windows.ui.composition.interactions.interactiontracker)

interactiontracker.isinertiafromimpulse <br> interactiontracker.tryupdateposition <br> interactiontracker.tryupdatepositionby

#### [<a name="interactiontrackerclampingoption"></a>interactiontrackerclampingoption](https://docs.microsoft.com/uwp/api/windows.ui.composition.interactions.interactiontrackerclampingoption)

interactiontrackerclampingoption

#### [<a name="interactiontrackerinertiastateenteredargs"></a>interactiontrackerinertiastateenteredargs](https://docs.microsoft.com/uwp/api/windows.ui.composition.interactions.interactiontrackerinertiastateenteredargs)

interactiontrackerinertiastateenteredargs.isinertiafromimpulse

#### [<a name="visualinteractionsource"></a>visualinteractionsource](https://docs.microsoft.com/uwp/api/windows.ui.composition.interactions.visualinteractionsource)

visualinteractionsource.pointerwheelconfig

### [<a name="windowsuicomposition"></a>windows.ui.composition](https://docs.microsoft.com/uwp/api/windows.ui.composition)

#### [<a name="animationpropertyaccessmode"></a>animationpropertyaccessmode](https://docs.microsoft.com/uwp/api/windows.ui.composition.animationpropertyaccessmode)

animationpropertyaccessmode

#### [<a name="animationpropertyinfo"></a>animationpropertyinfo](https://docs.microsoft.com/uwp/api/windows.ui.composition.animationpropertyinfo)

animationpropertyinfo <br> animationpropertyinfo.AccessMode

#### [<a name="booleankeyframeanimation"></a>booleankeyframeanimation](https://docs.microsoft.com/uwp/api/windows.ui.composition.booleankeyframeanimation)

booleankeyframeanimation <br> booleankeyframeanimation.insertKeyframe

#### [<a name="compositionanimation"></a>compositionanimation](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositionanimation)

compositionanimation.setexpressionreferenceparameter

#### [<a name="compositiongeometricclip"></a>compositiongeometricclip](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositiongeometricclip)

compositiongeometricclip <br> compositiongeometricclip.Geometry <br> compositiongeometricclip.Viewbox

#### [<a name="compositiongradientbrush"></a>compositiongradientbrush](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositiongradientbrush)

compositiongradientbrush.MappingMode

#### [<a name="compositionmappingmode"></a>compositionmappingmode](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositionmappingmode)

compositionmappingmode

#### [<a name="compositionobject"></a>compositionobject](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositionobject)

compositionobject.populatepropertyinfo <br> compositionobject.startanimationgroupwithianimationobject <br> compositionobject.startanimationwithianimationobject

#### [<a name="compositor"></a>compositor](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositor)

Compositor.createbooleankeyframeanimation <br> Compositor.creategeometricclip <br> Compositor.creategeometricclip <br> Compositor.createredirectvisual <br> Compositor.createredirectvisual

#### [<a name="ianimationobject"></a>ianimationobject](https://docs.microsoft.com/uwp/api/windows.ui.composition.ianimationobject)

ianimationobject <br> ianimationobject.populatepropertyinfo

#### [<a name="redirectvisual"></a>redirectvisual](https://docs.microsoft.com/uwp/api/windows.ui.composition.redirectvisual)

redirectvisual <br> redirectvisual.source

### [<a name="windowsuiinputinkingpreview"></a>Windows.UI.Input.inking.Preview](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.preview)

#### [<a name="palmrejectiondelayzonepreview"></a>palmrejectiondelayzonepreview](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.preview.palmrejectiondelayzonepreview)

palmrejectiondelayzonepreview <br> palmrejectiondelayzonepreview.Close <br> palmrejectiondelayzonepreview.createforvisual <br> palmrejectiondelayzonepreview.createforvisual

#### [<a name="windows"></a>windows](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.preview.windows)

Windows.UI.Input.inking.Preview

### [<a name="windowsuiinputinking"></a>windows.ui.input.inking](https://docs.microsoft.com/uwp/api/windows.ui.input.inking)

#### [<a name="handwritinglineheight"></a>handwritinglineheight](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.handwritinglineheight)

handwritinglineheight

#### [<a name="penandinksettings"></a>penandinksettings](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.penandinksettings)

penandinksettings <br> penandinksettings.fontFamilyName <br> penandinksettings.getdefault <br> penandinksettings.handwritinglineheight <br> penandinksettings.ishandwritingdirectlyintotextfieldenabled <br> penandinksettings.istouchhandwritingenabled <br> penandinksettings.penhandedness <br> penandinksettings.userconsentstohandwritingtelemetrycollection

#### [<a name="penhandedness"></a>penhandedness](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.penhandedness)

penhandedness

### [<a name="windowsuinotifications"></a>windows.ui.notifications](https://docs.microsoft.com/uwp/api/windows.ui.notifications)

#### [<a name="scheduledtoastnotificationshowingeventargs"></a>scheduledtoastnotificationshowingeventargs](https://docs.microsoft.com/uwp/api/windows.ui.notifications.scheduledtoastnotificationshowingeventargs)

scheduledtoastnotificationshowingeventargs <br> scheduledtoastnotificationshowingeventargs.Cancel <br> scheduledtoastnotificationshowingeventargs.getdeferral <br> scheduledtoastnotificationshowingeventargs.scheduledtoastnotification

#### [<a name="toastnotifier"></a>toastnotifier](https://docs.microsoft.com/uwp/api/windows.ui.notifications.toastnotifier)

toastnotifier.scheduledtoastnotificationshowing

### [<a name="windowsuishell"></a>windows.ui.shell](https://docs.microsoft.com/uwp/api/windows.ui.shell)

#### [<a name="securityappkind"></a>securityappkind](https://docs.microsoft.com/uwp/api/windows.ui.shell.securityappkind)

securityappkind

#### [<a name="securityappmanager"></a>securityappmanager](https://docs.microsoft.com/uwp/api/windows.ui.shell.securityappmanager)

securityappmanager <br> securityappmanager.Register <br> securityappmanager.securityappmanager <br> securityappmanager.Unregister <br> securityappmanager.updatestate

#### [<a name="securityappstate"></a>securityappstate](https://docs.microsoft.com/uwp/api/windows.ui.shell.securityappstate)

securityappstate

#### [<a name="securityappsubstatus"></a>securityappsubstatus](https://docs.microsoft.com/uwp/api/windows.ui.shell.securityappsubstatus)

securityappsubstatus

#### [<a name="taskbarmanager"></a>taskbarmanager](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager)

taskbarmanager.issecondarytilepinnedasync <br> taskbarmanager.requestpinsecondarytileasync <br> taskbarmanager.tryunpinsecondarytileasync

### [<a name="windowsuistartscreen"></a>windows.ui.startscreen](https://docs.microsoft.com/uwp/api/windows.ui.startscreen)

#### [<a name="startscreenmanager"></a>startscreenmanager](https://docs.microsoft.com/uwp/api/windows.ui.startscreen.startscreenmanager)

startscreenmanager.containssecondarytileasync <br> startscreenmanager.tryremovesecondarytileasync

### [<a name="windowsuitextcore"></a>Windows.UI.Text.Core](https://docs.microsoft.com/uwp/api/windows.ui.text.core)

#### [<a name="coretextlayoutrequest"></a>coretextlayoutrequest](https://docs.microsoft.com/uwp/api/windows.ui.text.core.coretextlayoutrequest)

coretextlayoutrequest.layoutboundsvisualpixels

### [<a name="windowsuitext"></a>windows.ui.text](https://docs.microsoft.com/uwp/api/windows.ui.text)

#### [<a name="richedittextdocument"></a>richedittextdocument](https://docs.microsoft.com/uwp/api/windows.ui.text.richedittextdocument)

richedittextdocument.clearundoredohistory

### [<a name="windowsuiviewmanagementcore"></a>windows.ui.viewmanagement.core](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.core)

#### [<a name="coreinputview"></a>coreinputview](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.core.coreinputview)

coreinputview.tryhide <br> coreinputview.tryshow <br> coreinputview.tryshow

#### [<a name="coreinputviewkind"></a>coreinputviewkind](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.core.coreinputviewkind)

coreinputviewkind

### [<a name="windowsuiwebui"></a>windows.ui.webui](https://docs.microsoft.com/uwp/api/windows.ui.webui)

#### [<a name="backgroundactivatedeventargs"></a>backgroundactivatedeventargs](https://docs.microsoft.com/uwp/api/windows.ui.webui.backgroundactivatedeventargs)

backgroundactivatedeventargs <br> backgroundactivatedeventargs.taskinstance

#### [<a name="backgroundactivatedeventhandler"></a>backgroundactivatedeventhandler](https://docs.microsoft.com/uwp/api/windows.ui.webui.backgroundactivatedeventhandler)

backgroundactivatedeventhandler

#### [<a name="newwebuiviewcreatedeventargs"></a>newwebuiviewcreatedeventargs](https://docs.microsoft.com/uwp/api/windows.ui.webui.newwebuiviewcreatedeventargs)

newwebuiviewcreatedeventargs <br> newwebuiviewcreatedeventargs.activatedeventargs <br> newwebuiviewcreatedeventargs.getdeferral <br> newwebuiviewcreatedeventargs.haspendingnavigate <br> newwebuiviewcreatedeventargs.webuiview

#### [<a name="webuiapplication"></a>webuiapplication](https://docs.microsoft.com/uwp/api/windows.ui.webui.webuiapplication)

webuiapplication.backgroundactivated <br> webuiapplication.newwebuiviewcreated

#### [<a name="webuiview"></a>webuiview](https://docs.microsoft.com/uwp/api/windows.ui.webui.webuiview)

webuiview <br> webuiview.Activated <br> webuiview.addinitializescript <br> webuiview.applicationviewid <br> webuiview.buildlocalstreamuri <br> webuiview.CanGoBack <br> webuiview.CanGoForward <br> webuiview.capturepreviewtostreamasync <br> webuiview.captureselectedcontenttodatapackageasync <br> webuiview.Closed <br> webuiview.containsfullscreenelement <br> webuiview.containsfullscreenelementchanged <br> webuiview.contentloading <br> webuiview.createasync <br> webuiview.createasync <br> webuiview.defaultbackgroundcolor <br> webuiview.deferredpermissionrequests <br> webuiview.DocumentTitle <br> webuiview.domcontentloaded <br> webuiview.framecontentloading <br> webuiview.framedomcontentloaded <br> webuiview.framenavigationcompleted <br> webuiview.framenavigationstarting <br> webuiview.getdeferredpermissionrequestbyid <br> webuiview.GoBack <br> webuiview.GoForward <br> webuiview.ignoreapplicationcontenturirulesnavigationrestrictions <br> webuiview.invokescriptasync <br> webuiview.longrunningscriptdetected <br> webuiview.Navigate <br> webuiview.navigatetolocalstreamuri <br> webuiview.navigatetostring <br> webuiview.navigatewithhttprequestmessage <br> webuiview.navigationcompleted <br> webuiview.navigationstarting <br> webuiview.newwindowrequested <br> webuiview.permissionrequested <br> webuiview.Refresh <br> webuiview.scriptnotify <br> webuiview.Settings <br> webuiview.source <br> webuiview.Stop <br> webuiview.unsafecontentwarningdisplaying <br> webuiview.unsupportedurischemeidentified <br> webuiview.unviewablecontentidentified <br> webuiview.webresourcerequested

### [<a name="windowsuixamlautomationpeers"></a>windows.ui.xaml.automation.peers](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers)

#### [<a name="appbarbuttonautomationpeer"></a>appbarbuttonautomationpeer](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.appbarbuttonautomationpeer)

appbarbuttonautomationpeer.Collapse <br> appbarbuttonautomationpeer.Expand <br> appbarbuttonautomationpeer.ExpandCollapseState

#### [<a name="automationpeer"></a>automationpeer](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer)

AutomationPeer.isdialog <br> AutomationPeer.isdialogcore

#### [<a name="menubarautomationpeer"></a>menubarautomationpeer](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.menubarautomationpeer)

menubarautomationpeer <br> menubarautomationpeer.menubarautomationpeer

#### [<a name="menubaritemautomationpeer"></a>menubaritemautomationpeer](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.menubaritemautomationpeer)

menubaritemautomationpeer <br> menubaritemautomationpeer.Collapse <br> menubaritemautomationpeer.Expand <br> menubaritemautomationpeer.ExpandCollapseState <br> menubaritemautomationpeer.Invoke <br> menubaritemautomationpeer.menubaritemautomationpeer

### [<a name="windowsuixamlautomation"></a>windows.ui.xaml.automation](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation)

#### [<a name="automationelementidentifiers"></a>automationelementidentifiers](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.automationelementidentifiers)

AutomationElementIdentifiers.isdialogproperty

#### [<a name="automationproperties"></a>automationproperties](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.automationproperties)

AutomationProperties.getisdialog <br> AutomationProperties.isdialogproperty <br> AutomationProperties.setisdialog

### [<a name="windowsuixamlcontrolsmaps"></a>windows.ui.xaml.controls.maps](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps)

#### [<a name="maptileanimationstate"></a>maptileanimationstate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.maptileanimationstate)

maptileanimationstate

#### [<a name="maptilebitmaprequestedeventargs"></a>maptilebitmaprequestedeventargs](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.maptilebitmaprequestedeventargs)

maptilebitmaprequestedeventargs.FrameIndex

#### [<a name="maptilesource"></a>maptilesource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.maptilesource)

maptilesource.animationstate <br> maptilesource.animationstateproperty <br> maptilesource.AutoPlay <br> maptilesource.autoplayproperty <br> maptilesource.FrameCount <br> maptilesource.framecountproperty <br> maptilesource.frameDuration <br> maptilesource.framedurationproperty <br> maptilesource.pause <br> maptilesource.Play <br> maptilesource.Stop

#### [<a name="maptileurirequestedeventargs"></a>maptileurirequestedeventargs](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.maptileurirequestedeventargs)

maptileurirequestedeventargs.FrameIndex

### [<a name="windowsuixamlcontrolsprimitives"></a>windows.ui.xaml.controls.primitives](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives)

#### [<a name="commandbarflyoutcommandbar"></a>commandbarflyoutcommandbar](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.commandbarflyoutcommandbar)

commandbarflyoutcommandbar <br> commandbarflyoutcommandbar.commandbarflyoutcommandbar <br> commandbarflyoutcommandbar.flyouttemplatesettings

#### [<a name="commandbarflyoutcommandbartemplatesettings"></a>commandbarflyoutcommandbartemplatesettings](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.commandbarflyoutcommandbartemplatesettings)

commandbarflyoutcommandbartemplatesettings <br> commandbarflyoutcommandbartemplatesettings.closeanimationendposition <br> commandbarflyoutcommandbartemplatesettings.contentcliprect <br> commandbarflyoutcommandbartemplatesettings.currentwidth <br> commandbarflyoutcommandbartemplatesettings.expanddownanimationendposition <br> commandbarflyoutcommandbartemplatesettings.expanddownanimationholdposition <br> commandbarflyoutcommandbartemplatesettings.expanddownanimationstartposition <br> commandbarflyoutcommandbartemplatesettings.expanddownoverflowverticalposition <br> commandbarflyoutcommandbartemplatesettings.expandedwidth <br> commandbarflyoutcommandbartemplatesettings.expandupanimationendposition <br> commandbarflyoutcommandbartemplatesettings.expandupanimationholdposition <br> commandbarflyoutcommandbartemplatesettings.expandupanimationstartposition <br> commandbarflyoutcommandbartemplatesettings.expandupoverflowverticalposition <br> commandbarflyoutcommandbartemplatesettings.openanimationendposition <br> commandbarflyoutcommandbartemplatesettings.openanimationstartposition <br> commandbarflyoutcommandbartemplatesettings.overflowcontentcliprect <br> commandbarflyoutcommandbartemplatesettings.widthexpansionanimationendposition <br> commandbarflyoutcommandbartemplatesettings.widthexpansionanimationstartposition <br> commandbarflyoutcommandbartemplatesettings.widthexpansiondelta <br> commandbarflyoutcommandbartemplatesettings.widthexpansionmorebuttonanimationendposition <br> commandbarflyoutcommandbartemplatesettings.widthexpansionmorebuttonanimationstartposition

#### [<a name="flyoutbase"></a>flyoutbase](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase)

flyoutbase.areopencloseanimationsenabled <br> flyoutbase.areopencloseanimationsenabledproperty <br> flyoutbase.inputdeviceprefersprimarycommands <br> flyoutbase.inputdeviceprefersprimarycommandsproperty <br> flyoutbase.IsOpen <br> flyoutbase.isopenproperty <br> flyoutbase.showat <br> flyoutbase.showmode <br> flyoutbase.showmodeproperty <br> flyoutbase.TargetProperty

#### [<a name="flyoutshowmode"></a>flyoutshowmode](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.flyoutshowmode)

flyoutshowmode

#### [<a name="flyoutshowoptions"></a>flyoutshowoptions](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.flyoutshowoptions)

flyoutshowoptions <br> flyoutshowoptions.exclusionrect <br> flyoutshowoptions.flyoutshowoptions <br> flyoutshowoptions.placement <br> flyoutshowoptions.position <br> flyoutshowoptions.showmode

#### [<a name="navigationviewitempresenter"></a>navigationviewitempresenter](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.navigationviewitempresenter)

navigationviewitempresenter <br> navigationviewitempresenter.Icon <br> navigationviewitempresenter.iconproperty <br> navigationviewitempresenter.navigationviewitempresenter

### [<a name="windowsuixamlcontrols"></a>windows.ui.xaml.controls](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls)

#### [<a name="anchorrequestedeventargs"></a>anchorrequestedeventargs](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.anchorrequestedeventargs)

anchorrequestedeventargs <br> anchorrequestedeventargs.Anchor <br> anchorrequestedeventargs.anchorcandidates

#### [<a name="appbarelementcontainer"></a>appbarelementcontainer](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.appbarelementcontainer)

appbarelementcontainer <br> appbarelementcontainer.appbarelementcontainer <br> appbarelementcontainer.dynamicoverfloworder <br> appbarelementcontainer.dynamicoverfloworderproperty <br> appbarelementcontainer.iscompact <br> appbarelementcontainer.iscompactproperty <br> appbarelementcontainer.isinoverflow <br> appbarelementcontainer.isinoverflowproperty

#### [<a name="autosuggestbox"></a>autosuggestbox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestbox)

autosuggestbox.Description <br> autosuggestbox.descriptionproperty

#### [<a name="backgroundsizing"></a>backgroundsizing](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.backgroundsizing)

backgroundsizing

#### [<a name="border"></a>bordure](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.border)

Border.backgroundsizing <br> Border.backgroundsizingproperty <br> Border.backgroundtransition

#### [<a name="calendardatepicker"></a>calendardatepicker](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.calendardatepicker)

calendardatepicker.Description <br> calendardatepicker.descriptionproperty

#### [<a name="combobox"></a>combobox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.combobox)

ComboBox.Description <br> ComboBox.descriptionproperty <br> ComboBox.iseditableproperty <br> ComboBox.Text <br> ComboBox.TextBoxStyle <br> ComboBox.textboxstyleproperty <br> ComboBox.TextProperty <br> ComboBox.textsubmitted

#### [<a name="comboboxtextsubmittedeventargs"></a>comboboxtextsubmittedeventargs](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.comboboxtextsubmittedeventargs)

comboboxtextsubmittedeventargs <br> comboboxtextsubmittedeventargs.Handled <br> comboboxtextsubmittedeventargs.Text

#### [<a name="commandbarflyout"></a>commandbarflyout](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.commandbarflyout)

commandbarflyout <br> commandbarflyout.commandbarflyout <br> commandbarflyout.primarycommands <br> commandbarflyout.secondarycommands

#### [<a name="contentpresenter"></a>ContentPresenter](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.contentpresenter)

ContentPresenter.backgroundsizing <br> ContentPresenter.backgroundsizingproperty <br> ContentPresenter.backgroundtransition

#### [<a name="control"></a>control](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control)

Control.backgroundsizing <br> Control.backgroundsizingproperty <br> Control.CornerRadius <br> Control.cornerradiusproperty

#### [<a name="datatemplateselector"></a>DataTemplateSelector](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.datatemplateselector)

DataTemplateSelector.GetElement <br> DataTemplateSelector.recycleelement

#### [<a name="datepicker"></a>DatePicker](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.datepicker)

DatePicker.SelectedDate <br> DatePicker.selecteddatechanged <br> DatePicker.selecteddateproperty

#### [<a name="datepickerselectedvaluechangedeventargs"></a>datepickerselectedvaluechangedeventargs](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.datepickerselectedvaluechangedeventargs)

datepickerselectedvaluechangedeventargs <br> datepickerselectedvaluechangedeventargs.NewDate <br> datepickerselectedvaluechangedeventargs.oldDate

#### [<a name="dropdownbutton"></a>DropDownButton](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.dropdownbutton)

DropDownButton <br> DropDownButton.DropDownButton

#### [<a name="dropdownbuttonautomationpeer"></a>dropdownbuttonautomationpeer](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.dropdownbuttonautomationpeer)

dropdownbuttonautomationpeer <br> dropdownbuttonautomationpeer.Collapse <br> dropdownbuttonautomationpeer.dropdownbuttonautomationpeer <br> dropdownbuttonautomationpeer.Expand <br> dropdownbuttonautomationpeer.ExpandCollapseState

#### [<a name="frame"></a>image](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.frame)

frame.isnavigationstackenabled <br> frame.isnavigationstackenabledproperty <br> frame.navigatetotype

#### [<a name="grid"></a>grid](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.grid)

Grid.backgroundsizing <br> Grid.backgroundsizingproperty

#### [<a name="iconsourceelement"></a>iconsourceelement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.iconsourceelement)

iconsourceelement <br> iconsourceelement.iconsource <br> iconsourceelement.iconsourceelement <br> iconsourceelement.iconsourceproperty

#### [<a name="iscrollanchorprovider"></a>iscrollanchorprovider](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.iscrollanchorprovider)

iscrollanchorprovider <br> iscrollanchorprovider.currentanchor <br> iscrollanchorprovider.registeranchorcandidate <br> iscrollanchorprovider.unregisteranchorcandidate

#### [<a name="menubar"></a>barre de menus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.menubar)

barre de menus <br> MenuBar.Items <br> MenuBar.itemsproperty <br> MenuBar.MenuBar

#### [<a name="menubaritem"></a>menubaritem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.menubaritem)

menubaritem <br> menubaritem.Items <br> menubaritem.itemsproperty <br> menubaritem.menubaritem <br> menubaritem.Title <br> menubaritem.titleproperty

#### [<a name="menubaritemflyout"></a>menubaritemflyout](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.menubaritemflyout)

menubaritemflyout <br> menubaritemflyout.menubaritemflyout

#### [<a name="navigationview"></a>navigationview](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview)

navigationview.contentoverlay <br> navigationview.contentoverlayproperty <br> navigationview.IsPaneVisible <br> navigationview.ispanevisibleproperty <br> navigationview.overflowlabelmode <br> navigationview.overflowlabelmodeproperty <br> navigationview.panecustomcontent <br> navigationview.panecustomcontentproperty <br> navigationview.panedisplaymode <br> navigationview.panedisplaymodeproperty <br> navigationview.paneheader <br> navigationview.paneheaderproperty <br> navigationview.selectionfollowsfocus <br> navigationview.selectionfollowsfocusproperty <br> navigationview.shouldernavigationenabled <br> navigationview.shouldernavigationenabledproperty <br> navigationview.templatesettings <br> navigationview.templatesettingsproperty

#### [<a name="navigationviewitem"></a>navigationviewitem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationviewitem)

navigationviewitem.selectsoninvoked <br> navigationviewitem.selectsoninvokedproperty

#### [<a name="navigationviewiteminvokedeventargs"></a>navigationviewiteminvokedeventargs](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationviewiteminvokedeventargs)

navigationviewiteminvokedeventargs.invokeditemcontainer <br> navigationviewiteminvokedeventargs.recommendednavigationtransitioninfo

#### [<a name="navigationviewoverflowlabelmode"></a>navigationviewoverflowlabelmode](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationviewoverflowlabelmode)

navigationviewoverflowlabelmode

#### [<a name="navigationviewpanedisplaymode"></a>navigationviewpanedisplaymode](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationviewpanedisplaymode)

navigationviewpanedisplaymode

#### [<a name="navigationviewselectionchangedeventargs"></a>navigationviewselectionchangedeventargs](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationviewselectionchangedeventargs)

navigationviewselectionchangedeventargs.recommendednavigationtransitioninfo <br> navigationviewselectionchangedeventargs.selecteditemcontainer

#### [<a name="navigationviewselectionfollowsfocus"></a>navigationviewselectionfollowsfocus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationviewselectionfollowsfocus)

navigationviewselectionfollowsfocus

#### [<a name="navigationviewshouldernavigationenabled"></a>navigationviewshouldernavigationenabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationviewshouldernavigationenabled)

navigationviewshouldernavigationenabled

#### [<a name="navigationviewtemplatesettings"></a>navigationviewtemplatesettings](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationviewtemplatesettings)

navigationviewtemplatesettings <br> navigationviewtemplatesettings.backbuttonvisibility <br> navigationviewtemplatesettings.backbuttonvisibilityproperty <br> navigationviewtemplatesettings.leftpanevisibility <br> navigationviewtemplatesettings.leftpanevisibilityproperty <br> navigationviewtemplatesettings.navigationviewtemplatesettings <br> navigationviewtemplatesettings.overflowbuttonvisibility <br> navigationviewtemplatesettings.overflowbuttonvisibilityproperty <br> navigationviewtemplatesettings.panetogglebuttonvisibility <br> navigationviewtemplatesettings.panetogglebuttonvisibilityproperty <br> navigationviewtemplatesettings.singleselectionfollowsfocus <br> navigationviewtemplatesettings.singleselectionfollowsfocusproperty <br> navigationviewtemplatesettings.TopPadding <br> navigationviewtemplatesettings.toppaddingproperty <br> navigationviewtemplatesettings.toppanevisibility <br> navigationviewtemplatesettings.toppanevisibilityproperty

#### [<a name="panel"></a>Panneau de configuration](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.panel)

Panel.backgroundtransition

#### [<a name="passwordbox"></a>passwordbox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.passwordbox)

PasswordBox.canpasteclipboardcontent <br> PasswordBox.canpasteclipboardcontentproperty <br> PasswordBox.Description <br> PasswordBox.descriptionproperty <br> PasswordBox.pastefromclipboard <br> PasswordBox.selectionflyout <br> PasswordBox.selectionflyoutproperty

#### [<a name="relativepanel"></a>relativepanel](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.relativepanel)

relativepanel.backgroundsizing <br> relativepanel.backgroundsizingproperty

#### [<a name="richeditbox"></a>richeditbox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.richeditbox)

richeditbox.Description <br> richeditbox.descriptionproperty <br> richeditbox.proofingmenuflyout <br> richeditbox.proofingmenuflyoutproperty <br> richeditbox.SelectionChanging <br> richeditbox.selectionflyout <br> richeditbox.selectionflyoutproperty <br> richeditbox.TextDocument

#### [<a name="richeditboxselectionchangingeventargs"></a>richeditboxselectionchangingeventargs](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.richeditboxselectionchangingeventargs)

richeditboxselectionchangingeventargs <br> richeditboxselectionchangingeventargs.Cancel <br> richeditboxselectionchangingeventargs.SelectionLength <br> richeditboxselectionchangingeventargs.SelectionStart

#### [<a name="richtextblock"></a>richtextblock](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.richtextblock)

richtextblock.copyselectiontoclipboard <br> richtextblock.selectionflyout <br> richtextblock.selectionflyoutproperty

#### [<a name="scrollcontentpresenter"></a>ScrollContentPresenter](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.scrollcontentpresenter)

ScrollContentPresenter.cancontentrenderoutsidebounds <br> ScrollContentPresenter.cancontentrenderoutsideboundsproperty <br> ScrollContentPresenter.sizescontenttotemplatedparent <br> ScrollContentPresenter.sizescontenttotemplatedparentproperty

#### [<a name="scrollviewer"></a>ScrollViewer](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.scrollviewer)

ScrollViewer.anchorrequested <br> ScrollViewer.cancontentrenderoutsidebounds <br> ScrollViewer.cancontentrenderoutsideboundsproperty <br> ScrollViewer.currentanchor <br> ScrollViewer.getcancontentrenderoutsidebounds <br> ScrollViewer.horizontalanchorratio <br> ScrollViewer.horizontalanchorratioproperty <br> ScrollViewer.reduceviewportforcoreinputviewocclusions <br> ScrollViewer.reduceviewportforcoreinputviewocclusionsproperty <br> ScrollViewer.registeranchorcandidate <br> ScrollViewer.setcancontentrenderoutsidebounds <br> ScrollViewer.unregisteranchorcandidate <br> ScrollViewer.verticalanchorratio <br> ScrollViewer.verticalanchorratioproperty

#### [<a name="splitbutton"></a>bouton partagé](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.splitbutton)

bouton partagé <br> SplitButton.Click <br> SplitButton.Command <br> SplitButton.CommandParameter <br> SplitButton.commandparameterproperty <br> SplitButton.commandproperty <br> SplitButton.flyout <br> SplitButton.flyoutproperty <br> SplitButton.SplitButton

#### [<a name="splitbuttonautomationpeer"></a>splitbuttonautomationpeer](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.splitbuttonautomationpeer)

splitbuttonautomationpeer <br> splitbuttonautomationpeer.Collapse <br> splitbuttonautomationpeer.Expand <br> splitbuttonautomationpeer.ExpandCollapseState <br> splitbuttonautomationpeer.Invoke <br> splitbuttonautomationpeer.splitbuttonautomationpeer

#### [<a name="splitbuttonclickeventargs"></a>splitbuttonclickeventargs](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.splitbuttonclickeventargs)

splitbuttonclickeventargs

#### [<a name="stackpanel"></a>stackpanel](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.stackpanel)

StackPanel.backgroundsizing <br> StackPanel.backgroundsizingproperty

#### [<a name="textblock"></a>textblock](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textblock)

TextBlock.copyselectiontoclipboard <br> TextBlock.selectionflyout <br> TextBlock.selectionflyoutproperty

#### [<a name="textbox"></a>textbox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox)

TextBox.canpasteclipboardcontent <br> TextBox.canpasteclipboardcontentproperty <br> TextBox.CanRedo <br> TextBox.canredoproperty <br> TextBox.CanUndo <br> TextBox.canundoproperty <br> TextBox.clearundoredohistory <br> TextBox.copyselectiontoclipboard <br> TextBox.cutselectiontoclipboard <br> TextBox.Description <br> TextBox.descriptionproperty <br> TextBox.pastefromclipboard <br> TextBox.proofingmenuflyout <br> TextBox.proofingmenuflyoutproperty <br> TextBox.Redo <br> TextBox.SelectionChanging <br> TextBox.selectionflyout <br> TextBox.selectionflyoutproperty <br> TextBox.Undo

#### [<a name="textboxselectionchangingeventargs"></a>textboxselectionchangingeventargs](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textboxselectionchangingeventargs)

textboxselectionchangingeventargs <br> textboxselectionchangingeventargs.Cancel <br> textboxselectionchangingeventargs.SelectionLength <br> textboxselectionchangingeventargs.SelectionStart

#### [<a name="textcommandbarflyout"></a>textcommandbarflyout](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textcommandbarflyout)

textcommandbarflyout <br> textcommandbarflyout.textcommandbarflyout

#### [<a name="timepicker"></a>TimePicker](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.timepicker)

TimePicker.selectedtime <br> TimePicker.selectedtimechanged <br> TimePicker.selectedtimeproperty

#### [<a name="timepickerselectedvaluechangedeventargs"></a>timepickerselectedvaluechangedeventargs](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.timepickerselectedvaluechangedeventargs)

timepickerselectedvaluechangedeventargs <br> timepickerselectedvaluechangedeventargs.newtime <br> timepickerselectedvaluechangedeventargs.oldtime

#### [<a name="togglesplitbutton"></a>togglesplitbutton](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.togglesplitbutton)

togglesplitbutton <br> togglesplitbutton.IsChecked <br> togglesplitbutton.ischeckedchanged <br> togglesplitbutton.togglesplitbutton

#### [<a name="togglesplitbuttonautomationpeer"></a>togglesplitbuttonautomationpeer](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.togglesplitbuttonautomationpeer)

togglesplitbuttonautomationpeer <br> togglesplitbuttonautomationpeer.Collapse <br> togglesplitbuttonautomationpeer.Expand <br> togglesplitbuttonautomationpeer.ExpandCollapseState <br> togglesplitbuttonautomationpeer.Toggle <br> togglesplitbuttonautomationpeer.togglesplitbuttonautomationpeer <br> togglesplitbuttonautomationpeer.ToggleState

#### [<a name="togglesplitbuttonischeckedchangedeventargs"></a>togglesplitbuttonischeckedchangedeventargs](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.togglesplitbuttonischeckedchangedeventargs)

togglesplitbuttonischeckedchangedeventargs

#### [<a name="tooltip"></a>info-bulle](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.tooltip)

ToolTip.placementrect <br> ToolTip.placementrectproperty

#### [<a name="treeview"></a>treeview](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.treeview)

TreeView.candragitems <br> TreeView.candragitemsproperty <br> TreeView.canreorderitems <br> TreeView.canreorderitemsproperty <br> TreeView.ContainerFromItem <br> TreeView.containerfromnode <br> TreeView.dragitemscompleted <br> TreeView.dragitemsstarting <br> TreeView.ItemContainerStyle <br> TreeView.itemcontainerstyleproperty <br> TreeView.ItemContainerStyleSelector <br> TreeView.itemcontainerstyleselectorproperty <br> TreeView.itemcontainertransitions <br> TreeView.itemcontainertransitionsproperty <br> TreeView.ItemFromContainer <br> TreeView.ItemsSource <br> TreeView.itemssourceproperty <br> TreeView.ItemTemplate <br> TreeView.itemtemplateproperty <br> TreeView.ItemTemplateSelector <br> TreeView.itemtemplateselectorproperty <br> TreeView.nodefromcontainer

#### [<a name="treeviewcollapsedeventargs"></a>treeviewcollapsedeventargs](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.treeviewcollapsedeventargs)

treeviewcollapsedeventargs.Item

#### [<a name="treeviewdragitemscompletedeventargs"></a>treeviewdragitemscompletedeventargs](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.treeviewdragitemscompletedeventargs)

treeviewdragitemscompletedeventargs <br> treeviewdragitemscompletedeventargs.dropresult <br> treeviewdragitemscompletedeventargs.Items

#### [<a name="treeviewdragitemsstartingeventargs"></a>treeviewdragitemsstartingeventargs](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.treeviewdragitemsstartingeventargs)

treeviewdragitemsstartingeventargs <br> treeviewdragitemsstartingeventargs.Cancel <br> treeviewdragitemsstartingeventargs.Data <br> treeviewdragitemsstartingeventargs.Items

#### [<a name="treeviewexpandingeventargs"></a>treeviewexpandingeventargs](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.treeviewexpandingeventargs)

treeviewexpandingeventargs.Item

#### [<a name="treeviewitem"></a>treeviewitem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.treeviewitem)

TreeViewItem.hasunrealizedchildren <br> TreeViewItem.hasunrealizedchildrenproperty <br> TreeViewItem.ItemsSource <br> TreeViewItem.itemssourceproperty

#### [<a name="webview"></a>webview](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview)

WebView.webresourcerequested

#### [<a name="webviewwebresourcerequestedeventargs"></a>webviewwebresourcerequestedeventargs](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webviewwebresourcerequestedeventargs)

webviewwebresourcerequestedeventargs <br> webviewwebresourcerequestedeventargs.getdeferral <br> webviewwebresourcerequestedeventargs.Request <br> webviewwebresourcerequestedeventargs.Response

### [<a name="windowsuixamlcoredirect"></a>Windows.UI.Xaml.Core.direct](https://docs.microsoft.com/uwp/api/windows.ui.xaml.core.direct)

#### [<a name="ixamldirectobject"></a>ixamldirectobject](https://docs.microsoft.com/uwp/api/windows.ui.xaml.core.direct.ixamldirectobject)

ixamldirectobject

#### [<a name="windows"></a>windows](https://docs.microsoft.com/uwp/api/windows.ui.xaml.core.direct.windows)

Windows.UI.Xaml.Core.direct

#### [<a name="xamldirect"></a>xamldirect](https://docs.microsoft.com/uwp/api/windows.ui.xaml.core.direct.xamldirect)

xamldirect <br> xamldirect.AddEventHandler <br> xamldirect.AddEventHandler <br> xamldirect.AddToCollection <br> xamldirect.clearcollection <br> xamldirect.clearproperty <br> xamldirect.CreateInstance <br> xamldirect.GetBooleanProperty <br> xamldirect.getcollectioncount <br> xamldirect.getcolorproperty <br> xamldirect.getcornerradiusproperty <br> xamldirect.getdatetimeproperty <br> xamldirect.getdefault <br> xamldirect.GetDoubleProperty <br> xamldirect.getdurationproperty <br> xamldirect.getenumproperty <br> xamldirect.getgridlengthproperty <br> xamldirect.getint32property <br> xamldirect.getmatrix3dproperty <br> xamldirect.getmatrixproperty <br> xamldirect.GetObject <br> xamldirect.getobjectproperty <br> xamldirect.getpointproperty <br> xamldirect.getrectproperty <br> xamldirect.getsizeproperty <br> xamldirect.getStringProperty <br> xamldirect.getthicknessproperty <br> xamldirect.gettimespanproperty <br> xamldirect.getxamldirectobject <br> xamldirect.getxamldirectobjectfromcollectionat <br> xamldirect.getxamldirectobjectproperty <br> xamldirect.insertintocollectionat <br> xamldirect.RemoveEventHandler <br> xamldirect.removefromcollection <br> xamldirect.removefromcollectionat <br> xamldirect.SetBooleanProperty <br> xamldirect.setcolorproperty <br> xamldirect.setcornerradiusproperty <br> xamldirect.setdatetimeproperty <br> xamldirect.SetDoubleProperty <br> xamldirect.setdurationproperty <br> xamldirect.setenumproperty <br> xamldirect.setgridlengthproperty <br> xamldirect.setint32property <br> xamldirect.setmatrix3dproperty <br> xamldirect.setmatrixproperty <br> xamldirect.SetObjectProperty <br> xamldirect.setpointproperty <br> xamldirect.setrectproperty <br> xamldirect.setsizeproperty <br> xamldirect.SetStringProperty <br> xamldirect.setthicknessproperty <br> xamldirect.settimespanproperty <br> xamldirect.setxamldirectobjectproperty

#### [<a name="xamleventindex"></a>xamleventindex](https://docs.microsoft.com/uwp/api/windows.ui.xaml.core.direct.xamleventindex)

xamleventindex

#### [<a name="xamlpropertyindex"></a>xamlpropertyindex](https://docs.microsoft.com/uwp/api/windows.ui.xaml.core.direct.xamlpropertyindex)

xamlpropertyindex

#### [<a name="xamltypeindex"></a>xamltypeindex](https://docs.microsoft.com/uwp/api/windows.ui.xaml.core.direct.xamltypeindex)

xamltypeindex

### [<a name="windowsuixamlhosting"></a>windows.ui.xaml.hosting](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting)

#### [<a name="desktopwindowxamlsource"></a>desktopwindowxamlsource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource)

desktopwindowxamlsource <br> desktopwindowxamlsource.Close <br> desktopwindowxamlsource.Content <br> desktopwindowxamlsource.desktopwindowxamlsource <br> desktopwindowxamlsource.GotFocus <br> desktopwindowxamlsource.HasFocus <br> desktopwindowxamlsource.navigatefocus <br> desktopwindowxamlsource.takefocusrequested

#### [<a name="desktopwindowxamlsourcegotfocuseventargs"></a>desktopwindowxamlsourcegotfocuseventargs](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsourcegotfocuseventargs)

desktopwindowxamlsourcegotfocuseventargs <br> desktopwindowxamlsourcegotfocuseventargs.Request

#### [<a name="desktopwindowxamlsourcetakefocusrequestedeventargs"></a>desktopwindowxamlsourcetakefocusrequestedeventargs](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsourcetakefocusrequestedeventargs)

desktopwindowxamlsourcetakefocusrequestedeventargs <br> desktopwindowxamlsourcetakefocusrequestedeventargs.Request

#### [<a name="windowsxamlmanager"></a>windowsxamlmanager](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager)

windowsxamlmanager <br> windowsxamlmanager.Close <br> windowsxamlmanager.initializeforcurrentthread

#### [<a name="xamlsourcefocusnavigationreason"></a>xamlsourcefocusnavigationreason](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.xamlsourcefocusnavigationreason)

xamlsourcefocusnavigationreason

#### [<a name="xamlsourcefocusnavigationrequest"></a>xamlsourcefocusnavigationrequest](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.xamlsourcefocusnavigationrequest)

xamlsourcefocusnavigationrequest <br> xamlsourcefocusnavigationrequest.CorrelationId <br> xamlsourcefocusnavigationrequest.hintrect <br> xamlsourcefocusnavigationrequest.Reason <br> xamlsourcefocusnavigationrequest.xamlsourcefocusnavigationrequest <br> xamlsourcefocusnavigationrequest.xamlsourcefocusnavigationrequest <br> xamlsourcefocusnavigationrequest.xamlsourcefocusnavigationrequest

#### [<a name="xamlsourcefocusnavigationresult"></a>xamlsourcefocusnavigationresult](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.xamlsourcefocusnavigationresult)

xamlsourcefocusnavigationresult <br> xamlsourcefocusnavigationresult.wasfocusmoved <br> xamlsourcefocusnavigationresult.xamlsourcefocusnavigationresult

### [<a name="windowsuixamlinput"></a>windows.ui.xaml.input](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input)

#### [<a name="canexecuterequestedeventargs"></a>canexecuterequestedeventargs](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.canexecuterequestedeventargs)

canexecuterequestedeventargs <br> canexecuterequestedeventargs.CanExecute <br> canexecuterequestedeventargs.Parameter

#### [<a name="executerequestedeventargs"></a>executerequestedeventargs](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.executerequestedeventargs)

executerequestedeventargs <br> executerequestedeventargs.Parameter

#### [<a name="focusmanager"></a>focusmanager](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.focusmanager)

FocusManager.gettingfocus <br> FocusManager.GotFocus <br> FocusManager.LosingFocus <br> FocusManager.LostFocus

#### [<a name="focusmanagergotfocuseventargs"></a>focusmanagergotfocuseventargs](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.focusmanagergotfocuseventargs)

focusmanagergotfocuseventargs <br> focusmanagergotfocuseventargs.CorrelationId <br> focusmanagergotfocuseventargs.newfocusedelement

#### [<a name="focusmanagerlostfocuseventargs"></a>focusmanagerlostfocuseventargs](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.focusmanagerlostfocuseventargs)

focusmanagerlostfocuseventargs <br> focusmanagerlostfocuseventargs.CorrelationId <br> focusmanagerlostfocuseventargs.oldfocusedelement

#### [<a name="gettingfocuseventargs"></a>gettingfocuseventargs](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.gettingfocuseventargs)

gettingfocuseventargs.CorrelationId

#### [<a name="losingfocuseventargs"></a>losingfocuseventargs](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.losingfocuseventargs)

losingfocuseventargs.CorrelationId

#### [<a name="standarduicommand"></a>standarduicommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand)

standarduicommand <br> standarduicommand.Kind <br> standarduicommand.kindproperty <br> standarduicommand.standarduicommand <br> standarduicommand.standarduicommand

#### [<a name="standarduicommandkind"></a>standarduicommandkind](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommandkind)

standarduicommandkind

#### [<a name="xamluicommand"></a>xamluicommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xamluicommand)

xamluicommand <br> xamluicommand.AccessKey <br> xamluicommand.AccessKeyProperty <br> xamluicommand.CanExecute <br> xamluicommand.CanExecuteChanged <br> xamluicommand.canexecuterequested <br> xamluicommand.Command <br> xamluicommand.commandproperty <br> xamluicommand.Description <br> xamluicommand.descriptionproperty <br> xamluicommand.Execute <br> xamluicommand.executerequested <br> xamluicommand.iconsource <br> xamluicommand.iconsourceproperty <br> xamluicommand.keyboardaccelerators <br> xamluicommand.keyboardacceleratorsproperty <br> xamluicommand.Label <br> xamluicommand.labelproperty <br> xamluicommand.notifycanexecutechanged <br> xamluicommand.xamluicommand

### [<a name="windowsuixamlmarkup"></a>windows.ui.xaml.markup](https://docs.microsoft.com/uwp/api/windows.ui.xaml.markup)

#### [<a name="fullxamlmetadataproviderattribute"></a>fullxamlmetadataproviderattribute](https://docs.microsoft.com/uwp/api/windows.ui.xaml.markup.fullxamlmetadataproviderattribute)

fullxamlmetadataproviderattribute <br> fullxamlmetadataproviderattribute.fullxamlmetadataproviderattribute

#### [<a name="ixamlbindscopediagnostics"></a>ixamlbindscopediagnostics](https://docs.microsoft.com/uwp/api/windows.ui.xaml.markup.ixamlbindscopediagnostics)

ixamlbindscopediagnostics <br> ixamlbindscopediagnostics.Disable

#### [<a name="ixamltype2"></a>ixamltype2](https://docs.microsoft.com/uwp/api/windows.ui.xaml.markup.ixamltype2)

ixamltype2 <br> ixamltype2.boxedtype

### [<a name="windowsuixamlmediaanimation"></a>windows.ui.xaml.media.animation](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation)

#### [<a name="basicconnectedanimationconfiguration"></a>basicconnectedanimationconfiguration](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.basicconnectedanimationconfiguration)

basicconnectedanimationconfiguration <br> basicconnectedanimationconfiguration.basicconnectedanimationconfiguration

#### [<a name="connectedanimation"></a>connectedanimation](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.connectedanimation)

connectedanimation.Configuration

#### [<a name="connectedanimationconfiguration"></a>connectedanimationconfiguration](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.connectedanimationconfiguration)

connectedanimationconfiguration

#### [<a name="directconnectedanimationconfiguration"></a>directconnectedanimationconfiguration](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.directconnectedanimationconfiguration)

directconnectedanimationconfiguration <br> directconnectedanimationconfiguration.directconnectedanimationconfiguration

#### [<a name="gravityconnectedanimationconfiguration"></a>gravityconnectedanimationconfiguration](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.gravityconnectedanimationconfiguration)

gravityconnectedanimationconfiguration <br> gravityconnectedanimationconfiguration.gravityconnectedanimationconfiguration

#### [<a name="slidenavigationtransitioneffect"></a>slidenavigationtransitioneffect](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.slidenavigationtransitioneffect)

slidenavigationtransitioneffect

#### [<a name="slidenavigationtransitioninfo"></a>slidenavigationtransitioninfo](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.slidenavigationtransitioninfo)

slidenavigationtransitioninfo.Effect <br> slidenavigationtransitioninfo.effectproperty

### [<a name="windowsuixamlmedia"></a>windows.ui.xaml.media](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media)

#### [<a name="brush"></a>brosse](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.brush)

Brush.populatepropertyinfo <br> Brush.populatepropertyinfooverride

### [<a name="windowsuixamlnavigation"></a>Windows.UI.Xaml.navigation](https://docs.microsoft.com/uwp/api/windows.ui.xaml.navigation)

#### [<a name="framenavigationoptions"></a>framenavigationoptions](https://docs.microsoft.com/uwp/api/windows.ui.xaml.navigation.framenavigationoptions)

framenavigationoptions <br> framenavigationoptions.framenavigationoptions <br> framenavigationoptions.isnavigationstackenabled <br> framenavigationoptions.transitioninfooverride

### [<a name="windowsuixaml"></a>windows.ui.xaml](https://docs.microsoft.com/uwp/api/windows.ui.xaml)

#### [<a name="brushtransition"></a>brushtransition](https://docs.microsoft.com/uwp/api/windows.ui.xaml.brushtransition)

brushtransition <br> brushtransition.brushtransition <br> brushtransition.Duration

#### [<a name="colorpaletteresources"></a>colorpaletteresources](https://docs.microsoft.com/uwp/api/windows.ui.xaml.colorpaletteresources)

colorpaletteresources <br> colorpaletteresources.accent <br> colorpaletteresources.althigh <br> colorpaletteresources.altlow <br> colorpaletteresources.altmedium <br> colorpaletteresources.altmediumhigh <br> colorpaletteresources.altmediumlow <br> colorpaletteresources.basehigh <br> colorpaletteresources.baselow <br> colorpaletteresources.basemedium <br> colorpaletteresources.basemediumhigh <br> colorpaletteresources.basemediumlow <br> colorpaletteresources.chromealtlow <br> colorpaletteresources.chromeblackhigh <br> colorpaletteresources.chromeblacklow <br> colorpaletteresources.chromeblackmedium <br> colorpaletteresources.chromeblackmediumlow <br> colorpaletteresources.chromedisabledhigh <br> colorpaletteresources.chromedisabledlow <br> colorpaletteresources.chromegray <br> colorpaletteresources.chromehigh <br> colorpaletteresources.chromelow <br> colorpaletteresources.chromemedium <br> colorpaletteresources.chromemediumlow <br> colorpaletteresources.chromewhite <br> colorpaletteresources.colorpaletteresources <br> colorpaletteresources.ErrorText <br> colorpaletteresources.listlow <br> colorpaletteresources.listmedium

#### [<a name="datatemplate"></a>DataTemplate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.datatemplate)

DataTemplate.GetElement <br> DataTemplate.recycleelement

#### [<a name="debugsettings"></a>debugsettings](https://docs.microsoft.com/uwp/api/windows.ui.xaml.debugsettings)

debugsettings.failfastonerrors

#### [<a name="effectiveviewportchangedeventargs"></a>effectiveviewportchangedeventargs](https://docs.microsoft.com/uwp/api/windows.ui.xaml.effectiveviewportchangedeventargs)

effectiveviewportchangedeventargs <br> effectiveviewportchangedeventargs.bringintoviewdistancex <br> effectiveviewportchangedeventargs.bringintoviewdistancey <br> effectiveviewportchangedeventargs.effectiveviewport <br> effectiveviewportchangedeventargs.maxviewport

#### [<a name="elementfactorygetargs"></a>elementfactorygetargs](https://docs.microsoft.com/uwp/api/windows.ui.xaml.elementfactorygetargs)

elementfactorygetargs <br> elementfactorygetargs.Data <br> elementfactorygetargs.elementfactorygetargs <br> elementfactorygetargs.parent

#### [<a name="elementfactoryrecycleargs"></a>elementfactoryrecycleargs](https://docs.microsoft.com/uwp/api/windows.ui.xaml.elementfactoryrecycleargs)

elementfactoryrecycleargs <br> elementfactoryrecycleargs.Element <br> elementfactoryrecycleargs.elementfactoryrecycleargs <br> elementfactoryrecycleargs.parent

#### [<a name="frameworkelement"></a>frameworkelement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement)

FrameworkElement.effectiveviewportchanged <br> FrameworkElement.invalidateviewport <br> FrameworkElement.IsLoaded

#### [<a name="ielementfactory"></a>ielementfactory](https://docs.microsoft.com/uwp/api/windows.ui.xaml.ielementfactory)

ielementfactory <br> ielementfactory.GetElement <br> ielementfactory.recycleelement

#### [<a name="scalartransition"></a>scalartransition](https://docs.microsoft.com/uwp/api/windows.ui.xaml.scalartransition)

scalartransition <br> scalartransition.Duration <br> scalartransition.scalartransition

#### [<a name="uielement"></a>uielement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement)

UIElement.canbescrollanchor <br> UIElement.canbescrollanchorproperty <br> UIElement.CenterPoint <br> UIElement.opacitytransition <br> UIElement.populatepropertyinfo <br> UIElement.populatepropertyinfooverride <br> UIElement.rotation <br> UIElement.rotationaxis <br> UIElement.rotationtransition <br> UIElement.Scale <br> UIElement.scaletransition <br> UIElement.StartAnimation <br> UIElement.stopanimation <br> UIElement.transformMatrix <br> UIElement.Translation <br> UIElement.translationtransition

#### [<a name="vector3transition"></a>vector3transition](https://docs.microsoft.com/uwp/api/windows.ui.xaml.vector3transition)

vector3transition <br> vector3transition.Components <br> vector3transition.Duration <br> vector3transition.vector3transition

#### [<a name="vector3transitioncomponents"></a>vector3transitioncomponents](https://docs.microsoft.com/uwp/api/windows.ui.xaml.vector3transitioncomponents)

vector3transitioncomponents

## <a name="windowsweb"></a>windows.web

### [<a name="windowswebuiinterop"></a>windows.web.ui.interop](https://docs.microsoft.com/uwp/api/windows.web.ui.interop)

#### [<a name="webviewcontrol"></a>webviewcontrol](https://docs.microsoft.com/uwp/api/windows.web.ui.interop.webviewcontrol)

webviewcontrol.addinitializescript <br> webviewcontrol.GotFocus <br> webviewcontrol.LostFocus

### [<a name="windowswebui"></a>windows.web.ui](https://docs.microsoft.com/uwp/api/windows.web.ui)

#### [<a name="iwebviewcontrol2"></a>iwebviewcontrol2](https://docs.microsoft.com/uwp/api/windows.web.ui.iwebviewcontrol2)

iwebviewcontrol2 <br> iwebviewcontrol2.addinitializescript

