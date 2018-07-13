---
title: Simulación de Azure IoT Edge en Windows | Microsoft Docs
description: Instalación del runtime de Azure IoT Edge en un dispositivo simulado en Windows e implementación del primer módulo
author: kgremban
manager: timlt
ms.author: kgremban
ms.reviewer: elioda
ms.date: 06/08/2018
ms.topic: tutorial
ms.service: iot-edge
services: iot-edge
ms.custom: mvc
ms.openlocfilehash: 5bde54a65160c58d8bfba2f6c4c3b6a4317e46ed
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/11/2018
ms.locfileid: "38540238"
---
# <a name="quickstart-deploy-your-first-iot-edge-module-from-the-azure-portal-to-a-windows-device---preview"></a>Guía de inicio rápido: implementación del primer módulo de IoT Edge desde Azure Portal a un dispositivo Windows (versión preliminar)

Azure IoT Edge le permite realizar análisis y procesamiento de datos en los dispositivos, en lugar de tener que insertar todos los datos en la nube. En los tutoriales de IoT Edge se muestra cómo implementar diferentes tipos de módulos, pero primero se necesita un dispositivo para probarlo. 

En esta guía de inicio rápido, aprenderá a hacer lo siguiente:

1. Creación de un IoT Hub
2. Registro de un dispositivo de IoT Edge
3. Inicio del runtime de IoT Edge
4. Implementación de un módulo

![Arquitectura del tutorial][2]

El módulo que se implementa en esta guía de inicio rápido es un sensor simulado que genera datos de temperatura, humedad y presión. Los otros tutoriales de Azure IoT Edge se basan en el trabajo que se realiza aquí mediante la implementación de módulos que analizan los datos simulados para obtener información empresarial. 

