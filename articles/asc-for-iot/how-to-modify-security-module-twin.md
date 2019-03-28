---
title: Para obtener instrucciones para modificar un ASC para el módulo gemelo de IoT en ASC para la versión preliminar de IoT | Microsoft Docs
description: Obtenga información sobre cómo modificar un ASC para el gemelo de módulo de seguridad de IoT de ASC para IoT.
services: ascforiot
documentationcenter: na
author: mlottner
manager: barbkess
editor: ''
ms.assetid: 1bc5dc86-0f33-4625-b3d3-f9b6c1a54e14
ms.service: ascforiot
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/25/2019
ms.author: mlottner
ms.openlocfilehash: 4bb25db7feddea66e75d9dd069aaec7785faf738
ms.sourcegitcommit: cf971fe82e9ee70db9209bb196ddf36614d39d10
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2019
ms.locfileid: "58541953"
---
# <a name="modify-an-asc-for-iot-module-twin"></a>Modificar un ASC para el módulo gemelo de IoT

> [!IMPORTANT]
> ASC para IoT está actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

En este artículo se explica cómo modificar la configuración de una existente **AzureIoTSecurity módulo gemelo** para un dispositivo existente. 

Consulte [crear un ASC para el módulo de IoT](quickstart-create-security-twin.md) para obtener información sobre cómo crear un nuevo módulo de seguridad para un dispositivo nuevo.  

## <a name="modification-considerations"></a>Consideraciones sobre la modificación

> [!IMPORTANT]
> Cada configuración en la configuración del gemelo invalida la configuración predeterminada. Si ya no está presente en la configuración del gemelo una configuración específica, se usa la configuración predeterminada. 

## <a name="configuration-schema-and-validation"></a>Esquema de configuración y validación 

