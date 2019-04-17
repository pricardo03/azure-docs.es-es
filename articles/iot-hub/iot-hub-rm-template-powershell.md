---
title: Creación de un centro de IoT de Azure mediante una plantilla (PowerShell) | Microsoft Docs
description: Cómo usar una plantilla de Azure Resource Manager para crear un centro de IoT con Azure PowerShell.
author: robinsh
manager: philmea
ms.author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 04/02/2019
ms.openlocfilehash: d23d3824c477d3bba4e4900bee355376f1317f92
ms.sourcegitcommit: 5f348bf7d6cf8e074576c73055e17d7036982ddb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/16/2019
ms.locfileid: "59609187"
---
# <a name="create-an-iot-hub-using-azure-resource-manager-template-powershell"></a>Creación de un centro de IoT con una plantilla de Azure Resource Manager (PowerShell)

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

Obtenga información sobre cómo usar una plantilla de Azure Resource Manager para crear un centro de IoT y un grupo de consumidores. Las plantillas de Resource Manager son archivos JSON que definen los recursos que necesita para implementar la solución. Para obtener más información sobre el desarrollo de plantillas de Resource Manager, consulte [documentación de Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/).

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="create-an-iot-hub"></a>Crear un centro de IoT

Es la plantilla de Resource Manager utilizada en esta guía de inicio rápido de [plantillas de inicio rápido de Azure](https://azure.microsoft.com/resources/templates/101-iothub-with-consumergroup-create/). Esta es una copia de la plantilla:

[!code-json[iothub-creation](~/quickstart-templates/101-iothub-with-consumergroup-create/azuredeploy.json)]

La plantilla crea un centro de Iot de Azure con tres puntos de conexión (centro de eventos, en la nube a dispositivo y mensajería) y un grupo de consumidores. Para obtener más ejemplos de plantillas, consulte [plantillas de inicio rápido de Azure](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Devices&pageNumber=1&sort=Popular). Puede encontrar el esquema de plantilla de Iot Hub [aquí](https://docs.microsoft.com/azure/templates/microsoft.devices/iothub-allversions).

Existen varios métodos para implementar una plantilla.  Usar Azure PowerShell en este tutorial.

Para ejecutar el script de PowerShell, seleccione **Pruébelo** para abrir Azure Cloud shell. Pegue el script, haga clic en el shell y, a continuación, seleccione Pegar:

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
$location = Read-Host -Prompt "Enter the location (i.e. centralus)"
$iotHubName = Read-Host -Prompt "Enter the IoT Hub name"

New-AzResourceGroup -Name $resourceGroupName -Location "$location"
New-AzResourceGroupDeployment `
    -ResourceGroupName $resourceGroupName `
    -TemplateUri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-iothub-with-consumergroup-create/azuredeploy.json" `
    -iotHubName $iotHubName
```

Como puede ver en el script de PowerShell, la plantilla utilizada es de plantillas de inicio rápido de Azure. Para usar su propia, debe cargar primero el archivo de plantilla en Cloud shell y, a continuación, utilice la `-TemplateFile` conmutador para especificar el nombre de archivo.  Para obtener un ejemplo, vea [implementar la plantilla](../azure-resource-manager/resource-manager-quickstart-create-templates-use-visual-studio-code.md?tabs=PowerShell#deploy-the-template).

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha implementado un centro de IoT mediante una plantilla de Azure Resource Manager, es posible que desee explorar más a fondo:

* Consulte las funcionalidades de la [API de REST del proveedor de recursos de IoT Hub][lnk-rest-api].
* Para más información sobre las funcionalidades de Azure Resource Manager, consulte [Información general de Azure Resource Manager][lnk-azure-rm-overview].
* Para conocer la sintaxis y las propiedades JSON que se usan en las plantillas, consulte [Microsoft.Devices resource types](/azure/templates/microsoft.devices/iothub-allversions) (Tipos de recursos de Microsoft.Devices).

Para obtener más información sobre cómo desarrollar para IoT Hub, consulte los siguientes artículos:

* [Introducción a C SDK][lnk-c-sdk]
* [SDK de IoT de Azure][lnk-sdks]

Para explorar aún más las funcionalidades de IoT Hub, consulte:

* [Implementación de Azure IoT Edge en un dispositivo simulado en Linux: versión preliminar][lnk-iotedge]

<!-- Links -->
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-azure-portal]: https://portal.azure.com/
[lnk-status]: https://azure.microsoft.com/status/
[lnk-powershell-install]: /powershell/azure/install-Az-ps
[lnk-rest-api]: https://docs.microsoft.com/rest/api/iothub/iothubresource
[lnk-azure-rm-overview]: ../azure-resource-manager/resource-group-overview.md
[lnk-powershell-arm]: ../azure-resource-manager/manage-resources-powershell.md

[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-iotedge]: ../iot-edge/tutorial-simulate-device-linux.md
