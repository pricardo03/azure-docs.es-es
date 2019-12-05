---
title: Administración de IoT Central desde la CLI de Azure | Microsoft Docs
description: En este artículo se describe cómo crear y administrar aplicaciones de IoT Central con la CLI. Puede ver, modificar y quitar la aplicación mediante la CLI.
services: iot-central
ms.service: iot-central
author: dominicbetts
ms.author: dobett
ms.date: 08/23/2019
ms.topic: conceptual
manager: philmea
ms.openlocfilehash: c5622f32dbf849b9a21a1fd2e458f35b8aa1d098
ms.sourcegitcommit: 8cf199fbb3d7f36478a54700740eb2e9edb823e8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/25/2019
ms.locfileid: "74480395"
---
# <a name="manage-iot-central-from-azure-cli"></a>Administración de IoT Central desde la CLI de Azure

[!INCLUDE [iot-central-selector-manage](../../../includes/iot-central-selector-manage.md)]

En lugar de crear y administrar aplicaciones de IoT Central desde el sitio web del [administrador de aplicaciones de Azure IoT Central](https://aka.ms/iotcentral), puede usar la [CLI de Azure](/cli/azure/) para administrar las aplicaciones.

## <a name="prerequisites"></a>Requisitos previos

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

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

Estos comandos crean en primer lugar un grupo de recursos en la ubicación Este de EE. UU. para la aplicación. En la siguiente tabla se describen los parámetros utilizados con el comando **az iotcentral app create**:

| Parámetro         | DESCRIPCIÓN |
| ----------------- | ----------- |
| resource-group    | Grupo de recursos que contiene a la aplicación. Este grupo de recursos ya debe existir en la suscripción. |
| location          | Este comando usa de forma predeterminada la ubicación del grupo de recursos. Actualmente, puede crear una aplicación IoT Central en **Estados Unidos**, **Australia**, **Asia Pacífico** o **Europa**. |
| Nombre              | Nombre de la aplicación en Azure Portal. |
| subdomain         | Subdominio en la dirección URL de la aplicación. En el ejemplo, la dirección URL de la aplicación es https://mysubdomain.azureiotcentral.com. |
| sku               | Actualmente, el único valor es **S1** (nivel estándar). Consulte [Precios de Azure IoT Central](https://azure.microsoft.com/pricing/details/iot-central/). |
| template          | Plantilla de aplicación que se va a usar. Para más información, vea la tabla siguiente: |
| display-name      | Nombre de la aplicación tal como se muestra en la interfaz de usuario. |

**Plantillas de aplicación con características disponibles con carácter general**

| Nombre de la plantilla            | DESCRIPCIÓN |
| ------------------------ | ----------- |
| iotc-default@1.0.0       | Permite crear una aplicación vacía para que pueda rellenarla con sus propias plantillas de dispositivo y dispositivos. |
| iotc-demo@1.0.0          | Crea una aplicación que incluye una plantilla de dispositivo que ya se ha creado para una máquina expendedora de refrigerados. Utilice esta plantilla para empezar a explorar Azure IoT Central. |
| iotc-devkit-sample@1.0.0 | Permite crear una aplicación con plantillas de dispositivo preparadas para que se conecte a un dispositivo MXChip o Raspberry Pi. Utilice esta plantilla si es un desarrollador de dispositivos que experimenta con alguno de estos dispositivos. |


**Plantillas de aplicación con características de versión preliminar pública**

| Nombre de la plantilla            | DESCRIPCIÓN |
| ------------------------ | ----------- |
| iotc-pnp-preview@1.0.0   | Permite crear una aplicación de vista previa Plug and Play vacía para que pueda rellenarla con sus propias plantillas de dispositivo y sus propios dispositivos. |
| iotc-condition@1.0.0     | Permite crear una aplicación con una plantilla de análisis en tienda: supervisión de condiciones. Use esta plantilla para conectar y supervisar el entorno de almacén. |
| iotc-consumption@1.0.0   | Permite crear una aplicación con la plantilla de supervisión del consumo de agua. Use esta plantilla para supervisar y controlar el flujo de agua. |
| iotc-distribution@1.0.0  | Permite crear una aplicación con una plantilla de distribución digital. Use esta plantilla para mejorar la eficacia de la salida de productos del almacén mediante la digitalización de recursos y acciones clave. |
| iotc-inventory@1.0.0     | Permite crear una aplicación con una plantilla de administración inteligente de inventarios. Use esta plantilla para automatizar la recepción, el movimiento de productos, el recuento de ciclos y el seguimiento y el seguimiento de sensores. |
| iotc-logistics@1.0.0     | Permite crear una aplicación con una plantilla de logística conectada. Use esta plantilla para realizar un seguimiento de su envío en tiempo real por aire, mar y tierra con supervisión del estado y la ubicación. |
| iotc-meter@1.0.0         | Permite crear una aplicación con una plantilla de supervisión de medidores inteligentes. Use esta plantilla para supervisar el consumo de energía y el estado de la red e identificar tendencias para mejorar la asistencia al cliente y la administración de medidores inteligentes.  |
| iotc-patient@1.0.0       | Permite crear una aplicación con una plantilla de supervisión continua de pacientes. Use esta plantilla para ampliar el cuidado de los pacientes, las readmisiones y el control de enfermedades. |
| iotc-power@1.0.0         | Permite crear una aplicación con una plantilla de supervisión de paneles solares. Use esta plantilla para supervisar el estado del panel solar y las tendencias de generación de energía. |
| iotc-quality@1.0.0       | Permite crear una aplicación con una plantilla de supervisión de la calidad del agua. Use esta plantilla para supervisar digitalmente la calidad del agua.|
| iotc-store@1.0.0         | Permite crear una aplicación con una plantilla de análisis en tienda: finalización de la compra. Use esta plantilla para supervisar y administrar el flujo finalización de compra en su tienda. |
| iotc-waste@1.0.0         | Permite crear una aplicación con una plantilla de administración de desechos conectada. Use esta plantilla para supervisar contenedores de residuos y operadores de campo de distribución. |

> [!NOTE]
> Las plantillas de vista previa de aplicación solo están disponibles actualmente en **Europa** y **Estados Unidos**.

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
