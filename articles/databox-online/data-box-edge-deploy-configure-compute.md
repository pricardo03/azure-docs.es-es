---
title: Transformación de datos con Azure Data Box Edge | Microsoft Docs
description: Aprenda a configurar el rol de proceso en Data Box Edge y a usarlo para transformar los datos antes de enviarlos a Azure.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: tutorial
ms.date: 11/27/2018
ms.author: alkohli
Customer intent: As an IT admin, I need to understand how to configure compute on Data Box Edge so I can use it to transform the data before sending it to Azure.
ms.openlocfilehash: c52c311f1e1cd1335ea5797eadacd0bc89e1b36c
ms.sourcegitcommit: c31a2dd686ea1b0824e7e695157adbc219d9074f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/18/2019
ms.locfileid: "54402122"
---
# <a name="tutorial-transform-data-with-azure-data-box-edge-preview"></a>Tutorial: Transformación de datos con Azure Data Box Edge (versión preliminar)

Este tutorial describe cómo configurar el rol de proceso en un dispositivo de Azure Data Box Edge. Una vez configurado el rol de proceso, Azure Data Box Edge puede transformar los datos antes de enviarlos a Azure.

Este procedimiento tarda aproximadamente entre 30 y 45 minutos en completarse.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Crear un recurso de Azure IoT Hub
> * Configurar un rol de proceso
> * Agregar un módulo de proceso
> * Comprobar la transformación y la transferencia de los datos

