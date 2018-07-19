---
title: Procedimiento para guardar los mensajes de IoT Hub en el almacenamiento de datos de Azure | Microsoft Docs
description: Use el enrutamiento de mensajes de IoT Hub para guardar los mensajes de IoT Hub en Azure Blob Storage. Los mensajes del IoT Hub contienen información, como datos del sensor, que se envía desde el dispositivo de IoT.
author: rangv
manager: ''
keywords: almacenamiento de datos de iot, almacenamiento de datos del sensor de iot
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.tgt_pltfrm: arduino
ms.date: 04/11/2018
ms.author: rangv
ms.openlocfilehash: 84355453a5cb8d8f42abdcbde5432651c9c035b0
ms.sourcegitcommit: 0b4da003fc0063c6232f795d6b67fa8101695b61
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37856297"
---
# <a name="save-iot-hub-messages-that-contain-sensor-data-to-your-azure-blob-storage"></a>Almacenamiento de mensajes de IoT Hub que contienen datos del sensor en Azure Blob Storage

![Diagrama integral](./media/iot-hub-store-data-in-azure-table-storage/1_route-to-storage.png)

[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

## <a name="what-you-learn"></a>Conocimientos que adquirirá

Obtendrá información sobre cómo crear una cuenta de almacenamiento de Azure y una aplicación de función de Azure para almacenar mensajes de IoT Hub en Azure Blob Storage.

## <a name="what-you-do"></a>Qué debe hacer

- Cree una cuenta de Azure Storage.
- Configure la instancia de IoT Hub para enrutar los mensajes al almacenamiento.

## <a name="what-you-need"></a>Lo que necesita

- [Instalar el dispositivo](iot-hub-raspberry-pi-kit-node-get-started.md) para que cumpla los siguientes requisitos:
  - Una suscripción de Azure activa.
  - Una instancia de IoT Hub en su suscripción. 
  - Una aplicación en ejecución que envíe mensajes a su instancia de IoT Hub.

## <a name="create-an-azure-storage-account"></a>Creación de una cuenta de Azure Storage

1. En [Azure Portal](https://portal.azure.com/) haga clic en **Crear un recurso** > **Almacenamiento** > **Cuenta de almacenamiento**.

2. Escriba la información necesaria para la cuenta de almacenamiento:

   ![Creación de una cuenta de almacenamiento en Azure Portal](./media/iot-hub-store-data-in-azure-table-storage/1_azure-portal-create-storage-account.png)

   * **Nombre**: el nombre de la cuenta de almacenamiento. El nombre debe ser único globalmente.

   * **Tipo de cuenta**: seleccione `Storage (general purpose v1)`.

   * **Ubicación**: seleccione la misma ubicación que emplea IoT Hub.

   * **Replicación**: elija `Locally-redundant storage (LRS)`.

   * **Rendimiento**: elija `Standard`.

   * **Se requiere transferencia segura**: elija `Disabled`.

   * **Suscripción**: seleccione su suscripción de Azure.

   * **Grupo de recursos**: use el mismo grupo de recursos que usa el centro de IoT.

   * **Anclar al panel**: seleccione esta opción para facilitar el acceso a IoT Hub desde el panel.

3. Haga clic en **Create**(Crear).

## <a name="prepare-your-iot-hub-to-route-messages-to-storage"></a>Preparación de una instancia de IoT Hub para enrutar mensajes al almacenamiento

IoT Hub admite de forma nativa enrutar mensajes a Azure Storage como blobs. Para más información sobre los puntos de conexión personalizados de Azure IoT Hub, consulte la [lista de puntos de conexión de IoT Hub integrados](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-endpoints#custom-endpoints).

### <a name="add-storage-as-a-custom-endpoint"></a>Adición de almacenamiento como punto de conexión personalizado

1. Vaya a IoT Hub en Azure Portal. 

2. Haga clic en **Puntos de conexión** > **Agregar**. 

3. Asigne un nombre al punto de conexión y seleccione **Contenedor de almacenamiento de Azure** como el tipo de punto de conexión. 

4. Utilice el selector para seleccionar la cuenta de almacenamiento que creó en la sección anterior. Cree un contenedor de almacenamiento, selecciónelo y, después, haga clic en **Aceptar**.

   ![Creación de un punto de conexión personalizado en IoT Hub](./media/iot-hub-store-data-in-azure-table-storage/2_custom-storage-endpoint.png)

### <a name="add-a-route-to-route-data-to-storage"></a>Adición de una ruta para enrutar datos al almacenamiento

1. Haga clic en **Rutas** > **Agregar** y escriba un nombre para la ruta. 

2. Seleccione **Mensajes del dispositivo** como origen de datos y seleccione el punto de conexión de almacenamiento recién creado como el punto de conexión de la ruta. 

3. Escriba `true` como la cadena de consulta y después haga clic en **Guardar**.

   ![Creación de una ruta en IoT Hub](./media/iot-hub-store-data-in-azure-table-storage/3_create-route.png)
  
### <a name="add-a-route-for-hot-path-telemetry-optional"></a>Adición de una ruta para telemetría de ruta de acceso activa (opcional)

De forma predeterminada, IoT Hub enruta todos los mensajes que no coinciden con ninguna otra ruta al punto de conexión integrado. Puesto que todos los mensajes de telemetría ahora coinciden con la regla que enruta los mensajes al almacenamiento, debe agregar otra ruta para que los mensajes se escriban en el punto de conexión integrado. No se aplica ningún cargo adicional por enrutar mensajes a varios puntos de conexión.

> [!NOTE]
> Puede omitir este paso si no realiza un procesamiento adicional de los mensajes de telemetría.

1. Haga clic en **Agregar** desde el panel de Rutas y escriba un nombre para la ruta. 

2. Seleccione **Mensajes del dispositivo** como el origen de datos y, **eventos** como el punto de conexión. 

3. Escriba `true` como la cadena de consulta y después haga clic en **Guardar**.

  ![Creación de una ruta de ruta de acceso activa en IoT Hub](./media/iot-hub-store-data-in-azure-table-storage/4_hot-path-route.png)

## <a name="verify-your-message-in-your-storage-container"></a>Verificación del mensaje en el contenedor de almacenamiento

1. Ejecute la aplicación de ejemplo en el dispositivo para enviar mensajes a su IoT Hub.

2. [Descargue e instale el Explorador de Azure Storage](http://storageexplorer.com/).

3. Abra el Explorador de Storage, haga clic en **Add an Azure Account** (Agregar una cuenta de Azure)  > **Iniciar sesión** e inicie sesión en su cuenta de Azure.

4. Haga clic en la suscripción de Azure > **Cuentas de almacenamiento** > su cuenta de almacenamiento > **Contenedores de blob** > su contenedor.

   Verá que los mensajes enviados desde el dispositivo a IoT Hub se han registrado en el contenedor de blobs.

## <a name="clean-up-resources"></a>Limpieza de recursos 

En este tutorial, ha agregado una cuenta de almacenamiento y después enrutamiento para que los mensajes de IoT Hub se escriban en la cuenta de almacenamiento. Para limpiar los recursos que ha creado, quite la información de enrutamiento y, después, elimine la cuenta de almacenamiento. 

1. Inicie sesión en [Azure Portal](https://portal.azure.com).

2. Haga clic en **Grupos de recursos** y seleccione el grupo de recursos que ha usado. Se muestra la lista de recursos del grupo. 

   > [!NOTE]
   > Si quiere quitar todos los recursos del grupo de recursos, haga clic en **Eliminar** para eliminar el grupo de recursos y, después, siga las instrucciones. Esta acción quitará todo el contenido de ese grupo de recursos, por lo que ha terminado de limpiar los recursos y puede ir a la sección siguiente.

3. Haga clic en el IoT Hub que ha usado para este tutorial. 

4. En el panel IoT Hub, haga clic en **Rutas**. Haga clic en la casilla situada junto a la regla de enrutamiento que ha agregado y luego haga clic en **Eliminar**. Cuando se le pregunte si está seguro de que quiere eliminar esa ruta, haga clic en **Sí**.

   ![Eliminación de la regla de enrutamiento](./media/iot-hub-store-data-in-azure-table-storage/cleanup-remove-routing.png)

   Cierre el panel Enrutamiento. Se le redirigirá al panel Grupo de recursos.

5. Haga clic de nuevo en IoT Hub. 

6. En el panel IoT Hub, haga clic en **Puntos de conexión**. Haga clic en la casilla situada junto al punto de conexión que ha agregado para el contenedor de almacenamiento y, después, haga clic en **Eliminar**. Cuando se le pregunte si está seguro de que quiere eliminar el punto de conexión seleccionado, haga clic en **Sí**.

    ![Eliminación del punto de conexión](./media/iot-hub-store-data-in-azure-table-storage/cleanup-remove-endpoint.png)

    Cierre el panel Puntos de conexión. Se le redirigirá al panel Grupo de recursos. 

7.  Haga clic en la cuenta de almacenamiento que ha establecido para este tutorial. 

8.  En el panel Cuenta de almacenamiento, haga clic en **Eliminar** para quitar la cuenta de almacenamiento. Se le dirigirá al panel **Eliminar cuenta de almacenamiento**.

   ![Eliminación de la cuenta de almacenamiento](./media/iot-hub-store-data-in-azure-table-storage/cleanup-remove-storageaccount.png)

8.  Escriba el nombre de la cuenta de almacenamiento y, después, haga clic en **Eliminar** en la parte inferior del panel. 

## <a name="next-steps"></a>Pasos siguientes

Ha creado correctamente la cuenta de Azure Storage y ha enrutado los mensajes desde IoT Hub a un contenedor de blobs en esa cuenta de almacenamiento.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
