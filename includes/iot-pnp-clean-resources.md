---
author: baanders
ms.author: baanders
ms.service: iot-pnp
ms.topic: include
ms.date: 11/15/2019
ms.openlocfilehash: e6ca830a7224326c17003924b76192b95ba6bcb2
ms.sourcegitcommit: 5cfe977783f02cd045023a1645ac42b8d82223bd
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/17/2019
ms.locfileid: "74152016"
---
## <a name="clean-up-resources"></a>Limpieza de recursos

Si planea leer artículos de IoT Plug and Play adicionales, puede conservar los recursos que usó en este inicio rápido y reutilizarlos. De lo contrario, puede eliminar los recursos que creó para este inicio rápido para evitar cargos adicionales.

Puede eliminar el centro y el dispositivo registrado a la vez si elimina todo el grupo de recursos con el siguiente comando de la CLI de Azure. (No obstante, no lo use si estos recursos comparten un grupo de recursos con otros recursos que tenga para otros fines).

```azurecli-interactive
az group delete --name <YourResourceGroupName>
```
Para eliminar solo la instancia de IoT Hub, ejecute el siguiente comando con la CLI de Azure:

```azurecli-interactive
az iot hub delete --name <YourIoTHubName>
```

Para eliminar solo la identidad del dispositivo que registró en IoT Hub, ejecute el comando siguiente mediante la CLI de Azure:

```azurecli-interactive
az iot hub device-identity delete --hub-name <YourIoTHubName> --device-id <YourDeviceID>
```

También puede que quiera quitar los archivos del SDK clonados de la máquina de desarrollo.