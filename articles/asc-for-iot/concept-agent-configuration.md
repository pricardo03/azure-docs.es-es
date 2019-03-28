---
title: Configurar un ASC para vista previa del agente de IoT | Microsoft Docs
description: Obtenga información sobre cómo configurar a los agentes para su uso con ASC para IoT.
services: ascforiot
documentationcenter: na
author: mlottner
manager: barbkess
editor: ''
ms.assetid: f95c445a-4f0d-4198-9c6c-d01446473bd0
ms.service: ascforiot
ms.devlang: na
ms.topic: concept
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/26/2019
ms.author: mlottner
ms.openlocfilehash: 3a3806c73ef254abab91bd28cd8761917371235f
ms.sourcegitcommit: cf971fe82e9ee70db9209bb196ddf36614d39d10
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2019
ms.locfileid: "58541923"
---
# <a name="configure-security-agents"></a>Configurar a los agentes de seguridad

> [!IMPORTANT]
> ASC para IoT está actualmente en versión preliminar pública.
> Esta versión preliminar se proporciona sin un contrato de nivel de servicio y no se recomienda para las cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

En este artículo se proporciona una explicación de cómo configurar a un agente para su uso con ASC para IoT.

## <a name="agents"></a>Agentes

ASC para los agentes de seguridad de IoT recopilan datos desde dispositivos IoT y realizar acciones de seguridad para mitigar las vulnerabilidades detectadas. Configuración de seguridad del agente es puede controlar mediante un conjunto de propiedades del módulo gemelo que se puede personalizar. En general, las actualizaciones secundarias de estas propiedades son poco frecuentes.  

ASC para el objeto de configuración de gemelo de IoT seguridad del agente es un objeto con formato JSON. El objeto de configuración es un conjunto de propiedades puede controlables que se pueden definir para controlar el comportamiento del agente. 

Estas configuraciones le ayuda a personalizar al agente para cada escenario requerido. Por ejemplo, automáticamente excluyendo algunos eventos, o mantener el consumo de energía a un nivel mínimo son posibles mediante la configuración de estas propiedades.  

