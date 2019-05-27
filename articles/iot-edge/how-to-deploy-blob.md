---
title: 'Implementar el módulo de Azure Blob Storage en dispositivos: Azure IoT Edge | Microsoft Docs'
description: Implementación de un módulo de Azure Blob Storage en un dispositivo de IoT Edge para almacenar datos en el perímetro.
author: kgremban
ms.author: kgremban
ms.date: 05/21/2019
ms.topic: article
ms.service: iot-edge
ms.custom: seodec18
ms.reviewer: arduppal
manager: philmea
ms.openlocfilehash: d844e81de9cfb556e91ab5c0d5a8074c822cce0a
ms.sourcegitcommit: cfbc8db6a3e3744062a533803e664ccee19f6d63
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/21/2019
ms.locfileid: "65990466"
---
# <a name="deploy-the-azure-blob-storage-on-iot-edge-module-to-your-device"></a>Implementar el almacenamiento de blobs de Azure en el módulo de IoT Edge en el dispositivo

Hay varias maneras de implementar módulos en un dispositivo IoT Edge y todos ellos funcionan en Azure Blob Storage en módulos de IoT Edge. Los dos métodos más sencillos son usar Azure Portal o las plantillas de Visual Studio Code.

## <a name="prerequisites"></a>Requisitos previos