Asegúrese de validar la configuración del agente contra esto [esquema](https://github.com/Azure/asc-for-iot/schema/security_module_twin). Un agente no se iniciará si el objeto de configuración no coincide con el esquema.

 
Si, mientras se ejecuta el agente, el objeto de configuración se cambia a una configuración no válida (la configuración no coincide con el esquema), el agente pasará por alto la configuración no válida y continuará con la configuración actual. 

## <a name="edit-a-property"></a>Editar una propiedad  

Establecer todas las propiedades personalizadas dentro del objeto de configuración de agente dentro del módulo gemelo de AzureIoTSecurity. 

Para establecer una propiedad, agregue la clave de propiedad para el objeto de configuración con el valor deseado. Para usar un valor de propiedad predeterminado, quite la propiedad del objeto de configuración:

```json
"desired": { //AzureIoTSecurity Module Identity Twin – desired properties section  
  "azureiot*com^securityAgentConfiguration^1*0*0": { //ASC for IoT Agent 
      // configuration section  
    "hubResourceId": "/subscriptions/82392767-31d3-4bd2-883d-9b60596f5f42/resourceGroups/myResourceGroup/providers/Microsoft.Devices/IotHubs/myIotHub",     
    "lowPriorityMessageFrequency": "PT1H",     
    "highPriorityMessageFrequency": "PT7M",    
    "eventPriorityFirewallConfiguration": "High",     
    "eventPriorityConnectionCreate": "Off" 
  } 
}, 
```

## <a name="properties"></a>Properties (Propiedades) 
En la tabla siguiente contiene todas las propiedades configurables que controlan los procesos de ASC para agentes de IoT. 
          

| NOMBRE| Status | Valores válidos| Valores predeterminados| DESCRIPCIÓN |
|----------|------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------|---------------|
|highPriorityMessageFrequency|Requerido: false |Valores válidos:  Duración en formato ISO 8601 |Valor predeterminado: PT7M |Tiempo máximo antes que los mensajes de prioridad alta se envían.|
|lowPriorityMessageFrequency |Requerido: false|Valores válidos:  Duración en formato ISO 8601 |Valor predeterminado: PT5H |Tiempo máximo antes que los mensajes de prioridad baja se envían.| 
|snapshotFrequency |Requerir: false|Valores: duración válida en formato ISO 8601 |Valor predeterminado PT13H |Intervalo de tiempo para la creación de instantáneas del estado de dispositivo.| 
|maxLocalCacheSizeInBytes |Requerido: false |Valores válidos:  |Valor predeterminado: 2560000, mayor que 8192 | Almacenamiento máximo (en bytes) permitido para la caché de mensajes de un agente. Cantidad máxima de espacio permitido para almacenar mensajes en el dispositivo, antes de que se envían los mensajes.| 
|maxMessageSizeInBytes |Requerido: false |Valores válidos:  Un número positivo, mayor que 8192, 262144 menor que |Valor predeterminado: 204800 |Máximo tamaño permitido de un agente de mensaje en la nube. Esta configuración controla la cantidad de datos máximos que se envían en cada mensaje. |
|eventPriority${EventName} |Requerido: false |Valores válidos:  Máximo, mínimo, desactivado |Valores predeterminados: |Prioridad de cada agente genera el evento. | 
|

### <a name="events"></a>Eventos

La siguiente lista de eventos son todos los eventos de los procesos de ASC para el agente de IoT pueden recopilar de los dispositivos. Use el módulo gemelo de AzureIotSecurity para configurar cuál de estos eventos se recopilan y decidir su prioridad en la solución. 
 
|Nombre del evento| PropertyName | Valor predeterminado| Evento de instantánea| Estado detallado  |
|----------|------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------|---------------|
|Evento de diagnóstico|eventPriorityDiagnostic| Off| False| Los eventos de diagnóstico relacionados con el agente. Use este evento para el registro detallado.| 
Error de configuración |eventPriorityConfigurationError |Bajo |False |No se pudo analizar la configuración de agente. Compruebe la configuración en el esquema.| 
|Estadísticas de eventos descartados |eventPriorityDroppedEventsStatistics |Bajo |True|Estadísticas de eventos relacionados con el agente. |
|Estadísticas de mensajes|eventPriorityMessageStatistics |Bajo |True |Estadísticas de mensajes relacionados con el agente. |
|Hardware conectado|eventPriorityConnectedHardware |Bajo |True |Instantánea de todo el hardware conectado al dispositivo.|
|Puertos de escucha|eventPriorityListeningPorts |Alto |True |Instantánea de todos los puertos de escucha en el dispositivo.|
| Crear proceso |eventPriorityProcessCreate |Bajo |False |En el dispositivo de creación del proceso de las auditorías.|
| Terminar proceso|eventPriorityProcessTerminate |Bajo |False |Las auditorías de finalización en el dispositivo del proceso.| 
 Información del sistema |eventPrioritySystemInformation |Bajo |True |Una instantánea de la información del sistema como la CPU o del sistema operativo.|
|Usuarios locales| eventPriorityLocalUsers |Alto |True|Una instantánea de los usuarios registrados locales dentro del sistema. |
|Inicio de sesión|  eventPriorityLogin |Alto|False|Las auditorías de los eventos de inicio de sesión en el dispositivo (inicios de sesión locales y remotos).|
|Crear conexión |eventPriorityConnectionCreate|Bajo|False|Las auditorías de las conexiones TCP creadas hacia y desde el dispositivo. |
|Configuración del firewall| eventPriorityFirewallConfiguration|Bajo|True|Instantánea de configuración de firewall de dispositivo (reglas de firewall). |
|Línea base del SO| eventPriorityOSBaseline| Bajo|True|Compruebe la instantánea de línea base del sistema operativo de dispositivo.| 
|


## <a name="next-steps"></a>Pasos siguientes

- Leer los procesos de ASC para IoT [información general](overview.md)
- Obtenga información acerca de ASC para IoT [arquitectura](architecture.md)
- Conocer y explorar [ASC para las alertas de IoT](concept-security-alerts.md)
- Descubra cómo obtener acceso a su [datos de seguridad](how-to-security-data-access.md)
