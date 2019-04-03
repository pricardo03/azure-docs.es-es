---
title: Configurar Azure Security Center para la versión preliminar del agente de IoT | Microsoft Docs
description: Obtenga información sobre cómo configurar a los agentes para su uso con Azure Security Center para IoT.
services: asc-for-iot
ms.service: ascforiot
documentationcenter: na
author: mlottner
manager: barbkess
editor: ''
ms.assetid: f95c445a-4f0d-4198-9c6c-d01446473bd0
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/26/2019
ms.author: mlottner
ms.openlocfilehash: 1e212e752309fd8347836d462a3394da2fca4a15
ms.sourcegitcommit: d83fa82d6fec451c0cb957a76cfba8d072b72f4f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/02/2019
ms.locfileid: "58862784"
---
# <a name="tutorial-configure-security-agents"></a>Tutorial: Configuración de agentes de seguridad

> [!IMPORTANT]
> Azure Security Center para IoT está actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin contrato de nivel de servicio y no es aconsejable usarla para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Este artículo se explica con Azure Security Center (ASC) para el agente de seguridad de IoT, cómo cambiar ellos configuración ASC para los agentes de seguridad de IoT.

> [!div class="checklist"]
> * Configuración de agentes de seguridad
> * Cambiar el comportamiento del agente mediante la edición de propiedades gemelas
> * Detectar la configuración predeterminada

## <a name="agents"></a>Agentes

ASC para los agentes de seguridad de IoT recopilan datos desde dispositivos IoT y realizar acciones de seguridad para mitigar las vulnerabilidades detectadas. Configuración de seguridad del agente es puede controlar mediante un conjunto de propiedades del módulo gemelo que se puede personalizar. En general, las actualizaciones secundarias de estas propiedades son poco frecuentes.  

ASC para el objeto de configuración de gemelo de IoT seguridad del agente es un objeto con formato JSON. El objeto de configuración es un conjunto de propiedades puede controlables que se pueden definir para controlar el comportamiento del agente. 

Estas configuraciones le ayuda a personalizar al agente para cada escenario requerido. Por ejemplo, automáticamente excluyendo algunos eventos, o mantener el consumo de energía a un nivel mínimo son posibles mediante la configuración de estas propiedades.  

Utilice la ASC para la configuración de agente de seguridad de IoT [esquema](https://aka.ms/iot-security-github-module-schema) para realizar cambios.  

## <a name="configuration-objects"></a>Objetos de configuración 

Cada ASC para el agente de IoT de seguridad relacionados con la propiedad se encuentra en el objeto de configuración del agente, en la sección de propiedades deseadas de los **azureiotsecurity** módulo. 

Para modificar la configuración, crear y modificar este objeto dentro de la **azureiotsecurity** identidad de módulo gemelo. 

Si el objeto de configuración de agente no existe en el **azureiotsecurity** módulo gemelo, todos los valores de propiedad de agente de seguridad se establecen en la configuración predeterminada. 

```json
"desired": {
  "azureiot*com^securityAgentConfiguration^1*0*0": {
  } 
}
```

No olvide validar los cambios de configuración de agente contra esto [esquema](https://aka.ms/iot-security-github-module-schema).
El agente no se iniciará si el objeto de configuración no coincide con el esquema.

## <a name="configuration-schema-and-validation"></a>Esquema de configuración y validación 

Asegúrese de validar la configuración del agente contra esto [esquema](https://aka.ms/iot-security-github-module-schema). Un agente no se iniciará si el objeto de configuración no coincide con el esquema.

 
Si, mientras se ejecuta el agente, el objeto de configuración se cambia a una configuración no válida (la configuración no coincide con el esquema), el agente pasará por alto la configuración no válida y continuará con la configuración actual. 

## <a name="editing-a-property"></a>Editar una propiedad 

Se deben establecer todas las propiedades personalizadas dentro del objeto de configuración de agente dentro de la **azureiotsecurity** módulo gemelo.
Para usar un valor de propiedad predeterminado, quite la propiedad del objeto de configuración.

### <a name="setting-a-property"></a>Establecer una propiedad

1. En su centro de IoT, busque y seleccione el dispositivo que desee cambiar.

1. Haga clic en el dispositivo y, a continuación, en **azureiotsecurity** módulo.

1. Haga clic en **identidad de módulo gemelo**.

1. Editar las propiedades deseadas del módulo de seguridad.
   
   Por ejemplo, para configurar eventos de conexión como de prioridad alta y recopilar eventos de prioridad alta cada 7 minutos, utilice la siguiente configuración.
   
   ```json
    "desired": {
      "azureiot*com^securityAgentConfiguration^1*0*0": {
        "highPriorityMessageFrequency": "PT7M",    
        "eventPriorityConnectionCreate": "High" 
      } 
    }, 
    ```

1. Haga clic en **Save**(Guardar).

### <a name="using-a-default-value"></a>Uso de un valor predeterminado

Para usar un valor de propiedad predeterminado, quite la propiedad del objeto de configuración.

## <a name="default-properties"></a>Propiedades predeterminadas 

En la tabla siguiente contiene las propiedades controlables de ASC para los agentes de seguridad de IoT.

Los valores predeterminados están disponibles en el esquema apropiado en [Github](https://aka.ms/iot-security-module-default).

| NOMBRE| Status | Valores válidos| Valores predeterminados| DESCRIPCIÓN |
|----------|------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------|---------------|
|highPriorityMessageFrequency|Requerido: false |Valores válidos:  Duración en formato ISO 8601 |Valor predeterminado: PT7M |Tiempo máximo antes que los mensajes de prioridad alta se envían.|
|lowPriorityMessageFrequency |Requerido: false|Valores válidos:  Duración en formato ISO 8601 |Valor predeterminado: PT5H |Tiempo máximo antes que los mensajes de prioridad baja se envían.| 
|snapshotFrequency |Requerir: false|Valores: duración válida en formato ISO 8601 |Valor predeterminado PT13H |Intervalo de tiempo para la creación de instantáneas del estado de dispositivo.| 
|maxLocalCacheSizeInBytes |Requerido: false |Valores válidos:  |Valor predeterminado: 2560000, mayor que 8192 | Almacenamiento máximo (en bytes) permitido para la caché de mensajes de un agente. Cantidad máxima de espacio permitido para almacenar mensajes en el dispositivo, antes de que se envían los mensajes.| 
|maxMessageSizeInBytes |Requerido: false |Valores válidos:  Un número positivo, mayor que 8192, 262144 menor que |Valor predeterminado: 204800 |Máximo tamaño permitido de un agente de mensaje en la nube. Esta configuración controla la cantidad de datos máximos que se envían en cada mensaje. |
|eventPriority${EventName} |Requerido: false |Valores válidos:  Máximo, mínimo, desactivado |Valores predeterminados: |Prioridad de cada agente generó el evento | 

### <a name="supported-security-events"></a>Eventos de seguridad admitidos

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
 

## <a name="next-steps"></a>Pasos siguientes

- [Descripción de ASC de recomendaciones de IoT](concept-recommendations.md)
- [Explore ASC para alertas de IoT](concept-security-alerts.md)
- [Acceso a datos de seguridad sin procesar](how-to-security-data-access.md)
