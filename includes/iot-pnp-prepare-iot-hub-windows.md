---
author: baanders
ms.author: baanders
ms.service: iot-pnp
ms.topic: include
ms.date: 10/24/2019
ms.openlocfilehash: 6b6c481a7443953a70fe3db45121f5f9982d8710
ms.sourcegitcommit: 5cfe977783f02cd045023a1645ac42b8d82223bd
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/17/2019
ms.locfileid: "74152036"
---
## <a name="prepare-an-iot-hub"></a>Preparación de un centro de IoT

También necesitará un centro de IoT de Azure en la suscripción de Azure para completar este inicio rápido. Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar. Si no tiene una instancia de IoT Hub, siga [estas instrucciones para crear una](../articles/iot-hub/iot-hub-create-using-cli.md).

> [!IMPORTANT]
> Durante la versión preliminar pública, las características de IoT Plug and Play solo están disponibles en los centros de IoT creados en las regiones **Centro de EE. UU.** , **Norte de Europa** y **Este de Japón**.

Ejecute el siguiente comando para agregar la Extensión de IoT de Microsoft Azure para CLI de Azure a la instancia de Cloud Shell:

```azurecli-interactive
az extension add --name azure-cli-iot-ext
```

Ejecute el siguiente comando la nueva identidad del dispositivo en su centro de IoT. Reemplace los marcadores de posición **YourIoTHubName** y **YourDeviceID** por su propio _nombre de IoT Hub_ y un _identificador de dispositivo_ de su elección.

```azurecli-interactive
az iot hub device-identity create --hub-name <YourIoTHubName> --device-id <YourDeviceID>
```

Ejecute el siguiente comando para obtener la _cadena de conexión del dispositivo_ que acaba de registrar (anótela para usarla más adelante):

```azurecli-interactive
az iot hub device-identity show-connection-string --hub-name <YourIoTHubName> --device-id <YourDevice> --output table
```

Ejecute el siguiente comando para obtener la _cadena de conexión de IoT Hub_ para el centro (anótela para usarla más adelante):

```azurecli-interactive
az iot hub show-connection-string --hub-name <YourIoTHubName> --output table
```