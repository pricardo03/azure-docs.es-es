---
title: Administración de IoT Central desde la CLI de Azure | Microsoft Docs
description: Administración de IoT Central desde la CLI de Azure
services: iot-central
ms.service: iot-central
author: dominicbetts
ms.author: dobett
ms.date: 08/23/2019
ms.topic: conceptual
manager: philmea
ms.openlocfilehash: 1ec83541c62d93eee91348531797ecdeb8c9fc6e
ms.sourcegitcommit: 23389df08a9f4cab1f3bb0f474c0e5ba31923f12
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2019
ms.locfileid: "70873481"
---
# <a name="manage-iot-central-from-azure-cli"></a>Administración de IoT Central desde la CLI de Azure

[!INCLUDE [iot-central-selector-manage](../../includes/iot-central-selector-manage.md)]

En lugar de crear y administrar aplicaciones de IoT Central desde el sitio web del [administrador de aplicaciones de Azure IoT Central](https://aka.ms/iotcentral), puede usar la [CLI de Azure](/cli/azure/) para administrar las aplicaciones.

## <a name="prerequisites"></a>Requisitos previos

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Si prefiere ejecutar la CLI de Azure en el equipo local, vea [Instalación de la CLI de Azure](/cli/azure/install-azure-cli). Cuando ejecute la CLI de Azure localmente, use el comando **az login** para iniciar sesión en Azure antes de probar los comandos descritos en este artículo.

## <a name="create-an-application"></a>Creación de una aplicación

Use el comando [az iotcentral app create](/cli/azure/iotcentral/app#az-iotcentral-app-create) para crear una aplicación de IoT Central en su suscripción de Azure. Por ejemplo:

```azurecli-interactive
# Create a resource group for the IoT Central application
az group create --location "East US" \
    --name "MyIoTCentralResourceGroup"
```

```azurecli-interactive
# Create an IoT Central application
az iotcentral app create \
  --resource-group "MyIoTCentralResourceGroup" \
  --name "myiotcentralapp" --subdomain "mysubdomain" \
  --sku S1 --template "iotc-demo@1.0.0" \
  --display-name "My Custom Display Name"
```

Estos comandos crean en primer lugar un grupo de recursos en la región Este de EE. UU. para la aplicación. En la siguiente tabla se describen los parámetros utilizados con el comando **az iotcentral app create**:

| Parámetro         | DESCRIPCIÓN |
| ----------------- | ----------- |
| resource-group    | Grupo de recursos que contiene a la aplicación. Este grupo de recursos ya debe existir en la suscripción. |
| location          | Este comando usa de forma predeterminada la ubicación del grupo de recursos. Actualmente, puede crear una aplicación de IoT Central en las regiones **Este de EE. UU.** , **Oeste de EE. UU.** , **Norte de Europa** u **Oeste de Europa**, o incluso en la región geográfica de **Australia**. |
| Nombre              | Nombre de la aplicación en Azure Portal. |
| subdomain         | Subdominio en la dirección URL de la aplicación. En el ejemplo, la dirección URL de la aplicación es https://mysubdomain.azureiotcentral.com. |
| sku               | Actualmente, el único valor es **S1** (nivel estándar). Consulte [Precios de Azure IoT Central](https://azure.microsoft.com/pricing/details/iot-central/). |
| template          | Plantilla de aplicación que se va a usar. Para más información, vea la tabla siguiente: |
| display-name      | Nombre de la aplicación tal como se muestra en la interfaz de usuario. |

**Plantillas de aplicación**

| Nombre de la plantilla            | DESCRIPCIÓN |
| ------------------------ | ----------- |
| iotc-default@1.0.0       | Permite crear una aplicación vacía para que pueda rellenarla con sus propias plantillas de dispositivo y dispositivos. |
| iotc-demo@1.0.0          | Crea una aplicación que incluye una plantilla de dispositivo que ya se ha creado para una máquina expendedora de refrigerados. Utilice esta plantilla para empezar a explorar Azure IoT Central. |
| iotc-devkit-sample@1.0.0 | Permite crear una aplicación con plantillas de dispositivo preparadas para que se conecte a un dispositivo MXChip o Raspberry Pi. Utilice esta plantilla si es un desarrollador de dispositivos que experimenta con alguno de estos dispositivos. |

> [!NOTE]
> La plantilla de **Vista previa de aplicación** solo está disponible actualmente en las regiones **Norte de Europa** y **Centro de EE. UU.** .

## <a name="view-your-applications"></a>Visualización de sus aplicaciones

Use el comando [az iotcentral app list](/cli/azure/iotcentral/app#az-iotcentral-app-list) para enumerar las aplicaciones de IoT Central y ver los metadatos.

## <a name="modify-an-application"></a>Modificación de una aplicación

Use el comando [az iotcentral app update](/cli/azure/iotcentral/app#az-iotcentral-app-update) para actualizar los metadatos de una aplicación de IoT Central. Por ejemplo, para cambiar el nombre para mostrar de la aplicación, use:

```azurecli-interactive
az iotcentral app update --name myiotcentralapp \
  --resource-group MyIoTCentralResourceGroup \
  --set displayName="My new display name"
```

## <a name="remove-an-application"></a>Eliminación de una aplicación

Use el comando [az iotcentral app delete](/cli/azure/iotcentral/app#az-iotcentral-app-delete) para eliminar una aplicación de IoT Central. Por ejemplo:

```azurecli-interactive
az iotcentral app delete --name myiotcentralapp \
  --resource-group MyIoTCentralResourceGroup
```

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha aprendido a administrar aplicaciones de Azure IoT Central desde la CLI de Azure, le sugerimos el paso siguiente:

> [!div class="nextstepaction"]
> [Administrar su aplicación](howto-administer.md)
