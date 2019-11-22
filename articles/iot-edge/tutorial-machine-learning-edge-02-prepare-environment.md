---
title: 'Tutorial: Configuración del entorno: Machine Learning en Azure IoT Edge'
description: 'Tutorial: Prepare el entorno para desarrollar e implementar módulos de aprendizaje automático en el perímetro.'
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 11/11/2019
ms.topic: tutorial
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 2ea4248ebaedd318e4112e41169f72bc80b1120f
ms.sourcegitcommit: 598c5a280a002036b1a76aa6712f79d30110b98d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/15/2019
ms.locfileid: "74114075"
---
# <a name="tutorial-set-up-an-environment-for-machine-learning-on-iot-edge"></a>Tutorial: Configuración de un entorno de aprendizaje automático en IoT Edge

> [!NOTE]
> Este artículo forma parte de un tutorial sobre el uso de Azure Machine Learning en IoT Edge. Si ha llegado directamente a este artículo, le recomendamos que comience con el [primer el artículo](tutorial-machine-learning-edge-01-intro.md) de la serie para obtener los mejores resultados.

Este artículo del tutorial integral de Azure Machine Learning en IoT Edge le ayuda a preparar el entorno para desarrollo e implementación. En primer lugar, configure una máquina de desarrollo con todas las herramientas que necesita. A continuación, cree los recursos en la nube necesarios en Azure.

## <a name="set-up-a-development-machine"></a>Configuración de una máquina de desarrollo

Normalmente, este paso está a cargo de un desarrollador de nube. Algunos de los ejemplos de software también pueden resultar útiles para un científico de datos.

