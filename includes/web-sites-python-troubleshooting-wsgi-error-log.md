---
title: archivo de inclusión
description: archivo de inclusión
services: app-service
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 06/11/2018
ms.author: cephalin
ms.custom: include file
ms.openlocfilehash: 5169432af65a046465c9d1ced349687d6fdc8bb7
ms.sourcegitcommit: caebf2bb2fc6574aeee1b46d694a61f8b9243198
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/12/2018
ms.locfileid: "35394033"
---
Si Python encuentra un error al iniciar la aplicación, se devolverá solo una página de error simple (p. ej. "La página no se puede mostrar porque se ha producido un error interno del servidor").

Para capturar errores de aplicaciones Python:

1. En Azure Portal, en la aplicación web, seleccione **Configuración**.
2. En la pestaña **Configuración**, seleccione **Configuración de la aplicación**.
3. En **Configuración de la aplicación**, escriba el siguiente par clave-valor:
    * Clave: WSGI_LOG
    * Valor: D:\home\site\wwwroot\logs.txt (escriba el nombre de archivo que elija)

Ahora debería ver los errores en el archivo logs.txt en la carpeta wwwroot.
