---
title: Obtención de datos de sensor de los asociados
description: En este artículo se describe cómo obtener datos de sensor de los asociados.
author: uhabiba04
ms.topic: article
ms.date: 11/04/2019
ms.author: v-umha
ms.openlocfilehash: 9364c344c58d17f9f6e6404dd8aa850af032cee9
ms.sourcegitcommit: b95983c3735233d2163ef2a81d19a67376bfaf15
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/11/2020
ms.locfileid: "77138370"
---
# <a name="get-sensor-data-from-sensor-partners"></a>Obtención de datos de sensor de los asociados de sensor

Azure FarmBeats le ayuda a introducir los datos de streaming de los dispositivos y sensores de IoT en un centro de datos. Actualmente, se admiten los siguientes asociados de dispositivos de sensor.

  ![Asociados de FarmBeats](./media/get-sensor-data-from-sensor-partner/partner-information-2.png)

La integración de los datos de los dispositivos con Azure FarmBeats ayuda a mover los datos del suelo de los sensores de IoT implementados en la granja al centro de datos. Los datos, una vez disponibles, se pueden visualizar mediante el acelerador de FarmBeats. Los datos se pueden usar para la fusión de datos y la creación de modelos de inteligencia artificial o de aprendizaje automático (ML/AI) mediante FarmBeats.

Para iniciar la transmisión de los datos del sensor, compruebe lo siguiente:

-  Ha instalado FarmBeats en Azure Marketplace.
-  Ha decidido sobre los sensores y los dispositivos que quiere instalar en la granja.
-  Si tiene previsto usar sensores de humedad del suelo, utilice el mapa de colocación de sensores de humedad del suelo de FarmBeats para obtener una recomendación sobre el número de sensores y dónde deben colocarse exactamente. Para más información, consulte [Generación de mapas](generate-maps-in-azure-farmbeats.md).
- Compre e implemente dispositivos o sensores de su asociado de dispositivo en la granja. Asegúrese de que puede acceder a los datos del sensor a través de la solución de los asociados de dispositivo.

## <a name="enable-device-integration-with-farmbeats"></a>Habilitación de la integración de dispositivos con FarmBeats

Tras iniciar la transmisión de los datos del sensor, puede comenzar el proceso de introducir los datos en el sistema FarmBeats. Para habilitar la integración con FarmBeats, proporcione la siguiente información al proveedor de datos:

 - Punto de conexión de API
 - Id. de inquilino
 - Id. de cliente
 - Secreto del cliente
 - Cadena de conexión de EventHub

Para generar la información anterior siga estos pasos: (Tenga en cuenta que estos pasos se deben realizar en Azure, por lo que necesitará acceso a la suscripción de Azure en la que se implementó FarmBeats)

