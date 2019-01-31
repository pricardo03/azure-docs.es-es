---
title: 'Almacenamiento de blob en bloques en los dispositivos: Azure IoT Edge | Microsoft Docs'
description: Implementación de un módulo de Azure Blob Storage en un dispositivo de IoT Edge para almacenar datos en el perímetro.
author: kgremban
manager: philmea
ms.author: kgremban
ms.reviewer: arduppal
ms.date: 01/04/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: 9faed53540d449f8658655ff7285b38aa20bee6c
ms.sourcegitcommit: 644de9305293600faf9c7dad951bfeee334f0ba3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/25/2019
ms.locfileid: "54901826"
---
# <a name="store-data-at-the-edge-with-azure-blob-storage-on-iot-edge-preview"></a>Almacene datos en el perímetro con Azure Blob Storage en IoT Edge (versión preliminar)

Azure Blob Storage en IoT Edge proporciona una solución de almacenamiento de [blobs en bloques](https://docs.microsoft.com/rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs#about-block-blobs) en el perímetro. En los dispositivos con IoT Edge, los módulos de almacenamiento de blobs se comportan como un servicio de blobs en bloques de Azure, pero los blobs en bloques se almacenan localmente en el dispositivo IoT Edge. Para acceder a los blobs puede usar los mismos métodos que el SDK de Azure Storage o las llamadas API del blob en bloques a las que está acostumbrado. 

Como buenos ejemplos para usar este módulo destacan escenarios donde es necesario almacenar de forma local datos como vídeos, imágenes, datos financieros, datos de hospitales o cualquier otro dato, que posteriormente puedan procesarse localmente o transferirse a la nube.

En este artículo se proporcionan instrucciones para la implementación de Azure Blob Storage en un contenedor de IoT Edge en el que se ejecuta un servicio de blobs en un dispositivo de IoT Edge. 

>[!NOTE]
>Azure Blob Storage en IoT Edge está en [versión preliminar pública](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). 

## <a name="prerequisites"></a>Requisitos previos

Un dispositivo de Azure IoT Edge:

* Puede usar la máquina de desarrollo o una máquina virtual como dispositivo Edge siguiendo los pasos que se indican en la guía de inicio rápido para dispositivos de [Linux](quickstart-linux.md) o de [Windows](quickstart.md).
* El módulo de Azure Blob Storage en IoT Edge admite las siguientes configuraciones de dispositivo:

   | Sistema operativo | Arquitectura |
   | ---------------- | ------------ |
   | Ubuntu Server 16.04 | AMD64 |
   | Ubuntu Server 18.04 | AMD64 |
   | IoT Core para Windows 10 (actualización de octubre) | AMD64 |
   | IoT Enterprise para Windows 10 (actualización de octubre) | AMD64 |
   | Windows Server 2019 | AMD64 |
   | Raspbian Stretch | ARM32 |

Recursos en la nube:

* Una instancia de [IoT Hub](../iot-hub/iot-hub-create-through-portal.md) de nivel estándar en Azure. 


## <a name="deploy-blob-storage-to-your-device"></a>Implementación de Blob Storage en un dispositivo

Hay varias maneras de implementar módulos en un dispositivo de IoT Edge, y todas ellas funcionan en los módulos de Azure Blob Storage en IoT Edge. Los dos métodos más sencillos son usar Azure Portal o las plantillas de Visual Studio Code. 

### <a name="azure-portal"></a>Azure Portal

Azure Marketplace ofrece módulos IoT Edge que se pueden implementar directamente en sus dispositivos IoT Edge, incluido Azure Blob Storage en IoT Edge. Siga estos pasos para implementar el módulo en Azure Portal.

1. En [Azure Portal](https://portal.azure.com), busque "Azure Blob Storage en IoT Edge". **Seleccione** el resultado de la búsqueda en Marketplace.

   ![Creación del módulo en Marketplace](./media/how-to-store-data-blob/marketplace-module.png)

2. Elija un dispositivo IoT Edge para recibir este módulo. En la página **Dispositivos de destino para el módulo de IoT Edge**, proporcione la siguiente información:

   1. Seleccione la **suscripción** que contiene el centro de IoT que usa.

   2. Elija **IoT Hub**.

   3. Si conoce el **nombre del dispositivo IoT Edge**, escríbalo en el cuadro de texto. También puede seleccionar **Buscar dispositivo** para elegir de una lista de dispositivos IoT Edge en el centro de IoT. 
   
   4. Seleccione **Crear**.

   Ahora que eligió un módulo de IoT Edge en Azure Marketplace y un dispositivo IoT Edge para recibir el módulo, verá un asistente de tres pasos que lo ayudará a definir exactamente cómo se implementará el módulo.

3. En el paso **Agregar módulos** del Asistente para establecer módulos, tenga en cuenta que el módulo **AzureBlobStorageonIoTEdge** ya aparece en **Módulos de implementación**. 

2. Seleccione el módulo de Blob Storage de la lista de módulos de implementación para abrir los detalles del módulo. 

   ![Selección del nombre del módulo para abrir los detalles del módulo](./media/how-to-store-data-blob/open-module-details.png)

3. En la página **Módulos personalizados de IoT Edge**, actualice el módulo Azure Blob Storage en IoT Edge siguiendo estos pasos:

   1. Cambie el **nombre** del módulo a minúsculas. Puede modificar el nombre del módulo si lo desea, o usar `azureblobstorageoniotedge`. 

      >[!IMPORTANT]
      >Azure IoT Edge distingue mayúsculas y minúsculas al realizar llamadas a los módulos y el SDK de Storage toma como valor predeterminado la minúsculas. Para asegurarse de que no se interrumpan las conexiones al módulo de Azure Blob Storage en IoT Edge, asígnele un nombre en minúsculas. 

   2. Las **opciones de creación del contenedor** son los enlaces de puerto que el contenedor necesita, pero también debe agregar la información de su cuenta de almacenamiento y un enlace para el directorio de almacenamiento del dispositivo. Sobrescriba el archivo JSON en el portal con el siguiente JSON:
    
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
   3. Actualice el archivo JSON que ha copiado con la siguiente información: 

      * Reemplace `<your storage account name>` por un nombre que pueda recordar. Los nombres de cuenta deben tener entre 3 y 24 caracteres, e incluir números y letras minúsculas.
      * Actualice `<your storage account key>` por una clave base64 de 64 bytes. Puede generar una clave con herramientas como [GeneratePlus](https://generate.plus/en/base64?gp_base64_base[length]=64). Estas credenciales las usará para acceder a Blob Storage desde otros módulos.
      * Reemplace `<storage directory bind>` según el sistema operativo del contenedor. Especifique el nombre de un [volumen](https://docs.docker.com/storage/volumes/) o la ruta de acceso absoluta a un directorio del dispositivo de IoT Edge en el que desea que el módulo del blob almacene sus datos. El enlace del directorio de almacenamiento asigna una ubicación del dispositivo proporcionada a una ubicación establecida del módulo. 

         * Contenedores de Linux: **\<ruta de acceso de almacenamiento >:/blobroot**. Por ejemplo, /srv/containerdata:/blobroot. O bien, my-volume:/blobroot. 
         * Contenedores de Windows: **\<ruta de acceso de almacenamiento >: C:/BlobRoot**. Por ejemplo, C:/ContainerData:C:/BlobRoot. O bien, my-volume:C:/blobroot.
   
      > [!IMPORTANT]
      > No cambie la segunda mitad del valor del enlace del directorio de almacenamiento, que señala a una ubicación específica del módulo. El enlace del directorio de almacenamiento siempre debe terminar con **:/blobroot** para contenedores de Linux y **:C:/BlobRoot** para contenedores de Windows.

      ![Opciones de creación del contenedor del módulo de actualización: portal](./media/how-to-store-data-blob/edit-module.png)

   4. Seleccione **Guardar**.

4. Seleccione **Siguiente** para pasar al próximo paso del asistente.
5. En el paso **Especificar rutas**, seleccione **Siguiente**.
6. En el paso **Review Deployment** (Revisar implementación) del asistente, seleccione **Submit** (Enviar).
7. Después de enviar la implementación, vuelva a la página **IoT Edge** del centro de IoT. Seleccione el dispositivo IoT Edge elegido como destino con la implementación para abrir sus detalles. 
8. En los detalles del dispositivo, compruebe que el módulo de Blob Storage aparece como **Especificado en la implementación** y **Notificado por el dispositivo**. Puede tardar unos minutos para que el módulo se inicie en el dispositivo y, a continuación, notifique a IoT Hub. Actualice la página para ver el estado actualizado. 

### <a name="visual-studio-code-templates"></a>Plantillas de Visual Studio Code

Azure IoT Edge proporciona plantillas de Visual Studio Code que facilitan el desarrollo de soluciones de perímetro. Estos pasos requieren que [Visual Studio Code](https://code.visualstudio.com/) esté instalado en la máquina de desarrollo y configurado con las [herramientas de Azure IoT Edge](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools).

Siga estos pasos para crear una nueva solución de IoT Edge con un módulo de Blob Storage y configurar el manifiesto de implementación. 

1. Seleccione **Ver** > **Paleta de comandos**. 

2. En la paleta de comandos, escriba y ejecute el comando **Azure IoT Edge: New IoT Edge solution** (Nueva solución de IoT Edge). Para crear la solución, siga las indicaciones de la paleta de comandos.

   | Campo | Valor |
   | ----- | ----- |
   | Seleccionar carpeta | Elija la ubicación en el equipo de desarrollo en la que VS Code creará los archivos de la solución. |
   | Proporcionar un nombre de la solución | Escriba un nombre descriptivo para la solución o acepte el valor predeterminado **EdgeSolution**. |
   | Seleccionar plantilla del módulo | Elija **Existing Module (Enter full image URL)** (Módulo existente [escribir URL completa de la imagen]). |
   | Proporcionar un nombre de módulo | Escriba un nombre en minúsculas para el módulo, como **azureblobstorage**.<br><br>Es importante usar un nombre en minúsculas para el módulo de Azure Blob Storage en IoT Edge. IoT Edge distingue mayúsculas y minúsculas al hacer referencia a módulos y el SDK de Storage toma como valor predeterminado la minúsculas. |
   | Provide Docker image for the module (Proporcionar imagen de Docker para el módulo) | Proporcione el identificador URI: **mcr.microsoft.com/azure-blob-storage:latest** |

   VS Code toma la información que le ha proporcionado, crea una solución IoT Edge y la carga en una nueva ventana. La plantilla de solución crea una plantilla de manifiesto de implementación que incluye la imagen del módulo de Blob Storage, pero es preciso configurar las opciones de creación del módulo. 

3. Abra **deployment.template.json** en el área de trabajo de la solución nueva y busque la sección **modules**. Realice los cambios de configuración siguientes:

   1. Elimine el módulo **tempSensor**, ya que no es necesaria para esta implementación. 

   2. Copie y pegue el código siguiente en el campo **createOptions** del módulo de Blob Storage: 

      ```json
      "Env": [
        "LOCAL_STORAGE_ACCOUNT_NAME=$STORAGE_ACCOUNT_NAME","LOCAL_STORAGE_ACCOUNT_KEY=$STORAGE_ACCOUNT_KEY"
      ],
      "HostConfig":{
        "Binds": ["<storage directory bind>"],
        "PortBindings":{
          "11002/tcp": [{"HostPort":"11002"}]
        }
      }
      ```

      ![Actualización del módulo createOptions: VS Code](./media/how-to-store-data-blob/create-options.png)

4. En el JSON de las opciones de creación, actualice `<storage directory bind>` en función del sistema operativo del contenedor. Especifique el nombre de un [volumen](https://docs.docker.com/storage/volumes/) o la ruta de acceso absoluta a un directorio del dispositivo de IoT Edge en el que desea que el módulo del blob almacene sus datos. El enlace del directorio de almacenamiento asigna una ubicación del dispositivo proporcionada a una ubicación establecida del módulo.  

   * Contenedores de Linux: **\<ruta de acceso de almacenamiento >:/blobroot**. Por ejemplo, /srv/containerdata:/blobroot. O bien, my-volume:/blobroot.
   * Contenedores de Windows: **\<ruta de acceso de almacenamiento >: C:/BlobRoot**. Por ejemplo, C:/ContainerData:C:/BlobRoot. O bien, my-volume:C:/blobroot.
   
   > [!IMPORTANT]
   > No cambie la segunda mitad del valor del enlace del directorio de almacenamiento, que señala a una ubicación específica del módulo. El enlace del directorio de almacenamiento siempre debe terminar con **:/blobroot** para contenedores de Linux y **:C:/BlobRoot** para contenedores de Windows.

5. Guarde el archivo **deployment.template.json**.

6. Abra el archivo **.env** en el área de trabajo de la solución. 

7. El archivo .env está configurado para recibir las credenciales del registro de contenedor, pero no es necesario para la imagen de Blob Storage porque está públicamente disponible. En su lugar, reemplace el archivo por dos nuevas variables de entorno: 

   ```env
   STORAGE_ACCOUNT_NAME=
   STORAGE_ACCOUNT_KEY=
   ```

8. Proporcione un valor para `STORAGE_ACCOUNT_NAME`; los nombres de cuenta deben tener entre 3 y 24 caracteres, así como incluir números y letras minúsculas. Proporcione una clave base64 de 64 bytes para `STORAGE_ACCOUNT_KEY`. Puede generar una clave con herramientas como [GeneratePlus](https://generate.plus/en/base64?gp_base64_base[length]=64). Estas credenciales las usará para acceder a Blob Storage desde otros módulos. 

   No incluya espacios o comillas alrededor de los valores proporcionados. 

9. Guarde el archivo **.env**. 

10. Haga clic con el botón derecho en **deployment.template.json** y seleccione **Generar manifiesto de implementación de IoT Edge**. 

11. Visual Studio Code toma la información que especificó en deployment.template.json y .env, y la usa para crear un nuevo archivo de manifiesto de implementación. El manifiesto de implementación se crea en una carpeta **config** nueva en el área de trabajo de la solución. Cuando tenga el archivo, puede seguir los pasos de [Implementación de módulos de Azure IoT Edge desde Visual Studio Code](how-to-deploy-modules-vscode.md) o [Implementación de módulos de Azure IoT Edge con la CLI de Azure 2.0](how-to-deploy-modules-cli.md).

## <a name="connect-to-your-blob-storage-module"></a>Conexión a un módulo de Blob Storage

Puede usar el nombre y la clave de cuenta que configuró para el módulo para acceder al almacenamiento de blobs en el dispositivo de IoT Edge. 

Especifique el dispositivo de IoT Edge como el punto de conexión del blob para todas las solicitudes de almacenamiento que realice en él. Puede [crear una cadena de conexión para un punto de conexión de almacenamiento explícito](../storage/common/storage-configure-connection-string.md#create-a-connection-string-for-an-explicit-storage-endpoint) mediante la información del dispositivo de IoT Edge y el nombre de cuenta que configuró. 

1. Para los módulos que se implementan en el mismo dispositivo perimetral en que se está ejecutando "Azure Blob Storage en IoT Edge", el punto de conexión del blob es: `http://<module name>:11002/<account name>`. 
2. Para los módulos que se implementan en dispositivos perimetrales distintos al que está ejecutando "Azure Blob Storage en IoT Edge", de acuerdo con su configuración, el punto de conexión del blob es: `http://<device IP >:11002/<account name>`, `http://<IoT Edge device hostname>:11002/<account name>` o `http://<FQDN>:11002/<account name>`

## <a name="logs"></a>Registros

Puede encontrar los registros dentro del contenedor, en la ruta: 
* Para Linux: /blobroot/logs/platformblob.log

## <a name="deploy-multiple-instances"></a>Implementación de varias instancias

Si desea implementar varias instancias de Azure Blob Storage en IoT Edge, solo tiene que cambiar el puerto HostPort al que enlaza el módulo. Los módulos de Blob Storage siempre exponen el puerto 11002 del contenedor, pero se puede declarar el puerto al que está enlazado en el host. 

Edite las opciones de creación del módulo para cambiar el valor de HostPort:

```json
\"PortBindings\": {\"11002/tcp\": [{\"HostPort\":\"<port number>\"}]}
```

Cuando se conecta a módulos de Blob Storage adicionales, cambie el punto de conexión para que apunte al puerto de host actualizado. 

### <a name="try-it-out"></a>Prueba

La documentación de Azure Blob Storage incluye guías de inicio rápido que proporcionan ejemplo de código en varios idiomas. Puede ejecutar estos ejemplos para probar Azure Blob Storage en IoT Edge cambiando el punto de conexión del blob para que apunte a su módulo de Blob Storage.

Las siguientes guías de inicio rápido usan lenguajes que también son compatibles con IoT Edge, por lo que pueden implementarlas como módulos de IoT Edge, junto con el módulo de Blob Storage:

* [.NET](../storage/blobs/storage-quickstart-blobs-dotnet.md)
* [Java](../storage/blobs/storage-quickstart-blobs-java.md)
* [Python](../storage/blobs/storage-quickstart-blobs-python.md)
* [Node.js](../storage/blobs/storage-quickstart-blobs-nodejs.md)

## <a name="supported-storage-operations"></a>Operaciones de Blob Storage

Los módulos de Blob Storage en IoT Edge usan los mismos SDK de Azure Storage y son coherentes con la versión 2018-03-28 de la API de Azure Storage para los puntos de conexión del blob en bloques. Las versiones posteriores dependen de las necesidades del cliente. 

No todas las operaciones de Azure Blob Storage son compatibles con Azure Blob Storage en IoT Edge. En las siguientes secciones explican las operaciones que son compatibles, y las que no lo son. 

### <a name="account"></a>Cuenta

Compatible: 
* Enumerar contenedores

No compatible: 
* Obtener y definir las propiedades del servicio Blob
* Preparar solicitud de blob
* Obtener estadísticas del servicio Blob
* Obtención de información de la cuenta

### <a name="containers"></a>Contenedores

Compatible: 
* Crear y eliminar contenedor
* Obtener de propiedades y metadatos del contenedor
* Enumeración de blobs

No compatible: 
* Obtener y definir lista de control de acceso de contenedor
* Conceder contenedor
* Establecer metadatos de contenedor

### <a name="blobs"></a>Blobs

Compatible: 
* Poner, obtener y eliminar blob
* Obtener y definir propiedades de blob
* Obtener y definir metadatos de blob

No compatible: 
* Conceder blob
* Instantánea de blob
* Copiar y anular blob de copia
* Recuperar blob
* Establecer nivel de blob

### <a name="block-blobs"></a>Blobs en bloques

Compatible: 
* Colocar el bloque: el bloque debe tener un tamaño menor o igual que 4 MB
* Colocar y obtener lista de bloques

No compatible:
* Colocar bloque desde dirección URL

## <a name="next-steps"></a>Pasos siguientes

Más información acerca de [Azure Blob Storage](../storage/blobs/storage-blobs-introduction.md)

