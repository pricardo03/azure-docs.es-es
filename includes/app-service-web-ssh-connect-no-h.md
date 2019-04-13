---
title: archivo de inclusión
description: archivo de inclusión
services: app-service
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 03/29/2019
ms.author: cephalin
ms.custom: include file
ms.openlocfilehash: 060bc1039982cc0a77214d5dbe2a08de7a839c84
ms.sourcegitcommit: 031e4165a1767c00bb5365ce9b2a189c8b69d4c0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/13/2019
ms.locfileid: "59550015"
---
Para que abra una sesión SSH directa con el contenedor, debe ejecutar la aplicación.

Pegue la siguiente dirección URL en el explorador y reemplazar \<app-name > por su nombre de aplicación:

```
https://<app-name>.scm.azurewebsites.net/webssh/host
```

Si aún no está autenticado, es necesario para autenticarse con su suscripción de Azure para conectarse. Una vez autenticado, verá un shell del explorador en el que puede ejecutar comandos dentro del contenedor.

![Conexión SSH](./media/app-service-web-ssh-connect-no-h/app-service-linux-ssh-connection.png)
