---
title: 'Inicio rápido: Creación de alertas personalizadas para Azure Security Center para IoT'
description: En esta guía de inicio rápido, cree y asigne alertas de dispositivo personalizadas para Azure Security Center para IoT.
services: asc-for-iot
ms.service: asc-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.assetid: d1757868-da3d-4453-803a-7e3a309c8ce8
ms.subservice: asc-for-iot
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/08/2019
ms.author: mlottner
ms.openlocfilehash: eca5d69efb04cf8210b0b2aa502bcee5cd4f5264
ms.sourcegitcommit: bc193bc4df4b85d3f05538b5e7274df2138a4574
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2019
ms.locfileid: "73904137"
---
# <a name="quickstart-create-custom-alerts"></a>Inicio rápido: Creación de alertas personalizadas


Mediante el uso de alertas y grupos de seguridad personalizados, saque el máximo provecho de la información de seguridad de un extremo a otro y del conocimiento de las categorías de dispositivos para garantizar una mejora de la seguridad en la solución de IoT. 

## <a name="why-use-custom-alerts"></a>¿Por qué se usan las alertas personalizadas? 

Usted es quien mejor conoce sus dispositivos IoT.

Para los clientes que conocen a la perfección el comportamiento esperado de su dispositivo, Azure Security Center para IoT permite convertir este conocimiento en una directiva de comportamiento del dispositivo y en una alerta ante cualquier desviación con respecto al comportamiento normal y esperado.

## <a name="security-groups"></a>Grupos de seguridad

Los grupos de seguridad permiten definir grupos lógicos de dispositivos y administran su estado de seguridad de forma centralizada.

Estos grupos pueden representar los dispositivos con un hardware concreto, los dispositivos implementados en una ubicación determinada o cualquier otro grupo adecuado para sus necesidades específicas.

Los grupos de seguridad los define una propiedad de la etiqueta del dispositivo gemelo denominada **SecurityGroup**. De manera predeterminada, en IoT Hub cada solución de IoT de tiene un grupo de seguridad denominado **predeterminado**. Para cambiar el grupo de seguridad de un dispositivo, cambie el valor de la propiedad **SecurityGroup**.
 
Por ejemplo:

```
{
  "deviceId": "VM-Contoso12",
  "etag": "AAAAAAAAAAM=",
  "deviceEtag": "ODA1BzA5QjM2",
  "status": "enabled",
  "statusUpdateTime": "0001-01-01T00:00:00",
  "connectionState": "Disconnected",
  "lastActivityTime": "0001-01-01T00:00:00",
  "cloudToDeviceMessageCount": 0,
  "authenticationType": "sas",
  "x509Thumbprint": {
    "primaryThumbprint": null,
    "secondaryThumbprint": null
  },
  "version": 4,
  "tags": {
    "SecurityGroup": "default"
  }, 
```

Use los grupos de seguridad para agrupar los dispositivos en categorías lógicas. Después de crear los grupos asígnelos a las alertas personalizadas que prefiera para la solución de un extremo a otro de seguridad de IoT más eficaz. 

## <a name="customize-an-alert"></a>Personalización de una alerta

1. Abra IoT Hub. 
2. Haga clic en **Alertas personalizadas** en la sección **Seguridad**. 
3. Elija un grupo de seguridad al que desee aplicar la personalización. 
4. Haga clic en **Add a custom alert** (Agregar una alerta personalizada).
5. Seleccione una alerta personalizada en la lista desplegable. 
6. Edite las propiedades necesarias y haga clic en **OK** (Aceptar).
7. Asegúrese de hacer clic en **SAVE** (Guardar). Si no se guarda la nueva alerta, la alerta se eliminará la próxima vez que se cierra IoT Hub.

 
## <a name="alerts-available-for-customization"></a>Alertas disponibles para la personalización

En la tabla siguiente se proporciona un resumen de las alertas disponibles para la personalización.


