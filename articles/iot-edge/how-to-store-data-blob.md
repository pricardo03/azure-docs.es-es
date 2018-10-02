---
title: Azure Blob Storage en dispositivos de Azure IoT Edge | Microsoft Docs
description: Implementación de un módulo de Azure Blob Storage en un dispositivo de IoT Edge para almacenar datos en el perímetro.
author: kgremban
manager: timlt
ms.author: kgremban
ms.reviewer: arduppal
ms.date: 09/20/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: b9e48eba4b46f024b056fe53b3b3df24feec802e
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "46995676"
---
# <a name="store-data-at-the-edge-with-azure-blob-storage-on-iot-edge-preview"></a>Almacene datos en el perímetro con Azure Blob Storage en IoT Edge (versión preliminar)

Azure Blob Storage en IoT Edge proporciona una solución de almacenamiento de blobs en bloques en el perímetro. En los dispositivos con IoT Edge, los módulos de almacenamiento de blobs se comportan servicios de blobs de Azure, pero los blobs en bloques se almacenan localmente en el dispositivo IoT Edge. Para acceder a los blobs puede usar los mismos métodos que el SDK de Azure Storage o las llamadas API del blob en bloques a las que está acostumbrado. 

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

Azure Blob Storage en IoT Edge proporciona tres imágenes de contenedor estándar: dos para los contenedores de Linux (arquitecturas AMD64 y ARM32) y otra para los contenedores de Windows (AMD64). Si se usa una de estas imágenes de módulo para implementar Blob Storage en un dispositivo de IoT Edge, se proporcionan tres datos para configurar la instancia de módulo para el dispositivo:

* Un **nombre de cuenta** y una **clave de cuenta**. Para mantener la coherencia con Azure Storage, los módulos de Blob Storage usan nombres de cuenta y claves de cuenta para administrar el acceso. Los nombres de cuenta deben tener entre 3 y 24 caracteres, e incluir números y letras minúsculas. Las claves de cuenta deben codificación base64 y una longitud de 64 bytes. Puede generar una clave con herramientas como [GeneratePlus](https://generate.plus/en/base64).
* Una **opción de almacenamiento local**. El módulo de Blob Storage almacena blobs localmente en el dispositivo de IoT Edge, para que los blobs no desaparezcan si el módulo se detiene o reinicia. Declare un [volumen](https://docs.docker.com/storage/volumes
) existente o una ruta de acceso a la carpeta local en la que deberían estar almacenados los blobs en el dispositivo. 

Hay varias maneras de implementar módulos en un dispositivo de IoT Edge, y todas ellas funcionan en los módulos de Azure Blob Storage en IoT Edge. Los dos métodos más sencillos son usar Azure Portal o las plantillas de Visual Studio Code. 

### <a name="azure-portal"></a>Azure Portal

Para implementar Blob Storage desde Azure Portal, siga los pasos que se describen en [Implementación de módulos de Azure IoT Edge desde Azure Portal](how-to-deploy-modules-portal.md). Antes de implementar el módulo, copie el identificador URI de la imagen y prepare las opciones de creación del contenedor en función del sistema operativo del contenedor. Use estos valores de la sección **Configuración de un manifiesto de implementación** del artículo anterior. 

Especifique el identificador URI del módulo de Blob Storage: **mcr.microsoft.com/azure-blob-storage**. 
   
Utilice la siguiente plantilla JSON para el campo **Opciones de creación del contenedor**. Configure el archivo JSON con el nombre y la clave de la cuenta de almacenamiento, y enlace del directorio de almacenamiento.  
   
   ```json
   {
       "Env":[
           "LOCAL_STORAGE_ACCOUNT_NAME=<your storage account name>",
           "LOCAL_STORAGE_ACCOUNT_KEY=<your storage account key>"
       ],
       "HostConfig":[
           "Binds":[
               "<storage directory bind>"
           ],
           "PortBindings":{
               "11002/tcp":[{"HostPort":"11002"}]
           }
       ]
   }
   ```   
   
En el JSON de las opciones de creación, actualice `\<your storage account name\>` con cualquier nombre. Actualice `\<your storage account key\>` con una clave de base64 de 64 bytes. Dicha clave se puede con herramientas como [GeneratePlus](https://generate.plus/en/base64), que permite seleccionar la longitud de bytes. Estas credenciales las usará para acceder a Blob Storage desde otros módulos.

En el JSON de las opciones de creación, actualice `<storage directory bind>` en función del sistema operativo del contenedor. Especifique el nombre de un [volumen](https://docs.docker.com/storage/volumes/) o la ruta de acceso absoluta a un directorio del dispositivo de IoT Edge en el que desea que el módulo del blob almacene sus datos.  

   * Contenedores de Linux: **\<ruta de acceso de almacenamiento >:/blobroot**. Por ejemplo, /srv/containerdata:/blobroot. O bien, my-volume:/blobroot. 
   * Contenedores de Windows: **\<ruta de acceso de almacenamiento >: C:/BlobRoot**. Por ejemplo, C:/ContainerData:C:/BlobRoot. O bien, my-volume:C:/blobroot. 


No es preciso especificar proporcionar las credenciales del registro para acceder a Azure Blob Storage en IoT Edge, ni tampoco lo es declarar ninguna ruta para la implementación. 

### <a name="visual-studio-code-templates"></a>Plantillas de Visual Studio Code

Azure IoT Edge proporciona plantillas de Visual Studio Code que facilitan el desarrollo de soluciones de perímetro. Estos pasos requieren que [Visual Studio Code](https://code.visualstudio.com/) esté instalado en el equipo de desarrollo y configurado con la [extensión de Azure IoT Edge](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge).

Siga estos pasos para crear una nueva solución de IoT Edge con un módulo de Blob Storage y configurar el manifiesto de implementación. 

1. Seleccione **Ver** > **Paleta de comandos**. 

2. En la paleta de comandos, escriba y ejecute el comando **Azure IoT Edge: New IoT Edge Solution** (Azure IoT Edge: nueva solución de IoT Edge). 

3. Siga las indicaciones para crear una solución: 

   1. **Seleccionar carpeta**: vaya a la carpeta en la que desea crear la solución.  
   
   2. **Especificar el nombre de la solución**: escriba un nombre para la solución o acepte el predeterminado.
   
   3. **Seleccionar plantilla de módulo**: elija **Existing Module (Enter full image URL)** [Módulo existente (escribir URL completa de la imagen)].
   
   4. **Especificar un nombre de módulo**: escriba un nombre reconocible para el módulo, como **azureBlobStorage**.
   
   5. **Especificar imagen de Docker para el módulo**: especifique el identificador URI de la imagen: **mcr.microsoft.com/azure-blob-storage**

VS Code toma la información que le ha proporcionado, crea una solución IoT Edge y la carga en una nueva ventana. 

La plantilla de solución crea una plantilla de manifiesto de implementación que incluye la imagen del módulo de Blob Storage, pero es preciso configurar las opciones de creación del módulo. 

1. Abra **deployment.template.json** en el área de trabajo de la solución nueva y busque la sección **modules**. 

2. Elimine el módulo **tempSensor**, ya que no es necesaria para esta implementación. 

3. Copie y pegue el código siguiente en el campo **createOptions** del módulo de Blob Storage: 

   ```json
   {\"Env\": [\"LOCAL_STORAGE_ACCOUNT_NAME=$STORAGE_ACCOUNT_NAME\",\" LOCAL_STORAGE_ACCOUNT_KEY=$STORAGE_ACCOUNT_KEY\"],\"HostConfig\": {\"Binds\": [\"<storage directory bind>\"],\"PortBindings\": {\"11002/tcp\": [{\"HostPort\":\"11002\"}]}}}
   ```

   ![Actualizar opciones de creación del módulo](./media/how-to-store-data-blob/create-options.png)

4. En el JSON de las opciones de creación, actualice `<storage directory bind>` en función del sistema operativo del contenedor. Especifique el nombre de un [volumen](https://docs.docker.com/storage/volumes/) o la ruta de acceso absoluta a un directorio del dispositivo de IoT Edge en el que desea que el módulo del blob almacene sus datos.  

   * Contenedores de Linux: **\<ruta de acceso de almacenamiento >:/blobroot**. Por ejemplo, /srv/containerdata:/blobroot. O bien, my-volume:/blobroot.
   * Contenedores de Windows: **\<ruta de acceso de almacenamiento >: C:/BlobRoot**. Por ejemplo, C:/ContainerData:C:/BlobRoot. O bien, my-volume:C:/blobroot.

5. Guarde **deployment.template.json**.

6. Abra **.env** en el área de trabajo de la solución. 

7. No es preciso que especifique ningún valor del registro de contenedor para la imagen de Blob Storage, ya que está públicamente disponible. En su lugar, agregue dos nuevas variables de entorno: 

   ```env
   STORAGE_ACCOUNT_NAME=
   STORAGE_ACCOUNT_KEY=
   ```

8. Especifique un nombre para el nombre de la cuenta de almacenamiento y una clave en base64 de 64 bytes para la clave de la cuenta de almacenamiento. Puede generar una clave con herramientas como [GeneratePlus](https://generate.plus/en/base64). Estas credenciales las usará para acceder a Blob Storage desde otros módulos. 

9. Guarde **.env**. 

10. Haga clic con el botón derecho en **deployment.template.json** y seleccione **Generar manifiesto de implementación de IoT Edge**. 

Visual Studio Code toma la información que especificó en deployment.template.json y .env, y la usa para crear un nuevo archivo de manifiesto de implementación. El manifiesto de implementación se crea en una carpeta **config** nueva en el área de trabajo de la solución. Cuando tenga el archivo, puede seguir los pasos de [Implementación de módulos de Azure IoT Edge desde Visual Studio Code](how-to-deploy-modules-vscode.md) o [Implementación de módulos de Azure IoT Edge con la CLI de Azure 2.0](how-to-deploy-modules-cli.md).

## <a name="connect-to-your-blob-storage-module"></a>Conexión a un módulo de Blob Storage

Puede usar el nombre y la clave de cuenta que configuró para el módulo para acceder al almacenamiento de blobs en el dispositivo de IoT Edge. 

Especifique el dispositivo de IoT Edge como el punto de conexión del blob para todas las solicitudes de almacenamiento que realice en él. Puede [crear una cadena de conexión para un punto de conexión de almacenamiento explícito](../storage/common/storage-configure-connection-string.md#create-a-connection-string-for-an-explicit-storage-endpoint) mediante la información del dispositivo de IoT Edge y el nombre de cuenta que configuró. 

El punto de conexión de blobs de Azure Blob Storage en IoT Edge es `http://<IoT Edge device hostname>:11002/<account name>`. 

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
* Colocar bloque
* Colocar y obtener lista de bloques

No compatible:
* Colocar bloque desde dirección URL

## <a name="next-steps"></a>Pasos siguientes

Más información acerca de [Azure Blob Storage](../storage/blobs/storage-blobs-introduction.md)