- Una instancia de [IoT Hub](../iot-hub/iot-hub-create-through-portal.md) en la suscripción de Azure.
- Un [dispositivo de IoT Edge](how-to-register-device-portal.md) que tenga instalado el entorno de ejecución de Azure IoT Edge.
- [Código de Visual Studio](https://code.visualstudio.com/) y [herramientas de Azure IoT](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools) si la implementación desde Visual Studio Code.

## <a name="deploy-from-the-azure-portal"></a>Implementar desde el portal de Azure

El portal de Azure le guía a través de la creación de un manifiesto de implementación y la inserción de la implementación en un dispositivo IoT Edge.

### <a name="select-your-device"></a>Selección del dispositivo

1. Inicie sesión en [Azure Portal](https://portal.azure.com) y vaya a IoT Hub.
1. Seleccione **IoT Edge** en el menú.
1. Haga clic en el identificador del dispositivo de destino en la lista de dispositivos.
1. Seleccione **Set modules** (Establecer módulos).

### <a name="configure-a-deployment-manifest"></a>Configuración de un manifiesto de implementación

Un manifiesto de implementación es un documento JSON que describe qué módulos se van a implementar, cómo fluyen los datos entre los módulos y las propiedades deseadas de los módulos gemelos. El portal de Azure tiene un asistente que le guiará a través de la creación de un manifiesto de implementación, en lugar de generar manualmente el documento JSON. Consta de tres pasos: **Adición de módulos**, **Especificación de rutas** y **Revisión de la implementación**.

#### <a name="add-modules"></a>Adición de módulos

1. En la sección **Módulos de implementación** de la página, seleccione **Agregar**.

1. En los tipos de módulos en la lista desplegable, seleccione **módulo de IoT Edge**.

1. Proporcione un nombre para el módulo y, a continuación, especifique la imagen de contenedor:

   - **Nombre** -azureblobstorageoniotedge
   - **URI de la imagen** -mcr.microsoft.com/azure-blob-storage:latest

   > [!IMPORTANT]
   > Azure IoT Edge distingue mayúsculas de minúsculas al realizar llamadas a los módulos y el SDK de Storage también tiene como valor predeterminado a minúsculas. Aunque el nombre del módulo en el [Azure Marketplace](how-to-deploy-modules-portal.md#deploy-modules-from-azure-marketplace) es **AzureBlobStorageonIoTEdge**, cambiar el nombre a minúsculas ayuda a garantizar que las conexiones a Azure Blob Storage en el módulo de IoT Edge no se interrumpan.

1. El valor predeterminado **opciones de creación del contenedor** valores definen los enlaces de puerto que el contenedor necesita, pero también deberá agregar la información de su cuenta de almacenamiento y un enlace para el directorio de almacenamiento en el dispositivo. Reemplace el valor predeterminado JSON en el portal con el siguiente JSON:

   ```json
   {
     "Env":[
       "LOCAL_STORAGE_ACCOUNT_NAME=<your storage account name>",
       "LOCAL_STORAGE_ACCOUNT_KEY=<your storage account key>"
     ],
     "HostConfig":{
       "Binds":[
           "<storage directory bind>"
       ],
     "PortBindings":{
       "11002/tcp":[{"HostPort":"11002"}]
       }
     }
   }
   ```

1. Actualice el archivo JSON que ha copiado con la siguiente información:

   - Reemplace `<your storage account name>` por un nombre que pueda recordar. Los nombres de cuenta deben ser 3 y 24 caracteres, letras minúsculas y números. Sin espacios en blanco.

   - Actualice `<your storage account key>` por una clave base64 de 64 bytes. Puede generar una clave con herramientas como [GeneratePlus](https://generate.plus/en/base64?gp_base64_base[length]=64). Estas credenciales las usará para acceder a Blob Storage desde otros módulos.

   - Reemplace `<storage directory bind>` según el sistema operativo de contenedor. Especifique el nombre de un [volumen](https://docs.docker.com/storage/volumes/) o la ruta de acceso absoluta a un directorio del dispositivo de IoT Edge en el que desea que el módulo del blob almacene sus datos. El enlace del directorio de almacenamiento asigna una ubicación del dispositivo proporcionada a una ubicación establecida del módulo.

     - Contenedores de Linux, el formato es  *\<ruta de acceso de almacenamiento >: / blobroot*. Por ejemplo, **/srv/containerdata: / blobroot** o **Mi volumen: / blobroot**.
     - Contenedores de Windows, el formato es  *\<ruta de acceso de almacenamiento >: C: / BlobRoot*. Por ejemplo, **C: / ContainerData:C: / BlobRoot** o **Mi-volumen: C: / blobroot**.

     > [!IMPORTANT]
     > No cambie la segunda mitad del valor del enlace del directorio de almacenamiento, que señala a una ubicación específica del módulo. El enlace del directorio de almacenamiento siempre debe terminar con **:/blobroot** para contenedores de Linux y **:C:/BlobRoot** para contenedores de Windows.

    ![Opciones de creación del contenedor del módulo de actualización: portal](./media/how-to-store-data-blob/edit-module.png)

1. Establecer [niveles](how-to-store-data-blob.md#tiering-properties) y [time-to-live](how-to-store-data-blob.md#time-to-live-properties) propiedades para el módulo copiando el siguiente JSON y pegándolo en la **propiedades deseadas del gemelo de módulo de conjunto** cuadro. Configurar cada propiedad con un valor apropiado, guárdelo y continuar con la implementación.

   ```json
   {
     "properties.desired": {
       "ttlSettings": {
         "ttlOn": <true, false>,
         "timeToLiveInMinutes": <timeToLiveInMinutes>
       },
       "tieringSettings": {
         "tieringOn": <true, false>,
         "backlogPolicy": "<NewestFirst, OldestFirst>",
         "remoteStorageConnectionString": "DefaultEndpointsProtocol=https;AccountName=<your Azure Storage Account Name>;AccountKey=<your Azure Storage Account Key>; EndpointSuffix=<your end point suffix>",
         "tieredContainers": {
           "<source container name1>": {
             "target": "<target container name1>"
           }
         }
       }
     }
   }

      ```

   ![establecer las propiedades de los niveles y período de vida](./media/how-to-store-data-blob/iotedge_custom_module.png)

   Para obtener información sobre cómo configurar los niveles y TTL una vez implementado el módulo, consulte [editar módulo gemelo](https://github.com/Microsoft/vscode-azure-iot-toolkit/wiki/Edit-Module-Twin). Para obtener más información acerca de las propiedades deseadas, consulte [definir o actualizar las propiedades deseadas](module-composition.md#define-or-update-desired-properties).

1. Seleccione **Guardar**.

1. Seleccione **Siguiente** para ir a la sección de rutas.

#### <a name="specify-routes"></a>Especificación de rutas

Mantener las rutas predeterminadas y seleccione **siguiente** para continuar con la sección de revisión.

#### <a name="review-deployment"></a>Revisión de la implementación

La sección de revisión le muestra el manifiesto de implementación en formato JSON que se ha creado en función de las selecciones de las dos secciones anteriores. También hay dos módulos declaran que no agregue: **$edgeAgent** y **$edgeHub**. Estos dos módulos constituyen el [entorno de ejecución de Azure IoT Edge](iot-edge-runtime.md) y son valores predeterminados necesarios en todas las implementaciones.

Revise la información de implementación y seleccione **Enviar**.

### <a name="verify-your-deployment"></a>Comprobar la implementación

Después de enviar la implementación, vuelva a la página **IoT Edge** del centro de IoT.

1. Seleccione el dispositivo IoT Edge elegido como destino con la implementación para abrir sus detalles.
1. En los detalles del dispositivo, compruebe que el módulo de Blob Storage aparece como **Especificado en la implementación** y **Notificado por el dispositivo**.

Puede tardar unos minutos para que el módulo se inicie en el dispositivo y, a continuación, notifique a IoT Hub. Actualice la página para ver el estado actualizado.

## <a name="deploy-from-visual-studio-code"></a>Implementar desde Visual Studio Code

Azure IoT Edge proporciona plantillas de Visual Studio Code que facilitan el desarrollo de soluciones de perímetro. Siga estos pasos para crear una nueva solución de IoT Edge con un módulo de almacenamiento de blobs y configurar el manifiesto de implementación.

1. Seleccione **Ver** > **Paleta de comandos**.

1. En la paleta de comandos, escriba y ejecute el comando **Azure IoT Edge: New IoT Edge solution** (Azure IoT Edge: nueva solución de IoT Edge).

   ![Ejecución de la nueva solución IoT Edge](./media/how-to-develop-csharp-module/new-solution.png)

   Para crear la solución, siga las indicaciones de la paleta de comandos.

   | Campo | Valor |
   | ----- | ----- |
   | Seleccionar carpeta | Elija la ubicación en el equipo de desarrollo para Visual Studio Code crear los archivos de solución. |
   | Proporcionar un nombre de la solución | Escriba un nombre descriptivo para la solución o acepte el valor predeterminado **EdgeSolution**. |
   | Seleccionar plantilla del módulo | Elija **Existing Module (Enter full image URL)** (Módulo existente [escribir URL completa de la imagen]). |
   | Proporcionar un nombre de módulo | Escriba un nombre en minúsculas para el módulo, como **azureblobstorage**.<br /><br />Es importante usar un nombre en minúsculas para el módulo de Azure Blob Storage en IoT Edge. IoT Edge distingue mayúsculas y minúsculas al hacer referencia a módulos y el SDK de Storage toma como valor predeterminado la minúsculas. |
   | Provide Docker image for the module (Proporcionar imagen de Docker para el módulo) | Proporcione el identificador URI: **mcr.microsoft.com/azure-blob-storage:latest** |

   Visual Studio Code toma la información que le ha proporcionado, crea una solución IoT Edge y la carga en una nueva ventana. La plantilla de solución crea una plantilla de manifiesto de implementación que incluye la imagen del módulo de Blob Storage, pero es preciso configurar las opciones de creación del módulo.

1. Abra *deployment.template.json* en el área de trabajo de la solución nueva y busque la sección **modules**. Realice los cambios de configuración siguientes:

   1. Elimine el módulo **tempSensor**, ya que no es necesaria para esta implementación.

   1. Copie y pegue el código siguiente en el `createOptions` campo:

      ```json
      "Env":[
       "LOCAL_STORAGE_ACCOUNT_NAME=<your storage account name>",
       "LOCAL_STORAGE_ACCOUNT_KEY=<your storage account key>"
      ],
      "HostConfig":{
        "Binds": ["<storage directory bind>"],
        "PortBindings":{
          "11002/tcp": [{"HostPort":"11002"}]
        }
      }
      ```

      ![Actualizar módulo createOptions - Visual Studio Code](./media/how-to-store-data-blob/create-options.png)

1. Reemplace `<your storage account name>` por un nombre que pueda recordar. Los nombres de cuenta deben ser 3 y 24 caracteres, letras minúsculas y números. Sin espacios en blanco.

1. Actualice `<your storage account key>` por una clave base64 de 64 bytes. Puede generar una clave con herramientas como [GeneratePlus](https://generate.plus/en/base64?gp_base64_base[length]=64). Estas credenciales las usará para acceder a Blob Storage desde otros módulos.

1. Reemplace `<storage directory bind>` según el sistema operativo de contenedor. Especifique el nombre de un [volumen](https://docs.docker.com/storage/volumes/) o la ruta de acceso absoluta a un directorio del dispositivo de IoT Edge en el que desea que el módulo del blob almacene sus datos. El enlace del directorio de almacenamiento asigna una ubicación del dispositivo proporcionada a una ubicación establecida del módulo.  

      - Contenedores de Linux, el formato es  *\<ruta de acceso de almacenamiento >: / blobroot*. Por ejemplo, **/srv/containerdata: / blobroot** o **Mi volumen: / blobroot**.
      - Contenedores de Windows, el formato es  *\<ruta de acceso de almacenamiento >: C: / BlobRoot*. Por ejemplo, **C: / ContainerData:C: / BlobRoot** o **Mi-volumen: C: / blobroot**.

      > [!IMPORTANT]
      > No cambie la segunda mitad del valor del enlace del directorio de almacenamiento, que señala a una ubicación específica del módulo. El enlace del directorio de almacenamiento siempre debe terminar con **:/blobroot** para contenedores de Linux y **:C:/BlobRoot** para contenedores de Windows.

1. Configurar [niveles](how-to-store-data-blob.md#tiering-properties) y [time-to-live](how-to-store-data-blob.md#time-to-live-properties) propiedades para el módulo agregando el siguiente JSON para el *deployment.template.json* archivo. Configure cada propiedad con un valor apropiado y guarde el archivo.

   ```json
   "<your azureblobstorageoniotedge module name>":{
     "properties.desired": {
       "ttlSettings": {
         "ttlOn": <true, false>,
         "timeToLiveInMinutes": <timeToLiveInMinutes>
       },
       "tieringSettings": {
         "tieringOn": <true, false>,
         "backlogPolicy": "<NewestFirst, OldestFirst>",
         "remoteStorageConnectionString": "DefaultEndpointsProtocol=https;AccountName=<your Azure Storage Account Name>;AccountKey=<your Azure Storage Account Key>;EndpointSuffix=<your end point suffix>",
         "tieredContainers": {
           "<source container name1>": {
             "target": "<target container name1>"
           }
         }
       }
     }
   }
   ```

   ![establecer las propiedades deseadas para azureblobstorageoniotedge - Visual Studio Code](./media/how-to-store-data-blob/tiering_ttl.png)

   Para obtener información sobre cómo configurar los niveles y TTL una vez implementado el módulo, consulte [editar módulo gemelo](https://github.com/Microsoft/vscode-azure-iot-toolkit/wiki/Edit-Module-Twin). Para obtener más información sobre el contenedor, opciones de creación, reinicie la directiva y el estado deseado, consulte [propiedades deseadas de EdgeAgent](module-edgeagent-edgehub.md#edgeagent-desired-properties).

1. Guarde el archivo *deployment.template.json*.

1. Haga clic con el botón derecho en **deployment.template.json** y seleccione **Generar manifiesto de implementación de IoT Edge**.

1. Código de Visual Studio toma la información que proporcionó en *deployment.template.json* y lo usa para crear un nuevo archivo de manifiesto de implementación. El manifiesto de implementación se crea en una carpeta **config** nueva en el área de trabajo de la solución. Cuando tenga el archivo, puede seguir los pasos de [Implementación de módulos de Azure IoT Edge desde Visual Studio Code](how-to-deploy-modules-vscode.md) o [Implementación de módulos de Azure IoT Edge con la CLI de Azure 2.0](how-to-deploy-modules-cli.md).

## <a name="deploy-multiple-module-instances"></a>Implementación de varias instancias de módulo

Si desea implementar varias instancias de Azure Blob Storage en el módulo de IoT Edge, deberá proporcionar una ruta de acceso de almacenamiento diferente y cambiar el `HostPort` valor que se enlaza el módulo. Los módulos de Blob Storage siempre exponen el puerto 11002 del contenedor, pero se puede declarar el puerto al que está enlazado en el host.

Editar **opciones de creación del contenedor** (en el portal de Azure) o el **createOptions** campo (en el *deployment.template.json* archivo en Visual Studio Code) para cambiar el `HostPort` valor:

```json
"PortBindings":{
  "11002/tcp": [{"HostPort":"<port number>"}]
}
```

Cuando se conecta a módulos de Blob Storage adicionales, cambie el punto de conexión para que apunte al puerto de host actualizado.

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre los manifiestos de implementación y cómo crearlos, consulte [Descripción de cómo se pueden utilizar, configurar y reutilizar los módulos de IoT Edge](module-composition.md).
