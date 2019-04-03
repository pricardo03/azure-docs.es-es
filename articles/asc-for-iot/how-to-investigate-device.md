---
title: Azure Security Center para la Guía de investigación de dispositivo IoT Preview | Microsoft Docs
description: Esta guía explica cómo usar Azure Security Center para IoT para investigar un dispositivo de IoT sospechoso mediante Log Analytics.
services: asc-for-iot
ms.service: ascforiot
documentationcenter: na
author: mlottner
manager: barbkess
editor: ''
ms.assetid: b18b48ae-b445-48f8-9ac0-365d6e065b64
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/27/2019
ms.author: mlottner
ms.openlocfilehash: 56378e94bf448da4c3047c30be3ae25887f113b5
ms.sourcegitcommit: d83fa82d6fec451c0cb957a76cfba8d072b72f4f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/02/2019
ms.locfileid: "58862631"
---
# <a name="investigate-a-suspicious-iot-device"></a>Investigación de un dispositivo de IoT sospechoso

> [!IMPORTANT]
> Azure Security Center para IoT está actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin contrato de nivel de servicio y no es aconsejable usarla para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Azure Security Center (ASC) para la evidencia y las alertas del servicio IoT proporcionan indicaciones claras cuando se sospecha de dispositivos IoT de participación en actividades sospechosas o cuando las indicaciones existan que un dispositivo está en peligro. 

En esta guía, use las sugerencias de investigación proporcionadas para ayudar a determinar los riesgos potenciales para su organización, decida cómo corregir y descubrir las mejores formas para evitar ataques similar en el futuro.  

> [!div class="checklist"]
> * Busque los datos del dispositivo
> * Investigue el uso de consultas kql


## <a name="how-can-i-access-my-data"></a>¿Cómo puedo tener acceso a Mis datos?

De forma predeterminada, ASC for IoT almacena tanto las recomendaciones como las alertas de seguridad en su área de trabajo de Log Analytics. También puede elegir almacenar los datos de seguridad sin formato.

Para buscar el área de trabajo de Log Analytics para el almacenamiento de datos:

1. Abra IoT Hub. 
1. Haga clic en **Security** (Seguridad) y seleccione **Settings** (Configuración).
1. Cambie los detalles de la configuración del área de trabajo de Log Analytics. 
1. Haga clic en **Save**(Guardar). 

Después de la configuración, realice las siguientes operaciones para acceder a los datos almacenados en el área de trabajo de Log Analytics:

1. Seleccione una alerta de ASC for IoT en IoT Hub y haga clic en el ella. 
1. Haga clic en **Further investigation** (Investigación adicional). 
1. Seleccione **To see which devices have this alert click here and view the DeviceId column** (Para ver los dispositivos que tienen esta alerta haga clic aquí y vea la columna DeviceId).

## <a name="investigation-steps-for-suspicious-iot-devices"></a>Pasos de la investigación de dispositivos de IoT sospechosos

