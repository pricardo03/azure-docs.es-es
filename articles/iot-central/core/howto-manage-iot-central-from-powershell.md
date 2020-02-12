---
title: Administración de IoT Central desde Azure PowerShell | Microsoft Docs
description: En este artículo se describe cómo crear y administrar aplicaciones de IoT Central desde Azure PowerShell.
services: iot-central
ms.service: iot-central
author: dominicbetts
ms.author: dobett
ms.date: 07/11/2019
ms.topic: conceptual
manager: philmea
ms.openlocfilehash: a95b59c6cc0d486c1d4b10f39d0d272dd4b34f54
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/05/2020
ms.locfileid: "77018999"
---
# <a name="manage-iot-central-from-azure-powershell"></a>Administración de IoT Central desde Azure PowerShell

[!INCLUDE [iot-central-selector-manage](../../../includes/iot-central-selector-manage.md)]

En lugar de crear y administrar aplicaciones de IoT Central desde el sitio web del [administrador de aplicaciones de Azure IoT Central](https://aka.ms/iotcentral), puede usar [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview) para administrar las aplicaciones.

## <a name="prerequisites"></a>Prerequisites

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Si prefiere ejecutar Azure PowerShell en el equipo local, consulte [Install the Azure PowerShell module](https://docs.microsoft.com/powershell/azure/install-az-ps) (Instalación del módulo de Azure PowerShell). Cuando ejecute Azure PowerShell localmente, use el cmdlet **Connect-AzAccount** para iniciar sesión en Azure antes de probar los cmdlets descritos en este artículo.

## <a name="install-the-iot-central-module"></a>Instalación del módulo de IoT Central

Ejecute el siguiente comando para comprobar si el [módulo de IoT Central](https://docs.microsoft.com/powershell/module/az.iotcentral/) está instalado en el entorno de PowerShell:

```powershell
Get-InstalledModule -name Az.I*
```

Si la lista de módulos instalados no incluye **Az.IotCentral**, ejecute el siguiente comando:

```powershell
Install-Module Az.IotCentral
```

## <a name="create-an-application"></a>Crear una aplicación

Use el cmdlet [New-AzIotCentralApp](https://docs.microsoft.com/powershell/module/az.iotcentral/New-AzIotCentralApp) para crear una aplicación de IoT Central en su suscripción de Azure. Por ejemplo:

```powershell
# Create a resource group for the IoT Central application
New-AzResourceGroup -ResourceGroupName "MyIoTCentralResourceGroup" `
  -Location "East US"
```

```powershell
# Create an IoT Central application
New-AzIotCentralApp -ResourceGroupName "MyIoTCentralResourceGroup" `
  -Name "myiotcentralapp" -Subdomain "mysubdomain" `
  -Sku "ST1" -Template "iotc-demo@1.0.0" `
  -DisplayName "My Custom Display Name"
```

El script crea primero un grupo de recursos en la región Este de EE. UU. para la aplicación. En la tabla siguiente se describen los parámetros utilizados con el comando **New-AzIotCentralApp**:

|Parámetro         |Descripción |
|------------------|------------|
|ResourceGroupName |Grupo de recursos que contiene a la aplicación. Este grupo de recursos ya debe existir en la suscripción. |
|Location |De forma predeterminada, este cmdlet usa la ubicación del grupo de recursos. Actualmente, se pueden crear aplicaciones de IoT Central en **Este de EE. UU.** , **Oeste de EE. UU.** , **Norte de Europa** u **Oeste de Europa**, o incluso en las regiones geográficas de **Australia** o **Asia Pacifico**.  |
|Nombre              |Nombre de la aplicación en Azure Portal. |
|Subdominio         |Subdominio en la dirección URL de la aplicación. En el ejemplo, la dirección URL de la aplicación es https://mysubdomain.azureiotcentral.com. |
|SKU               |Actualmente, puede usar **ST1** o **ST2**. Consulte [Precios de Azure IoT Central](https://azure.microsoft.com/pricing/details/iot-central/). |
|Plantilla          | Plantilla de aplicación que se va a usar. Para más información, vea la tabla siguiente: |
|DisplayName       |Nombre de la aplicación tal como se muestra en la interfaz de usuario. |

**Plantillas de aplicación**

| Nombre de la plantilla            | Descripción |
| ------------------------ | ----------- |
| iotc-default@1.0.0       | Permite crear una aplicación vacía para que pueda rellenarla con sus propias plantillas de dispositivo y dispositivos.
| iotc-pnp-preview@1.0.0   | Crea una aplicación de Plug and Play (versión preliminar) vacía para completarla con sus propios dispositivos y plantillas de dispositivo. |
| iotc-condition@1.0.0     | Crea una aplicación con una plantilla de supervisión de condiciones de análisis en la tienda. Use esta plantilla para conectar y supervisar el entorno del almacén. |
| iotc-consumption@1.0.0   | Permite crear una aplicación con la plantilla de supervisión del consumo de agua. Use esta plantilla para supervisar y controlar el flujo de agua. |
| iotc-distribution@1.0.0  | Permite crear una aplicación con una plantilla de distribución digital. Use esta plantilla para mejorar la eficacia de la salida de productos del almacén mediante la digitalización de recursos y acciones clave. |
| iotc-inventory@1.0.0     | Permite crear una aplicación con una plantilla de administración inteligente de inventarios. Use esta plantilla para automatizar la recepción, el movimiento de productos, el recuento de ciclos y el seguimiento y el seguimiento de sensores. |
| iotc-logistics@1.0.0     | Permite crear una aplicación con una plantilla de logística conectada. Use esta plantilla para realizar un seguimiento de su envío en tiempo real por aire, mar y tierra con supervisión del estado y la ubicación. |
| iotc-meter@1.0.0         | Permite crear una aplicación con una plantilla de supervisión de medidores inteligentes. Use esta plantilla para supervisar el consumo de energía y el estado de la red e identificar tendencias para mejorar la asistencia al cliente y la administración de medidores inteligentes.  |
| iotc-patient@1.0.0       | Permite crear una aplicación con una plantilla de supervisión continua de pacientes. Use esta plantilla para ampliar el cuidado de los pacientes, las readmisiones y el control de enfermedades. |
| iotc-power@1.0.0         | Permite crear una aplicación con una plantilla de supervisión de paneles solares. Use esta plantilla para supervisar el estado del panel solar y las tendencias de generación de energía. |
| iotc-quality@1.0.0       | Permite crear una aplicación con una plantilla de supervisión de la calidad del agua. Use esta plantilla para supervisar digitalmente la calidad del agua.|
| iotc-store@1.0.0         | Permite crear una aplicación con una plantilla de análisis en tienda y finalización de la compra. Use esta plantilla para supervisar y administrar el flujo de finalización de compra en su tienda. |
| iotc-waste@1.0.0         | Permite crear una aplicación con una plantilla de administración de desechos conectada. Use esta plantilla para supervisar contenedores de residuos y operadores de campo de distribución. |

## <a name="view-your-iot-central-applications"></a>Visualización de las aplicaciones de IoT Central

Use el cmdlet [Get-AzIotCentralApp](https://docs.microsoft.com/powershell/module/az.iotcentral/Get-AzIotCentralApp) para enumerar las aplicaciones de IoT Central y ver los metadatos.

## <a name="modify-an-application"></a>Modificación de una aplicación

Use el cmdlet [Set-AzIotCentralApp](https://docs.microsoft.com/powershell/module/az.iotcentral/set-aziotcentralapp) para actualizar los metadatos de una aplicación de IoT Central. Por ejemplo, para cambiar el nombre para mostrar de la aplicación, use:

```powershell
Set-AzIotCentralApp -Name "myiotcentralapp" `
  -ResourceGroupName "MyIoTCentralResourceGroup" `
  -DisplayName "My new display name"
```

## <a name="remove-an-application"></a>Eliminación de una aplicación

Use el cmdlet [Remove-AzIotCentralApp](https://docs.microsoft.com/powershell/module/az.iotcentral/Remove-AzIotCentralApp) para eliminar una aplicación de IoT Central. Por ejemplo:

```powershell
Remove-AzIotCentralApp -ResourceGroupName "MyIoTCentralResourceGroup" `
 -Name "myiotcentralapp"
```

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha aprendido a administrar aplicaciones de Azure IoT Central desde Azure PowerShell, le sugerimos el paso siguiente:

> [!div class="nextstepaction"]
> [Administrar su aplicación](howto-administer.md)
