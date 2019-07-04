---
author: kgremban
ms.service: iot-edge
ms.topic: include
ms.date: 04/25/2019
ms.author: kgremban
ms.openlocfilehash: 485a76cb91e5146f59e6be592ffd9cbba68e585a
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/18/2019
ms.locfileid: "67186432"
---
## <a name="create-a-container-registry"></a>Creación de un Registro de contenedor

En este tutorial se usa la extensión de Azure IoT Tools para fin de compilar un módulo y crear una **imagen de contenedor** a partir de los archivos. Después, insertará esta imagen en un **Registro** donde se almacenan y administran las imágenes. Por último, implementará la imagen en el Registro para que se ejecute en el dispositivo IoT Edge.

Puede usar cualquier registro compatible con Docker para almacenar las imágenes de contenedor. Dos de los servicios de registro de Docker más conocidos son [Azure Container Registry](https://docs.microsoft.com/azure/container-registry/) y [Docker Hub](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags). En este tutorial, utilizaremos Azure Container Registry.

Si no dispone de un registro de contenedores, siga estos pasos para crear uno nuevo en Azure:

1. En [Azure Portal](https://portal.azure.com), seleccione **Crear un recurso** > **Contenedores** > **Container Registry**.

2. Especifique los siguientes valores para crear un registro de contenedor:

   | Campo | Valor |
   | ----- | ----- |
   | Nombre de registro | Especifique un nombre único. |
   | Subscription | Seleccione una suscripción en la lista desplegable. |
   | Grupos de recursos | Se recomienda usar el mismo grupo de recursos para todos los recursos de prueba que se crean en las guías de inicio rápido y los tutoriales de IoT Edge. Por ejemplo, **IoTEdgeResources**. |
   | Ubicación | Elija una ubicación cercana a usted. |
   | Usuario administrador | Seleccione **Habilitar**. |
   | SKU | Seleccione **Básica**. |

5. Seleccione **Crear**.

6. Una vez que se haya creado el Registro de contenedor, desplácese hasta él y seleccione **Claves de acceso**.

7. Copie los valores de **Servidor de inicio de sesión**, **Nombre de usuario** y **Contraseña**, y guárdelos en cualquier lugar que le resulte práctico. Dichos valores se usan en todo el tutorial para proporcionar acceso al registro de contenedor.

   ![Copiar el servidor de inicio de sesión, nombre de usuario y contraseña del registro de contenedor](./media/iot-edge-create-container-registry/registry-access-key.png)