1. Descargue el [archivo zip](https://aka.ms/farmbeatspartnerscriptv2) y extráigalo en la unidad local. Habrá un archivo dentro del archivo ZIP.

2. Inicie sesión en https://portal.azure.com/.

3. **Si está en la versión 1.2.7 o posterior de FarmBeats, omita los pasos 3a, 3b y 3c y vaya al paso 4.** . Para comprobar la versión de FarmBeats, haga clic en el icono de configuración en la parte superior derecha de la interfaz de usuario de FarmBeats.

3a. Vaya a Azure Active Directory -> Registro de aplicaciones.

3b. Haga clic en el registro de la aplicación que se ha creado como parte de la implementación de FarmBeats. Tendrá el mismo nombre que el centro de datos de FarmBeats.

3c. Haga clic en "Exponer una API" -> "Agregar una aplicación cliente", escriba **04b07795-8ddb-461a-bbee-02f9e1bf7b46** y active "Authorize Scope" (Autorizar ámbito). De esta forma, se proporciona acceso a la CLI de Azure (Cloud Shell) para realizar los pasos siguientes.

4. Abra Cloud Shell. Esta opción está disponible en la barra de herramientas de la esquina superior derecha de Azure Portal.

    ![Barra de herramientas de Azure Portal](./media/get-drone-imagery-from-drone-partner/navigation-bar-1.png)

5. Asegúrese de que el entorno esté establecido en **PowerShell**. De forma predeterminada, se establece en Bash.

    ![Valor de la barra de herramientas de PowerShell](./media/get-sensor-data-from-sensor-partner/power-shell-new-1.png)

6. Cargue el archivo del paso 1 en la instancia de Cloud Shell.

    ![Botón Cargar de la barra de herramientas](./media/get-sensor-data-from-sensor-partner/power-shell-two-1.png)

7. Vaya al directorio donde se ha cargado el archivo. De forma predeterminada, los archivos se cargan en el directorio particular bajo el nombre de usuario.

8. Ejecute el siguiente script. El script solicita el identificador de inquilino, que se puede obtener en la página Azure Active Directory -> Información general.

    ```azurepowershell-interactive 

    ./generatePartnerCredentials.ps1   

    ```

9. Siga las instrucciones en pantalla para capturar los valores del **punto de conexión de API**, el **identificador de inquilino**, el **identificador de cliente**, el **secreto de cliente** y la **cadena de conexión de EventHub**.

### <a name="integrate-device-data-by-using-the-generated-credentials"></a>Integración de los datos de los dispositivos con las credenciales generadas

Ahora tiene la siguiente información generada desde la sección anterior.
 - Punto de conexión de API
 - Cadena de conexión de EventHub
 - Id. de cliente
 - Secreto del cliente
 - Id. de inquilino
 
Deberá proporcionar esta información a su asociado de dispositivo para la vinculación con FarmBeats. Vaya al portal de asociados de dispositivos para hacer lo mismo. Por ejemplo, en caso de que use dispositivos de Davis Instruments, vaya a la página siguiente:

[Davis Instruments](https://weatherlink.github.io/azure-farmbeats/setup)

 El proveedor del dispositivo confirma una integración correcta. Tras la confirmación, puede ver todos los dispositivos y sensores en Azure FarmBeats.

## <a name="view-devices-and-sensors"></a>Visualización de dispositivos y sensores

Use la sección siguiente para ver los dispositivos y sensores de la granja.

### <a name="view-devices"></a>Vista de dispositivos

Actualmente, FarmBeats admite los siguientes dispositivos:

- **Nodo**: dispositivo al que se conectan uno o varios sensores.
- **Puerta de enlace**: dispositivo al que están conectados uno o varios nodos.

Siga estos pasos.

1. En la página principal, seleccione **Devices** (Dispositivos) en el menú.
  En la página **Devices** (Dispositivos) se muestra el tipo de dispositivo, el modelo, el estado, la granja en la que se encuentra y la fecha de la última actualización de los metadatos. De forma predeterminada, la columna de la granja está establecida en *NULL*. Puede elegir asignar un dispositivo a una granja. Para más información, consulte [Asignación de dispositivos](#assign-devices).
2. Seleccione el dispositivo para ver sus propiedades, la telemetría y los dispositivos secundarios conectados a él.

    ![Página de dispositivos](./media/get-sensor-data-from-sensor-partner/view-devices-1.png)

### <a name="view-sensors"></a>Visualización de los sensores

Siga estos pasos.

1. En la página principal, seleccione **Sensors** (Sensores) en el menú.
  La página **Sensors** (Sensores) muestra detalles sobre el tipo de sensor, la granja a la que está conectado, el dispositivo principal, el nombre del puerto, el tipo de puerto y el último estado actualizado.
2. Seleccione el sensor para ver sus propiedades, alertas activas y telemetría.

    ![Página Sensors (Sensores)](./media/get-sensor-data-from-sensor-partner/view-sensors-1.png)

## <a name="assign-devices"></a>Asignación de dispositivos  

Una vez que fluyan los datos del sensor, puede asignarlos a la granja en la que ha implementado estos sensores.

1. En la página principal, seleccione **Farms** (Granjas) en el menú. Aparece la página de lista **Farms** (Granjas).
2. Seleccione la granja a la que quiere asignar el dispositivo y seleccione **Add Devices** (Agregar dispositivos).
3. Aparece la ventana **Add Devices** (Agregar dispositivos). Seleccione el dispositivo que quiere asignar a la granja.

    ![Ventana Add Devices (Agregar dispositivos)](./media/get-sensor-data-from-sensor-partner/add-devices-1.png)

4. Seleccione **Add Devices** (Agregar dispositivos). También puede ir al menú **Devices** (Dispositivos), seleccionar los dispositivos que quiere asignar a una granja y elegir **Associate Devices** (Asociar dispositivos).
5. En la ventana **Associate Devices** (Asociar dispositivos), seleccione la granja de la lista desplegable y elija **Apply to All** (Aplicar a todos) para asociar la granja con todos los dispositivos seleccionados.

    ![Ventana Associate Devices (Asociar dispositivos)](./media/get-sensor-data-from-sensor-partner/associate-devices-1.png)

6. Para asociar cada dispositivo a otra granja, seleccione la flecha desplegable de la columna **Assign to Farm** (Asignar a granja) y elija una granja para cada fila de dispositivo.
7. Seleccione **Assign** (Asignar) para finalizar la asignación de dispositivos.

### <a name="visualize-sensor-data"></a>Visualización de los datos del sensor

Siga estos pasos.

1. En la página principal, seleccione **Farms** (Granjas) en el menú para ver la página **Farms** (Granjas).
2. Seleccione la **granja** para la que quiere ver los datos del sensor.
3. En el panel **Farm** (Granja), puede ver los datos de telemetría. Puede ver la telemetría activa o usar **Custom Range** (Intervalo personalizado) para ver un intervalo de fechas específico.

    ![Panel Farm (Granja)](./media/get-sensor-data-from-sensor-partner/telemetry-data-1.png)

## <a name="delete-a-sensor"></a>Eliminación de un sensor

Siga estos pasos.

1. En la página `principal, seleccione **Sensors** (Sensores) en el menú para ver la página **Sensors** (Sensores).
2. Seleccione el dispositivo que quiere eliminar y elija **Delete** (Eliminar) en la ventana de confirmación.

    ![Botón Eliminar](./media/get-sensor-data-from-sensor-partner/delete-sensors-1.png)

Un mensaje de confirmación muestra que el sensor se ha eliminado correctamente.

## <a name="delete-devices"></a>Eliminar dispositivos

Siga estos pasos.

1. En la página principal, seleccione **Devices** (Dispositivos) en el menú para ver la página **Devices** (Dispositivos).
2. Seleccione el dispositivo que quiere eliminar y elija **Delete** (Eliminar) en la ventana de confirmación.

    ![Botón Eliminar](./media/get-sensor-data-from-sensor-partner/delete-device-1.png)

## <a name="next-steps"></a>Pasos siguientes

Ahora los datos del sensor fluyen a la instancia de Azure FarmBeats. A continuación, aprenda a [generar mapas](generate-maps-in-azure-farmbeats.md#generate-maps) para las granjas.
