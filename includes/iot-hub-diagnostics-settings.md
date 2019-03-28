---
title: archivo de inclusión
description: archivo de inclusión
services: iot-hub
author: dominicbetts
ms.service: iot-hub
ms.topic: include
ms.date: 02/20/2019
ms.author: dobett
ms.custom: include file
ms.openlocfilehash: dd5dc53311c8611a4ca4d174401bba797fe5c4b1
ms.sourcegitcommit: f24fdd1ab23927c73595c960d8a26a74e1d12f5d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2019
ms.locfileid: "58505838"
---
### <a name="enable-logging-with-diagnostics-settings"></a>Habilitación del registro con la configuración de diagnóstico

[!INCLUDE [updated-for-az](./updated-for-az.md)]

1. Inicie sesión en [Azure Portal](https://portal.azure.com) y vaya a IoT Hub.

2. Seleccione **Configuración de diagnóstico**.

3. Seleccione **Activar diagnósticos**.

   ![Activación del diagnóstico](./media/iot-hub-diagnostics-settings/turnondiagnostics.png)

4. Asigne un nombre a la configuración de diagnóstico.

5. Elija dónde desea enviar los registros. También puede seleccionar cualquier combinación de las tres opciones:

   * Archivar en una cuenta de almacenamiento
   * Transmitir a un centro de eventos
   * Enviar a Log Analytics

6. Elija qué operaciones desea supervisar y habilite los registros para esas operaciones. Las operaciones sobre las que puede informar la configuración de diagnóstico son:

   * Conexiones
   * Telemetría de dispositivo
   * Mensajes de nube a dispositivo
   * Operaciones de identidad de dispositivos
   * Cargas de archivos
   * Enrutamiento de mensajes
   * Operaciones gemelas de la nube al dispositivo
   * Operaciones gemelas del dispositivo a la nube
   * Operaciones gemelas
   * Operaciones de trabajos
   * Métodos directos  
   * Seguimiento distribuido (versión preliminar)
   * Configurations
   * Secuencias de dispositivo
   * Métricas del dispositivo

6. Guarde la configuración nueva. 

Si desea activar la configuración de diagnóstico con PowerShell, utilice el código siguiente:

```azurepowershell
Connect-AzAccount
Select-AzSubscription -SubscriptionName <subscription that includes your IoT Hub>
Set-AzDiagnosticSetting -ResourceId <your resource Id> -ServiceBusRuleId <your service bus rule Id> -Enabled $true
```

La nueva configuración surte efecto en unos 10 minutos. Después, los registros aparecen en el destino de archivo configurado, en la hoja **Configuración de diagnóstico**. Para más información sobre la configuración de diagnósticos, consulte [Recopilación y uso de los datos de registro provenientes de los recursos de Azure](../articles/azure-monitor/platform/diagnostic-logs-overview.md).
