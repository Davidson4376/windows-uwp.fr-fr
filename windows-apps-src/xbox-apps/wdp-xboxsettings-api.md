---
author: payzer
title: Device Portal Xbox Developer settings API reference
description: Learn how to access Xbox developer settings.
translationtype: Human Translation
ms.sourcegitcommit: c51eff41e63d815f6298b4fc46a9b11314bc8bc9
ms.openlocfilehash: 5a983714cda9b5a5f45e555e2cb6f980f082a003

---

# Developer settings API reference   
You can access Xbox One settings that are useful for development using this API.

## Get all developer settings at once

**Request**

You can use the following request to get all developer settings in a single request.

Method      | Request URI
:------     | :-----
GET | /ext/settings
<br />
**URI parameters**

- None

**Request headers**

- None

**Request body**

- None

**Response**   
The response is a Settings JSON array containing all the settings. Each settings object contains the following fields:   

Name - (String) The name of the setting.   
Value - (String) The value of the setting.   
RequiresReboot - ("Yes" | "No") This field indicates whether the setting requires a reboot to take effect.
Category - (String) The category of the setting

**Status code**

This API has the following expected status codes.

HTTP status code      | Description
:------     | :-----
200 | Request was successful
4XX | Error codes
5XX | Error codes

## Get settings one at a time
Settings can also be retrieved individually.

**Request**

You can use the following request to get information about an individual setting.

Method      | Request URI
:------     | :-----
GET | /ext/settings/\<setting name\>
<br />
**URI parameters**

- None

**Request headers**

- None

**Request body**

- None

**Response**   
The response is a JSON object with following fields:   

Name - (String) The name of the setting.   
Value - (String) The value of the setting.   
RequiresReboot - ("Yes" | "No") This field indicates whether the setting requires a reboot to take effect.
Category - (String) The category of the setting

**Status code**

This API has the following expected status codes.

HTTP status code      | Description
:------     | :-----
200 | Request was successful
4XX | Error codes
5XX | Error codes

## Set the value of a setting
You can set the value of a setting.

**Request**

You can use the following request to set the value for a setting.

Method      | Request URI
:------     | :-----
PUT | /ext/settings/\<setting name\>
<br />
**URI parameters**

- None

**Request headers**

- None

**Request body**   
The request body is JSON object containing the following field:   
Value - (String) The new value for the setting.

**Response**   

- None

**Status code**

This API has the following expected status codes.

HTTP status code      | Description
:------     | :-----
200 | Request was successful
4XX | Error codes
5XX | Error codes

<br />
**Available device families**

* Windows Xbox




<!--HONumber=Aug16_HO3-->


