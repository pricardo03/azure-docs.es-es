---
title: Guía de inicio rápido de Azure IoT Edge + Windows | Microsoft Docs
description: Pruebe Azure IoT Edge mediante la realización de análisis en un dispositivo perimetral simulado
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 06/24/2018
ms.topic: quickstart
ms.service: iot-edge
services: iot-edge
ms.custom: mvc
ms.openlocfilehash: 1437c3552a7af5d5474cf3bdaabe95d5415af603
ms.sourcegitcommit: 96f498de91984321614f09d796ca88887c4bd2fb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/02/2018
ms.locfileid: "39414218"
---
# <a name="quickstart-deploy-your-first-iot-edge-module-from-the-azure-portal-to-a-windows-device---preview"></a>Guía de inicio rápido: implementación del primer módulo de IoT Edge desde Azure Portal a un dispositivo Windows (versión preliminar)

En esta guía de inicio rápido, use la interfaz en la nube de Azure IoT Edge para implementar código creado previamente de manera remota en un dispositivo de IoT Edge. Para realizar esta tarea, en primer lugar, use un dispositivo de Windows para simular un dispositivo de IoT Edge y, posteriormente, puede implementar un módulo en él.

En esta guía de inicio rápido, aprenderá a hacer lo siguiente:

1. Cree un centro de IoT Hub.
2. Registre un dispositivo IoT Edge en su instancia de IoT Hub.
3. Instale e inicie el entorno de ejecución de Azure IoT Edge en el dispositivo.
4. Implemente un módulo de forma remota en un dispositivo IoT Edge y envíe telemetría a IoT Hub.

![Arquitectura del tutorial][2]

El módulo que se implementa en esta guía de inicio rápido es un sensor simulado que genera datos de temperatura, humedad y presión. Los otros tutoriales de Azure IoT Edge se basan en el trabajo que se realiza aquí mediante la implementación de módulos que analizan los datos simulados para obtener información empresarial. 

