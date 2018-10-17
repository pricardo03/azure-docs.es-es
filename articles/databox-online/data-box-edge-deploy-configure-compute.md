---
title: Transformación de datos con Azure Data Box Edge | Microsoft Docs
description: Aprenda a configurar el rol de proceso en Data Box Edge y a usarlo para transformar los datos antes de enviarlos a Azure.
services: databox-edge-gateway
documentationcenter: NA
author: alkohli
manager: twooley
editor: ''
ms.assetid: ''
ms.service: databox-edge-gateway
ms.devlang: NA
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/08/2018
ms.author: alkohli
ms.custom: ''
Customer intent: As an IT admin, I need to understand how to configure compute on Data Box Edge so I can use it to transform the data before sending it to Azure.
ms.openlocfilehash: fd04c906f57a9378879ef6357e2724695533d370
ms.sourcegitcommit: 26cc9a1feb03a00d92da6f022d34940192ef2c42
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/06/2018
ms.locfileid: "48833108"
---
# <a name="tutorial-transform-data-with-azure-data-box-edge-preview"></a>Tutorial: Transformación de datos con Azure Data Box Edge (versión preliminar)

Este tutorial describe cómo configurar el rol de proceso en Data Box Edge. Una vez configurado el rol de proceso, Azure Data Box Edge puede transformar los datos antes de enviarlos a Azure.

Este procedimiento tarda aproximadamente 30-45 minutos en completarse. 

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Crear un recurso de IoT Hub
> * Configurar un rol de proceso
> * Agregar un módulo de proceso
> * Comprobar la transformación y la transferencia de los datos

> [!IMPORTANT]
> Data Box Edge se encuentra en versión preliminar. Antes de solicitar e implementar esta solución revise los [términos del servicio de Azure para la versión preliminar](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). 
 
## <a name="prerequisites"></a>Requisitos previos

Antes de configurar procesos en Data Box Edge, asegúrese de que:

* El dispositivo de Data Box Edge se activa tal y como se describe en [Connect and activate your Azure Data Box Edge](data-box-edge-deploy-connect-setup-activate.md) (Conexión y activación de Azure Data Box Edge).


## <a name="create-an-iot-hub-resource"></a>Crear un recurso de IoT Hub

Antes de configurar el rol de proceso en Data Box Edge, debe crear un recurso de IoT Hub.

