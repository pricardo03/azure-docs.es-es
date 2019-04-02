---
title: Creación de alertas personalizadas para ASC for IoT, versión preliminar | Microsoft Docs
description: Cree y asigne alertas de dispositivos personalizadas para ASC for IoT.
services: ascforiot
documentationcenter: na
author: mlottner
manager: barbkess
editor: ''
ms.assetid: d1757868-da3d-4453-803a-7e3a309c8ce8
ms.service: ascforiot
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/19/2019
ms.author: mlottner
ms.openlocfilehash: 591000f251d384b961569f9d7ca09ae93edea617
ms.sourcegitcommit: cf971fe82e9ee70db9209bb196ddf36614d39d10
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2019
ms.locfileid: "58541771"
---
# <a name="quickstart-create-custom-alerts"></a>Inicio rápido: Creación de alertas personalizadas

> [!IMPORTANT]
> ASC for IoT está actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Mediante el uso de alertas y grupos de seguridad personalizados, saque el máximo provecho de la información de seguridad de un extremo a otro y del conocimiento de las categorías de dispositivos para garantizar una mejora de la seguridad en la solución de IoT. 

## <a name="why-use-custom-alerts"></a>¿Por qué se usan las alertas personalizadas? 

Usted es quien mejor conoce sus dispositivos IoT.

Para los clientes que conocen a la perfección el comportamiento esperado de su dispositivo, ASC for IoT permite convertir este conocimiento en una directiva de comportamiento del dispositivo y en una alerta ante cualquier desviación con respecto al comportamiento normal y esperado.

## <a name="security-groups"></a>Grupos de seguridad

Los grupos de seguridad permiten definir grupos lógicos de dispositivos y administran su estado de seguridad de forma centralizada.

Estos grupos pueden representar los dispositivos con un hardware concreto, los dispositivos implementados en una ubicación determinada o cualquier otro grupo adecuado para sus necesidades específicas.

Los grupos de seguridad los define una propiedad de la etiqueta del módulo gemelo de seguridad denominada **SecurityGroup**. Para cambiar el grupo de seguridad de un dispositivo cambie el valor de esta propiedad.  

De manera predeterminada, en IoT Hub cada solución de IoT de tiene un grupo de seguridad denominado **predeterminado**.

Use los grupos de seguridad para agrupar los dispositivos en categorías lógicas. Después de crear los grupos asígnelos a las alertas personalizadas que prefiera para la solución de un extremo a otro más eficaz. 

## <a name="customize-an-alert"></a>Personalización de una alerta

1. Abra IoT Hub. 
2. Seleccione **Security** (Seguridad) y ,después, seleccione **Custom alerts** (Alertas personalizadas). 
3. Elija los grupos de seguridad a los que desea aplicar la personalización. 
4. Haga clic en **Add a custom alert** (Agregar una alerta personalizada)
5. Escriba un nombre de alerta (tenga en cuenta que los nombres de las alertas no se puede cambiar después de la creación). 
6. Seleccione el comportamiento de una alerta personalizada en la lista desplegable. 
7. Edite las propiedades necesarias y haga clic en **OK** (Aceptar).
8. Asegúrese de hacer clic en **SAVE** (Guardar). Si no se guarda la nueva alerta, la alerta se eliminará la próxima vez que se cierra IoT Hub.

 
## <a name="alerts-available-for-customization"></a>Alertas disponibles para la personalización

En la tabla siguiente se proporciona un resumen de las alertas disponibles para la personalización.

