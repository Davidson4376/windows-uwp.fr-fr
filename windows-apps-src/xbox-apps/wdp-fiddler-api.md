---
author: WilliamsJason
title: Device Portal Fiddler API reference
description: Learn how to enable/disable Fiddler tracing programatically.
translationtype: Human Translation
ms.sourcegitcommit: 3cc2a4bd1859e46a73f3e806489eac7381fa6c17
ms.openlocfilehash: bd215058c71118d8b3e5ce81e2302ce8b151c3f6

---

# Fiddler settings API reference   
You can enable and disable Fiddler network tracing on your devkit using this REST API.

## Enable Fiddler tracing

**Request**

You can enable Fiddler tracing for the devkit using the following request.  Note that the device must be restarted before this takes effect.

Method      | Request URI
:------     | :-----
POST | /ext/fiddler
<br />
**URI parameters**

You can specify the following additional parameters on the request URI:

| URI parameter      | Description     | 
| ------------------ |-----------------|
| proxyAddress       | The IP address or hostname of the device running Fiddler |
| proxyPort          | The port which Fiddler is using for monitoring traffic. Defaults to 8888 |
| updateCert (optional)| A boolean value indicating if the root Fiddler cert is provided. This must be true if Fiddler has never been configured on this devkit or was configured for a different host.  |
<br>

**Request headers**

- None

**Request body**

- None if updateCert is false or not provided. Multi-part conforming http body containing the FiddlerRoot.cer file otherwise.

**Response**   

- None  

**Status code**

This API has the following expected status codes.

HTTP status code      | Description
:------     | :-----
204 | The request to enable Fiddler was accepted. Fiddler will be enabled the next time the device reboots.
4XX | Error codes
5XX | Error codes

## Disable Fiddler tracing on the devkit

**Request**

You can disable Fiddler tracing on the device using the following request. Note that the device must be restarted before this takes effect.

Method      | Request URI
:------     | :-----
DELETE | /ext/fiddler
<br />
**URI parameters**

- None

**Request headers**

- None

**Request body**   

- None

**Response**   

- None 

**Status code**

This API has the following expected status codes.

HTTP status code      | Description
:------     | :-----
204 | The request to disable Fiddler tracing was successful. Tracing will be disabled on the next reboot of the device.
4XX | Error codes
5XX | Error codes

<br />
**Available device families**

* Windows Xbox

## See also
- [Configuring Fiddler for UWP on Xbox](uwp-fiddler.md)




<!--HONumber=Aug16_HO3-->


