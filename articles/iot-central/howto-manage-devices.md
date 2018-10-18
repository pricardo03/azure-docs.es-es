---
title: Administración de dispositivos en una aplicación de Azure IoT Central | Microsoft Docs
description: Como operador, aprenda a administrar dispositivos en la aplicación de Azure IoT Central.
author: ellenfosborne
ms.author: elfarber
ms.date: 01/21/2018
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: 4b4ded86075e49277bca84f5261b6762b0f4fcae
ms.sourcegitcommit: 1b561b77aa080416b094b6f41fce5b6a4721e7d5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/17/2018
ms.locfileid: "45737318"
---
# <a name="manage-devices-in-your-azure-iot-central-application"></a>Administración de dispositivos en la aplicación de Azure IoT Central

En este artículo se describe cómo administrar dispositivos en su aplicación de Microsoft Azure IoT Central como operador. Como operador, puede hacer lo siguiente:

- Usar la página **Explorer** para ver, agregar y eliminar los dispositivos conectados a la aplicación de Azure IoT Central.
- Mantener un inventario actualizado de los dispositivos.
- Mantener actualizados los metadatos del dispositivo cambiando los valores almacenados en las propiedades del dispositivo.
- Controlar el comportamiento de los dispositivos mediante la actualización de una configuración en un dispositivo específico desde la página **Configuración**.

## <a name="view-your-devices"></a>Visualización de los dispositivos

Para ver un dispositivo determinado:

1. Elija **Explorer** en el menú de navegación de la izquierda. Aquí verá una lista de las [plantillas de dispositivo](howto-set-up-template.md).

1. Elija una **Plantilla de dispositivo** en el panel izquierdo.

