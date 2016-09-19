---
author: WilliamsJason
title: Media Capture API reference
description: Learn how to access the media capture API programatically.
translationtype: Human Translation
ms.sourcegitcommit: 4356bd2cfc7697905ed91d60b5829c06d850e109
ms.openlocfilehash: 2f77c565acf272a1a8f147eb07ac6c7f08bf8ef0

---

# Media Capture API reference #

**Request**

You can capture a PNG representation of the current screen by using the following request format.

| Method        | Request URI     | 
| ------------- |-----------------|
| GET           | /ext/screenshot |
<br>

**URI parameters**

You can specify the following additional parameters on the request URI:


| URI parameter      | Description     | 
| ------------------ |-----------------|
| download (optional)| A boolean value indicating if HTTP response headers should be set indicating that the host browser should download the screenshot as an attachment rather than rendering it in the browser.  |
<br>

**Request headers**

* None

**Request body**

* None

###Response###

**Status code**

This API has the following expected status codes.

| HTTP status code   | Description     | 
| ------------------ |-----------------|
| 200                | Screenshot request successful and capture returned |
| 5XX                | Error codes for unexpected failures |
<br>

**Available device families**

* Windows Xbox




<!--HONumber=Aug16_HO3-->


