---
title: Supervisar la conectividad de dispositivos con Azure IoT Central Explorer
description: Supervise los mensajes de los dispositivo y observe los cambios de los dispositivos gemelos mediante la CLI de IoT Central Explorer.
author: viv-liu
ms.author: viviali
ms.date: 09/12/2018
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: 962f394607d20869bf00db624533996b0060eaf2
ms.sourcegitcommit: 776b450b73db66469cb63130c6cf9696f9152b6a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/18/2018
ms.locfileid: "45987246"
---
# <a name="monitor-device-connectivity-using-the-azure-iot-central-explorer"></a>Supervisar la conectividad de dispositivos con Azure IoT Central Explorer

*Este tema se aplica a los compiladores y administradores.*

Use la CLI de IoT Central Explorer para ver los mensajes que sus dispositivos envían a IoT Central y observe los cambios en el IoT Hub gemelo. Puede usar esta herramienta de código abierto para obtener una visión más detallada del estado de la conectividad del dispositivo y así diagnosticar los problemas de los mensajes del dispositivo que no llegan a la nube o los dispositivos que no responden a cambios gemelos.

## <a name="visit-the-iotc-explorer-repo-in-githubhttpsakamsiotciotcexplorercligithub"></a>[Visite el repositorio de iotc-explorer en GitHub](https://aka.ms/iotciotcexplorercligithub)

## <a name="prerequisites"></a>Requisitos previos
+ Node.js, versión 8.x o posterior - https://nodejs.org
+ Necesitará que un administrador de su aplicación genere un token de acceso para usarlo en iotc-explorer.

## <a name="installing-iotc-explorer"></a>Instalación de iotc-explorer

Ejecute el siguiente comando desde su línea de comando paras instalarlo:

```
npm install -g iotc-explorer
```

> [!NOTE]
> Normalmente es necesario ejecutar el comando de instalación con `sudo` en entornos similares a Unix.

## <a name="running-iotc-explorer"></a>Ejecutar iotc-explorer

A continuación, se muestran algunos comandos y opciones comunes que puede ejecutar cuando use `iotc-explorer`. Para ver el conjunto completo de comandos y opciones, puede pasar `--help` a `iotc-explorer` o a cualquiera de sus subcomandos.

### <a name="login"></a>Inicio de sesión

Antes de comenzar, un administrador de su aplicación de IoT Central debe obtener un token de acceso para que usted pueda usarlo. El administrador debe realizar los siguientes pasos:
1. Debe ir a **Administración / tokens de acceso**. 
1. Haga clic en **Generar**.
![Captura de pantalla de la página de tokens de acceso](media/howto-use-iotc-explorer/accesstokenspage.png)

1. Escriba un nombre para el token, haga clic en **Siguiente**, y **copie el valor del token**.
    > [!NOTE]
    > El valor del token solo se mostrará una vez, por lo que debe copiarlo antes de cerrar el cuadro de diálogo. Después de cerrar el cuadro de diálogo no lo volverá a ver más.

    ![Captura de pantalla del cuadro diálogo de la opción para copiar el token de acceso](media/howto-use-iotc-explorer/copyaccesstoken.png)

A continuación, puede usar ese token para iniciar sesión en la CLI; para ello, ejecute lo siguiente:

```sh
iotc-explorer login "<Token value>"
```

Si prefiere que el token no se guarde en su historial de shell, puede omitirlo y proporcionarlo cuando se le solicite:

```
iotc-explorer login
```

### <a name="monitor-device-messages"></a>Supervisar los mensajes del dispositivo

Puede ver los mensajes provenientes de un dispositivo específico o de todos los dispositivos en su aplicación mediante el comando `monitor-messages`. Esta opción iniciará un monitor que emitirá continuamente nuevos mensajes a medida que vayan llegando.

Para ver todos los dispositivos en la aplicación, ejecute el siguiente comando:

```
iotc-explorer monitor-messages
```
Salida: ![salida del comando monitor-messages](media/howto-use-iotc-explorer/monitormessages.PNG)

Para ver un dispositivo específico, simplemente agregue el id. de dispositivo al final del comando:

```
iotc-explorer monitor-messages <your-device-id>
```

También puede hacer que el comando genere un formato que sea más fácil de reconocer para la máquina si agrega la opción `--raw` al comando:

```
iotc-explorer monitor-messages --raw
```

### <a name="get-device-twin"></a>Obtener dispositivo gemelo

Puede usar el comando `get-twin` para obtener el contenido del dispositivo gemelo para un dispositivo de IoT Central. Para ello, ejecute el siguiente comando:

```
iotc-explorer get-twin <your-device-id>
```

Salida: ![salida del comando get-twin](media/howto-use-iotc-explorer/getdevicetwin.PNG)

Igual que sucede con `monitor-messages`, puede obtener un resultado que sea más fácil de reconocer para la máquina si usa la opción `--raw`:

```
iotc-explorer get-twin <your-device-id> --raw
```

## <a name="next-steps"></a>Pasos siguientes
Ahora que ya ha aprendido a usar IoT Central Explorer, el siguiente paso sugerido es explorar las opciones de [administración de dispositivos de IoT Central](howto-manage-devices.md).
