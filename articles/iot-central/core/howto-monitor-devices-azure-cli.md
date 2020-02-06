---
title: Supervisar la conectividad de dispositivos con Azure IoT Central Explorer
description: Supervise los mensajes de los dispositivo y observe los cambios de los dispositivos gemelos mediante la CLI de IoT Central Explorer.
author: viv-liu
ms.author: viviali
ms.date: 12/18/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: corywink
ms.openlocfilehash: 3f18537b4b038844c9aa824593e354c23c792370
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/05/2020
ms.locfileid: "77026420"
---
# <a name="monitor-device-connectivity-using-azure-cli"></a>Supervisión de la conectividad de dispositivos mediante la CLI de Azure

*Este tema se aplica a los compiladores y administradores.*

Use la extensión de IoT de la CLI de Azure para ver los mensajes que sus dispositivos envían a IoT Central y observe los cambios en el dispositivo gemelo. Puede usar esta herramienta para depurar y observar la conectividad del dispositivo y diagnosticar los problemas de los mensajes del dispositivo que no llegan a la nube o los dispositivos que no responden a cambios gemelos.

[Consulte la referencia de extensiones de la CLI de Azure para obtener más detalles](https://docs.microsoft.com/cli/azure/ext/azure-cli-iot-ext/iot/central)

## <a name="prerequisites"></a>Prerequisites

+ CLI de Azure instalada, versión 2.0.7 o posterior. Compruebe la versión de la CLI de Azure al ejecutar `az --version`. Obtenga información sobre la instalación y actualización en los [docs de la CLI de Azure](https://docs.microsoft.com/cli/azure/install-azure-cli)
+ Una cuenta profesional o educativa de Azure, agregada como usuario en una aplicación de IoT Central.

## <a name="install-the-iot-central-extension"></a>Instalación de la extensión de IoT Central

Ejecute el siguiente comando desde su línea de comando paras instalarlo:

```cmd/sh
az extension add --name azure-cli-iot-ext
```

Compruebe la versión de la extensión al ejecutar 
```cmd/sh
az --version
```
Debe ver que la extensión azure-cli-iot-ext es 0.8.1 o posterior. Si no es así, ejecute
```cmd/sh
az extension update --name azure-cli-iot-ext
```

## <a name="using-the-extension"></a>Uso de la extensión

En las secciones siguientes se describen comandos y opciones comunes que puede usar al ejecutar `az iot central`. Para ver el conjunto completo de comandos y opciones, pase `--help` a `az iot central` o a cualquiera de sus subcomandos.

### <a name="login"></a>Inicio de sesión

Empiece por iniciar sesión en la CLI de Azure. 

```cmd/sh
az login
```

### <a name="get-the-application-id-of-your-iot-central-app"></a>Obtener el id. de la aplicación de la aplicación de IoT Central
En **Administración/Configuración de la aplicación**, copie el **Id. de la aplicación**. Se usará más adelante.

### <a name="monitor-messages"></a>Controlar mensajes
Supervise los mensajes que se envían a la aplicación de IoT Central desde sus dispositivos. Esto incluirá todos los encabezados y anotaciones.

```cmd/sh
az iot central app monitor-events --app-id <app-id> --properties all
```

### <a name="view-device-properties"></a>Visualización de las propiedades del dispositivo
Consulte las propiedades actuales del dispositivo de lectura y de lectura y escritura para un dispositivo determinado.

```cmd/sh
az iot central device-twin show --app-id <app-id> --device-id <device-id>
```

## <a name="next-steps"></a>Pasos siguientes

Ahora que ya ha aprendido a usar IoT Central Explorer, el siguiente paso sugerido es explorar la [administración de dispositivos de IoT Central](howto-manage-devices.md).
