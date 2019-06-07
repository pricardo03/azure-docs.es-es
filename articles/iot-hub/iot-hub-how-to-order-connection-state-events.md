---
title: Ordenación de eventos de conexión de dispositivos de Azure IoT Hub mediante Azure Cosmos DB | Microsoft Docs
description: En este artículo se describe cómo ordenar y registrar los eventos de conexión de dispositivos de Azure IoT Hub mediante Azure Cosmos DB para mantener el estado de conexión más reciente
services: iot-hub
ms.service: iot-hub
author: ash2017
ms.topic: conceptual
ms.date: 04/11/2019
ms.author: asrastog
ms.openlocfilehash: f4baab6e0909144efc613572207e7f24c4b4fe1f
ms.sourcegitcommit: 7042ec27b18f69db9331b3bf3b9296a9cd0c0402
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/06/2019
ms.locfileid: "66743262"
---
# <a name="order-device-connection-events-from-azure-iot-hub-using-azure-cosmos-db"></a>Ordenación de eventos de conexión de dispositivos de Azure IoT Hub mediante Azure Cosmos DB

Azure Event Grid le ayuda a compilar aplicaciones basadas en eventos y a integrar fácilmente eventos de IoT en sus soluciones empresariales. En este artículo se describe una configuración que se puede usar para realizar un seguimiento y almacenar el estado de conexión de dispositivo más reciente en Cosmos DB. Se usará el número de secuencia disponible en los eventos de dispositivo conectado y de dispositivo desconectado, y se almacenará el estado más reciente en Cosmos DB. Se va a usar un procedimiento almacenado, que es una lógica de aplicación que se ejecuta en una colección de Cosmos DB.

El número de secuencia es una representación de cadena de un número hexadecimal. Puede usar la comparación de cadenas para identificar el número más grande. Si va a convertir la cadena a hexadecimal, el número será de 256 bits. El número de secuencia es estrictamente ascendente por lo que el registro más reciente tendrá un número mayor que el de los otros eventos. Esto resulta útil si tiene conexiones y desconexiones frecuentes de dispositivo y quiere asegurarse de que solo el evento más reciente se usa para desencadenar una acción de nivel inferior, ya que Azure Event Grid no admite la ordenación de eventos.

## <a name="prerequisites"></a>Requisitos previos

