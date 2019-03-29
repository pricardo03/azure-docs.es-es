---
title: Acceso a datos mediante procesos de ASC para la versión preliminar de IoT | Microsoft Docs
description: Obtenga información sobre cómo obtener acceso a los datos de alerta y la recomendación de seguridad al usar ASC para IoT.
services: ascforiot
documentationcenter: na
author: mlottner
manager: barbkess
editor: ''
ms.assetid: fbd96ddd-cd9f-48ae-836a-42aa86ca222d
ms.service: ascforiot
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/25/2019
ms.author: mlottner
ms.openlocfilehash: d81a8973772879f4f4b143701a1f4be3ecad95d9
ms.sourcegitcommit: c63fe69fd624752d04661f56d52ad9d8693e9d56
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2019
ms.locfileid: "58576646"
---
# <a name="access-your-security-data"></a>Obtener acceso a los datos de seguridad 

> [!IMPORTANT]
> ASC para IoT está actualmente en versión preliminar pública.
> Esta versión preliminar se proporciona sin un contrato de nivel de servicio y no se recomienda para las cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

ASC para IoT almacena los datos sin procesar de seguridad, recomendaciones y alertas de seguridad (si decide guardarlo) en el área de trabajo de Log Analytics.

## <a name="log-analytics"></a>Log Analytics

Para configurar el área de trabajo de Log Analytics que se usa:

1. Abra el IoT Hub.
1. Haga clic en **seguridad**
2. Haga clic en **configuración**y cambiar la configuración de área de trabajo de Log Analytics.

Para obtener acceso a su área de trabajo de Log Analytics después de la configuración:

1. Elija una alerta o recomendación en ASC para IoT. 
2. Haga clic en **una investigación más minuciosa**, a continuación, haga clic en **para ver los dispositivos que tienen esta alerta haga clic aquí y ver la columna DeviceId**.

