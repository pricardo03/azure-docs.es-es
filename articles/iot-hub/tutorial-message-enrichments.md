---
title: 'Tutorial: uso de grado de enriquecimiento del mensaje de Azure IoT Hub'
description: Tutorial que muestra cómo utilizar el grado de enriquecimiento del mensaje para los mensajes de IoT Hub de Azure
author: robinsh
manager: philmea
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 05/10/2019
ms.author: robinsh
ms.openlocfilehash: e4906bf9f2aead69c315ddb7b2e3b10489378d87
ms.sourcegitcommit: 25a60179840b30706429c397991157f27de9e886
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/28/2019
ms.locfileid: "66259080"
---
# <a name="tutorial-using-azure-iot-hub-message-enrichments-preview"></a>Tutorial: Uso de enriquecimientos de mensaje de Azure IoT Hub (versión preliminar)

*Mensaje enriquecimientos* es la capacidad de IoT Hub a *marca* mensajes con información adicional antes de que los mensajes se envían al punto de conexión designado. Una razón para utilizar el grado de enriquecimiento del mensaje es incluir datos que se pueden usar para simplificar el procesamiento de bajada. Por ejemplo, enriquecimiento de mensajes de telemetría de dispositivo con una etiqueta de dispositivo gemelo puede reducir la carga en los clientes para realizar llamadas de API para obtener esta información de dispositivo gemelo. Para obtener más información, consulte el [información general del grado de enriquecimiento del mensaje](iot-hub-message-enrichments-overview.md).