| Gravedad | NOMBRE                                                                                                    | Origen de datos | DESCRIPCIÓN                                                                                                                                     |
|----------|---------------------------------------------------------------------------------------------------------|-------------|-------------------------------------------------------------------------------------------------------------------------------------------------|
| Bajo      | Alerta personalizada: el número de mensajes de la nube al dispositivo en el protocolo AMQP no se encuentra en el intervalo permitido          | IoT Hub     | La cantidad de mensajes de la nube al dispositivo (protocolo AMQP) en un período de tiempo no está en el intervalo permitido configurado                                  |
| Bajo      | Alerta personalizada: el número de mensajes de la nube al dispositivo rechazados en el protocolo AMQP no se encuentra en el intervalo permitido | IoT Hub     | La cantidad de mensajes de la nube al dispositivo (protocolo AMQP) que rechazaba el dispositivo en un periodo de tiempo no se encuentra en el intervalo permitido configurado |
| Bajo      | Alerta personalizada: el número de mensajes del dispositivo a la nube en el protocolo AMQP no se encuentra en el intervalo permitido          | IoT Hub     | La cantidad de mensajes del dispositivo a la nube (protocolo AMQP) en un período de tiempo no está en el intervalo permitido configurado                                  |
| Bajo      | Alerta personalizada: el número de invocaciones del método directo no se encuentra en el intervalo permitido                              | IoT Hub     | La cantidad de invocaciones del método directo en un período de tiempo no está en el intervalo permitido configurado                                                     |
| Bajo      | Alerta personalizada: el número de cargas de archivos no se encuentra en el intervalo permitido                                       | IoT Hub     | La cantidad de cargas de archivos en un período de tiempo no está en el intervalo permitido configurado                                                              |
| Bajo      | Alerta personalizada: el número de mensajes de la nube al dispositivo en el protocolo HTTP no se encuentra en el intervalo permitido          | IoT Hub     | La cantidad de mensajes de la nube al dispositivo (protocolo HTTP) en un período de tiempo no está en el intervalo permitido configurado                                  |
| Bajo      | Alerta personalizada: el número de mensajes de la nube al dispositivo rechazados en el protocolo HTTP no se encuentra en el intervalo permitido | IoT Hub     | La cantidad de mensajes de la nube al dispositivo (protocolo HTTP) que rechazaba el dispositivo en un periodo de tiempo no se encuentra en el intervalo permitido configurado |
| Bajo      | Alerta personalizada: el número de mensajes del dispositivo a la nube en el protocolo HTTP no se encuentra en el intervalo permitido          | IoT Hub     | La cantidad de mensajes del dispositivo a la nube (protocolo HTTP) en un período de tiempo no está en el intervalo permitido configurado                                  |
| Bajo      | Alerta personalizada: el número de mensajes de la nube al dispositivo en el protocolo MQTT no se encuentra en el intervalo permitido          | IoT Hub     | La cantidad de mensajes de la nube al dispositivo (protocolo MQTT) en un período de tiempo no está en el intervalo permitido configurado                                  |
| Bajo      | Alerta personalizada: el número de mensajes de la nube al dispositivo rechazados en el protocolo MQTT no se encuentra en el intervalo permitido | IoT Hub     | La cantidad de mensajes de la nube al dispositivo (protocolo MQTT) que rechazaba el dispositivo en un periodo de tiempo no se encuentra en el intervalo permitido configurado |
| Bajo      | Alerta personalizada: el número de mensajes del dispositivo a la nube en el protocolo MQTT no se encuentra en el intervalo permitido          | IoT Hub     | La cantidad de mensajes del dispositivo a la nube (protocolo MQTT) en un período de tiempo no está en el intervalo permitido configurado                                  |
| Bajo      | Alerta personalizada: el número de purgas de la cola de comandos no se encuentra en el intervalo permitido                               | IoT Hub     | La cantidad de purgas de la cola de comandos en un período de tiempo no está en el intervalo permitido configurado                                                      |
| Bajo      | Alerta personalizada: el número de actualizaciones gemelas no se encuentra en el intervalo permitido                                       | IoT Hub     | La cantidad de actualizaciones gemelas en un período de tiempo no está en el intervalo permitido configurado                                                              |
| Bajo      | Alerta personalizada: el número de operaciones sin autorizar no se encuentra en el intervalo permitido                            | IoT Hub     | La cantidad de operaciones sin autorizar en un período de tiempo no está en el intervalo permitido configurado                                                   |
| Bajo      | Alerta personalizada: el número de conexiones activas no se encuentra en el intervalo permitido                                        | Agente       | La cantidad de conexiones activas en un período de tiempo no está en el intervalo permitido configurado                                                        |
| Bajo      | Alerta personalizada: se ha creado una conexión saliente a una dirección IP que no está permitida                              | Agente       | Se ha creado una conexión saliente a una dirección IP que no está permitida                                                                                  |
| Bajo      | Alerta personalizada: el número de inicios de sesión locales con errores no se encuentra en el intervalo permitido                                | Agente       | La cantidad de inicios de sesión locales con errores en un período de tiempo no está en el intervalo permitido configurado                                                       |
| Bajo      | Alerta personalizada: el inicio de sesión de un usuario que no está permitido                                                      | Agente       | Un usuario local no permitido ha iniciado sesión en el dispositivo                                                                                        |
| Bajo      | Alerta personalizada: la ejecución de un proceso que no se permite                                               | Agente       | Se ha ejecutado en el dispositivo un proceso que no se permite |          |

## <a name="next-steps"></a>Pasos siguientes

Pase al siguiente artículo para aprender a implementar a un agente de seguridad...

> [!div class="nextstepaction"]
> [Implementación de un agente de seguridad](select-deploy-agent.md)
