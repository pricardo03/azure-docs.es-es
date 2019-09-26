---
title: Instalación y uso Azure IoT Explorer | Microsoft Docs
description: Use la herramienta Azure IoT Explorer para interactuar con el dispositivo IoT Plug and Play (versión preliminar) conectado a su instancia de IoT Hub.
author: miagdp
ms.author: miag
ms.date: 07/02/2019
ms.topic: conceptual
ms.service: iot-pnp
services: iot-pnp
ms.custom: mvc
ms.openlocfilehash: 4e23a440f46b52633a88d0212e08c7b584f61a38
ms.sourcegitcommit: f3f4ec75b74124c2b4e827c29b49ae6b94adbbb7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/12/2019
ms.locfileid: "70932470"
---
# <a name="install-and-use-azure-iot-explorer"></a>Instalación y uso de Azure IoT Explorer

Azure IoT Explorer es una herramienta gráfica para interactuar con dispositivos IoT Plug and Play (versión preliminar) y probarlos. Después de instalar la herramienta en la máquina local, puede usarla para conectarse a un dispositivo. Puede usar la herramienta para ver la telemetría que el dispositivo envía, trabajar con propiedades de dispositivo y llamar a comandos.

Este artículo muestra cómo:

- Instale y configure la herramienta Azure IoT Explorer.
- Use la herramienta para interactuar con los dispositivos y probarlos.

## <a name="prerequisites"></a>Requisitos previos

Para usar la herramienta Azure IoT Explorer, necesitará lo siguiente:

- Un centro de Azure IoT. Hay muchas maneras de agregar una instancia de IoT Hub a la suscripción de Azure, como la [creación de un centro de IoT mediante la CLI de Azure](../iot-hub/iot-hub-create-using-cli.md). Necesitará la cadena de conexión de IoT Hub para ejecutar la herramienta Azure IoT Explorer. Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.
- Un dispositivo registrado en su centro de IoT. Puede usar comando de la CLI de Azure siguiente para registrar un dispositivo. Asegúrese de reemplazar los marcadores de posición `{YourIoTHubName}` y `{YourDeviceID}` por sus valores:

    ```azurecli-interactive
    az iot hub device-identity create --hub-name {YourIoTHubName} --device-id {YourDeviceID}
    ```

## <a name="install-azure-iot-explorer"></a>Instalación de Azure IoT Explorer