>[!NOTE]
>El entorno de ejecución de Azure IoT Edge en Windows se encuentra en [versión preliminar pública](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Si no tiene una suscripción activa a Azure, cree una [cuenta gratuita][lnk-account] antes de comenzar.

## <a name="prerequisites"></a>Requisitos previos

En esta guía de inicio rápido se asume que utiliza un equipo o máquina virtual con Windows para simular un dispositivo IoT. Si ejecuta Windows en una máquina virtual, habilite la [virtualización anidada][lnk-nested] y asigne al menos 2 GB de memoria. 

Asegúrese de cumplir los siguientes requisitos previos en la máquina que va a usar con un dispositivo IoT Edge:

1. Asegúrese de que usa una versión de Windows compatible:
   * Windows 10 o posterior
   * Windows Server 2016 o posterior
2. Instale [Docker para Windows][lnk-docker] y asegúrese de que se ejecuta.
3. Configure Docker para usar [contenedores de Linux](https://docs.docker.com/docker-for-windows/#switch-between-windows-and-linux-containers)

## <a name="create-an-iot-hub"></a>Crear un centro de IoT

Para comenzar la guía de inicio rápido, cree su instancia de IoT Hub en Azure Portal.
![Creación de una instancia de IoT Hub][3]

[!INCLUDE [iot-hub-create-hub](../../includes/iot-hub-create-hub.md)]

## <a name="register-an-iot-edge-device"></a>Registro de un dispositivo de IoT Edge

Registre un dispositivo de IoT Edge con la instancia de IoT Hub recién creada.
![Registro de un dispositivo][4]

[!INCLUDE [iot-edge-register-device](../../includes/iot-edge-register-device.md)]

## <a name="install-and-start-the-iot-edge-runtime"></a>Instale e inicie el runtime de IoT Edge

Instale e inicie el entorno de ejecución de Azure IoT Edge en el dispositivo IoT Edge. 
![Registro de un dispositivo][5]

El runtime de IoT Edge se implementa en todos los dispositivos de IoT Edge. Tiene tres componentes. El **demonio de seguridad de IoT Edge** se inicia cada vez que se inicia un dispositivo perimetral y arranca el dispositivo mediante el inicio del agente de IoT Edge. El **agente de IoT Edge** facilita la implementación y supervisión de los módulos en el dispositivo IoT Edge, incluido el centro de IoT Edge. El **centro de IoT Edge** administra las comunicaciones entre los módulos del dispositivo de IoT Edge y entre el dispositivo y la instancia de IoT Hub. 

>[!NOTE]
>Los pasos de instalación de esta sección son manuales por ahora mientras se está en proceso de desarrollar un script de instalación. 

Las instrucciones de esta sección configuran el entorno de ejecución de Azure IoT Edge con contenedores de Linux. Si quiere usar contenedores de Windows, consulte [Install Azure IoT Edge runtime on Windows to use with Windows containers](how-to-install-iot-edge-windows-with-windows.md) (Instalación del entorno de ejecución de Azure IoT Edge en Windows para usar con contenedores de Windows).

### <a name="download-and-install-the-iot-edge-service"></a>Descarga e instalación del servicio IoT Edge

1. En el dispositivo IoT Edge, ejecute PowerShell como administrador.

2. Descargue el paquete del servicio IoT Edge.

  ```powershell
  Invoke-WebRequest https://aka.ms/iotedged-windows-latest -o .\iotedged-windows.zip
  Expand-Archive .\iotedged-windows.zip C:\ProgramData\iotedge -f
  Move-Item c:\ProgramData\iotedge\iotedged-windows\* C:\ProgramData\iotedge\ -Force
  rmdir C:\ProgramData\iotedge\iotedged-windows
  $env:Path += ";C:\ProgramData\iotedge"
  SETX /M PATH "$env:Path"
  ```

3. Instale vcruntime.

  ```powershell
  Invoke-WebRequest -useb https://download.microsoft.com/download/0/6/4/064F84EA-D1DB-4EAA-9A5C-CC2F0FF6A638/vc_redist.x64.exe -o vc_redist.exe
  .\vc_redist.exe /quiet /norestart
  ```

4. Cree e inicie el servicio IoT Edge.

   ```powershell
   New-Service -Name "iotedge" -BinaryPathName "C:\ProgramData\iotedge\iotedged.exe -c C:\ProgramData\iotedge\config.yaml"
   Start-Service iotedge
   ```

5. Agregue excepciones del firewall para los puertos que usa el servicio IoT Edge.

   ```powershell
   New-NetFirewallRule -DisplayName "iotedged allow inbound 15580,15581" -Direction Inbound -Action Allow -Protocol TCP -LocalPort 15580-15581 -Program "C:\programdata\iotedge\iotedged.exe" -InterfaceType Any
   ```

6. Cree un nuevo archivo denominado **iotedge.reg** y ábralo con un editor de texto. 

7. Agregue el contenido siguiente y guarde el archivo. 

   ```input
   Windows Registry Editor Version 5.00
   [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\EventLog\Application\iotedged]
   "CustomSource"=dword:00000001
   "EventMessageFile"="C:\\ProgramData\\iotedge\\iotedged.exe"
   "TypesSupported"=dword:00000007
   ```

8. Vaya al archivo en el Explorador de archivos y haga doble clic en él para importar los cambios en el Registro de Windows. 

### <a name="configure-the-iot-edge-runtime"></a>Configuración del runtime de IoT Edge 

Configure el entorno de ejecución con la cadena de conexión del dispositivo IoT Edge que copió cuando registró un nuevo dispositivo. A continuación, configure la red del entorno de ejecución. 

1. Abra el archivo de configuración de IoT Edge, que se encuentra en `C:\ProgramData\iotedge\config.yaml`. Este archivo está protegido, así que ejecute un editor de texto como el Bloc de notas como administrador y, luego, use el editor para abrir el archivo. 

2. Busque la sección **Provisioning** (Aprovisionamiento) y actualice el valor de **device_connection_string** con la cadena que copió de los detalles del dispositivo IoT Edge. 

3. En la ventana de administrador de PowerShell, recupere el nombre de host del dispositivo IoT Edge y copie la salida. 

   ```powershell
   hostname
   ```

4. En el archivo de configuración, busque la sección **Edge device hostname** (Nombre de host de dispositivo perimetral). Actualice el valor de **nombre de host** con el nombre de host que copió de PowerShell.

3. En la ventana de administrador de PowerShell, recupere la dirección IP del dispositivo IoT Edge. 

   ```powershell
   ipconfig
   ```

4. Copie el valor de **dirección IPv4** en la sección **vEthernet (DockerNAT)** de la salida. 

5. Cree una variable de entorno llamada **IOTEDGE_HOST** y sustituya *\<ip_address\>* por la dirección IP del dispositivo IoT Edge. 

  ```powershell
  [Environment]::SetEnvironmentVariable("IOTEDGE_HOST", "http://<ip_address>:15580")
  ```

  Conserve la variable de entorno entre los reinicios.

  ```powershell
  SETX /M IOTEDGE_HOST "http://<ip_address>:15580"
  ```

6. En el archivo `config.yaml`, busque la sección **Connect settings** (Configuración de conexión). Actualice los valores de **management_uri** y **workload_uri** con la dirección IP y los puertos que abrió en la sección anterior. Reemplace **\<GATEWAY_ADDRESS\>** por su dirección IP. 

   ```yaml
   connect: 
     management_uri: "http://<GATEWAY_ADDRESS>:15580"
     workload_uri: "http://<GATEWAY_ADDRESS>:15581"
   ```

7. Busque la sección **Listen settings** (Configuración de escucha) y agregue los mismos valores para **management_uri** y **workload_uri**. 

   ```yaml
   listen:
     management_uri: "http://<GATEWAY_ADDRESS>:15580"
     workload_uri: "http://<GATEWAY_ADDRESS>:15581"
   ```

8. Busque la sección **Moby Container Runtime settings** (Configuración del entorno de ejecución de contenedores de Moby) y compruebe que el valor de **red** está establecido en `nat`.

9. Guarde el archivo de configuración. 

10. En PowerShell, reinicie el servicio IoT Edge.

   ```powershell
   Stop-Service iotedge -NoWait
   sleep 5
   Start-Service iotedge
   ```

### <a name="view-the-iot-edge-runtime-status"></a>Visualización del estado del entorno de ejecución de Azure IoT Edge

Compruebe que el entorno de ejecución se ha instalado y configurado correctamente.

1. Compruebe el estado del servicio IoT Edge.

   ```powershell
   Get-Service iotedge
   ```

2. Si necesita solucionar problemas del servicio, recupere los registros del servicio. 

   ```powershell
   # Displays logs from today, newest at the bottom.

   Get-WinEvent -ea SilentlyContinue `
    -FilterHashtable @{ProviderName= "iotedged";
      LogName = "application"; StartTime = [datetime]::Today} |
    select TimeCreated, Message |
    sort-object @{Expression="TimeCreated";Descending=$false}
   ```

3. Vea todos los módulos que se ejecutan en el dispositivo IoT Edge. Como el servicio se acaba de iniciar por primera vez, solo verá la ejecución del módulo **edgeAgent**. El módulo edgeAgent se ejecuta de forma predeterminada, y le ayuda a instalar e iniciar módulos adicionales que puede implementar en el dispositivo. 

   ```powershell
   iotedge list
   ```

   ![Visualización de un módulo en el dispositivo](./media/quickstart/iotedge-list-1.png)

## <a name="deploy-a-module"></a>Implementación de un módulo

Administre el dispositivo Azure IoT Edge desde la nube para implementar un módulo que enviará datos de telemetría a IoT Hub.
![Registro de un dispositivo][6]

[!INCLUDE [iot-edge-deploy-module](../../includes/iot-edge-deploy-module.md)]

## <a name="view-generated-data"></a>Visualización de datos generados

En esta guía de inicio rápido, ha creado un nuevo dispositivo de IoT Edge y ha instalado el runtime de IoT Edge en él. Luego, ha usado Azure Portal para insertar un módulo de IoT Edge para que se ejecute en el dispositivo sin tener que realizar cambios en el propio dispositivo. En este caso, el módulo que ha insertado crea datos del entorno que se pueden usar para los tutoriales. 

Confirme que el módulo implementado desde la nube se está ejecutando en el dispositivo IoT Edge. 

```powershell
iotedge list
```

   ![Ver tres módulos en el dispositivo](./media/quickstart/iotedge-list-2.png)

Vea los mensajes que se envían desde el módulo tempSensor a la nube. 

```powershell
iotedge logs tempSensor -f
```

  ![Ver los datos desde el módulo](./media/quickstart/iotedge-logs.png)

También puede ver los mensajes recibidos por su centro de IoT mediante la [herramienta de exploración de IoT Hub][lnk-iothub-explorer] o la [extensión Azure IoT Toolkit para Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit). 

## <a name="clean-up-resources"></a>Limpieza de recursos

Puede usar el dispositivo simulado que configuró en esta guía de inicio rápido para probar los tutoriales de IoT Edge. Si quiere impedir que el módulo tempSensor envíe datos desde su centro de IoT, use el siguiente comando para detener el servicio IoT Edge y eliminar los contenedores que se crearon en el dispositivo. Si quiere volver a usar su máquina como dispositivo IoT Edge, no olvide iniciar el servicio. 

   ```powershell
   Stop-Service iotedge -NoWait
   docker rm -f $(docker ps -aq)
   ```

Cuando ya no necesite la instancia de IoT Hub que ha creado, puede usar el comando Azure Portal para quitar el recurso y todos los dispositivos que tenga asociados. Vaya a la página de información general del centro de IoT y seleccione **Eliminar**. 

## <a name="next-steps"></a>Pasos siguientes

En esta guía de inicio rápido, ha creado un nuevo dispositivo IoT Edge y ha usado la interfaz en la nube de Azure IoT Edge para implementar el código en el dispositivo. Ahora tiene un dispositivo de prueba que genera datos sin procesar acerca de su entorno. 

Está listo para continuar con cualquiera de los demás tutoriales para saber cómo Azure IoT Edge puede ayudarlo a transformar estos datos en información empresarial en el perímetro.

> [!div class="nextstepaction"]
> [Filtrado de los datos del sensor mediante una función de Azure](tutorial-deploy-function.md)

<!-- Images -->
[2]: ./media/quickstart/install-edge-full.png
[3]: ./media/quickstart/create-iot-hub.png
[4]: ./media/quickstart/register-device.png
[5]: ./media/quickstart/start-runtime.png
[6]: ./media/quickstart/deploy-module.png

<!-- Links -->
[lnk-nested]: https://docs.microsoft.com/virtualization/hyper-v-on-windows/user-guide/nested-virtualization
[lnk-docker]: https://docs.docker.com/docker-for-windows/install/ 
[lnk-python]: https://www.python.org/downloads/
[lnk-docker-containers]: https://docs.microsoft.com/virtualization/windowscontainers/quick-start/quick-start-windows-10#2-switch-to-windows-containers
[lnk-iothub-explorer]: https://github.com/azure/iothub-explorer
[lnk-install-iotcore]: how-to-install-iot-core.md
[lnk-account]: https://azure.microsoft.com/free
