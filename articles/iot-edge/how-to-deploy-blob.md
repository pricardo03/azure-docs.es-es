---
title: 'Implementación del módulo de Azure Blob Storage en dispositivos: Azure IoT Edge | Microsoft Docs'
description: Implementación de un módulo de Azure Blob Storage en un dispositivo de IoT Edge para almacenar datos en el perímetro.
author: kgremban
ms.author: kgremban
ms.date: 08/07/2019
ms.topic: article
ms.service: iot-edge
ms.custom: seodec18
ms.reviewer: kgremban
manager: mchad
ms.openlocfilehash: 089c90abb999751db77bbe1d89d1d118ae712b52
ms.sourcegitcommit: 124c3112b94c951535e0be20a751150b79289594
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/10/2019
ms.locfileid: "68947074"
---
# <a name="deploy-the-azure-blob-storage-on-iot-edge-module-to-your-device"></a>Implementación del módulo de Azure Blob Storage en IoT Edge en el dispositivo

Hay varias maneras de implementar módulos en un dispositivo de IoT Edge, y todas ellas funcionan en los módulos de Azure Blob Storage en IoT Edge. Los dos métodos más sencillos son usar Azure Portal o las plantillas de Visual Studio Code.

## <a name="prerequisites"></a>Requisitos previos