> [!IMPORTANT]
> Data Box Edge se encuentra en versión preliminar. Antes de solicitar e implementar esta solución, revise los [términos del servicio de Azure para la versión preliminar](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
 
## <a name="prerequisites"></a>Requisitos previos

Antes de configurar un rol de proceso en un dispositivo de Data Box Edge, asegúrese de que:

* Ha activado el dispositivo físico de Data Box Edge tal como se describe en [Conexión, configuración y activación de Azure Data Box Edge](data-box-edge-deploy-connect-setup-activate.md).


## <a name="create-an-iot-hub-resource"></a>Crear un recurso de IoT Hub

Antes de configurar un rol de proceso en Data Box Edge, debe crear un recurso de IoT Hub.

Para obtener instrucciones detalladas, vaya a [Crear un centro de IoT](https://docs.microsoft.com/azure/iot-hub/iot-hub-create-through-portal#create-an-iot-hub). Use la misma suscripción y grupo de recursos que para el recurso de Data Box Edge.

![Crear un recurso de IoT Hub](./media/data-box-edge-deploy-configure-compute/create-iothub-resource-1.png)

Si no se ha configurado un rol de proceso de Edge, se aplican las siguientes observaciones:

- El recurso de IoT Hub no tiene dispositivos de Azure IoT ni de Azure IoT Edge.
- No puede crear recursos compartidos locales de Edge. Cuando agrega un recurso compartido, no está habilitada la opción para crear un recurso compartido local para el proceso de Edge.


## <a name="set-up-compute-role"></a>Configurar un rol de proceso

Cuando el rol de proceso de Edge está configurado en el dispositivo de Edge, este crea dos dispositivos: uno IoT y el otro IoT Edge. Ambos se pueden ver en el recurso de IoT Hub.

Para configurar el rol de proceso en el dispositivo, haga lo siguiente:

1. Vaya al recurso de Data Box Edge y, a continuación, vaya a **Introducción** y seleccione **Configurar rol de proceso**. 

    ![Vínculo Información general del panel izquierdo](./media/data-box-edge-deploy-configure-compute/setup-compute-1.png)
   
    Si lo desea, puede ir a **Módulos** y seleccionar **Configurar proceso**.

    ![Vínculos de "Módulos" y "Configurar proceso"](./media/data-box-edge-deploy-configure-compute/setup-compute-2.png)
 
1. En la lista desplegable, seleccione el **recurso de IoT Hub** que creó en el paso anterior.  
    En este momento, solo está disponible la plataforma Linux para el dispositivo IoT Edge. 
    
1. Haga clic en **Create**(Crear).

    ![El botón Crear](./media/data-box-edge-deploy-configure-compute/setup-compute-3.png)
 
    El rol de proceso tarda un par de minutos en crearse. Debido a un error en esta versión, no se actualiza la pantalla aunque se haya configurado el rol de proceso. Para confirmar que se ha configurado el rol de proceso de Edge, vaya a **Módulos**.  

    ![La lista de dispositivos "Configurar proceso de Edge"](./media/data-box-edge-deploy-configure-compute/setup-compute-4.png)

1. Vaya a **Información general** de nuevo.  
    La pantalla se actualiza para indicar que se ha configurado el rol de proceso.

    ![Configuración de un rol de proceso](./media/data-box-edge-deploy-configure-compute/setup-compute-5.png)
 
1. En el centro IoT Hub que usó cuando creó el rol de proceso de Edge, vaya a **Dispositivos IoT**.  
    Ahora hay un dispositivo IoT habilitado. 

    ![La página "Dispositivos IoT"](./media/data-box-edge-deploy-configure-compute/setup-compute-6.png)

1. En el panel izquierdo, seleccione **IoT Edge**.  
    También se habilita un dispositivo IoT Edge.

    ![Configuración de un rol de proceso](./media/data-box-edge-deploy-configure-compute/setup-compute-7.png)
 
1. Seleccione y haga clic en el dispositivo de IoT Edge.  
    Un agente de Edge se está ejecutando en este dispositivo de IoT Edge. 

    ![Página Detalles del dispositivo](./media/data-box-edge-deploy-configure-compute/setup-compute-8.png) 

    No hay ningún módulo personalizado en este dispositivo de Edge, pero ahora puede agregar uno. Para información sobre cómo crear un módulo personalizado, vaya a [Desarrollo de un módulo C# para un dispositivo de Data Box Edge](data-box-edge-create-iot-edge-module.md).


## <a name="add-a-custom-module"></a>Incorporación de un módulo personalizado

En esta sección, agregará un módulo personalizado al dispositivo IoT Edge que creó en [Desarrollo de un módulo C# para Data Box Edge](data-box-edge-create-iot-edge-module.md). 

El procedimiento siguiente usa un ejemplo en el que el módulo personalizado toma archivos de un recurso compartido local del dispositivo de Edge y los mueve a un recurso compartido en la nube en el dispositivo. A continuación, el recurso compartido en la nube inserta los archivos en la cuenta de Azure Storage asociada con este. 

1. Agregue un recurso compartido local en el dispositivo de Edge. Para ello, haga lo siguiente:

     a. En el recurso de Data Box Edge, vaya a **Recursos compartidos**. 
    
    b. Seleccione **Agregar recurso compartido** y, después, proporcione su nombre y seleccione su tipo. 
    
    c. Para crear un recurso compartido local, seleccione la casilla **Configurar como recurso compartido local de Edge**. 
    
    d. Seleccione **Crear nuevo** o **Usar existente**, y, a continuación, seleccione **Crear**.

    ![Agregar un módulo personalizado](./media/data-box-edge-deploy-configure-compute/add-a-custom-module-1.png) 

    Si ha creado un recurso compartido NFS local, use la siguiente opción de comando de sincronización remota (rsync) para copiar archivos en él:

    `rsync --inplace <source file path> < destination file path>`

    Para más información sobre el comando rsync, consulte la [documentación sobre rsync](https://www.computerhope.com/unix/rsync.htm). 

    Se crea el recurso compartido local y recibirá una notificación para indicar que la creación se produjo correctamente. Puede que la lista de recursos compartidos se actualice, pero debe esperar a que la creación del recurso compartido se complete.
    
1. Vaya a la lista de recursos compartidos. 

    ![Agregar un módulo personalizado](./media/data-box-edge-deploy-configure-compute/add-a-custom-module-2.png) 
 
1. Para ver las propiedades del recurso compartido local recién creado, selecciónelo. 

1. En el cuadro **Punto de montaje local para módulos de proceso de Edge**, copie el valor correspondiente a este recurso compartido.  
    Usará este punto de montaje local al implementar el módulo.

    ![El cuadro "Punto de montaje local para módulos del proceso de Edge"](./media/data-box-edge-deploy-configure-compute/add-a-custom-module-3.png) 
 
1. En un recurso compartido en la nube existente que se creó en el dispositivo de Data Box Edge, en el cuadro **Punto de montaje local para módulos de proceso de Edge**, copie el punto de montaje local para los módulos de proceso de Edge correspondientes a este recurso compartido en la nube.  
    Usará este punto de montaje local al implementar el módulo.

    ![Agregar un módulo personalizado](./media/data-box-edge-deploy-configure-compute/add-a-custom-module-4.png)  

1. Para agregar un módulo personalizado al dispositivo IoT Edge, vaya al recurso IoT Hub y, a continuación, vaya al **dispositivo IoT Edge**. 

1. Seleccione el dispositivo y, a continuación, en **Detalles del dispositivo**, seleccione **Establecer módulos**. 

    ![El vínculo Establecer módulos](./media/data-box-edge-deploy-configure-compute/add-a-custom-module-5.png) 

1. En **Agregar módulos**, haga lo siguiente:

     a. Proporcione el nombre, dirección, nombre de usuario y contraseña de la configuración del Registro del contenedor para el módulo personalizado.  
    El nombre, la dirección y las credenciales mostradas se utilizan para recuperar aquellos módulos con una dirección URL que coincida. Para implementar este módulo, en **Módulos de implementación**, seleccione **Módulo de IoT Edge**. Este módulo de IoT Edge es un contenedor de Docker que puede implementar en un dispositivo IoT Edge asociado con el dispositivo de Data Box Edge.

    ![La página Establecer módulos](./media/data-box-edge-deploy-configure-compute/add-a-custom-module-6.png) 
 
    b. Especifique la configuración para el módulo personalizado de IoT Edge; para ello, escriba el nombre de su módulo y el URI de imagen para la imagen de contenedor correspondiente. 
    
    ![La página Módulos personalizados de IoT Edge](./media/data-box-edge-deploy-configure-compute/add-a-custom-module-7.png) 

    c. En el cuadro **Opciones de creación del contenedor**, escriba los puntos de montaje local para los módulos de Edge que ha copiado en los pasos anteriores para la nube y el recurso compartido local.
    > [!IMPORTANT]
    > Utilice las rutas de acceso copiadas; no cree rutas nuevas. Los puntos de montaje locales se asignan a las rutas **InputFolderPath** y **OutputFolderPath** correspondientes que especificó en el módulo cuando [actualizó el módulo con código personalizado](data-box-edge-create-iot-edge-module.md#update-the-module-with-custom-code). 
    
    En el cuadro **Opciones de creación del contenedor**, puede pegar el ejemplo siguiente: 
    
    ```
    {
        "HostConfig": {
        "Binds": [
        "/home/hcsshares/mysmblocalshare:/home/LocalShare",
        "/home/hcsshares/mysmbshare1:/home/CloudShare"
        ]
        }
    }
    ```

    También se proporcionan aquí todas las variables de entorno del módulo.

    ![El cuadro Opciones de creación del contenedor](./media/data-box-edge-deploy-configure-compute/add-a-custom-module-8.png) 
 
    d. Si es necesario, configure las opciones avanzadas del entorno de ejecución de Edge y, a continuación, haga clic en **Siguiente**.

    ![Agregar un módulo personalizado](./media/data-box-edge-deploy-configure-compute/add-a-custom-module-9.png) 
 
1.  En **Especificar rutas**, defina las rutas entre módulos.  
    En este ejemplo, proporcione el nombre del recurso compartido local que insertará los datos en el recurso compartido en la nube.

    Puede reemplazar la *ruta* por la cadena de ruta siguiente:       `"route": "FROM /* WHERE topic = 'mysmblocalshare' INTO BrokeredEndpoint(\"/modules/filemovemodule/inputs/input1\")"`

    ![La sección "Especificar rutas"](./media/data-box-edge-deploy-configure-compute/add-a-custom-module-10.png) 

1. Seleccione **Next** (Siguiente). 

1.  En **Revisar la implementación**, revise todos los valores y, después, seleccione **Enviar** a fin de enviar el módulo para la implementación.

    ![La página Establecer módulos](./media/data-box-edge-deploy-configure-compute/add-a-custom-module-11.png) 
 
    De este modo, se inicia la implementación del módulo tal como se muestra en la siguiente imagen:

    ![Agregar un módulo personalizado](./media/data-box-edge-deploy-configure-compute/add-a-custom-module-12.png) 

### <a name="verify-data-transform-and-transfer"></a>Comprobar la transformación y la transferencia de los datos

El último paso es asegurarse de que el módulo está conectado y funciona según lo previsto. El estado del entorno de ejecución del módulo debe ser "en ejecución" para el dispositivo IoT Edge del recurso de IoT Hub.

![Comprobar transformación de datos](./media/data-box-edge-deploy-configure-compute/verify-data-transform-1.png) 
 
Para comprobar que el módulo se está ejecutando, haga lo siguiente:

1. Seleccione el módulo y, después, examine la Identidad de módulo gemela.  
    El estado del cliente para el dispositivo y el módulo de Edge debe aparecer como *Conectado*.

    ![Comprobar transformación de datos](./media/data-box-edge-deploy-configure-compute/verify-data-transform-2.png) 
 
    Una vez que el módulo está en ejecución, también aparece en la lista de módulos de Edge en el recurso de Data Box Edge. El estado del entorno de ejecución del módulo que se agrega es *En ejecución*.

    ![Comprobar transformación de datos](./media/data-box-edge-deploy-configure-compute/verify-data-transform-3.png) 
 
1.  En el Explorador de archivos, conecte tanto con el recurso compartido local como con el de la nube que creó antes.

    ![Comprobar transformación de datos](./media/data-box-edge-deploy-configure-compute/verify-data-transform-4.png) 
 
1.  Agregue datos al recurso compartido local.

    ![Comprobar transformación de datos](./media/data-box-edge-deploy-configure-compute/verify-data-transform-5.png) 
 
    Los datos se mueven al recurso compartido en la nube.

    ![Comprobar transformación de datos](./media/data-box-edge-deploy-configure-compute/verify-data-transform-6.png)  

    A continuación, los datos se trasladan del recurso compartido en la nube a la cuenta de almacenamiento. Para ver los datos vaya al Explorador de Storage.

    ![Comprobar transformación de datos](./media/data-box-edge-deploy-configure-compute/verify-data-transform-7.png) 
 
Ha completado el proceso de validación.


## <a name="next-steps"></a>Pasos siguientes

En este tutorial aprendió lo siguiente:

> [!div class="checklist"]
> * Crear un recurso de IoT Hub
> * Configurar un rol de proceso
> * Agregar un módulo de proceso
> * Comprobar la transformación y la transferencia de los datos

Para aprender a administrar el dispositivo de Data Box Edge, consulte:

> [!div class="nextstepaction"]
> [Uso de la interfaz de usuario web local para administrar una instancia de Data Box Edge](https://aka.ms/dbg-docs)


