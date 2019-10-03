---
title: Instalación de Azure IoT Edge en Windows | Microsoft Docs
description: Instrucciones de instalación de Azure IoT Edge en Windows 10, Windows Server y Windows IoT Core
author: kgremban
manager: philmea
ms.reviewer: veyalla
ms.service: iot-edge
services: iot-edge
ms.topic: conceptual
ms.date: 07/10/2019
ms.author: kgremban
ms.custom: seodec18
ms.openlocfilehash: 6118c4ddf1386ff4cc816148938e1f5ddeaecc9e
ms.sourcegitcommit: 3f22ae300425fb30be47992c7e46f0abc2e68478
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/25/2019
ms.locfileid: "71266075"
---
# <a name="install-the-azure-iot-edge-runtime-on-windows"></a>Instalación del entorno de ejecución de Azure IoT Edge en Windows

El entorno de ejecución de Azure IoT Edge es lo que convierte a un dispositivo en un dispositivo IoT Edge. El entorno de ejecución se puede implementar en dispositivos tan pequeños como un Raspberry Pi o tan grandes como un servidor industrial. Una vez que un dispositivo está configurado con el entorno de ejecución de IoT Edge, puede empezar a implementar lógica de negocios en él desde la nube. 

Para obtener más información sobre el entorno de ejecución de IoT Edge, vea [Información del entorno de ejecución de Azure IoT Edge y su arquitectura](iot-edge-runtime.md).

En este artículo se enumeran los pasos necesarios para instalar el runtime de Azure IoT Edge en un sistema Windows x64 (AMD e Intel) mediante contenedores Windows.

> [!NOTE]
> Un problema conocido del sistema operativo Windows impide la transición a los estados de suspensión e hibernación cuando se ejecutan módulos de IoT Edge (contenedores de Windows Nano Server con procesos aislados). Este problema afecta a la duración de la batería del dispositivo.
>
> Como alternativa, use el comando `Stop-Service iotedge` para detener todos los módulos de IoT Edge en ejecución antes de usar estos estados. 

En la configuración de producción de Azure IoT Edge, no se recomienda ni se permite utilizar contenedores Linux en sistemas Windows. Sin embargo, se puede usar con fines de desarrollo y pruebas. Para más información, consulte [Uso de IoT Edge en Windows para ejecutar contenedores de Linux](how-to-install-iot-edge-windows-with-linux.md).

