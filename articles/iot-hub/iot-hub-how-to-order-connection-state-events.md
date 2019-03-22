---
title: Ordenación de eventos de conexión de dispositivos de Azure IoT Hub mediante Azure Cosmos DB | Microsoft Docs
description: En este artículo se describe cómo ordenar y registrar los eventos de conexión de dispositivos de Azure IoT Hub mediante Azure Cosmos DB para mantener el estado de conexión más reciente
services: iot-hub
ms.service: iot-hub
author: ash2017
ms.topic: conceptual
ms.date: 07/06/2018
ms.author: asrastog
ms.openlocfilehash: edaa0cdcd98ce529ccbf9bf6aafaacf57a3ea383
ms.sourcegitcommit: 02d17ef9aff49423bef5b322a9315f7eab86d8ff
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/21/2019
ms.locfileid: "58337534"
---
# <a name="order-device-connection-events-from-azure-iot-hub-using-azure-cosmos-db"></a>Ordenación de eventos de conexión de dispositivos de Azure IoT Hub mediante Azure Cosmos DB

Azure Event Grid le ayuda a compilar aplicaciones basadas en eventos y a integrar fácilmente eventos de IoT en sus soluciones empresariales. En este artículo se describe una configuración que se puede usar para realizar un seguimiento y almacenar el estado de conexión de dispositivo más reciente en Cosmos DB. Se usará el número de secuencia disponible en los eventos de dispositivo conectado y de dispositivo desconectado, y se almacenará el estado más reciente en Cosmos DB. Se va a usar un procedimiento almacenado, que es una lógica de aplicación que se ejecuta en una colección de Cosmos DB.

El número de secuencia es una representación de cadena de un número hexadecimal. Puede usar la comparación de cadenas para identificar el número más grande. Si va a convertir la cadena a hexadecimal, el número será de 256 bits. El número de secuencia es estrictamente ascendente por lo que el registro más reciente tendrá un número mayor que el de los otros eventos. Esto resulta útil si tiene conexiones y desconexiones frecuentes de dispositivo y quiere asegurarse de que solo el evento más reciente se usa para desencadenar una acción de nivel inferior, ya que Azure Event Grid no admite la ordenación de eventos.

## <a name="prerequisites"></a>Requisitos previos