Para obtener más información sobre las consultas de datos de Log Analytics, consulte [empezar a trabajar con consultas en Log Analytics](https://docs.microsoft.com//azure/log-analytics/query-language/get-started-queries).

## <a name="security-alerts"></a>Alertas de seguridad

Las alertas de seguridad se almacenan en _AzureSecurityOfThings.SecurityAlert_ tabla en el área de trabajo de Log Analytics configurado para los procesos de ASC para solución de IoT.

Se ha proporcionado un número de consultas útiles que le ayudarán a empezar a explorar las alertas de seguridad.

### <a name="sample-records"></a>Registros de ejemplo

Seleccione unos pocos registros aleatorios

```
// Select a few random records
//
SecurityAlert
| project 
    TimeGenerated, 
    IoTHubId=ResourceId, 
    DeviceId=tostring(parse_json(ExtendedProperties)["DeviceId"]),
    AlertSeverity, 
    DisplayName,
    Description,
    ExtendedProperties
| take 3
```

| TimeGenerated           | IoTHubId                                                                                                       | deviceId      | AlertSeverity | DisplayName                           | DESCRIPCIÓN                                             | ExtendedProperties                                                                                                                                                             |
|-------------------------|----------------------------------------------------------------------------------------------------------------|---------------|---------------|---------------------------------------|---------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 2018-11-18T18:10:29.000 | /subscriptions/<subscription_id>/resourceGroups/<resource_group>/providers/Microsoft.Devices/IotHubs/<iot_hub> | <device_name> | Alto          | Ataque por fuerza bruta se realizó correctamente           | Un ataque de fuerza bruta en el dispositivo fue correcta        |    {"Dirección de origen completo": "[\"10.165.12.18:\"]", "Los nombres de usuario": "[\"\"]", "DeviceId": "IoT-dispositivo-Linux"}                                                                       |
| 2018-11-19T12:40:31.000 | /subscriptions/<subscription_id>/resourceGroups/<resource_group>/providers/Microsoft.Devices/IotHubs/<iot_hub> | <device_name> | Alto          | Inicio de sesión local correcto en el dispositivo      | Se detectó un inicio de sesión local correcto al dispositivo     | {"Dirección remota": "?", "Puerto remoto": "", "Puerto Local": "", "Inicio de sesión de Shell": "/ bin/su", "Id. de proceso de inicio de sesión": "28207", "User Name": "atacante", "DeviceId": "IoT-dispositivo-Linux"} |
| 2018-11-19T12:40:31.000 | /subscriptions/<subscription_id>/resourceGroups/<resource_group>/providers/Microsoft.Devices/IotHubs/<iot_hub> | <device_name> | Alto          | Error del intento de inicio de sesión local en el dispositivo  | Se detectó un intento de inicio de sesión local en el dispositivo |  {"Dirección remota": "?", "Puerto remoto": "", "Puerto Local": "", "Inicio de sesión de Shell": "/ bin/su", "Id. de proceso de inicio de sesión": "22644", "User Name": "atacante", "DeviceId": "IoT-dispositivo-Linux"} |

### <a name="device-summary"></a>Resumen del dispositivo

Seleccione el número de alertas de seguridad distinto ha detectado la semana pasada por IoT Hub, dispositivo, gravedad de alerta, tipo de alerta.

```
// Select number of distinct security alerts detected last week by 
//   IoT hub, device, alert severity, alert type
//
SecurityAlert
| where TimeGenerated > ago(7d)
| summarize Cnt=dcount(SystemAlertId) by
    IoTHubId=ResourceId, 
    DeviceId=tostring(parse_json(ExtendedProperties)["DeviceId"]),
    AlertSeverity,
    DisplayName
```

| IoTHubId                                                                                                       | deviceId      | AlertSeverity | DisplayName                           | Número |
|----------------------------------------------------------------------------------------------------------------|---------------|---------------|---------------------------------------|-----|
| /subscriptions/<subscription_id>/resourceGroups/<resource_group>/providers/Microsoft.Devices/IotHubs/<iot_hub> | <device_name> | Alto          | Ataque por fuerza bruta se realizó correctamente           | 9   |   
| /subscriptions/<subscription_id>/resourceGroups/<resource_group>/providers/Microsoft.Devices/IotHubs/<iot_hub> | <device_name> | Mediano        | Error del intento de inicio de sesión local en el dispositivo  | 242 |    
| /subscriptions/<subscription_id>/resourceGroups/<resource_group>/providers/Microsoft.Devices/IotHubs/<iot_hub> | <device_name> | Alto          | Inicio de sesión local correcto en el dispositivo      | 31  |
| /subscriptions/<subscription_id>/resourceGroups/<resource_group>/providers/Microsoft.Devices/IotHubs/<iot_hub> | <device_name> | Mediano        | Moneda Crypto Miner                     | 4   |

### <a name="iot-hub-summary"></a>Resumen de IoT hub

Seleccione un número de distintos dispositivos que tenían las alertas en la última semana, por IoT Hub, la gravedad de alerta, tipo de alerta

```
// Select number of distinct devices which had alerts in the last week, by 
//   IoT hub, alert severity, alert type
//
SecurityAlert
| where TimeGenerated > ago(7d)
| extend DeviceId=tostring(parse_json(ExtendedProperties)["DeviceId"])
| summarize CntDevices=dcount(DeviceId) by
    IoTHubId=ResourceId, 
    AlertSeverity,
    DisplayName
```

| IoTHubId                                                                                                       | AlertSeverity | DisplayName                           | CntDevices |
|----------------------------------------------------------------------------------------------------------------|---------------|---------------------------------------|------------|
| /subscriptions/<subscription_id>/resourceGroups/<resource_group>/providers/Microsoft.Devices/IotHubs/<iot_hub> | Alto          | Ataque por fuerza bruta se realizó correctamente           | 1          |    
| /subscriptions/<subscription_id>/resourceGroups/<resource_group>/providers/Microsoft.Devices/IotHubs/<iot_hub> | Mediano        | Error del intento de inicio de sesión local en el dispositivo  | 1          | 
| /subscriptions/<subscription_id>/resourceGroups/<resource_group>/providers/Microsoft.Devices/IotHubs/<iot_hub> | Alto          | Inicio de sesión local correcto en el dispositivo      | 1          |
| /subscriptions/<subscription_id>/resourceGroups/<resource_group>/providers/Microsoft.Devices/IotHubs/<iot_hub> | Mediano        | Moneda Crypto Miner                     | 1          |

## <a name="security-recommendations"></a>Recomendaciones de seguridad

Recomendaciones de seguridad se almacenan en _AzureSecurityOfThings.SecurityRecommendation_ tabla en el área de trabajo de Log Analytics configurado para los procesos de ASC para solución de IoT.

Hemos proporcionado un número de consultas útiles que le ayudarán a empezar a explorar las recomendaciones de seguridad.

### <a name="sample-records"></a>Registros de ejemplo

Seleccione unos pocos registros aleatorios

```
// Select a few random records
//
SecurityRecommendation
| project 
    TimeGenerated, 
    IoTHubId=AssessedResourceId, 
    DeviceId,
    RecommendationSeverity,
    RecommendationState,
    RecommendationDisplayName,
    Description,
    RecommendationAdditionalData
| take 2
```
    
| TimeGenerated | IoTHubId | deviceId | RecommendationSeverity | RecommendationState | RecommendationDisplayName | DESCRIPCIÓN | RecommendationAdditionalData |
|---------------|----------|----------|------------------------|---------------------|---------------------------|-------------|------------------------------|
| 2019-03-22T10:21:06.060 | /subscriptions/<subscription_id>/resourceGroups/<resource_group>/providers/Microsoft.Devices/IotHubs/<iot_hub> | <device_name> | Mediano | Active | Se encontró la regla de firewall permisiva en la cadena de entrada | Se ha encontrado una regla en el firewall que contiene un patrón permisivo para una amplia variedad de puertos o direcciones IP. | {"Rules":"[{\"SourceAddress\":\"\",\"SourcePort\":\"\",\"DestinationAddress\":\"\",\"DestinationPort\":\"1337\"}]"} |
| 2019-03-22T10:50:27.237 | /subscriptions/<subscription_id>/resourceGroups/<resource_group>/providers/Microsoft.Devices/IotHubs/<iot_hub> | <device_name> | Mediano | Active | Se encontró la regla de firewall permisiva en la cadena de entrada | Se ha encontrado una regla en el firewall que contiene un patrón permisivo para una amplia variedad de puertos o direcciones IP. | {"Rules":"[{\"SourceAddress\":\"\",\"SourcePort\":\"\",\"DestinationAddress\":\"\",\"DestinationPort\":\"1337\"}]"} |

### <a name="device-summary"></a>Resumen del dispositivo

Seleccione el número de recomendaciones de seguridad activa distintos por IoT Hub, dispositivo, tipo y gravedad de la recomendación.

```
// Select number of distinct active security recommendations by 
//   IoT hub, device, recommendation severity and type
//
SecurityRecommendation
| extend IoTHubId=AssessedResourceId
| summarize CurrentState=arg_max(RecommendationState, DiscoveredTimeUTC) by IoTHubId, DeviceId, RecommendationSeverity, RecommendationDisplayName
| where CurrentState == "Active"
| summarize Cnt=count() by IoTHubId, DeviceId, RecommendationSeverity
```

| IoTHubId                                                                                                       | deviceId      | RecommendationSeverity | Número |
|----------------------------------------------------------------------------------------------------------------|---------------|------------------------|-----|
| /subscriptions/<subscription_id>/resourceGroups/<resource_group>/providers/Microsoft.Devices/IotHubs/<iot_hub> | <device_name> | Alto          | 2   |    
| /subscriptions/<subscription_id>/resourceGroups/<resource_group>/providers/Microsoft.Devices/IotHubs/<iot_hub> | <device_name> | Mediano        | 1 |  
| /subscriptions/<subscription_id>/resourceGroups/<resource_group>/providers/Microsoft.Devices/IotHubs/<iot_hub> | <device_name> | Alto          | 1  |
| /subscriptions/<subscription_id>/resourceGroups/<resource_group>/providers/Microsoft.Devices/IotHubs/<iot_hub> | <device_name> | Mediano        | 4   |


## <a name="next-steps"></a>Pasos siguientes

- Leer los procesos de ASC para IoT [información general](overview.md)
- Obtenga información acerca de ASC para IoT [arquitectura](architecture.md)
- Conocer y explorar [ASC para las alertas de IoT](concept-security-alerts.md)
- Conocer y explorar [ASC para la recomendación de IoT](concept-recommendations.md)