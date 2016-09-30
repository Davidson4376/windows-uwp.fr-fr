---
author: TylerMSFT
Description: The JavaScript API for the Microsoft Take a Test app allows you to do secure assessments. Take a Test provides a secure browser that prevents students from using other computer or internet resources during a test.
title: Microsoft Take a Test JavaScript API.
translationtype: Human Translation
ms.sourcegitcommit: f2838d95da66eda32d9cea725a33fc4084d32359
ms.openlocfilehash: d7f185e83e81583fd6d7920e5412f76f3a97edd0

---

# Microsoft Take a Test JavaScript API

**Take a Test** is a browser-based app that renders locked down online assessments for high-stakes testing. It supports the SBAC browser API standard for high stakes common core testing and allows you to focus on the assessment content rather than how to lock down Windows.

**Take a Test**, powered by Microsoft's Edge browser, provides a JavaScript API that Web applications can use to provide a locked down experience for taking tests.

The API (based on the [Common Core SBAC API](http://www.smarterapp.org/documents/SecureBrowserRequirementsSpecifications_0-3.pdf)) provides text to speech and the capability to query if the device is locked down, what the running user and system running processes are, and more.

See the [Take a Test app technical reference](https://technet.microsoft.com/en-us/edu/windows/take-a-test-app-technical?f=255&MSPPError=-2147217396) for information about the app itself.

**Important**

The APIs do not work in a remote session.  
Take a Test does not handle HTTP new window requests.

For troubleshooting help, see [Troubleshoot Microsoft Take a Test with the event viewer](troubleshooting.md).

**The Take a Test API consists of the following namespaces:**  

| Namespace | Description |
|-----------|-------------|
|[security namespace](#security-namespace)| Text to speech functionality|
|[tts namespace](#tts-namespace)|Enables you to lock down the device|


 ## security namespace

Enables you to lock down the device, check the list of user and system processes, obtain MAC and IP addresses, and clear cached web resources.

| Method | Description   |
|--------|---------------|
|[clearCache](#clearCache) | Clears cached web resources |
|[close](#close) | Closes the browser and unlocks the device |
|[enableLockDown](#enableLockDown) | Locks down the device. Also used to unlock the device |
|[getIPAddressList](#getIPAddressList) | Gets the list of IP addresses for the device |
|[getMACAddress](#getMACAddress)|Gets the list of MAC addresses for the device|
|[getProcessList](#getProcessList)|Gets the list of running user and system processes|
|[isEnvironmentSecure](#isEnvironmentSecure)|Determines whether the lockdown context is still applied to the device|

<span id="clearCache" />
### void clearCache()
Clear cached web resources.

**Syntax**  
`browser.security.clearCache();`

**Parameters**  
`None`

**Return value**  
`None`

**Requirements**  
Windows 10, version 1607

---

<span id="close"/>
### close(boolean restart)
Closes the browser and unlocks the device.

**Syntax**  
`browser.security.close(false);`

**Parameters**  
`restart` - this parameter is ignored but must be provided.

**Return value**  
`None`

**Requirements**  
Windows 10, version 1607

---

<span id="enableLockDown"/>
### enableLockdown(boolean lockdown)
Locks down the device. Also used to unlock the device.

**Syntax**  
`browser.security.enableLockDown(true|false);`

**Parameters**  
`lockdown` - `true` to run the Take-a-Test app above the lock screen and apply policies discussed in this [document](https://technet.microsoft.com/en-us/edu/windows/take-a-test-app-technical?f=255&MSPPError=-2147217396). `False` stops running Take-a-Test above the lock screen and closes it unless the app is not locked down; in which case there is no effect.

**Return value**  
`None`

**Requirements**  
Windows 10, version 1607

---

<span id="getIPAddressList"/>
### string[] getIPAddressList()
Gets the list of IP addresses for the device.

**Syntax**  
`browser.security.getIPAddressList();`

**Parameters**  
`None`

**Return value**  
`An array of IP addresses.`

<span id="getMACAddress" />
### string[] getMACAddress()
Gets the list of MAC addresses for the device.

**Syntax**  
`browser.security.getMACAddress();`

**Parameters**  
`None`

**Return value**  
`An array of MAC addresses.`

**Requirements**  
Windows 10, version 1607

---

<span id="getProcessList" />
### string[] getProcessList()
Gets the list the user’s running processes.

**Syntax**  
`browser.security.getProcessList();`

**Parameters**  
`None`

**Return value**  
`An array of running process names.`

**Remarks** The list does not include system processes.

**Requirements**  
Windows 10, version 1607

---

<span id="isEnvironmentSecure" />
### boolean isEnvironmentSecure()
Determines whether the lockdown context is still applied to the device.

**Syntax**  
`browser.security.isEnvironmentSecure();`

**Parameters**  
`None`

**Return value**  
`True indicates that the lockdown context is applied to the device; otherwise false.`

**Requirements**  
Windows 10, version 1607

---

## tts namespace
| Method | Description |
|--------|-------------|
|[getStatus](#getStatus) | Gets the speech playback status|
|[getVoices](#getVoices) | Gets a list of available voice packs|
|[pause](#pause)|Pauses speech synthesis|
|[resume](#resume)|Resume paused speech synthesis|
|[speak](#speak)|Client-side text to speech synthesis|
|[stop](#stop)|Stops speech synthesis|

> [!Tip]
> The [Microsoft Edge Speech Synthesis API](https://blogs.windows.com/msedgedev/2016/06/01/introducing-speech-synthesis-api/) is an implementation of the [W3C Speech Api](https://dvcs.w3.org/hg/speech-api/raw-file/tip/webspeechapi.html) and we recommend that developers use that API when possible.

<span id="getStatus" />
### string getStatus()
Gets the speech playback status.

**Syntax**  
`browser.tts.getStatus();`

**Parameters**  
`None`

**Return value**  
`The speech playback status. Possible values are: “available”, “idle”, “paused”, and “speaking”.`

**Requirements**  
Windows 10, version 1607

---

<span id="getVoices" />
### string[] getVoices()
Gets a list of available voice packs.

**Syntax**  
`browser.tts.getVoices();`

**Parameters**  
`None`

**Return value**  
`The available voice packs. For example: “Microsoft Zira Mobile”, “Microsoft Mark Mobile”`

**Requirements**  
Windows 10, version 1607

---

<span id="pause" />
### void pause()

Pauses speech synthesis.

**Syntax**  
`browser.tts.pause();`

**Parameters**

`None`

**Return value**

`None`

**Requirements**  
Windows 10, version 1607

---

<span id="resume" />
### void resume()
Resume paused speech synthesis.

**Syntax**  
`browser.tts.resume();`

**Parameters**
`None`

**Return value**
`None`

**Requirements**  
Windows 10, version 1607

---

<span id="speak" />
### void speak(string text, object options, function callback)
Client-side text to speech synthesis.

**Syntax**  
`void browser.tts.speak(“Hello world”, options, callback);`

**Parameters**  
`Speech options such as gender, pitch, rate, volume. For example:`  
```
var options = {
   'gender': this.currentGender,
   'language': this.currentLanguage,
   'pitch': 1,
   'rate': 1,
   'voice': this.currentVoice,
   'volume': 1
};
```

**Return value**  
`None`

**Remarks** Option variables must be lowercase. The gender, language, and voice parameters take strings.
Volume, pitch, and rate must be marked up within the speech synthesis markup language file (SSML), not within the options object.

The options object must follow the order, naming, and casing shown in the example above.

**Requirements**  
Windows 10, version 1607

---
<span id="stop" />
### void stop()
Stops speech synthesis.

**Syntax**  
`void browser.tts.speak(“Hello world”, options, callback);`

**Parameters**  
`None`

**Return value**  
`None`

**Requirements**  
Windows 10, version 1607



<!--HONumber=Aug16_HO3-->