>[!NOTE]
>El entorno de ejecución de Azure IoT Edge en Windows se encuentra en [versión preliminar pública](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Si no tiene una suscripción activa a Azure, cree una [cuenta gratuita][lnk-account] antes de comenzar.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Usará la CLI de Azure para completar muchos de los pasos de esta guía de inicio rápido; además, Azure IoT cuenta con una extensión para habilitar funcionalidad adicional. 

Agregue la extensión de Azure IoT a la instancia de Cloud Shell.

   ```azurecli-interactive
   az extension add --name azure-cli-iot-ext
   ```

## <a name="prerequisites"></a>Requisitos previos

Recursos en la nube: 

* Un grupo de recursos para administrar todos los recursos que se van a usar en esta guía de inicio rápido. 

   ```azurecli-interactive
   az group create --name IoTEdgeResources --location westus
   ```

Un equipo Windows o máquina virtual que actúe como dispositivo de IoT Edge: 

* Use una versión de Windows compatible:
   * Windows 10 o posterior
   * Windows Server 2016 o posterior
* Si es una máquina virtual, habilite la [virtualización anidada][lnk-nested] y asigne al menos 2 GB de memoria. 
* Instale [Docker para Windows][lnk-docker] y asegúrese de que se ejecuta.
* Configure Docker para usar [contenedores de Linux](https://docs.docker.com/docker-for-windows/#switch-between-windows-and-linux-containers)

## <a name="create-an-iot-hub"></a>Crear un centro de IoT

Para comenzar la guía de inicio rápido, cree su instancia de IoT Hub con la CLI de Azure. 

![Crear IoT Hub][3]

El nivel gratuito de IoT Hub funciona para esta guía de inicio rápido. Si ha usado IoT Hub en el pasado y ya tiene un centro gratis creado, puede usar ese centro de IoT. Cada suscripción no puede tener más de un centro de IoT gratuito. 

El código siguiente crea un centro **F1** gratis en el grupo de recursos **IoTEdgeResources**. Reemplace *{hub_name}* por un nombre único para el centro de IoT.

   ```azurecli-interactive
   az iot hub create --resource-group IoTEdgeResources --name {hub_name} --sku F1 
   ```

   Si se produce un error porque ya hay un centro gratis en la suscripción, cambie la SKU a **S1**. 

## <a name="register-an-iot-edge-device"></a>Registro de un dispositivo de IoT Edge

Registre un dispositivo de IoT Edge con la instancia de IoT Hub recién creada.
![Registro de un dispositivo][4]

Cree una identidad para el dispositivo simulado, con el fin de que pueda comunicarse con su centro de IoT. Dado que los dispositivos de IoT Edge se comportan y se pueden administrar de manera diferente a los dispositivos de IoT típicos, declare que este es un dispositivo de IoT Edge desde el principio. 

1. En Azure Cloud Shell, escriba el comando siguiente para crear un dispositivo denominado **myEdgeDevice** en el centro.

   ```azurecli-interactive
   az iot hub device-identity create --device-id myEdgeDevice --hub-name {hub_name} --edge-enabled
   ```

1. Recupere la cadena de conexión del dispositivo, que vincula el dispositivo físico con su identidad en IoT Hub. 

   ```azurecli-interactive
   az iot hub device-identity show-connection-string --device-id myEdgeDevice --hub-name {hub_name}
   ```

1. Copie la cadena de conexión y guárdela. Este valor se usará para configurar el runtime de IoT Edge en la sección siguiente. 

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
   $sysenv = "HKLM:\SYSTEM\CurrentControlSet\Control\Session Manager\Environment"
   $path = (Get-ItemProperty -Path $sysenv -Name Path).Path + ";C:\ProgramData\iotedge"
   Set-ItemProperty -Path $sysenv -Name Path -Value $path
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

6. En el archivo `config.yaml`, busque la sección **Connect settings** (Configuración de conexión). Actualice los valores de **management_uri** y **workload_uri** con la dirección IP y los puertos que abrió en la sección anterior. Reemplace **\<GATEWAY_ADDRESS\>** por la dirección IP de DockerNAT que copió. 

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

8. Busque la sección **Moby Container Runtime settings** (Configuración del entorno de ejecución de contenedores de Moby) y compruebe que se le ha quitado la marca de comentario al valor de **red** y que se ha establecido en **azure-iot-edge**

   ```yaml
   moby_runtime:
     docker_uri: "npipe://./pipe/docker_engine"
     network: "azure-iot-edge"
   ```

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
    sort-object @{Expression="TimeCreated";Descending=$false} |
    format-table -autosize -wrap
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

Si desea continuar con los tutoriales de IoT Edge, puede usar el dispositivo que ha registrado y configurado en esta guía de inicio rápido. De lo contrario, puede eliminar los recursos de Azure que ha creado y quitar el entorno de ejecución de Azure IoT Edge del dispositivo. 

### <a name="delete-azure-resources"></a>Eliminación de recursos de Azure

Si ha creado una máquina virtual y un centro de IoT en un nuevo grupo de recursos, puede eliminar dicho grupo y todos los recursos asociados. Si hay algo en ese grupo de recursos que desee mantener, solo hay que eliminar los recursos individuales que se quieran limpiar. 

Quite el grupo **IoTEdgeResources**. 

   ```azurecli-interactive
   az group delete --name IoTEdgeResources 
   ```

### <a name="remove-the-iot-edge-runtime"></a>Eliminación del entorno de ejecución de Azure IoT Edge

Si planea usar el dispositivo de IoT Edge para futuras pruebas, pero quiere hacer que el módulo tempSensor deje de enviar datos a la instancia de IoT Hub mientras no esté en uso, utilice el siguiente comando para detener el servicio IoT Edge. 

   ```powershell
   Stop-Service iotedge -NoWait
   ```

Puede reiniciar el servicio cuando esté listo para empezar las pruebas de nuevo.

   ```powershell
   Start-Service iotedge
   ```

Si desea quitar las instalaciones desde su dispositivo, use los siguientes comandos.  

Quite el entorno de ejecución de Azure IoT Edge.

   ```powershell
   cmd /c sc delete iotedge
   rm -r c:\programdata\iotedge
   ```

Cuando se quita el entorno de ejecución de Azure IoT Edge, los contenedores que se han creado se detienen, pero siguen existiendo en el dispositivo. Vea todos los contenedores.

   ```powershell
   docker ps -a
   ```

Elimine los contenedores que se crearon en el dispositivo por el entorno de ejecución de Azure IoT Edge. Cambie el nombre del contenedor tempSensor si lo llama de otra manera. 

   ```powershell
   docker rm -f tempSensor
   docker rm -f edgeHub
   docker rm -f edgeAgent
   ```
   
## <a name="next-steps"></a>Pasos siguientes

En esta guía de inicio rápido, ha creado un nuevo dispositivo IoT Edge y ha usado la interfaz en la nube de Azure IoT Edge para implementar el código en el dispositivo. Ahora tiene un dispositivo de prueba que genera datos sin procesar acerca de su entorno. 

Está listo para continuar con cualquiera de los demás tutoriales para saber cómo Azure IoT Edge puede ayudarlo a transformar estos datos en información empresarial en el perímetro.

> [!div class="nextstepaction"]
> [Filtrado de los datos del sensor mediante una función de Azure](tutorial-deploy-function.md)


<!-- Images -->
[1]: ./media/quickstart/cloud-shell.png
[2]: ./media/quickstart/install-edge-full.png
[3]: ./media/quickstart/create-iot-hub.png
[4]: ./media/quickstart/register-device.png
[5]: ./media/quickstart/start-runtime.png
[6]: ./media/quickstart/deploy-module.png


<!-- Links -->
[lnk-docker]: https://docs.docker.com/docker-for-windows/install/ 
[lnk-iothub-explorer]: https://github.com/azure/iothub-explorer
[lnk-account]: https://azure.microsoft.com/free
[lnk-portal]: https://portal.azure.com
[lnk-nested]: https://docs.microsoft.com/virtualization/hyper-v-on-windows/user-guide/nested-virtualization
[lnk-delete]: https://docs.microsoft.com/cli/azure/iot/hub?view=azure-cli-latest#az_iot_hub_delete

<!-- Anchor links -->
[anchor-register]: #register-an-iot-edge-device