| severity | NOMBRE | Origen de datos | DESCRIPCIÓN | Corrección sugerida|
|---|---|---|---|---|
| Bajo      | Alerta personalizada: el número de mensajes de la nube al dispositivo en el protocolo AMQP está fuera del intervalo permitido.          | IoT Hub     | El número de mensajes de nube a dispositivo (protocolo AMQP) dentro de un período de tiempo específico está fuera del intervalo configurado y permitido actualmente.||
| Bajo      | Alerta personalizada: el número de mensajes de la nube al dispositivo rechazados en el protocolo AMQP está fuera del intervalo permitido. | IoT Hub     | El número de mensajes de nube a dispositivo (protocolo AMQP) rechazados por el dispositivo dentro de un período de tiempo específico está fuera del intervalo configurado y permitido actualmente.||
| Bajo      | Alerta personalizada: el número de mensajes del dispositivo a la nube en el protocolo AMQP está fuera del intervalo permitido.      | IoT Hub     | La cantidad de mensajes de dispositivo a nube (protocolo AMQP) dentro de un período de tiempo específico está fuera del intervalo configurado y permitido actualmente.|   |
| Bajo      | Alerta personalizada: el número de invocaciones del método directo está fuera del intervalo permitido. | IoT Hub     | La cantidad de invocaciones del método directo dentro de un período de tiempo específico está fuera del intervalo configurado y permitido actualmente.||
| Bajo      | Alerta personalizada: el número de cargas de archivos está fuera del intervalo permitido. | IoT Hub     | La cantidad de cargas de archivos dentro de un período de tiempo específico está fuera del intervalo configurado y permitido actualmente.| |
| Bajo      | Alerta personalizada: el número de mensajes de la nube al dispositivo en el protocolo HTTP está fuera del intervalo permitido. | IoT Hub     | La cantidad de mensajes de la nube al dispositivo (protocolo HTTP) en un período de tiempo no está en el intervalo permitido configurado                                  |
| Bajo      | Alerta personalizada: el número de mensajes de la nube al dispositivo rechazados en el protocolo HTTP no se encuentra en el intervalo permitido. | IoT Hub     | La cantidad de mensajes de nube a dispositivo (protocolo HTTP) dentro de un período de tiempo específico está fuera del intervalo configurado y permitido actualmente. |
| Bajo      | Alerta personalizada: el número de mensajes del dispositivo a la nube en el protocolo HTTP está fuera del intervalo permitido. | IoT Hub| La cantidad de mensajes de dispositivo a nube (protocolo HTTP) dentro de un período de tiempo específico está fuera del intervalo configurado y permitido actualmente.|    |
| Bajo      | Alerta personalizada: el número de mensajes de la nube al dispositivo en el protocolo MQTT está fuera del intervalo permitido. | IoT Hub     | La cantidad de mensajes de nube a dispositivo (protocolo MQTT) dentro de un período de tiempo específico está fuera del intervalo configurado y permitido actualmente.|   |
| Bajo      | Alerta personalizada: el número de mensajes de la nube al dispositivo rechazados en el protocolo MQTT está fuera del intervalo permitido. | IoT Hub     | La cantidad de mensajes de nube a dispositivo (protocolo MQTT) rechazados por el dispositivo dentro de un período de tiempo específico está fuera del intervalo configurado y permitido actualmente. |
| Bajo      | Alerta personalizada: el número de mensajes del dispositivo a la nube en el protocolo MQTT está fuera del intervalo permitido.          | IoT Hub     | La cantidad de mensajes de dispositivo a nube (protocolo MQTT) dentro de un período de tiempo específico está fuera del intervalo configurado y permitido actualmente.|
| Bajo      | Alerta personalizada: el número de purgas de la cola de comandos está fuera del intervalo permitido.                               | IoT Hub     | La cantidad de purgas de la cola de comandos dentro de un período de tiempo específico está fuera del intervalo configurado y permitido actualmente.||
| Bajo      | Alerta personalizada: el número de actualizaciones de módulos gemelos está fuera del intervalo permitido.                                       | IoT Hub     | La cantidad de actualizaciones de módulos gemelos dentro de un período de tiempo específico está fuera del intervalo configurado y permitido actualmente.|
| Bajo      | Alerta personalizada: el número de operaciones sin autorizar está fuera del intervalo permitido.  | IoT Hub     | La cantidad de operaciones sin autorizar dentro de un período de tiempo específico está fuera del intervalo configurado y permitido actualmente.|
| Bajo      | Alerta personalizada: el número de conexiones activas está fuera del intervalo permitido.  | Agente       | El número de conexiones activas dentro de un período de tiempo específico está fuera del intervalo configurado y permitido actualmente.|  Investigue los registros del dispositivo. Averigüe dónde se originó la conexión y determine si es inofensiva o malintencionada. Si es malintencionada, quite el posible malware y reconozca el origen. Si es inofensiva, agregue el origen a la lista de conexiones permitidas.  |
| Bajo      | Alerta personalizada: se ha creado una conexión saliente a una dirección IP que no está permitida.                             | Agente       | Se ha creado una conexión saliente a una dirección IP que está fuera de la lista de direcciones IP permitidas. |Investigue los registros del dispositivo. Averigüe dónde se originó la conexión y determine si es inofensiva o malintencionada. Si es malintencionada, quite el posible malware y reconozca el origen. Si es inofensiva, agregue el origen a la lista de direcciones IP permitidas.                        |
| Bajo      | Alerta personalizada: el número de inicios de sesión locales con errores está fuera del intervalo permitido.                               | Agente       | La cantidad de inicios de sesión locales con errores dentro de un período de tiempo específico está fuera del intervalo configurado y permitido actualmente. |   |
| Bajo      | Alerta personalizada: inicio de sesión de un usuario que no está en la lista de usuarios permitidos. | Agente       | Un usuario local no incluido en la lista de usuarios permitidos ha iniciado sesión en el dispositivo.|  Si va a guardar datos sin procesar, vaya a la cuenta de Log Analytics y use los datos para investigar el dispositivo, identifique el origen y, a continuación, corrija la lista de permitidos o bloqueados para esos valores. Si no va a guardar datos sin procesar, vaya al dispositivo y corrija la lista de permitidos o bloqueados para esos valores.|
| Bajo      | Alerta personalizada: se ha ejecutado un proceso no permitido. | Agente       | Se ha ejecutado en el dispositivo un proceso que no se permite. |Si va a guardar datos sin procesar, vaya a la cuenta de Log Analytics y use los datos para investigar el dispositivo, identifique el origen y, a continuación, corrija la lista de permitidos o bloqueados para esos valores. Si no va a guardar datos sin procesar, vaya al dispositivo y corrija la lista de permitidos o bloqueados para esos valores.  |
|


## <a name="next-steps"></a>Pasos siguientes

Pase al siguiente artículo para aprender a implementar a un agente de seguridad...

> [!div class="nextstepaction"]
> [Implementación de un agente de seguridad](how-to-deploy-agent.md)
