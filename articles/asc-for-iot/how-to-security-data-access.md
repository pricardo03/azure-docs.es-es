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
ms.openlocfilehash: e394f6025f7898aad7dde7b1acefd9f95029a554
ms.sourcegitcommit: cf971fe82e9ee70db9209bb196ddf36614d39d10
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2019
ms.locfileid: "58541998"
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

1. Elija una alerta en ASC para IoT. 
2. Haga clic en **una investigación más minuciosa**, a continuación, haga clic en **para ver los dispositivos que tienen esta alerta haga clic aquí y ver la columna DeviceId**.

Para obtener más información sobre las consultas de datos de Log Analytics, consulte [empezar a trabajar con consultas en Log Analytics](https://docs.microsoft.com//azure/log-analytics/query-language/get-started-queries).

## <a name="security-alerts"></a>Alertas de seguridad

Las alertas de seguridad se almacenan en el **ASCforIoT.SecurityAlert** tabla dentro de su área de trabajo de Log Analytics configurada.

Utilice las siguientes consultas kql básica para empezar a explorar las alertas de seguridad.

### <a name="sample-records-query"></a>Consulta de registros de ejemplo

Para seleccionar de forma aleatoria una serie de registros: 

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

#### <a name="sample-query-results"></a>Resultados de la consulta de ejemplo 

| TimeGenerated           | IoTHubId                                                                                                       | deviceId      | AlertSeverity | DisplayName                           | DESCRIPCIÓN                                             | ExtendedProperties                                                                                                                                                             |
|-------------------------|----------------------------------------------------------------------------------------------------------------|---------------|---------------|---------------------------------------|---------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 2018-11-18T18:10:29.000 | /subscriptions/<subscription_id>/resourceGroups/<resource_group>/providers/Microsoft.Devices/IotHubs/<iot_hub> | <device_name> | Alto          | Ataque por fuerza bruta se realizó correctamente           | Un ataque de fuerza bruta en el dispositivo fue correcta        |    {"Dirección de origen completo": "[\"10.165.12.18:\"]", "Los nombres de usuario": "[\"\"]", "DeviceId": "IoT-dispositivo-Linux"}                                                                       |
| 2018-11-19T12:40:31.000 | /subscriptions/<subscription_id>/resourceGroups/<resource_group>/providers/Microsoft.Devices/IotHubs/<iot_hub> | <device_name> | Alto          | Inicio de sesión local correcto en el dispositivo      | Se detectó un inicio de sesión local correcto al dispositivo     | {"Dirección remota": "?", "Puerto remoto": "", "Puerto Local": "", "Inicio de sesión de Shell": "/ bin/su", "Id. de proceso de inicio de sesión": "28207", "User Name": "atacante", "DeviceId": "IoT-dispositivo-Linux"} |
| 2018-11-19T12:40:31.000 | /subscriptions/<subscription_id>/resourceGroups/<resource_group>/providers/Microsoft.Devices/IotHubs/<iot_hub> | <device_name> | Alto          | Error del intento de inicio de sesión local en el dispositivo  | Se detectó un intento de inicio de sesión local en el dispositivo |  {"Dirección remota": "?", "Puerto remoto": "", "Puerto Local": "", "Inicio de sesión de Shell": "/ bin/su", "Id. de proceso de inicio de sesión": "22644", "User Name": "atacante", "DeviceId": "IoT-dispositivo-Linux"} |

### <a name="device-summary-query"></a>Consulta de resumen de dispositivos

Use esta consulta kql para seleccionar un número de alertas de seguridad distinto ha detectado la semana pasada por IoT Hub, dispositivo, gravedad de alerta, tipo de alerta.

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

#### <a name="device-summary-query-results"></a>Resultados de la consulta de resumen de dispositivo

| IoTHubId | deviceId| AlertSeverity| DisplayName | Número |
|----------|---------|------------------|---------|---------|
|/subscriptions/<subscription_id>/resourceGroups/<resource_group>/providers/Microsoft.Devices/IotHubs/<iot_hub> | <device_name> | Alto          | Ataque por fuerza bruta se realizó correctamente           | 9   |    
| /subscriptions/<subscription_id>/resourceGroups/<resource_group>/providers/Microsoft.Devices/IotHubs/<iot_hub> | <device_name> | Mediano        | Error del intento de inicio de sesión local en el dispositivo  | 242 |    
| /subscriptions/<subscription_id>/resourceGroups/<resource_group>/providers/Microsoft.Devices/IotHubs/<iot_hub> | <device_name> | Alto          | Inicio de sesión local correcto en el dispositivo      | 31  |
| /subscriptions/<subscription_id>/resourceGroups/<resource_group>/providers/Microsoft.Devices/IotHubs/<iot_hub> | <device_name> | Mediano        | Moneda Crypto Miner                     | 4   |
|

### <a name="iot-hub-summary"></a>Resumen de IoT Hub

Utilice esta consulta kql para seleccionar un número de distintos dispositivos que tenían las alertas en la última semana, por IoT hub, la gravedad de alerta, tipo de alerta:

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

#### <a name="iot-hub-summary-query-results"></a>Resultados de la consulta de resumen de IoT Hub

| IoTHubId                                                                                                       | AlertSeverity | DisplayName                           | CntDevices |
|----------------------------------------------------------------------------------------------------------------|---------------|---------------------------------------|------------|
| /subscriptions/<subscription_id>/resourceGroups/<resource_group>/providers/Microsoft.Devices/IotHubs/<iot_hub> | Alto          | Ataque por fuerza bruta se realizó correctamente           | 1          |    
| /subscriptions/<subscription_id>/resourceGroups/<resource_group>/providers/Microsoft.Devices/IotHubs/<iot_hub> | Mediano        | Error del intento de inicio de sesión local en el dispositivo  | 1          | 
| /subscriptions/<subscription_id>/resourceGroups/<resource_group>/providers/Microsoft.Devices/IotHubs/<iot_hub> | Alto          | Inicio de sesión local correcto en el dispositivo      | 1          |
| /subscriptions/<subscription_id>/resourceGroups/<resource_group>/providers/Microsoft.Devices/IotHubs/<iot_hub> | Mediano        | Moneda Crypto Miner                     | 1          |



## <a name="next-steps"></a>Pasos siguientes

- Leer los procesos de ASC para IoT [información general](overview.md)
- Obtenga información acerca de ASC para IoT [arquitectura](architecture.md)
- Conocer y explorar [ASC para las alertas de IoT](concept-security-alerts.md)