1. En el panel derecho, verá una lista de dispositivos creados a partir de esa plantilla de dispositivo. Elija un dispositivo determinado para ver la página **Detalles del dispositivo** para dicho dispositivo:

    [![Página Detalles del dispositivo](./media/howto-manage-devices/image1.png)](./media/howto-manage-devices/image1.png#lightbox)

## <a name="add-a-device"></a>Agregar un dispositivo

Para agregar un dispositivo real a una aplicación de Azure IoT Central:

1. Elija **Explorer** en el menú de navegación de la izquierda.

1. Elija la plantilla de dispositivo desde la que va a crear un dispositivo.

1. Elija + **Nuevo**.

1. Elija **Real** o **Simulado**. Un dispositivo real hace referencia a un dispositivo físico que se conecta a la aplicación de Azure IoT Central. Un dispositivo simulado tiene datos de ejemplo generados por Azure IoT Central, que los pone a su disposición. En este ejemplo se utiliza un dispositivo real. Elija **Real** para navegar hasta la página **Detalles del dispositivo** para el dispositivo nuevo.


## <a name="import-devices"></a>Importación de dispositivos

Para conectar un gran número de dispositivos a la aplicación, Azure IoT Central ofrece importar dispositivos en bloque a través de un archivo CSV. El archivo CSV debe tener las siguientes columnas (y encabezados)
1.  IOTC_DeviceID **<span style="color:Red">(debe estar en minúscula)</span>**
1.  IOTC_DeviceName (opcional)


Para registrar dispositivos en la aplicación en bloque:

1. Elija **Explorer** en el menú de navegación de la izquierda.

1. En el panel izquierdo, elija la plantilla de dispositivo para la que desea crear dispositivos en bloque.

 >   [!NOTE] 
    Si aún no tiene una plantilla de dispositivo, puede importar dispositivos en **Unassociated devices** (Dispositivos no asociados) y registrarlos sin ninguna plantilla. Una vez que se han importado, puede asociarlos con una plantilla como un paso posterior.

1. Haga clic en **Import**.

    [![Acción de importación](./media/howto-manage-devices/BulkImport1.png)](./media/howto-manage-devices/BulkImport1.png#lightbox)

1. Seleccione el archivo CSV que tiene la lista de identificadores de dispositivo que desea importar.

1. La importación de dispositivos se inicia una vez que se ha cargado el archivo. Puede realizar un seguimiento del estado de la importación en la parte superior de la cuadrícula de dispositivo.

1. Una vez finalizada la importación, se muestra un mensaje de confirmación en la cuadrícula de dispositivos.

    [![Importación correcta](./media/howto-manage-devices/BulkImport3.png)](./media/howto-manage-devices/BulkImport3.png#lightbox)

Si se produce un error en la operación de importación de dispositivos, aparecerá un mensaje de error en la cuadrícula de dispositivos. Se genera un archivo de registro que captura todos los errores, que se puede descargar haciendo clic en el mensaje de error.


**Asociación de dispositivos con una plantilla**

Si registra dispositivos mediante la importación en **Unassociated devices** (Dispositivos no asociados), los dispositivos se crean sin asociación con una plantilla de dispositivo. El dispositivo debe estar asociado con una plantilla para explorar los datos y otros detalles sobre él. Siga estos pasos para asociar dispositivos con una plantilla:
1. Elija **Explorer** en el menú de navegación de la izquierda.
1. En el panel izquierdo, elija **Unassociated devices** (Dispositivos no asociados).
    [![Dispositivos no asociados](./media/howto-manage-devices/UnassociatedDevices1.png)](./media/howto-manage-devices/UnassociatedDevices1.png#lightbox)
1. Seleccione los dispositivos que quiere asociar con una plantilla.
1. Haga clic en la opción **Associate** (Asociar).
    [![Asociar dispositivos](./media/howto-manage-devices/UnassociatedDevices2.png)](./media/howto-manage-devices/UnassociatedDevices2.png#lightbox)
1. Elija la plantilla de la lista de plantillas disponibles y haga clic en el botón **Associate** (Asociar).
1. Los dispositivos seleccionados se mueven a la plantilla de dispositivo correspondiente.

 >   [!NOTE] 
    Una vez que un dispositivo se ha asociado con una plantilla, no se puede desasociar ni asociar con una plantilla diferente.

## <a name="export-devices"></a>Exportación de dispositivos

Para aprovisionar dispositivos para conectarse a IoT Central, necesitará la cadena de conexión del dispositivo que se genera mediante IoT Central. Puede usar la característica de exportación para obtener las cadenas de conexión y otras propiedades de los dispositivos en masa desde la aplicación. La exportación crea un archivo CSV con la identidad, el nombre y la cadena de conexión principal de todos los dispositivos seleccionados.

Para realizar la exportación masiva de dispositivos desde la aplicación:
1. Elija **Explorer** en el menú de navegación de la izquierda.

1. En el panel izquierdo, elija la plantilla de dispositivo para la que desea exportar los dispositivos.

1. Seleccione los dispositivos que desea exportar y, a continuación, haga clic en la acción **Exportar**.

    [![Exportar](./media/howto-manage-devices/Export1.png)](./media/howto-manage-devices/Export1.png#lightbox)

1. Se inicia el proceso de exportación y puede realizar el seguimiento del estado en la parte superior de la cuadrícula. 

1. Una vez finalizada la exportación, se muestra un mensaje de confirmación junto con un vínculo para descargar el archivo generado.

1. Haga clic en el **mensaje de operación correcta** para descargar el archivo en una carpeta local en el disco.

    [![Exportación correcta](./media/howto-manage-devices/Export2.png)](./media/howto-manage-devices/Export2.png#lightbox)

1. El archivo CSV exportado tendrá la siguiente información de columnas: **id. del dispositivo, nombre del dispositivo, claves primaria y secundaria del dispositivo y huellas digitales del certificado primario y secundario**
    *   IOTC_DEVICEID
    *   IOTC_DEVICENAME
    *   IOTC_SASKEY_PRIMARY
    *   IOTC_SASKEY_SECONDARY
    *   IOTC_X509THUMBPRINT_PRIMARY 
    *   IOTC_X509THUMBPRINT_SECONDARY

## <a name="delete-a-device"></a>Eliminar un dispositivo

Para eliminar ya sea un dispositivo real o simulado de la aplicación de Azure IoT Central:

1. Elija **Explorer** en el menú de navegación.

1. Elija la plantilla de dispositivo del dispositivo que desea eliminar.

1. Active la casilla situada junto al dispositivo que desea eliminar.

1. Elija **Eliminar**.

## <a name="change-a-device-setting"></a>Cambio de la configuración de un dispositivo

La configuración controla el comportamiento de un dispositivo. En otras palabras, le permite proporcionar entradas para el dispositivo. Puede ver y actualizar la configuración del dispositivo en la página **Detalles del dispositivo**.

1. Elija **Explorer** en el menú de navegación.

1. Elija la plantilla de dispositivo del dispositivo cuya configuración desea modificar.

1. Elija la pestaña **Configuración** . Aquí verá toda la configuración que tiene el dispositivo y sus valores actuales. Para cada valor, puede ver si el dispositivo se está sincronizando todavía.

1. Modifique la configuración a los valores deseados. Puede modificar varias opciones de configuración a la vez y actualizarlas todas al mismo tiempo.

1. Seleccione **Actualizar**. Los valores se envían al dispositivo. Cuando el dispositivo confirma el cambio de configuración, el estado de la configuración se cambia de nuevo a **synced** (sincronizado).

## <a name="change-a-property"></a>Cambio de una propiedad

Las propiedades son los metadatos de dispositivo asociados al dispositivo, como la ciudad y el número de serie. Puede ver y actualizar las propiedades en la página **Detalles del dispositivo**.

1. Elija **Explorer** en el menú de navegación.

1. Elija la plantilla de dispositivo del dispositivo cuyas propiedades desea modificar.

1. Elija la pestaña **Propiedades**, donde verá todas las propiedades.

1. Modifique las propiedades a los valores deseados. Puede modificar varias propiedades a la vez y actualizarlas todas al mismo tiempo.

1. Seleccione **Actualizar**.

> [!NOTE]
> No se puede cambiar el valor de las _propiedades del dispositivo_. Las propiedades del dispositivo se establecen con el dispositivo y son de solo lectura en la aplicación de Azure IoT Central.

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha aprendido a administrar dispositivos en la aplicación de Azure IoT Central, le sugerimos el paso siguiente:

> [!div class="nextstepaction"]
> [How to use device sets](howto-use-device-sets.md) (Uso de conjuntos de dispositivos)

<!-- Next how-tos in the sequence -->
