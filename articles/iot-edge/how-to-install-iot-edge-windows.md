---
title: Instalación de Azure IoT Edge en Windows | Microsoft Docs
description: Instrucciones de instalación de Azure IoT Edge en Windows 10, Windows Server y Windows IoT Core
author: kgremban
manager: philmea
ms.reviewer: veyalla
ms.service: iot-edge
services: iot-edge
ms.topic: conceptual
ms.date: 05/06/2019
ms.author: kgremban
ms.custom: seodec18
ms.openlocfilehash: 8907ae61fb03b417a74eb32e1fd09aece75d5e2c
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/23/2019
ms.locfileid: "66151715"
---
# <a name="install-the-azure-iot-edge-runtime-on-windows"></a>Instalación del entorno de ejecución de Azure IoT Edge en Windows

El entorno de ejecución de Azure IoT Edge es lo que convierte a un dispositivo en un dispositivo IoT Edge. El entorno de ejecución se puede implementar en dispositivos tan pequeños como un Raspberry Pi o tan grandes como un servidor industrial. Una vez que un dispositivo está configurado con el entorno de ejecución de IoT Edge, puede empezar a implementar lógica de negocios en él desde la nube. 

Para obtener más información sobre el entorno de ejecución de IoT Edge, vea [Información del entorno de ejecución de Azure IoT Edge y su arquitectura](iot-edge-runtime.md).

En este artículo se enumera los pasos para instalar el runtime de Azure IoT Edge en su Windows x64 (AMD e Intel) mediante contenedores de Windows del sistema.

> [!NOTE]
> Un problema conocido de sistema operativo de Windows impide que la transición para de suspensión e hibernación Estados de energía cuando se ejecutan los módulos de IoT Edge (contenedores de Windows Nano Server con aislamiento de proceso). Este problema afecta a la duración de la batería del dispositivo.
>
> Como alternativa, use el comando `Stop-Service iotedge` detener cualquier ejecución de módulos de IoT Edge antes de usar estos Estados de energía. 

<!--
> [!NOTE]
> Using Linux containers on Windows systems is not a recommended or supported production configuration for Azure IoT Edge. However, it can be used for development and testing purposes.
-->

En la configuración de producción de Azure IoT Edge, no se recomienda ni se permite utilizar contenedores Linux en sistemas Windows. Sin embargo, se puede usar con fines de desarrollo y pruebas. Para obtener más información, consulte [Use IoT Edge en Windows para ejecutar contenedores de Linux](how-to-install-iot-edge-windows-with-linux.md).

