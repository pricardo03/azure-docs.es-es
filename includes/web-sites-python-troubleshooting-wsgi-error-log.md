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
ms.openlocfilehash: 020e59f029b09f3c7656f67039731e4141e68d31
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "61270208"
---
Si Python encuentra un error al iniciar la aplicación, se devolverá solo una página de error simple (p. ej. "La página no se puede mostrar porque se ha producido un error interno del servidor").

Para capturar errores de aplicaciones Python:

1. En Azure Portal, en la aplicación web, seleccione **Configuración**.
2. En la pestaña **Configuración**, seleccione **Configuración de la aplicación**.
3. En **Configuración de la aplicación**, escriba el siguiente par clave-valor:
    * Clave: WSGI_LOG
    * Valor: D:\home\site\wwwroot\logs.txt (escriba el nombre de archivo)

Ahora debería ver los errores en el archivo logs.txt en la carpeta wwwroot.