Vaya a las [versiones de Azure IoT Explorer](https://github.com/Azure/azure-iot-explorer/releases) y expanda la lista de recursos de la versión más reciente. Descargue e instale la versión más reciente de la aplicación.

## <a name="use-azure-iot-explorer"></a>Uso de Azure IoT Explorer

Para un dispositivo, puede conectar su propio dispositivo o usar uno de nuestros dispositivos simulados de ejemplo. Siga [estas instrucciones](https://github.com/Azure/azure-iot-sdk-c/tree/public-preview/iothub_client/samples) para ejecutar el ejemplo de dispositivo simulado.

### <a name="connect-to-your-hub"></a>Conexión con el centro

La primera vez que ejecute Azure IoT Explorer, se le solicitará la cadena de conexión de su centro de IoT. Después de agregar la cadena de conexión, seleccione **Connect** (Conectar). Puede usar la configuración de la herramienta para cambiar a otro centro de IoT mediante la actualización de la cadena de conexión.

La definición del modelo para un dispositivo IoT Plug and Play se almacena en el repositorio público, en el repositorio de la empresa o en el dispositivo conectado. De forma predeterminada, la herramienta busca la definición del modelo en el repositorio de modelos público y en el dispositivo conectado. Puede agregar y quitar orígenes, o configurar la prioridad de los orígenes en **Settings** (Configuración):

Para agregar un origen:

1. Vaya a **Configuración**.
1. Seleccione **New** (Nuevo) y elija el origen.
1. Si va a agregar el repositorio de modelos de empresa, proporcione la cadena de conexión.

Para quitar un origen:

1. Vaya a **Configuración**.
1. Busque el origen que desea quitar.
1. Seleccione **X** para quitarlo. No se puede quitar el repositorio de modelos público porque las definiciones de interfaz comunes provienen de este repositorio.

Cambie las prioridades de origen:

Puede arrastrar y colocar uno de los orígenes de la definición de modelo en otra clasificación de la lista. Si hay un conflicto, los orígenes de definición con clasificaciones superiores reemplazan los orígenes con clasificaciones inferiores.

### <a name="overview-page"></a>Página de información general

#### <a name="device-overview"></a>Información general del dispositivo

Cuando la herramienta se conecta al centro de IoT, muestra una página de información general que contiene todas las identidades de dispositivo registradas con Azure IoT Hub. Seleccione un dispositivo para ver más detalles.

#### <a name="device-management"></a>Administración de dispositivos

- Para registrar un nuevo dispositivo en el centro, seleccione **Add** (Agregar). Especifique un identificador de dispositivo. Use la configuración predeterminada para la generación automática de claves de autenticación y permitir la conexión con el centro.
- Para eliminar una identidad de dispositivo, seleccione **Delete** (Eliminar). Revise los detalles del dispositivo antes de completar esta acción para asegurarse de que está eliminando la identidad de dispositivo correcta.
- La herramienta admite la consulta mediante `capabilityID` y `interfaceID`. Agregue `capabilityID` o `interfaceID` como un parámetro para consultar los dispositivos.

## <a name="interact-with-a-device"></a>Interactuación con un dispositivo

Haga doble clic en un dispositivo en la página de información general para ver el siguiente nivel de detalle. Hay dos secciones: **Dispositivo** y **gemelo digital**.

### <a name="device"></a>Dispositivo

Esta sección incluye las pestañas **Device Identity** (Identidad del dispositivo), **Telemetry** (Telemetría) y **Device Twin** (Dispositivo gemelo).

- Puede ver y actualizar la información de la identidad del dispositivo en la pestaña **Device identity** (Identidad del dispositivo).
- Si un dispositivo está conectado y envía datos de forma activa, puede ver la telemetría en la pestaña **Telemetry** (Telemetría).
- Puede acceder a la información del dispositivo gemelo en la pestaña **Device Twin** (Dispositivo gemelo).

### <a name="digital-twin"></a>Gemelo digital

Puede usar la herramienta para ver una instancia del gemelo digital del dispositivo. En el caso de un dispositivo IoT Plug and Play, en este artículo se muestran todas las interfaces asociadas al modelo de funcionalidad del dispositivo. Seleccione una interfaz para expandir las correspondientes [primitivas de IoT Plug and Play](https://github.com/Azure/IoTPlugandPlay/tree/master/DTDL).

#### <a name="properties"></a>properties (Propiedades)

Puede ver las propiedades de solo lectura definidas en una interfaz en la página **Properties** (Propiedades). Puede actualizar las propiedades grabables definidas en una interfaz en la página **Writeable properties** (Propiedades que se pueden escribir).

1. Vaya a la página **Writeable properties** (Propiedades que se pueden escribir).
1. Haga clic en la propiedad que desea actualizar.
1. Introduzca el nuevo valor de la propiedad.
1. Obtenga una vista previa de la carga que se va a enviar al dispositivo.
1. Envíe el cambio.

Después de enviar un cambio, puede realizar un seguimiento del estado de actualización: **sincronizando**, **correcto** o **error**. Una vez completada la sincronización, verá el nuevo valor de la propiedad en la columna **Reported Property** (Propiedad notificada). Si se desplaza a otras páginas antes de que se complete la sincronización, la herramienta le notificará cuando se complete la actualización. También puede usar el centro de notificaciones de la herramienta para ver el historial de notificaciones.

#### <a name="commands"></a>Comandos:

Para enviar un comando a un dispositivo, vaya a la página **Commands** (Comandos):

1. En la lista de comandos, expanda el comando que desea desencadenar.
1. Escriba los valores requeridos para el comando.
1. Obtenga una vista previa de la carga que se va a enviar al dispositivo.
1. Envíe el comando.

#### <a name="telemetry"></a>Telemetría

Para ver la telemetría de la interfaz seleccionada, vaya a su página **Telemetry** (Telemetría).

## <a name="next-steps"></a>Pasos siguientes

En este artículo de procedimientos, ha aprendido a instalar y usar Azure IoT Explorer para interactuar con los dispositivos IoT Plug and Play. El siguiente paso sugerido es obtener información sobre cómo [instalar y usar la extensión de la CLI de Azure](./howto-install-pnp-cli.md).