* Una cuenta de Azure activa. En caso de no tener ninguna, puede [crear una gratis](https://azure.microsoft.com/pricing/free-trial/).

* Una cuenta activa de la API de SQL de Azure Cosmos DB. Si aún no la ha creado, consulte [Creación de una cuenta de base de datos](https://docs.microsoft.com/azure/cosmos-db/create-sql-api-dotnet#create-an-account) para ver un tutorial.

* Una colección en la base de datos. Consulte [Incorporación de una colección](https://docs.microsoft.com/azure/cosmos-db/create-sql-api-dotnet#add-a-database-and-a-collection) para ver un tutorial.

* Una instancia de IoT Hub en Azure. Si aún no ha creado ninguna, vea un tutorial en [Introducción a Azure IoT Hub](../iot-hub/iot-hub-csharp-csharp-getstarted.md). 

## <a name="create-a-stored-procedure"></a>Creación de un procedimiento almacenado

En primer lugar, cree un procedimiento almacenado y configúrelo para ejecutar una lógica que compare los números de secuencia de los eventos de entrada y que registre el evento más reciente por dispositivo en la base de datos.

1. En la API de SQL de Cosmos DB, seleccione **Explorador de datos** > **Elementos** > **Nuevo procedimiento almacenado**.

   ![Crear procedimiento almacenado](./media/iot-hub-how-to-order-connection-state-events/create-stored-procedure.png)

2. Escriba un identificador de procedimiento almacenado y pegue lo siguiente en el "cuerpo del procedimiento almacenado". Tenga en cuenta que este código debe reemplazar cualquier código ya existente en el cuerpo del procedimiento almacenado. Este código incluye una fila por cada identificador de dispositivo y registra el último estado de conexión de ese identificador de dispositivo mediante la identificación del número de secuencia más alto. 

    ```javascript
    // SAMPLE STORED PROCEDURE
    function UpdateDevice(deviceId, moduleId, hubName, connectionState, connectionStateUpdatedTime, sequenceNumber) {
      var collection = getContext().getCollection();
      var response = {};
      
      var docLink = getDocumentLink(deviceId, moduleId);

      var isAccepted = collection.readDocument(docLink, function(err, doc) {
        if (err) {
          console.log('Cannot find device ' + docLink + ' - ');
          createDocument();
        } else {
          console.log('Document Found - ');
          replaceDocument(doc);
        }
      });

      function replaceDocument(document) {
        console.log(
          'Old Seq :' +
            document.sequenceNumber +
            ' New Seq: ' +
            sequenceNumber +
            ' - '
        );
        if (sequenceNumber > document.sequenceNumber) {
          document.connectionState = connectionState;
          document.connectionStateUpdatedTime = connectionStateUpdatedTime;
          document.sequenceNumber = sequenceNumber;

          console.log('replace doc - ');

          isAccepted = collection.replaceDocument(docLink, document, function(
            err,
            updated
          ) {
            if (err) {
              getContext()
                .getResponse()
                .setBody(err);
            } else {
              getContext()
                .getResponse()
                .setBody(updated);
            }
          });
        } else {
          getContext()
            .getResponse()
            .setBody('Old Event - current: ' + document.sequenceNumber + ' Incoming: ' + sequenceNumber);
        }
      }
      function createDocument() {
        document = {
          id: deviceId + '-' + moduleId,
          deviceId: deviceId,
          moduleId: moduleId,
          hubName: hubName,
          connectionState: connectionState,
          connectionStateUpdatedTime: connectionStateUpdatedTime,
          sequenceNumber: sequenceNumber
        };
        console.log('Add new device - ' + collection.getAltLink());
        isAccepted = collection.createDocument(
          collection.getAltLink(),
          document,
          function(err, doc) {
            if (err) {
              getContext()
                .getResponse()
                .setBody(err);
            } else {
              getContext()
                .getResponse()
                .setBody(doc);
            }
          }
        );
      }

      function getDocumentLink(deviceId, moduleId) {
        return collection.getAltLink() + '/docs/' + deviceId + '-' + moduleId;
      }
    }
    ```

3. Guarde el procedimiento almacenado: 

    ![guardar el procedimiento almacenado](./media/iot-hub-how-to-order-connection-state-events/save-stored-procedure.png)

## <a name="create-a-logic-app"></a>Creación de una aplicación lógica

En primer lugar, cree una aplicación lógica y agregue un desencadenador de la cuadrícula de eventos que supervise el grupo de recursos de la máquina virtual. 

### <a name="create-a-logic-app-resource"></a>Creación de un recurso de aplicación lógica

1. En [Azure Portal](https://portal.azure.com), seleccione **Nuevo** > **Integration** > **Logic App**.

   ![Creación de la aplicación lógica](./media/iot-hub-how-to-order-connection-state-events/select-logic-app.png)

2. Asigne un nombre a la aplicación lógica que sea único en su suscripción; a continuación, seleccione la misma suscripción, el grupo de recursos y la ubicación en que se encuentra la instancia de IoT Hub. 

3. Elija **Anclar al panel** y **Crear**.

   Ahora ha creado un recurso de Azure para la aplicación lógica. Después de que Azure implemente la aplicación lógica, el Diseñador de aplicaciones lógicas muestra plantillas de patrones comunes de modo que pueda empezar con mayor rapidez.

   > [!NOTE] 
   > Cuando selecciona **Anclar al panel**, la aplicación lógica se abre automáticamente en el Diseñador de aplicaciones lógicas. En caso contrario, puede buscarla y abrirla manualmente.

4. En el Diseñador de aplicación lógica que se encuentra en **Plantillas**, elija **Aplicación lógica en blanco** para que pueda compilar la aplicación lógica desde el principio.

### <a name="select-a-trigger"></a>Selección de un desencadenador

Un desencadenador es un evento específico que inicia la aplicación lógica. Para este tutorial, el desencadenador que activa el flujo de trabajo recibe una solicitud a través de HTTP.  

1. En la barra de búsqueda de conectores y desencadenadores, escriba **HTTP**.

2. Seleccione **Solicitud: Cuando se recibe una solicitud HTTP** como desencadenador. 

   ![Selección del desencadenador de solicitud HTTP](./media/iot-hub-how-to-order-connection-state-events/http-request-trigger.png)

3. Seleccione **Usar una carga de ejemplo para generar el esquema**. 

   ![Use una carga de ejemplo para generar el esquema](./media/iot-hub-how-to-order-connection-state-events/sample-payload.png)

4. Pegue el siguiente código JSON de ejemplo en el cuadro de texto y después seleccione **Listo**:

   ```json
   [{
    "id": "fbfd8ee1-cf78-74c6-dbcf-e1c58638ccbd",
    "topic":
      "/SUBSCRIPTIONS/DEMO5CDD-8DAB-4CF4-9B2F-C22E8A755472/RESOURCEGROUPS/EGTESTRG/PROVIDERS/MICROSOFT.DEVICES/IOTHUBS/MYIOTHUB",
    "subject": "devices/Demo-Device-1",
    "eventType": "Microsoft.Devices.DeviceConnected",
    "eventTime": "2018-07-03T23:20:11.6921933+00:00",
    "data": {
      "deviceConnectionStateEventInfo": {
        "sequenceNumber":
          "000000000000000001D4132452F67CE200000002000000000000000000000001"
      },
      "hubName": "MYIOTHUB",
      "deviceId": "48e44e11-1437-4907-83b1-4a8d7e89859e",
      "moduleId": ""
    },
    "dataVersion": "1",
    "metadataVersion": "1"
   }]
   ```

5. Puede recibir una notificación emergente que dice: **Recuerde incluir un encabezado Content-Type establecido en application/json en la solicitud.** Puede ignorar esta sugerencia sin ningún problema y pasar a la sección siguiente. 

### <a name="create-a-condition"></a>Creación de una condición

En el flujo de trabajo de la aplicación lógica, las condiciones ayudan a ejecutar acciones concretas cuando se produce una condición específica. Una vez que se cumple la condición, se puede definir una acción deseada. En este tutorial, la condición es comprobar si eventType es Dispositivo conectado o Dispositivo desconectado. La acción consiste en ejecutar el procedimiento almacenado en la base de datos. 

1. Seleccione **Nuevo paso** y, a continuación, **Integraciones** y **Condición**. 

2. Rellene la condición como se muestra a continuación para que solo se ejecute en caso de eventos de Dispositivo conectado y Dispositivo desconectado:

   * Elija un valor: **eventType**
   * Cambie "es igual a" por **termina con**
   * Elija un valor: **nected**

     ![Rellenar condición](./media/iot-hub-how-to-order-connection-state-events/condition-detail.png)

3. Si la condición es true, haga clic en **Agregar una acción**.
  
   ![Agregar acción si es true](./media/iot-hub-how-to-order-connection-state-events/action-if-true.png)

4. Busque Cosmos DB y haga clic en **Azure Cosmos DB: Ejecutar procedimiento almacenado**

   ![Buscar CosmosDB](./media/iot-hub-how-to-order-connection-state-events/cosmosDB-search.png)

5. Rellene el formulario de la opción Ejecutar procedimiento almacenado seleccionando los valores de la base de datos. Escriba el valor de la clave de partición y los parámetros como se muestra a continuación. 

   ![rellenar acción de aplicación lógica](./media/iot-hub-how-to-order-connection-state-events/logicapp-stored-procedure.png)

6. Guarde la aplicación lógica. 

### <a name="copy-the-http-url"></a>Copia de la dirección URL HTTP

Antes de cerrar el Diseñador de aplicaciones lógicas, copie la dirección URL en la que se realizan las escuchas de un desencadenador de aplicación lógica. Use esta dirección URL para configurar Event Grid. 

1. Haga clic en el cuadro de configuración del desencadenador **Cuando se recibe una solicitud HTTP** para expandirlo. 

2. Seleccione el botón de copia que se encuentra junto al valor **Dirección URL de HTTP POST** para copiarlo. 

   ![Copia de la dirección URL de HTTP POST](./media/iot-hub-how-to-order-connection-state-events/copy-url.png)

3. Guarde esta dirección URL para poder hacer referencia a ella en la siguiente sección. 

## <a name="configure-subscription-for-iot-hub-events"></a>Configurar la suscripción de eventos de IoT Hub

En esta sección, va a configurar IoT Hube para publicar eventos cuando se produzcan. 

1. En Azure Portal, vaya hasta el centro de IoT. 

2. Seleccione **Eventos**.

   ![Abrir los detalles de Event Grid](./media/iot-hub-how-to-order-connection-state-events/event-grid.png)

3. Seleccione **Suscripción de eventos**. 

   ![Crear una suscripción de eventos](./media/iot-hub-how-to-order-connection-state-events/event-subscription.png)

4. Cree la suscripción de eventos con los siguientes valores: 

   * **Tipo de evento**: Desactive la opción Suscribirse a todos los tipos de evento y seleccione **Dispositivo conectado** y **Dispositivo desconectado** en el menú.

   * **Detalles del punto de conexión**: En Tipo de punto de conexión, seleccione **Webhook**, haga clic en Seleccionar punto de conexión y pegue la dirección URL que copió de la aplicación lógica y confirme la selección.

       ![seleccionar dirección URL de punto de conexión](./media/iot-hub-how-to-order-connection-state-events/endpoint-url.png)

   * **Detalles de suscripciones de eventos**: Proporcione un nombre descriptivo y seleccione **Esquema de Event Grid**.
   El formulario es similar al del siguiente ejemplo: 

       ![Formulario de ejemplo de suscripción de eventos](./media/iot-hub-how-to-order-connection-state-events/subscription-form.png)

5. Seleccione **Crear** para guardar la suscripción de eventos.

## <a name="observe-events"></a>Observación de eventos

Ahora que la suscripción de eventos se ha configurado, vamos a probar a conectar un dispositivo.

### <a name="register-a-device-in-iot-hub"></a>Registro de un dispositivo en IoT Hub

1. En IoT Hub, seleccione **Dispositivos IoT**. 

2. Seleccione **Agregar**.

3. En **Id. de dispositivo**, escriba `Demo-Device-1`.

4. Seleccione **Guardar**. 

5. Puede agregar varios dispositivos con distintos identificadores de dispositivo.

   ![Resultado del procedimiento](./media/iot-hub-how-to-order-connection-state-events/AddIoTDevice.png)

6. Copie el valor de **Cadena de conexión: clave principal** para usarlo más adelante.

   ![Resultado del procedimiento](./media/iot-hub-how-to-order-connection-state-events/DeviceConnString.png)

### <a name="start-raspberry-pi-simulator"></a>Inicio del simulador de Raspberry Pi

1. Vamos a usar el simulador web de Raspberry Pi para simular la conexión del dispositivo.

[Iniciar simulador de Raspberry Pi](https://azure-samples.github.io/raspberry-pi-web-simulator/#Getstarted)

### <a name="run-a-sample-application-on-the-raspberry-pi-web-simulator"></a>Ejecución de una aplicación de ejemplo en el simulador web de Raspberry Pi

Esto desencadenará un evento de dispositivo conectado.

1. En el área de codificación, reemplace el marcador de posición de la línea 15 con la cadena de conexión del dispositivo de Azure IoT Hub.

   ![Resultado del procedimiento](./media/iot-hub-how-to-order-connection-state-events/raspconnstring.png)

2. Para ejecutar la aplicación haga clic en **Ejecutar**.

Debería ver el resultado siguiente, que muestra los datos del sensor y los mensajes que se envían a IoT Hub.

   ![Resultado del procedimiento](./media/iot-hub-how-to-order-connection-state-events/raspmsg.png)

   Haga clic en **Detener** para detener el simulador y desencadenar un evento de **Dispositivo desconectado**.

Ahora ya ha ejecutado una aplicación de ejemplo para recopilar datos de un sensor y enviarlos a IoT Hub. 

### <a name="observe-events-in-cosmos-db"></a>Observación de eventos en Cosmos DB

Puede ver los resultados del procedimiento almacenado ejecutado en el documento de Cosmos DB. Se ven de la siguiente forma. Cada fila contiene el estado de conexión de dispositivo más reciente por cada dispositivo.

   ![Resultado del procedimiento](./media/iot-hub-how-to-order-connection-state-events/cosmosDB-outcome.png)

## <a name="use-the-azure-cli"></a>Uso de la CLI de Azure

En lugar de usar [Azure Portal](https://portal.azure.com), puede llevar a cabo los pasos de IoT Hub mediante la CLI de Azure. Para más información, consulte las páginas de la CLI de Azure para [crear una suscripción de eventos](https://docs.microsoft.com/cli/azure/eventgrid/event-subscription) y [crear un dispositivo IoT](/cli/azure/ext/azure-cli-iot-ext/iot/hub/device-identity#ext-azure-cli-iot-ext-az-iot-hub-device-identity-create).

## <a name="clean-up-resources"></a>Limpieza de recursos

En este tutorial se usaron recursos que generan gastos en su suscripción de Azure. Cuando haya terminado de probar el tutorial y los resultados de las pruebas, deshabilite o elimine los recursos que no desea conservar. 

Si no desea perder el trabajo realizado en la aplicación lógica, deshabilítela en lugar de eliminarla. 

1. Vaya a la aplicación lógica.

2. En la hoja **Información general**, seleccione **Eliminar** o **Deshabilitar**. 

Cada suscripción solo puede tener una instancia de IoT Hub gratuita. Si creó una instancia de IoT Hub gratis para este tutorial, no es necesario eliminarla para evitar cargos.

1. Vaya a su instancia de IoT Hub. 

2. En la página **Información general**, seleccione **Eliminar**. 

Aunque conserve la instancia de IoT Hub, puede que desee eliminar la suscripción de eventos que creó. 

1. En la instancia de IoT Hub, seleccione **Event Grid**.

2. Seleccione la suscripción de eventos que desea eliminar. 

3. Seleccione **Eliminar**. 

Para quitar una cuenta de Azure Cosmos DB de Azure Portal, haga clic con el botón derecho en el nombre de la cuenta y haga clic en **Eliminar cuenta**. Consulte las instrucciones detalladas para [eliminar una cuenta de Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/manage-account).

## <a name="next-steps"></a>Pasos siguientes

* Obtenga más información sobre [reacción a eventos de IoT Hub mediante Event Grid para desencadenar acciones](../iot-hub/iot-hub-event-grid.md).

* [Pruebe el tutorial de eventos de IoT Hub](../event-grid/publish-iot-hub-events-to-logic-apps.md)

* Obtenga información sobre qué más puede hacer con [Event Grid](../event-grid/overview.md).