En el transcurso de este artículo llevamos a cabo varias tareas para desarrolladores, entre las que se incluyen programar, compilar, configurar e implementar los módulos de IoT Edge y los dispositivos de IoT. Para facilitar su uso, hemos creado un script de PowerShell que crea una máquina virtual de Azure con la mayoría de los requisitos previos ya configurados. La máquina virtual que se cree debe ser capaz de hacer frente a la [virtualización anidada](https://docs.microsoft.com/azure/virtual-machines/windows/nested-virtualization), motivo por el que elegimos el tamaño de máquina [Standard_D8s_v3](../virtual-machines/windows/sizes-general.md#dsv3-series-1).

La máquina virtual de desarrollo se configurará con:

* Windows 10
* [Chocolatey](https://chocolatey.org/)
* [Docker Desktop para Windows](https://www.docker.com/products/docker-desktop)
* [Git para Windows](https://gitforwindows.org/)
* [Administrador de credenciales de Git para Windows](https://github.com/Microsoft/Git-Credential-Manager-for-Windows)
* [SDK de .Net Core](https://dotnet.microsoft.com/)
* [Python 3](https://www.python.org/)
* [Visual Studio Code](https://code.visualstudio.com/)
* [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview?view=azps-1.1.0)
* [Extensiones de VS Code](https://marketplace.visualstudio.com/search?target=VSCode)
  * [Herramientas de Azure IoT](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools)
  * [Python](https://marketplace.visualstudio.com/items?itemName=ms-python.python)
  * [C#](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
  * [Docker](https://marketplace.visualstudio.com/items?itemName=PeterJausovec.vscode-docker)
  * [PowerShell](https://marketplace.visualstudio.com/items?itemName=ms-vscode.PowerShell)

La máquina virtual de desarrollador no es realmente necesaria; todas las herramientas de desarrollo se pueden ejecutar en un equipo local. Sin embargo, es muy recomendable usar la máquina virtual para garantizar condiciones uniformes.

La máquina virtual tarda unos 30 minutos en crearse y configurarse.

### <a name="get-the-script"></a>Obtención del script

Clone o descargue el script de PowerShell desde el repositorio de ejemplo de [Machine Learning e IoT Edge](https://github.com/Azure-Samples/IoTEdgeAndMlSample).

### <a name="create-an-azure-virtual-machine"></a>Creación de una máquina virtual de Azure

El directorio DevVM contiene los archivos necesarios para crear una máquina virtual de Azure adecuada para completar este tutorial.

1. Abra PowerShell como administrador y navegue al directorio donde descargó el código. Haremos referencia al directorio raíz para el origen de datos como `<srcdir>`.

    ```powershell
    cd <srcdir>\IoTEdgeAndMlSample\DevVM
    ```

2. Ejecute el siguiente comando para permitir la ejecución de los scripts. Elija **Sí a todo** cuando se le solicite.

    ```powershell
    Set-ExecutionPolicy Bypass -Scope Process
    ```

3. Ejecute Create-AzureDevVM.ps1 desde este directorio.

    ```powershell
    .\Create-AzureDevVm.ps1
    ```

    * Cuando se le solicite, proporcione la siguiente información:
      * **Identificador de suscripción de Azure**: su Id. de suscripción; lo encontrará en Azure Portal.
      * **Nombre del grupo de recursos**: nombre de un grupo de recursos nuevo o existente en Azure.
      * **Ubicación**: seleccione la ubicación de Azure donde se creará la máquina virtual. Por ejemplo, westus2 o northeurope. Para obtener más información, consulte [Ubicaciones de Azure](https://azure.microsoft.com/global-infrastructure/locations/).
      * **AdminUsername**: proporcione un nombre fácil de recordar para la cuenta de administrador que quiere crear y usar en la máquina virtual.
      * **AdminPassword**: establezca una contraseña para la cuenta de administrador de la máquina virtual.

    * Si no tiene instalado Azure PowerShell, el script instalará el [módulo Az de Azure PowerShell](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-1.1.0).

    * Se le pedirá que inicie sesión en Azure.

    * El script confirma la información para crear la máquina virtual. Pulse `y` o `Enter` para continuar.

El script se ejecuta durante varios minutos mientras realiza los pasos siguientes:

* Creación del grupo de recursos si todavía no existe
* Implementación de la máquina virtual
* Habilitación de Hyper-V en la máquina virtual
* Instalación del software necesario para el desarrollo y clonación del repositorio de ejemplo
* Reinicie la máquina virtual
* Creación de un archivo RDP en el escritorio para conectarse a la máquina virtual

### <a name="set-auto-shutdown-schedule"></a>Establecimiento de la programación de apagado automático

Para ayudar a reducir los costos, la máquina virtual se crea con una programación de apagado automático que se establece en 1900 PST. Quizá necesite actualizar esta cifra según su ubicación y programación. Para actualizar la programación de apagado:

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).

2. Vaya a la máquina virtual en el grupo de recursos que proporcionó en la sección anterior.

3. Seleccione **Apagado automático** en el navegador lateral.

4. Escriba una nueva hora de apagado en **Apagado programado** o cambie la **Zona horaria** y después haga clic en **Guardar**.

### <a name="connect-and-configure-development-machine"></a>Conexión y configuración del equipo de desarrollo

Ahora que hemos creado una máquina virtual, es necesario finalizar la instalación del software necesario para completar el tutorial.

#### <a name="start-a-remote-desktop-session"></a>Inicio de una sesión de Escritorio remoto

1. El script de creación de la máquina virtual crea un archivo RDP en el escritorio.

2. Haga doble clic en el archivo **\<Nombre de la máquina virtual de Azure\>.rdp**.

3. Aparecerá un cuadro de diálogo que indica que el publicador de la conexión remota es desconocido. Haga clic en la casilla de verificación **No volver a preguntarme sobre conexiones a este equipo** y seleccione **Conectar**.

4. Cuando se le solicite, proporcione la AdminPassword que se usó cuando ejecutó el script para configurar la máquina virtual y haga clic en **Aceptar**.

5. Se le pedirá que acepte el certificado para la máquina virtual. Seleccione **No volver a preguntarme sobre conexiones a este equipo** y elija **Sí**.

#### <a name="install-visual-studio-code-extensions"></a>Instalación de extensiones de Visual Studio Code

Ahora que se ha conectado a la máquina de desarrollo, agregue algunas extensiones útiles a Visual Studio Code para facilitar la experiencia de desarrollo.

1. En una ventana de PowerShell, vaya a **C:\\source\\IoTEdgeAndMlSample\\DevVM**.

2. Escriba lo siguiente para permitir que los scripts se ejecuten en la máquina virtual.

    ```powershell
    Set-ExecutionPolicy Bypass -Scope CurrentUser -Force
    ```

3. Ejecute el script.

    ```powershell
    .\Enable-CodeExtensions.ps1
    ```

4. El script se ejecutará durante unos minutos para instalar las siguientes extensiones de VS Code:

    * Herramientas de Azure IoT
    * Python
    * C#
    * Docker
    * PowerShell

## <a name="set-up-iot-hub-and-storage"></a>Configuración de IoT Hub y Storage

Normalmente, estos pasos están a cargo de un desarrollador de nube.

Azure IoT Hub es el corazón de todas las aplicaciones de IoT. Se encarga de la comunicación segura entre dispositivos IoT y la nube. Es el punto de coordinación principal para el funcionamiento de la solución de aprendizaje automático de IoT Edge.

* IoT Hub utiliza rutas para dirigir los datos entrantes desde los dispositivos IoT hacia otros servicios de bajada. Aprovecharemos las rutas de IoT Hub para enviar datos del dispositivo a Azure Storage, donde Azure Machine Learning podrá consumirlos para entrenar el clasificador de vida útil restante.

* Más adelante en el tutorial, usaremos IoT Hub para configurar y administrar el dispositivo de Azure IoT Edge.

En esta sección, use un script para crear un centro de IoT de Azure y una cuenta de Azure Storage. A continuación, configure una ruta que reenvíe los datos que recibe el centro a un contenedor de Azure Storage Blob mediante Azure Portal. Estos pasos tardan aproximadamente 10 minutos en completarse.

### <a name="create-cloud-resources"></a>Creación de recursos en la nube

1. Abra una ventana de PowerShell en el equipo de desarrollo.

1. Cambie al directorio IoTHub.

    ```powershell
    cd C:\source\IoTEdgeAndMlSample\IoTHub
    ```

1. Ejecute el script de creación. Use los mismos valores de Id. de suscripción, ubicación y grupo de recursos que usó al crear la máquina virtual de desarrollo.

    ```powershell
    .\New-HubAndStorage.ps1 -SubscriptionId <subscription id> -Location
    <location> -ResourceGroupName <resource group>
    ```

    * Se le pedirá que inicie sesión en Azure.
    * El script confirma la información para crear su cuenta de IoT Hub y Storage. Pulse `y` o `Enter` para continuar.

El script tarda dos minutos en ejecutarse. Una finalizado, el script genera el nombre de la cuenta de almacenamiento y de IoT Hub.

### <a name="review-route-to-storage-in-iot-hub"></a>Revisión de la ruta al almacenamiento en IoT Hub

Como parte de la creación del centro de IoT, el script que se ejecutó en la sección anterior también crea un punto de conexión personalizado y una ruta. Las rutas de IoT Hub constan de una expresión de consulta y un punto de conexión. Si un mensaje coincide con la expresión, los datos se envían a lo largo de la ruta hasta el punto de conexión asociado. Los puntos de conexión pueden ser Event Hubs, colas de Service Bus y temas. En este caso, el punto de conexión es un contenedor de blobs en una cuenta de almacenamiento. Vamos a usar Azure Portal para revisar la ruta que creó el script.

1. Abra [Azure Portal](https://portal.azure.com).

1. Seleccione todos los servicios desde el navegador de la izquierda, escriba IoT en el cuadro de búsqueda y seleccione **IoT Hub**.

1. Seleccione la instancia de IoT Hub que creó en los pasos anteriores.

1. En el navegador lateral de IoT Hub, elija **Enrutamiento de mensajes**.

1. La página de enrutamiento de mensajes tiene dos pestañas, **Rutas** y **Puntos de conexión personalizados**. Seleccione la pestaña **Puntos de conexión personalizados**.

1. En **Blob Storage**, seleccione **turbofanDeviceStorage**.

1. Tenga en cuenta que este punto de conexión apunta a un contenedor de blobs denominado **devicedata** en la cuenta de almacenamiento que creó en el último paso, que se denomina **iotedgeandml\<sufijo único\>** .

1. También tenga en cuenta que se ha cambiado el formato predeterminado del **formato de nombre de archivo de Blob** ya que ahora se coloca la partición como último elemento del nombre. Este formato parece más práctico para las operaciones de archivo que se realizarán con Azure Notebooks más adelante durante el tutorial.

1. Cierre la hoja de detalles del punto de conexión para volver a la página **Enrutamiento de mensajes**.

1. Seleccione la pestaña **Rutas**.

1. Seleccione la ruta denominada **turbofanDeviceDataToStorage**.

1. Tenga en cuenta que el punto de conexión de la ruta es el punto de conexión personalizado **turbofanDeviceStorage**.

1. Examine la consulta de enrutamiento, que está establecida en **true**. Esto significa que todos los mensajes de telemetría de dispositivo coincidirán con esta ruta y, por tanto, todos los mensajes se enviarán al punto de conexión **turbofanDeviceStorage**.

1. Cierre los detalles de ruta.

## <a name="next-steps"></a>Pasos siguientes

En este artículo, creamos una instancia de IoT Hub y configuramos una ruta a una cuenta de Azure Storage. En el siguiente artículo, se enviarán datos de un conjunto de dispositivos simulados a la cuenta de almacenamiento a través de la instancia de IoT Hub. Más adelante en el tutorial, tras configurar al dispositivo y módulos IoT Edge, volveremos a revisar las rutas y profundizaremos un poco más en la consulta de enrutamiento.

Para obtener más información acerca de los pasos descritos en esta parte del tutorial de Machine Learning en IoT Edge, consulte:

* [Aspectos básicos de Azure IoT](https://docs.microsoft.com/azure/iot-fundamentals/)
* [Configuración del enrutamiento de mensajes con IoT Hub](../iot-hub/tutorial-routing.md)
* [Creación de una instancia de IoT Hub mediante Azure Portal](../iot-hub/iot-hub-create-through-portal.md)

Continúe con el siguiente artículo para crear un dispositivo simulado para supervisar.

> [!div class="nextstepaction"]
> [Generación de datos de dispositivo](tutorial-machine-learning-edge-03-generate-data.md)
