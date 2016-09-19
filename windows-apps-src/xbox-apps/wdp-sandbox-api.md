---
author: payzer
title: Device Portal Xbox Live sandbox API reference
description: Learn how to access the Xbox Live Sandbox programatically.
translationtype: Human Translation
ms.sourcegitcommit: a857ba338a971e651653193ff2149f08b1665a36
ms.openlocfilehash: 2a0bfa2eecffb2b0f5ed0bc691cb90bcd7191321

---

# Xbox Live Sandbox API reference   
You can get and set your Xbox Live sandbox using this REST API.

## Get the Xbox Live sandbox

**Request**

You can read the current value for the device's Xbox Live sandbox using the following request:

Method      | Request URI
:------     | :-----
GET | /ext/xboxlive/sandbox
<br />
**URI parameters**

- None

**Request headers**

- None

**Request body**

- None

**Response**   
Sandbox - (String) The current Sandbox the device is in.   

**Status code**

This API has the following expected status codes.

HTTP status code      | Description
:------     | :-----
200 | The request to access the credentials for the file share was granted.
4XX | Error codes
5XX | Error codes

## Set the Xbox Live sandbox
You can change the Xbox Live sandbox for the device using the following request. Note that on Xbox One, the device needs to be restarted before the setting takes effect.

**Request**

You can set the current value for the device's Xbox Live sandbox using the following request:

Method      | Request URI
:------     | :-----
PUT | /ext/xboxlive/sandbox
<br />
**URI parameters**

- None

**Request headers**

- None

**Request body**   
The request body is a JSON object containing the following field:   
Sandbox - (String) The new value to set the device's sandbox to.

**Response**   
Sandbox - (String) The current Sandbox the device is in.   

**Status code**

This API has the following expected status codes.

HTTP status code      | Description
:------     | :-----
200 | The request to access the credentials for the file share was granted.
4XX | Error codes
5XX | Error codes

<br />
**Available device families**

* Windows Xbox




<!--HONumber=Aug16_HO3-->


