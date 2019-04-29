---
title: 'Almacenamiento de blob en bloques en los dispositivos: Azure IoT Edge | Microsoft Docs'
description: Implementación de un módulo de Azure Blob Storage en un dispositivo de IoT Edge para almacenar datos en el perímetro.
author: kgremban
manager: philmea
ms.author: kgremban
ms.reviewer: arduppal
ms.date: 03/07/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: 3a0df408e70ed61355ffba319f6261f90d8e4348
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60595586"
---
# <a name="store-data-at-the-edge-with-azure-blob-storage-on-iot-edge-preview"></a>Almacene datos en el perímetro con Azure Blob Storage en IoT Edge (versión preliminar)

Azure Blob Storage en IoT Edge proporciona una solución de almacenamiento de [blobs en bloques](https://docs.microsoft.com/rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs#about-block-blobs) en el perímetro. En los dispositivos con IoT Edge, los módulos de almacenamiento de blobs se comportan como un servicio de blobs en bloques de Azure, pero los blobs en bloques se almacenan localmente en el dispositivo IoT Edge. Para acceder a los blobs puede usar los mismos métodos que el SDK de Azure Storage o las llamadas API del blob en bloques a las que está acostumbrado. 

Este módulo incluye **los niveles de Auto** y **Auto caducidad** características.

> [!NOTE]
> Actualmente solo están disponibles en Linux AMD64 y Linux ARM32 funcionalidad auto caducidad y las capas de automático.

**Los niveles de Auto** es una funcionalidad configurable, que le permite cargar automáticamente los datos de su almacenamiento de blobs local a Azure con compatibilidad con conectividad de internet intermitentes. Le permite:
- Encendido de activar la característica de niveles
- Elija el orden en que aprenderá a los datos se copia en Azure como NewestFirst o OldestFirst
- Especifique la cuenta de Azure Storage al que desea que los datos cargados.
- Especifique el contenedor que desee cargar en Azure. Este módulo le permite especificar los nombres de contenedor de origen y destino.
- Total de los niveles de blob (mediante `Put Blob` operación) y bloquear el nivel de organización en niveles (mediante `Put Block` y `Put Block List` operaciones).

Este módulo utiliza el bloque niveles, cuando el blob se compone de bloques. Estos son algunos de los escenarios comunes:
- La aplicación actualiza algunos bloques de un blob cargado anteriormente, este módulo cargue únicamente los bloques actualizados y no la totalidad del blob.
- El módulo carga el blob y conexión a internet desaparece, cuando la conectividad es volver a intentarlo cargará únicamente los bloques restantes y no la totalidad del blob.

Si se produce una finalización inesperada de proceso (por ejemplo, un corte de suministro eléctrico) durante una carga de blobs, todos los bloques que vencieron para la carga se cargará de nuevo, cuando vuelve a conectar el módulo.

**Auto caducidad** es una funcionalidad configurable en este módulo elimina automáticamente los blobs desde el almacenamiento local cuando expira el período de vida (TTL). Se mide en minutos. Le permite:
- Encendido de activar la característica de auto caducidad
- Especifique el valor de TTL en minutos

Escenarios donde los datos, como vídeos, imágenes, datos financieros, datos de hospital o los datos que tengan que almacenarse localmente, más adelante que podían procesarse de forma local o transferidos a la nube son buenos ejemplos para usar este módulo.

En este artículo se proporcionan instrucciones para la implementación de Azure Blob Storage en un contenedor de IoT Edge en el que se ejecuta un servicio de blobs en un dispositivo de IoT Edge. 

>[!NOTE]
>Azure Blob Storage en IoT Edge está en [versión preliminar pública](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). 

Ver el vídeo de introducción rápida
> [!VIDEO https://www.youtube.com/embed/wkprcfVidyM]

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

   4. Establecer [auto caducidad y las capas de auto](#configure-auto-tiering-and-auto-expiration-via-azure-portal) en las propiedades deseadas. Lista de [los niveles de auto](#auto-tiering-properties) y [auto caducidad](#auto-expiration-properties) propiedades y sus valores posibles. 

   5. Seleccione **Guardar**. 

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

5. Configurar [auto caducidad y las capas de auto](#configure-auto-tiering-and-auto-expiration-via-vscode). Lista de [los niveles de auto](#auto-tiering-properties) y [auto caducidad](#auto-expiration-properties) propiedades

6. Guarde el archivo **deployment.template.json**.

7. Abra el archivo **.env** en el área de trabajo de la solución. 

8. El archivo .env está configurado para recibir las credenciales del registro de contenedor, pero no es necesario para la imagen de Blob Storage porque está públicamente disponible. En su lugar, reemplace el archivo por dos nuevas variables de entorno: 

   ```env
   STORAGE_ACCOUNT_NAME=
   STORAGE_ACCOUNT_KEY=
   ```

9. Proporcione un valor para `STORAGE_ACCOUNT_NAME`; los nombres de cuenta deben tener entre 3 y 24 caracteres, así como incluir números y letras minúsculas. Proporcione una clave base64 de 64 bytes para `STORAGE_ACCOUNT_KEY`. Puede generar una clave con herramientas como [GeneratePlus](https://generate.plus/en/base64?gp_base64_base[length]=64). Estas credenciales las usará para acceder a Blob Storage desde otros módulos. 

   No incluya espacios o comillas alrededor de los valores proporcionados. 

10. Guarde el archivo **.env**. 

11. Haga clic con el botón derecho en **deployment.template.json** y seleccione **Generar manifiesto de implementación de IoT Edge**. 

12. Visual Studio Code toma la información que especificó en deployment.template.json y .env, y la usa para crear un nuevo archivo de manifiesto de implementación. El manifiesto de implementación se crea en una carpeta **config** nueva en el área de trabajo de la solución. Cuando tenga el archivo, puede seguir los pasos de [Implementación de módulos de Azure IoT Edge desde Visual Studio Code](how-to-deploy-modules-vscode.md) o [Implementación de módulos de Azure IoT Edge con la CLI de Azure 2.0](how-to-deploy-modules-cli.md).

## <a name="auto-tiering-and-auto-expiration-properties-and-configuration"></a>Los niveles de Auto y Auto caducidad Propiedades y configuración

Use las propiedades deseadas para establecer propiedades de auto caducidad y las capas de automático. Puede establecer durante la implementación o cambiar más adelante mediante la edición del módulo gemelo sin necesidad de volver a implementar. Se recomienda comprobar el "módulo gemelo" para `reported configuration` y `configurationValidation` para asegurarse de que los valores se propagan correctamente.

### <a name="auto-tiering-properties"></a>Propiedades de la organización en niveles automáticamente 
El nombre de esta configuración es `tieringSettings`

| Campo | Valores posibles | Explicación |
| ----- | ----- | ---- |
| tieringOn | true, false | De forma predeterminada se establece en `false`, si desea convertirlo en establézcalo en `true`|
| backlogPolicy | NewestFirst, OldestFirst | Le permite elegir el orden en que aprenderá a los datos se copia en Azure. De forma predeterminada se establece en `OldestFirst`. El orden viene determinada por la hora última modificación del Blob |
| remoteStorageConnectionString |  | `"DefaultEndpointsProtocol=https;AccountName=<your Azure Storage Account Name>;AccountKey=<your Azure Storage Account Key>;EndpointSuffix=<your end point suffix>"` ¿se carga una cadena de conexión que le permite especificar la cuenta de almacenamiento de Azure a la que desea que los datos. Especificar `Azure Storage Account Name`, `Azure Storage Account Key`, `End point suffix`. Agregar adecuado EndpointSuffix de Azure donde se cargarán los datos, varía para Global Azure, Azure Government y Microsoft Azure Stack. |
| tieredContainers | `"<source container name1>": {"target": "<target container name>"}`,<br><br> `"<source container name1>": {"target": "%h-%d-%m-%c"}`, <br><br> `"<source container name1>": {"target": "%d-%c"}` | Le permite especificar los nombres de contenedor que desea cargar en Azure. Este módulo le permite especificar los nombres de contenedor de origen y destino. Si no especifica el nombre del contenedor de destino, asignará automáticamente el nombre del contenedor como `<IoTHubName>-<IotEdgeDeviceName>-<ModuleName>-<ContainerName>`. Puede crear cadenas de plantillas para el nombre del contenedor de destino, la columna de valores posibles de desprotección. <br>* %h -> nombre de IoT Hub (entre 3 y 50 caracteres). <br>* %d -> identificador (1 a 129 caracteres) de dispositivo de IoT. <br>* %m -> nombre del módulo (1 a 64 caracteres). <br>* %c -> nombre del contenedor de origen (3 y 63 caracteres). <br><br>Tamaño máximo del nombre del contenedor es de 63 caracteres, al asignar automáticamente el nombre del contenedor de destino si el tamaño del contenedor es superior a 63 caracteres, que eliminará cada sección (IoTHubName, IotEdgeDeviceName, ModuleName, ContainerName) y 15 caracteres. |

### <a name="auto-expiration-properties"></a>Propiedades de Auto caducidad
El nombre de esta configuración es `ttlSettings`

| Campo | Valores posibles | Explicación |
| ----- | ----- | ---- |
| ttlOn | true, false | De forma predeterminada se establece en `false`, si desea convertirlo en establézcalo en `true`|
| timeToLiveInMinutes | `<minutes>` | Especifique el valor de TTL en minutos. El módulo eliminará automáticamente los blobs de almacenamiento local cuando expira el período de vida |

### <a name="configure-auto-tiering-and-auto-expiration-via-azure-portal"></a>Configurar expiración automática y las capas de automática a través del portal de Azure

Establezca las propiedades deseadas para habilitar los niveles de auto y expiración automática, puede establecer estos valores:

- **Durante la implementación inicial**: Copie el archivo JSON en **propiedades deseadas del gemelo de módulo de conjunto** cuadro. Configure cada propiedad con el valor adecuado, guárdelo y continuar con la implementación.

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

  ![establecer las propiedades de expiración automática y las capas de automático](./media/how-to-store-data-blob/iotedge_custom_module.png)

- **Después de que el módulo se implementa a través de la característica de "Identidad de módulo gemelo"**: Vaya a "Módulo gemelo de identidad" de este módulo, copie el archivo JSON en las propiedades deseadas, configure cada propiedad con el valor adecuado y guardar. En Json "Identidad de módulo gemelo" Asegúrese de que cada vez que agrega o actualiza cualquier propiedad, de deseada del `reported configuration` sección refleja los cambios y el `configurationValidation` sección informa de éxito para cada propiedad.

   ```json 
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

   ```

![niveles + ttl module_identity_twin](./media/how-to-store-data-blob/module_identity_twin.png) 

### <a name="configure-auto-tiering-and-auto-expiration-via-vscode"></a>Configurar la expiración automática a través de VSCode y las capas de automático

- **Durante la implementación inicial**: Agregue el siguiente código JSON en su deployment.template.json para definir las propiedades deseadas para este módulo. Configure cada propiedad con el valor adecuado y guárdelo.

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

Este es un ejemplo de las propiedades deseadas para este módulo: ![establecer propiedades deseadas para azureblobstorageoniotedge - VS Code](./media/how-to-store-data-blob/tiering_ttl.png)

- **Una vez implementado el módulo a través "Módulo gemelo"**: [Editar el módulo gemelo](https://github.com/Microsoft/vscode-azure-iot-toolkit/wiki/Edit-Module-Twin) de este módulo, copie el archivo JSON en las propiedades deseadas, configure cada propiedad con el valor adecuado y guardar. En Json "Módulo gemelo" Asegúrese de que cada vez que agrega o actualiza cualquier propiedad, de deseada del `reported configuration` sección refleja los cambios y el `configurationValidation` sección informa de éxito para cada propiedad.

   ```json 
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

   ```
## <a name="logs"></a>Registros

Siga las instrucciones para [configurar los registros de docker para módulos de IoT Edge](production-checklist.md#set-up-logs-and-diagnostics)

## <a name="connect-to-your-blob-storage-module"></a>Conexión a un módulo de Blob Storage

Puede usar el nombre y la clave de cuenta que configuró para el módulo para acceder al almacenamiento de blobs en el dispositivo de IoT Edge. 

Especifique el dispositivo de IoT Edge como el punto de conexión del blob para todas las solicitudes de almacenamiento que realice en él. Puede [crear una cadena de conexión para un punto de conexión de almacenamiento explícito](../storage/common/storage-configure-connection-string.md#create-a-connection-string-for-an-explicit-storage-endpoint) mediante la información del dispositivo de IoT Edge y el nombre de cuenta que configuró. 

1. Para los módulos, que se implementan en el mismo dispositivo de edge que se está ejecutando "Azure Blob Storage en IoT Edge", es el punto de conexión de blob: `http://<module name>:11002/<account name>`. 
2. Para los módulos, que se implementan en el dispositivo de edge diferentes, que el dispositivo perimetral, donde se está ejecutando "Azure Blob Storage en IoT Edge", según la configuración de extremo del blob es: `http://<device IP >:11002/<account name>` o `http://<IoT Edge device hostname>:11002/<account name>` o `http://<FQDN>:11002/<account name>`

## <a name="deploy-multiple-instances"></a>Implementación de varias instancias

Si desea implementar varias instancias de Azure Blob Storage en IoT Edge, deberá proporcionar la ruta de acceso de almacenamiento diferente y cambiar el HostPort que se enlaza el módulo. Los módulos de Blob Storage siempre exponen el puerto 11002 del contenedor, pero se puede declarar el puerto al que está enlazado en el host. 

Edite las opciones de creación del módulo para cambiar el valor de HostPort:

```json
\"PortBindings\": {\"11002/tcp\": [{\"HostPort\":\"<port number>\"}]}
```

Cuando se conecta a módulos de Blob Storage adicionales, cambie el punto de conexión para que apunte al puerto de host actualizado. 

## <a name="try-it-out"></a>Prueba

### <a name="azure-blob-storage-quickstart-samples"></a>Ejemplos de inicio rápido de almacenamiento de blobs de Azure
La documentación de Azure Blob Storage incluye guías de inicio rápido que proporcionan ejemplo de código en varios idiomas. Puede ejecutar estos ejemplos para probar Azure Blob Storage en IoT Edge cambiando el punto de conexión del blob para que apunte a su módulo de Blob Storage. Siga los pasos para [conectarse a su módulo de almacenamiento de blobs](#connect-to-your-blob-storage-module)

Las siguientes guías de inicio rápido usan lenguajes que también son compatibles con IoT Edge, por lo que pueden implementarlas como módulos de IoT Edge, junto con el módulo de Blob Storage:

* [.NET](../storage/blobs/storage-quickstart-blobs-dotnet.md)
* [Java](../storage/blobs/storage-quickstart-blobs-java.md)
* [Python](../storage/blobs/storage-quickstart-blobs-python.md)
* [Node.js](../storage/blobs/storage-quickstart-blobs-nodejs.md) 

### <a name="azure-storage-explorer"></a>Explorador de Azure Storage
También puede intentar "Explorador de Azure Storage" para conectarse a su cuenta de almacenamiento local. Funciona con [Explorador de Azure Storage versión 1.5.0](https://github.com/Microsoft/AzureStorageExplorer/releases/tag/v1.5.0).

> [!NOTE]
> Pueden producirse errores al realizar los pasos siguientes, como agregar una conexión a una cuenta de almacenamiento local, o la creación de contenedores en la cuenta de almacenamiento local. Omítalo y actualizar. 

1. Descargue e instale el Explorador de Azure Storage
2. Conectarse a Azure Storage mediante una cadena de conexión
3. Proporcionar la cadena de conexión: `DefaultEndpointsProtocol=http;BlobEndpoint=http://<host device name>:11002/<your local account name>;AccountName=<your local account name>;AccountKey=<your local account key>;`
4. Siga los pasos para conectarse.
5. Crear contenedor dentro de su cuenta de almacenamiento local
6. Iniciar la carga de archivos como blobs en bloques.
   > [!NOTE]
   > Desactive la casilla de verificación para cargarlo como blobs en páginas. Este módulo no es compatible con blobs en páginas. Obtendrá este símbolo del sistema al cargar los archivos como .iso, .vhd, .vhdx o los archivos de gran tamaño.

7. Puede elegir conectarse a cuentas de Azure storage donde va a cargar los datos. Proporciona una vista única para la cuenta de almacenamiento local y la cuenta de almacenamiento de Azure

## <a name="supported-storage-operations"></a>Operaciones de Blob Storage

Los módulos de almacenamiento de blobs en IoT Edge usan el mismo SDK de almacenamiento de Azure y son coherentes con la versión 2017-04-17 de la API de Azure Storage para puntos de conexión de blob de bloque. Las versiones posteriores dependen de las necesidades del cliente.

No todas las operaciones de Azure Blob Storage son compatibles con Azure Blob Storage en IoT Edge. En la siguiente sección se enumera las operaciones admitidas y no admitidas.

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
* Obtener y definir lista de control de acceso de contenedor
* Establecer metadatos de contenedor

No compatible:
* Conceder contenedor

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
* Colocar bloque
* Colocar y obtener lista de bloques

No compatible:
* Colocar bloque desde dirección URL

## <a name="feedback"></a>Comentarios:
Sus comentarios son muy importantes para nosotros, para que este módulo y sus características, útil y fácil de usar. Comparta sus comentarios y háganos saber cómo podemos mejorar.

Puede ponerse en contacto con nosotros en absiotfeedback@microsoft.com 

## <a name="next-steps"></a>Pasos siguientes

Más información acerca de [Azure Blob Storage](../storage/blobs/storage-blobs-introduction.md)

