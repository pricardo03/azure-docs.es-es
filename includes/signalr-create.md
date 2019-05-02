---
title: archivo de inclusión
description: archivo de inclusión
services: signalr
author: wesmc7777
ms.service: signalr
ms.topic: include
ms.date: 04/17/2018
ms.author: wesmc
ms.custom: include file
ms.openlocfilehash: 57407606214d8d3a305476cfbfdabca9eee937e8
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60690321"
---
1. Para crear un recurso del servicio Azure SignalR nuevo, inicie sesión en [Azure Portal](https://portal.azure.com). En la parte superior izquierda de la página, haga clic en **+ Crear un recurso**. En el cuadro de texto **Buscar en Marketplace**, escriba **Servicio SignalR** y presione **Entrar**.

2. Haga clic en **Servicio SignalR** en los resultados y haga clic en **Crear**.

3. En la página de configuración de **SignalR**, agregue la configuración siguiente para el recurso SignalR nuevo:

    | Nombre | Valor recomendado | Descripción |
    | ---- | ----------------- | ----------- |
    | Nombre de recurso | *testsignalr* | Escriba un nombre de recurso único para usarlo en el recurso SignalR. El nombre debe ser una cadena de entre 1 y 63 caracteres y solo puede contener números, letras y el carácter `-`. El nombre no puede comenzar ni terminar por el carácter `-` y no se pueden usar varios caracteres `-` consecutivos.|
    | Suscripción | Elija una suscripción |  Seleccione la suscripción de Azure que quiera usar para probar SignalR. Si su cuenta solo dispone de una suscripción, esta se seleccionará automáticamente y no aparecerá la lista desplegable **Suscripción**.|
    | Grupos de recursos | Cree un nuevo grupo de recursos denominado *SignalRTestResources*| Seleccione o cree un grupo de recursos para el recurso SignalR. Este grupo es útil para organizar los distintos recursos que quiera eliminar al mismo tiempo mediante la eliminación del grupo de recursos. Para más información, consulte el artículo sobre el [uso de grupos de recursos para administrar los recursos de Azure](../articles/azure-resource-manager/resource-group-overview.md). |
    | Ubicación | *Este de EE. UU.* | Use **Ubicación** para especificar la ubicación geográfica en la que se hospeda el recurso SignalR. Para optimizar el rendimiento, recomendamos que cree el recurso en la misma región que los demás componentes de la aplicación. |
    | Plan de tarifa | *Gratis* | Actualmente están disponibles las opciones **Gratis** y **Estándar**. |
    | Anclar al panel | ✔ | Active esta casilla para anclar el recurso al panel, para que sea más fácil encontrarlo. |

4. Haga clic en **Crear**. La implementación puede tardar unos minutos en completarse.

5. Una vez que se complete la implementación, haga clic en **Claves** en **CONFIGURACIÓN**. Copie la cadena de conexión de clave principal. La usará más adelante para configurar la aplicación para que use el recurso del servicio Azure SignalR.

    La cadena de conexión tendrá la forma siguiente:
    
        Endpoint=<service_endpoint>;AccessKey=<access_key>;