Para obtener información acerca de qué se incluye en la versión más reciente de IoT Edge, consulte [Azure IoT Edge libera](https://github.com/Azure/azure-iotedge/releases).

## <a name="prerequisites"></a>Requisitos previos

Utilice esta sección para revisar si el dispositivo Windows puede admitir IoT Edge y para prepararlo para un motor de contenedor antes de la instalación. 

### <a name="supported-windows-versions"></a>Versiones de Windows admitidas

Para escenarios de desarrollo y pruebas, Azure IoT Edge con contenedores de Windows puede instalarse en cualquier versión de Windows 10 o Windows Server 2019 (compilación 17763) que admite la característica de contenedores. Para obtener información sobre qué sistemas operativos se admiten actualmente para escenarios de producción, consulte [sistemas compatibles con Azure IoT Edge](support.md#operating-systems). 

### <a name="prepare-for-a-container-engine"></a>Preparación de un motor de contenedor 

Azure IoT Edge se basa en un motor de contenedor [compatible con OCI](https://www.opencontainers.org/). Para escenarios de producción, use el motor de Moby incluido en el script de instalación para ejecutar contenedores de Windows en el dispositivo Windows. 

## <a name="install-iot-edge-on-a-new-device"></a>Instalación de IoT Edge en un dispositivo nuevo

>[!NOTE]
>Los paquetes de software de Azure IoT Edge están sujetos a los términos de licencia ubicados en los paquetes (en el directorio LICENSE). Lea los términos de licencia antes de usar el paquete. La instalación y uso del paquete constituye la aceptación de estos términos. Si no acepta los términos de licencia, no utilice el paquete.

Un script de PowerShell descarga e instala el demonio de seguridad de Azure IoT Edge. Después, el demonio de seguridad inicia el primero de los dos módulos en tiempo de ejecución, el agente de IoT Edge, que permite implementaciones remotas de otros módulos. 

Cuando instale por primera vez el tiempo de ejecución de IoT Edge en un dispositivo, debe aprovisionar el dispositivo con una identidad de un IoT hub. Un único dispositivo de IoT Edge se puede aprovisionar manualmente mediante una cadena de conexión de dispositivo proporcionada por IoT Hub. O bien, puede usar el servicio Device Provisioning para aprovisionar dispositivos automáticamente, lo que resulta útil cuando tiene muchos dispositivos para configurar. Según la elección de aprovisionamiento, elija el script de instalación adecuado. 

En las siguientes secciones se describen los casos de uso comunes y los parámetros para el script de instalación de IoT Edge en un dispositivo nuevo. 

### <a name="option-1-install-and-manually-provision"></a>Opción 1: Instalación y aprovisionamiento manual

En esta primera opción, se proporciona un **cadena de conexión de dispositivo** generados por IoT Hub para aprovisionar el dispositivo. 

En este ejemplo se muestra una instalación manual con contenedores de Windows:

1. Si no lo ha hecho ya, registre un nuevo dispositivo de IoT Edge y recuperar el **cadena de conexión de dispositivo**. Copie la cadena de conexión para usarlo más adelante en esta sección. Puede completar este paso con las siguientes herramientas:

   * [Azure Portal](how-to-register-device-portal.md)
   * [CLI de Azure](how-to-register-device-cli.md)
   * [Visual Studio Code](how-to-register-device-vscode.md)

2. Ejecute PowerShell como administrador.

   >[!NOTE]
   >Utilice una sesión AMD64 de PowerShell para instalar IoT Edge, no PowerShell (x86). Si no está seguro de qué tipo de sesión que está usando, ejecute el siguiente comando:
   >
   >```powershell
   >(Get-Process -Id $PID).StartInfo.EnvironmentVariables["PROCESSOR_ARCHITECTURE"]
   >```

3. El **implementar IoTEdge** comando comprueba que el equipo de Windows está en una versión compatible, activa la característica de contenedores y, a continuación, descarga el tiempo de ejecución moby y el tiempo de ejecución de IoT Edge. De forma predeterminada, el comando usa contenedores de Windows. 

   ```powershell
   . {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; `
   Deploy-IoTEdge
   ```

4. En este momento, los dispositivos de IoT Core pueden reiniciarse automáticamente. Otros dispositivos Windows 10 o Windows Server pueden pedirle que reinicie. Si es así, reinicie ahora el dispositivo. Una vez que el dispositivo está listo, ejecute PowerShell como administrador de nuevo.

5. El comando **Initialize-IoTEdge** configura el entorno de ejecución de Azure IoT Edge en el equipo. El comportamiento predeterminado del comando es el aprovisionamiento manual con contenedores de Windows. 

   ```powershell
   . {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; `
   Initialize-IoTEdge
   ```

6. Cuando se le solicite, proporcione la cadena de conexión de dispositivo que recuperó en el paso 1. La cadena de conexión del dispositivo, asocia el dispositivo físico con un identificador de dispositivo en IoT Hub. 

   La cadena de conexión de dispositivo tiene el formato siguiente y no debe incluir entre comillas: `HostName={IoT hub name}.azure-devices.net;DeviceId={device name};SharedAccessKey={key}`

7. Siga los pasos de [comprobar la correcta instalación](#verify-successful-installation) para comprobar el estado de IoT Edge en el dispositivo. 

Al instalar y aprovisionar un dispositivo manualmente, puede usar parámetros adicionales para modificar la instalación, incluyendo:
* Dirigir el tráfico para que pase por un servidor proxy
* Apuntar el instalador a un directorio sin conexión
* Declarar una imagen de contenedor del agente específica y proporcionar las credenciales si se encuentra en un registro privado

Para obtener más información acerca de estas opciones de instalación, vaya directamente para obtener información acerca de [todos los parámetros de instalación](#all-installation-parameters).

### <a name="option-2-install-and-automatically-provision"></a>Opción 2: Instalación y aprovisionamiento automático

En esta segunda opción, aprovisionará el dispositivo mediante el servicio IoT Hub Device Provisioning. Proporcione el **Id. de ámbito** de una instancia del servicio Device Provisioning y el **Id. de registro** del dispositivo.

El ejemplo siguiente muestra una instalación automática con contenedores de Windows:

1. Siga los pasos de [creación y aprovisionamiento de un dispositivo de TPM IoT Edge simulado en Windows](how-to-auto-provision-simulated-device-windows.md) para configurar el servicio Device Provisioning y recuperar su **Id. de ámbito**, simular un dispositivo de TPM y recuperar su **Id. de registro**, a continuación, crear una inscripción individual. Una vez que el dispositivo está registrado en IoT hub, continúe con estos pasos de instalación.  

   >[!TIP]
   >Mantenga abierta la ventana que se está ejecutando el simulador de TPM durante la instalación y prueba. 

2. Ejecute PowerShell como administrador.

   >[!NOTE]
   >Utilice una sesión AMD64 de PowerShell para instalar IoT Edge, no PowerShell (x86). Si no está seguro de qué tipo de sesión que está usando, ejecute el siguiente comando:
   >
   >```powershell
   >(Get-Process -Id $PID).StartInfo.EnvironmentVariables["PROCESSOR_ARCHITECTURE"]
   >```

3. El **implementar IoTEdge** comando comprueba que el equipo de Windows está en una versión compatible, activa la característica de contenedores y, a continuación, descarga el tiempo de ejecución moby y el tiempo de ejecución de IoT Edge. De forma predeterminada, el comando usa contenedores de Windows. 

   ```powershell
   . {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; `
   Deploy-IoTEdge
   ```

4. En este momento, los dispositivos de IoT Core pueden reiniciarse automáticamente. Otros dispositivos Windows 10 o Windows Server pueden pedirle que reinicie. Si es así, reinicie ahora el dispositivo. Una vez que el dispositivo está listo, ejecute PowerShell como administrador de nuevo.

6. El comando **Initialize-IoTEdge** configura el entorno de ejecución de Azure IoT Edge en el equipo. El comportamiento predeterminado del comando es el aprovisionamiento manual con contenedores de Windows. Utilice el `-Dps` marca para usar el servicio Device Provisioning en lugar de aprovisionamiento manual.

   ```powershell
   . {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; `
   Initialize-IoTEdge -Dps
   ```

7. Cuando se le solicite, proporcione el identificador de ámbito de su servicio Device Provisioning y el identificador de registro del dispositivo, que los cuales debe recuperada en el paso 1.

8. Siga los pasos de [comprobar la correcta instalación](#verify-successful-installation) para comprobar el estado de IoT Edge en el dispositivo. 

Al instalar y aprovisionar un dispositivo manualmente, puede usar parámetros adicionales para modificar la instalación, incluyendo:
* Dirigir el tráfico para que pase por un servidor proxy
* Apuntar el instalador a un directorio sin conexión
* Declarar una imagen de contenedor del agente específica y proporcionar las credenciales si se encuentra en un registro privado

Para obtener más información sobre estas opciones de instalación, continúe leyendo este artículo o salte para obtener información sobre [Todos los parámetros de instalación](#all-installation-parameters).

## <a name="offline-installation"></a>Instalación sin conexión

Durante la instalación se descargan los dos archivos: 
* Archivo cab de Microsoft Azure IoT Edge, que contiene el demonio de seguridad de IoT Edge (iotedged), motor de contenedor Moby y Moby CLI.
* Un archivo msi del paquete redistribuible de Visual C++ (tiempo de ejecución de VC)

Puede descargar uno o ambos de estos archivos antes de tiempo en el dispositivo y luego seleccione el script de instalación en el directorio que contiene los archivos. El instalador comprueba en primer lugar el directorio y luego descarga solo los componentes que no se pueden encontrar. Si todos los archivos están disponibles sin conexión, puede instalar sin conexión a internet. También puede usar esta característica para instalar una versión específica de los componentes.  

Para los archivos de instalación de IoT Edge más recientes junto con las versiones anteriores, consulte [Azure IoT Edge libera](https://github.com/Azure/azure-iotedge/releases).

Para instalar con componentes sin conexión, use el `-OfflineInstallationPath` parámetro como parte de la implementación-IoTEdge comando y proporcione la ruta de acceso absoluta al directorio de archivos. Por ejemplo,

```powershell
. {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; `
Deploy-IoTEdge -OfflineInstallationPath C:\Downloads\iotedgeoffline
```

También puede usar el parámetro de ruta de acceso de instalación sin conexión con el comando Update-IoTEdge, introducido más adelante en este artículo. 

## <a name="verify-successful-installation"></a>Comprobación de instalación correcta

Compruebe el estado del servicio IoT Edge. Esta debe aparecer como en ejecución.  

```powershell
Get-Service iotedge
```

Examine los registros de servicio de los últimos 5 minutos. Si acaba de instalar el runtime de IoT Edge, puede ver una lista de errores desde el tiempo transcurrido entre la ejecución **implementar IoTEdge** y **Initialize IoTEdge**. Se esperan que estos errores, como el servicio está intentando iniciar antes de que se está configurando. 

```powershell
. {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; Get-IoTEdgeLog
```

Enumere los módulos en ejecución. Después de una instalación nueva, el módulo solo debería ver la ejecución es **edgeAgent**. Después de [implementar módulos IoT Edge](how-to-deploy-modules-portal.md), verá que otros usuarios. 

```powershell
iotedge list
```

## <a name="manage-module-containers"></a>Administrar contenedores de módulo

El servicio de IoT Edge requiere un motor de contenedor que se ejecuta en el dispositivo. Al implementar un módulo en un dispositivo, el tiempo de ejecución de IoT Edge utiliza el motor de contenedor para extraer la imagen de contenedor de un registro en la nube. El servicio de IoT Edge le permite interactuar con los módulos y recuperar los registros, pero a veces es posible que desea usar el motor de contenedor para interactuar con el propio contenedor. 

Para obtener más información sobre los conceptos de módulo, consulte [módulos de Azure IoT Edge comprender](iot-edge-modules.md). 

Si ejecutas contenedores de Windows en el dispositivo Windows IoT Edge, la instalación de IoT Edge incluye el motor de contenedor Moby. El motor de Moby se basaba en los mismos estándares que Docker y estaba diseñado para ejecutarse en paralelo en el mismo equipo que el escritorio de Docker. Por ese motivo, si desea agregar contenedores de destino que administra el motor Moby, tiene como destino específicamente que el motor en lugar de Docker. 

Por ejemplo, para enumerar todas las imágenes de Docker, use el siguiente comando:

```powershell
docker images
```

Para obtener una lista de todas las imágenes de Moby, modifique el mismo comando con un puntero al motor de Moby: 

```powershell
docker -H npipe:////./pipe/iotedge_moby_engine images
```

El motor de URI se muestra en la salida del script de instalación, o puede encontrarlo en la sección de configuración de contenedor en tiempo de ejecución para el archivo config.yaml. 

![uri de moby_runtime en config.yaml](./media/how-to-install-iot-edge-windows/moby-runtime-uri.png)

Para obtener más información acerca de los comandos que se puede usar para interactuar con contenedores e imágenes que se ejecutan en el dispositivo, consulte [interfaces de línea de comandos de Docker](https://docs.docker.com/engine/reference/commandline/docker/).

## <a name="update-an-existing-installation"></a>Actualización de una instalación existente

Si ya ha instalado el runtime de IoT Edge en un dispositivo antes y lo aprovisionó con una identidad de IoT Hub, puede actualizar el tiempo de ejecución sin tener que volver a escribir la información del dispositivo. 

Para obtener más información, vea [Actualice el archivo de configuración del demonio de seguridad y el entorno de ejecución de IoT Edge](how-to-update-iot-edge.md).

En este ejemplo se muestra una instalación que apunta a un archivo de configuración existente y usa contenedores de Windows: 

```powershell
. {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; `
Update-IoTEdge
```

Cuando se actualiza de IoT Edge, puede usar parámetros adicionales para modificar la actualización, incluidos:
* Dirigir el tráfico a pasar por un servidor proxy, o
* Apuntar el instalador a un directorio sin conexión 
* Reiniciar sin preguntar al usuario si es necesario

No se puede declarar una imagen de contenedor del agente de IoT Edge con los parámetros de script porque esa información ya está establecida en el archivo de configuración de la instalación anterior. Si desea modificar la imagen del contenedor del agente, debe hacerlo en el archivo config.yaml. 

Para obtener más información sobre estas opciones de actualización, use el comando `Get-Help Update-IoTEdge -full` o hacer referencia a [todos los parámetros de instalación](#all-installation-parameters).

## <a name="uninstall-iot-edge"></a>Desinstalación de IoT Edge

Si quiere quitar la instalación de IoT Edge del dispositivo Windows, use el siguiente comando de una ventana de PowerShell administrativa. Este comando quita el entorno de ejecución de IoT Edge, junto con la configuración existente y los datos de motor de Moby. 

```powershell
Uninstall-IoTEdge
```

El comando de desinstalación IoTEdge no funciona en Windows IoT Core. Para quitar IoT Edge desde los dispositivos de Windows IoT Core, debe volver a la imagen de Windows IoT Core. 

Para obtener más información acerca de las opciones de desinstalación, use el comando `Get-Help Uninstall-IoTEdge -full`. 

## <a name="all-installation-parameters"></a>Todos los parámetros de instalación

En las secciones anteriores se presentaron escenarios de instalación comunes con ejemplos de cómo usar parámetros para modificar el script de instalación. Esta sección proporciona las tablas de referencia de los parámetros comunes utilizados para instalar, actualizar o desinstalar IoT Edge. 

### <a name="deploy-iotedge"></a>Deploy-IoTEdge

El comando implementar IoTEdge descarga e implementa el demonio de seguridad de IoT Edge y sus dependencias. El comando de implementación acepta estos parámetros comunes, entre otros. Para obtener la lista completa, use el comando `Get-Help Deploy-IoTEdge -full`.  

| Parámetro | Valores aceptados | Comentarios |
| --------- | --------------- | -------- |
| **ContainerOs** | **Windows** o **Linux** | Si no se especifica ningún sistema operativo de contenedor, Windows es el valor predeterminado.<br><br>Para los contenedores de Windows, IoT Edge utiliza el motor de contenedor de moby incluido en la instalación. Para los contenedores de Linux, deberá instalar un motor de contenedor antes de iniciar la instalación. |
| **Proxy** | Dirección URL del proxy | Incluya este parámetro si el dispositivo debe pasar por un servidor proxy para conectarse a Internet. Para más información, consulte [Configuración de un dispositivo de IoT Edge para que se comunique a través de un servidor proxy](how-to-configure-proxy-support.md). |
| **OfflineInstallationPath** | Ruta de acceso del directorio | Si se incluye este parámetro, el instalador comprobará el directorio indicado para los archivos de MSI en tiempo de ejecución de VC necesarios para la instalación y el archivo cab de IoT Edge. Se descargan los archivos no encontrados en el directorio. Si ambos archivos se encuentran en el directorio, puede instalar IoT Edge sin una conexión a internet. También puede usar este parámetro para usar una versión específica. |
| **InvokeWebRequestParameters** | Tabla de hash de parámetros y valores | Durante la instalación se realizan varias solicitudes web. Utilice este campo para definir parámetros para esas solicitudes web. Este parámetro es útil para configurar las credenciales para los servidores proxy. Para más información, consulte [Configuración de un dispositivo de IoT Edge para que se comunique a través de un servidor proxy](how-to-configure-proxy-support.md). |
| **RestartIfNeeded** | ninguno | Esta marca permite que el script de implementación reiniciar el equipo sin preguntar, si es necesario. |

### <a name="initialize-iotedge"></a>Inicializar IoTEdge

El comando Initialize IoTEdge configura IoT Edge con su cadena de conexión del dispositivo y los detalles operativos. Gran parte de la información generada por este comando, a continuación, se almacena en el archivo iotedge\config.yaml. El comando de inicialización acepta estos parámetros comunes, entre otros. Para obtener la lista completa, use el comando `Get-Help Initialize-IoTEdge -full`. 

| Parámetro | Valores aceptados | Comentarios |
| --------- | --------------- | -------- |
| **Manual** | None | **Parámetro de modificador**. Si no se especifica ningún tipo de aprovisionamiento, el valor predeterminado es manual.<br><br>Declara que usted proporcionará una cadena de conexión de dispositivo para aprovisionar el dispositivo manualmente |
| **Dps** | None | **Parámetro de modificador**. Si no se especifica ningún tipo de aprovisionamiento, el valor predeterminado es manual.<br><br>Declara que usted proporcionará un Id. de ámbito de Device Provisioning Service (DPS) y el Id. de registro del dispositivo para aprovisionar a través de DPS.  |
| **DeviceConnectionString** | Una cadena de conexión desde un dispositivo de IoT Edge registrado en IoT Hub, entre comillas simples | **Obligatorio** para la instalación manual. Si no proporciona una cadena de conexión en los parámetros del script, se le pedirá una durante la instalación. |
| **ScopeId** | Un Id. de ámbito de una instancia de Device Provisioning Service asociada con IoT Hub. | **Obligatorio** para la instalación de DPS. Si no proporciona un Id. de ámbito en los parámetros del script, se le pedirá uno durante la instalación. |
| **RegistrationId** | Un identificador de registro generado por el dispositivo | **Obligatorio** para la instalación de DPS. Si no proporciona un Id. de registro en los parámetros del script, se le pedirá uno durante la instalación. |
| **ContainerOs** | **Windows** o **Linux** | Si no se especifica ningún sistema operativo de contenedor, Windows es el valor predeterminado.<br><br>Para los contenedores de Windows, IoT Edge utiliza el motor de contenedor de moby incluido en la instalación. Para los contenedores de Linux, deberá instalar un motor de contenedor antes de iniciar la instalación. |
| **InvokeWebRequestParameters** | Tabla de hash de parámetros y valores | Durante la instalación se realizan varias solicitudes web. Utilice este campo para definir parámetros para esas solicitudes web. Este parámetro es útil para configurar las credenciales para los servidores proxy. Para más información, consulte [Configuración de un dispositivo de IoT Edge para que se comunique a través de un servidor proxy](how-to-configure-proxy-support.md). |
| **AgentImage** | URI de la imagen del agente de IoT Edge | De forma predeterminada, una nueva instalación de IoT Edge utiliza la etiqueta gradual más reciente de la imagen del agente de IoT Edge. Utilice este parámetro para establecer una etiqueta específica para la versión de la imagen o para proporcionar su propia imagen de agente. Para obtener más información, vea [Información sobre las etiquetas de IoT Edge](how-to-update-iot-edge.md#understand-iot-edge-tags). |
| **Nombre de usuario** | Nombre de usuario de registro de contenedor | Utilice este parámetro solo si establece el parámetro -AgentImage en un contenedor en un registro privado. Proporcione un nombre de usuario con acceso al registro. |
| **Contraseña** | Cadena de contraseña segura | Utilice este parámetro solo si establece el parámetro -AgentImage en un contenedor en un registro privado. Proporcione la contraseña para acceder al registro. | 

### <a name="update-iotedge"></a>Update-IoTEdge

| Parámetro | Valores aceptados | Comentarios |
| --------- | --------------- | -------- |
| **ContainerOs** | **Windows** o **Linux** | Si no se especifica el sistema operativo de contenedor, Windows es el valor predeterminado. Para los contenedores de Windows, se incluirá un motor de contenedor en la instalación. Para los contenedores de Linux, deberá instalar un motor de contenedor antes de iniciar la instalación. |
| **Proxy** | Dirección URL del proxy | Incluya este parámetro si el dispositivo debe pasar por un servidor proxy para conectarse a Internet. Para más información, consulte [Configuración de un dispositivo de IoT Edge para que se comunique a través de un servidor proxy](how-to-configure-proxy-support.md). |
| **InvokeWebRequestParameters** | Tabla de hash de parámetros y valores | Durante la instalación se realizan varias solicitudes web. Utilice este campo para definir parámetros para esas solicitudes web. Este parámetro es útil para configurar las credenciales para los servidores proxy. Para más información, consulte [Configuración de un dispositivo de IoT Edge para que se comunique a través de un servidor proxy](how-to-configure-proxy-support.md). |
| **OfflineInstallationPath** | Ruta de acceso del directorio | Si se incluye este parámetro, el instalador comprobará el directorio indicado para los archivos de MSI en tiempo de ejecución de VC necesarios para la instalación y el archivo cab de IoT Edge. Se descargan los archivos no encontrados en el directorio. Si ambos archivos se encuentran en el directorio, puede instalar IoT Edge sin una conexión a internet. También puede usar este parámetro para usar una versión específica. |
| **RestartIfNeeded** | ninguno | Esta marca permite que el script de implementación reiniciar el equipo sin preguntar, si es necesario. |


### <a name="uninstall-iotedge"></a>Uninstall-IoTEdge

| Parámetro | Valores aceptados | Comentarios |
| --------- | --------------- | -------- |
| **Force** | ninguno | Esta marca fuerza la desinstalación en caso de que el intento anterior para desinstalar no tuvo éxito. 
| **RestartIfNeeded** | ninguno | Esta marca permite que el script de desinstalación reiniciar el equipo sin preguntar, si es necesario. |


## <a name="next-steps"></a>Pasos siguientes

Ahora que tiene un dispositivo IoT Edge aprovisionado con el entorno de ejecución instalado, puede [implementar módulos de IoT Edge](how-to-deploy-modules-portal.md).

Si tiene problemas con la instalación correcta de IoT Edge, vea la página de [solución de problemas](troubleshoot.md).

Para actualizar una instalación existente a la versión más reciente de IoT Edge, vea [Actualice el archivo de configuración del demonio de seguridad y el entorno de ejecución de IoT Edge](how-to-update-iot-edge.md).