Para obtener instrucciones detalladas, vaya a [Crear un centro de IoT](https://docs.microsoft.com/azure/iot-hub/iot-hub-create-through-portal#create-an-iot-hub). Use la misma suscripción y grupo de recursos que usó para el recurso de Data Box Edge.

![Crear un recurso de IoT Hub](./media/data-box-edge-deploy-configure-compute/create-iothub-resource-1.png)

Cuando el rol de proceso de Edge no está configurado, tenga en cuenta que 
- El recurso de IoT Hub no tiene dispositivos IoT ni de IoT Edge.
- No puede crear recursos compartidos locales de Edge. Cuando agrega un recurso compartido, no está habilitada la opción para crear un recurso compartido local para el proceso de Edge.


## <a name="set-up-compute-role"></a>Configurar un rol de proceso

Cuando el rol de proceso de Edge está configurado en el dispositivo de Edge, este crea dos dispositivos: uno es un dispositivo IoT y el otro es un dispositivo de IoT Edge. Ambos dispositivos se pueden ver en el recurso de IoT Hub.

Para configurar el rol de proceso en el dispositivo, realice los pasos siguientes.

1. Vaya al recurso de Data Box Edge y, a continuación, vaya a **Introducción** y haga clic en **Configurar rol de proceso**. 

    ![Configurar un rol de proceso](./media/data-box-edge-deploy-configure-compute/setup-compute-1.png)
   
    También puede ir a **Módulos** y hacer clic en **Configurar proceso**.

    ![Configurar un rol de proceso](./media/data-box-edge-deploy-configure-compute/setup-compute-2.png)
 
2. En la lista desplegable, seleccione el **recurso de IoT Hub** que creó en el paso anterior. En este momento solo está disponible la plataforma Linux para el dispositivo de IoT Edge. Haga clic en **Create**(Crear).

    ![Configurar un rol de proceso](./media/data-box-edge-deploy-configure-compute/setup-compute-3.png)
 
3. El rol de proceso tarda un par de minutos en crearse. Debido a un error en esta versión, no se actualiza la pantalla aunque se haya configurado el rol de proceso. Vaya a **Módulos** y podrá ver que el proceso de Edge se ha configurado.  

    ![Configurar un rol de proceso](./media/data-box-edge-deploy-configure-compute/setup-compute-4.png)

4. Vaya a **Introducción** de nuevo y, en esta ocasión, la pantalla se actualizará para indicar que se ha configurado el rol de proceso.

    ![Configurar un rol de proceso](./media/data-box-edge-deploy-configure-compute/setup-compute-5.png)
 
5. Vaya a la instancia de IoT Hub que utilizó al crear el rol de proceso de Edge. Vaya a **Dispositivos IoT**. Ahora puede ver que hay un dispositivo IoT habilitado. 

    ![Configurar un rol de proceso](./media/data-box-edge-deploy-configure-compute/setup-compute-6.png)

6. Vaya a **IoT Edge** y verá que un dispositivo de IoT Edge también está habilitado.

    ![Configurar un rol de proceso](./media/data-box-edge-deploy-configure-compute/setup-compute-7.png)
 
7. Seleccione y haga clic en el dispositivo de IoT Edge. Un agente de Edge se está ejecutando en este dispositivo de IoT Edge. 

    ![Configurar un rol de proceso](./media/data-box-edge-deploy-configure-compute/setup-compute-8.png) 

Sin embargo, no hay módulos personalizados en este dispositivo de Edge. Ahora puede agregar uno a este dispositivo.


## <a name="add-a-custom-module"></a>Incorporación de un módulo personalizado

En esta sección, agregará un módulo personalizado al dispositivo de IoT Edge. 

Este procedimiento usa un ejemplo en el que el módulo personalizado que se usa toma archivos de un recurso compartido local del dispositivo de Edge y los mueve a un recurso compartido en la nube en el dispositivo. A continuación, el recurso compartido en la nube inserta los archivos en la cuenta de Azure Storage asociada con este. 

1. El primer paso es agregar un recurso compartido local en el dispositivo de Edge. En el recurso de Data Box Edge, vaya a **Recursos compartidos**. Haga clic en **Agregar recurso compartido**. Proporcione el nombre del recurso compartido y seleccione el tipo de recurso. Para crear un recurso compartido local, seleccione la opción **Configurar como recurso compartido local de Edge**. Seleccione un **usuario existente** o **cree uno nuevo**. Haga clic en **Create**(Crear).

    ![Agregar un módulo personalizado](./media/data-box-edge-deploy-configure-compute/add-a-custom-module-1.png) 

    Si ha creado un recurso compartido NFS local, use la siguiente opción de comando rsync para copiar archivos en él:

    `rsync --inplace <source file path> < destination file path>`

     Para más información sobre el comando rsync, consulte la [documentación sobre rsync](https://www.computerhope.com/unix/rsync.htm). 

 
2. Una vez que haya creado el recurso compartido local y haya recibido una notificación de creación correcta (la lista de recursos compartidos se puede actualizar antes pero debe esperar a que termine la creación del recurso compartido), vaya a la lista de recursos compartidos. 

    ![Agregar un módulo personalizado](./media/data-box-edge-deploy-configure-compute/add-a-custom-module-2.png) 
 
3. Seleccione y haga clic en el recurso compartido local recién creado y vea sus propiedades. Copie y guarde el **punto de montaje local para los módulos de Edge** correspondiente a este recurso compartido.

    ![Agregar un módulo personalizado](./media/data-box-edge-deploy-configure-compute/add-a-custom-module-3.png) 
 
    Vaya a un recurso compartido en la nube ya existente que se haya creado en el dispositivo de Data Box Edge. De nuevo, copie y guarde el punto de montaje local de los módulos de proceso de Edge para este recurso compartido en la nube. Estos puntos de montaje locales se usarán al implementar el módulo.

    ![Agregar un módulo personalizado](./media/data-box-edge-deploy-configure-compute/add-a-custom-module-4.png)  

4. Para agregar un módulo personalizado al dispositivo de IoT Edge, vaya al recurso de IoT Hub y, a continuación, vaya al **dispositivo de IoT Edge**. Seleccione y haga clic en el dispositivo. En **Detalles del dispositivo**, en la barra de comandos de la parte superior, haga clic en **Establecer módulos**. 

    ![Agregar un módulo personalizado](./media/data-box-edge-deploy-configure-compute/add-a-custom-module-5.png) 

5. En **Agregar módulos**, realice los pasos siguientes:

    1. Proporcione el **nombre**, **dirección**, **nombre de usuario** y **contraseña** de la **Configuración de Container Registry** para el módulo personalizado. El nombre, dirección y las credenciales enumeradas se utilizan para recuperar aquellos módulos con una dirección URL que coincida. Para implementar este módulo, en **Módulos de implementación**, seleccione **Módulo de IoT Edge**. Este módulo de IoT Edge es un contenedor de Docker que puede implementar en un dispositivo de IoT Edge asociado con el dispositivo de Data Box Edge.

        ![Agregar un módulo personalizado](./media/data-box-edge-deploy-configure-compute/add-a-custom-module-6.png) 
 
    2. Especifique la configuración para el módulo personalizado de IoT Edge. Proporcione el **nombre** del módulo y el **identificador URI de la imagen**. 
    
        ![Agregar un módulo personalizado](./media/data-box-edge-deploy-configure-compute/add-a-custom-module-7.png) 

    3. En **Opciones de creación del contenedor**, proporcione los puntos de montaje locales para los módulos de Edge que se copiaron en los pasos anteriores para el recurso compartido en la nube y el recurso compartido local (es importante usar estas rutas de acceso en lugar de crear otras nuevas). Estos recursos compartidos se asignan a los puntos de montaje de contenedor correspondientes. También se proporcionan aquí todas las variables de entorno del módulo.

        ![Agregar un módulo personalizado](./media/data-box-edge-deploy-configure-compute/add-a-custom-module-8.png) 
 
    4. **Configure las opciones avanzadas del entorno de ejecución de Edge** si es necesario y, a continuación, haga clic en **Siguiente**.

        ![Agregar un módulo personalizado](./media/data-box-edge-deploy-configure-compute/add-a-custom-module-9.png) 
 
6.  En **Especificar rutas**, defina las rutas entre módulos. En este caso, proporcione el nombre del recurso compartido local que insertará los datos en el recurso compartido en la nube. Haga clic en **Next**.

    ![Agregar un módulo personalizado](./media/data-box-edge-deploy-configure-compute/add-a-custom-module-10.png) 
 
7.  En **Revisar la implementación**, revise todos los valores y, si está de acuerdo, **envíe** el módulo para la implementación.

    ![Agregar un módulo personalizado](./media/data-box-edge-deploy-configure-compute/add-a-custom-module-11.png) 
 
Esto inicia la implementación del módulo, como se muestra en **Módulo personalizado de IoT Edge** en **Módulos**.

![Agregar un módulo personalizado](./media/data-box-edge-deploy-configure-compute/add-a-custom-module-12.png) 

### <a name="verify-data-transform-and-transfer"></a>Comprobar la transformación y la transferencia de los datos

El último paso es asegurarse de que el módulo está conectado y funciona según lo previsto. Realice los pasos siguientes para comprobar que el módulo se está ejecutando.

1. El estado del entorno de ejecución del módulo debe ser "en ejecución" para el dispositivo de IoT Edge del recurso de IoT Hub.

    ![Comprobar transformación de datos](./media/data-box-edge-deploy-configure-compute/verify-data-transform-1.png) 
 
2. Seleccione y haga clic en el módulo y examine **Identidad de módulo gemela**. El estado de cliente para el dispositivo y el módulo de Edge debe aparecer como **Conectado**.

    ![Comprobar transformación de datos](./media/data-box-edge-deploy-configure-compute/verify-data-transform-2.png) 
 
3.  Una vez que el módulo está en ejecución, también aparece en la lista de módulos de Edge en el recurso de Data Box Edge. El **estado del entorno de ejecución** del módulo agregado es **en ejecución**.

    ![Comprobar transformación de datos](./media/data-box-edge-deploy-configure-compute/verify-data-transform-3.png) 
 
4.  Conecte el recurso compartido local y el de la nube que creó mediante el Explorador de archivos.

    ![Comprobar transformación de datos](./media/data-box-edge-deploy-configure-compute/verify-data-transform-4.png) 
 
5.  Agregue datos al recurso compartido local.

    ![Comprobar transformación de datos](./media/data-box-edge-deploy-configure-compute/verify-data-transform-5.png) 
 
6.  Los datos se mueven al recurso compartido en la nube.

    ![Comprobar transformación de datos](./media/data-box-edge-deploy-configure-compute/verify-data-transform-6.png)  

7.  A continuación, los datos se trasladan del recurso compartido en la nube a la cuenta de almacenamiento. Vaya al Explorador de Storage para ver los datos.

    ![Comprobar transformación de datos](./media/data-box-edge-deploy-configure-compute/verify-data-transform-7.png) 
 
Esto concluye el proceso de validación.


## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha obtenido información acerca de varios temas relacionados con Data Box Edge, como:

> [!div class="checklist"]
> * Crear un recurso de IoT Hub
> * Configurar un rol de proceso
> * Agregar un módulo de proceso
> * Comprobar la transformación y la transferencia de los datos

Pase al siguiente tutorial para aprender a administrar Data Box Edge.

> [!div class="nextstepaction"]
> [Uso de la interfaz de usuario web local para administrar una instancia de Data Box Edge](http://aka.ms/dbg-docs)