Para obtener información acerca de lo que se incluye en la versión más reciente de IoT Edge, vea las [versiones de Azure IoT Edge](https://github.com/Azure/azure-iotedge/releases).

## <a name="prerequisites"></a>Requisitos previos

Utilice esta sección para revisar si el dispositivo Windows puede admitir IoT Edge y para prepararlo para un motor de contenedor antes de la instalación. 

### <a name="supported-windows-versions"></a>Versiones de Windows admitidas

En los escenarios de desarrollo y pruebas, Azure IoT Edge con contenedores Windows puede instalarse en cualquier versión de Windows 10 o Windows Server 2019 (compilación 17763) que admita la característica de contenedores. Para obtener información acerca de qué sistemas operativos se admiten actualmente en los escenarios de producción, consulte [Sistemas compatibles con Azure IoT Edge](support.md#operating-systems). 

Los dispositivos IoT Core deben incluir la característica opcional de contenedores de Windows IoT Core para admitir el tiempo de ejecución de IoT Edge. Use el siguiente comando en una [sesión remota de PowerShell](https://docs.microsoft.com/windows/iot-core/connect-your-device/powershell) para comprobar que los contenedores de Windows son compatibles con el dispositivo: 

```powershell
Get-Service vmcompute
```

Si el servicio está presente, debería obtener una respuesta correcta con el estado del servicio indicado como **en ejecución**. Si no se encuentra el servicio vmcompute, el dispositivo no cumple los requisitos de IoT Edge. Póngase en contacto con su proveedor de hardware para consultar la compatibilidad con esta característica. 

### <a name="prepare-for-a-container-engine"></a>Preparación de un motor de contenedor 

Azure IoT Edge se basa en un motor de contenedor [compatible con OCI](https://www.opencontainers.org/). Para escenarios de producción, use el motor de Moby incluido en el script de instalación para ejecutar contenedores de Windows en el dispositivo Windows. 

## <a name="install-iot-edge-on-a-new-device"></a>Instalación de IoT Edge en un dispositivo nuevo

>[!NOTE]
>Los paquetes de software de Azure IoT Edge están sujetos a los términos de licencia ubicados en los paquetes (en el directorio LICENSE). Lea los términos de licencia antes de usar el paquete. La instalación y uso del paquete constituye la aceptación de estos términos. Si no acepta los términos de licencia, no utilice el paquete.

Un script de PowerShell descarga e instala el demonio de seguridad de Azure IoT Edge. Después, el demonio de seguridad inicia el primero de los dos módulos en tiempo de ejecución, el agente de IoT Edge, que permite implementaciones remotas de otros módulos. 

>[!TIP]
>En el caso de los dispositivos IoT Core, se recomienda ejecutar los comandos de instalación mediante una sesión de RemotePowerShell. Para más información, consulte [Uso de PowerShell para Windows IoT](https://docs.microsoft.com/windows/iot-core/connect-your-device/powershell).

Cuando instale por primera vez el tiempo de ejecución de IoT Edge en un dispositivo, debe aprovisionar el dispositivo con una identidad de un IoT hub. Un dispositivo IoT Edge individual se puede aprovisionar manualmente mediante una cadena de conexión de dispositivo que proporciona IoT Hub. O bien, puede usar el servicio Device Provisioning (DPS) para aprovisionar dispositivos automáticamente, lo que resulta útil cuando tiene muchos dispositivos para configurar. Según la elección de aprovisionamiento, elija el script de instalación adecuado. 

En las siguientes secciones se describen los casos de uso comunes y los parámetros para el script de instalación de IoT Edge en un dispositivo nuevo. 

### <a name="option-1-install-and-manually-provision"></a>Opción 1: Instalación y aprovisionamiento manual

En esta primera opción, se proporciona una **cadena de conexión de dispositivo** generada por IoT Hub para aprovisionar el dispositivo. 

En este ejemplo se muestra una instalación manual con contenedores Windows:

1. Si aún no lo ha hecho, registre un nuevo dispositivo IoT Edge y recupere la **cadena de conexión del dispositivo**. Copie la cadena de conexión para usarla más adelante en esta sección. Este paso se puede completar con las siguientes herramientas:

   * [Azure Portal](how-to-register-device-portal.md)
   * [CLI de Azure](how-to-register-device-cli.md)
   * [Visual Studio Code](how-to-register-device-vscode.md)

2. Ejecute PowerShell como administrador.

   >[!NOTE]
   >Use una sesión de AMD64 de PowerShell para instalar IoT Edge, no PowerShell (x86). Si no está seguro de qué tipo de sesión usa, ejecute el comando siguiente:
   >
   >```powershell
   >(Get-Process -Id $PID).StartInfo.EnvironmentVariables["PROCESSOR_ARCHITECTURE"]
   >```

3. El comando **Deploy-IoTEdge** comprueba si la versión del equipo Windows es compatible, activa la característica de contenedores y descarga tanto el runtime de Moby como el de IoT Edge. De forma predeterminada el comando usa contenedores Windows. 

   ```powershell
   . {Invoke-WebRequest -useb https://aka.ms/iotedge-win} | Invoke-Expression; `
   Deploy-IoTEdge
   ```

4. En este momento, los dispositivos IoT Core pueden reiniciarse automáticamente. Es posible que otros dispositivos Windows 10 o Windows Server soliciten su reinicio. En ese caso, reinícielo ahora. Una vez que el dispositivo esté listo, vuelva a ejecutar PowerShell como administrador.

5. El comando **Initialize-IoTEdge** configura el entorno de ejecución de Azure IoT Edge en el equipo. El comportamiento predeterminado del comando es el aprovisionamiento manual con contenedores de Windows. 

   ```powershell
   . {Invoke-WebRequest -useb https://aka.ms/iotedge-win} | Invoke-Expression; `
   Initialize-IoTEdge
   ```

6. Cuando se solicite, especifique la cadena de conexión del dispositivo que recuperó en el paso 1. La cadena de conexión del dispositivo asocia el dispositivo físico con un identificador de dispositivo de IoT Hub. 

   La cadena de conexión del dispositivo adopta el formato siguiente y no debe incluir comillas: `HostName={IoT hub name}.azure-devices.net;DeviceId={device name};SharedAccessKey={key}`

7. Siga los pasos de [Comprobación de instalación correcta](#verify-successful-installation) para comprobar el estado de IoT Edge en el dispositivo. 

Al instalar y aprovisionar un dispositivo manualmente, puede usar parámetros adicionales para modificar la instalación, incluyendo:

* Dirigir el tráfico para que pase por un servidor proxy
* Apuntar el instalador a un directorio sin conexión
* Declarar una imagen de contenedor del agente específica y proporcionar las credenciales si se encuentra en un registro privado

Para más información acerca de estas opciones de instalación, más adelante encontrará [todos los parámetros de instalación](#all-installation-parameters).

### <a name="option-2-install-and-automatically-provision"></a>Opción 2: Instalación y aprovisionamiento automático

En esta segunda opción, aprovisionará el dispositivo mediante el servicio IoT Hub Device Provisioning. Proporcione el **Id. de ámbito** de una instancia del servicio Device Provisioning y el **Id. de registro** del dispositivo. Es posible que se requieran valores adicionales en función del mecanismo de atestación al aprovisionar con DPS, como cuando se usan [claves simétricas](how-to-auto-provision-symmetric-keys.md).

En el siguiente ejemplo se muestra una instalación automática con contenedores Windows y atestación TPM:

1. Siga los pasos de [Creación y aprovisionamiento de un dispositivo TPM IoT Edge simulado en Windows](how-to-auto-provision-simulated-device-windows.md) para configurar el servicio Device Provisioning y recuperar su **identificador de ámbito**, simular un dispositivo TPM y recuperar su **identificador de registro**y, finalmente, cree una inscripción individual. Una vez que el dispositivo esté registrado en IoT Hub, continúe con estos pasos de instalación.  

   >[!TIP]
   >Mantenga abierta la ventana que se está ejecutando el simulador de TPM durante la instalación y prueba. 

1. Ejecute PowerShell como administrador.

   >[!NOTE]
   >Use una sesión de AMD64 de PowerShell para instalar IoT Edge, no PowerShell (x86). Si no está seguro de qué tipo de sesión usa, ejecute el comando siguiente:
   >
   >```powershell
   >(Get-Process -Id $PID).StartInfo.EnvironmentVariables["PROCESSOR_ARCHITECTURE"]
   >```

1. El comando **Deploy-IoTEdge** comprueba si la versión del equipo Windows es compatible, activa la característica de contenedores y descarga tanto el runtime de Moby como el de IoT Edge. De forma predeterminada el comando usa contenedores Windows. 

   ```powershell
   . {Invoke-WebRequest -useb https://aka.ms/iotedge-win} | Invoke-Expression; `
   Deploy-IoTEdge
   ```

1. En este momento, los dispositivos IoT Core pueden reiniciarse automáticamente. Es posible que otros dispositivos Windows 10 o Windows Server soliciten su reinicio. En ese caso, reinícielo ahora. Una vez que el dispositivo esté listo, vuelva a ejecutar PowerShell como administrador.

1. El comando **Initialize-IoTEdge** configura el entorno de ejecución de Azure IoT Edge en el equipo. El comportamiento predeterminado del comando es el aprovisionamiento manual con contenedores de Windows. Utilice la marca `-Dps` para usar Device Provisioning Service, en lugar del aprovisionamiento manual. Sustituya `{scope ID}` por el identificador de ámbito de Device Provisioning Service y `{registration ID}` por el identificador de registro del dispositivo, que debería haber recuperado en el paso 1.

   Utilice el comando **Initialize-IoTEdge** para usar DPS con la atestación de TPM:

   ```powershell
   . {Invoke-WebRequest -useb https://aka.ms/iotedge-win} | Invoke-Expression; `
   Initialize-IoTEdge -Dps -ScopeId {scope ID} -RegistrationId {registration ID}
   ```

   Utilice el comando **Initialize-IoTEdge** para usar DPS con la atestación de clave simétrica. Reemplace `{symmetric key}` por una clave de dispositivo.

   ```powershell
   . {Invoke-WebRequest -useb https://aka.ms/iotedge-win} | Invoke-Expression; `
   Initialize-IoTEdge -Dps -ScopeId {scope ID} -RegistrationId {registration ID} -SymmetricKey {symmetric key}
   ```

1. Siga los pasos de [Comprobación de instalación correcta](#verify-successful-installation) para comprobar el estado de IoT Edge en el dispositivo. 

Al instalar y aprovisionar un dispositivo manualmente, puede usar parámetros adicionales para modificar la instalación, incluyendo:

* Dirigir el tráfico para que pase por un servidor proxy
* Apuntar el instalador a un directorio sin conexión
* Declarar una imagen de contenedor del agente específica y proporcionar las credenciales si se encuentra en un registro privado

Para obtener más información sobre estas opciones de instalación, continúe leyendo este artículo o salte para obtener información sobre [Todos los parámetros de instalación](#all-installation-parameters).

## <a name="offline-installation"></a>Instalación sin conexión

Durante la instalación se descargan dos archivos:

* un archivo .cab de Microsoft Azure IoT Edge, que contiene el demonio de seguridad de IoT Edge (iotedged), el motor del contenedor Moby y la CLI de Moby.
* Un archivo msi del paquete redistribuible de Visual C++ (tiempo de ejecución de VC)

Puede descargar uno de los archivos, o ambos, por adelantado en el dispositivo y luego seleccionar el script de instalación en el directorio que contiene los archivos. El instalador comprueba en primer lugar el directorio y luego descarga solo los componentes que no se pueden encontrar. Si los archivos están disponibles sin conexión, la instalación se puede realizar sin conexión a Internet. También puede usar esta característica para instalar una versión concreta de los componentes.  

Para obtener los archivos de instalación de IoT Edge más recientes, junto con las versiones anteriores, vea las [versiones de Azure IoT Edge](https://github.com/Azure/azure-iotedge/releases).

Para realizar la instalación con componentes sin conexión, use el parámetro `-OfflineInstallationPath` como parte del comando Deploy-IoTEdge y especifique la ruta de acceso absoluta al directorio de archivos. Por ejemplo,

```powershell
. {Invoke-WebRequest -useb https://aka.ms/iotedge-win} | Invoke-Expression; `
Deploy-IoTEdge -OfflineInstallationPath C:\Downloads\iotedgeoffline
```

También puede usar el parámetro de ruta de acceso de instalación sin conexión con el comando Update-IoTEdge, que se introducirá más adelante en este mismo artículo. 

## <a name="verify-successful-installation"></a>Comprobación de instalación correcta

Compruebe el estado del servicio IoT Edge. Debe aparecer como en ejecución.  

```powershell
Get-Service iotedge
```

Examine los registros de servicio de los últimos 5 minutos. Si acaba de instalar el runtime de IoT Edge, puede ver una lista de errores desde el tiempo transcurrido entre la ejecución de **Deploy-IoTEdge** e **Initialize IoTEdge**. Estos errores son esperables, ya que el servicio está intentando iniciar antes de que se haya configurado. 

```powershell
. {Invoke-WebRequest -useb https://aka.ms/iotedge-win} | Invoke-Expression; Get-IoTEdgeLog
```

Enumere los módulos en ejecución. Después de una instalación nueva, el único módulo que debería en ejecución es **edgeAgent**. Después de [implementar los módulos de IoT Edge](how-to-deploy-modules-portal.md) por primera vez, el otro módulo del sistema, **edgeHub**, se iniciará también en el dispositivo. 

```powershell
iotedge list
```

## <a name="manage-module-containers"></a>Administración de contenedores de módulo

El servicio IoT Edge requiere que un motor de contenedor se ejecute en el dispositivo. Cuando se implementa un módulo en un dispositivo, el runtime de IoT Edge utiliza el motor de contenedor para extraer la imagen del contenedor de un registro en la nube. El servicio de IoT Edge le permite interactuar con los módulos y recuperar los registros, pero también se puede usar el motor del contenedor para interactuar con el propio contenedor. 

Para más información acerca de los conceptos de los módulos, consulte [Información de los módulos de Azure IoT Edge](iot-edge-modules.md). 

Si ejecuta contenedores Windows en un dispositivo Windows IoT Edge, la instalación de IoT Edge incluye el motor de contenedor Moby. El motor de Moby se basaba en los mismos estándares que Docker y estaba diseñado para ejecutarse en paralelo en el mismo equipo que Docker Desktop. Por ese motivo, si desea agregar contenedores de destino administrados por el motor de Moby, tiene que dirigirse específicamente a dicho motor, en lugar de a Docker. 

Por ejemplo, para ver una lista de todas las imágenes de Docker, use el comando siguiente:

```powershell
docker images
```

Para ver una lista de todas las imágenes de Moby, modifique el mismo comando con un puntero al motor de Moby: 

```powershell
docker -H npipe:////./pipe/iotedge_moby_engine images
```

El identificador URI del motor se muestra en la salida del script de instalación, pero también puede encontrarlo en la sección de configuración del runtime del contenedor para el archivo config.yaml. 

![uri de moby_runtime en config.yaml](./media/how-to-install-iot-edge-windows/moby-runtime-uri.png)

Para más información acerca de los comandos que se pueden usar para interactuar con los contenedores e imágenes que se ejecutan en el dispositivo, consulte [Interfaces de la línea de comandos de Docker](https://docs.docker.com/engine/reference/commandline/docker/).

## <a name="update-an-existing-installation"></a>Actualización de una instalación existente

Si ya ha instalado el runtime de IoT Edge en un dispositivo y lo ha aprovisionado con una identidad de IoT Hub, puede actualizar el runtime sin tener que volver a escribir la información del dispositivo. 

Para obtener más información, vea [Actualice el archivo de configuración del demonio de seguridad y el entorno de ejecución de IoT Edge](how-to-update-iot-edge.md).

En este ejemplo se muestra una instalación que apunta a un archivo de configuración existente y usa contenedores de Windows: 

```powershell
. {Invoke-WebRequest -useb https://aka.ms/iotedge-win} | Invoke-Expression; `
Update-IoTEdge
```

Al actualizar IoT Edge, puede usar parámetros adicionales para modificar dicha actualización, entre los que se incluyen:

* Dirigir el tráfico para que pase por un servidor proxy
* Apuntar el instalador a un directorio sin conexión 
* Reiniciar sin preguntar al usuario si es necesario

No se puede declarar una imagen de contenedor del agente de IoT Edge con parámetros de script, porque esa información ya está establecida en el archivo de configuración de la instalación anterior. Si desea modificar la imagen del contenedor del agente, debe hacerlo en el archivo config.yaml. 

Para más información acerca de estas opciones de actualización, use el comando `Get-Help Update-IoTEdge -full` o consulte [todos los parámetros de instalación](#all-installation-parameters).

## <a name="uninstall-iot-edge"></a>Desinstalación de IoT Edge

Si quiere quitar la instalación de IoT Edge del dispositivo Windows, use el siguiente comando de una ventana de PowerShell administrativa. Este comando quita el entorno de ejecución de IoT Edge, junto con la configuración existente y los datos de motor de Moby. 

```powershell
. {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; `
Uninstall-IoTEdge
```

El comando Uninstall-IoTEdge no funciona en Windows IoT Core. Para quitar IoT Edge de los dispositivos de Windows IoT Core, es preciso volver a implementar la imagen de Windows IoT Core. 

Para más información acerca de las opciones de desinstalación, use el comando `Get-Help Uninstall-IoTEdge -full`. 

## <a name="all-installation-parameters"></a>Todos los parámetros de instalación

En las secciones anteriores se presentaron escenarios de instalación comunes con ejemplos de cómo usar parámetros para modificar el script de instalación. En esta sección se proporcionan tablas de referencia de los parámetros que se suelen usar para instalar, actualizar o desinstalar IoT Edge. 

### <a name="deploy-iotedge"></a>Deploy-IoTEdge

El comando Deploy-IoTEdge descarga e implementa el demonio de seguridad de IoT Edge y sus dependencias. El comando de implementación acepta estos parámetros comunes, entre otros. Para obtener la lista completa, use el comando `Get-Help Deploy-IoTEdge -full`.  

| Parámetro | Valores aceptados | Comentarios |
| --------- | --------------- | -------- |
| **ContainerOs** | **Windows** o **Linux** | Si no se especifica el sistema operativo del contenedor, el valor predeterminado es Windows.<br><br>En el caso de los contenedores Windows, IoT Edge usa el motor de contenedor de Moby que se incluye en la instalación. Para los contenedores de Linux, deberá instalar un motor de contenedor antes de iniciar la instalación. |
| **Proxy** | Dirección URL del proxy | Incluya este parámetro si el dispositivo debe pasar por un servidor proxy para conectarse a Internet. Para más información, consulte [Configuración de un dispositivo de IoT Edge para que se comunique a través de un servidor proxy](how-to-configure-proxy-support.md). |
| **OfflineInstallationPath** | Ruta de acceso del directorio | Si se incluye este parámetro, el instalador buscará en el directorio enumerado el archivo CAB de IoT Edge y los archivos MSI del runtime de VC necesarios para la instalación. Se descargan los archivos que no se encuentren en el directorio. Si ambos archivos se encuentran en el directorio, IoT Edge se puede instalar sin una conexión a Internet. Este parámetro también se puede usar para utilizar una versión concreta. |
| **InvokeWebRequestParameters** | Tabla de hash de parámetros y valores | Durante la instalación se realizan varias solicitudes web. Utilice este campo para definir parámetros para esas solicitudes web. Este parámetro es útil para configurar las credenciales para los servidores proxy. Para más información, consulte [Configuración de un dispositivo de IoT Edge para que se comunique a través de un servidor proxy](how-to-configure-proxy-support.md). |
| **RestartIfNeeded** | None | Esta marca permite al script de implementación reiniciar el equipo sin preguntar, en caso de que sea necesario. |

### <a name="initialize-iotedge"></a>Initialize-IoTEdge

El comando Initialize-IoTEdge configura IoT Edge con su cadena de conexión del dispositivo y los detalles operativos. Gran parte de la información que genera este comando se almacena en el archivo iotedge\config.yaml. El comando de inicialización acepta estos parámetros comunes, entre otros. Para obtener la lista completa, use el comando `Get-Help Initialize-IoTEdge -full`. 

| Parámetro | Valores aceptados | Comentarios |
| --------- | --------------- | -------- |
| **Manual** | None | **Parámetro de modificador**. Si no se especifica ningún tipo de aprovisionamiento, el valor predeterminado es manual.<br><br>Declara que usted proporcionará una cadena de conexión de dispositivo para aprovisionar el dispositivo manualmente |
| **Dps** | None | **Parámetro de modificador**. Si no se especifica ningún tipo de aprovisionamiento, el valor predeterminado es manual.<br><br>Declara que usted proporcionará un Id. de ámbito de Device Provisioning Service (DPS) y el Id. de registro del dispositivo para aprovisionar a través de DPS.  |
| **DeviceConnectionString** | Una cadena de conexión desde un dispositivo de IoT Edge registrado en IoT Hub, entre comillas simples | **Obligatorio** para la instalación manual. Si no proporciona una cadena de conexión en los parámetros del script, se le pedirá una durante la instalación. |
| **ScopeId** | Un Id. de ámbito de una instancia de Device Provisioning Service asociada con IoT Hub. | **Obligatorio** para la instalación de DPS. Si no proporciona un Id. de ámbito en los parámetros del script, se le pedirá uno durante la instalación. |
| **RegistrationId** | Un identificador de registro generado por el dispositivo | **Obligatorio** para la instalación de DPS. |
| **SymmetricKey** | La clave simétrica usada para aprovisionar la identidad del dispositivo de IoT Edge al usar DPS | **Obligatorio** para la instalación de DPS si se usa la atestación de clave simétrica. |
| **ContainerOs** | **Windows** o **Linux** | Si no se especifica el sistema operativo del contenedor, el valor predeterminado es Windows.<br><br>En el caso de los contenedores Windows, IoT Edge usa el motor de contenedor de Moby que se incluye en la instalación. Para los contenedores de Linux, deberá instalar un motor de contenedor antes de iniciar la instalación. |
| **InvokeWebRequestParameters** | Tabla de hash de parámetros y valores | Durante la instalación se realizan varias solicitudes web. Utilice este campo para definir parámetros para esas solicitudes web. Este parámetro es útil para configurar las credenciales para los servidores proxy. Para más información, consulte [Configuración de un dispositivo de IoT Edge para que se comunique a través de un servidor proxy](how-to-configure-proxy-support.md). |
| **AgentImage** | URI de la imagen del agente de IoT Edge | De forma predeterminada, una nueva instalación de IoT Edge utiliza la etiqueta gradual más reciente de la imagen del agente de IoT Edge. Utilice este parámetro para establecer una etiqueta específica para la versión de la imagen o para proporcionar su propia imagen de agente. Para obtener más información, vea [Información sobre las etiquetas de IoT Edge](how-to-update-iot-edge.md#understand-iot-edge-tags). |
| **Nombre de usuario** | Nombre de usuario de registro de contenedor | Utilice este parámetro solo si establece el parámetro -AgentImage en un contenedor en un registro privado. Proporcione un nombre de usuario con acceso al registro. |
| **Contraseña** | Cadena de contraseña segura | Utilice este parámetro solo si establece el parámetro -AgentImage en un contenedor en un registro privado. Proporcione la contraseña para acceder al registro. | 

### <a name="update-iotedge"></a>Update-IoTEdge

| Parámetro | Valores aceptados | Comentarios |
| --------- | --------------- | -------- |
| **ContainerOs** | **Windows** o **Linux** | Si no se especifica el sistema operativo del contenedor, el valor predeterminado es Windows. Para los contenedores de Windows, se incluirá un motor de contenedor en la instalación. Para los contenedores de Linux, deberá instalar un motor de contenedor antes de iniciar la instalación. |
| **Proxy** | Dirección URL del proxy | Incluya este parámetro si el dispositivo debe pasar por un servidor proxy para conectarse a Internet. Para más información, consulte [Configuración de un dispositivo de IoT Edge para que se comunique a través de un servidor proxy](how-to-configure-proxy-support.md). |
| **InvokeWebRequestParameters** | Tabla de hash de parámetros y valores | Durante la instalación se realizan varias solicitudes web. Utilice este campo para definir parámetros para esas solicitudes web. Este parámetro es útil para configurar las credenciales para los servidores proxy. Para más información, consulte [Configuración de un dispositivo de IoT Edge para que se comunique a través de un servidor proxy](how-to-configure-proxy-support.md). |
| **OfflineInstallationPath** | Ruta de acceso del directorio | Si se incluye este parámetro, el instalador buscará en el directorio enumerado el archivo CAB de IoT Edge y los archivos MSI del runtime de VC necesarios para la instalación. Se descargan los archivos que no se encuentren en el directorio. Si ambos archivos se encuentran en el directorio, IoT Edge se puede instalar sin una conexión a Internet. Este parámetro también se puede usar para utilizar una versión concreta. |
| **RestartIfNeeded** | None | Esta marca permite al script de implementación reiniciar el equipo sin preguntar, en caso de que sea necesario. |

### <a name="uninstall-iotedge"></a>Uninstall-IoTEdge

| Parámetro | Valores aceptados | Comentarios |
| --------- | --------------- | -------- |
| **Force** | None | Esta marca fuerza la desinstalación en caso de que el intento anterior de desinstalación no diera unos resultados satisfactorio. 
| **RestartIfNeeded** | None | Esta marca permite al script de desinstalación reiniciar el equipo sin preguntar, en caso de que sea necesario. |

## <a name="next-steps"></a>Pasos siguientes

Ahora que tiene un dispositivo IoT Edge aprovisionado con el entorno de ejecución instalado, puede [implementar módulos de IoT Edge](how-to-deploy-modules-portal.md).

Si tiene problemas con la instalación correcta de IoT Edge, vea la página de [solución de problemas](troubleshoot.md).

Para actualizar una instalación existente a la versión más reciente de IoT Edge, vea [Actualice el archivo de configuración del demonio de seguridad y el entorno de ejecución de IoT Edge](how-to-update-iot-edge.md).