En este tutorial, use la CLI de Azure para configurar los recursos, incluidos dos puntos de conexión que apuntan a dos contenedores de almacenamiento diferente: **enriquecido** y **original**. A continuación, usar el [portal Azure](https://portal.azure.com) para configurar el grado de enriquecimiento del mensaje que se aplicará solo a los mensajes enviados al punto de conexión con el **enriquecido** contenedor de almacenamiento. Enviar mensajes a IoT Hub, que se enrutan a dos contenedores de almacenamiento. Solo los mensajes enviados al punto de conexión para el **enriquecido** estará enriquecido contenedor de almacenamiento.

Estas son las tareas que llevará a cabo para completar este tutorial:

**Uso de grado de enriquecimiento del mensaje de instancia de IoT Hub**
> [!div class="checklist"]
> * Con la CLI de Azure, cree los recursos, un centro de IoT, una cuenta de almacenamiento con dos puntos de conexión y la configuración de enrutamiento.
> * Use el portal de Azure para configurar el grado de enriquecimiento del mensaje.
> * Ejecutar una aplicación que simula un dispositivo IoT envía mensajes al centro.
> * Ver los resultados y compruebe que el grado de enriquecimiento del mensaje funcionan según lo previsto.

## <a name="prerequisites"></a>Requisitos previos

* Debe tener una suscripción de Azure. Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

* Instale [Visual Studio](https://www.visualstudio.com/).

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="retrieve-the-sample-code"></a>Recuperar el código de ejemplo

Descargar [IoT Device Simulation](https://github.com/Azure-Samples/azure-iot-samples-csharp/archive/master.zip) y descomprímalo. Este repositorio tiene varias aplicaciones, incluidas las que se usará para enviar mensajes a IoT hub.

Esta descarga también contiene el script para crear los recursos que se usa para probar el grado de enriquecimiento del mensaje. El script está en /azure-iot-samples-csharp/iot-hub/Tutorials/Routing/SimulatedDevice/resources/iothub_msgenrichment_cli.azcli. Por ahora, puede mirar la secuencia de comandos y usarlo. También puede copiar el script directamente desde el artículo.

Cuando esté listo para comenzar a probar, usará la aplicación de simulación de dispositivo desde esta descarga para enviar mensajes a IoT hub.

## <a name="set-up-and-configure-resources"></a>Instalar y configurar los recursos

Además de crear los recursos necesarios, el script de CLI de Azure también configura las dos rutas a los puntos de conexión que son contenedores de almacenamiento independiente. Para obtener más información sobre cómo configurar el enrutamiento, consulte el [tutorial enrutamiento](tutorial-routing.md). Después de configuran los recursos, use el [portal Azure](https://portal.azure.com) para configurar el grado de enriquecimiento del mensaje para cada punto de conexión y, a continuación, continúe con el paso de prueba.

> [!NOTE]
> Todos los mensajes se enrutan a ambos puntos de conexión, pero sólo los mensajes que pasen al punto de conexión con el grado de enriquecimiento del mensaje configurado estará enriquecidos.
>

Puede usar el siguiente script o abra el script en la carpeta Resources del repositorio descargado. Estos son los pasos que se llevará a cabo la secuencia de comandos:

* Cree un centro de IoT Hub.
* Cree una cuenta de almacenamiento.
* Cree dos contenedores en la cuenta de almacenamiento: uno para los mensajes enriquecidos y otro para los mensajes que no se complementan.
* Configurar el enrutamiento para las dos cuentas de almacenamiento diferente.
    * Crear un punto de conexión para cada contenedor de la cuenta de almacenamiento.
    * Cree una ruta a cada uno de los puntos de conexión del contenedor de cuenta de almacenamiento.

Hay varios nombres de recurso que deben ser únicos globalmente, como el nombre de IoT Hub y el nombre de la cuenta de almacenamiento. Para hacer más sencillo el funcionamiento de la secuencia de comandos, se anexan los nombres de los recursos con un valor alfanumérico aleatorio denominado *randomValue*. El valor RandomValue se genera una vez en la parte superior del script y se anexa a los nombres de los recursos según sea necesario en el script. Si no desea que sea aleatorio, puede establecerlo en una cadena vacía o en un valor específico.

Si aún no lo ha hecho, abra un [ventana Cloud Shell de Bash.](https://shell.azure.com). Abra el script en el repositorio sin comprimir, use Ctrl-A para seleccionar todo y, después, CTRL+c para copiarlo. Como alternativa, puede copiar el siguiente script CLI o abrirlo directamente en cloud shell. Pegue el script en la ventana de shell de la nube de Azure con el botón secundario en la línea de comandos y seleccionando **pegar**. El script se ejecuta una instrucción a la vez. Una vez que el script deja de ejecutarse, seleccione **ENTRAR** para asegurarse de que se ejecuta el último comando. El bloque de código siguiente muestra la secuencia de comandos que se usa con los comentarios que explican lo que está haciendo.

Estos son los recursos creados por la secuencia de comandos. **Enriquecido** significa que el recurso es para los mensajes con enriquecimientos. **Original** significa que el recurso es para los mensajes que no se complementan.

| NOMBRE | Valor |
|-----|-----|
| resourceGroup | ContosoResourcesMsgEn |
| Nombre del contenedor | Texto original en  |
| Nombre del contenedor | enriquecido  |
| Nombre de dispositivo de IoT | Dispositivo de prueba de Contoso |
| Nombre de IoT Hub | ContosoTestHubMsgEn |
| Nombre de la cuenta de almacenamiento | contosostorage |
| 1 nombre de punto de conexión | ContosoStorageEndpointOriginal |
| punto de conexión nombre 2 | ContosoStorageEndpointEnriched|
| 1 nombre de ruta | ContosoStorageRouteOriginal |
| ruta Name 2 | ContosoStorageRouteEnriched |

```azurecli-interactive
# This command retrieves the subscription id of the current Azure account.
# This field is used when setting up the routing rules.
subscriptionID=$(az account show --query id -o tsv)

# Concatenate this number onto the resources that have to be globally unique.
# You can set this to "" or to a specific value if you don't want it to be random.
# This retrieves a random value.
randomValue=$RANDOM

# This command installs the IOT Extension for Azure CLI.
# You only need to install this the first time.
# You need it to create the device identity.
az extension add --name azure-cli-iot-ext

# Set the values for the resource names that
#   don't have to be globally unique.
location=westus2
resourceGroup=ContosoResourcesMsgEn
containerName1=original
containerName2=enriched
iotDeviceName=Contoso-Test-Device

# Create the resource group to be used
#   for all the resources for this tutorial.
az group create --name $resourceGroup \
    --location $location

# The IoT hub name must be globally unique,
#   so add a random value to the end.
iotHubName=ContosoTestHubMsgEn$randomValue
echo "IoT hub name = " $iotHubName

# Create the IoT hub.
az iot hub create --name $iotHubName \
    --resource-group $resourceGroup \
    --sku S1 --location $location

# You need a storage account that will have two containers
#   -- one for the original messages and
#   one for the enriched messages.
# The storage account name must be globally unique,
#   so add a random value to the end.
storageAccountName=contosostorage$randomValue
echo "Storage account name = " $storageAccountName

# Create the storage account to be used as a routing destination.
az storage account create --name $storageAccountName \
    --resource-group $resourceGroup \
    --location $location \
    --sku Standard_LRS

# Get the primary storage account key.
#    You need this to create the containers.
storageAccountKey=$(az storage account keys list \
    --resource-group $resourceGroup \
    --account-name $storageAccountName \
    --query "[0].value" | tr -d '"')

# See the value of the storage account key.
echo "storage account key = " $storageAccountKey

# Create the containers in the storage account.
az storage container create --name $containerName1 \
    --account-name $storageAccountName \
    --account-key $storageAccountKey \
    --public-access off

az storage container create --name $containerName2 \
    --account-name $storageAccountName \
    --account-key $storageAccountKey \
    --public-access off

# Create the IoT device identity to be used for testing.
az iot hub device-identity create --device-id $iotDeviceName \
    --hub-name $iotHubName

# Retrieve the information about the device identity, then copy the primary key to
#   Notepad. You need this to run the device simulation during the testing phase.
# If you are using Cloud Shell, you can scroll the window back up to retrieve this value.
az iot hub device-identity show --device-id $iotDeviceName \
    --hub-name $iotHubName

##### ROUTING FOR STORAGE #####

# You're going to have two routes and two endpoints.
# One points to container1 in the storage account
#   and includes all messages.
# The other points to container2 in the same storage account
#   and only includes enriched messages.

endpointType="azurestoragecontainer"
endpointName1="ContosoStorageEndpointOriginal"
endpointName2="ContosoStorageEndpointEnriched"
routeName1="ContosoStorageRouteOriginal"
routeName2="ContosoStorageRouteEnriched"

# for both endpoints, retrieve the messages going to storage
condition='level="storage"'

# Get the connection string for the storage account.
# Adding the "-o tsv" makes it be returned without the default double quotes around it.
storageConnectionString=$(az storage account show-connection-string \
  --name $storageAccountName --query connectionString -o tsv)

# Create the routing endpoints and routes.
# Set the encoding format to either avro or json.

# This is the endpoint for container 1, for endpoint messages that are not enriched.
az iot hub routing-endpoint create \
  --connection-string $storageConnectionString \
  --endpoint-name $endpointName1 \
  --endpoint-resource-group $resourceGroup \
  --endpoint-subscription-id $subscriptionID \
  --endpoint-type $endpointType \
  --hub-name $iotHubName \
  --container $containerName1 \
  --resource-group $resourceGroup \
  --encoding json

# This is the endpoint for container 2, for endpoint messages that are enriched.
az iot hub routing-endpoint create \
  --connection-string $storageConnectionString \
  --endpoint-name $endpointName2 \
  --endpoint-resource-group $resourceGroup \
  --endpoint-subscription-id $subscriptionID \
  --endpoint-type $endpointType \
  --hub-name $iotHubName \
  --container $containerName2 \
  --resource-group $resourceGroup \
  --encoding json

# This is the route for messages that are not enriched.
# Create the route for the first storage endpoint.
az iot hub route create \
  --name $routeName1 \
  --hub-name $iotHubName \
  --source devicemessages \
  --resource-group $resourceGroup \
  --endpoint-name $endpointName1 \
  --enabled \
  --condition $condition

# This is the route for messages that are not enriched.
az iot hub route create \
  --name $routeName2 \
  --hub-name $iotHubName \
  --source devicemessages \
  --resource-group $resourceGroup \
  --endpoint-name $endpointName2 \
  --enabled \
  --condition $condition
```

En este momento, los recursos están establecidos de y se configura el enrutamiento. Puede ver la configuración de enrutamiento de mensajes en el portal y configurar el grado de enriquecimiento del mensaje para los mensajes que se va a la **enriquecido** contenedor de almacenamiento.

### <a name="view-routing-and-configure-the-message-enrichments"></a>Ver el enrutamiento y configurar el grado de enriquecimiento del mensaje

1. Vaya a IoT Hub seleccionando **grupos de recursos**, a continuación, seleccione el grupo de recursos configurado para este tutorial (**ContosoResources_MsgEn**). Busque el centro de IoT en la lista y selecciónela. Seleccione *enrutamiento de mensajes** para el centro de Iot.

   ![Seleccione el enrutamiento de mensajes](./media/tutorial-message-enrichments/select-iot-hub.png)

   El mensaje en el panel de enrutamiento tiene tres pestañas: **rutas**, **puntos de conexión personalizados**, y **enriquecer mensajes**. Puede examinar las dos primeras pestañas para ver la configuración de la configuración de la secuencia de comandos. Utilice la tercera pestaña para agregar el grado de enriquecimiento del mensaje. Vamos a enriquecer los mensajes dirigidos al punto de conexión para el contenedor de almacenamiento llamado **enriquecido**. Rellene el nombre y valor y, a continuación, seleccione el punto de conexión **ContosoStorageEndpointEnriched** en la lista desplegable. Este es un ejemplo de cómo configurar un enriquecimiento que agrega el nombre de IoT Hub al mensaje:

   ![Agregar la primera enriquecimiento](./media/tutorial-message-enrichments/add-message-enrichments.png)

2. Agregue estos valores a la lista para el punto de conexión ContosoStorageEndpointEnriched.

   | NOMBRE | Valor | Punto de conexión (lista desplegable) |
   | ---- | ----- | -------------------------|
   | myIotHub | $iothubname | AzureStorageContainers > ContosoStorageEndpointEnriched |
   | Ubicación del dispositivo | $twin.tags.location | AzureStorageContainers > ContosoStorageEndpointEnriched |
   |customerID | 6ce345b8-1e4a-411e-9398-d34587459a3a | AzureStorageContainers > ContosoStorageEndpointEnriched |

   > [!NOTE]
   > Si el dispositivo no tiene un gemelo, el valor que coloca aquí se marcarán como una cadena para el valor en el grado de enriquecimiento del mensaje. Para ver el dispositivo gemelo información, vaya a su centro en el portal, a continuación, seleccione **dispositivos IoT**, seleccione el dispositivo y, a continuación, seleccione **dispositivo gemelo** en la parte superior de la página.
   >
   > Puede editar la información del gemelo para agregar etiquetas (por ejemplo, la ubicación) y establézcalo en un valor específico si desea. Para más información, consulte [Información y uso de dispositivos gemelos en IoT Hub](iot-hub-devguide-device-twins.md)

3. Cuando haya terminado, el panel debe ser similar a esta imagen:

   ![Tabla con todos los enriquecimientos de agregado](./media/tutorial-message-enrichments/all-message-enrichments.png)

4. Seleccione **aplicar** para guardar los cambios.

## <a name="send-messages-to-the-iot-hub"></a>Enviar mensajes a IoT Hub

Ahora que el grado de enriquecimiento del mensaje se configura para el punto de conexión, ejecute la aplicación de dispositivo simulado para enviar mensajes a IoT Hub. El concentrador se estableció con reglas que realizar lo siguiente:

* Los mensajes se enruten al punto de conexión de almacenamiento ContosoStorageEndpointOriginal no estará enriquecidos y se almacenará en el contenedor de almacenamiento `original`.

* Los mensajes se enruten al punto de conexión de almacenamiento ContosoStorageEndpointEnriched se enriquecido y se almacenará en el contenedor de almacenamiento `enriched`.

La aplicación Simulated Device es una de las aplicaciones en la descarga sin comprimir. La aplicación envía mensajes para cada uno de los métodos de enrutamiento de mensajes diferente en el [Tutorial enrutamiento](tutorial-routing.md); Esto incluye el almacenamiento de Azure.

Haga doble clic en el archivo de la solución (IoT_SimulatedDevice.sln) para abrir el código en Visual Studio y abra Program.cs. Sustituya `{your hub name}` con el nombre del centro de IoT. El formato del nombre de host de IoT hub es **{su nombre de centro} .azure-devices.net**. Para este tutorial, es el nombre de host del centro **ContosoTestHubMsgEn.azure-devices.net**. A continuación, sustituya `{device key}` por la clave de dispositivo que guardó anteriormente al ejecutar el script para crear los recursos.

Si no tiene la clave del dispositivo, puede recuperarlo desde el portal. Después de iniciar sesión, vaya a **grupos de recursos**, seleccione el grupo de recursos y luego seleccione el centro de IoT. Mire en **dispositivos IoT** para el dispositivo de prueba y seleccione el dispositivo. Seleccione el icono de copiar junto a **clave principal** para copiarlo en el Portapapeles.

   ```csharp
        static string myDeviceId = "contoso-test-device";
        static string iotHubUri = "ContosoTestHubMsgEn.azure-devices.net";
        // This is the primary key for the device. This is in the portal.
        // Find your IoT hub in the portal > IoT devices > select your device > copy the key.
        static string deviceKey = "{your device key here}";
   ```

## <a name="run-and-test"></a>Ejecución y prueba

Ejecución de la aplicación de consola. Espere unos minutos. Los mensajes que se envían se muestran en la pantalla de la consola de la aplicación.

La aplicación envía un nuevo mensaje de dispositivo a nube a IoT Hub cada segundo. El mensaje contiene un objeto serializado mediante JSON serializado con el identificador de dispositivo, la temperatura, la humedad y el nivel de mensaje, cuyo valor predeterminado es `normal`. Asigna de forma aleatoria un nivel de `critical` o `storage`, lo que el mensaje se enruta a la cuenta de almacenamiento o al punto de conexión predeterminado. Los mensajes enviados a la **enriquecido** estará enriquecida contenedor en la cuenta de almacenamiento.

Después de han enviado varios mensajes de almacenamiento, ver los datos.

1. Seleccione **grupos de recursos**, a continuación, busque el grupo de recursos (ContosoResourcesMsgEn) y selecciónelo.

2. Seleccione la cuenta de almacenamiento (contosostorage). A continuación, seleccione **Explorador de Storage (versión preliminar)** desde el panel de selección de la izquierda.

   ![Seleccione Explorador de storage](./media/tutorial-message-enrichments/select-storage-explorer.png)

   Seleccione **contenedores de blobs** para ver los dos contenedores que se pueden usar.

   ![Ver los contenedores en la cuenta de almacenamiento](./media/tutorial-message-enrichments/show-blob-containers.png)

Los mensajes en el contenedor llamado **enriquecido** tiene el grado de enriquecimiento del mensaje incluido en los mensajes. Los mensajes en el contenedor llamado **original** tendrán los mensajes sin procesar con ningún enriquecimientos. Explore en profundidad en uno de los contenedores hasta llegar a la parte inferior y abra el archivo de mensaje más reciente y luego lo mismo para el contenedor de otro comprobar que no hay ningún enriquecimientos de agregado a los mensajes de ese contenedor.

Al examinar los mensajes que han sido enriquecidos, debería ver el "Centro de IoT mi" con el nombre del centro, así como la ubicación y el identificador de cliente, similar al siguiente:

```json
{"EnqueuedTimeUtc":"2019-05-10T06:06:32.7220000Z","Properties":{"level":"storage","my IoT Hub":"contosotesthubmsgen3276","device location":"$twin.tags.location","customerID":"6ce345b8-1e4a-411e-9398-d34587459a3a"},"SystemProperties":{"connectionDeviceId":"Contoso-Test-Device","connectionAuthMethod":"{\"scope\":\"device\",\"type\":\"sas\",\"issuer\":\"iothub\",\"acceptingIpFilterRule\":null}","connectionDeviceGenerationId":"636930642531278483","enqueuedTime":"2019-05-10T06:06:32.7220000Z"},"Body":"eyJkZXZpY2VJZCI6IkNvbnRvc28tVGVzdC1EZXZpY2UiLCJ0ZW1wZXJhdHVyZSI6MjkuMjMyMDE2ODQ4MDQyNjE1LCJodW1pZGl0eSI6NjQuMzA1MzQ5NjkyODQ0NDg3LCJwb2ludEluZm8iOiJUaGlzIGlzIGEgc3RvcmFnZSBtZXNzYWdlLiJ9"}
```

Y aquí es un mensaje no acondicionado. "" Mi centro de IoT","Ubicación del dispositivo"y"customerID"no aparecen aquí, ya que este punto de conexión no enriquecimientos.

```json
{"EnqueuedTimeUtc":"2019-05-10T06:06:32.7220000Z","Properties":{"level":"storage"},"SystemProperties":{"connectionDeviceId":"Contoso-Test-Device","connectionAuthMethod":"{\"scope\":\"device\",\"type\":\"sas\",\"issuer\":\"iothub\",\"acceptingIpFilterRule\":null}","connectionDeviceGenerationId":"636930642531278483","enqueuedTime":"2019-05-10T06:06:32.7220000Z"},"Body":"eyJkZXZpY2VJZCI6IkNvbnRvc28tVGVzdC1EZXZpY2UiLCJ0ZW1wZXJhdHVyZSI6MjkuMjMyMDE2ODQ4MDQyNjE1LCJodW1pZGl0eSI6NjQuMzA1MzQ5NjkyODQ0NDg3LCJwb2ludEluZm8iOiJUaGlzIGlzIGEgc3RvcmFnZSBtZXNzYWdlLiJ9"}
```

## <a name="clean-up-resources"></a>Limpieza de recursos

Si desea quitar todos los recursos que ha creado en este tutorial, elimine el grupo de recursos. Esta acción elimina también todos los recursos del grupo. En este caso, se quita el centro de IoT, la cuenta de almacenamiento y el propio grupo de recursos.

### <a name="use-the-azure-cli-to-clean-up-resources"></a>Uso de la CLI de Azure para quitar recursos

Para quitar el grupo de recursos, use el comando [az group delete](https://docs.microsoft.com/cli/azure/group?view=azure-cli-latest#az-group-delete). `$resourceGroup` se estableció en **ContosoResources** al principio de este tutorial.

```azurecli-interactive
az group delete --name $resourceGroup
```

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, configurado y había probado agregando enriquecimientos de mensaje para mensajes de IoT Hub mediante los pasos siguientes:

**Uso de grado de enriquecimiento del mensaje de instancia de IoT Hub**
> [!div class="checklist"]
> * Con la CLI de Azure, cree los recursos, un centro de IoT, una cuenta de almacenamiento con dos enendpoints y la configuración de enrutamiento.
> * Use el portal de Azure para configurar el grado de enriquecimiento del mensaje.
> * Ejecutar una aplicación que simula un mensaje envío de dispositivo IoT al concentrador.
> * Ver los resultados y compruebe que el grado de enriquecimiento del mensaje funcionan según lo previsto.

Para obtener más información sobre el grado de enriquecimiento del mensaje, vea el [información general del grado de enriquecimiento del mensaje](iot-hub-message-enrichments-overview.md).

Para obtener más información sobre el enrutamiento de mensajes, consulte estos artículos:

* [Usar el enrutamiento de mensajes de IoT Hub para enviar mensajes de dispositivo a nube a distintos puntos de conexión](iot-hub-devguide-messages-d2c.md)

* [Tutorial: Enrutamiento de IoT Hub](tutorial-routing.md)