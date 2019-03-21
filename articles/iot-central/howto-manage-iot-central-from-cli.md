---
title: Administrar IoT Central desde la CLI de Azure | Microsoft Docs
description: Administrar IoT Central desde la CLI de Azure.
services: iot-central
ms.service: iot-central
author: dominicbetts
ms.author: dobett
ms.date: 02/07/2019
ms.topic: conceptual
manager: philmea
ms.openlocfilehash: 9e5d842cece316bc9c53e1e8583f40a0f222b91d
ms.sourcegitcommit: 7f7c2fe58c6cd3ba4fd2280e79dfa4f235c55ac8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/25/2019
ms.locfileid: "56809399"
---
# <a name="manage-iot-central-from-azure-cli"></a>Administrar IoT Central desde la CLI de Azure

[!INCLUDE [iot-central-selector-manage](../../includes/iot-central-selector-manage.md)]

En lugar de crear y administrar aplicaciones de IoT Central desde el centro de IoT [Application Manager](https://aka.ms/iotcentral) página, puede usar [CLI de Azure](/cli/azure/) para administrar las aplicaciones.

## <a name="prerequisites"></a>Requisitos previos

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Si prefiere ejecutar la CLI de Azure en el equipo local, consulte [instalar la CLI de Azure](/cli/azure/install-azure-cli). Cuando se ejecuta localmente la CLI de Azure, use el **inicio de sesión de az** comando para iniciar sesión en Azure antes de intentar los comandos en este artículo.

## <a name="create-an-application"></a>Creación de una aplicación

Use la [az iotcentral aplicación crear](/cli/azure/iotcentral/app#az-iotcentral-app-create) comando para crear una aplicación IoT Central en su suscripción de Azure. Por ejemplo: 

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

En primer lugar, estos comandos crean un grupo de recursos en la región de EE. UU. para la aplicación de este. La tabla siguiente describen los parámetros utilizados con la **az iotcentral aplicación crear** comando:

| Parámetro         | DESCRIPCIÓN |
| ----------------- | ----------- |
| resource-group    | Grupo de recursos que contiene a la aplicación. Este grupo de recursos ya debe existir en la suscripción. |
| location          | De forma predeterminada, este comando usa la ubicación del grupo de recursos. Actualmente, puede crear una aplicación de IoT Central en las regiones **Este de EE. UU.**, **Oeste de EE. UU.**, **Europa del Norte** o **Europa Occidental**. |
| Nombre              | Nombre de la aplicación en Azure Portal. |
| subdominio         | Subdominio en la dirección URL de la aplicación. En el ejemplo, la dirección URL de la aplicación es https://mysubdomain.azureiotcentral.com. |
| sku               | Actualmente, el único valor es **S1** (nivel estándar). Consulte [Precios de Azure IoT Central](https://azure.microsoft.com/pricing/details/iot-central/). |
| template          | Plantilla de aplicación que se va a usar. Para más información, vea la tabla siguiente: |
| display-name      | Nombre de la aplicación tal como se muestra en la interfaz de usuario. |

**Plantillas de aplicación**

| Nombre de la plantilla            | DESCRIPCIÓN |
| ------------------------ | ----------- |
| iotc-default@1.0.0       | Permite crear una aplicación vacía para que pueda rellenarla con sus propias plantillas de dispositivo y dispositivos. |
| iotc-demo@1.0.0          | Crea una aplicación que incluye una plantilla de dispositivo que ya se ha creado para una máquina expendedora de refrigerados. Utilice esta plantilla para empezar a explorar Azure IoT Central. |
| iotc-devkit-sample@1.0.0 | Permite crear una aplicación con plantillas de dispositivo preparadas para que se conecte a un dispositivo MXChip o Raspberry Pi. Utilice esta plantilla si es un desarrollador de dispositivos que experimenta con alguno de estos dispositivos. |

## <a name="view-your-applications"></a>Visualización de sus aplicaciones

Use la [lista de aplicaciones de az iotcentral](/cli/azure/iotcentral/app#az-iotcentral-app-list) comando para enumerar las aplicaciones de IoT Central y ver los metadatos.

## <a name="modify-an-application"></a>Modificación de una aplicación

Use la [actualización de la aplicación de az iotcentral](/cli/azure/iotcentral/app#az-iotcentral-app-update) comando para actualizar los metadatos de una aplicación IoT Central. Por ejemplo, para cambiar el nombre para mostrar de la aplicación, use:

```azurecli-interactive
az iotcentral app update --name myiotcentralapp \
  --resource-group MyIoTCentralResourceGroup \
  --set displayName="My new display name"
```

## <a name="remove-an-application"></a>Eliminación de una aplicación

Use la [Eliminar aplicación de az iotcentral](/cli/azure/iotcentral/app#az-iotcentral-app-delete) comando para eliminar una aplicación IoT Central. Por ejemplo: 

```azurecli-interactive
az iotcentral app delete --name myiotcentralapp \
  --resource-group MyIoTCentralResourceGroup
```

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha aprendido cómo administrar aplicaciones de Azure IoT Central desde la CLI de Azure, aquí es el siguiente paso sugerido:

> [!div class="nextstepaction"]
> [Administrar su aplicación](howto-administer.md)
