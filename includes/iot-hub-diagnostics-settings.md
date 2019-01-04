---
title: archivo de inclusión
description: archivo de inclusión
services: iot-hub
author: dominicbetts
ms.service: iot-hub
ms.topic: include
ms.date: 05/17/2018
ms.author: dobett
ms.custom: include file
ms.openlocfilehash: e518263fc7f427ade4943f91fdc840b62a235205
ms.sourcegitcommit: 71ee622bdba6e24db4d7ce92107b1ef1a4fa2600
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/17/2018
ms.locfileid: "53550030"
---
### <a name="enable-logging-with-diagnostics-settings"></a>Habilitación del registro con la configuración de diagnóstico

1. Inicie sesión en [Azure Portal](https://portal.azure.com) y vaya a su instancia de IoT Hub.

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

6. Guarde la configuración nueva. 

Si desea activar la configuración de diagnóstico con PowerShell, utilice el código siguiente:

```azurepowershell
Connect-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName <subscription that includes your IoT Hub>
Set-AzureRmDiagnosticSetting -ResourceId <your resource Id> -ServiceBusRuleId <your service bus rule Id> -Enabled $true
```

La nueva configuración surte efecto en unos 10 minutos. Después, los registros aparecen en el destino de archivo configurado, en la hoja **Configuración de diagnóstico**. Para más información sobre la configuración de diagnósticos, consulte [Recopilación y uso de los datos de registro provenientes de los recursos de Azure](../articles/azure-monitor/platform/diagnostic-logs-overview.md).