Utilice la ASC para la configuración de agente de seguridad de IoT [esquema](https://github.com/azure/asc-for-iot-schemas/security/module/twin) para realizar cambios.  

## <a name="configuration-objects"></a>Objetos de configuración 

Cada ASC para el agente de seguridad relacionados con la propiedad se encuentra dentro del objeto de configuración del agente, en la sección de propiedades deseadas del módulo azureiotsecurity de IoT. 

Para modificar la configuración, crear y modificar este objeto dentro de la identidad de azureiotsecurity módulo gemelo. Si el objeto de configuración de agente no existe en el módulo gemelo de azureiotsecurity, todos los valores de propiedad de agente de seguridad se establecen en la configuración predeterminada. 

```json
"desired": { //azureiotsecurity Module Identity Twin – desired properties section  
  "azureiot*com^securityAgentConfiguration^1*0*0": { //Agent configuration object 
  … 
} 
}
```

No olvide validar los cambios de configuración de agente contra esto [esquema](https://aka.ms/iot-security-github-module-schema).
El agente no se iniciará si el objeto de configuración no coincide con el esquema.


## <a name="editing-a-property"></a>Editar una propiedad 

Todas las propiedades personalizadas deben establecerse en el objeto de configuración de agente dentro del módulo gemelo de azureiotsecurity. 

Establecer una propiedad invalida el valor predeterminado. Para establecer una propiedad, agregue la clave de propiedad para el objeto de configuración con el valor deseado. 

Para usar un valor de propiedad predeterminado, quite la propiedad del objeto de configuración. 

```json
"desired": { //azureiotsecurity Module Identity Twin – desired properties section  
  "azureiot*com^securityAgentConfiguration^1*0*0": { //ASC for IoT Agent 
      // configuration section  
    "lowPriorityMessageFrequency": "PT1H",     
    "highPriorityMessageFrequency": "PT7M",    
    "eventPriorityFirewallConfiguration": "High",     
    "eventPriorityConnectionCreate": "High" 
  } 
}, 
```

## <a name="default-properties"></a>Propiedades predeterminadas 
Conjunto de propiedades puede controlables que controlan los procesos de ASC para los agentes de seguridad de IoT.

Los valores predeterminados están disponibles en el esquema apropiado en [Github](https://aka.ms/iot-security-module-default).

| NOMBRE| Status | Valores válidos| Valores predeterminados| DESCRIPCIÓN |
|----------|------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------|---------------|
|highPriorityMessageFrequency|Requerido: false |Valores válidos:  Duración en formato ISO 8601 |Valor predeterminado: PT7M |Tiempo máximo antes que los mensajes de prioridad alta se envían.|
|lowPriorityMessageFrequency |Requerido: false|Valores válidos:  Duración en formato ISO 8601 |Valor predeterminado: PT5H |Tiempo máximo antes que los mensajes de prioridad baja se envían.| 
|snapshotFrequency |Requerir: false|Valores: duración válida en formato ISO 8601 |Valor predeterminado PT13H |Intervalo de tiempo para la creación de instantáneas del estado de dispositivo.| 
|maxLocalCacheSizeInBytes |Requerido: false |Valores válidos:  |Valor predeterminado: 2560000, mayor que 8192 | Almacenamiento máximo (en bytes) permitido para la caché de mensajes de un agente. Cantidad máxima de espacio permitido para almacenar mensajes en el dispositivo, antes de que se envían los mensajes.| 
|maxMessageSizeInBytes |Requerido: false |Valores válidos:  Un número positivo, mayor que 8192, 262144 menor que |Valor predeterminado: 204800 |Máximo tamaño permitido de un agente de mensaje en la nube. Esta configuración controla la cantidad de datos máximos que se envían en cada mensaje. |
|eventPriority${EventName} |Requerido: false |Valores válidos:  Máximo, mínimo, desactivado |Valores predeterminados: |Prioridad de cada agente generó el evento | 

### <a name="events"></a>Eventos

|Nombre del evento| PropertyName | Valor predeterminado| Evento de instantánea| Estado de los detalles  |
|----------|------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------|---------------|
|Evento de diagnóstico|eventPriorityDiagnostic| Off| False| Los eventos de diagnóstico relacionados con el agente. Use este evento para el registro detallado.| 
|Error de configuración |eventPriorityConfigurationError |Bajo |False |No se pudo analizar la configuración de agente. Compruebe la configuración en el esquema.| 
|Estadísticas de eventos descartados |eventPriorityDroppedEventsStatistics |Bajo |True|Estadísticas de eventos relacionados con el agente. |
|Estadísticas de mensajes|eventPriorityMessageStatistics |Bajo |True |Estadísticas de mensajes relacionados con el agente. |
|Hardware conectado|eventPriorityConnectedHardware |Bajo |True |Instantánea de todo el hardware conectado al dispositivo.|
|Puertos de escucha|eventPriorityListeningPorts |Alto |True |Instantánea de todos los puertos de escucha en el dispositivo.|
|Crear proceso |eventPriorityProcessCreate |Bajo |False |En el dispositivo de creación del proceso de las auditorías.|
|Terminar proceso|eventPriorityProcessTerminate |Bajo |False |Las auditorías de finalización en el dispositivo del proceso.| 
|Información del sistema |eventPrioritySystemInformation |Bajo |True |Una instantánea de la información del sistema (por ejemplo: Sistema operativo o CPU).| 
|Usuarios locales| eventPriorityLocalUsers |Alto |True|Una instantánea de los usuarios registrados locales dentro del sistema. |
|Inicio de sesión|  eventPriorityLogin |Alto|False|Auditar los eventos de inicio de sesión en el dispositivo (inicios de sesión locales y remotos).|
|Crear conexión |eventPriorityConnectionCreate|Bajo|False|Las auditorías de las conexiones TCP creadas hacia y desde el dispositivo. |
|Configuración del firewall| eventPriorityFirewallConfiguration|Bajo|True|Instantánea de configuración de firewall de dispositivo (reglas de firewall). |
|Línea base del SO| eventPriorityOSBaseline| Bajo|True|Compruebe la instantánea de línea base del sistema operativo de dispositivo.|
 

## <a name="see-also"></a>Vea también

- [Descripción de ASC de recomendaciones de IoT](concept-recommendations.md)
- [Explore ASC para alertas de IoT](concept-security-alerts.md)
- [Acceder a los datos sin procesar de seguridad](how-to-security-data-access.md)