* Una cuenta de Azure activa. En caso de no tener ninguna, puede [crear una gratis](https://azure.microsoft.com/pricing/free-trial/).

* Una cuenta activa de la API de SQL de Azure Cosmos DB. Si aún no la ha creado, consulte [Creación de una cuenta de base de datos](../cosmos-db/create-sql-api-dotnet.md#create-an-azure-cosmos-db-account) para ver un tutorial.

* Una colección en la base de datos. Consulte [Incorporación de una colección](../cosmos-db/create-sql-api-dotnet.md#add-a-database-and-a-collection) para ver un tutorial. Al crear la colección, use `/id` para la clave de partición.

* Una instancia de IoT Hub en Azure. Si aún no ha creado ninguna, vea un tutorial en [Introducción a Azure IoT Hub](iot-hub-csharp-csharp-getstarted.md).

## <a name="create-a-stored-procedure"></a>Creación de un procedimiento almacenado

En primer lugar, cree un procedimiento almacenado y configúrelo para ejecutar una lógica que compare los números de secuencia de los eventos de entrada y que registre el evento más reciente por dispositivo en la base de datos.

1. En la API de SQL de Cosmos DB, seleccione **Explorador de datos** > **Elementos** > **Nuevo procedimiento almacenado**.

   ![Crear procedimiento almacenado](./media/iot-hub-how-to-order-connection-state-events/create-stored-procedure.png)

2. Escriba **LatestDeviceConnectionState** para el Id. de procedimiento almacenado y pegue lo siguiente en el **cuerpo del procedimiento almacenado**. Tenga en cuenta que este código debe reemplazar cualquier código ya existente en el cuerpo del procedimiento almacenado. Este código incluye una fila por cada identificador de dispositivo y registra el último estado de conexión de ese identificador de dispositivo mediante la identificación del número de secuencia más alto.

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

1. En el [portal Azure](https://portal.azure.com), seleccione **+ crear un recurso**, seleccione **integración** y, a continuación, **aplicación lógica**.

   ![Creación de la aplicación lógica](./media/iot-hub-how-to-order-connection-state-events/select-logic-app.png)

2. Asigne un nombre a la aplicación lógica que sea único en su suscripción; a continuación, seleccione la misma suscripción, el grupo de recursos y la ubicación en que se encuentra la instancia de IoT Hub.

   ![Nueva aplicación lógica](./media/iot-hub-how-to-order-connection-state-events/new-logic-app.png)

3. Seleccione **Crear** para crear la aplicación lógica.

   Ahora ha creado un recurso de Azure para la aplicación lógica. Después de que Azure implemente la aplicación lógica, el Diseñador de aplicaciones lógicas muestra plantillas de patrones comunes de modo que pueda empezar con mayor rapidez.

   > [!NOTE]
   > Para buscar y vuelva a abrir la aplicación lógica, seleccione **grupos de recursos** y seleccione el grupo de recursos que se usa para este procedimiento. A continuación, seleccione la nueva aplicación lógica. Se abrirá el Diseñador de aplicaciones lógicas.

4. En el Diseñador de aplicaciones lógicas, desplácese a la derecha hasta que vea desencadenadores comunes. En **plantillas**, elija **aplicación lógica en blanco** para que pueda crear la aplicación lógica desde cero.

### <a name="select-a-trigger"></a>Selección de un desencadenador

Un desencadenador es un evento específico que inicia la aplicación lógica. Para este tutorial, el desencadenador que activa el flujo de trabajo recibe una solicitud a través de HTTP.

1. En los conectores y desencadenadores barra de búsqueda, escriba **HTTP** y presione ENTRAR.

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

   ![Carga JSON de ejemplo de pegar](./media/iot-hub-how-to-order-connection-state-events/paste-sample-payload.png)

5. Puede recibir una notificación emergente que dice: **Recuerde incluir un encabezado Content-Type establecido en application/json en la solicitud.** Puede ignorar esta sugerencia sin ningún problema y pasar a la sección siguiente.

### <a name="create-a-condition"></a>Creación de una condición

En el flujo de trabajo de la aplicación lógica, las condiciones ayudan a ejecutar acciones concretas cuando se produce una condición específica. Una vez que se cumple la condición, se puede definir una acción deseada. En este tutorial, la condición es comprobar si eventType es Dispositivo conectado o Dispositivo desconectado. La acción consiste en ejecutar el procedimiento almacenado en la base de datos.

1. Seleccione **+ nuevo paso** , a continuación, **integrada**, a continuación, busque y seleccione **condición**. Haga clic en **elegir un valor** y aparecerá un cuadro emergente que muestra el contenido dinámico, los campos que se pueden seleccionar. Rellene los campos tal como se muestra a continuación para ejecutar este procedimiento sólo para los eventos de dispositivo conectado y desconectado de dispositivo:

   * Elija un valor: **eventType** : seleccione esta opción en los campos que aparecen al hacer clic en este campo en el contenido dinámico.
   * Cambio "es igual a" **termina con**.
   * Elija un valor: **nected**.

     ![Rellenar condición](./media/iot-hub-how-to-order-connection-state-events/condition-detail.png)

2. En el **si es true** cuadro de diálogo, haga clic en **agregar una acción**.
  
   ![Agregar acción si es true](./media/iot-hub-how-to-order-connection-state-events/action-if-true.png)

3. Busque Cosmos DB y seleccione **Azure Cosmos DB: el procedimiento almacenado**

   ![Buscar CosmosDB](./media/iot-hub-how-to-order-connection-state-events/cosmosDB-search.png)

4. Rellene **cosmosdb-connection** para el **nombre de la conexión** y seleccione la entrada en la tabla, a continuación, seleccione **crear**. Verá el **Ejecutar procedimiento almacenado** panel. Escriba los valores para los campos:

   **Id. de base de datos**: ToDoList

   **Id. de colección**: Elementos

   **Id. de SPROC**: LatestDeviceConnectionState

5. Seleccione **Agregar nuevo parámetro**. En la lista desplegable que aparece, active las casillas junto a **clave de partición** y **parámetros para el procedimiento almacenado**, a continuación, haga clic en cualquier otro lugar de la pantalla; agregan un campo de valor de clave de partición y un campo para parámetros para el procedimiento almacenado.

   ![rellenar acción de aplicación lógica](./media/iot-hub-how-to-order-connection-state-events/logicapp-stored-procedure.png)

6. Ahora puede escribir el valor de clave de partición y los parámetros como se muestra a continuación. Asegúrese de colocar en los corchetes y comillas dobles como se muestra. Tendrá que hacer clic en **agregar contenido dinámico** para obtener los valores válidos que puede usar aquí.

   ![rellenar acción de aplicación lógica](./media/iot-hub-how-to-order-connection-state-events/logicapp-stored-procedure-2.png)

7. En la parte superior del panel, donde dice **para cada**, en **seleccionar una salida de los pasos anteriores**, asegúrese de que **cuerpo** está seleccionada.

   ![rellenar para cada aplicación de lógica](./media/iot-hub-how-to-order-connection-state-events/logicapp-foreach-body.png)

8. Guarde la aplicación lógica.

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

3. Seleccione **+ suscripción de eventos**.

   ![Crear una suscripción de eventos](./media/iot-hub-how-to-order-connection-state-events/event-subscription.png)

4. Rellene **detalles de la suscripción de evento**: Proporcione un nombre descriptivo y seleccione **Esquema de Event Grid**.

5. Rellene el **tipos de evento** campos. En la lista desplegable, seleccione sólo **dispositivo conectado** y **dispositivo desconectado** en el menú. Haga clic en cualquier otro lugar de la pantalla para cerrar la lista y guardar sus selecciones.

   ![Establecer tipos de evento que se busca](./media/iot-hub-how-to-order-connection-state-events/set-event-types.png)

6. Para **los detalles del extremo**, seleccione el tipo de punto de conexión como **Webhook** y haga clic en Seleccionar punto de conexión y pegue la dirección URL que copió de la aplicación lógica y confirme la selección.

   ![Seleccione la dirección url del extremo](./media/iot-hub-how-to-order-connection-state-events/endpoint-select.png)

7. El formulario debe parecerse al ejemplo siguiente:

   ![Formulario de ejemplo de suscripción de eventos](./media/iot-hub-how-to-order-connection-state-events/subscription-form.png)

   Seleccione **Crear** para guardar la suscripción de eventos.

## <a name="observe-events"></a>Observación de eventos

Ahora que la suscripción de eventos se ha configurado, vamos a probar a conectar un dispositivo.

### <a name="register-a-device-in-iot-hub"></a>Registro de un dispositivo en IoT Hub

1. En IoT Hub, seleccione **Dispositivos IoT**.

2. Seleccione **+ agregar** en la parte superior del panel.

3. En **Id. de dispositivo**, escriba `Demo-Device-1`.

4. Seleccione **Guardar**.

5. Puede agregar varios dispositivos con distintos identificadores de dispositivo.

   ![Agregado al centro de dispositivos](./media/iot-hub-how-to-order-connection-state-events/AddIoTDevice.png)

6. Haga clic en el dispositivo nuevo; Ahora las teclas y cadenas de conexión se rellenará. Copie el valor de **Cadena de conexión: clave principal** para usarlo más adelante.

   ![ConnectionString de dispositivo](./media/iot-hub-how-to-order-connection-state-events/DeviceConnString.png)

### <a name="start-raspberry-pi-simulator"></a>Inicio del simulador de Raspberry Pi

Vamos a usar el simulador web de Raspberry Pi para simular la conexión del dispositivo.

[Iniciar simulador de Raspberry Pi](https://azure-samples.github.io/raspberry-pi-web-simulator/#Getstarted)

### <a name="run-a-sample-application-on-the-raspberry-pi-web-simulator"></a>Ejecución de una aplicación de ejemplo en el simulador web de Raspberry Pi

Esto desencadenará un evento de dispositivo conectado.

1. En el área de codificación, reemplace el marcador de posición de la línea 15 con la cadena de conexión de dispositivo Azure IoT Hub que guardó al final de la sección anterior.

   ![Pegue en la cadena de conexión de dispositivo](./media/iot-hub-how-to-order-connection-state-events/raspconnstring.png)

2. Ejecute la aplicación seleccionando **ejecutar**.

Verá algo parecido a lo siguiente que muestra los datos del sensor y los mensajes que se envían a IoT hub.

   ![Ejecución de la aplicación](./media/iot-hub-how-to-order-connection-state-events/raspmsg.png)

   Haga clic en **Detener** para detener el simulador y desencadenar un evento de **Dispositivo desconectado**.

Ahora ya ha ejecutado una aplicación de ejemplo para recopilar datos de un sensor y enviarlos a IoT Hub.

### <a name="observe-events-in-cosmos-db"></a>Observación de eventos en Cosmos DB

Puede ver los resultados del procedimiento almacenado ejecutado en el documento de Cosmos DB. Se ven de la siguiente forma. Cada fila contiene el estado de conexión de dispositivo más reciente por cada dispositivo.

   ![Resultado del procedimiento](./media/iot-hub-how-to-order-connection-state-events/cosmosDB-outcome.png)

## <a name="use-the-azure-cli"></a>Uso de la CLI de Azure

En lugar de usar [Azure Portal](https://portal.azure.com), puede llevar a cabo los pasos de IoT Hub mediante la CLI de Azure. Para más información, consulte las páginas de la CLI de Azure para [crear una suscripción de eventos](https://docs.microsoft.com/cli/azure/eventgrid/event-subscription) y [crear un dispositivo IoT](/cli/azure/ext/azure-cli-iot-ext/iot/hub/device-identity#ext-azure-cli-iot-ext-az-iot-hub-device-identity-create).

## <a name="clean-up-resources"></a>Limpieza de recursos

En este tutorial se usaron recursos que generan gastos en su suscripción de Azure. Cuando haya terminado de probar el tutorial y las pruebas, los resultados de deshabilitar o eliminar los recursos que no desea conservar.

Si no desea perder el trabajo realizado en la aplicación lógica, deshabilítela en lugar de eliminarla.

1. Vaya a la aplicación lógica.

2. En la hoja **Información general**, seleccione **Eliminar** o **Deshabilitar**.

    Cada suscripción solo puede tener una instancia de IoT Hub gratuita. Si creó una instancia de IoT Hub gratis para este tutorial, no es necesario eliminarla para evitar cargos.

3. Vaya a su instancia de IoT Hub.

4. En la página **Información general**, seleccione **Eliminar**.

    Aunque conserve la instancia de IoT Hub, puede que desee eliminar la suscripción de eventos que creó.

5. En la instancia de IoT Hub, seleccione **Event Grid**.

6. Seleccione la suscripción de eventos que desea eliminar.

7. Seleccione **Eliminar**.

Para quitar una cuenta de Azure Cosmos DB de Azure Portal, haga clic con el botón derecho en el nombre de la cuenta y haga clic en **Eliminar cuenta**. Consulte las instrucciones detalladas para [eliminar una cuenta de Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/manage-account).

## <a name="next-steps"></a>Pasos siguientes

* Obtenga más información sobre [reacción a eventos de IoT Hub mediante Event Grid para desencadenar acciones](../iot-hub/iot-hub-event-grid.md).

* [Pruebe el tutorial de eventos de IoT Hub](../event-grid/publish-iot-hub-events-to-logic-apps.md)

* Obtenga información sobre qué más puede hacer con [Event Grid](../event-grid/overview.md).