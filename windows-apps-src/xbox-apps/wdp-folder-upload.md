---
author: WilliamsJason
title: Device Portal folder upload API reference
description: Learn how to access the folder upload APIs programatically.
translationtype: Human Translation
ms.sourcegitcommit: fdc25fa4bd7bd5bfa598b993f23cd0ae9783dd0e
ms.openlocfilehash: 6c3eeccdbb2bca315dc84293a36d0923f46e746d

---

# Upload a folder to the development directory

**Request**

You can upload an entire folder at once to the Known Folder Id for the DevelopmentFiles (or to a subfolder within that folder).

Method      | Request URI
:------     | :------
POST | /api/app/packagemanager/upload 
<br />
**URI parameters**

You can specify the following additional parameters on the request URI:

URI Parameter      | Description
:------     | :-----
destinationFolder  (required) | The destination folder name of the folder to be uploaded. This folder will be placed under d:\developmentfiles\LooseApps on the console. This folder name should be base64 encoded as it may contain path separators if the folder is a subfolder under LooseApps.
<br />

**Request headers**

- None

**Request body**

- multi-part conforming http body of the directory contents.

**Response**

**Status code**

This API has the following expected status codes.

HTTP status code      | Description
:------     | :-----
200 | Success
4XX | Error codes
5XX | Error codes
<br />
**Available device families**

* Windows Xbox




<!--HONumber=Aug16_HO3-->


