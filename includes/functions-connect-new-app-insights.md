---
title: archivo de inclusión
description: archivo de inclusión
services: functions
author: ggailey777
ms.service: functions
ms.topic: include
ms.date: 04/06/2019
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: b6cafcfe6c892cd43f056458fe3586da834c2fd1
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60403887"
---
Functions simplifica el proceso de agregar la integración de Application Insights a una aplicación de función desde [Azure Portal].

1. En el [portal][Azure Portal], seleccione **Todos los servicios> Aplicaciones de función**, seleccione su aplicación de función y, luego, elija el banner **Application Insights** de la parte superior de la ventana

    ![Habilitación de Application Insights desde el portal](media/functions-connect-new-app-insights/enable-application-insights.png)

1. Cree un recurso de Application Insights mediante la configuración especificada en la tabla que hay debajo de la imagen:

   ![Creación de recursos en Application Insights](media/functions-connect-new-app-insights/ai-general.png)

    | Configuración      | Valor sugerido  | Descripción                                        |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Nombre** | Nombre de aplicación único | Lo más fácil es usar el mismo nombre que usa para la aplicación de función, que debe ser único en su suscripción. | 
    | **Ubicación** | Europa occidental | Si es posible, use la misma [región](https://azure.microsoft.com/regions/) que la de la aplicación de función, o una que esté cerca. |

1. Elija **Aceptar**. El recurso de Application Insights se crea en el mismo grupo de recursos y suscripción que su aplicación de función. Una vez se complete la creación, cierre la ventana de Application Insights.

1. En la aplicación de función, seleccione **Configuración de la aplicación**y desplácese hacia abajo hasta **Configuración de la aplicación**. Cuando vea una configuración denominada `APPINSIGHTS_INSTRUMENTATIONKEY`, significa que la integración de Application Insights está habilitada para la aplicación de función que se ejecuta en Azure.

[Azure Portal]: https://portal.azure.com