- Una instancia de [IoT Hub](../iot-hub/iot-hub-create-through-portal.md) en la suscripción de Azure.
- Un [dispositivo de IoT Edge](how-to-register-device-portal.md) que tenga instalado el entorno de ejecución de Azure IoT Edge.
- [Visual Studio Code](https://code.visualstudio.com/) y las [Azure IoT Tools](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools) si la implementación se hace desde Visual Studio Code.

## <a name="deploy-from-the-azure-portal"></a>Implementación desde Azure Portal

Azure Portal le guía en la creación de un manifiesto de implementación y la inserción de la implementación en un dispositivo de IoT Edge.

### <a name="select-your-device"></a>Selección del dispositivo

1. Inicie sesión en [Azure Portal](https://portal.azure.com) y vaya a IoT Hub.
1. Seleccione **IoT Edge** en el menú.
1. Haga clic en el identificador del dispositivo de destino en la lista de dispositivos.
1. Seleccione **Set modules** (Establecer módulos).

### <a name="configure-a-deployment-manifest"></a>Configuración de un manifiesto de implementación

Un manifiesto de implementación es un documento JSON que describe qué módulos se van a implementar, cómo fluyen los datos entre los módulos y las propiedades deseadas de los módulos gemelos. Azure Portal tiene un asistente que le guía en la creación de un manifiesto de implementación, en lugar de crear el documento JSON de forma manual. Consta de tres pasos: **Adición de módulos**, **Especificación de rutas** y **Revisión de la implementación**.

#### <a name="add-modules"></a>Adición de módulos

1. En la sección **Módulos de implementación** de la página, seleccione **Agregar**.

1. En los tipos de módulos de la lista desplegable, seleccione **Módulo IoT Edge**.

1. Proporcione un nombre para el módulo y especifique la imagen de contenedor:

   - **Nombre**: azureblobstorageoniotedge
   - **URI de la imagen**: mcr.microsoft.com/azure-blob-storage:latest

   > [!IMPORTANT]
   > Azure IoT Edge distingue mayúsculas y minúsculas al realizar llamadas a los módulos y el SDK de Storage también toma como valor predeterminado la minúsculas. Aunque el nombre del módulo en [Azure Marketplace](how-to-deploy-modules-portal.md#deploy-modules-from-azure-marketplace) es **AzureBlobStorageonIoTEdge**, cambiar el nombre a minúsculas ayuda a garantizar que las conexiones al módulo Azure Blob Storage en IoT Edge no se interrumpan.

1. Los valores predeterminados de **Container Create Options** definen los enlaces de puerto que el contenedor necesita, pero también debe agregar la información de su cuenta de almacenamiento y un montaje para el almacenamiento del dispositivo. Reemplace el código JSON predeterminado en el portal por el siguiente código JSON:

   ```json
   {
     "Env":[
       "LOCAL_STORAGE_ACCOUNT_NAME=<your storage account name>",
       "LOCAL_STORAGE_ACCOUNT_KEY=<your storage account key>"
     ],
     "HostConfig":{
       "Binds":[
           "<storage mount>"
       ],
       "PortBindings":{
         "11002/tcp":[{"HostPort":"11002"}]
       }
     }
   }
   ```

1. Actualice el archivo JSON que ha copiado con la siguiente información:

   - Reemplace `<your storage account name>` por un nombre que pueda recordar. Los nombres de cuenta tener de 3 a 24 caracteres, con letras en minúscula y números. No deben tener espacios.

   - Actualice `<your storage account key>` por una clave base64 de 64 bytes. Puede generar una clave con herramientas como [GeneratePlus](https://generate.plus/en/base64?gp_base64_base[length]=64). Estas credenciales las usará para acceder a Blob Storage desde otros módulos.

   - Reemplace `<storage mount>` según el sistema operativo del contenedor. Especifique el nombre de un [volumen](https://docs.docker.com/storage/volumes/) o la ruta de acceso absoluta a un directorio del dispositivo de IoT Edge en el que desea que el módulo del blob almacene sus datos. El montaje de almacenamiento asigna una ubicación del dispositivo proporcionada a una ubicación establecida del módulo.

     - En el caso de contenedores de Linux, el formato es *\<ruta de acceso de almacenamiento o volumen>: /blobroot*. Por ejemplo:
         - use [montaje de volumen](https://docs.docker.com/storage/volumes/): **mi-volumen:/blobroot** 
         - use [montaje de enlace](https://docs.docker.com/storage/bind-mounts/): **/srv/containerdata:/blobroot**. Asegúrese de seguir los pasos para [conceder acceso al directorio al usuario del contenedor](how-to-store-data-blob.md#granting-directory-access-to-container-user-on-linux).
     - En el caso de contenedores de Windows, el formato es *\<ruta de acceso de almacenamiento o volumen>:C:/BlobRoot*. Por ejemplo:
         - use [montaje de volumen](https://docs.docker.com/storage/volumes/): **mi-volumen:C:/blobroot**. 
         - use [montaje de enlace](https://docs.docker.com/storage/bind-mounts/): **C:/ContainerData:C:/BlobRoot**.
         - En lugar de usar la unidad local, puede asignar la ubicación de red de SMB. Para obtener más información, consulte [uso del recurso compartido de SMB como almacenamiento local](how-to-store-data-blob.md#using-smb-share-as-your-local-storage).

     > [!IMPORTANT]
     > No cambie la segunda mitad del valor del montaje de almacenamiento, que señala a una ubicación específica del módulo. El montaje de almacenamiento siempre debe terminar con **:/blobroot** para contenedores de Linux y **:C:/BlobRoot** para contenedores de Windows.

1. Establezca las propiedades [deviceToCloudUploadProperties](how-to-store-data-blob.md#devicetoclouduploadproperties) y [deviceAutoDeleteProperties](how-to-store-data-blob.md#deviceautodeleteproperties) del módulo. Para ello, copie el siguiente código JSON y péguelo en el cuadro **Propiedades deseadas del módulo gemelo**. Configure cada propiedad con un valor apropiado, guárdela y siga con la implementación.

   ```json
   {
     "properties.desired": {
       "deviceAutoDeleteProperties": {
         "deleteOn": <true, false>,
         "deleteAfterMinutes": <timeToLiveInMinutes>,
         "retainWhileUploading":<true,false>
       },
       "deviceToCloudUploadProperties": {
         "uploadOn": <true, false>,
         "uploadOrder": "<NewestFirst, OldestFirst>",
         "cloudStorageConnectionString": "DefaultEndpointsProtocol=https;AccountName=<your Azure Storage Account Name>;AccountKey=<your Azure Storage Account Key>; EndpointSuffix=<your end point suffix>",
         "storageContainersForUpload": {
           "<source container name1>": {
             "target": "<target container name1>"
           }
         },
         "deleteAfterUpload":<true,false>
       }
     }
   }

      ```

   ![Establecer las opciones de creación de contenedor y las propiedades deviceAutoDeleteProperties y deviceToCloudUploadProperties](./media/how-to-deploy-blob/iotedge-custom-module.png)

   Para obtener información sobre cómo configurar deviceToCloudUploadProperties y deviceAutoDeleteProperties una vez implementado el módulo, consulte [Edición del módulo gemelo](https://github.com/Microsoft/vscode-azure-iot-toolkit/wiki/Edit-Module-Twin). Para más información sobre las propiedades deseadas, consulte [Definición o actualización de las propiedades deseadas](module-composition.md#define-or-update-desired-properties).

1. Seleccione **Guardar**.

1. Seleccione **Siguiente** para ir a la sección de rutas.

#### <a name="specify-routes"></a>Especificación de rutas

Conserve las rutas predeterminadas y seleccione **Siguiente** para continuar con la sección de revisión.

#### <a name="review-deployment"></a>Revisión de la implementación

La sección de revisión le muestra el manifiesto de implementación en formato JSON que se ha creado en función de las selecciones de las dos secciones anteriores. También hay dos módulos declarados que no agregó: **$edgeAgent** y **$edgeHub**. Estos dos módulos constituyen el [entorno de ejecución de Azure IoT Edge](iot-edge-runtime.md) y son valores predeterminados necesarios en todas las implementaciones.

Revise la información de implementación y seleccione **Enviar**.

### <a name="verify-your-deployment"></a>Comprobación de la implementación

Después de enviar la implementación, vuelva a la página **IoT Edge** del centro de IoT.

1. Seleccione el dispositivo IoT Edge elegido como destino con la implementación para abrir sus detalles.
1. En los detalles del dispositivo, compruebe que el módulo de Blob Storage aparece como **Especificado en la implementación** y **Notificado por el dispositivo**.

Puede tardar unos minutos para que el módulo se inicie en el dispositivo y, a continuación, notifique a IoT Hub. Actualice la página para ver el estado actualizado.

## <a name="deploy-from-visual-studio-code"></a>Implementación desde Visual Studio Code

Azure IoT Edge proporciona plantillas de Visual Studio Code que facilitan el desarrollo de soluciones de perímetro. Siga estos pasos para crear una nueva solución de IoT Edge con un módulo de Blob Storage y configurar el manifiesto de implementación.

1. Seleccione **Ver** > **Paleta de comandos**.

1. En la paleta de comandos, escriba y ejecute el comando **Azure IoT Edge: New IoT Edge solution** (Azure IoT Edge: nueva solución de IoT Edge).

   ![Ejecución de la nueva solución IoT Edge](./media/how-to-develop-csharp-module/new-solution.png)

   Para crear la solución, siga las indicaciones de la paleta de comandos.

   | Campo | Valor |
   | ----- | ----- |
   | Seleccionar carpeta | Elija la ubicación en el equipo de desarrollo en la que Visual Studio Code creará los archivos de la solución. |
   | Proporcionar un nombre de la solución | Escriba un nombre descriptivo para la solución o acepte el valor predeterminado **EdgeSolution**. |
   | Seleccionar plantilla del módulo | Elija **Existing Module (Enter full image URL)** (Módulo existente [escribir URL completa de la imagen]). |
   | Proporcionar un nombre de módulo | Escriba un nombre en minúsculas para el módulo, como **azureblobstorageoniotedge**.<br /><br />Es importante usar un nombre en minúsculas para el módulo de Azure Blob Storage en IoT Edge. IoT Edge distingue mayúsculas y minúsculas al hacer referencia a módulos y el SDK de Storage toma como valor predeterminado la minúsculas. |
   | Provide Docker image for the module (Proporcionar imagen de Docker para el módulo) | Proporcione el identificador URI: **mcr.microsoft.com/azure-blob-storage:latest** |

   Visual Studio Code toma la información que le ha proporcionado, crea una solución IoT Edge y la carga en una nueva ventana. La plantilla de solución crea una plantilla de manifiesto de implementación que incluye la imagen del módulo de Blob Storage, pero es preciso configurar las opciones de creación del módulo.

1. Abra *deployment.template.json* en el área de trabajo de la solución nueva y busque la sección **modules**. Realice los cambios de configuración siguientes:

   1. Elimine el módulo **SimulatedTemperatureSensor**, ya que no es necesario para esta implementación.

   1. Copie y pegue el código siguiente en el campo `createOptions`:

      ```json
      "Env":[
       "LOCAL_STORAGE_ACCOUNT_NAME=<your storage account name>",
       "LOCAL_STORAGE_ACCOUNT_KEY=<your storage account key>"
      ],
      "HostConfig":{
        "Binds": ["<storage mount>"],
        "PortBindings":{
          "11002/tcp": [{"HostPort":"11002"}]
        }
      }
      ```

      ![Actualizar el elemento createOptions del módulo: Visual Studio Code](./media/how-to-deploy-blob/create-options.png)

1. Reemplace `<your storage account name>` por un nombre que pueda recordar. Los nombres de cuenta tener de 3 a 24 caracteres, con letras en minúscula y números. No deben tener espacios.

1. Actualice `<your storage account key>` por una clave base64 de 64 bytes. Puede generar una clave con herramientas como [GeneratePlus](https://generate.plus/en/base64?gp_base64_base[length]=64). Estas credenciales las usará para acceder a Blob Storage desde otros módulos.

1. Reemplace `<storage mount>` según el sistema operativo del contenedor. Especifique el nombre de un [volumen](https://docs.docker.com/storage/volumes/) o la ruta de acceso absoluta a un directorio del dispositivo de IoT Edge en el que desea que el módulo del blob almacene sus datos. El montaje de almacenamiento asigna una ubicación del dispositivo proporcionada a una ubicación establecida del módulo.  

      
     - En el caso de contenedores de Linux, el formato es *\<ruta de acceso de almacenamiento o volumen>: /blobroot*. Por ejemplo:
         - use [montaje de volumen](https://docs.docker.com/storage/volumes/): **mi-volumen:/blobroot** 
         - use [montaje de enlace](https://docs.docker.com/storage/bind-mounts/): **/srv/containerdata:/blobroot**. Asegúrese de seguir los pasos para [conceder acceso al directorio al usuario del contenedor](how-to-store-data-blob.md#granting-directory-access-to-container-user-on-linux).
     - En el caso de contenedores de Windows, el formato es *\<ruta de acceso de almacenamiento o volumen>:C:/BlobRoot*. Por ejemplo:
         - use [montaje de volumen](https://docs.docker.com/storage/volumes/): **mi-volumen:C:/blobroot**. 
         - use [montaje de enlace](https://docs.docker.com/storage/bind-mounts/): **C:/ContainerData:C:/BlobRoot**.
         - En lugar de usar la unidad local, puede asignar la ubicación de red de SMB. Para obtener más información, consulte [uso del recurso compartido de SMB como almacenamiento local](how-to-store-data-blob.md#using-smb-share-as-your-local-storage).

     > [!IMPORTANT]
     > No cambie la segunda mitad del valor del montaje de almacenamiento, que señala a una ubicación específica del módulo. El montaje de almacenamiento siempre debe terminar con **:/blobroot** para contenedores de Linux y **:C:/BlobRoot** para contenedores de Windows.

1. Configure los elementos [deviceToCloudUploadProperties](how-to-store-data-blob.md#devicetoclouduploadproperties) y [deviceAutoDeleteProperties](how-to-store-data-blob.md#deviceautodeleteproperties) del módulo. Para ello, agregue el siguiente código JSON al archivo *deployment.template.json*. Configure cada propiedad con un valor adecuado y guarde el archivo.

   ```json
   "<your azureblobstorageoniotedge module name>":{
     "properties.desired": {
       "deviceAutoDeleteProperties": {
         "deleteOn": <true, false>,
         "deleteAfterMinutes": <timeToLiveInMinutes>,
         "retainWhileUploading": <true, false>
       },
       "deviceToCloudUploadProperties": {
         "uploadOn": <true, false>,
         "uploadOrder": "<NewestFirst, OldestFirst>",
         "cloudStorageConnectionString": "DefaultEndpointsProtocol=https;AccountName=<your Azure Storage Account Name>;AccountKey=<your Azure Storage Account Key>;EndpointSuffix=<your end point suffix>",
         "storageContainersForUpload": {
           "<source container name1>": {
             "target": "<target container name1>"
           }
         },
         "deleteAfterUpload": <true, false>
       }
     }
   }
   ```

   ![Establecer las propiedades deseadas para azureblobstorageoniotedge: Visual Studio Code](./media/how-to-deploy-blob/devicetocloud-deviceautodelete.png)

   Para obtener información sobre cómo configurar deviceToCloudUploadProperties y deviceAutoDeleteProperties una vez implementado el módulo, consulte [Edición del módulo gemelo](https://github.com/Microsoft/vscode-azure-iot-toolkit/wiki/Edit-Module-Twin). Para más información sobre las opciones de creación del contenedor, la directiva de reinicio y el estado deseado, consulte [Propiedades deseadas de EdgeAgent](module-edgeagent-edgehub.md#edgeagent-desired-properties).

1. Guarde el archivo *deployment.template.json*.

1. Haga clic con el botón derecho en **deployment.template.json** y seleccione **Generar manifiesto de implementación de IoT Edge**.

1. Visual Studio Code toma la información que especificó en *deployment.template.json* y la usa para crear un nuevo archivo de manifiesto de implementación. El manifiesto de implementación se crea en una carpeta **config** nueva en el área de trabajo de la solución. Cuando tenga el archivo, puede seguir los pasos de [Implementación de módulos de Azure IoT Edge desde Visual Studio Code](how-to-deploy-modules-vscode.md) o [Implementación de módulos de Azure IoT Edge con la CLI de Azure 2.0](how-to-deploy-modules-cli.md).

## <a name="deploy-multiple-module-instances"></a>Implementación de varias instancias de módulo

Si desea implementar varias instancias del módulo Azure Blob Storage en IoT Edge, debe proporcionar una ruta de acceso de almacenamiento diferente y cambiar el valor `HostPort` al que se enlaza el módulo. Los módulos de Blob Storage siempre exponen el puerto 11002 del contenedor, pero se puede declarar el puerto al que está enlazado en el host.

Edite el campo **Opciones de creación del contenedor** (en Azure Portal) o **createOptions** (en el archivo *deployment.template.json* en Visual Studio Code) para cambiar el valor `HostPort`:

```json
"PortBindings":{
  "11002/tcp": [{"HostPort":"<port number>"}]
}
```

Cuando se conecta a módulos de Blob Storage adicionales, cambie el punto de conexión para que apunte al puerto de host actualizado.

## <a name="next-steps"></a>Pasos siguientes
Obtenga más información sobre [Azure Blob Storage en IoT Edge](how-to-store-data-blob.md).

Para más información sobre los manifiestos de implementación y cómo crearlos, consulte [Descripción de cómo se pueden utilizar, configurar y reutilizar los módulos de IoT Edge](module-composition.md).