Para obtener acceso a información y datos sin procesar acerca de los dispositivos de IoT, vaya al área de trabajo de Log Analytics [para tener acceso a los datos](#how-can-i-access-my-data).

Comprobar e investigar los datos del dispositivo para la siguiente información y actividades mediante las siguientes consultas kql.

### <a name="related-alerts"></a>Alertas relacionadas

Para averiguar si se desencadenaron otras alertas aproximadamente a la misma hora, utilice la siguiente consulta kql:

  ~~~
  let device = "YOUR_DEVICE_ID";
  let hub = "YOUR_HUB_NAME";
  SecurityAlert
  | where ExtendedProperties contains device and ResourceId contains tolower(hub)
  | project TimeGenerated, AlertName, AlertSeverity, Description, ExtendedProperties
  ~~~

### <a name="users-with-access"></a>Usuarios con acceso

Para averiguar qué usuarios tienen acceso a este dispositivo, use la siguiente consulta kql: 

  ~~~
  let device = "YOUR_DEVICE_ID";
  let hub = "YOUR_HUB_NAME";
  SecurityIoTRawEvent
  | where
     DeviceId == device and AssociatedResourceId contains tolower(hub)
     and RawEventName == "LocalUsers"
  | project
     TimestampLocal=extractjson("$.TimestampLocal", EventDetails, typeof(datetime)),
     GroupNames=extractjson("$.GroupNames", EventDetails, typeof(string)),
     UserName=extractjson("$.UserName", EventDetails, typeof(string))
  | summarize FirstObserved=min(TimestampLocal) by GroupNames, UserName
  ~~~
Use estos datos para detectar: 
  1. Qué usuarios tienen acceso al dispositivo.
  2. ¿Tienen los usuarios con acceso a los niveles de permisos según lo previsto? 

### <a name="open-ports"></a>Abrir puertos

Para buscar fuera de los puertos en el dispositivo que están actualmente en uso o se han utilizado use la siguiente consulta kql: 

  ~~~
  let device = "YOUR_DEVICE_ID";
  let hub = "YOUR_HUB_NAME";
  SecurityIoTRawEvent
  | where
     DeviceId == device and AssociatedResourceId contains tolower(hub)
     and RawEventName == "ListeningPorts"
     and extractjson("$.LocalPort", EventDetails, typeof(int)) <= 1024 // avoid short-lived TCP ports (Ephemeral)
  | project
     TimestampLocal=extractjson("$.TimestampLocal", EventDetails, typeof(datetime)),
     Protocol=extractjson("$.Protocol", EventDetails, typeof(string)),
     LocalAddress=extractjson("$.LocalAddress", EventDetails, typeof(string)),
     LocalPort=extractjson("$.LocalPort", EventDetails, typeof(int)),
     RemoteAddress=extractjson("$.RemoteAddress", EventDetails, typeof(string)),
     RemotePort=extractjson("$.RemotePort", EventDetails, typeof(string))
  | summarize MinObservedTime=min(TimestampLocal), MaxObservedTime=max(TimestampLocal), AllowedRemoteIPAddress=makeset(RemoteAddress), AllowedRemotePort=makeset(RemotePort) by Protocol, LocalPort
  ~~~

    Use this data to discover:
  1. Qué sockets de escucha están actualmente activos en el dispositivo.
  2. ¿Deben permitirse los sockets de escucha que están activos actualmente?
  3. ¿Hay ninguna dirección remota sospechosa conectada al dispositivo?

### <a name="user-logins"></a>Inicios de sesión de usuario

Para averiguar los usuarios que ha iniciado sesión en el dispositivo use la siguiente consulta kql: 
 
  ~~~
  let device = "YOUR_DEVICE_ID";
  let hub = "YOUR_HUB_NAME";
  SecurityIoTRawEvent
  | where
     DeviceId == device and AssociatedResourceId contains tolower(hub)
     and RawEventName == "Login"
     // filter out local, invalid and failed logins
     and EventDetails contains "RemoteAddress"
     and EventDetails !contains '"RemoteAddress":"127.0.0.1"'
     and EventDetails !contains '"UserName":"(invalid user)"'
     and EventDetails !contains '"UserName":"(unknown user)"'
     //and EventDetails !contains '"Result":"Fail"'
  | project
     TimestampLocal=extractjson("$.TimestampLocal", EventDetails, typeof(datetime)),
     UserName=extractjson("$.UserName", EventDetails, typeof(string)),
     LoginHandler=extractjson("$.Executable", EventDetails, typeof(string)),
     RemoteAddress=extractjson("$.RemoteAddress", EventDetails, typeof(string)),
     Result=extractjson("$.Result", EventDetails, typeof(string))
  | summarize CntLoginAttempts=count(), MinObservedTime=min(TimestampLocal), MaxObservedTime=max(TimestampLocal), CntIPAddress=dcount(RemoteAddress), IPAddress=makeset(RemoteAddress) by UserName, Result, LoginHandler
  ~~~

    Use the query results to discover:
  1. Qué usuarios han iniciado sesión el dispositivo.
  2. ¿Se supone que los usuarios que ha iniciado sesión para iniciar sesión?
  3. Si los usuarios que han iniciado sesión se han conectado desde direcciones IP esperadas o inesperadas.
  
### <a name="process-list"></a>Lista de procesos

Para averiguar si la lista de procesos es la siguiente consulta kql como uso esperado: 

  ~~~
  let device = "YOUR_DEVICE_ID";
  let hub = "YOUR_HUB_NAME";
  SecurityIoTRawEvent
  | where
     DeviceId == device and AssociatedResourceId contains tolower(hub)
     and RawEventName == "ProcessCreate"
  | project
     TimestampLocal=extractjson("$.TimestampLocal", EventDetails, typeof(datetime)),
     Executable=extractjson("$.Executable", EventDetails, typeof(string)),
     UserId=extractjson("$.UserId", EventDetails, typeof(string)),
     CommandLine=extractjson("$.CommandLine", EventDetails, typeof(string))
  | join kind=leftouter (
     // user UserId details
     SecurityIoTRawEvent
     | where
        DeviceId == device and AssociatedResourceId contains tolower(hub)
        and RawEventName == "LocalUsers"
     | project
        UserId=extractjson("$.UserId", EventDetails, typeof(string)),
        UserName=extractjson("$.UserName", EventDetails, typeof(string))
     | distinct UserId, UserName
  ) on UserId
  | extend UserIdName = strcat("Id:", UserId, ", Name:", UserName)
  | summarize CntExecutions=count(), MinObservedTime=min(TimestampLocal), MaxObservedTime=max(TimestampLocal), ExecutingUsers=makeset(UserIdName), ExecutionCommandLines=makeset(CommandLine) by Executable
  ~~~

    Use the query results to discover:

  1. Si se ejecutó algún proceso en el dispositivo.
  2. Si los procesos los han ejecutado los usuarios adecuados.
  3. Si las ejecuciones de la línea de comandos contenían los argumentos correctos y esperados.

## <a name="next-steps"></a>Pasos siguientes
Tras investigar un dispositivo y conocer mejor los riesgos, es posible que desee considerar la posibilidad de [configurar alertas personalizadas](quickstart-create-custom-alerts.md) para mejorar la postura de seguridad de su solución de IoT. Si aún no tiene un agente de dispositivos, considere la posibilidad de [implementar un agente de seguridad](how-to-deploy-agent.md) o de [cambiar la configuración de un agente de dispositivos existente](how-to-agent-configuration.md) para mejorar los resultados. 