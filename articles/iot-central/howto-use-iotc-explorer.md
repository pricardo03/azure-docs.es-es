---
title: Supervisar la conectividad de dispositivos con Azure IoT Central Explorer
description: Supervise los mensajes de los dispositivo y observe los cambios de los dispositivos gemelos mediante la CLI de IoT Central Explorer.
author: viv-liu
ms.author: viviali
ms.date: 02/20/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: 16cb27ab330118d1bb59cf4f3d782bf55fa28d43
ms.sourcegitcommit: 5fbca3354f47d936e46582e76ff49b77a989f299
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/12/2019
ms.locfileid: "57779749"
---
# <a name="monitor-device-connectivity-using-the-azure-iot-central-explorer"></a>Supervisar la conectividad de dispositivos con Azure IoT Central Explorer

*Este tema se aplica a los compiladores y administradores.*

Use la CLI de IoT Central Explorer para ver los mensajes que sus dispositivos envían a IoT Central y observe los cambios en el IoT Hub gemelo. Puede usar esta herramienta de código abierto para obtener una visión más detallada del estado de la conectividad del dispositivo y así diagnosticar los problemas de los mensajes del dispositivo que no llegan a la nube o los dispositivos que no responden a cambios gemelos.

[Visite el repositorio CAOI explorer en GitHub.](https://aka.ms/iotciotcexplorercligithub)

## <a name="prerequisites"></a>Requisitos previos

+ Node.js, versión 8.x o posterior - https://nodejs.org
+ Un administrador de la aplicación debe generar un token de acceso para su uso en el Explorador de CAOI

## <a name="install-iotc-explorer"></a>Instale el Explorador de CAOI

Ejecute el siguiente comando desde su línea de comando paras instalarlo:

```cmd/sh
npm install -g iotc-explorer
```

> [!NOTE]
> Normalmente, deberá ejecutar el comando de instalación con `sudo` en entornos similares a Unix.

## <a name="run-iotc-explorer"></a>Ejecute CAOI-explorer

Las secciones siguientes describen los comandos y opciones que puede usar al ejecutar `iotc-explorer`. Para ver el conjunto completo de comandos y opciones, pasar `--help` a `iotc-explorer` o cualquiera de sus subcomandos.

### <a name="login"></a>Inicio de sesión

Antes de comenzar, un administrador de su aplicación de IoT Central debe obtener un token de acceso para que usted pueda usarlo. El administrador debe realizar los siguientes pasos:

1. Vaya a **administración** , a continuación, **Tokens de acceso**.
1. Seleccione **generar Token**.
    ![Captura de pantalla de la página de tokens de acceso](media/howto-use-iotc-explorer/accesstokenspage.png)

1. Escriba un nombre de símbolo (token), seleccione **siguiente**y, a continuación, **copia**.
    > [!NOTE]
    > El valor del token solo se muestra una vez, por lo que se debe copiar antes de cerrar el cuadro de diálogo. Después de cerrar el cuadro de diálogo, porque no es nunca volverá a mostrar.

    ![Captura de pantalla del cuadro diálogo de la opción para copiar el token de acceso](media/howto-use-iotc-explorer/copyaccesstoken.png)

Puede usar el token para iniciar sesión en la CLI como sigue:

```cmd/sh
iotc-explorer login "<Token value>"
```

Si prefiere no tener el token que se conserva en el historial de shell, puede dejar el token de alejar y proporciónelo en su lugar cuando se le solicite:

```cmd/sh
iotc-explorer login
```

### <a name="monitor-device-messages"></a>Supervisar los mensajes del dispositivo

Puede ver los mensajes provenientes de un dispositivo específico o de todos los dispositivos en su aplicación mediante el comando `monitor-messages`. Este comando inicia un observador que envía continuamente nuevos mensajes conforme llegan:

Para ver todos los dispositivos en la aplicación, ejecute el siguiente comando:

```cmd/sh
iotc-explorer monitor-messages
```

Salida:

![salida del comando monitor-messages](media/howto-use-iotc-explorer/monitormessages.png)

Para ver un dispositivo específico, simplemente agregue el identificador de dispositivo al final del comando:

```cmd/sh
iotc-explorer monitor-messages <your-device-id>
```

También puede generar un formato más reconocible por la máquina agregando los `--raw` opción del comando:

```
iotc-explorer monitor-messages --raw
```

### <a name="get-device-twin"></a>Obtener dispositivo gemelo

Puede usar el comando `get-twin` para obtener el contenido del dispositivo gemelo para un dispositivo de IoT Central. Para ello, ejecute el siguiente comando:

```cmd/sh
iotc-explorer get-twin <your-device-id>
```

Salida:

![salida del comando get-twin](media/howto-use-iotc-explorer/getdevicetwin.png)

Igual que sucede con `monitor-messages`, puede obtener un resultado que sea más fácil de reconocer para la máquina si usa la opción `--raw`:

```cmd/sh
iotc-explorer get-twin <your-device-id> --raw
```

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha aprendido a usar el Explorador de IoT Central, el siguiente paso sugerido es explorar [administración de dispositivos IoT Central](howto-manage-devices.md).
