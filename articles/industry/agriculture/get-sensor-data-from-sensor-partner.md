---
title: Obtención de datos de sensor de los asociados
description: Se describe cómo obtener datos de sensor de los asociados.
author: uhabiba04
ms.topic: article
ms.date: 11/04/2019
ms.author: v-umha
ms.openlocfilehash: 380c67e5aeaba9be60e016f173e4da127e4e5f14
ms.sourcegitcommit: 018e3b40e212915ed7a77258ac2a8e3a660aaef8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/07/2019
ms.locfileid: "73797385"
---
# <a name="get-sensor-data-from-sensor-partners"></a>Obtención de datos de sensor de los asociados de sensor

Azure FarmBeats le ayuda a introducir los datos de streaming de los dispositivos y sensores de IoT en un centro de datos. Actualmente, se admiten los siguientes asociados de dispositivos de sensor:

  ![Proyecto de FarmBeats](./media/get-sensor-data-from-sensor-partner/partner-information-1.png)

La integración de los datos de los dispositivos con Azure FarmBeats ayuda a mover los datos del suelo de los sensores de IoT implementados en la granja al centro de datos. Una vez disponibles, estos datos se pueden visualizar mediante el acelerador de FarmBeats y se pueden usar para la fusión de datos y la creación de modelos de IA y ML con FarmBeats.

Para iniciar la transmisión de los datos del sensor, compruebe lo siguiente:

-  Ha instalado FarmBeats desde Azure Marketplace.
-  Ha decidido sobre los sensores y los dispositivos que quiere instalar en la granja.
-  Si tiene previsto usar sensores de humedad del suelo, puede emplear el mapa de colocación de sensores de humedad del suelo de FarmBeats para obtener una recomendación sobre el número de sensores y dónde deben colocarse exactamente. Para más información, consulte [Generación de mapas](generate-maps.md).

- Compre e implemente dispositivos y sensores de su asociado de dispositivo de la granja. Asegúrese de que pueda acceder a los datos del sensor a través de la solución de los asociados de dispositivo.

### <a name="enable-device-integration-with-farmbeats"></a>Habilitación de la integración de dispositivos con FarmBeats   

Cuando haya iniciado la transmisión de los datos del sensor, puede comenzar el proceso de introducir los datos en el sistema FarmBeats. Para habilitar la integración con FarmBeats, debe proporcionar la siguiente información al proveedor de datos:  

 - Punto de conexión de API  
 - Id. de inquilino  
 - Id. de cliente  
 - Secreto del cliente  
 - Cadena de conexión de EventHub

El integrador de sistemas le proporciona la información anterior. Si tiene algún problema al habilitar las integraciones de dispositivos, póngase en contacto con él.

También puede generar las credenciales mediante la ejecución de este script desde Azure Cloud Shell. Siga los pasos siguientes:

