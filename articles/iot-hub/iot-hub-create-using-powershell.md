---
title: Creación de una instancia de Azure IoT Hub mediante un cmdlet de PowerShell | Microsoft Docs
description: Cómo usar un cmdlet de PowerShell para crear una instancia de IoT Hub.
author: robinsh
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 08/29/2018
ms.author: robinsh
ms.openlocfilehash: fd376728a1ebdf769c7f2ae76d61a60703e13711
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "66146606"
---
# <a name="create-an-iot-hub-using-the-new-aziothub-cmdlet"></a>Crear un IoT hub mediante el cmdlet New-AzIotHub

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

## <a name="introduction"></a>Introducción

Puede usar cmdlets de Azure PowerShell para crear y administrar instancias de Azure IoT Hub. En este tutorial se muestra cómo crear una instancia de IoT Hub con PowerShell.

Para completar este procedimiento, se necesita una suscripción de Azure. Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="connect-to-your-azure-subscription"></a>Conexión a su suscripción de Azure

Si usa Cloud Shell, ya ha iniciado sesión en su suscripción. Si, por el contrario, ejecuta PowerShell de forma local, escriba el siguiente comando para iniciar sesión en su suscripción de Azure:

```powershell
# Log into Azure account.
Login-AzAccount
```

## <a name="create-a-resource-group"></a>Crear un grupo de recursos

Necesita un grupo de recursos para implementar una instancia de IoT Hub. Puede usar un grupo de recursos existente o crear uno nuevo.

Para crear un grupo de recursos del centro de IoT, use el [New AzResourceGroup](https://docs.microsoft.com/powershell/module/az.Resources/New-azResourceGroup) comando. En este ejemplo se crea un grupo de recursos denominado **MyIoTRG1** en la región **Este de EE. UU.**:

```azurepowershell-interactive
New-AzResourceGroup -Name MyIoTRG1 -Location "East US"
```

## <a name="create-an-iot-hub"></a>Crear un centro de IoT

Para crear un centro de IoT en el grupo de recursos que creó en el paso anterior, use el [New AzIotHub](https://docs.microsoft.com/powershell/module/az.IotHub/New-azIotHub) comando. Este ejemplo crea un centro **S1** llamado **MyTestIoTHub** en la región **Este de EE. UU.**:

```azurepowershell-interactive
New-AzIotHub `
    -ResourceGroupName MyIoTRG1 `
    -Name MyTestIoTHub `
    -SkuName S1 -Units 1 `
    -Location "East US"
```

El nombre de su instancia de IoT Hub debe único globalmente.

[!INCLUDE [iot-hub-pii-note-naming-hub](../../includes/iot-hub-pii-note-naming-hub.md)]

Puede enumerar todos los centros de IoT en su suscripción mediante el [Get AzIotHub](https://docs.microsoft.com/powershell/module/az.IotHub/Get-azIotHub) comando:

```azurepowershell-interactive
Get-AzIotHub
```

En este ejemplo se muestra la instancia de IoT Hub estándar S1 que creó en el paso anterior.

Puede eliminar el IoT hub mediante el [Remove-AzIotHub](https://docs.microsoft.com/powershell/module/az.iothub/remove-aziothub) comando:

```azurepowershell-interactive
Remove-AzIotHub `
    -ResourceGroupName MyIoTRG1 `
    -Name MyTestIoTHub
```

Como alternativa, puede quitar un grupo de recursos y todos los recursos que contiene utilizando el [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.Resources/Remove-azResourceGroup) comando:

```azurepowershell-interactive
Remove-AzResourceGroup -Name MyIoTRG1
```

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha implementado una instancia de IoT Hub mediante un cmdlet de PowerShell, si desea desee seguir explorando, lea los siguientes artículos:

* [Cmdlets de PowerShell para trabajar con una instancia de IoT Hub](https://docs.microsoft.com/powershell/module/az.iothub/).

* [API REST del proveedor de recursos de IoT Hub](https://docs.microsoft.com/rest/api/iothub/iothubresource).

Para obtener más información sobre cómo desarrollar para IoT Hub, consulte los siguientes artículos:

* [Introducción a SDK para C](iot-hub-device-sdk-c-intro.md)

* [SDK de IoT de Azure](iot-hub-devguide-sdks.md)

Para explorar aún más las funcionalidades de IoT Hub, consulte:

* [Implementación de IA en dispositivos perimetrales con Azure IoT Edge](../iot-edge/tutorial-simulate-device-linux.md)