1. Descargue el [archivo ZIP](https://aka.ms/farmbeatspartnerscript) y extráigalo en la unidad local. Encontrará dos archivos dentro del archivo ZIP.
2. Inicie sesión en https://portal.azure.com/ y abra Cloud Shell (esta opción está disponible en la barra superior derecha del portal).  

    ![Proyecto de FarmBeats](./media/get-drone-imagery-from-drone-partner/navigation-bar-1.png)

3. Asegúrese de que el entorno está establecido en **PowerShell** (el entorno predeterminado es Bash).

    ![Proyecto de FarmBeats](./media/get-sensor-data-from-sensor-partner/power-shell-new-1.png)

4. Cargue los dos archivos (del paso 1 anterior) en Cloud Shell.

    ![Proyecto de FarmBeats](./media/get-sensor-data-from-sensor-partner/power-shell-two-1.png)

5. Vaya al directorio donde se cargaron los archivos (de forma predeterminada, se cargan en el directorio particular > nombre de usuario).
6. Ejecute el siguiente script:

    ```azurepowershell-interactive 

    PS> ./generateCredentials.ps1   

    ```
7. Siga las instrucciones en pantalla para capturar los valores. (punto de conexión de API, identificador de inquilino, identificador de cliente, secreto de cliente y cadena de conexión de EventHub).

**Integración de los datos de los dispositivos con las credenciales generadas**

Visite el portal de asociados de dispositivo para vincular FarmBeats con el conjunto de credenciales que generó en la sección anterior.

 - Punto de conexión de API  
 - Cadena de conexión de EventHub  
 - Id. de cliente  
 - Secreto del cliente  
 - Id. de inquilino  

 El proveedor del dispositivo confirma una integración correcta. Tras la confirmación, puede ver todos los dispositivos y sensores en Azure FarmBeats.

## <a name="view-devices-and-sensors"></a>Visualización de dispositivos y sensores

Use la sección siguiente para ver los dispositivos y sensores de la granja.

### <a name="view-devices"></a>Vista de dispositivos

Actualmente, FarmBeats admite los siguientes dispositivos:

- **Nodo**: dispositivo al que se conectan uno o varios sensores.
- **Puerta de enlace**: un dispositivo al que están conectados uno o varios sensores.

Para ello, siga los pasos que se describen a continuación:

1. En la página principal, seleccione **Devices** (Dispositivos) en el menú.
  En esta página se muestra el tipo de dispositivo, el modelo, el estado, la granja en la que se encuentra y la fecha de la última actualización de los metadatos. De forma predeterminada, la columna de la granja está establecida en NULL. Puede elegir asignar un dispositivo a una granja. Para más información, consulte [Asignación de dispositivos](#assign-devices).
2. Seleccione el dispositivo para ver sus propiedades, la telemetría y los dispositivos secundarios conectados a él.  

    ![Proyecto de FarmBeats](./media/get-sensor-data-from-sensor-partner/view-devices-1.png)

### <a name="view-sensors"></a>Visualización de los sensores

Para ello, siga los pasos que se describen a continuación:

1. En la página principal, seleccione **Sensors** (Sensores) en el menú.
  La página de sensores muestra detalles sobre el tipo de sensor, la granja a la que está conectado, el dispositivo principal, el nombre del puerto, el tipo de puerto y el último estado actualizado.
2. Seleccione el sensor para ver sus propiedades, alertas activas y telemetría.

    ![Proyecto de FarmBeats](./media/get-sensor-data-from-sensor-partner/view-sensors-1.png)

## <a name="assign-devices"></a>Asignación de dispositivos  

Una vez que fluyan los datos del sensor, puede asignarlos a la granja en la que ha implementado estos sensores.

1. En la página principal, seleccione **Farms** (Granjas) en el menú; se muestra la página de lista **Farms** (Granjas).  
2. Seleccione la granja a la que quiere asignar el dispositivo y seleccione **Add Devices** (Agregar dispositivos).  
3. Aparece la ventana **Add Devices** (Agregar dispositivos). Seleccione el dispositivo que quiere asignar a la granja.

    ![Proyecto de FarmBeats](./media/get-sensor-data-from-sensor-partner/add-devices-1.png)

4. Seleccione **Add Devices** (Agregar dispositivos). También puede ir al menú **Devices** (Dispositivos), seleccionar los dispositivos que quiere asignar a una granja y elegir **Associate Devices** (Asociar dispositivos).  
5. En la ventana **Associate Devices**, seleccione la granja del menú desplegable y elija **Apply to All** (Aplicar a todos) para asociar la granja con todos los dispositivos seleccionados.

    ![Proyecto de FarmBeats](./media/get-sensor-data-from-sensor-partner/associate-devices-1.png)

6. Para asociar cada dispositivo a otra granja, seleccione la lista desplegable de la columna **Assign to Farm** (Asignar a granja) y elija una granja para cada fila de dispositivo.  
7. Seleccione **Assign** (Asignar) para finalizar la asignación de dispositivos.

### <a name="visualize-sensor-data"></a>Visualización de los datos del sensor

Para ello, siga los pasos que se describen a continuación:

1. En la página principal, seleccione **Farms** (Granjas) en el menú para ver la página **Farms** (Granjas).  
2. Seleccione la **granja** para la que quiere ver los datos del sensor.  
3. En el panel **Farm** (Granja), puede ver los datos de telemetría. Puede optar por ver la telemetría activa o usar **Custom Range** (Intervalo personalizado) para ver un intervalo de fechas específico.

    ![Proyecto de FarmBeats](./media/get-sensor-data-from-sensor-partner/telemetry-data-1.png)

## <a name="delete-sensor"></a>Eliminación del sensor

Siga estos pasos:

1. En la página `principal, seleccione **Sensors** (Sensores) en el menú para ver la página **Sensors** (Sensores).  
2. Seleccione el dispositivo que quiere eliminar y elija **Delete** (Eliminar) en la ventana de confirmación.

    ![Proyecto de FarmBeats](./media/get-sensor-data-from-sensor-partner/delete-sensors-1.png)

Un mensaje de confirmación muestra que el sensor se ha eliminado correctamente.  

## <a name="delete-devices"></a>Eliminar dispositivos

Siga estos pasos:

1. En la página principal, seleccione **Devices** (Dispositivos) en el menú para ver la página de dispositivos.  
2. Seleccione el dispositivo que quiere eliminar y elija **Delete** (Eliminar) en la ventana de confirmación.

    ![Proyecto de FarmBeats](./media/get-sensor-data-from-sensor-partner/delete-device-1.png)

## <a name="next-steps"></a>Pasos siguientes

Ahora los datos del sensor fluyen a la instancia de Azure FarmBeats. A continuación, aprenda a [generar mapas](generate-maps.md#generate-maps) para las granjas.
